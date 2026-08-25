---
title: "Authorization Flow with a CLI"
date: 2020-06-19T00:19:52Z
draft: false
image: "cover.svg"
color: "#b45309"
description: "A reusable Go package for the OAuth2 Authorization Code flow from the command line, with a working Spotify example built on top of it."
---

OAuth2's Authorization Code flow expects a web page: redirect the user to the authorization server, they sign in and grant consent, it redirects back to *your* page with a code. A CLI doesn't have a page to redirect back to — so it has to fake one. [`cliauthorizationflow`](https://github.com/cleverswine/cliauthorizationflow) packages that whole dance into a single `NewClient` call, and [`cliauthorizationflow-spotify`](https://github.com/cleverswine/cliauthorizationflow-spotify) is a minimal example wired up to Spotify's API to print your top tracks.

## The trick: a throwaway local server

`NewClient` builds a standard `golang.org/x/oauth2.Config` from whatever authorization/token URLs and scopes you pass in, generates a random `state` string, and registers a `/callback` handler on `localhost:8080` (configurable via `CallbackPort`). It prints the authorization URL for you to open in a browser, then blocks on a channel until that local server catches the redirect:

```go
state := randStringBytes(40)
fmt.Printf("\n--- to continue, please log in and authorize at the following url --- \n%s\n\n", oauthConfig.AuthCodeURL(state))

queryValCh := make(chan url.Values)
http.HandleFunc("/callback", func(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Thank you. You may continue using the application now that you are signed in.")
    queryValCh <- r.URL.Query()
})
go http.ListenAndServe(fmt.Sprintf(":%d", config.CallbackPort), nil)

queryVals := <-queryValCh
code := queryVals.Get("code")
if actualState := queryVals.Get("state"); actualState != state {
    return nil, errors.New("redirect state parameter doesn't match")
}
token, err := oauthConfig.Exchange(ctx, code)
```

Once it has a token, `oauthConfig.Client(ctx, token)` hands back a `*http.Client` with the auth header (and refresh) already wired in — the returned `Client` embeds it directly, so it's a drop-in replacement anywhere an `*http.Client` is expected.

## Token storage is an interface, not a decision

Persistence is pluggable through a small `TokenStorage` interface (`Get`/`Save`), so the caller decides where tokens live. `DefaultTokenStorage` is the batteries-included option — JSON files under `~/.config/{appName}/`, keyed by the auth server's hostname:

```go
client, err := auth.NewClient(ctx, config, auth.NewDefaultTokenStorage("spotify-cli"))
if err != nil {
    log.Fatal(err)
}
defer client.Persist()
```

That hostname-only cache key is also its sharpest edge: two apps requesting different scopes against the same provider, sharing one `appName`, will happily hand back a cached token that doesn't cover the scopes just requested — there's no scope check on the cache-hit path, just a `// TODO: is the refresh token still valid?` sitting right where it returns.

## It says it's a prototype, and it shows

The README is upfront about this: *"beta/prototype level package and shouldn't be used in production without understanding what this code does."* A few concrete reasons why:

- Every step prints an unconditional `AUTH_DEBUG::`/`TOK_DEBUG::` line — there's no verbosity flag, it just always talks.
- `/callback` is registered on the default `http.ServeMux`, and the server is never shut down after the token exchange completes. Call `NewClient` twice in one process — say, two different providers — and the second registration panics.

Both repos were pushed the same evening, which reads like an attempt to stop hand-rolling this same "spin up a server, print a URL, block on a channel" scaffolding in every new project. It didn't stick, though — later Spotify tools of mine, [spotui](/posts/spotui/) in 2023 and [SpotNet](/posts/spotnet/) in 2024, each reimplemented that exact pattern from scratch rather than depending on this package.

## The Spotify example

`cliauthorizationflow-spotify` is the whole point made concrete — configure Spotify's endpoints and a scope, get back an authenticated client, and hand it straight to [zmb3/spotify](https://github.com/zmb3/spotify):

```go
config := &auth.Config{
    ClientID:         os.Getenv("SPOTIFY_ID"),
    ClientSecret:     os.Getenv("SPOTIFY_SECRET"),
    AuthorizationURL: SpotifyAuthURL,
    TokenURL:         SpotifyTokenURL,
    Scopes:           []string{"user-top-read"},
}

client, err := auth.NewClient(ctx, config, auth.NewDefaultTokenStorage("spotify-cli"))
defer client.Persist()

spotifyClient := spotify.NewClient(client.Client)
tracks, err := spotifyClient.CurrentUsersTopTracks()
for _, track := range tracks.Tracks {
    fmt.Println(track.String())
}
```

No manual URL-building, no hand-written callback handler — just a `Config` and a `TokenStorage`.

Source: [github.com/cleverswine/cliauthorizationflow](https://github.com/cleverswine/cliauthorizationflow) and [github.com/cleverswine/cliauthorizationflow-spotify](https://github.com/cleverswine/cliauthorizationflow-spotify)

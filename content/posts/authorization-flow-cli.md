---
title: "Authorization Flow with a CLI"
date: 2020-06-18T19:01:22Z
draft: false
---

When calling a public API on behalf of a user, you'll often need to use the [Authorization Code Flow](https://tools.ietf.org/html/rfc6749#section-4.1) to get a token to use on the API calls. This flow invloves redirecting the user to the authorization server where they will sign in and give consent for your application to access the requested resources (scopes).

From a command-line application, we don't have a web page to do this redirect flow. What we can do is start up a simple web server and present the user with a URL to visit. When the authorization server redirects back to our simple web server, we can handle the authorization code exchange.

Here's a small library in Go that handles the entire Authorization Code flow and returns an authenticated http client.

```go
package main

import (
    "context"
    "errors"
    "log"
    "math/rand"
    "net/http"
    "net/url"
    "time"

    "golang.org/x/oauth2"
)

type AuthorizationFlowClient struct {
    *http.Client
    token *oauth2.Token
}

func NewAuthorizationFlowClient(ctx context.Context,
    authorizationURL string, tokenURL string,
    clientID string, clientSecret string,
    scopes []string) (*AuthorizationFlowClient, error) {

    // use golang's oauth2 package for some magic
    config := &oauth2.Config{
        ClientID:     clientID,
        ClientSecret: clientSecret,
        Endpoint: oauth2.Endpoint{
            AuthURL:  authorizationURL,
            TokenURL: tokenURL,
        },
        RedirectURL: "http://localhost:8080/callback",
        Scopes:      scopes,
    }
    // try cache
    token := tokenFromCache() // cache code excluded for brevity
    if token != nil {
        return &AuthorizationFlowClient{ Client: config.Client(ctx, token), token:  token }, nil
    }
    // get a token via authorization flow
    state := randStringBytes(40)
    log.Println("to continue, please authorize this application at: " + config.AuthCodeURL(state))
    // start an http server and wait for callback
    queryValCh := make(chan url.Values)
    http.HandleFunc("/callback", func(w http.ResponseWriter, r *http.Request) {
        queryValCh <- r.URL.Query()
    })
    go http.ListenAndServe(":8080", nil)
    queryVals := <-queryValCh
    // verify response and extract auth code
    code := queryVals.Get("code")
    if code == "" {
        return nil, errors.New("didn't get access code")
    }
    if actualState := queryVals.Get("state"); actualState != state {
        return nil, errors.New("redirect state parameter doesn't match")
    }
    // exchange auth code for token
    token, err = config.Exchange(ctx, code)
    if err != nil {
        return nil, err
    }
    return &AuthorizationFlowClient{
        Client: config.Client(ctx, token),
        token:  token,
    }, nil
}

func (a *AuthorizationFlowClient) Close() {
    saveTokenToCache(a.token) // cache code excluded for brevity
}
```

Here's how you would use the library to get a user's top tracks from the [Spotify API](https://developer.spotify.com/documentation/web-api/).

```go
// get an authenticated http client
client, _ := NewAuthorizationFlowClient(context.Background(),
    "https://accounts.spotify.com/authorize", "https://accounts.spotify.com/api/token",
    os.Getenv("SPOTIFY_ID"), os.Getenv("SPOTIFY_SECRET"), []string{"user-top-read"})
defer client.Close()

// get user's top tracks
resp, err := client.Get("https://api.spotify.com/v1/me/top/tracks")
```

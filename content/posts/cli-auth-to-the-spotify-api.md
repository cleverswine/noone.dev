---
title: "CLI Auth to the Spotify API"
date: 2020-01-30T17:00:27-08:00
draft: true
---

In this post, I will show how write a command-line based application (CLI)
that can authenticate to Spotify as a user and then use the [Spotify API](https://developer.spotify.com/documentation/web-api/) on behalf of that user.

I'll be using the Spotify client library from [github.com/zmb3/spotify](https://github.com/zmb3/spotify).

The tricky bit is getting the user's access token after logging in. The Spotify Web API uses
[OAuth 2.0](https://developer.spotify.com/documentation/general/guides/authorization-guide/) for authorization, which requires some browser interactions and redirects.

To do that from the command-line, we have to get a little bit creative.
What we can do is generate the authorization url, which will look something like this:
```https://accounts.spotify.com/authorize?client_id=myclientid&redirect_uri=http%3A%2F%2Flocalhost%3A8080%2Fcallback&response_type=code&scope=user-top-read&state=BOWi76LUJ7uGW9zPn2jbt_xIWILUk8_Tw2MXFsY1```

The user will have to open that url in the browser and log in to their account. After logging in,
Spotify will redirect to a local url with the user's access token. For that, we can start a minimal http server
that waits for that request and stores the token to be used for API calls.

Because we don't know what is happening outside of our application, we can make use of a channel to wait for authentication to complete.

The code is simplified for readability. You wouldn't hard-code the client id and secret - you might get them from environment variables.
You'd also want to temporarily cache the token offline so you don't have to go through the auth process every time you start the application.

{{< highlight go >}}
package main

import (
	"fmt"
	"log"
	"math/rand"
	"net/http"
	"time"

	"github.com/zmb3/spotify"
)

// used to build an oauth2 state value
const letterBytes = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ123456789_"

func init() {
	rand.Seed(time.Now().UnixNano())
}

func main() {
	// get an authenticated Spotify client with default config
	spotifyClientBuilder := NewSpotifyClientBuilder(nil)
	spotifyClient, err := spotifyClientBuilder.GetClient()
	if err != nil {
		log.Fatal(err)
	}
	// get the logged in user
	user, err := spotifyClient.CurrentUser()
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Hello " + user.DisplayName)
	// get the user's top tracks
	tracks, err := spotifyClient.CurrentUsersTopTracks()
	if err != nil {
		log.Fatal(err)
	}
	for i := 0; i < len(tracks.Tracks); i++ {
		fmt.Println(tracks.Tracks[i].Name)
	}
}

// SpotifyClientBuilderConfig is the configuration that ClientBuilder uses to initialize a Spotify client
type SpotifyClientBuilderConfig struct {
	ClientID     string
	ClientSecret string
	Scopes       []string
	LocalPort    string
}

// SpotifyClientBuilder builds an authenticated Spotify client
type SpotifyClientBuilder struct {
	Config *SpotifyClientBuilderConfig
	auth   spotify.Authenticator
	state  string
	ch     chan *spotify.Client
}

// NewSpotifyClientBuilder creates a new ClientBuilder with optional config
func NewSpotifyClientBuilder(config *SpotifyClientBuilderConfig) *SpotifyClientBuilder {
	c := &SpotifyClientBuilder{}
	if config == nil {
		c.Config = &SpotifyClientBuilderConfig{
			Scopes:       []string{spotify.ScopeUserTopRead},
			LocalPort:    "8080",
			ClientID:     "myclientid",
			ClientSecret: "mysecret",
		}
	} else {
		c.Config = config
	}
	c.auth = spotify.NewAuthenticator(fmt.Sprintf("http://localhost:%s/callback", c.Config.LocalPort), c.Config.Scopes...)
	if c.Config.ClientID != "" && c.Config.ClientSecret != "" {
		c.auth.SetAuthInfo(c.Config.ClientID, c.Config.ClientSecret)
	}
	c.state = randStringBytes(40)
	c.ch = make(chan *spotify.Client)
	return c
}

// GetClient uses the oauth2 flow to get an authenticated Spotify client
func (c *SpotifyClientBuilder) GetClient() (*spotify.Client, error) {
	// start an HTTP server and initiate an oidc flow
	http.HandleFunc("/callback", c.completeAuth)
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		log.Println("Got request for:", r.URL.String())
	})
	go http.ListenAndServe(":8080", nil)
	url := c.auth.AuthURL(c.state)
	fmt.Println("Please log in to Spotify by visiting the following page in your browser:", url)
	// wait for auth to complete
	client := <-c.ch
	return client, nil
}

func (c *SpotifyClientBuilder) completeAuth(w http.ResponseWriter, r *http.Request) {
	tok, err := c.auth.Token(c.state, r)
	if err != nil {
		http.Error(w, "Couldn't get token", http.StatusForbidden)
		log.Fatal(err)
	}
	if st := r.FormValue("state"); st != c.state {
		http.NotFound(w, r)
		log.Fatalf("State mismatch: %s != %s\n", st, c.state)
	}
	// use the token to get an authenticated client
	client := c.auth.NewClient(tok)
	fmt.Fprintf(w, "Login Completed!")
	c.ch <- &client
}

func randStringBytes(n int) string {
	b := make([]byte, n)
	for i := range b {
		b[i] = letterBytes[rand.Intn(len(letterBytes))]
	}
	return string(b)
}
{{< /highlight >}}
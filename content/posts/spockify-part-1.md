---
title: "Spockify - Part 1"
date: 2021-05-26T13:23:04-07:00
draft: false
---

I've decided to start a simple new "hobby project" that I'm calling **Spockify**. The idea is that I'll present a relatively simple web-based UI that will allow a Spotify user to log in and build new playlists based on track features like "danceability", "tempo", "genre", and whatever else makes sense. I'm thinking that the base set of tracks will be the user's saved (or liked) tracks. From there, I can use the Spotify API to get various bits of metadata for each track and then allow filtering on that metadata to build a list of songs that can be saved as a playlist.

What I'd like to do is implement this project in small steps and create some "how-to" blog posts as I go.

Why? Because it's fun.

## Planning

To begin, I've discovered which Spotify endpoints to use for the functionality that I need.

### Build a list of filterable tracks

requires the *user-library-read* scope

- `/me/tracks` - get the user's saved tracks
- `/audio-features` - get track features, like "danceability"
- `/audio-analysis` - get track audio analysys, like "tempo"

### Create a playlist

requires the *playlist-modify-public* and *playlist-modify-private* scopes

- `/users/{user_id}/playlists` - create a playlist
- `/playlists/{playlist_id}/tracks` - add/replace tracks of the playlist

## Next

In part 2 I'll bootstrap a minimal application skeleton.
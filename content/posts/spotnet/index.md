---
title: "SpotNet"
date: 2024-07-31T21:58:47Z
draft: false
image: "cover.png"
color: "#0e7490"
description: "A terminal Spotify player, because a terminal is where I actually am most of the day."
---

I spend most of the workday in a terminal, so alt-tabbing to a whole separate app just to skip a song or see what's playing was always a little annoying. SpotNet is a small, self-contained TUI player built on the Spotify Web API — a live-updating now-playing table with a handful of single-key controls, that never needs its own window.

## Two apps, one job each

Spotify's OAuth flow needs a real redirect URI, which a CLI can't host on its own, so the repo splits into two pieces:

- A tiny ASP.NET Core **web app** whose only job is `/login` → redirect to Spotify → `/loggedIn`. You run it, sign in once, and it writes the token to `{ApplicationData}/spotnet`.
- The **CLI**, which reads that cached token, refreshes it automatically when it expires, and does everything else. The web app doesn't need to be running once a token exists.

Publish the CLI as a self-contained binary and it becomes a normal command:

{{< highlight shell >}}
dotnet publish src/cli --self-contained --output $HOME/bin/SpotNet
ln -s $HOME/bin/SpotNet/SpotNet.Cli $HOME/bin/spotnet
{{< /highlight >}}

## The player loop

The table itself is a [Spectre.Console](https://spectreconsole.net/) `Live` display. Two background tasks feed it through a `Channel<ConsoleKey>`: one blocks on `Console.ReadKey`, the other just sleeps and enqueues a synthetic refresh key every 10 seconds, so the now-playing row keeps advancing even if you never touch the keyboard.

{{< highlight csharp >}}
await AnsiConsole.Live(table).StartAsync(async ctx =>
{
    await UpdateView(ctx);
    await foreach (var k in ch.Reader.ReadAllAsync())
    {
        switch (k)
        {
            case ConsoleKey.Q: cancellationTokenSource.Cancel(); break;
            case ConsoleKey.R: await UpdateView(ctx); break;
            case ConsoleKey.N: await client.Post("/v1/me/player/next", user, cancellationToken); break;
            case ConsoleKey.P: await client.Post("/v1/me/player/previous", user, cancellationToken); break;
            case ConsoleKey.Spacebar:
                if (paused) await client.Put("/v1/me/player/play", user, cancellationToken);
                else await client.Put("/v1/me/player/pause", user, cancellationToken);
                paused = !paused;
                break;
        }
        await UpdateView(ctx);
    }
});
{{< /highlight >}}

`q` quits, `r` forces a refresh, `space` toggles play/pause, `n`/`p` skip forward and back. Every update refetches the currently-playing track and the next several items in the queue, so the table is always showing what's actually coming up on that device — not a locally-guessed playlist order.

## Starting from nothing

If nothing is already playing when SpotNet starts, it doesn't just fail — it walks you through picking a playback device (cached from previous runs, since Spotify stops listing a device as soon as it goes idle), a playlist, and a shuffle setting, then starts playback itself before dropping into the live view. Most of the time, though, something's already playing somewhere and it skips straight to the table.

Source: [github.com/cleverswine/spotnet](https://github.com/cleverswine/spotnet)

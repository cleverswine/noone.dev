---
title: "Todo App"
date: 2026-02-17T21:55:55Z
draft: false
image: "cover.png"
color: "#1d4ed8"
description: "A dead-simple, self-hosted todo app for tracking household repairs and projects — no account, no subscription, no third-party service."
---

Every house has a running list of things that need attention — a cracked closet floor, a window seal that needs paint, the dozen small jobs that pile up between the big ones. I didn't want to hand that list to another app with an account, a subscription, and a company on the other end deciding what happens to the data. I just wanted something simple that lived on my own server.

So this is about as plain as a todo app gets: add an item, mark it done, drag to reorder, search to find something again. A couple of extras earned their place because they matched how the list actually gets used around the house — a spot to note the cost of a job, and a place to record whether a contractor handled it and who they were, for the jobs that aren't DIY. Switch between a grid, a list, or an expanded detail view depending on whether you want an overview or the full notes on one item, and export any single todo to a PDF when it's easier to hand someone a page than to hand them an app.

## Running it

{{< highlight shell >}}
docker compose up -d --build
{{< /highlight >}}

The database is a single SQLite file, mounted from the host, so there's nothing else to stand up and nothing to lose between rebuilds.

Source: [github.com/cleverswine/todo-app](https://github.com/cleverswine/todo-app)

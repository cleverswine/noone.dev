---
title: "HN Express"
date: 2026-08-24T22:38:25Z
draft: false
image: "cover.png"
color: "#1e293b"
description: "Shows the Hacker News front page with an AI-generated summary and representative image for each linked article, generated locally via Ollama."
---

Shows the Hacker News front page (via the official HN API, in HN's own rank order), with an AI-generated summary and representative image for each linked article. Summaries are generated in the background by a locally-running [Ollama](https://ollama.com) model — never on the request path. The web UI is plain server-rendered HTML that only reads from a local SQLite database.

Source: [github.com/cleverswine/hn-express](https://github.com/cleverswine/hn-express)

## Code organization

Three npm workspaces sharing one SQLite file:

- `db/` — SQLite schema + query helpers.
- `worker/` — background process: polls the HN API into the database, then summarizes articles (fetch → extract text/image, falling back to a headless browser when needed → Ollama).
- `web/` — Express server that renders the front page from the database.

## Running with Docker

Each workspace has its own `Dockerfile` (`web/Dockerfile`, `worker/Dockerfile`); both are built from the **repo root** as the build context, since they need the sibling `db/` package. `docker-compose.yml` runs both together, bind-mounting `$HOME/.config/hn/data` from the host into both containers so they share the same SQLite database — the same path the app uses by default outside Docker (see [Data](#data)).

Ollama itself is expected to keep running on the host, not in a container — the worker reaches it at `http://host.docker.internal:11434` by default (wired up via `extra_hosts` in the compose file, works on Docker 20.10+ on Linux/Mac/Windows).

```
docker compose up --build
```

Then open `http://localhost:3000`. Optionally copy `.env.example` to `.env` first — compose reads it automatically for the defaults shown in `docker-compose.yml` (`HN_FRONTPAGE_SIZE`, `OLLAMA_MODEL`, `PORT`, etc.); everything works without one.

## Configuration

All configuration is via environment variables (see `.env.example` for the full list and defaults), including `PORT`, `HN_FRONTPAGE_SIZE`, `FETCH_INTERVAL_MS`, `SUMMARY_CONCURRENCY`, `OLLAMA_HOST`, and `OLLAMA_MODEL`.

## Data

The SQLite database lives at `$HOME/.config/hn/data/hn.sqlite3` (created automatically on first run, including under Docker via a bind mount to the same host path — see [Running with Docker](#running-with-docker)). Delete it to start fresh. Override the location with `DB_PATH` (see `.env.example`).

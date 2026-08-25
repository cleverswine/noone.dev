---
title: "Rebuilding This Site with Claude Code"
date: 2026-08-24T18:00:00-07:00
draft: false
image: "cover.svg"
color: "#4d7c0f"
description: "Turning a plain Hugo blog into a portfolio homepage and writing real posts about my own projects — mostly done by talking to Claude Code."
---

This whole site just went through a pretty major overhaul, and almost none of it was typed by hand. I described what I wanted, in plain sentences, over the course of a long back-and-forth with Claude Code, and it did the actual editing — Hugo templates, Tailwind classes, content, the works.

## What changed

The homepage used to be a plain paginated list of posts. Now it's a card grid: the eight most recent posts, each with a cover image up top and a solid-color panel underneath for the title, date, and description. First pass had the text laid directly over the image with a gradient scrim — readable, but not readable enough — so it got reworked into image-on-top, color-panel-below, which was a real improvement.

Most of those cards are genuine posts about my own projects — [HN Express](/posts/hn-express/), [Auntie Boo Crafts](/posts/auntie-boo-crafts/), [Exif Date Organizer](/posts/exif-date-organizer/), [Todo App](/posts/todo-app/), [Watch Board](/posts/watchboard/), [SpotNet](/posts/spotnet/), and [Dotnet SDK Version Manager](/posts/dotnet-sdk-version-manager/) — written up by pointing Claude at each repo (some local, some just on GitHub) and having it read the actual README and source before writing anything. It pulled real screenshots where they existed, scraped a live screenshot of my wife's shop site when they didn't, and even built a little folder-tree diagram by hand for the one tool that had no screenshot to speak of. Each post's date is the real latest commit date from its repo, not just "today." A few more followed the same way afterward — [spotui](/posts/spotui/) among them — and [Authorization Flow with a CLI](/posts/authorization-flow-cli/) got rewritten from its actual source instead of the hand-simplified pseudocode it started as, though not everything old made the cut: a stale, never-finished "Spockify" draft got retired outright.

Twenty years of the original blog — everything published before a 2013 post called "Trying out Windows Live Writer" — now lives in its own `/archive/` section, split out from `/posts/` so the main listing stays to what's actually worth finding. It's not linked from anywhere yet, just reachable if you go looking. The listing template itself was also redone as a clean year-grouped list instead of a flat `date :: title` table. The About page got trimmed way down too: retired now, moved from Portland to near Kansas City, and a wall of bullet-pointed skills collapsed into a couple of sentences, since a resume-style list isn't all that valuable at this point.

## Future

This isn't a one-time thing. I'm going to keep leaning on AI to simplify the actual workflow of blogging — not just writing individual posts like this one, but the whole loop of noticing something worth writing about, turning it into a post, and getting it published. Right now that still means me driving Claude Code by hand through a terminal. Eventually I'd like some of that turned into actual tools, so the next round of "write a post about this repo" takes even less effort than typing this sentence did.

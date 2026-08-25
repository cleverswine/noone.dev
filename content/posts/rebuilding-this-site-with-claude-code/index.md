---
title: "Rebuilding This Site with Claude Code"
date: 2026-08-24T18:00:00-07:00
draft: true
image: "cover.svg"
color: "#4d7c0f"
description: "Turning a plain Hugo blog into a portfolio homepage, writing real posts about my own projects, and a few bugs along the way — mostly done by talking to Claude Code."
---

This whole site just went through a pretty major overhaul, and almost none of it was typed by hand. I described what I wanted, in plain sentences, over the course of a long back-and-forth with Claude Code, and it did the actual editing — Hugo templates, Tailwind classes, content, the works.

## What changed

The homepage used to be a plain paginated list of posts. Now it's a card grid: the six most recent posts, each with a cover image up top and a solid-color panel underneath for the title, date, and description. First pass had the text laid directly over the image with a gradient scrim — readable, but not readable enough — so it got reworked into image-on-top, color-panel-below, which was a real improvement.

Six of those cards are now genuine posts about my own projects — [HN Express](/posts/hn-express/), [Watch Board](/posts/watchboard/), [Auntie Boo Crafts](/posts/auntie-boo-crafts/), [Exif Date Organizer](/posts/exif-date-organizer/), [Todo App](/posts/todo-app/), and [SpotNet](/posts/spotnet/) — written up by pointing Claude at each repo (some local, some just on GitHub) and having it read the actual README and source before writing anything. It pulled real screenshots where they existed, scraped a live screenshot of my wife's shop site when they didn't, and even built a little folder-tree diagram by hand for the one tool that had no screenshot to speak of. Each post's date is the real latest commit date from its repo, not just "today."

The old `/archives/` section — twenty years of the original blog, going back to 2006 — got folded straight into `/posts/`, and the listing itself was redone as a clean year-grouped list instead of a flat `date :: title` table. The About page got trimmed way down too: retired now, moved from Portland to near Kansas City, and a wall of bullet-pointed skills collapsed into a couple of sentences, since a resume-style list isn't all that valuable at this point.

## A few real bugs

Not everything was smooth. A couple of things stood out:

- One of the layout templates, `single.html`, turned out to be dead code — this version of Hugo renders ordinary pages through `page.html` instead, so an image and date I'd added to `single.html` were silently never showing up. Fixed by moving that logic into the template that's actually used, and deleting the one that wasn't.
- A slideshow's inline `<script>` block got mangled by Hugo's Markdown renderer — a blank line inside the JavaScript was enough to convince it the raw-HTML block had ended, so the rest got run through Markdown and smart-quoted into nonsense. Moving the script to start its own HTML block fixed it.
- The dev server turned out to be serving straight from the built `docs/` output rather than rendering in memory, and a plain rebuild doesn't delete files that no longer have a source — so after merging the archives away, the old `/archives/` page kept right on existing until I ran a build with `--cleanDestinationDir`. That same rebuild also nearly deleted `docs/CNAME`, the file that points the custom domain at this site, since it never had a real source file behind it — that's fixed now too, with a proper `static/CNAME`.

## The part that mattered most

Some of what I asked for the first time around wasn't quite right — a post claimed there was a self-service admin app when there isn't; I actually rerun a script and push to git by hand. That's really the useful part of working this way: not that everything came out perfect on the first try, but that saying "that's not accurate, here's what actually happens" got it fixed in about as long as it took to type the correction.

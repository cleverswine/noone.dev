---
title: "Watch Board"
date: 2026-02-08T02:39:01Z
draft: false
image: "cover.png"
color: "#be185d"
description: "A vertical kanban board for tracking what you're watching, with TMDB search and drag-and-drop lists."
---

"What should I watch" turns into a lot of overhead once you're tracking more than one show at a time: a movie a friend recommended, the series you're mid-season on, the one you keep meaning to start. A note-taking app or a sticky note works for a day, but it doesn't hold up over months. Watch Board is a small, self-hosted board built for exactly that: one place to see everything you're planning to watch, currently watching, and have finished, without it turning into a chore to maintain.

It works the way a kanban board works for tasks. Search adds a show or movie by name — no manual data entry, since it pulls the poster, description, and cast straight from TMDB. Drag it into whichever list fits ("Watching," "Up Next," "Finished," or whatever you name your own lists). Mark which service you're watching it on, so "wait, is that on Netflix or Prime?" stops being a recurring question. Keep separate boards for movies and TV, or for different households sharing the same server, so unrelated lists don't get tangled together.

## Running it

{{< highlight shell >}}
docker compose up -d
{{< /highlight >}}

That's the whole setup — it comes up on port 8090, backed by its own local database.

## Screenshots

<div class="not-prose relative mt-6 overflow-hidden rounded-xl bg-slate-900 shadow-sm ring-1 ring-slate-900/5 dark:ring-white/10" id="wb-slideshow">
  <div class="relative aspect-video">
    <img src="cover.png" alt="Watch Board — a board with lists and items" class="absolute inset-0 size-full object-cover transition-opacity duration-300 opacity-100">
    <img src="img_1.png" alt="Watch Board — item detail dialog" class="absolute inset-0 size-full object-cover transition-opacity duration-300 opacity-0">
    <img src="img_2.png" alt="Watch Board — searching TMDB to add a title" class="absolute inset-0 size-full object-cover transition-opacity duration-300 opacity-0">
    <img src="img_3.png" alt="Watch Board — settings" class="absolute inset-0 size-full object-cover transition-opacity duration-300 opacity-0">
    <button type="button" aria-label="Previous screenshot" data-wb-prev class="absolute left-3 top-1/2 flex h-9 w-9 -translate-y-1/2 items-center justify-center rounded-full bg-black/50 text-white hover:bg-black/70">‹</button>
    <button type="button" aria-label="Next screenshot" data-wb-next class="absolute right-3 top-1/2 flex h-9 w-9 -translate-y-1/2 items-center justify-center rounded-full bg-black/50 text-white hover:bg-black/70">›</button>
  </div>
  <div class="flex justify-center gap-2 bg-slate-950 py-3" data-wb-dots>
    <button type="button" aria-label="Go to screenshot 1" class="h-2 w-2 rounded-full bg-white"></button>
    <button type="button" aria-label="Go to screenshot 2" class="h-2 w-2 rounded-full bg-white/40"></button>
    <button type="button" aria-label="Go to screenshot 3" class="h-2 w-2 rounded-full bg-white/40"></button>
    <button type="button" aria-label="Go to screenshot 4" class="h-2 w-2 rounded-full bg-white/40"></button>
  </div>
</div>

<script>
(function () {
  var root = document.getElementById('wb-slideshow');
  var slides = root.querySelectorAll('img');
  var dots = root.querySelectorAll('[data-wb-dots] button');
  var index = 0;
  function show(i) {
    index = (i + slides.length) % slides.length;
    slides.forEach(function (img, n) {
      img.className = 'absolute inset-0 size-full object-cover transition-opacity duration-300 ' + (n === index ? 'opacity-100' : 'opacity-0');
    });
    dots.forEach(function (dot, n) {
      dot.className = 'h-2 w-2 rounded-full ' + (n === index ? 'bg-white' : 'bg-white/40');
    });
  }
  root.querySelector('[data-wb-prev]').addEventListener('click', function () { show(index - 1); });
  root.querySelector('[data-wb-next]').addEventListener('click', function () { show(index + 1); });
  dots.forEach(function (dot, n) { dot.addEventListener('click', function () { show(n); }); });
})();
</script>

Source: [github.com/cleverswine/watchboard](https://github.com/cleverswine/watchboard)

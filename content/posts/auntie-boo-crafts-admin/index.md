---
title: "Auntie Boo Crafts Gets an Admin Page"
date: 2026-09-13T16:07:22Z
draft: false
image: "cover.png"
color: "#7e22ce"
description: "A simple new admin page for the handful of items on Auntie Boo Crafts that don't come from Etsy — built by just describing it to Claude Code."
---

[Auntie Boo Crafts](/posts/auntie-boo-crafts/) mostly mirrors my wife's Etsy shop automatically, but a few things — like items only sold in person at craft fairs — aren't on Etsy at all and have to be added by hand. That used to mean editing a raw data file directly, which was fine for a couple of items and increasingly annoying once there were dozens.

So now there's a small admin page just for that. Everything pulled in from Etsy still shows up as read-only, but the hand-added stuff can be edited right there: titles, descriptions, whether something's shown, and its photos — including uploading new ones or picking from what's already there.

<div class="not-prose relative mt-6 overflow-hidden rounded-xl bg-slate-950 shadow-sm ring-1 ring-slate-900/5 dark:ring-white/10" id="abca-slideshow">
  <div class="relative h-[420px]">
    <img src="img_2.png" alt="Auntie Boo Crafts admin — the section list, hand-added sections editable, Etsy sections read-only" class="absolute inset-0 mx-auto size-full object-contain p-6 transition-opacity duration-300 opacity-100">
    <img src="img_3.png" alt="Auntie Boo Crafts admin — a section's items, each with its own photo carousel" class="absolute inset-0 mx-auto size-full object-contain p-6 transition-opacity duration-300 opacity-0">
    <img src="img_1.png" alt="Auntie Boo Crafts admin — the edit item screen, with title, description, and link fields" class="absolute inset-0 mx-auto size-full object-contain p-6 transition-opacity duration-300 opacity-0">
    <button type="button" aria-label="Previous screenshot" data-abca-prev class="absolute left-3 top-1/2 flex h-9 w-9 -translate-y-1/2 items-center justify-center rounded-full bg-black/50 text-white hover:bg-black/70">‹</button>
    <button type="button" aria-label="Next screenshot" data-abca-next class="absolute right-3 top-1/2 flex h-9 w-9 -translate-y-1/2 items-center justify-center rounded-full bg-black/50 text-white hover:bg-black/70">›</button>
  </div>
  <div class="flex justify-center gap-2 bg-black py-3" data-abca-dots>
    <button type="button" aria-label="Go to screenshot 1" class="h-2 w-2 rounded-full bg-white"></button>
    <button type="button" aria-label="Go to screenshot 2" class="h-2 w-2 rounded-full bg-white/40"></button>
    <button type="button" aria-label="Go to screenshot 3" class="h-2 w-2 rounded-full bg-white/40"></button>
  </div>
</div>

<script>
(function () {
  var root = document.getElementById('abca-slideshow');
  var slides = root.querySelectorAll('img');
  var dots = root.querySelectorAll('[data-abca-dots] button');
  var index = 0;
  var timer;
  function show(i) {
    index = (i + slides.length) % slides.length;
    slides.forEach(function (img, n) {
      img.className = 'absolute inset-0 mx-auto size-full object-contain p-6 transition-opacity duration-300 ' + (n === index ? 'opacity-100' : 'opacity-0');
    });
    dots.forEach(function (dot, n) {
      dot.className = 'h-2 w-2 rounded-full ' + (n === index ? 'bg-white' : 'bg-white/40');
    });
  }
  function startAutoplay() { timer = setInterval(function () { show(index + 1); }, 4000); }
  function stopAutoplay() { clearInterval(timer); }
  function restartAutoplay() { stopAutoplay(); startAutoplay(); }
  root.querySelector('[data-abca-prev]').addEventListener('click', function () { show(index - 1); restartAutoplay(); });
  root.querySelector('[data-abca-next]').addEventListener('click', function () { show(index + 1); restartAutoplay(); });
  dots.forEach(function (dot, n) { dot.addEventListener('click', function () { show(n); restartAutoplay(); }); });
  root.addEventListener('mouseenter', stopAutoplay);
  root.addEventListener('mouseleave', startAutoplay);
  root.addEventListener('focusin', stopAutoplay);
  root.addEventListener('focusout', startAutoplay);
  startAutoplay();
})();
</script>

The whole thing came together just by describing what I wanted to Claude Code — a working first version in one sitting, then a handful of small follow-up asks (clearer warnings before deleting something, an easier way to flip through an item's photos) each turned around in a few minutes. It's just for me to use at home, so it didn't need to be anything more than exactly what it needed to be.

Source: [github.com/cleverswine/abc11ty](https://github.com/cleverswine/abc11ty) (`dev` branch)

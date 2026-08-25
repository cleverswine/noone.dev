---
author: Kevin
categories:
- Uncategorized
date: 2012-08-07T15:15:17Z
guid: http://blog.cleverswine.net/?p=799
id: 799
tags:
- Hacking
title: Project &#8211; La Cocina
url: /2012/08/07/project-la-cocina/
---

[<img style="border: solid 2px #000000;" src="https://i2.wp.com/farm9.staticflickr.com/8422/7735155734_7110a42dea_m.jpg?w=840" alt="" data-recalc-dims="1" />](http://www.flickr.com/photos/cleverswine/7735155734/ "photo sharing")

I&#8217;ve been working on a new Rails application for storing our recipes. It&#8217;s at a point where it is being hosted and used by us. Some of the style is borrowed (stolen) from <a href="http://www.themarthablog.com/" target="_blank">Martha Stewart&#8217;s blog</a>.

The big feature of La Cocina is it&#8217;s ability to pull in recipes from a number of other recipe sites.  I&#8217;m using the <a href="http://nokogiri.org/" target="_blank">nokogiri</a> gem to do screen scraping for recipe data.  There&#8217;s an admin interface for creating new screen scrapers (which I call &#8220;scarfers&#8221;), so it&#8217;s easy to add new recipe sites by entering DOM paths (CSS or XPath) and some other options.  With one click of a bookmarklet on a supported site (we have 5 configured so far), the recipe is in our database.

Other stuff in use: <a href="https://github.com/amatsuda/kaminari" target="_blank">kaminari</a> for pagination, <a href="https://github.com/thoughtbot/paperclip/" target="_blank">paperclip</a> for image attachments (including fetching from a URL, resizing, and generating thumbnails), <a href="https://github.com/mbleigh/acts-as-taggable-on/" target="_blank">acts-as-taggable-on</a> for tagging, <a href="http://sass-lang.com/" target="_blank">SASS</a> for CSS, <a href="http://twitter.github.com/bootstrap/" target="_blank">Twitter Bootstrap</a> for some design and layout, <a href="http://www.mysql.com/" target="_blank">MySql</a> as the database, and miscellaneous <a href="http://jquery.com/" target="_blank">jQuery</a> for client-side things.

There are a few things left for me to do.  For one, I want to make it a true multi-user site so I can share it or create a demo account for showing it off.
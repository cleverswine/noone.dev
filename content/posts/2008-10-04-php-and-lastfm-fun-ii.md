---
author: Kevin
categories:
- Uncategorized
date: 2008-10-04T00:31:23Z
guid: http://cleverswine.net/?p=456
id: 456
tags:
- Hacking
title: PHP and Last.fm Fun II
url: /2008/10/04/php-and-lastfm-fun-ii/
---

[<img class="alignleft" src="https://i1.wp.com/farm3.static.flickr.com/2022/2793204095_7751ec18e7_s.jpg?resize=75%2C75" alt="McNutty" data-recalc-dims="1" />](http://www.flickr.com/photos/cleverswine/2793204095/){.tt-flickr.tt-flickr-Square} It seems like I can&#8217;t use the following code on my server:
  
$dom = new DOMDocument(); $dom->load($recentTracksUrl);
  
because &#8220;URL file-access is disabled in the server &#8220;. I guess I&#8217;ll have to find another way to handle REST responses with php. I&#8217;m guessing there&#8217;s a better solution anyway. Research begins&#8230;
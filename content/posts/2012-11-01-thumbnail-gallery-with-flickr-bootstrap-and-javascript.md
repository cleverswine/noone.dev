---
author: Kevin
categories:
- Uncategorized
date: 2012-11-01T15:38:59Z
guid: http://blog.cleverswine.net/?p=809
id: 809
tags:
- Hacking
title: Thumbnail Gallery with Flickr, Bootstrap, and Javascript
url: /2012/11/01/thumbnail-gallery-with-flickr-bootstrap-and-javascript/
wp-syntax-cache-content:
- |
  a:2:{i:1;s:608:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="html" style="font-family:monospace;">&lt;div class=&quot;row&quot;&gt;
  &lt;div class=&quot;span16&quot;&gt;
  &lt;ul class=&quot;thumbnails&quot; id=&quot;my-photos&quot;&gt;
  &lt;/ul&gt;
  &lt;/div&gt;
  &lt;/div&gt;</pre></td></tr></table><p class="theCode" style="display:none;">&lt;div class=&quot;row&quot;&gt;
  &lt;div class=&quot;span16&quot;&gt;
  &lt;ul class=&quot;thumbnails&quot; id=&quot;my-photos&quot;&gt;
  &lt;/ul&gt;
  &lt;/div&gt;
  &lt;/div&gt;</p></div>
  ;i:2;s:7908:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="javascript" style="font-family:monospace;"><span style="color: #006600; font-style: italic;">// see the list of feeds here - http://www.flickr.com/services/feeds/</span>
  <span style="color: #006600; font-style: italic;">// this is a handy tool to get people and group ids - http://idgettr.com/</span>
  $<span style="color: #009900;">&#40;</span>document<span style="color: #009900;">&#41;</span>.<span style="color: #660066;">ready</span><span style="color: #009900;">&#40;</span><span style="color: #000066; font-weight: bold;">function</span> <span style="color: #009900;">&#40;</span><span style="color: #009900;">&#41;</span> <span style="color: #009900;">&#123;</span>
  <span style="color: #000066; font-weight: bold;">var</span> flickrFeedUrl <span style="color: #339933;">=</span> <span style="color: #3366CC;">&quot;http://api.flickr.com/services/feeds/groups_pool.gne?id=35468135854@N01&amp;lang=en-us&amp;jsoncallback=?&quot;</span><span style="color: #339933;">;</span>
  $.<span style="color: #660066;">getJSON</span><span style="color: #009900;">&#40;</span>flickrFeedUrl<span style="color: #339933;">,</span> <span style="color: #009900;">&#123;</span> format<span style="color: #339933;">:</span> <span style="color: #3366CC;">&quot;json&quot;</span> <span style="color: #009900;">&#125;</span><span style="color: #339933;">,</span>
  <span style="color: #000066; font-weight: bold;">function</span> <span style="color: #009900;">&#40;</span>data<span style="color: #009900;">&#41;</span> <span style="color: #009900;">&#123;</span>
  $.<span style="color: #660066;">each</span><span style="color: #009900;">&#40;</span>data.<span style="color: #660066;">items</span><span style="color: #339933;">,</span> <span style="color: #000066; font-weight: bold;">function</span> <span style="color: #009900;">&#40;</span>i<span style="color: #339933;">,</span> item<span style="color: #009900;">&#41;</span> <span style="color: #009900;">&#123;</span>
  &nbsp;
  <span style="color: #006600; font-style: italic;">// build some elements and then append them accordingly</span>
  <span style="color: #000066; font-weight: bold;">var</span> li <span style="color: #339933;">=</span> $<span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;&lt;li&gt;&quot;</span><span style="color: #009900;">&#41;</span>.<span style="color: #660066;">attr</span><span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;class&quot;</span><span style="color: #339933;">,</span> <span style="color: #3366CC;">&quot;span2&quot;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  <span style="color: #000066; font-weight: bold;">var</span> a <span style="color: #339933;">=</span> $<span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;&lt;a&gt;&quot;</span><span style="color: #009900;">&#41;</span>.<span style="color: #660066;">attr</span><span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;href&quot;</span><span style="color: #339933;">,</span> item.<span style="color: #660066;">link</span><span style="color: #009900;">&#41;</span>.<span style="color: #660066;">attr</span><span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;class&quot;</span><span style="color: #339933;">,</span> <span style="color: #3366CC;">&quot;thumbnail&quot;</span><span style="color: #009900;">&#41;</span>.<span style="color: #660066;">attr</span><span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;title&quot;</span><span style="color: #339933;">,</span> item.<span style="color: #660066;">title</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  <span style="color: #006600; font-style: italic;">// we want the larger square (150px) thumbnail instead of the smaller one (75px)</span>
  <span style="color: #000066; font-weight: bold;">var</span> img <span style="color: #339933;">=</span> $<span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;&lt;img/&gt;&quot;</span><span style="color: #009900;">&#41;</span>.<span style="color: #660066;">attr</span><span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;src&quot;</span><span style="color: #339933;">,</span> item.<span style="color: #660066;">media</span>.<span style="color: #660066;">m</span>.<span style="color: #660066;">replace</span><span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;_m.jpg&quot;</span><span style="color: #339933;">,</span> <span style="color: #3366CC;">&quot;_q.jpg&quot;</span><span style="color: #009900;">&#41;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  img.<span style="color: #660066;">appendTo</span><span style="color: #009900;">&#40;</span>a<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  a.<span style="color: #660066;">appendTo</span><span style="color: #009900;">&#40;</span>li<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  $<span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;#my-photos&quot;</span><span style="color: #009900;">&#41;</span>.<span style="color: #660066;">append</span><span style="color: #009900;">&#40;</span>li<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  &nbsp;
  <span style="color: #006600; font-style: italic;">// stop at 18 photos (i is 0 based)</span>
  <span style="color: #000066; font-weight: bold;">if</span> <span style="color: #009900;">&#40;</span>i <span style="color: #339933;">==</span> <span style="color: #CC0000;">17</span><span style="color: #009900;">&#41;</span> <span style="color: #000066; font-weight: bold;">return</span> <span style="color: #003366; font-weight: bold;">false</span><span style="color: #339933;">;</span>
  <span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  <span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  <span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span></pre></td></tr></table><p class="theCode" style="display:none;">// see the list of feeds here - http://www.flickr.com/services/feeds/
  // this is a handy tool to get people and group ids - http://idgettr.com/
  $(document).ready(function () {
  var flickrFeedUrl = &quot;http://api.flickr.com/services/feeds/groups_pool.gne?id=35468135854@N01&amp;lang=en-us&amp;jsoncallback=?&quot;;
  $.getJSON(flickrFeedUrl, { format: &quot;json&quot; },
  function (data) {
  $.each(data.items, function (i, item) {

  // build some elements and then append them accordingly
  var li = $(&quot;&lt;li&gt;&quot;).attr(&quot;class&quot;, &quot;span2&quot;);
  var a = $(&quot;&lt;a&gt;&quot;).attr(&quot;href&quot;, item.link).attr(&quot;class&quot;, &quot;thumbnail&quot;).attr(&quot;title&quot;, item.title);
  // we want the larger square (150px) thumbnail instead of the smaller one (75px)
  var img = $(&quot;&lt;img/&gt;&quot;).attr(&quot;src&quot;, item.media.m.replace(&quot;_m.jpg&quot;, &quot;_q.jpg&quot;));
  img.appendTo(a);
  a.appendTo(li);
  $(&quot;#my-photos&quot;).append(li);

  // stop at 18 photos (i is 0 based)
  if (i == 17) return false;
  });
  });
  });</p></div>
  ";}
---

I recently created a thumbnail gallery of my latest Flickr photos using the <a href="http://www.flickr.com/services/feeds/" target="_blank">Flickr feeds</a> API, <a href="http://twitter.github.com/bootstrap/index.html" target="_blank">Twitter Bootstrap</a>, and Javascript/<a href="http://jquery.com/" target="_blank">jQuery</a>.  There is no server side code for this solution, and the client side code is really simple.  To start, create a blank html page (I like to start with the <a href="http://html5boilerplate.com/" target="_blank">HTML5 Boilerplate</a>).  Make sure you include the Bootstrap css and jQuery.  I used the <a href="http://twitter.github.com/bootstrap/components.html#thumbnails" target="_blank">Thumbnails</a> component of Bootstrap for the gallery layout.  Here&#8217;s the div to hold the thumbnails:

<pre lang="html"><div class="row">
  <div class="span16">
    <ul class="thumbnails" id="my-photos">
      
    </ul>
        
  </div>
  
</div></pre>

Here&#8217;s the javascript to fetch the photos and populate the div. For this example, I&#8217;m displaying the pdx group pool photos.

<pre lang="javascript">// see the list of feeds here - http://www.flickr.com/services/feeds/
// this is a handy tool to get people and group ids - http://idgettr.com/
$(document).ready(function () {
    var flickrFeedUrl = "http://api.flickr.com/services/feeds/groups_pool.gne?id=35468135854@N01&lang=en-us&jsoncallback=?";
    $.getJSON(flickrFeedUrl, { format: "json" },
        function (data) {
            $.each(data.items, function (i, item) {

                // build some elements and then append them accordingly
                var li = $("

<li>
  ").attr("class", "span2");
                  var a = $("<a>").attr("href", item.link).attr("class", "thumbnail").attr("title", item.title);
                  // we want the larger square (150px) thumbnail instead of the smaller one (75px)
                  var img = $("<img />").attr("src", item.media.m.replace("_m.jpg", "_q.jpg"));
                  img.appendTo(a);
                  a.appendTo(li);
                  $("#my-photos").append(li);
  
                  // stop at 18 photos (i is 0 based)
                  if (i == 17) return false;
              });
        });
  });</pre>
  
  
  <p>
    That&#8217;s it.  This minimal amount of client side code produces a nice, responsive gallery of thumbnail links to Flickr photos.
  </p>
  
  
  <p>
    <em><strong>Edit 12/31/2012:</strong> I am now using <a href="https://github.com/janl/mustache.js/" target="_blank">mustache.js</a> for templating, which is much more elegant than building the html via js.</em>
  </p>
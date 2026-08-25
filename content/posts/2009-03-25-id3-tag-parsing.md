---
author: Kevin
categories:
- Uncategorized
date: 2009-03-25T19:29:26Z
guid: http://cleverswine.net/?p=544
id: 544
tags:
- Hacking
- MP3
- Music
title: ID3 Tag Parsing
url: /2009/03/25/id3-tag-parsing/
---

I&#8217;ve been working on a project to read and update [ID3 tags](http://www.id3.org/Home) on my music collection. An ID3 tag is the meta data embedded in mp3 files that contains artist and other information. My initial goal is to batch update the &#8220;genre&#8221; field in my mp3 files. I&#8217;ve written a simple .Net forms application to do this. I found a really good library created by Novell called [TagLib Sharp](http://developer.novell.com/wiki/index.php/TagLib_Sharp). The library makes it super easy to read and write ID3 tags.
  
`<br />
                TagLib.File tagFile = TagLib.File.Create(filePath);<br />
                Tag tagId3v2 = tagFile.GetTag(TagTypes.Id3v2);<br />
                if (tagId3v2 != null) tagId3v2.Genres = genreArray;<br />
                Tag tagId3v1 = tagFile.GetTag(TagTypes.Id3v1);<br />
                if (tagId3v1 != null) tagId3v1.Genres = genreArray;<br />
                tagFile.Save();<br />
` 

The UI is pretty lame, but it works&#8230;
  
[<img src="https://i0.wp.com/farm4.static.flickr.com/3537/3388496248_76046f444e_m_d.jpg?w=840" alt="tagger" data-recalc-dims="1" />](http://www.flickr.com/photos/cleverswine/3388496248/sizes/o/)

[<img src="https://i0.wp.com/farm4.static.flickr.com/3471/3388496280_11c16fdeb1_m_d.jpg?w=840" alt="tagger" data-recalc-dims="1" />](http://www.flickr.com/photos/cleverswine/3388496280/sizes/o/)
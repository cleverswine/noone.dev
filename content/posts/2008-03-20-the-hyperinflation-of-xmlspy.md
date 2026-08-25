---
author: Kevin
categories:
- Uncategorized
date: 2008-03-20T14:24:35Z
guid: http://cleverswine.net/?p=351
id: 352
tags:
- Hacking
title: The Hyperinflation of XMLSpy
url: /2008/03/20/the-hyperinflation-of-xmlspy/
---

Just a couple of days ago, I was using [XMLSpy](https://shop.altova.com/category.asp?category_name=XMLSPY) at work to do some test XSLT transformations. Our XSL templates here at work are pretty complex, with a few xsl:include tags to import common code. After trying for nearly a day to get XMLSpy to render my transformations, I gave up. I tried every available rendering engine, I tried inserting all of the included code into one file, I tried simplifying the XML &#8211; all to no avail. I finally wrote a small C# program to do the transformation, which worked much better than XMLSpy. I&#8217;m writing this because [I just came across a story](http://www.yafla.com/dforbes/The_Hyperinflation_Of_XMLSpy/) about the ever-increasing price of XMLSpy. I have wondered myself why it&#8217;s so expensive, but large companies seem to be willing to dish out the dollars.

> By late 2000, the price of XML Spy had inflated to $149 a user. By the end of the next year it hit $399 a user. By late 2006 it was up to $499 a user (at some point dropping the space between XML and Spy, becoming XMLSpy). As I write this it’s up to $539 a user.
---
author: Kevin
categories:
- Uncategorized
date: 2011-03-08T20:13:27Z
guid: http://blog.cleverswine.net/?p=639
id: 639
tags:
- Hacking
title: A Diet and an Application
url: /2011/03/08/a-diet-and-an-application/
---

I&#8217;ve started on the <a title="Paleo Lifestyle" href="http://paleodietlifestyle.com/" target="_blank">Paleo Diet</a>, which, of course, means I have to create a software application to go along with it.

I decided this was a good time to give <a title="Asp.net MVC" href="http://www.asp.net/mvc" target="_blank">MVC3</a> and <a title="MongoDB" href="http://www.mongodb.org/" target="_blank">MongoDB</a> a test drive.  After diving in, I realized that I could benefit from some dependency injection via MVC3&#8217;s DependencyResolver.  I&#8217;ve been wanting to learn <a title="Autofac" href="http://code.google.com/p/autofac/" target="_blank">Autofac</a>, so I threw that in as the container.  This was sweet, as it injected my repository implementations into my controllers with very little setup.

At this point, I&#8217;ve learned all about the Razor view engine, DI with Autofac and DependencyResolver, MongoDB with the NoRM driver, jQuery, jQuery-ui, and some other MVC3 goodness.

For various reasons, including the fact that my web hosting provider is a Linux/non-ASP.NET provider, I ported the whole thing to Rails/MySql.  I&#8217;m glad I went through the MVC3 process, because I learned a lot, but in the end, Rails is just simpler.  As for MongoDB, and NoSQL in general, I think it&#8217;s best served as an edge case storage medium for fast read operations.  Data is relational.

Alas, here is a screenshot of the recipe database.  I&#8217;ll be adding lots of features, including menus and weight management.  You can&#8217;t see it here, but it has a cool tag cloud feature and theme switching.

[<img class="alignnone size-medium wp-image-640" title="recipes" src="https://i2.wp.com/blog.cleverswine.net/wp-content/uploads/2011/03/recipes.png?resize=300%2C139" alt="Recipes" srcset="https://i2.wp.com/blog.cleverswine.net/wp-content/uploads/2011/03/recipes.png?resize=300%2C139 300w, https://i2.wp.com/blog.cleverswine.net/wp-content/uploads/2011/03/recipes.png?w=876 876w" sizes="(max-width: 300px) 85vw, 300px" data-recalc-dims="1" />](https://i2.wp.com/blog.cleverswine.net/wp-content/uploads/2011/03/recipes.png)
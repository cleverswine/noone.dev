---
author: Kevin
categories:
- Uncategorized
date: 2012-04-27T12:36:39Z
guid: http://blog.cleverswine.net/?p=774
id: 774
tags:
- Hacking
title: FizzBuzz one-liner in C#
url: /2012/04/27/fizzbuzz-one-liner-in-c/
wp-syntax-cache-content:
- |
  a:1:{i:1;s:4330:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="csharp" style="font-family:monospace;">Console<span style="color: #008000;">.</span><span style="color: #0000FF;">WriteLine</span><span style="color: #008000;">&#40;</span><span style="color: #6666cc; font-weight: bold;">String</span><span style="color: #008000;">.</span><span style="color: #0600FF; font-weight: bold;">Join</span><span style="color: #008000;">&#40;</span>Environment<span style="color: #008000;">.</span><span style="color: #0000FF;">NewLine</span>, Enumerable<span style="color: #008000;">.</span><span style="color: #0000FF;">Range</span><span style="color: #008000;">&#40;</span><span style="color: #FF0000;">1</span>, <span style="color: #FF0000;">100</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">.</span><span style="color: #0600FF; font-weight: bold;">Select</span><span style="color: #008000;">&#40;</span>i <span style="color: #008000;">=&gt;</span> <span style="color: #008000;">&#40;</span>i <span style="color: #008000;">%</span> <span style="color: #FF0000;">3</span> <span style="color: #008000;">==</span> <span style="color: #FF0000;">0</span><span style="color: #008000;">&#41;</span> <span style="color: #008000;">?</span> <span style="color: #008000;">new</span> <span style="color: #008000;">&#123;</span> n <span style="color: #008000;">=</span> i, w <span style="color: #008000;">=</span> <span style="color: #666666;">&quot;Fizz&quot;</span> <span style="color: #008000;">&#125;</span> <span style="color: #008000;">:</span> <span style="color: #008000;">new</span> <span style="color: #008000;">&#123;</span> n <span style="color: #008000;">=</span> i, w <span style="color: #008000;">=</span> <span style="color: #666666;">&quot;&quot;</span> <span style="color: #008000;">&#125;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">.</span><span style="color: #0600FF; font-weight: bold;">Select</span><span style="color: #008000;">&#40;</span>o <span style="color: #008000;">=&gt;</span> <span style="color: #008000;">&#40;</span>o<span style="color: #008000;">.</span><span style="color: #0000FF;">n</span> <span style="color: #008000;">%</span> <span style="color: #FF0000;">5</span> <span style="color: #008000;">==</span> <span style="color: #FF0000;">0</span><span style="color: #008000;">&#41;</span> <span style="color: #008000;">?</span> <span style="color: #008000;">new</span> <span style="color: #008000;">&#123;</span> n <span style="color: #008000;">=</span> o<span style="color: #008000;">.</span><span style="color: #0000FF;">n</span>, w <span style="color: #008000;">=</span> o<span style="color: #008000;">.</span><span style="color: #0000FF;">w</span> <span style="color: #008000;">+</span> <span style="color: #666666;">&quot;Buzz&quot;</span> <span style="color: #008000;">&#125;</span> <span style="color: #008000;">:</span> o<span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">.</span><span style="color: #0600FF; font-weight: bold;">Select</span><span style="color: #008000;">&#40;</span>o <span style="color: #008000;">=&gt;</span> o<span style="color: #008000;">.</span><span style="color: #0000FF;">w</span> <span style="color: #008000;">==</span> <span style="color: #666666;">&quot;&quot;</span> <span style="color: #008000;">?</span> o<span style="color: #008000;">.</span><span style="color: #0000FF;">n</span><span style="color: #008000;">.</span><span style="color: #0000FF;">ToString</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span> <span style="color: #008000;">:</span> o<span style="color: #008000;">.</span><span style="color: #0000FF;">w</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span></pre></td></tr></table><p class="theCode" style="display:none;">Console.WriteLine(String.Join(Environment.NewLine, Enumerable.Range(1, 100)
  .Select(i =&gt; (i % 3 == 0) ? new { n = i, w = &quot;Fizz&quot; } : new { n = i, w = &quot;&quot; })
  .Select(o =&gt; (o.n % 5 == 0) ? new { n = o.n, w = o.w + &quot;Buzz&quot; } : o)
  .Select(o =&gt; o.w == &quot;&quot; ? o.n.ToString() : o.w)));</p></div>
  ";}
---

FizzBuzz: _Write a program that prints the numbers from 1 to 100. But for multiples of three print &#8220;Fizz&#8221; instead of the number and for the multiples of five print &#8220;Buzz&#8221;. For numbers which are multiples of both three and five print &#8220;FizzBuzz&#8221;. <a href="http://www.codinghorror.com/blog/2007/02/why-cant-programmers-program.html" target="_blank">Reference</a>_

<pre lang="csharp">Console.WriteLine(String.Join(Environment.NewLine, Enumerable.Range(1, 100)
    .Select(i => (i % 3 == 0) ? new { n = i, w = "Fizz" } : new { n = i, w = "" })
    .Select(o => (o.n % 5 == 0) ? new { n = o.n, w = o.w + "Buzz" } : o)
    .Select(o => o.w == "" ? o.n.ToString() : o.w)));
</pre>
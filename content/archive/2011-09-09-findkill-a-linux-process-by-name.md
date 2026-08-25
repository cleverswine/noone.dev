---
author: Kevin
categories:
- Uncategorized
date: 2011-09-09T17:19:40Z
guid: http://blog.cleverswine.net/?p=730
id: 730
tags:
- Hacking
- MiscTech
title: Find/Kill a Linux Process by Name
url: /2011/09/09/findkill-a-linux-process-by-name/
---

I was doing some research to figure out how to easily kill a Linux process given part of it&#8217;s name. It&#8217;s fairly simple to do it manually, but if you didn&#8217;t already know, programmers are lazy.

The first thing I was doing&#8230;
  
> ps aux | grep Foo
  
find the process ID (PID) with my eyes, and then kill it&#8230;
  
> kill -9 123456

Next, I started finding complex piped commands to find and extract the PID. For instance&#8230;
  
> pid=\`ps -eo pid,args | grep Foo | grep -v grep | cut -c1-6\`

and slightly simpler&#8230;
  
> ps aux | grep Foo | grep -v grep | awk &#8216;{print $2}&#8217;

Finally, I discovered <a href="http://www.linuxmanpages.com/man1/pgrep.1.php" title="pgrep" target="_blank">pgrep and pkill</a>. There are quite a few options for each command, but simply put:

To get the PID for a process with a name containing &#8216;Foo&#8217;&#8230;
  
> pgrep Foo

To get the PID for a process with a command line containing &#8216;Bar&#8217; (as in > Foo -n Bar)&#8230;
  
> pgrep -f Bar
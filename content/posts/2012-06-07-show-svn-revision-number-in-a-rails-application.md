---
author: Kevin
categories:
- Uncategorized
date: 2012-06-07T16:06:18Z
guid: http://blog.cleverswine.net/?p=785
id: 785
tags:
- Hacking
title: Show SVN revision number in a Rails application
url: /2012/06/07/show-svn-revision-number-in-a-rails-application/
wp-syntax-cache-content:
- |
  a:3:{i:1;s:582:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="ruby" style="font-family:monospace;">version 1.0.<span style="color:#006600; font-weight:bold;">&lt;%</span>=render <span style="color:#ff3333; font-weight:bold;">:partial</span> <span style="color:#006600; font-weight:bold;">=&gt;</span> <span style="color:#996600;">'layouts/revision'</span><span style="color:#006600; font-weight:bold;">%&gt;</span></pre></td></tr></table><p class="theCode" style="display:none;">version 1.0.&lt;%=render :partial =&gt; 'layouts/revision'%&gt;</p></div>
  ;i:2;s:875:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="ruby" style="font-family:monospace;">desc <span style="color:#996600;">&quot;Write current revision to app/layouts/_revision.rhtml&quot;</span>
  task <span style="color:#ff3333; font-weight:bold;">:publish_revision</span> <span style="color:#9966CC; font-weight:bold;">do</span>
  run <span style="color:#996600;">&quot;svnversion #{deploy_to}/current/app &gt; #{deploy_to}/current/app/views/layouts/_revision.html.erb&quot;</span>
  <span style="color:#9966CC; font-weight:bold;">end</span></pre></td></tr></table><p class="theCode" style="display:none;">desc &quot;Write current revision to app/layouts/_revision.rhtml&quot;
  task :publish_revision do
  run &quot;svnversion #{deploy_to}/current/app &gt; #{deploy_to}/current/app/views/layouts/_revision.html.erb&quot;
  end</p></div>
  ;i:3;s:362:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="ruby" style="font-family:monospace;">before <span style="color:#996600;">'deploy:restart'</span>, <span style="color:#996600;">'publish_revision'</span></pre></td></tr></table><p class="theCode" style="display:none;">before 'deploy:restart', 'publish_revision'</p></div>
  ";}
---

This is a hack to display the Subversion revision in a Rails application using <a href="http://capify.org" title="Capistrano" target="_blank">Capistrano</a> to insert the current revision number on each deploy.

Create a partial that will contain the revision number. For example, _~/app/views/layouts/_revision.html.erb_. You can put anything in that file, as it will be overwritten by Capistrano on each deploy. The word &#8220;Development&#8221; or &#8220;Current&#8221; seems a good choice.

In the page where you want to display the revision number (probably in the application layout), render the partial. In development mode, you&#8217;ll see something like &#8220;version 1.0.Development&#8221;. In your deployment environment, you&#8217;ll see the SVN revision number, like so: &#8220;version 1.0.504&#8221;.

<pre lang="ruby">version 1.0.&lt;%=render :partial => 'layouts/revision'%></pre>

To set up Capistrano, edit the _~/config/deploy.rb_ file (assuming you are already using Capistrano for deployments). Add a task to get the SVN version and write that to your revision partial. For some reason, pointing to the &#8220;/current&#8221; directory doesn&#8217;t work &#8211; probably because of the way Capistrano links directories. Therefore, just point SVN to any subdirectory or file to get the version.

<pre lang="ruby">desc "Write current revision to app/layouts/_revision.rhtml"
task :publish_revision do
  run "svnversion #{deploy_to}/current/app > #{deploy_to}/current/app/views/layouts/_revision.html.erb"
end</pre>

And tell Capistrano to run this task just before restarting the application:

<pre lang="ruby">before 'deploy:restart', 'publish_revision'</pre>
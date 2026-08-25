---
author: Kevin
categories:
- Uncategorized
date: 2012-03-09T17:00:07Z
guid: http://blog.cleverswine.net/?p=744
id: 744
tags:
- Hacking
title: jQuery Autocomplete and Rails
url: /2012/03/09/jquery-autocomplete-and-rails/
wp-syntax-cache-content:
- |
  a:7:{i:1;s:276:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="ruby" style="font-family:monospace;">gem <span style="color:#996600;">'jquery-rails'</span></pre></td></tr></table><p class="theCode" style="display:none;">gem 'jquery-rails'</p></div>
  ;i:2;s:710:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="ruby" style="font-family:monospace;"><span style="color:#006600; font-weight:bold;">//</span>= <span style="color:#CC0066; font-weight:bold;">require</span> jquery
  <span style="color:#006600; font-weight:bold;">//</span>= <span style="color:#CC0066; font-weight:bold;">require</span> jquery_ujs
  <span style="color:#006600; font-weight:bold;">//</span>= <span style="color:#CC0066; font-weight:bold;">require</span> jquery<span style="color:#006600; font-weight:bold;">-</span>ui</pre></td></tr></table><p class="theCode" style="display:none;">//= require jquery
  //= require jquery_ujs
  //= require jquery-ui</p></div>
  ;i:3;s:568:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="ruby" style="font-family:monospace;"><span style="color:#006600; font-weight:bold;">*</span>= <span style="color:#CC0066; font-weight:bold;">require</span> jquery<span style="color:#006600; font-weight:bold;">-</span>ui<span style="color:#006600; font-weight:bold;">-</span>1.8.18.<span style="color:#9900CC;">custom</span>.<span style="color:#9900CC;">css</span></pre></td></tr></table><p class="theCode" style="display:none;">*= require jquery-ui-1.8.18.custom.css</p></div>
  ;i:4;s:1519:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="ruby" style="font-family:monospace;">  <span style="color:#9966CC; font-weight:bold;">def</span> search_on_title
  <span style="color:#008000; font-style:italic;"># search_on_title is a method on the model to do a wildcard search on keyword</span>
  recipes = Recipe.<span style="color:#9900CC;">search_on_title</span><span style="color:#006600; font-weight:bold;">&#40;</span>params<span style="color:#006600; font-weight:bold;">&#91;</span><span style="color:#ff3333; font-weight:bold;">:term</span><span style="color:#006600; font-weight:bold;">&#93;</span>, params<span style="color:#006600; font-weight:bold;">&#91;</span><span style="color:#ff3333; font-weight:bold;">:category_id</span><span style="color:#006600; font-weight:bold;">&#93;</span><span style="color:#006600; font-weight:bold;">&#41;</span>
  render json: recipes.<span style="color:#9900CC;">map</span><span style="color:#006600; font-weight:bold;">&#40;</span><span style="color:#006600; font-weight:bold;">&amp;</span>:title<span style="color:#006600; font-weight:bold;">&#41;</span>
  <span style="color:#9966CC; font-weight:bold;">end</span></pre></td></tr></table><p class="theCode" style="display:none;">  def search_on_title
  # search_on_title is a method on the model to do a wildcard search on keyword
  recipes = Recipe.search_on_title(params[:term], params[:category_id])
  render json: recipes.map(&amp;:title)
  end</p></div>
  ;i:5;s:722:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="ruby" style="font-family:monospace;">  resources <span style="color:#ff3333; font-weight:bold;">:recipes</span> <span style="color:#9966CC; font-weight:bold;">do</span>
  get <span style="color:#996600;">'search_on_title'</span>, <span style="color:#ff3333; font-weight:bold;">:on</span> <span style="color:#006600; font-weight:bold;">=&gt;</span> <span style="color:#ff3333; font-weight:bold;">:collection</span>
  <span style="color:#9966CC; font-weight:bold;">end</span></pre></td></tr></table><p class="theCode" style="display:none;">  resources :recipes do
  get 'search_on_title', :on =&gt; :collection
  end</p></div>
  ;i:6;s:2353:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="ruby" style="font-family:monospace;">  <span style="color:#006600; font-weight:bold;">&lt;%</span>= form_tag <span style="color:#996600;">''</span>, <span style="color:#ff3333; font-weight:bold;">:method</span> <span style="color:#006600; font-weight:bold;">=&gt;</span> <span style="color:#ff3333; font-weight:bold;">:get</span> <span style="color:#9966CC; font-weight:bold;">do</span> <span style="color:#006600; font-weight:bold;">%&gt;</span>
  <span style="color:#006600; font-weight:bold;">&lt;%</span>= search_field_tag <span style="color:#ff3333; font-weight:bold;">:term</span>, params<span style="color:#006600; font-weight:bold;">&#91;</span><span style="color:#ff3333; font-weight:bold;">:term</span><span style="color:#006600; font-weight:bold;">&#93;</span>, :<span style="color:#9966CC; font-weight:bold;">class</span><span style="color:#006600; font-weight:bold;">=&gt;</span> <span style="color:#996600;">&quot;input-large&quot;</span> <span style="color:#006600; font-weight:bold;">%&gt;</span>
  <span style="color:#006600; font-weight:bold;">&lt;%</span>= submit_tag <span style="color:#996600;">&quot;Filter&quot;</span>, <span style="color:#ff3333; font-weight:bold;">:name</span> <span style="color:#006600; font-weight:bold;">=&gt;</span> <span style="color:#0000FF; font-weight:bold;">nil</span>, :<span style="color:#9966CC; font-weight:bold;">class</span> <span style="color:#006600; font-weight:bold;">=&gt;</span> <span style="color:#996600;">'button btn'</span>, <span style="color:#ff3333; font-weight:bold;">:id</span> <span style="color:#006600; font-weight:bold;">=&gt;</span> <span style="color:#996600;">&quot;search_bn&quot;</span> <span style="color:#006600; font-weight:bold;">%&gt;</span>
  <span style="color:#006600; font-weight:bold;">&lt;%</span> <span style="color:#9966CC; font-weight:bold;">end</span> <span style="color:#006600; font-weight:bold;">%&gt;</span></pre></td></tr></table><p class="theCode" style="display:none;">  &lt;%= form_tag '', :method =&gt; :get do %&gt;
  &lt;%= search_field_tag :term, params[:term], :class=&gt; &quot;input-large&quot; %&gt;
  &lt;%= submit_tag &quot;Filter&quot;, :name =&gt; nil, :class =&gt; 'button btn', :id =&gt; &quot;search_bn&quot; %&gt;
  &lt;% end %&gt;</p></div>
  ;i:7;s:1212:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="javascript" style="font-family:monospace;">$<span style="color: #009900;">&#40;</span><span style="color: #3366CC;">&quot;input#term&quot;</span><span style="color: #009900;">&#41;</span>.<span style="color: #660066;">autocomplete</span><span style="color: #009900;">&#40;</span><span style="color: #009900;">&#123;</span>
  source<span style="color: #339933;">:</span> <span style="color: #3366CC;">'&lt;%=search_on_title_recipes_path%&gt;?category_id=&lt;%=params[:category_id]%&gt;'</span><span style="color: #339933;">,</span>
  minLength<span style="color: #339933;">:</span> <span style="color: #CC0000;">2</span><span style="color: #339933;">,</span> delay<span style="color: #339933;">:</span> <span style="color: #CC0000;">500</span>
  <span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span></pre></td></tr></table><p class="theCode" style="display:none;">$(&quot;input#term&quot;).autocomplete({
  source: '&lt;%=search_on_title_recipes_path%&gt;?category_id=&lt;%=params[:category_id]%&gt;',
  minLength: 2, delay: 500
  });</p></div>
  ";}
---

This is an example of how I used the <a href="http://jqueryui.com/demos/autocomplete/" target="_blank">jQuery Automcomplete Widget</a> with Rails 3.2 for a recipe search field. Some of the jQuery stuff mentioned below is included out of the box with a new Rails application.

  * Make sure the jQuery gem is referenced in the _Gemfile_

<pre lang="ruby">gem 'jquery-rails'</pre>

  * Include the jQuery javascripts in _app/assets/javascripts/application.js_

<pre lang="ruby">//= require jquery
//= require jquery_ujs
//= require jquery-ui</pre>

  * Download one of the jQuery-ui css files, or a custom one, and put it in one of you asset locations. I put mine in vendor/assets/stylesheets/jquery-ui-1.8.18.custom.css. Include the css file in _app/assets/stylesheets/application.css_

<pre lang="ruby">*= require jquery-ui-1.8.18.custom.css</pre>

  * Create a controller action to search and return values for the autocomplete dropdown menu. In my Recipes controller (you could also create a seperate Search controller), I created an Action to return a list of matching recipe titles as a json array. The search can also be applied to recipes in a single category, hence the category_id parameter.

<pre lang="ruby">def search_on_title
    # search_on_title is a method on the model to do a wildcard search on keyword
    recipes = Recipe.search_on_title(params[:term], params[:category_id])
    render json: recipes.map(&:title)
  end</pre>

  * Add a route for the search\_on\_title action.

<pre lang="ruby">resources :recipes do
    get 'search_on_title', :on => :collection
  end</pre>

  * The search form is just a normal form.

<pre lang="ruby">&lt;%= form_tag '', :method => :get do %>
	&lt;%= search_field_tag :term, params[:term], :class=> "input-large" %>
	&lt;%= submit_tag "Filter", :name => nil, :class => 'button btn', :id => "search_bn" %>
  &lt;% end %>         
</pre>

  * The javascript to hook up the autocomplete functionality looks like this. Notice the extra parameter to the source url &#8211; that&#8217;s how you would pass multiple values to your filter.

<pre lang="javascript">$("input#term").autocomplete({
    source: '&lt;%=search_on_title_recipes_path%>?category_id=&lt;%=params[:category_id]%>',
    minLength: 2, delay: 500
});</pre>

There&#8217;s also a Rails gem to make autocomplete easier, but I found this to be easy enough such that I didn&#8217;t have to depend on an additional plugin.
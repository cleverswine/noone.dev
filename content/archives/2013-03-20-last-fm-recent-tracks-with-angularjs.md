---
author: Kevin
categories:
- Uncategorized
date: 2013-03-20T20:51:55Z
guid: http://blog.cleverswine.net/?p=841
id: 841
tags:
- angularjs
- bootstrap
- Hacking
title: Last.fm Recent Tracks with AngularJS
url: /2013/03/20/last-fm-recent-tracks-with-angularjs/
wp-syntax-cache-content:
- |
  a:2:{i:1;s:5228:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="javascript" style="font-family:monospace;"><span style="color: #3366CC;">'use strict'</span><span style="color: #339933;">;</span>
  &nbsp;
  <span style="color: #000066; font-weight: bold;">var</span> LastFmApp <span style="color: #339933;">=</span> angular.<span style="color: #660066;">module</span><span style="color: #009900;">&#40;</span><span style="color: #3366CC;">'lastfm-app'</span><span style="color: #339933;">,</span> <span style="color: #009900;">&#91;</span><span style="color: #009900;">&#93;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  &nbsp;
  LastFmApp.<span style="color: #660066;">controller</span><span style="color: #009900;">&#40;</span><span style="color: #3366CC;">'RecentTracksController'</span><span style="color: #339933;">,</span>
  <span style="color: #000066; font-weight: bold;">function</span> RecentTracksController<span style="color: #009900;">&#40;</span>$scope<span style="color: #339933;">,</span> $http<span style="color: #009900;">&#41;</span> <span style="color: #009900;">&#123;</span>
  <span style="color: #000066; font-weight: bold;">var</span> url <span style="color: #339933;">=</span> <span style="color: #3366CC;">'http://ws.audioscrobbler.com/2.0/'</span><span style="color: #339933;">;</span>
  <span style="color: #000066; font-weight: bold;">var</span> params <span style="color: #339933;">=</span> <span style="color: #009900;">&#123;</span>
  method<span style="color: #339933;">:</span> <span style="color: #3366CC;">'user.getrecenttracks'</span><span style="color: #339933;">,</span>
  api_key<span style="color: #339933;">:</span> <span style="color: #3366CC;">'your_api_key'</span><span style="color: #339933;">,</span>
  limit<span style="color: #339933;">:</span> <span style="color: #CC0000;">12</span><span style="color: #339933;">,</span>
  user<span style="color: #339933;">:</span> <span style="color: #3366CC;">'user_name'</span><span style="color: #339933;">,</span>
  format<span style="color: #339933;">:</span> <span style="color: #3366CC;">'json'</span>
  <span style="color: #009900;">&#125;</span><span style="color: #339933;">;</span>
  $http.<span style="color: #000066; font-weight: bold;">get</span><span style="color: #009900;">&#40;</span>url<span style="color: #339933;">,</span> <span style="color: #009900;">&#123;</span> params<span style="color: #339933;">:</span> params <span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span>
  .<span style="color: #660066;">success</span><span style="color: #009900;">&#40;</span><span style="color: #000066; font-weight: bold;">function</span> <span style="color: #009900;">&#40;</span>data<span style="color: #009900;">&#41;</span> <span style="color: #009900;">&#123;</span>
  $scope.<span style="color: #660066;">songs</span> <span style="color: #339933;">=</span> data.<span style="color: #660066;">recenttracks</span>.<span style="color: #660066;">track</span><span style="color: #339933;">;</span>
  <span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span>
  .<span style="color: #660066;">error</span><span style="color: #009900;">&#40;</span><span style="color: #000066; font-weight: bold;">function</span> <span style="color: #009900;">&#40;</span>data<span style="color: #339933;">,</span> status<span style="color: #009900;">&#41;</span> <span style="color: #009900;">&#123;</span>
  console.<span style="color: #660066;">log</span><span style="color: #009900;">&#40;</span>data <span style="color: #339933;">||</span> <span style="color: #3366CC;">&quot;Request failed&quot;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  console.<span style="color: #660066;">log</span><span style="color: #009900;">&#40;</span>status<span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  <span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span>
  <span style="color: #009900;">&#125;</span><span style="color: #009900;">&#41;</span><span style="color: #339933;">;</span></pre></td></tr></table><p class="theCode" style="display:none;">'use strict';

  var LastFmApp = angular.module('lastfm-app', []);

  LastFmApp.controller('RecentTracksController',
  function RecentTracksController($scope, $http) {
  var url = 'http://ws.audioscrobbler.com/2.0/';
  var params = {
  method: 'user.getrecenttracks',
  api_key: 'your_api_key',
  limit: 12,
  user: 'user_name',
  format: 'json'
  };
  $http.get(url, { params: params })
  .success(function (data) {
  $scope.songs = data.recenttracks.track;
  })
  .error(function (data, status) {
  console.log(data || &quot;Request failed&quot;);
  console.log(status);
  });
  });</p></div>
  ;i:2;s:4386:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="html" style="font-family:monospace;">&lt;!DOCTYPE html&gt;
  &lt;html lang=&quot;en&quot; ng-app=&quot;lastfm-app&quot;&gt;
  &lt;head&gt;
  &lt;meta charset=&quot;utf-8&quot;&gt;
  &lt;title&gt;Recent Tracks&lt;/title&gt;
  &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
  &lt;link rel=&quot;stylesheet&quot; href=&quot;lib/bootstrap/css/bootstrap.min.css&quot;&gt;
  &lt;link rel=&quot;stylesheet&quot; href=&quot;lib/bootstrap/css/bootstrap-responsive.min.css&quot;&gt;
  &lt;/head&gt;
  &lt;body&gt;
  &lt;div class=&quot;container&quot; ng-controller=&quot;RecentTracksController&quot;&gt;
  &lt;ul class=&quot;thumbnails&quot;&gt;
  &lt;li class=&quot;span4&quot; ng-repeat=&quot;song in songs&quot;&gt;
  &lt;a class=&quot;thumbnail&quot; href=&quot;{{song.url}}&quot; title=&quot;{{song.artist['#text']}} - {{song.name}}&quot; style=&quot;display: block;&quot;&gt;
  &lt;div class=&quot;media&quot;&gt;
  &lt;div class=&quot;pull-left&quot; href=&quot;#&quot;&gt;
  &lt;img class=&quot;media-object&quot; src=&quot;{{song.image[2]['#text']}}&quot;&gt;
  &lt;/div&gt;
  &lt;div class=&quot;media-body&quot;&gt;
  &lt;strong&gt;{{song.artist['#text']}}&lt;/strong&gt;&lt;br /&gt;
  {{song.name}}&lt;br /&gt;
  &lt;em&gt;{{song.date['#text']}}&lt;/em&gt;
  &lt;/div&gt;
  &lt;/div&gt;
  &lt;/a&gt;
  &lt;/li&gt;
  &lt;/ul&gt;
  &lt;/div&gt;
  &lt;script src=&quot;http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js&quot;&gt;&lt;/script&gt;
  &lt;script src=&quot;lib/bootstrap/js/bootstrap.min.js&quot;&gt;&lt;/script&gt;
  &lt;script src=&quot;https://ajax.googleapis.com/ajax/libs/angularjs/1.0.5/angular.min.js&quot;&gt;&lt;/script&gt;
  &lt;script src=&quot;js/main.js&quot;&gt;&lt;/script&gt;
  &lt;/body&gt;
  &lt;/html&gt;</pre></td></tr></table><p class="theCode" style="display:none;">&lt;!DOCTYPE html&gt;
  &lt;html lang=&quot;en&quot; ng-app=&quot;lastfm-app&quot;&gt;
  &lt;head&gt;
  &lt;meta charset=&quot;utf-8&quot;&gt;
  &lt;title&gt;Recent Tracks&lt;/title&gt;
  &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
  &lt;link rel=&quot;stylesheet&quot; href=&quot;lib/bootstrap/css/bootstrap.min.css&quot;&gt;
  &lt;link rel=&quot;stylesheet&quot; href=&quot;lib/bootstrap/css/bootstrap-responsive.min.css&quot;&gt;
  &lt;/head&gt;
  &lt;body&gt;
  &lt;div class=&quot;container&quot; ng-controller=&quot;RecentTracksController&quot;&gt;
  &lt;ul class=&quot;thumbnails&quot;&gt;
  &lt;li class=&quot;span4&quot; ng-repeat=&quot;song in songs&quot;&gt;
  &lt;a class=&quot;thumbnail&quot; href=&quot;{{song.url}}&quot; title=&quot;{{song.artist['#text']}} - {{song.name}}&quot; style=&quot;display: block;&quot;&gt;
  &lt;div class=&quot;media&quot;&gt;
  &lt;div class=&quot;pull-left&quot; href=&quot;#&quot;&gt;
  &lt;img class=&quot;media-object&quot; src=&quot;{{song.image[2]['#text']}}&quot;&gt;
  &lt;/div&gt;
  &lt;div class=&quot;media-body&quot;&gt;
  &lt;strong&gt;{{song.artist['#text']}}&lt;/strong&gt;&lt;br /&gt;
  {{song.name}}&lt;br /&gt;
  &lt;em&gt;{{song.date['#text']}}&lt;/em&gt;
  &lt;/div&gt;
  &lt;/div&gt;
  &lt;/a&gt;
  &lt;/li&gt;
  &lt;/ul&gt;
  &lt;/div&gt;
  &lt;script src=&quot;http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js&quot;&gt;&lt;/script&gt;
  &lt;script src=&quot;lib/bootstrap/js/bootstrap.min.js&quot;&gt;&lt;/script&gt;
  &lt;script src=&quot;https://ajax.googleapis.com/ajax/libs/angularjs/1.0.5/angular.min.js&quot;&gt;&lt;/script&gt;
  &lt;script src=&quot;js/main.js&quot;&gt;&lt;/script&gt;
  &lt;/body&gt;
  &lt;/html&gt;</p></div>
  ";}
---

<a href="http://angularjs.org/" title="AngularJS" target="_blank">AngularJS</a> is an MVC framework for javascript. It is best suited for round-tripping resources with a RESTful service. In this post, I will simply get a list of recent tracks from Last.fm, treating each track as a resource. The Last.fm API is documented <a href="http://www.last.fm/api" title="Last.fm API" target="_blank">here</a>. There is no round-tripping in this example. I&#8217;ll also be utilizing <a href="http://twitter.github.com/bootstrap/" title="Bootstrap" target="_blank">Twitter Bootstrap</a> for layout.

To start, create the AngularJS application and a controller for recent tracks. Since this is a simple application, there are no routes and views. The controller will be invoked inline on the main page. Routes and views are easily added once you get a handle on the framework.

<pre lang="javascript">'use strict';

var LastFmApp = angular.module('lastfm-app', []);

LastFmApp.controller('RecentTracksController',
    function RecentTracksController($scope, $http) {
        var url = 'http://ws.audioscrobbler.com/2.0/';
        var params = {
            method: 'user.getrecenttracks',
            api_key: 'your_api_key',
            limit: 12,
            user: 'user_name',
            format: 'json'
        };
        $http.get(url, { params: params })
            .success(function (data) {
                $scope.songs = data.recenttracks.track;
            })
            .error(function (data, status) {
                console.log(data || "Request failed");
                console.log(status);
            });       
    });</pre>

The HTML (simplified for brevity). 

<pre lang="html">



    

<div class="container" ng-controller="RecentTracksController">
  <ul class="thumbnails">
    <li class="span4" ng-repeat="song in songs">
      <a class="thumbnail" href="{{song.url}}" title="{{song.artist['#text']}} - {{song.name}}" style="display: block;">
                          
      
      <div class="media">
        <div class="pull-left" href="#">
          <img class="media-object" src="{{song.image[2]['#text']}}" />
                                  
        </div>
                                
        
        <div class="media-body">
          <strong>{{song.artist['#text']}}</strong><br />
                                      {{song.name}}<br />
                                      <em>{{song.date['#text']}}</em>
                                  
        </div>
                            
      </div>
                      </a>
                  
    </li>
            
  </ul>
      
</div>
    
    
    
    

</pre>

The resulting grid will look something like this:

[<img src="https://i1.wp.com/blog.cleverswine.net/wp-content/uploads/2013/03/Capture.png?resize=300%2C167" alt="Recent Tracks" class="aligncenter size-medium wp-image-842" srcset="https://i1.wp.com/blog.cleverswine.net/wp-content/uploads/2013/03/Capture.png?resize=300%2C167 300w, https://i1.wp.com/blog.cleverswine.net/wp-content/uploads/2013/03/Capture.png?resize=624%2C349 624w, https://i1.wp.com/blog.cleverswine.net/wp-content/uploads/2013/03/Capture.png?w=631 631w" sizes="(max-width: 300px) 85vw, 300px" data-recalc-dims="1" />](https://i1.wp.com/blog.cleverswine.net/wp-content/uploads/2013/03/Capture.png)
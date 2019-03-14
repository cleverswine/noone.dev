---
author: Kevin
categories:
- Uncategorized
date: 2013-08-06T18:06:41Z
guid: http://blog.cleverswine.net/?p=879
id: 879
tags:
- Hacking
title: Using cookies with HttpClient
url: /2013/08/06/using-cookies-with-httpclient/
wp-syntax-cache-content:
- |
  a:1:{i:1;s:6837:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">class</span> WebClient
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">private</span> CookieContainer _cookieContainer<span style="color: #008000;">;</span>
  <span style="color: #0600FF; font-weight: bold;">private</span> HttpClientHandler _handler<span style="color: #008000;">;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">private</span> HttpClientHandler Handler
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">get</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">if</span> <span style="color: #008000;">&#40;</span>_handler <span style="color: #008000;">==</span> <span style="color: #0600FF; font-weight: bold;">null</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  _cookieContainer <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> CookieContainer<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  _handler <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> HttpClientHandler <span style="color: #008000;">&#123;</span> CookieContainer <span style="color: #008000;">=</span> _cookieContainer, UseCookies <span style="color: #008000;">=</span> <span style="color: #0600FF; font-weight: bold;">true</span>, AllowAutoRedirect <span style="color: #008000;">=</span> <span style="color: #0600FF; font-weight: bold;">false</span> <span style="color: #008000;">&#125;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #0600FF; font-weight: bold;">return</span> _handler<span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">private</span> HttpClient GetClient<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #008080; font-style: italic;">// the second param prevents the Handler from being disposed with the client</span>
  <span style="color: #0600FF; font-weight: bold;">var</span> client <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> HttpClient<span style="color: #008000;">&#40;</span>Handler, <span style="color: #0600FF; font-weight: bold;">false</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  client<span style="color: #008000;">.</span><span style="color: #0000FF;">DefaultRequestHeaders</span><span style="color: #008000;">.</span><span style="color: #0000FF;">Accept</span><span style="color: #008000;">.</span><span style="color: #0600FF; font-weight: bold;">Add</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">new</span> MediaTypeWithQualityHeaderValue<span style="color: #008000;">&#40;</span><span style="color: #666666;">&quot;application/json&quot;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #0600FF; font-weight: bold;">return</span> client<span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> async Task <span style="color: #0600FF; font-weight: bold;">Get</span><span style="color: #008000;">&lt;</span>T<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #6666cc; font-weight: bold;">string</span> path<span style="color: #008000;">&#41;</span> <span style="color: #0600FF; font-weight: bold;">where</span> T <span style="color: #008000;">:</span> <span style="color: #6666cc; font-weight: bold;">class</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">using</span> <span style="color: #008000;">&#40;</span><span style="color: #0600FF; font-weight: bold;">var</span> client <span style="color: #008000;">=</span> GetClient<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">var</span> content <span style="color: #008000;">=</span> await client<span style="color: #008000;">.</span><span style="color: #0000FF;">GetStringAsync</span><span style="color: #008000;">&#40;</span>path<span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #0600FF; font-weight: bold;">return</span> JsonConvert<span style="color: #008000;">.</span><span style="color: #0000FF;">DeserializeObject</span><span style="color: #008000;">&lt;</span>T<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span>content<span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">public class WebClient
  {
  private CookieContainer _cookieContainer;
  private HttpClientHandler _handler;

  private HttpClientHandler Handler
  {
  get
  {
  if (_handler == null)
  {
  _cookieContainer = new CookieContainer();
  _handler = new HttpClientHandler { CookieContainer = _cookieContainer, UseCookies = true, AllowAutoRedirect = false };
  }
  return _handler;
  }
  }

  private HttpClient GetClient()
  {
  // the second param prevents the Handler from being disposed with the client
  var client = new HttpClient(Handler, false);
  client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue(&quot;application/json&quot;));
  return client;
  }

  public async Task Get&lt;T&gt;(string path) where T : class
  {
  using (var client = GetClient())
  {
  var content = await client.GetStringAsync(path);
  return JsonConvert.DeserializeObject&lt;T&gt;(content);
  }
  }
  }</p></div>
  ";}
---

This simple code snippet shows how to store and use cookies from a web request using the .Net <a href="http://msdn.microsoft.com/en-us/library/system.net.http.httpclient.aspx" target="_blank">HttpClient</a>. For example, if an API or site sends a token or authentication cookie, you may need to store it and send it on all subsequent requests to show that you are authenticated. This would be for a session-based service. The key is to create a single <a href="http://msdn.microsoft.com/en-us/library/system.net.cookiecontainer.aspx" target="_blank">CookieContainer</a> instance and a single <a href="http://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler.aspx" target="_blank">HttpClientHandler</a> instance to hold it. Assume the following class is registered as a singleton.

<pre lang="csharp">public class WebClient
{
    private CookieContainer _cookieContainer;
    private HttpClientHandler _handler;

    private HttpClientHandler Handler
    {
        get
        {
            if (_handler == null)
            {
                _cookieContainer = new CookieContainer();
                _handler = new HttpClientHandler { CookieContainer = _cookieContainer, UseCookies = true, AllowAutoRedirect = false };
            }
            return _handler;
        }
    }

    private HttpClient GetClient()
    {
        // the second param prevents the Handler from being disposed with the client
        var client = new HttpClient(Handler, false);
        client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        return client;
    }

    public async Task Get&lt;T>(string path) where T : class
    {
        using (var client = GetClient())
        {
            var content = await client.GetStringAsync(path);
            return JsonConvert.DeserializeObject&lt;T>(content);
        }
    }
}</pre>
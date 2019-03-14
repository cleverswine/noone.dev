---
author: Kevin
categories:
- Uncategorized
date: 2011-09-26T17:55:56Z
guid: http://blog.cleverswine.net/?p=735
id: 735
tags:
- Hacking
title: WCF Client and the &#8220;using&#8221; Statement
url: /2011/09/26/wcf-client-and-the-using-statement/
wp-syntax-cache-content:
- |
  a:1:{i:1;s:8011:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #008080; font-style: italic;">// Get a disposable MyClientWrapper object</span>
  <span style="color: #0600FF; font-weight: bold;">using</span><span style="color: #008000;">&#40;</span><span style="color: #0600FF; font-weight: bold;">var</span> clientWrapper <span style="color: #008000;">=</span> ClientFactory<span style="color: #008000;">.</span><span style="color: #0000FF;">GetMyClientWrapper</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  clientWrapper<span style="color: #008000;">.</span><span style="color: #0000FF;">Client</span><span style="color: #008000;">.</span><span style="color: #0000FF;">DoSomething</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">class</span> MyClientWrapper <span style="color: #008000;">:</span> IDisposable
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">private</span> <span style="color: #0600FF; font-weight: bold;">readonly</span> MyClient _client<span style="color: #008000;">;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> MyClientWrapper<span style="color: #008000;">&#40;</span>Binding binding, Uri endpoint<span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #008080; font-style: italic;">// creates an instance of MyClient, which is derived from ClientBase&lt;T&gt;</span>
  _client <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> MyClient<span style="color: #008000;">&#40;</span>binding, <span style="color: #008000;">new</span> EndpointAddress<span style="color: #008000;">&#40;</span>endpoint<span style="color: #008000;">.</span><span style="color: #0000FF;">ToString</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> MyClient Client
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">get</span> <span style="color: #008000;">&#123;</span> <span style="color: #0600FF; font-weight: bold;">return</span> _client<span style="color: #008000;">;</span> <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">void</span> Dispose<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #008080; font-style: italic;">// safe dispose with extension method (below)</span>
  _client<span style="color: #008000;">.</span><span style="color: #0000FF;">CloseProxy</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #008080; font-style: italic;">// extension method for safe Close() on ClientBase&lt;T&gt;</span>
  <span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #0600FF; font-weight: bold;">static</span> <span style="color: #6666cc; font-weight: bold;">void</span> CloseProxy<span style="color: #008000;">&#40;</span><span style="color: #0600FF; font-weight: bold;">this</span> ClientBase<span style="color: #008000;">&lt;</span>T<span style="color: #008000;">&gt;</span> proxy<span style="color: #008000;">&#41;</span> <span style="color: #0600FF; font-weight: bold;">where</span> T <span style="color: #008000;">:</span> <span style="color: #6666cc; font-weight: bold;">class</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">try</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">if</span> <span style="color: #008000;">&#40;</span>proxy<span style="color: #008000;">.</span><span style="color: #0000FF;">State</span> <span style="color: #008000;">!=</span> CommunicationState<span style="color: #008000;">.</span><span style="color: #0000FF;">Closed</span>
  <span style="color: #008000;">&amp;&amp;</span> proxy<span style="color: #008000;">.</span><span style="color: #0000FF;">State</span> <span style="color: #008000;">!=</span> CommunicationState<span style="color: #008000;">.</span><span style="color: #0000FF;">Faulted</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  proxy<span style="color: #008000;">.</span><span style="color: #0000FF;">Close</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span> <span style="color: #008080; font-style: italic;">// may throw exception while closing</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #0600FF; font-weight: bold;">else</span>
  <span style="color: #008000;">&#123;</span>
  proxy<span style="color: #008000;">.</span><span style="color: #0000FF;">Abort</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #0600FF; font-weight: bold;">catch</span> <span style="color: #008000;">&#40;</span>CommunicationException<span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  proxy<span style="color: #008000;">.</span><span style="color: #0000FF;">Abort</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #0600FF; font-weight: bold;">throw</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">// Get a disposable MyClientWrapper object
  using(var clientWrapper = ClientFactory.GetMyClientWrapper())
  {
  clientWrapper.Client.DoSomething();
  }

  public class MyClientWrapper : IDisposable
  {
  private readonly MyClient _client;

  public MyClientWrapper(Binding binding, Uri endpoint)
  {
  // creates an instance of MyClient, which is derived from ClientBase&lt;T&gt;
  _client = new MyClient(binding, new EndpointAddress(endpoint.ToString()));
  }

  public MyClient Client
  {
  get { return _client; }
  }

  public void Dispose()
  {
  // safe dispose with extension method (below)
  _client.CloseProxy();
  }
  }

  // extension method for safe Close() on ClientBase&lt;T&gt;
  public static void CloseProxy(this ClientBase&lt;T&gt; proxy) where T : class
  {
  try
  {
  if (proxy.State != CommunicationState.Closed
  &amp;&amp; proxy.State != CommunicationState.Faulted)
  {
  proxy.Close(); // may throw exception while closing
  }
  else
  {
  proxy.Abort();
  }
  }
  catch (CommunicationException)
  {
  proxy.Abort();
  throw;
  }
  }</p></div>
  ";}
---

As I was researching patterns and practices for a WCF client implementation, I came across a bug in the .Net implementation of ClientBase<T> (whether it&#8217;s actually a bug is arguable). It&#8217;s well documented on the web, but it could cause major headaches if you happened to miss it.

The core of the issue is that ClientBase<T> implements IDisposable. With this knowledge, just about any programmer would naturally wrap its usage in a using{} block. Digging deeper, you&#8217;ll find that the ClientBase<T> Dispose() method simply calls Close() on the underlying connection. This is problematic because in certain connection states (such as &#8220;Faulted&#8221;), the Close() will fail. As I said, this is well documented elsewhere, for example: <a title="Avoiding Problems with the Using Statement" href="http://msdn.microsoft.com/en-us/library/aa355056.aspx" target="_blank">on MSDN</a>, <a href="http://stackoverflow.com/questions/573872/what-is-the-best-workaround-for-the-wcf-client-using-block-issue" title="What is the best workaround for the WCF client `using` block issue?" target="_blank">Stack Overflow</a>, and <a href="http://blogs.msdn.com/b/jjameson/archive/2010/03/18/avoiding-problems-with-the-using-statement-and-wcf-service-proxies.aspx" title="Avoiding Problems with the Using Statement and WCF Service Proxies" target="_blank">here</a>.

There are a few different solutions to this issue. I went with a simple Disposable wrapper&#8230;

<pre lang="csharp">// Get a disposable MyClientWrapper object
using(var clientWrapper = ClientFactory.GetMyClientWrapper())
{
    clientWrapper.Client.DoSomething();
}

public class MyClientWrapper : IDisposable
{
    private readonly MyClient _client;

    public MyClientWrapper(Binding binding, Uri endpoint)
    {
        // creates an instance of MyClient, which is derived from ClientBase&lt;T>
        _client = new MyClient(binding, new EndpointAddress(endpoint.ToString()));
    }

    public MyClient Client
    {
        get { return _client; }
    }

    public void Dispose()
    {
        // safe dispose with extension method (below)
        _client.CloseProxy();
    }
}

// extension method for safe Close() on ClientBase&lt;T>
public static void CloseProxy(this ClientBase&lt;T> proxy) where T : class
{
    try
    {
        if (proxy.State != CommunicationState.Closed 
                && proxy.State != CommunicationState.Faulted)
        {
            proxy.Close(); // may throw exception while closing
        }
        else
        {
            proxy.Abort();
        }
    }
    catch (CommunicationException)
    {
        proxy.Abort();
        throw;
    }
}</pre>
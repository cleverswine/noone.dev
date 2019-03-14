---
author: Kevin
categories:
- Uncategorized
date: 2011-04-20T17:40:32Z
guid: http://blog.cleverswine.net/?p=690
id: 690
tags:
- Hacking
title: Simple Moq
url: /2011/04/20/simple-moq/
wp-syntax-cache-content:
- |
  a:1:{i:1;s:4057:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">var</span> mockRepository <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> Mock<span style="color: #008000;">&lt;</span>IDocumentRepository<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  &nbsp;
  <span style="color: #008080; font-style: italic;">// if the query is for text/plain documents only, mock a response for that</span>
  mockRepository<span style="color: #008000;">.</span><span style="color: #0000FF;">Setup</span><span style="color: #008000;">&#40;</span>p <span style="color: #008000;">=&gt;</span> p<span style="color: #008000;">.</span><span style="color: #0000FF;">FindDocuments</span><span style="color: #008000;">&#40;</span>
  It<span style="color: #008000;">.</span><span style="color: #008000;">Is</span><span style="color: #008000;">&lt;</span>DocumentQueryParams<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span>q <span style="color: #008000;">=&gt;</span> q<span style="color: #008000;">.</span><span style="color: #0000FF;">MimeType</span> <span style="color: #008000;">==</span> <span style="color: #666666;">&quot;text/plain&quot;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">.</span><span style="color: #0000FF;">Returns</span><span style="color: #008000;">&#40;</span>DeSerializeResults<span style="color: #008000;">&lt;</span>List<span style="color: #008000;">&lt;</span>Document<span style="color: #008000;">&gt;&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #666666;">&quot;TextDocuments.xml&quot;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  &nbsp;
  <span style="color: #008080; font-style: italic;">// use It.IsAny to return the same result regardless of the parameters</span>
  mockRepository<span style="color: #008000;">.</span><span style="color: #0000FF;">Setup</span><span style="color: #008000;">&#40;</span>p <span style="color: #008000;">=&gt;</span> p<span style="color: #008000;">.</span><span style="color: #0000FF;">FindDocuments</span><span style="color: #008000;">&#40;</span>
  It<span style="color: #008000;">.</span><span style="color: #0000FF;">IsAny</span><span style="color: #008000;">&lt;</span>DocumentQueryParams<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">.</span><span style="color: #0000FF;">Returns</span><span style="color: #008000;">&#40;</span>DeSerializeResults<span style="color: #008000;">&lt;</span>List<span style="color: #008000;">&lt;</span>Document<span style="color: #008000;">&gt;&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #666666;">&quot;AllDocuments.xml&quot;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span></pre></td></tr></table><p class="theCode" style="display:none;">var mockRepository = new Mock&lt;IDocumentRepository&gt;();

  // if the query is for text/plain documents only, mock a response for that
  mockRepository.Setup(p =&gt; p.FindDocuments(
  It.Is&lt;DocumentQueryParams&gt;(q =&gt; q.MimeType == &quot;text/plain&quot;)))
  .Returns(DeSerializeResults&lt;List&lt;Document&gt;&gt;(&quot;TextDocuments.xml&quot;));

  // use It.IsAny to return the same result regardless of the parameters
  mockRepository.Setup(p =&gt; p.FindDocuments(
  It.IsAny&lt;DocumentQueryParams&gt;()))
  .Returns(DeSerializeResults&lt;List&lt;Document&gt;&gt;(&quot;AllDocuments.xml&quot;));</p></div>
  ";}
---

I recently worked on a project in which 90% of its tests were integration tests because the core purpose of the project was to interact with external entities. In an effort create more unit tests, I employed <a title="Moq" href="http://code.google.com/p/moq/" target="_blank">Moq</a> (and lots of refactoring).  This is an example of basic Moq usage.

Let&#8217;s say we have a repository that gets documents from a database.  The assumption is that a repository interface, IDocumentRepository, is being implemented.

In my case, I found it easier to deserialize results from file, rather than building the results by hand for each mocked response.  I used System.Xml.Serialization to serialize real responses to file.

Let&#8217;s Moq it&#8230;

<pre lang="csharp">var mockRepository = new Mock&lt;IDocumentRepository>();

// if the query is for text/plain documents only, mock a response for that
mockRepository.Setup(p => p.FindDocuments(
	It.Is&lt;DocumentQueryParams>(q => q.MimeType == "text/plain")))
	.Returns(DeSerializeResults&lt;List&lt;Document>>("TextDocuments.xml"));
	
// use It.IsAny to return the same result regardless of the parameters
mockRepository.Setup(p => p.FindDocuments(
	It.IsAny&lt;DocumentQueryParams>()))
	.Returns(DeSerializeResults&lt;List&lt;Document>>("AllDocuments.xml"));</pre>
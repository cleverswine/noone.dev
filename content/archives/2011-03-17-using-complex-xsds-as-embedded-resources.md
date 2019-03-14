---
author: Kevin
categories:
- Uncategorized
date: 2011-03-17T18:44:43Z
guid: http://blog.cleverswine.net/?p=662
id: 662
tags:
- Hacking
title: Complex XSDs as Embedded Resources
url: /2011/03/17/using-complex-xsds-as-embedded-resources/
wp-syntax-cache-content:
- |
  a:4:{i:1;s:4926:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">class</span> ManifestResourceResolver <span style="color: #008000;">:</span> XmlUrlResolver
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">private</span> <span style="color: #0600FF; font-weight: bold;">readonly</span> Assembly _resourceAssembly<span style="color: #008000;">;</span>
  <span style="color: #0600FF; font-weight: bold;">private</span> <span style="color: #0600FF; font-weight: bold;">readonly</span> <span style="color: #6666cc; font-weight: bold;">string</span> _baseNamespaceForReferences<span style="color: #008000;">;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> ManifestResourceResolver<span style="color: #008000;">&#40;</span>Assembly resourceAssembly, <span style="color: #6666cc; font-weight: bold;">string</span> baseNamespaceForReferences<span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  _resourceAssembly <span style="color: #008000;">=</span> resourceAssembly<span style="color: #008000;">;</span>
  _baseNamespaceForReferences <span style="color: #008000;">=</span> baseNamespaceForReferences<span style="color: #008000;">.</span><span style="color: #0000FF;">EndsWith</span><span style="color: #008000;">&#40;</span><span style="color: #666666;">&quot;.&quot;</span><span style="color: #008000;">&#41;</span> <span style="color: #008000;">?</span> baseNamespaceForReferences <span style="color: #008000;">:</span> baseNamespaceForReferences <span style="color: #008000;">+</span> <span style="color: #666666;">&quot;.&quot;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">override</span> <span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">object</span> GetEntity<span style="color: #008000;">&#40;</span>Uri absoluteUri, <span style="color: #6666cc; font-weight: bold;">string</span> role, Type ofObjectToReturn<span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">if</span> <span style="color: #008000;">&#40;</span>absoluteUri<span style="color: #008000;">.</span><span style="color: #0000FF;">IsFile</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">var</span> file <span style="color: #008000;">=</span> Path<span style="color: #008000;">.</span><span style="color: #0000FF;">GetFileName</span><span style="color: #008000;">&#40;</span>absoluteUri<span style="color: #008000;">.</span><span style="color: #0000FF;">AbsolutePath</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #0600FF; font-weight: bold;">var</span> stream <span style="color: #008000;">=</span> _resourceAssembly<span style="color: #008000;">.</span><span style="color: #0000FF;">GetManifestResourceStream</span><span style="color: #008000;">&#40;</span>_baseNamespaceForReferences <span style="color: #008000;">+</span> file<span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #0600FF; font-weight: bold;">return</span> stream<span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #0600FF; font-weight: bold;">return</span> <span style="color: #0600FF; font-weight: bold;">null</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">public class ManifestResourceResolver : XmlUrlResolver
  {
  private readonly Assembly _resourceAssembly;
  private readonly string _baseNamespaceForReferences;

  public ManifestResourceResolver(Assembly resourceAssembly, string baseNamespaceForReferences)
  {
  _resourceAssembly = resourceAssembly;
  _baseNamespaceForReferences = baseNamespaceForReferences.EndsWith(&quot;.&quot;) ? baseNamespaceForReferences : baseNamespaceForReferences + &quot;.&quot;;
  }

  override public object GetEntity(Uri absoluteUri, string role, Type ofObjectToReturn)
  {
  if (absoluteUri.IsFile)
  {
  var file = Path.GetFileName(absoluteUri.AbsolutePath);
  var stream = _resourceAssembly.GetManifestResourceStream(_baseNamespaceForReferences + file);
  return stream;
  }
  return null;
  }
  }</p></div>
  ;i:2;s:2861:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">var</span> xmlSchemaSet <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> XmlSchemaSet<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  xmlSchemaSet<span style="color: #008000;">.</span><span style="color: #0000FF;">XmlResolver</span> <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> ManifestResourceResolver<span style="color: #008000;">&#40;</span>Assembly<span style="color: #008000;">.</span><span style="color: #0000FF;">GetExecutingAssembly</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span>, <span style="color: #666666;">&quot;MyApp.Schemas&quot;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #0600FF; font-weight: bold;">using</span> <span style="color: #008000;">&#40;</span><span style="color: #0600FF; font-weight: bold;">var</span> stream <span style="color: #008000;">=</span> Assembly<span style="color: #008000;">.</span><span style="color: #0000FF;">GetCallingAssembly</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">.</span><span style="color: #0000FF;">GetManifestResourceStream</span><span style="color: #008000;">&#40;</span><span style="color: #666666;">&quot;MyApp.Schemas.MainSchema.xsd&quot;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">var</span> xmlSchema <span style="color: #008000;">=</span> XmlSchema<span style="color: #008000;">.</span><span style="color: #0000FF;">Read</span><span style="color: #008000;">&#40;</span>stream, <span style="color: #0600FF; font-weight: bold;">null</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  xmlSchemaSet<span style="color: #008000;">.</span><span style="color: #0600FF; font-weight: bold;">Add</span><span style="color: #008000;">&#40;</span>xmlSchema<span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">var xmlSchemaSet = new XmlSchemaSet();
  xmlSchemaSet.XmlResolver = new ManifestResourceResolver(Assembly.GetExecutingAssembly(), &quot;MyApp.Schemas&quot;);
  using (var stream = Assembly.GetCallingAssembly().GetManifestResourceStream(&quot;MyApp.Schemas.MainSchema.xsd&quot;))
  {
  var xmlSchema = XmlSchema.Read(stream, null);
  xmlSchemaSet.Add(xmlSchema);
  }</p></div>
  ;i:3;s:2276:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="csharp" style="font-family:monospace;">XmlReaderSettings settings <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> XmlReaderSettings<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  settings<span style="color: #008000;">.</span><span style="color: #0000FF;">ValidationType</span> <span style="color: #008000;">=</span> ValidationType<span style="color: #008000;">.</span><span style="color: #0000FF;">Schema</span><span style="color: #008000;">;</span>
  settings<span style="color: #008000;">.</span><span style="color: #0000FF;">Schemas</span> <span style="color: #008000;">=</span> xmlSchemaSet<span style="color: #008000;">;</span>
  settings<span style="color: #008000;">.</span><span style="color: #0000FF;">ValidationEventHandler</span> <span style="color: #008000;">+=</span> <span style="color: #008000;">new</span> ValidationEventHandler <span style="color: #008000;">&#40;</span>ValidationCallBack<span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  &nbsp;
  XmlReader reader <span style="color: #008000;">=</span> XmlReader<span style="color: #008000;">.</span><span style="color: #0000FF;">Create</span><span style="color: #008000;">&#40;</span><span style="color: #666666;">&quot;SomeXml.xml&quot;</span>, settings<span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #0600FF; font-weight: bold;">while</span> <span style="color: #008000;">&#40;</span>reader<span style="color: #008000;">.</span><span style="color: #0000FF;">Read</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span></pre></td></tr></table><p class="theCode" style="display:none;">XmlReaderSettings settings = new XmlReaderSettings();
  settings.ValidationType = ValidationType.Schema;
  settings.Schemas = xmlSchemaSet;
  settings.ValidationEventHandler += new ValidationEventHandler (ValidationCallBack);

  XmlReader reader = XmlReader.Create(&quot;SomeXml.xml&quot;, settings);
  while (reader.Read());</p></div>
  ;i:4;s:1201:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">private</span> <span style="color: #0600FF; font-weight: bold;">static</span> <span style="color: #6666cc; font-weight: bold;">void</span> ValidationCallBack<span style="color: #008000;">&#40;</span><span style="color: #6666cc; font-weight: bold;">object</span> sender, ValidationEventArgs e<span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  Console<span style="color: #008000;">.</span><span style="color: #0000FF;">WriteLine</span><span style="color: #008000;">&#40;</span><span style="color: #666666;">&quot;Validation Error: {0}&quot;</span>, e<span style="color: #008000;">.</span><span style="color: #0000FF;">Message</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">private static void ValidationCallBack(object sender, ValidationEventArgs e)
  {
  Console.WriteLine(&quot;Validation Error: {0}&quot;, e.Message);
  }</p></div>
  ";}
---

I was recently tasked with validating XML files against a very complex set of XSD schema files.  This is easily accomplished if your XSD files live on the filesystem, because the .Net xml resolver can find referenced schemas via a relative or absolute Uri.  In my case, the schema files were compiled as embedded resources in my project.  As expected, the XML schema loader didn&#8217;t know how to find referenced schemas &#8211; it was likely searching for them  in the path of the running application.  To solve this, I had to help out the schema loader by implementing a custom resolver that knew how to pull schema from the embedded resources.  The code&#8230;

Implement a custom XmlUrlResolver to find schema references as embedded resources.

<pre lang="csharp">public class ManifestResourceResolver : XmlUrlResolver
{
    private readonly Assembly _resourceAssembly;
    private readonly string _baseNamespaceForReferences;

    public ManifestResourceResolver(Assembly resourceAssembly, string baseNamespaceForReferences)
    {
        _resourceAssembly = resourceAssembly;
        _baseNamespaceForReferences = baseNamespaceForReferences.EndsWith(".") ? baseNamespaceForReferences : baseNamespaceForReferences + ".";
    }

    override public object GetEntity(Uri absoluteUri, string role, Type ofObjectToReturn)
    {
        if (absoluteUri.IsFile)
        {
            var file = Path.GetFileName(absoluteUri.AbsolutePath);
            var stream = _resourceAssembly.GetManifestResourceStream(_baseNamespaceForReferences + file);
            return stream;
        }
        return null;
    }
}
</pre>

Create a XmlSchemaSet, and set the XmlResolver property to the custom resolver implemented above. Then, load up the main XSD schema.

<pre lang="csharp">var xmlSchemaSet = new XmlSchemaSet();
xmlSchemaSet.XmlResolver = new ManifestResourceResolver(Assembly.GetExecutingAssembly(), "MyApp.Schemas");
using (var stream = Assembly.GetCallingAssembly().GetManifestResourceStream("MyApp.Schemas.MainSchema.xsd"))
{
    var xmlSchema = XmlSchema.Read(stream, null);
    xmlSchemaSet.Add(xmlSchema);
}
</pre>

Create a XmlReaderSettings object and set the Schemas property to the schema set loaded above. Lastly, load up an XML file with an XmlReader and read it.

<pre lang="csharp">XmlReaderSettings settings = new XmlReaderSettings();
settings.ValidationType = ValidationType.Schema;
settings.Schemas = xmlSchemaSet;
settings.ValidationEventHandler += new ValidationEventHandler (ValidationCallBack);

XmlReader reader = XmlReader.Create("SomeXml.xml", settings);
while (reader.Read());
</pre>

The validation callback function will be invoked on each validation error while reading the file with the XmlReader.

<pre lang="csharp">private static void ValidationCallBack(object sender, ValidationEventArgs e)
{
    Console.WriteLine("Validation Error: {0}", e.Message);
}
</pre>

References:

  * [Validation Using the XmlSchemaSet](http://msdn.microsoft.com/en-us/library/as3tta56%28v=VS.90%29.aspx)
  * [Loading XmlSchema files out of Assembly Resources](http://www.hanselman.com/blog/LoadingXmlSchemaFilesOutOfAssemblyResources.aspx)   
    (contains some obsoleted code, but it was still helpful)
---
author: Kevin
categories:
- Uncategorized
date: 2011-03-08T22:31:08Z
guid: http://blog.cleverswine.net/?p=646
id: 646
tags:
- Hacking
title: MVC3 + MongoDB + Autofac
url: /2011/03/08/mvc3-mongodb-autofac/
wp-syntax-cache-content:
- |
  a:6:{i:1;s:1447:"
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="line_numbers"><pre>1
  2
  3
  4
  </pre></td><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">interface</span> ISession <span style="color: #008000;">:</span> IDisposable
  <span style="color: #008000;">&#123;</span>
  <span style="color: #000000;">System</span><span style="color: #008000;">.</span><span style="color: #0000FF;">Linq</span><span style="color: #008000;">.</span><span style="color: #0000FF;">IQueryable</span><span style="color: #008000;">&lt;</span>T<span style="color: #008000;">&gt;</span> All<span style="color: #008000;">&lt;</span>T<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span> <span style="color: #0600FF; font-weight: bold;">where</span> T <span style="color: #008000;">:</span> <span style="color: #6666cc; font-weight: bold;">class</span>, <span style="color: #008000;">new</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">public interface ISession : IDisposable
  {
  System.Linq.IQueryable&lt;T&gt; All&lt;T&gt;() where T : class, new();
  }</p></div>
  ;i:2;s:4300:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="line_numbers"><pre>1
  2
  3
  4
  5
  6
  7
  8
  9
  10
  11
  12
  13
  14
  15
  16
  17
  18
  19
  20
  21
  22
  23
  </pre></td><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">class</span> MongoSession <span style="color: #008000;">:</span> ISession
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">private</span> <span style="color: #0600FF; font-weight: bold;">readonly</span> IMongo _provider<span style="color: #008000;">;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> MongoSession<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #008080; font-style: italic;">//this looks for a connection string in your Web.config</span>
  <span style="color: #008080; font-style: italic;">//&lt;connectionStrings&gt;</span>
  <span style="color: #008080; font-style: italic;">//  &lt;add name=&quot;db&quot; connectionString=&quot;mongodb://localhost/testdb?strict=true&quot;/&gt;</span>
  <span style="color: #008080; font-style: italic;">//&lt;/connectionStrings&gt;</span>
  _provider <span style="color: #008000;">=</span> Mongo<span style="color: #008000;">.</span><span style="color: #0000FF;">Create</span><span style="color: #008000;">&#40;</span><span style="color: #666666;">&quot;db&quot;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> IQueryable<span style="color: #008000;">&lt;</span>T<span style="color: #008000;">&gt;</span> All<span style="color: #008000;">&lt;</span>T<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span> <span style="color: #0600FF; font-weight: bold;">where</span> T <span style="color: #008000;">:</span> <span style="color: #6666cc; font-weight: bold;">class</span>, <span style="color: #008000;">new</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">return</span> _provider<span style="color: #008000;">.</span><span style="color: #0000FF;">GetCollection</span><span style="color: #008000;">&lt;</span>T<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">.</span><span style="color: #0000FF;">AsQueryable</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">void</span> Dispose<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  _provider<span style="color: #008000;">.</span><span style="color: #0000FF;">Dispose</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">public class MongoSession : ISession
  {
  private readonly IMongo _provider;

  public MongoSession()
  {
  //this looks for a connection string in your Web.config
  //&lt;connectionStrings&gt;
  //  &lt;add name=&quot;db&quot; connectionString=&quot;mongodb://localhost/testdb?strict=true&quot;/&gt;
  //&lt;/connectionStrings&gt;
  _provider = Mongo.Create(&quot;db&quot;);
  }

  public IQueryable&lt;T&gt; All&lt;T&gt;() where T : class, new()
  {
  return _provider.GetCollection&lt;T&gt;().AsQueryable();
  }

  public void Dispose()
  {
  _provider.Dispose();
  }
  }</p></div>
  ;i:3;s:1133:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="line_numbers"><pre>1
  2
  3
  4
  </pre></td><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">interface</span> IRecipeRepository
  <span style="color: #008000;">&#123;</span>
  IEnumerable<span style="color: #008000;">&lt;</span>RecipeCard<span style="color: #008000;">&gt;</span> GetAll<span style="color: #008000;">&#40;</span><span style="color: #6666cc; font-weight: bold;">int</span> page <span style="color: #008000;">=</span> <span style="color: #FF0000;">1</span>, <span style="color: #6666cc; font-weight: bold;">int</span> pageSize <span style="color: #008000;">=</span> <span style="color: #FF0000;">10</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">public interface IRecipeRepository
  {
  IEnumerable&lt;RecipeCard&gt; GetAll(int page = 1, int pageSize = 10);
  }</p></div>
  ;i:4;s:3847:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="line_numbers"><pre>1
  2
  3
  4
  5
  6
  7
  8
  9
  10
  </pre></td><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">public</span> <span style="color: #6666cc; font-weight: bold;">class</span> RecipeRepository<span style="color: #008000;">&lt;</span>SessionT<span style="color: #008000;">&gt;</span> <span style="color: #008000;">:</span> IRecipeRepository <span style="color: #0600FF; font-weight: bold;">where</span> SessionT <span style="color: #008000;">:</span> ISession, <span style="color: #008000;">new</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">public</span> IEnumerable<span style="color: #008000;">&lt;</span>RecipeCard<span style="color: #008000;">&gt;</span> GetAll<span style="color: #008000;">&#40;</span><span style="color: #6666cc; font-weight: bold;">int</span> page <span style="color: #008000;">=</span> <span style="color: #FF0000;">1</span>, <span style="color: #6666cc; font-weight: bold;">int</span> pageSize <span style="color: #008000;">=</span> <span style="color: #FF0000;">10</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">using</span> <span style="color: #008000;">&#40;</span><span style="color: #0600FF; font-weight: bold;">var</span> session <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> SessionT<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">return</span> session<span style="color: #008000;">.</span><span style="color: #0000FF;">All</span><span style="color: #008000;">&lt;</span>RecipeCard<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">.</span><span style="color: #0000FF;">OrderByDescending</span><span style="color: #008000;">&#40;</span>r <span style="color: #008000;">=&gt;</span> r<span style="color: #008000;">.</span><span style="color: #0000FF;">UpdatedDate</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">.</span><span style="color: #0000FF;">Skip</span><span style="color: #008000;">&#40;</span>pageSize <span style="color: #008000;">*</span> <span style="color: #008000;">&#40;</span>page <span style="color: #008000;">-</span> <span style="color: #FF0000;">1</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">.</span><span style="color: #0000FF;">Take</span><span style="color: #008000;">&#40;</span>pageSize<span style="color: #008000;">&#41;</span><span style="color: #008000;">.</span><span style="color: #0000FF;">ToList</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">public class RecipeRepository&lt;SessionT&gt; : IRecipeRepository where SessionT : ISession, new()
  {
  public IEnumerable&lt;RecipeCard&gt; GetAll(int page = 1, int pageSize = 10)
  {
  using (var session = new SessionT())
  {
  return session.All&lt;RecipeCard&gt;().OrderByDescending(r =&gt; r.UpdatedDate).Skip(pageSize * (page - 1)).Take(pageSize).ToList();
  }
  }
  }</p></div>
  ;i:5;s:4242:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="line_numbers"><pre>1
  2
  3
  4
  5
  6
  7
  8
  9
  10
  11
  12
  13
  14
  </pre></td><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">protected</span> <span style="color: #6666cc; font-weight: bold;">void</span> Application_Start<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">var</span> builder <span style="color: #008000;">=</span> <span style="color: #008000;">new</span> ContainerBuilder<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  builder<span style="color: #008000;">.</span><span style="color: #0000FF;">Register</span><span style="color: #008000;">&#40;</span>r <span style="color: #008000;">=&gt;</span> <span style="color: #008000;">new</span> RecipeRepository<span style="color: #008000;">&lt;</span>MongoSession<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">.</span><span style="color: #0600FF; font-weight: bold;">As</span><span style="color: #008000;">&lt;</span>IRecipeRepository<span style="color: #008000;">&gt;</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">.</span><span style="color: #0000FF;">InstancePerLifetimeScope</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  builder<span style="color: #008000;">.</span><span style="color: #0000FF;">RegisterControllers</span><span style="color: #008000;">&#40;</span>Assembly<span style="color: #008000;">.</span><span style="color: #0000FF;">GetExecutingAssembly</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">var</span> container <span style="color: #008000;">=</span> builder<span style="color: #008000;">.</span><span style="color: #0000FF;">Build</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  DependencyResolver<span style="color: #008000;">.</span><span style="color: #0000FF;">SetResolver</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">new</span> AutofacDependencyResolver<span style="color: #008000;">&#40;</span>container<span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  &nbsp;
  AreaRegistration<span style="color: #008000;">.</span><span style="color: #0000FF;">RegisterAllAreas</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  &nbsp;
  RegisterGlobalFilters<span style="color: #008000;">&#40;</span>GlobalFilters<span style="color: #008000;">.</span><span style="color: #0000FF;">Filters</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  RegisterRoutes<span style="color: #008000;">&#40;</span>RouteTable<span style="color: #008000;">.</span><span style="color: #0000FF;">Routes</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">protected void Application_Start()
  {
  var builder = new ContainerBuilder();
  builder.Register(r =&gt; new RecipeRepository&lt;MongoSession&gt;()).As&lt;IRecipeRepository&gt;().InstancePerLifetimeScope();
  builder.RegisterControllers(Assembly.GetExecutingAssembly());

  var container = builder.Build();
  DependencyResolver.SetResolver(new AutofacDependencyResolver(container));

  AreaRegistration.RegisterAllAreas();

  RegisterGlobalFilters(GlobalFilters.Filters);
  RegisterRoutes(RouteTable.Routes);
  }</p></div>
  ;i:6;s:1899:
  <div class="wp_syntax" style="position:relative;"><table><tr><td class="line_numbers"><pre>1
  2
  3
  4
  5
  6
  7
  8
  9
  10
  11
  </pre></td><td class="code"><pre class="csharp" style="font-family:monospace;"><span style="color: #0600FF; font-weight: bold;">private</span> <span style="color: #0600FF; font-weight: bold;">readonly</span> IRecipeRepository recipeRepository<span style="color: #008000;">;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> RecipesController<span style="color: #008000;">&#40;</span>IRecipeRepository recipeRepository<span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">this</span><span style="color: #008000;">.</span><span style="color: #0000FF;">recipeRepository</span> <span style="color: #008000;">=</span> recipeRepository<span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span>
  &nbsp;
  <span style="color: #0600FF; font-weight: bold;">public</span> ViewResult Index<span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span>
  <span style="color: #008000;">&#123;</span>
  <span style="color: #0600FF; font-weight: bold;">return</span> View<span style="color: #008000;">&#40;</span>recipeRepository<span style="color: #008000;">.</span><span style="color: #0000FF;">GetAll</span><span style="color: #008000;">&#40;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">&#41;</span><span style="color: #008000;">;</span>
  <span style="color: #008000;">&#125;</span></pre></td></tr></table><p class="theCode" style="display:none;">private readonly IRecipeRepository recipeRepository;

  public RecipesController(IRecipeRepository recipeRepository)
  {
  this.recipeRepository = recipeRepository;
  }

  public ViewResult Index()
  {
  return View(recipeRepository.GetAll());
  }</p></div>
  ";}
---

I recently posted a [brief summary](http://blog.cleverswine.net/2011/03/08/a-diet-and-an-application/) of creating a recipe database.  I decided to expand on that and go into the implementation details of using ASP.NET MVC3 with MongoDB and Autofac.  This code depends on the [NoRM](https://github.com/atheken/NoRM) driver for [MongoDB](http://www.mongodb.org/), [Autofac](http://code.google.com/p/autofac/), and [MVC3](http://www.asp.net/mvc) (and it&#8217;s dependencies). This is by no means the only (or best) way to do things, but this is how I chose to do it.  I&#8217;ve simplified the repository to one method for brevity, but you&#8217;d obviously want to add all of the required CRUD operations.

First, we&#8217;ll want to create an interface for our database session.  The implementation of this interface can be swapped out depending on the backing store.

<pre lang="csharp" line="1">public interface ISession : IDisposable
{
    System.Linq.IQueryable&lt;T> All&lt;T>() where T : class, new();
}
</pre>

Add a MongoDB implementation of ISession.  This implementation will eventually be used in our repository.

<pre lang="csharp" line="1">public class MongoSession : ISession
{
    private readonly IMongo _provider;

    public MongoSession()
    {
        //this looks for a connection string in your Web.config
        //&lt;connectionStrings>
        //  &lt;add name="db" connectionString="mongodb://localhost/testdb?strict=true"/>
        //&lt;/connectionStrings>
        _provider = Mongo.Create("db");
    }

    public IQueryable&lt;T> All&lt;T>() where T : class, new()
    {
        return _provider.GetCollection&lt;T>().AsQueryable();
    }

    public void Dispose()
    {
        _provider.Dispose();
    }
}
</pre>

Create an interface for the repository.  This is what the controller will expect to be injected via it&#8217;s constructor.

<pre lang="csharp" line="1">public interface IRecipeRepository
{
    IEnumerable&lt;RecipeCard> GetAll(int page = 1, int pageSize = 10);
}
</pre>

Add the IRecipeRepository implementation, which is created with a new()-able type of ISession.  Why didn&#8217;t I just inject an ISession into the repository?  It&#8217;s because Mongo sessions are expected to be disposed of after use, as connections are pooled and reused.  Therefore we have to create and dispose of our sessions when using them.  This might require some workarounds if you really did switch repositories (do nothing on Dispose(), etc).

<pre lang="csharp" line="1">public class RecipeRepository&lt;SessionT> : IRecipeRepository where SessionT : ISession, new()
{
    public IEnumerable&lt;RecipeCard> GetAll(int page = 1, int pageSize = 10)
    {
        using (var session = new SessionT())
        {
            return session.All&lt;RecipeCard>().OrderByDescending(r => r.UpdatedDate).Skip(pageSize * (page - 1)).Take(pageSize).ToList();
        }
    }
}
</pre>

Now it&#8217;s time set up our IoC container.  We&#8217;ll do this in the Global.asax.cs.  Here we are registering a recipe repository of type MongoSession such that any controller that expects an IRecipeRepository in the constructor will get this.  We&#8217;re letting Autofac handle controller creation and dependency injection.  See the [Autofac MVC3 integration page](http://code.google.com/p/autofac/wiki/Mvc3Integration) for more details.

<pre lang="csharp" line="1">protected void Application_Start()
{
    var builder = new ContainerBuilder();
    builder.Register(r => new RecipeRepository&lt;MongoSession>()).As&lt;IRecipeRepository>().InstancePerLifetimeScope();
    builder.RegisterControllers(Assembly.GetExecutingAssembly());

    var container = builder.Build();
    DependencyResolver.SetResolver(new AutofacDependencyResolver(container));

    AreaRegistration.RegisterAllAreas();

    RegisterGlobalFilters(GlobalFilters.Filters);
    RegisterRoutes(RouteTable.Routes);
}
</pre>

Finally, here is our controller.  Notice we never have to create a repository object &#8211; one was injected for us.

<pre lang="csharp" line="1">private readonly IRecipeRepository recipeRepository;

public RecipesController(IRecipeRepository recipeRepository)
{
    this.recipeRepository = recipeRepository;
}

public ViewResult Index()
{
    return View(recipeRepository.GetAll());
}
</pre>

That&#8217;s it.
---
title: "Spockify - Part 2 - dotnet core serving vue.js"
date: 2021-05-26T17:17:33-07:00
draft: true
---

In this post, I will get started writing code for a hobby project called **Spockify**, which I describe in [Spockify - Part 1]({{< ref "/posts/spockify-part-1" >}} "Spockify - Part 1")

## Goal

Create the simplest skeleton project that has a server and a UI.

## Technology

I have chosen to use [dotnet core](https://dotnet.microsoft.com/learn/aspnet/what-is-aspnet-core) as the backend because that's what I'm comfortable with. This application is simple enough that almost any technology could have been used (nodejs, python, go, etc.).

For the front end, I have chosen [vue.js](https://vuejs.org/) because I find it quite a bit more intuitive than some of the other front end frameworks. I'm not a front end developer by trade, so what do I know?

## Bootstrapping the project

Prerequisites: [dotnet core](https://dotnet.microsoft.com/download), [vue.js](https://v3.vuejs.org/guide/installation.html)

These commands will generate a skeleton dotnet server and a UI for the project.

```bash
mkdir spockify
cd spockify
dotnet new web -o server -n Spockify
vue create ui
```

## Configure the vue.js ui

In the vue.js project, we want to change the output of the build to go to a location in the server directory. By default, builds will go to `./ui/dist`.

In the `./ui` directory, add a file called `vue.config.js` with the following content

```javascript
// ./ui/vue.config.js
module.exports = {
    outputDir: '../server/wwwroot'
}
```

## Update the dotnet server

The `dotnet new` command created a simple web project. We're going to strip it down even further so that it's single purpose is to serve our vue.js application.

Microsoft provides a nice middleware component called `UseProxyToSpaDevelopmentServer` that will allow us to proxy requests to the vue.js server (runningon port 8080) during development. To use it, we need to install the `Microsoft.AspNetCore.SpaServices.Extensions` package. From the `./server` directory, run:

```bash
dotnet add package Microsoft.AspNetCore.SpaServices.Extensions
```

Modify `./server/Startup.cs` to serve static files and use the SPA proxy in development mode.

```csharp
// ./server/Startup.cs
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

namespace Spockify
{
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            // for production builds, the optimized vue.js application will be served from wwwroot
            services.AddSpaStaticFiles(configuration: options => { options.RootPath = "wwwroot"; });
        }

        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            app.UseSpaStaticFiles();
            app.UseSpa(configuration: builder =>
            {
                if (env.IsDevelopment())
                {
                    // here we are proxying requests to the vue.js development server so we get hot reloading
                    builder.UseProxyToSpaDevelopmentServer("http://localhost:8080");
                }
            });
        }
    }
}
```

## Try it out

For this simplest version, We'll need to start the vue.js development server and then start the dotnet server. There are ways to combine this into a single step, but I'd rather run 2 things and keep everything else simple.

From `./ui`, run the vue.js server. It will start on localhost:8080.

```bash
yarn serve
```

From `./server`, run the dotnet application

```bash
dotnet run
```

Navigate to http://localhost:5000 and you should see a basic vue.js application.

*If you are using an IDE, you can use it's debugging features to run the server code*

## Extra

I'll want a `.gitignore` file. There's a [cool service](https://www.toptal.com/developers/gitignore) that will build a file for you. From the project root, run

```bash
curl -o .gitignore https://www.toptal.com/developers/gitignore/api/vuejs,dotnetcore,visualstudiocode
```


## Summary

I can serve up a vue.js SPA from a dotnet core server!

## Next

Add Spotify authentication.

Also, I'd like to get this code into my github repo and tag it for each blog post.


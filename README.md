## Introduction
This is a demo project that adds Blazor Webassembly components to a non-SPA Razor pages application

## Steps
* Create a Blazor Webassembly application with seperate project for webassembly components
* Delete the Blazor server project
* In Blazor webassembly client project, comment out the line `builder.RootComponents.Add<App>("#app");` of Program.cs file. This avoids the blazor WASM to search for a div named `app` while loading.
* Delete favicon.ico file from the blazor WASM client project's wwwroot folder since it will create conflicting asset error while running server
* Create a Razor pages / ASP.NET Core Web App project in the solution
* Add a Nuget package `Microsoft.AspNetCore.Components.WebAssembly.Server` to the razor pages project
* Add reference to the Blazor client project in the razor pages project
* In the Razor pages project, add `app.UseWebAssemblyDebugging();` , `app.UseBlazorFrameworkFiles();` in the Program.cs request pipeline as follows
```cs
if (!app.Environment.IsDevelopment())
{
    // ...
}
else
{
    app.UseWebAssemblyDebugging();
    // ...
}
app.UseBlazorFrameworkFiles();
```
* In the Index.cshtml razor page
  * add reference to the blazor components of the blazor webassembly client project with @using statement
  * add a script tag to include blazor webassembly js file
  * use a component from the blazor webassembly project
```cs
@page

@using BlazorInRazor.Client.Pages

@model IndexModel

@{
    ViewData["Title"] = "Home page";
}

<div class="text-center">
    <h1 class="display-4">Welcome</h1>
    <p>Learn about <a href="https://docs.microsoft.com/aspnet/core">building Web apps with ASP.NET Core</a>.</p>
</div>

<component type="typeof(Counter)" render-mode="WebAssemblyPrerendered" />

<script src="_framework/blazor.webassembly.js"></script>
```


## References
* https://www.telerik.com/blogs/integrate-blazor-webassembly-existing-aspnet-core-web-application , https://jonhilton.net/blazor-wasm-in-razor-pages/ , https://stackoverflow.com/a/75682925/2746323 - integrate blazor assembly to existing razor pages application
* Render mode for blazor components - https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.rendering.rendermode?view=aspnetcore-6.0
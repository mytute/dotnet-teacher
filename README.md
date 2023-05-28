# dotnet-teacher

### Whhat is .NET Core ?
.NET Core is a new version of .NET Framework, which  is free, open-source, general-purpose develepment platform maintained by Microsoft.

.NET Framework     
1. Allication created using .NET Framework can only run on Windows operating system.    
2. With .NET core, you can install only those packages from NuGet package which you actually for your application development.    

.NET Core    
1. .NET core is a cross-platform Framework that can run Windows, macOS, and Linux operation systems.   
2. When you install .NET framework in your machine, it installs all the packages which .NET framework provides. Even thought you are not going to use all of them.   

### Features of .NET core     

* Open-source framework: .NET Core is an open-source framework which is maintained by Microsoft and is available on GitHub.   
* Cross-platform: .NET Core runs on Windows, macOS and Linux operation systems.   
* Wide range of application: Various types of application can be developed and run-on .NET core platform such as mobile, desktop, web, cloud, IoT, machine learning, microservices, game, etc.    
* Modular Architecture: .NET core supports modular Architecture approach using NuGet packages.   
* CLI Tools: .NET core includes command-line interface(CLI tools) for development and continuous- integration.  

### Composition of .NET Core    
1. CLI Tools : A set of tooling for development & deployment.    
2. Roslyn : Language compiler for C# & Visual basic.    
3. CoreFx : A set of framework libraries.    
4. CoreCLR : A JIT based CLR(Common language runtime).    

### what is ASP.NET Core    
 ASP.NET Core is a free, open-source, and cross-platform framework for building cloud-based application, such  as web apps, IoT apps, and mobile backends. It is designed to run on cloud as well as on-premises.    

 Features of ASP.NET core    
 * ASP.NET Core application are cross-platform : can host on any operation system like Windows, MacOS or Linux.    
 * ASP.NET Core is open source : available on GitHub for free > github.com/dotnet/aspnetcore     
 * ASP.NET Core is cloud-enabled : has cloud supports.    

### WebForms vs MVC vs ASP.NET Core    

* WebForms (2002)   
  performances issues store state of each page in "ViewState" transfer to client to server and server to client in every request.
  unit test is difficult.
  not cloud friendly.
  use event-driven approach.
  not dependency injection supports.   
* ASP.NET MVC (2009)  
   built in top of WebForms
  unit test is not difficult.
  only can host on windows.
  semi cloud friendly.  
  use MVC Architecture.   
  can use dependency injection but not inbuilt    
* ASP.NET Core (2016)   
  hight performances,
  cross platform.
  cloud enable.
  completely cloud friendly.  
  use MVC Architecture.   
  inbuilt dependency injection support.   

### Frist Application

to crate a web project
``bash
$ dotnet new -l # to view all the templates
$ dotnet new web # create template with short name   
$ dotnet new web --name test002 # with folder inside
```

to add hot reload go to 'launchSettings.json' file and add "hotReloadProfile"
```json
"profiles": {
    "hotreloadprofile": {
        "commandName": "Project",
        "launchBrowser": true,
        "environmentVariables": {
            "ASPNETCORE_ENVIRONMENT": "Development",
            "Key": "Value"
        },
        "applicationUrl": "https://localhost:5001;http://localhost:5000",
        "hotReloadProfile": "aspnetcore"  // add here...
    }
}
```
and to run

```bash
$ dotnet watch run --launch-profile hotreloadprofile
```

> Program.cs      

```cs
// "Program.cs" file first file going to execute first and think as "static void main method"

// return instance of WebApplicationBuilder class
var builder = WebApplication.CreateBuilder(args);

// retirn instance of WebApplication
var app = builder.Build();

// creating a route for http get method
app.MapGet("/", () => "this is my first dot net core app.... "); // '/' root url

// starting the server
app.Run();
```

### Kestrel web server

IIS was the default server for ASP.NET WebForms & ASP.NET MVC(only windows)   
Kestrel web server for ASP.NET Core and it's cross platform.

Kestrel is a light-weight web server & can handle limited responsibilities.for that
in production we use Kestrel with a reserse proxy server like IIS, NGINX, APACHE etc.  

client HTTP Request get by "Kestrel" server and create "httpContextObject" to "ASP.NET core Application"
and it return results to "Kestrel" server and it return to client as HTTP Response.    

### Overview of launchSettings.json   

by default select "http" as profile and run with "Kestrel" server.

* "http": > profile name can be change to any name.
* "commandName": "Project" >  "Project" mean it's a "Kestrel" server. cannot change.
* "applicationUrl": "http://localhost:5289" "Kestrel" server running url and can change port     
  (1024 <Port number < 65535)    
* "dotnetRunMessages": true > for command line interface show state message   
* "launchBrowser": true, > automatically open the URL on browser

```json
"profiles": {
  "http": {
    "commandName": "Project",
    "dotnetRunMessages": true,
    "launchBrowser": true,
    "applicationUrl": "http://localhost:5289",
    "environmentVariables": {
      "ASPNETCORE_ENVIRONMENT": "Development"
    },
    "hotReloadProfile": "aspnetcore"
  },

}
```

### 07 Understanding client server architecture


TCP/IP Protocal are the communication protocols that define how the data travels across the web.    
TCP is breakup the request & response into thousands of small chunks called packets, before they are sent.   
Ones packets reach destination TCP will re-assemble all the packets in to original request or response.    
job is IP protocal send and route small packets through internet by using IP address.   

### 08 HTTP Request & Response


SSL - SECURED SOCKET LAYER.

### 09 Request & Response Headers

Request headers are the key value pairs, that are sent by the browser to the server. It contains information
about the request which can be used by the server for processing the request.

Headers are prepared by browser   
how to access headers in dotnet application.    
```cs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", (HttpContext context) =>
{
    var UserAgent = "";
    if(context.Request.Headers.ContainsKey("User-Agent")){
        UserAgent = context.Request.Headers["User-Agent"];
    }
    context.Response.StatusCode = 200;
    return "User Agent"+UserAgent;
});

app.Run();
```

how to set headers in dotnet application.(for the browser)
```cs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", (HttpContext context) =>
{
    context.Response.Headers["Content-type"] = "text/html";
    // if above header not set as html it will show as string only
    return "<h2> This is a Text response </h2>";

    /*  we can use custom header
    context.Response.Headers["my-header"] = "Hello world";
    */
});

app.Run();
```

### 10 HTTP status codes    

All the status codes starting from 100 to 199 are informational in nature.   
ex 101 - switching protocal - when the request switches from http to https   

All the status codes between 200-299 indicates successful responses. These status
code means that the request was successfully received and processed.   
ex 200 - OK
   201 - Created  

All the status codes between 300-399, they are meant for redirection messages.    
ex 302 - Found - it indecates the redirection from on URL to another URL
   304 - not modifued - means the file is already available in the browser cache(image, css, html)

All the status code between 400- 499 stands for client errors.
ex 400 bad request

All the status code between 500- 599 stands for server errors  
ex - 500 - internal server error   

If we didn't set status code by default it will set 200 code.   

let's see how to set status code in dotnet application to the browser.

```cs
app.MapGet("/", (HttpContext context) =>
{

});

/* use middleware
for handdle responses status code, we can use middleware.    
in middleware it will call end of any api call

*/
app.Run(async (HttpContext context) =>
{
    string path = context.Request.Path;
    if(path == "/" || path == "/Home")
    {
      context.Response.StatusCode = 200;
      await context.Response.WriteAsync("You are in Home page");
    }else if(path == "/Contact")
    {
      context.Response.StatusCode = 200;
      await context.Response.WriteAsync("You are in Contact page");
    }else
    {
      context.Response.StatusCode = 404;
      await context.Response.WriteAsync("The page you are looking for is not found!");
    }

});

app.Run();

```

### 11 Query Strings | HTTP Request & Response    

Query string is a way of passing some extra data to the server with the request, using the URL.   
Query string only for get request.   


example for Quary string for product route        
http://localhost:5289/Product?id=101&name=IPhone    

in `context.Request.Query` there is a dictionary of Query values  

```cs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", (HttpContext context) =>
{
});

// middleware
app.Run(async (HttpContext context) =>
{
    string path = context.Request.Path;
    if(path == "/Product" )
    {
      // check is both query values are existed or not
      if(context.Request.Query.ContainsKey("id") && context.Request.Query.ContainsKey("name") )
      {
        string id = context.Request.Query["id"];
        string name = context.Request.Query["name"];
        await context.Response.WriteAsync("You select the product with ID"+id+" and Name" + name);
        return;
      }
      context.Response.StatusCode = 200;
      await context.Response.WriteAsync("You are in Home page");
    }
    else
    {
      context.Response.StatusCode = 404;
      await context.Response.WriteAsync("The page you are looking for is not found!");
    }

});

app.Run();
```

### 12 HTTP Request Methods | HTTP Request & Response     

GET : used ofr fetch data(html page, static files, JSON data) from the server.(without body)    
http://localhost:5289/Customers    
http://localhost:5289/Customers/101  (using route parameters)     
http://localhost:5289/Customers?id=101  (using query parameters)    

POST : send data to the server to create a new entity. use to insert data into database.(with body)     
http://localhost:5289/Customers     

PUT : used to update an entity on the server. It does full update.(with body)      
http://localhost:5289/Customers/101  (101 is unique identifier that should provide)

PATCH :  used to update an entity on the server. It update only required properties in db.(with body)     
http://localhost:5289/Customers/101  (101 is unique identifier that should provide)

DELETE : send a request to delete an existing record on the server. (without body)      
http://localhost:5289/Customers/101  (101 is unique identifier that should provide)

### 13 Reading Request Body | HTTP Request & Response    

Here post method use row-XML format and it should convert to dictionary using `QueryHelpers.ParseQuery(data)`   
example : d=101&name=IPhone&name=Samsung (here tow 'name' keys included)

To the get reqest     
http://localhost:5289/Product?id=101&name=IPhone

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.WebUtilities;
using Microsoft.Extensions.Primitives;

var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Run(async (HttpContext context) =>
{
    string path = context.Request.Path;
    string method = context.Request.Method;
    Console.WriteLine("method ::"+method);
    if( method == "GET" && path == "/Product" )
    {
      if(context.Request.Query.ContainsKey("id") && context.Request.Query.ContainsKey("name") )
      {
        string id = context.Request.Query["id"];
        string name = context.Request.Query["name"];
        await context.Response.WriteAsync("You select the product with ID "+id+" and Name " + name);
        return;
      }
      await context.Response.WriteAsync("You are in Product page");
    }
    else if(method == "POST" && path == "/Product")
    {
      string id = "";
      string name = "";

      // reuqest body read as stream   
      StreamReader reader = new StreamReader(context.Request.Body); // create instance
      string data = await reader.ReadToEndAsync();

      // body query-string have to conver to dictionary(key-value paire)
      // body > id=101&name=IPhone&name=Samsung
      Dictionary<string, StringValues> dict = QueryHelpers.ParseQuery(data);

      if(dict.ContainsKey("id"))
      {
        id = dict["id"];
      }
      if(dict.ContainsKey("name"))
      {
        // because of tow 'name' keys in body it's values load to array
        name = dict["name"][1]; // dict["name"] =['IPhone', 'Samsung']
      }
      await context.Response.WriteAsync(" id :"+id+ "\n name :"+name);
    }

});

app.Run();
```

### 14 Server-side vs client-side rendering  

using dotnet core can create SERVER-SIDE RENDERING and  CLIENT-SIDE RENDERING.    

### 15 Introduction to Middleware    

Middleware is a component that is assembled into the application pipeline to handle request and response.

Middleware are chained one after the other and they are executed in the order in which they are defined in ASP.NET core application.     

In order to call the next middleware in request pipeline, we need to explicitly call it by calling 'Next' method.    

Each middleware should perform a single task.    

Middleware can be created in two ways - using Request delegate( Anonymous function or lambda expression ) or by using class.   

* Request pipeline    

HTTP Request > Kestrel > Middleware 1 > Middleware 2 > Middleware 3(terminate middleware) > Middleware 3 > Middleware 2 > Middleware 1 > Kestrel >  HTTP Response      


### 16 Creating a Custom Middleware    

Here browser show result of First middleware only.     
Since Middleware not calling `next` method pipe not continue to next middleware and it works as terminate middleware. `app.Run` does not have `next` function argument.   
In debug tools also `Second middleware` not hitting.    

```cs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Run(async(HttpContext context) =>
{
  await context.Response.WriteAsync("First middleware works");
});

app.Run(async(HttpContext context) =>
{
  await context.Response.WriteAsync("Second middleware works");
});

app.Run();
```

### 17 Chaining Multiple Middleware      

Middleware works order in the code that define it.   

Inside Middleware can call `next(context)` to move next Middleware and should wait until.    

Order of middleware pip       
Middleware 1 > Middleware 2 > Middleware 3 > Middleware 2 > Middleware 1

```cs  
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

/*
browser response   >
'First middleware works second middleware works Third middleware works'
*/

// Middleware 1
app.Use(async(HttpContext context, RequestDelegate next) =>
{
  await context.Response.WriteAsync("First middleware works");
  await next(context);
  // Can write code after `next` method and execute   
});

// Middleware 2
// here if you  not specify datatype then you should call `next` function
app.Use(async(HttpContext context, RequestDelegate next) =>
{
  await context.Response.WriteAsync("Second middleware works");
  await next(context);
  // Can write code after `next` method and execute  
});

// Middleware 3
app.Run(async(HttpContext context) =>
{
  await context.Response.WriteAsync("Third middleware works");
});

app.Run();
```

### 18 Custom middleware class

If middleware have lot of code is not good.   
Middleware class improve more reusable, maintainable, readable.    

create class with inharite IMiddleware.   
register middleware class before `builder.Build()` method.     

create folder name `Middleware` and inside it create a file name `CustomMiddleware`.     

```cs
using Middleware.CustomMiddleware;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddTransient<MyMiddleware>(); // + add

var app = builder.Build();

/*
browser response   >
  First middleware works    
  Custom Middleware started!     
  Third middleware works     
  Custom Middleware finished!     
*/

// Middleware 1
app.Use(async(HttpContext context, RequestDelegate next) =>
{
  await context.Response.WriteAsync("First middleware works \n");
  await next(context);
  // Can write code after `next` method and execute   
});

// Middleware 2
app.UseMiddleware<MyMiddleware>(); // + add

// Middleware 3
app.Run(async(HttpContext context) =>
{
  await context.Response.WriteAsync("Third middleware works \n");
});

app.Run();
```

> CustomMiddleware.cs middleware class    
```cs
namespace Middleware.CustomMiddleware
{
    public class MyMiddleware : IMiddleware
    {
        public async Task InvokeAsync(HttpContext context, RequestDelegate next)
        {
            await context.Response.WriteAsync("Custom Middleware started! \n");
            await next(context);
            await context.Response.WriteAsync("Custom Middleware finished!  \n");
        }
    }
}
```    

### 19 Custom Middleware Extensions    

In order to access class middleware from outside class we can use `Middleware Extensions`.   



```cs
using Middleware.CustomMiddleware;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddTransient<MyMiddleware>(); // + add register as service
var app = builder.Build();

/*
browser response   >
  First middleware works    
  Custom Middleware started!     
  Third middleware works     
  Custom Middleware finished!     
*/

// Middleware 1
app.Use(async(HttpContext context, RequestDelegate next) =>
{
  await context.Response.WriteAsync("First middleware works \n");
  await next(context);
  // Can write code after `next` method and execute   
});

// Middleware 2
// app.UseMiddleware<MyMiddleware>(); // - remove
app.MyMiddleware();                   // + add

// Middleware 3
app.Run(async(HttpContext context) =>
{
  await context.Response.WriteAsync("Third middleware works \n");
});

app.Run();
```

```cs
namespace Middleware.CustomMiddleware
{
    public class MyMiddleware : IMiddleware
    {
        public async Task InvokeAsync(HttpContext context, RequestDelegate next)
        {
            await context.Response.WriteAsync("Custom Middleware started! \n");
            await next(context);
            await context.Response.WriteAsync("Custom Middleware finished!  \n");
        }
    }

    public static class CustomMiddlewareExtension // static class
    {
        public static IApplicationBuilder MyMiddleware(this IApplicationBuilder app) // static method
        {
           return app.UseMiddleware<MyMiddleware>();
        }
    }
}
```

### 20 Conventional Custom Middleware Class

In this method we don't need to register as service.   

```cs
using Middleware.CustomMiddleware;

var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

/*
browser response   >
  First middleware works
  Custom Middleware started!
  Third middleware works
  Custom Middleware finished!     
*/

// Middleware 1
app.Use(async(HttpContext context, RequestDelegate next) =>
{
  await context.Response.WriteAsync("First middleware works \n");
  await next(context);
  // Can write code after `next` method and execute   
});

// Middleware 2
// app.UseMiddleware<MyMiddleware>();
// app.MyMiddleware();
app.AnotherCustomMiddleware(); // + add

// Middleware 3
app.Run(async(HttpContext context) =>
{
  await context.Response.WriteAsync("Third middleware works \n");
});

app.Run();
```

> AnotherCustomMiddleware.cs
```cs
using Microsoft.AspNetCore.Http;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Builder;

namespace Middleware.CustomMiddleware
{
    public class AnotherCustomMiddleware
    {
        private readonly RequestDelegate _next;
        public AnotherCustomMiddleware(RequestDelegate next)
        {
            _next = next;
        }
        public async Task InvokeAsync(HttpContext context)
        {
            await context.Response.WriteAsync("Custom Middleware started! \n");
            await _next(context);
            await context.Response.WriteAsync("Custom Middleware finished!  \n");
        }
    }

    public static class AnotherMiddlewareExtension // static class
    {
        public static IApplicationBuilder AnotherCustomMiddleware(this IApplicationBuilder app) // static method
        {
           return app.UseMiddleware<AnotherCustomMiddleware>();
        }
    }
}
```   

### 21 Middleware execution order

builtin middleware also can re order but not recommoned .

Request/Response  > ExeptionHandler > HSTS > HttpsRedirection > Static Files > Routing > CORS > Authentication > Authorization > # CustomMiddleware 1 > # CustomMiddleware 2 ... > Endpoint.


### 22 Executing middleware conditionally    

hare we are adding some condition for middleware to execute or not;

```cs
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

/*
browser response for (http://localhost:5289/?IsAuthorized=true)  >
Middleware called
last middleware works

browser response for (http://localhost:5289)  >
last middleware works
*/

// Middleware 1
app.UseWhen((HttpContext context)=> context.Request.Query.ContainsKey("IsAuthorized"),
      app =>
      {
        app.Use(async(HttpContext context, RequestDelegate next)=>
        {
          await context.Response.WriteAsync("Middleware called \n");
          await next(context);
        });
      });

app.Run(async(HttpContext context) =>
{
  await context.Response.WriteAsync("last middleware works \n");
});

app.Run();    
```

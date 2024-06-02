# Controller ❤🚀

**Links:**

- [ASP.NET MVC Controller](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#aspnet-mvc-controller-)
- [Controller Base](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#controller-base-)
- [Dependency Injection](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#dependency-injection-)
- [Benefit of DI instead `new` keyword](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#benefit-of-di-instead-new-keyword-)
- [AddSingleton vs AddScoped vs AddTransient](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#addsingleton-vs-addscoped-vs-addtransient-)

## **ASP.NET MVC Controller:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#controller-)

### ASP.NET MVC Controller

In ASP.NET MVC, a controller is a fundamental component responsible for handling user inputs, processing them, and returning appropriate responses. Controllers are classes that derive from `System.Web.Mvc.Controller` and are placed in the `Controllers` folder of an MVC project. Each public method in a controller is known as an action method, which can be invoked via URL requests.

### Configuration Details

1. **Routing Configuration**: Routes need to be configured to map URLs to the appropriate controller actions. This is typically done in the `RouteConfig.cs` file.
2. **Controller Naming Convention**: By convention, controller class names end with "Controller" (e.g., `HomeController`). This helps the framework to identify them as controllers.

### Key Components of a Controller

1. **Action Methods**: Methods that handle HTTP requests. They return `ActionResult` or its derived types.
2. **ViewBag/ViewData/TempData**: Mechanisms to pass data from controllers to views.
3. **Filters**: Attributes that allow you to execute code before or after action methods (e.g., `[Authorize]`, `[ValidateAntiForgeryToken]`).

### Basic Controller Example

### Step 1: Create a Controller

```csharp
using System.Web.Mvc;

namespace MyMvcApp.Controllers
{
    public class HomeController : Controller
    {
        // Action method for the default home page
        public ActionResult Index()
        {
            return View();
        }

        // Action method for the About page
        public ActionResult About()
        {
            ViewBag.Message = "Your application description page.";
            return View();
        }

        // Action method for the Contact page
        public ActionResult Contact()
        {
            ViewBag.Message = "Your contact page.";
            return View();
        }
    }
}
```

### Step 2: Configure Routing

Ensure your routes are set up in `RouteConfig.cs` to map incoming requests to the appropriate controller actions.

**RouteConfig.cs**:
```csharp
using System.Web.Mvc;
using System.Web.Routing;

public class RouteConfig
{
    public static void RegisterRoutes(RouteCollection routes)
    {
        routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
        );
    }
}
```

### Step 3: Add Views

Create corresponding views for each action method in the `Views/Home` directory.

**Views/Home/Index.cshtml**:
```html
@{
    ViewBag.Title = "Home Page";
}

<h2>Index</h2>
<p>Welcome to the Home Page!</p>
```

**Views/Home/About.cshtml**:
```html
@{
    ViewBag.Title = "About";
}

<h2>About</h2>
<p>@ViewBag.Message</p>
```

**Views/Home/Contact.cshtml**:
```html
@{
    ViewBag.Title = "Contact";
}

<h2>Contact</h2>
<p>@ViewBag.Message</p>
```

### Advanced Features

#### Using Parameters

You can create action methods that accept parameters. These parameters can be passed via the URL or query string.

**Controller with Parameterized Action**:
```csharp
public class HomeController : Controller
{
    public ActionResult Details(int id)
    {
        ViewBag.Id = id;
        return View();
    }
}
```

**Views/Home/Details.cshtml**:
```html
@{
    ViewBag.Title = "Details";
}

<h2>Details</h2>
<p>ID: @ViewBag.Id</p>
```

### Using Filters

Filters can be applied to controller actions to handle cross-cutting concerns like authentication, authorization, and logging.

**Controller with Filters**:
```csharp
[Authorize]
public class AdminController : Controller
{
    public ActionResult Dashboard()
    {
        return View();
    }
}
```

### Data Passing Mechanisms

1. **ViewBag**: A dynamic object to pass data from controller to view.
2. **ViewData**: A dictionary for passing data from controller to view.
3. **TempData**: Used to pass data between two consecutive requests.

**Using ViewBag**:
```csharp
public ActionResult About()
{
    ViewBag.Message = "Your application description page.";
    return View();
}
```

### Controller with Dependency Injection

ASP.NET MVC supports dependency injection to make controllers more testable and decoupled from specific implementations.

**Controller with Dependency Injection**:
```csharp
public class HomeController : Controller
{
    private readonly IMyService _myService;

    public HomeController(IMyService myService)
    {
        _myService = myService;
    }

    public ActionResult Index()
    {
        var data = _myService.GetData();
        return View(data);
    }
}
```

### Summary

Controllers are an essential part of the ASP.NET MVC framework, managing the flow of data and interaction between the user, model, and views. Properly configuring routes, using action methods, and leveraging filters and data passing mechanisms are crucial for developing robust and maintainable MVC applications.

## **Controller Base:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#controller-)

### Controller Base Class in ASP.NET MVC

In ASP.NET MVC, the `Controller` base class is the foundation for all controllers. It provides a range of methods and properties to handle HTTP requests, manage data, and return responses. Here’s an in-depth look at the `Controller` base class, its key features, and usage examples.

#### Namespace

```csharp
using System.Web.Mvc;
```

### Key Features of the `Controller` Base Class

1. **Action Methods**: Define methods to handle HTTP requests and return action results.
2. **Action Results**: Methods return various types of `ActionResult` to generate responses.
3. **ViewData and ViewBag**: Facilitate data passing between the controller and views.
4. **TempData**: Provides a way to store data temporarily across requests.
5. **ModelState**: Manages validation errors.
6. **Filters**: Apply cross-cutting concerns like authorization, caching, etc.
7. **HTTP Utility Methods**: Handle HTTP-specific details like requests, responses, cookies, and sessions.

### Key Properties and Methods

#### Properties

- **ControllerContext**: Provides information about the current request and the controller.
- **HttpContext**: Gives access to the current HTTP context.
- **Request**: Represents the current HTTP request.
- **Response**: Represents the current HTTP response.
- **RouteData**: Contains the route data for the current request.
- **ModelState**: Holds the state of model binding and validation errors.
- **TempData**: Stores temporary data across requests.
- **ViewBag**: A dynamic object to pass data from the controller to the view.
- **ViewData**: A dictionary to pass data from the controller to the view.

#### Methods

- **View()**: Returns a `ViewResult` that renders a view.
- **RedirectToAction()**: Redirects to a different action method.
- **RedirectToRoute()**: Redirects to a different route.
- **Json()**: Returns a `JsonResult` that serializes an object to JSON format.
- **Content()**: Returns a `ContentResult` that renders plain text.
- **File()**: Returns a `FileResult` that sends a file to the response.
- **PartialView()**: Returns a `PartialViewResult` that renders a partial view.
- **HttpNotFound()**: Returns a `HttpNotFoundResult`.
- **HttpStatusCodeResult()**: Returns a `HttpStatusCodeResult` with a specific status code.

Sure, here is a detailed explanation of the methods and properties of the `Controller` base class in ASP.NET MVC, presented in tabular form:

### Properties

| Property            | Description                                                                                                                                                 | Example                                                                                      |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| `ControllerContext` | Provides information about the current request and the controller, including HTTP context, route data, and controller metadata.                              | `var context = this.ControllerContext;`                                                      |
| `HttpContext`       | Gives access to the current HTTP context, including request and response objects, session state, and server utility methods.                                 | `var httpContext = this.HttpContext;`                                                        |
| `Request`           | Represents the current HTTP request, providing access to request data like query string, form values, cookies, and headers.                                  | `var request = this.Request;`                                                                |
| `Response`          | Represents the current HTTP response, allowing you to control the response sent to the client, including status code, headers, and cookies.                 | `this.Response.StatusCode = 404;`                                                            |
| `RouteData`         | Contains the route data for the current request, including route parameters and data tokens.                                                                | `var routeData = this.RouteData;`                                                            |
| `ModelState`        | Holds the state of model binding and validation errors, providing methods to check for and display validation errors.                                        | `if (!ModelState.IsValid) { /* Handle validation error */ }`                                 |
| `TempData`          | Stores data that needs to persist between requests, typically for the duration of a single redirect.                                                        | `TempData["Message"] = "This is a temporary message.";`                                      |
| `ViewBag`           | A dynamic object to pass data from the controller to the view, similar to `ViewData`, but with dynamic typing.                                              | `ViewBag.Title = "Home Page";`                                                               |
| `ViewData`          | A dictionary to pass data from the controller to the view, allowing strongly-typed access to data within views.                                             | `ViewData["Message"] = "Welcome to the home page!";`                                         |

### Methods

| Method                     | Description                                                                                                                                                 | Example                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| `View()`                   | Returns a `ViewResult` that renders a view.                                                                                                                  | `return View();`                                                                             |
| `RedirectToAction()`       | Redirects to a different action method.                                                                                                                      | `return RedirectToAction("About");`                                                          |
| `RedirectToRoute()`        | Redirects to a different route.                                                                                                                              | `return RedirectToRoute("Default", new { controller = "Home", action = "Index" });`          |
| `Json()`                   | Returns a `JsonResult` that serializes an object to JSON format.                                                                                             | `return Json(new { Name = "John", Age = 30 }, JsonRequestBehavior.AllowGet);`                |
| `Content()`                | Returns a `ContentResult` that renders plain text.                                                                                                           | `return Content("This is plain text.");`                                                     |
| `File()`                   | Returns a `FileResult` that sends a file to the response.                                                                                                    | `return File(fileBytes, System.Net.Mime.MediaTypeNames.Application.Octet, "Sample.pdf");`    |
| `PartialView()`            | Returns a `PartialViewResult` that renders a partial view.                                                                                                   | `return PartialView("_PartialViewName");`                                                    |
| `HttpNotFound()`           | Returns a `HttpNotFoundResult`, which represents a 404 Not Found status code.                                                                                | `return HttpNotFound();`                                                                     |
| `HttpStatusCodeResult()`   | Returns a `HttpStatusCodeResult` with a specific status code, allowing you to return custom HTTP status codes.                                               | `return new HttpStatusCodeResult(403);`                                                      |
| `OnActionExecuting()`      | Called before an action method is executed, can be overridden to implement custom logic to be executed before action methods.                                 | `protected override void OnActionExecuting(ActionExecutingContext filterContext) { /* ... */ }`|
| `OnActionExecuted()`       | Called after an action method is executed, can be overridden to implement custom logic to be executed after action methods.                                  | `protected override void OnActionExecuted(ActionExecutedContext filterContext) { /* ... */ }`|
| `OnResultExecuting()`      | Called before a result is executed, can be overridden to implement custom logic to be executed before the result is executed.                                | `protected override void OnResultExecuting(ResultExecutingContext filterContext) { /* ... */ }`|
| `OnResultExecuted()`       | Called after a result is executed, can be overridden to implement custom logic to be executed after the result is executed.                                  | `protected override void OnResultExecuted(ResultExecutedContext filterContext) { /* ... */ }`|

### Example Code Using Properties and Methods

Here is an example of a simple controller using some of these properties and methods:

```csharp
using System.Web.Mvc;

namespace MyMvcApp.Controllers
{
    public class HomeController : Controller
    {
        // Action method for the home page
        public ActionResult Index()
        {
            ViewBag.Message = "Welcome to the home page!";
            return View();
        }

        // Action method for the about page
        public ActionResult About()
        {
            ViewData["Message"] = "Your application description page.";
            return View();
        }

        // Action method for the contact page
        public ActionResult Contact()
        {
            TempData["Message"] = "Your contact page.";
            return View();
        }

        // Action method that returns a JSON result
        public JsonResult GetJsonData()
        {
            var data = new { Name = "John", Age = 30 };
            return Json(data, JsonRequestBehavior.AllowGet);
        }

        // Action method that returns a file
        public FileResult DownloadFile()
        {
            byte[] fileBytes = System.IO.File.ReadAllBytes(Server.MapPath("~/Content/Sample.pdf"));
            string fileName = "Sample.pdf";
            return File(fileBytes, System.Net.Mime.MediaTypeNames.Application.Octet, fileName);
        }

        // Action method that returns plain text
        public ContentResult GetPlainText()
        {
            return Content("This is plain text.");
        }

        // Action method that returns HTTP status code
        public ActionResult CustomStatus()
        {
            return new HttpStatusCodeResult(403, "Forbidden");
        }

        // Overriding OnActionExecuting to implement custom logic before action methods
        protected override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            // Custom logic here
            base.OnActionExecuting(filterContext);
        }

        // Overriding OnActionExecuted to implement custom logic after action methods
        protected override void OnActionExecuted(ActionExecutedContext filterContext)
        {
            // Custom logic here
            base.OnActionExecuted(filterContext);
        }

        // Overriding OnResultExecuting to implement custom logic before result is executed
        protected override void OnResultExecuting(ResultExecutingContext filterContext)
        {
            // Custom logic here
            base.OnResultExecuting(filterContext);
        }

        // Overriding OnResultExecuted to implement custom logic after result is executed
        protected override void OnResultExecuted(ResultExecutedContext filterContext)
        {
            // Custom logic here
            base.OnResultExecuted(filterContext);
        }
    }
}
```

In this example, the `HomeController` demonstrates the use of various properties and methods provided by the `Controller` base class. This includes returning different types of results, using `ViewBag`, `ViewData`, and `TempData` to pass data to views, and overriding action and result filter methods to implement custom logic before and after action methods and results are executed.

## **Dependency Injection:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#controller-)

### Dependency Injection in ASP.NET Core MVC

Dependency Injection (DI) is a design pattern used to implement IoC (Inversion of Control). It allows the creation of dependent objects outside of a class and provides those objects to a class in various ways. ASP.NET Core has built-in support for dependency injection, making it easier to manage and test the dependencies of your classes.

### Key Concepts of Dependency Injection

1. **Service**: A class that provides functionality to other classes.
2. **Client**: A class that depends on a service.
3. **Service Lifetime**: The duration a service object is available:
   - **Transient**: Created each time they are requested.
   - **Scoped**: Created once per request.
   - **Singleton**: Created once and shared throughout the application's lifetime.

### Steps to Implement Dependency Injection in ASP.NET Core MVC

1. **Create a Service Interface and Implementation**
2. **Register the Service in the `Startup.cs`**
3. **Inject the Service into a Controller**

#### Step-by-Step Example

1. **Create a Service Interface and Implementation**

   First, define an interface and its implementation. Let's create a simple service that performs basic arithmetic operations.

   **IArithmeticService.cs**

   ```csharp
   public interface IArithmeticService
   {
       int Add(int a, int b);
       int Subtract(int a, int b);
       int Multiply(int a, int b);
       int Divide(int a, int b);
   }
   ```

   **ArithmeticService.cs**

   ```csharp
   public class ArithmeticService : IArithmeticService
   {
       public int Add(int a, int b)
       {
           return a + b;
       }

       public int Subtract(int a, int b)
       {
           return a - b;
       }

       public int Multiply(int a, int b)
       {
           return a * b;
       }

       public int Divide(int a, int b)
       {
           if (b == 0)
           {
               throw new DivideByZeroException("Divider cannot be zero.");
           }
           return a / b;
       }
   }
   ```

2. **Register the Service in `Startup.cs`**

   In the `ConfigureServices` method of the `Startup` class, register your service with the DI container.

   **Startup.cs**

   ```csharp
   public class Startup
   {
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddControllersWithViews();
           
           // Registering the ArithmeticService with a transient lifetime
           services.AddTransient<IArithmeticService, ArithmeticService>();
       }

       public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
       {
           if (env.IsDevelopment())
           {
               app.UseDeveloperExceptionPage();
           }
           else
           {
               app.UseExceptionHandler("/Home/Error");
               app.UseHsts();
           }

           app.UseHttpsRedirection();
           app.UseStaticFiles();

           app.UseRouting();

           app.UseAuthorization();

           app.UseEndpoints(endpoints =>
           {
               endpoints.MapControllerRoute(
                   name: "default",
                   pattern: "{controller=Home}/{action=Index}/{id?}");
           });
       }
   }
   ```

3. **Inject the Service into a Controller**

   Use constructor injection to get the service in a controller.

   **HomeController.cs**

   ```csharp
   public class HomeController : Controller
   {
       private readonly IArithmeticService _arithmeticService;

       public HomeController(IArithmeticService arithmeticService)
       {
           _arithmeticService = arithmeticService;
       }

       public IActionResult Index()
       {
           var result = _arithmeticService.Add(5, 3);
           ViewBag.Result = result;
           return View();
       }
   }
   ```

4. **Create a View to Display the Result**

   **Views/Home/Index.cshtml**

   ```html
   @*
       Sample View to display the result
   *@
   @model dynamic
   <div>
       <h1>Arithmetic Operations Result</h1>
       <p>Result of Addition: @ViewBag.Result</p>
   </div>
   ```

### Summary

By following these steps, you can implement dependency injection in your ASP.NET Core MVC application. The example demonstrates how to define a service interface and its implementation, register the service with the DI container, and inject the service into a controller using constructor injection.

### Full Example Code

Here is the complete code in one place for easy reference:

**IArithmeticService.cs**

```csharp
public interface IArithmeticService
{
    int Add(int a, int b);
    int Subtract(int a, int b);
    int Multiply(int a, int b);
    int Divide(int a, int b);
}
```

**ArithmeticService.cs**

```csharp
public class ArithmeticService : IArithmeticService
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Subtract(int a, int b)
    {
        return a - b;
    }

    public int Multiply(int a, int b)
    {
        return a * b;
    }

    public int Divide(int a, int b)
    {
        if (b == 0)
        {
            throw new DivideByZeroException("Divider cannot be zero.");
        }
        return a / b;
    }
}
```

**Startup.cs**

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllersWithViews();
        
        services.AddTransient<IArithmeticService, ArithmeticService>();
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/Home/Error");
            app.UseHsts();
        }

        app.UseHttpsRedirection();
        app.UseStaticFiles();

        app.UseRouting();

        app.UseAuthorization();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapControllerRoute(
                name: "default",
                pattern: "{controller=Home}/{action=Index}/{id?}");
        });
    }
}
```

**HomeController.cs**

```csharp
public class HomeController : Controller
{
    private readonly IArithmeticService _arithmeticService;

    public HomeController(IArithmeticService arithmeticService)
    {
        _arithmeticService = arithmeticService;
    }

    public IActionResult Index()
    {
        var result = _arithmeticService.Add(5, 3);
        ViewBag.Result = result;
        return View();
    }
}
```

**Views/Home/Index.cshtml**

```html
@model dynamic
<div>
    <h1>Arithmetic Operations Result</h1>
    <p>Result of Addition: @ViewBag.Result</p>
</div>
```

This example covers the basic implementation of dependency injection in ASP.NET Core MVC, demonstrating how to set up and use services within your application.

## **Benefit of DI instead `new` keyword:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#controller-)

### Why Use Dependency Injection Instead of the `new` Keyword?

Dependency Injection (DI) is preferred over directly instantiating objects with the `new` keyword in scenarios where you want to achieve better decoupling, testability, and maintainability of your code. DI allows you to inject dependencies into your classes, rather than having the classes create the dependencies themselves.

### Problem Scenario Without Dependency Injection

Let's consider a simple example where we have a `HomeController` that uses a service `ArithmeticService`.

#### Without Dependency Injection

```csharp
public interface IArithmeticService
{
    int Add(int a, int b);
}

public class ArithmeticService : IArithmeticService
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}

public class HomeController : Controller
{
    private readonly IArithmeticService _arithmeticService;

    public HomeController()
    {
        _arithmeticService = new ArithmeticService();
    }

    public IActionResult Index()
    {
        var result = _arithmeticService.Add(5, 3);
        ViewBag.Result = result;
        return View();
    }
}
```

### Problems with Direct Instantiation

1. **Tight Coupling**: The `HomeController` is tightly coupled to the `ArithmeticService`. If you need to change the implementation of `ArithmeticService`, you would have to modify the `HomeController`.

2. **Difficult to Test**: Unit testing the `HomeController` becomes difficult because you cannot easily replace `ArithmeticService` with a mock implementation. This makes it hard to isolate the behavior of the controller.

3. **Limited Flexibility**: If you want to introduce a different implementation of `IArithmeticService` (e.g., `AdvancedArithmeticService`), you would need to change the constructor of the `HomeController`, making the code less flexible.

### Solution with Dependency Injection

By using dependency injection, you can overcome these problems by injecting the dependency rather than creating it inside the class.

#### With Dependency Injection

1. **Define the Service Interface and Implementation**

```csharp
public interface IArithmeticService
{
    int Add(int a, int b);
}

public class ArithmeticService : IArithmeticService
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}
```

2. **Register the Service in `Startup.cs`**

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllersWithViews();
        
        // Register the ArithmeticService with a transient lifetime
        services.AddTransient<IArithmeticService, ArithmeticService>();
    }

    // Other methods omitted for brevity
}
```

3. **Inject the Service into the Controller**

```csharp
public class HomeController : Controller
{
    private readonly IArithmeticService _arithmeticService;

    public HomeController(IArithmeticService arithmeticService)
    {
        _arithmeticService = arithmeticService;
    }

    public IActionResult Index()
    {
        var result = _arithmeticService.Add(5, 3);
        ViewBag.Result = result;
        return View();
    }
}
```

### Advantages of Dependency Injection

1. **Loose Coupling**: The `HomeController` is not directly coupled to `ArithmeticService`. It only depends on the `IArithmeticService` interface. This allows you to change the implementation without modifying the controller.

2. **Improved Testability**: You can easily mock `IArithmeticService` and test the `HomeController` in isolation. This is crucial for writing effective unit tests.

```csharp
public class HomeControllerTests
{
    [Fact]
    public void Index_ReturnsCorrectResult()
    {
        // Arrange
        var mockService = new Mock<IArithmeticService>();
        mockService.Setup(s => s.Add(5, 3)).Returns(8);
        var controller = new HomeController(mockService.Object);

        // Act
        var result = controller.Index() as ViewResult;

        // Assert
        Assert.Equal(8, result.ViewBag.Result);
    }
}
```

3. **Increased Flexibility**: You can easily switch between different implementations of `IArithmeticService` (e.g., `AdvancedArithmeticService`) by changing the registration in `Startup.cs`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    
    // Register a different implementation
    services.AddTransient<IArithmeticService, AdvancedArithmeticService>();
}
```

### Summary

Dependency Injection offers a way to decouple the creation of an object from its usage. This promotes code reusability, easier maintenance, and better testability. By using DI, you can focus on the behavior of your classes without worrying about the creation and lifecycle management of their dependencies.

## **AddSingleton vs AddScoped vs AddTransient:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/3%20-%20Controller.md#controller-)

Here’s a detailed comparison of `AddSingleton`, `AddScoped`, and `AddTransient` in ASP.NET Core dependency injection, including scenarios where each might be used:

| Feature/Aspect               | AddSingleton                                         | AddScoped                                           | AddTransient                                        |
|------------------------------|------------------------------------------------------|----------------------------------------------------|----------------------------------------------------|
| **Lifetime**                 | Singleton                                            | Scoped                                              | Transient                                          |
| **Instance Creation**        | Created once and shared across all requests.         | Created once per request (or per scope).            | Created every time it's requested.                 |
| **Usage Scenario**           | When a single instance should be shared across the entire application. | When an instance should be shared within a request, but not across requests. | When a new instance is needed every time it's requested. |
| **Memory Consumption**       | Lower memory footprint as only one instance is created. | Moderate memory footprint, one instance per request. | Higher memory footprint, new instance per request. |
| **Performance**              | High performance as only one instance is created and reused. | Moderate performance, one instance per request.     | Lower performance, new instance creation each time.|
| **Thread Safety**            | Must be thread-safe as it's shared across multiple threads. | Can be thread-safe, but only within the request scope. | Generally not required to be thread-safe.          |
| **Example Service**          | Configuration settings, Logging services, Caching services. | Database context (EF Core), Unit of Work pattern.   | Lightweight, stateless services, utilities.        |
| **Configuration in Startup** | `services.AddSingleton<IService, Service>();`         | `services.AddScoped<IService, Service>();`           | `services.AddTransient<IService, Service>();`      |
| **Instance Example**         | Used for configurations loaded once and shared. Example: `services.AddSingleton<IConfiguration>(Configuration);` | Used for services that need to maintain state within a request. Example: `services.AddScoped<IDbContext, AppDbContext>();` | Used for short-lived services. Example: `services.AddTransient<IEmailSender, EmailSender>();` |

### Scenarios

#### Singleton Scenario

You have a configuration service that reads settings from a configuration file. This configuration is constant throughout the lifetime of the application and should be shared.

```csharp
public interface IConfigurationService
{
    string GetSetting(string key);
}

public class ConfigurationService : IConfigurationService
{
    private readonly IConfiguration _configuration;

    public ConfigurationService(IConfiguration configuration)
    {
        _configuration = configuration;
    }

    public string GetSetting(string key)
    {
        return _configuration[key];
    }
}

// Registering as Singleton
services.AddSingleton<IConfigurationService, ConfigurationService>();
```

#### Scoped Scenario

You have a database context that needs to be instantiated per web request to ensure that changes are saved correctly and not shared across requests.

```csharp
public interface IApplicationDbContext
{
    DbSet<User> Users { get; set; }
    int SaveChanges();
}

public class ApplicationDbContext : DbContext, IApplicationDbContext
{
    public DbSet<User> Users { get; set; }
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options) { }
}

// Registering as Scoped
services.AddScoped<IApplicationDbContext, ApplicationDbContext>();
```

#### Transient Scenario

You have an email service that sends emails. Each request to send an email should be independent of others.

```csharp
public interface IEmailSender
{
    void SendEmail(string to, string subject, string body);
}

public class EmailSender : IEmailSender
{
    public void SendEmail(string to, string subject, string body)
    {
        // Logic to send email
    }
}

// Registering as Transient
services.AddTransient<IEmailSender, EmailSender>();
```

### Summary

- **AddSingleton**: Best for services that maintain state across the entire application lifespan and can be safely shared across threads. Ideal for configurations, logging, and caching services.
- **AddScoped**: Best for services that maintain state within a single request or operation. Ideal for database contexts and business logic that should be consistent within a request.
- **AddTransient**: Best for lightweight, stateless services that can be created and disposed of quickly. Ideal for utilities and short-lived operations like email sending or logging individual messages.
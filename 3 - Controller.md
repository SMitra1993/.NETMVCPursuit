# Controller ❤🚀

## **ASP.NET MVC Controller:**

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

#### Step 1: Create a Controller

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

## **Controller Base:**

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

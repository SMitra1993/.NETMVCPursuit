# Action ❤🚀

**Links:**

- [Action Method](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-method-)
- [Action Result](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-result-)
- [Action Selector](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-selector-)
- [Action Filter](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-filter-)
- [Action Selector Vs Action Filter](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-selector-vs-action-filter-)
- [Register Filter at different level](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#register-filter-at-different-level-)
- [Action Filter Vs Result Filter](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-filter-vs-result-filter-)

## **Action Method:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-)

In ASP.NET MVC, action methods are methods in a controller that handle incoming HTTP requests and determine the response to be sent back to the client. These methods are the core of the MVC framework and facilitate the processing of requests, manipulation of data, and return of appropriate responses.

### Types of Action Methods

1. **Standard Action Methods**:
   - These are the most common type of action methods. They process the request, perform any necessary business logic or data access, and then return a result.

2. **Async Action Methods**:
   - These methods are designed to handle asynchronous operations, making it possible to perform long-running tasks without blocking the main thread.

3. **Child Action Methods**:
   - These are used in conjunction with the `Html.Action` or `Html.RenderAction` helper methods to render a partial view within a view. They are typically marked with the `[ChildActionOnly]` attribute to indicate that they cannot be called directly from the browser.

4. **Non-Action Methods**:
   - These are methods within a controller that are not exposed as action methods. They are marked with the `[NonAction]` attribute.

### Standard Action Methods Example

Here’s an example of a standard action method in an ASP.NET MVC controller:

```csharp
using System.Web.Mvc;

namespace MyMvcApp.Controllers
{
    public class HomeController : Controller
    {
        // Standard action method
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
    }
}
```

### Async Action Methods Example

Here’s an example of an asynchronous action method:

```csharp
using System.Threading.Tasks;
using System.Web.Mvc;

namespace MyMvcApp.Controllers
{
    public class HomeController : Controller
    {
        // Asynchronous action method
        public async Task<ActionResult> AsyncOperation()
        {
            var result = await LongRunningOperationAsync();
            ViewBag.Result = result;
            return View();
        }

        private async Task<string> LongRunningOperationAsync()
        {
            // Simulate a long-running operation
            await Task.Delay(5000);
            return "Operation Completed";
        }
    }
}
```

### Child Action Methods Example

Here’s an example of a child action method:

```csharp
using System.Web.Mvc;

namespace MyMvcApp.Controllers
{
    public class HomeController : Controller
    {
        // Child action method
        [ChildActionOnly]
        public ActionResult Sidebar()
        {
            var sidebarItems = GetSidebarItems();
            return PartialView("_Sidebar", sidebarItems);
        }

        private List<string> GetSidebarItems()
        {
            return new List<string> { "Item 1", "Item 2", "Item 3" };
        }
    }
}
```

And in the view, you would call this child action method like this:

```html
@Html.Action("Sidebar", "Home")
```

### Non-Action Methods Example

Here’s an example of a non-action method:

```csharp
using System.Web.Mvc;

namespace MyMvcApp.Controllers
{
    public class HomeController : Controller
    {
        // Standard action method
        public ActionResult Index()
        {
            var data = GetData();
            ViewBag.Data = data;
            return View();
        }

        // Non-action method
        [NonAction]
        private string GetData()
        {
            return "Some Data";
        }
    }
}
```

### Summary Table

| **Action Method Type** | **Description**                                                                               | **Example**                                    |
|------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------|
| Standard Action Methods| Handle standard HTTP requests.                                                               | `public ActionResult Index()`                  |
| Async Action Methods   | Handle long-running tasks asynchronously.                                                    | `public async Task<ActionResult> AsyncOperation()` |
| Child Action Methods   | Render partial views within a view, cannot be called directly from the browser.              | `[ChildActionOnly] public ActionResult Sidebar()` |
| Non-Action Methods     | Methods within a controller that are not exposed as action methods.                          | `[NonAction] private string GetData()`         |

### Explanation of Action Method Execution Flow

1. **Routing**: The MVC routing system maps the incoming request to the appropriate controller and action method.
2. **Model Binding**: The framework automatically maps the incoming request data to the parameters of the action method.
3. **Action Execution**: The action method executes, performing the necessary business logic or data access.
4. **Result Execution**: The action method returns an `ActionResult` (e.g., `ViewResult`, `JsonResult`, `RedirectToRouteResult`), which the framework processes to generate the response sent to the client.

Action methods are central to handling requests in ASP.NET MVC, allowing for various types of operations and responses tailored to the application's needs.

## **Action Result:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-)

In ASP.NET MVC, action methods return objects derived from the `ActionResult` class. Each type of `ActionResult` represents a different kind of response to the client. Below is a detailed explanation of the various `ActionResult` types, along with examples for each, presented in a tabular format.

| **ActionResult Type** | **Description**                                                                                 | **Example**                                                                                      |
|-----------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| `ViewResult`          | Renders a view as a web page.                                                                  | `return View();`                                                                                 |
| `PartialViewResult`   | Renders a partial view, typically used for updating a part of a web page.                      | `return PartialView("_PartialViewName");`                                                        |
| `RedirectResult`      | Redirects to a different URL.                                                                  | `return Redirect("https://www.example.com");`                                                    |
| `RedirectToRouteResult` | Redirects to a different route, which can be another action method.                           | `return RedirectToAction("ActionName", "ControllerName");`                                       |
| `ContentResult`       | Returns a user-defined content type (e.g., text, HTML).                                        | `return Content("Hello World");`                                                                 |
| `JsonResult`          | Returns JSON-formatted data, useful for AJAX requests.                                         | `return Json(new { name = "John", age = 30 }, JsonRequestBehavior.AllowGet);`                    |
| `FileResult`          | Returns a file to the client.                                                                  | `return File("~/path/to/file.pdf", "application/pdf");`                                          |
| `HttpStatusCodeResult` | Returns a specific HTTP status code.                                                          | `return new HttpStatusCodeResult(404);`                                                          |
| `JavaScriptResult`    | Returns JavaScript code that will be executed on the client side.                              | `return JavaScript("alert('Hello World');");`                                                    |
| `EmptyResult`         | Represents a no-op result, returns nothing.                                                    | `return new EmptyResult();`                                                                      |
| `HttpUnauthorizedResult` | Returns an HTTP 401 status code indicating unauthorized access.                             | `return new HttpUnauthorizedResult();`                                                           |
| `FileContentResult`   | Returns a file as a byte array.                                                                | `return File(fileBytes, "application/pdf");`                                                     |
| `FileStreamResult`    | Returns a file from a stream.                                                                  | `return new FileStreamResult(stream, "application/pdf");`                                        |
| `VirtualFileResult`   | Returns a file from the virtual file system.                                                   | `return new VirtualFileResult("~/path/to/file.pdf", "application/pdf");`                         |

### Detailed Examples

#### 1. `ViewResult`
Renders a view as a web page.
```csharp
public ActionResult Index()
{
    return View();
}
```

#### 2. `PartialViewResult`
Renders a partial view.
```csharp
public ActionResult GetPartialView()
{
    return PartialView("_PartialViewName");
}
```

#### 3. `RedirectResult`
Redirects to a different URL.
```csharp
public ActionResult RedirectToExternalSite()
{
    return Redirect("https://www.example.com");
}
```

#### 4. `RedirectToRouteResult`
Redirects to a different route.
```csharp
public ActionResult RedirectToAnotherAction()
{
    return RedirectToAction("AnotherAction", "AnotherController");
}
```

#### 5. `ContentResult`
Returns user-defined content.
```csharp
public ActionResult GetContent()
{
    return Content("Hello World", "text/plain");
}
```

#### 6. `JsonResult`
Returns JSON-formatted data.
```csharp
public ActionResult GetJsonData()
{
    var data = new { name = "John", age = 30 };
    return Json(data, JsonRequestBehavior.AllowGet);
}
```

#### 7. `FileResult`
Returns a file.
```csharp
public ActionResult DownloadFile()
{
    string filePath = Server.MapPath("~/path/to/file.pdf");
    return File(filePath, "application/pdf");
}
```

#### 8. `HttpStatusCodeResult`
Returns a specific HTTP status code.
```csharp
public ActionResult NotFound()
{
    return new HttpStatusCodeResult(404);
}
```

#### 9. `JavaScriptResult`
Returns JavaScript code.
```csharp
public ActionResult GetJavaScript()
{
    return JavaScript("alert('Hello World');");
}
```

#### 10. `EmptyResult`
Represents a no-op result.
```csharp
public ActionResult DoNothing()
{
    return new EmptyResult();
}
```

#### 11. `HttpUnauthorizedResult`
Returns an HTTP 401 status code.
```csharp
public ActionResult UnauthorizedAccess()
{
    return new HttpUnauthorizedResult();
}
```

#### 12. `FileContentResult`
Returns a file as a byte array.
```csharp
public ActionResult DownloadFileContent()
{
    byte[] fileBytes = System.IO.File.ReadAllBytes(Server.MapPath("~/path/to/file.pdf"));
    return File(fileBytes, "application/pdf");
}
```

#### 13. `FileStreamResult`
Returns a file from a stream.
```csharp
public ActionResult DownloadFileStream()
{
    var stream = new FileStream(Server.MapPath("~/path/to/file.pdf"), FileMode.Open);
    return new FileStreamResult(stream, "application/pdf");
}
```

#### 14. `VirtualFileResult`
Returns a file from the virtual file system.
```csharp
public ActionResult DownloadVirtualFile()
{
    return new VirtualFileResult("~/path/to/file.pdf", "application/pdf");
}
```

These `ActionResult` types allow ASP.NET MVC controllers to return a variety of responses to handle different scenarios, making the framework flexible and powerful for web application development.

## **Action Selector:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-)

In ASP.NET MVC, action selectors are used to control which action method should be invoked in a controller. They are attributes that help in differentiating between multiple actions in the same controller based on certain criteria. This is particularly useful when you have actions that can be invoked based on different conditions like HTTP methods, custom logic, or action names.

Here are the main types of action selectors in ASP.NET MVC:

1. **ActionNameSelectorAttribute**
2. **NonActionAttribute**
3. **AcceptVerbsAttribute**

### 1. ActionNameSelectorAttribute

The `ActionNameSelectorAttribute` allows you to map a different name to an action method. This is useful if you want the method name to be different from the action name in the URL.

**Example:**
```csharp
public class ProductsController : Controller
{
    [ActionName("Show")]
    public ActionResult Display()
    {
        return View();
    }
}
```
In this example, the action method `Display` will be invoked when the URL is `/Products/Show`.

### 2. NonActionAttribute

The `NonActionAttribute` marks a public method in a controller as not an action method. This is useful if you want to expose helper methods within your controller that should not be treated as actions.

**Example:**
```csharp
public class HomeController : Controller
{
    public ActionResult Index()
    {
        return View();
    }

    [NonAction]
    public string HelperMethod()
    {
        return "This is a helper method.";
    }
}
```
In this example, `HelperMethod` will not be treated as an action method and cannot be invoked via a URL.

### 3. AcceptVerbsAttribute

The `AcceptVerbsAttribute` restricts an action method to be invoked only for specific HTTP verbs. This is useful for creating RESTful services where the same URL may support different verbs for different operations.

**Example:**
```csharp
public class AccountController : Controller
{
    [HttpGet]
    public ActionResult Login()
    {
        return View();
    }

    [HttpPost]
    public ActionResult Login(string username, string password)
    {
        // Logic to authenticate the user
        return RedirectToAction("Index", "Home");
    }
}
```
In this example, the `Login` method with `[HttpGet]` attribute will handle GET requests, and the `Login` method with `[HttpPost]` attribute will handle POST requests.

### Custom Action Selectors

You can also create custom action selectors by deriving from `ActionMethodSelectorAttribute`. This allows you to define complex selection logic for your action methods.

**Example:**
```csharp
public class CustomActionSelectorAttribute : ActionMethodSelectorAttribute
{
    public override bool IsValidForRequest(ControllerContext controllerContext, MethodInfo methodInfo)
    {
        // Custom logic to determine if the method is valid for the request
        return controllerContext.HttpContext.Request.QueryString["custom"] == "true";
    }
}

public class HomeController : Controller
{
    [CustomActionSelector]
    public ActionResult CustomAction()
    {
        return View();
    }
}
```
In this example, `CustomAction` will only be invoked if the query string contains `custom=true`.

### Summary of Action Selectors

| **Attribute**               | **Purpose**                                                                                      | **Example**                                                                                                      |
|-----------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `ActionNameSelectorAttribute` | Maps a different action name to a method.                                                       | `[ActionName("Show")] public ActionResult Display() { return View(); }`                                           |
| `NonActionAttribute`          | Marks a method as not an action method.                                                        | `[NonAction] public string HelperMethod() { return "Helper"; }`                                                   |
| `AcceptVerbsAttribute`        | Restricts an action method to specific HTTP verbs.                                             | `[HttpGet] public ActionResult Login() { return View(); } [HttpPost] public ActionResult Login(string u, string p) { //... }` |
| Custom Action Selectors       | Allows creating custom selection logic for action methods.                                     | `public class CustomActionSelectorAttribute : ActionMethodSelectorAttribute { //... }`                            |

Action selectors provide a flexible way to control which methods are exposed as actions and under what conditions they can be invoked. This allows for greater control over the routing and handling of HTTP requests in an ASP.NET MVC application.

## **Action Filter:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-)

In ASP.NET MVC, action filters are used to inject extra processing logic before and after action method execution. They provide a way to apply cross-cutting concerns such as logging, authorization, caching, and more. Action filters can be applied globally, at the controller level, or at the action method level.

### Types of Action Filters

ASP.NET MVC provides several types of action filters:

1. **Authorization Filters**
2. **Action Filters**
3. **Result Filters**
4. **Exception Filters**

### 1. Authorization Filters

Authorization filters are used to control access to action methods. They are executed before any other filters.

**Example:**
```csharp
public class CustomAuthorizeAttribute : AuthorizeAttribute
{
    protected override bool AuthorizeCore(HttpContextBase httpContext)
    {
        // Custom authorization logic
        return httpContext.User.Identity.IsAuthenticated;
    }

    protected override void HandleUnauthorizedRequest(AuthorizationContext filterContext)
    {
        // Custom logic for handling unauthorized requests
        filterContext.Result = new RedirectResult("~/Account/Login");
    }
}
```
Usage:
```csharp
[CustomAuthorize]
public ActionResult SecureAction()
{
    return View();
}
```

### 2. Action Filters

Action filters run before and after the execution of action methods. They can be used for tasks such as logging, input validation, or modifying the action result.

**Example:**
```csharp
public class LogActionFilter : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext filterContext)
    {
        // Code to run before the action method execution
        Log("OnActionExecuting", filterContext.RouteData);
    }

    public override void OnActionExecuted(ActionExecutedContext filterContext)
    {
        // Code to run after the action method execution
        Log("OnActionExecuted", filterContext.RouteData);
    }

    private void Log(string methodName, RouteData routeData)
    {
        // Logging logic
        var controllerName = routeData.Values["controller"];
        var actionName = routeData.Values["action"];
        // Log the details
    }
}
```
Usage:
```csharp
[LogActionFilter]
public ActionResult Index()
{
    return View();
}
```

### 3. Result Filters

Result filters run before and after the execution of the action result. They are useful for modifying the response before it is sent to the client.

**Example:**
```csharp
public class CustomResultFilter : ResultFilterAttribute
{
    public override void OnResultExecuting(ResultExecutingContext filterContext)
    {
        // Code to run before the result execution
    }

    public override void OnResultExecuted(ResultExecutedContext filterContext)
    {
        // Code to run after the result execution
    }
}
```
Usage:
```csharp
[CustomResultFilter]
public ActionResult Index()
{
    return View();
}
```

### 4. Exception Filters

Exception filters are used to handle exceptions thrown by action methods. They provide a way to log exceptions or return custom error responses.

**Example:**
```csharp
public class CustomExceptionFilter : FilterAttribute, IExceptionFilter
{
    public void OnException(ExceptionContext filterContext)
    {
        // Handle the exception
        filterContext.ExceptionHandled = true;
        filterContext.Result = new ViewResult
        {
            ViewName = "Error"
        };
    }
}
```
Usage:
```csharp
[CustomExceptionFilter]
public ActionResult Index()
{
    throw new Exception("Test exception");
    return View();
}
```

### Applying Filters

Filters can be applied in different ways:

1. **Globally**: In `Global.asax`
```csharp
public class MvcApplication : System.Web.HttpApplication
{
    protected void Application_Start()
    {
        GlobalFilters.Filters.Add(new CustomExceptionFilter());
    }
}
```

2. **At the Controller Level**
```csharp
[CustomExceptionFilter]
public class HomeController : Controller
{
    public ActionResult Index()
    {
        return View();
    }
}
```

3. **At the Action Method Level**
```csharp
public class HomeController : Controller
{
    [CustomExceptionFilter]
    public ActionResult Index()
    {
        return View();
    }
}
```

### Summary of Action Filters

| **Filter Type**      | **Purpose**                                                      | **Example Usage**                                                  |
|----------------------|------------------------------------------------------------------|--------------------------------------------------------------------|
| **Authorization Filters** | Control access to action methods.                                | `[CustomAuthorize] public ActionResult SecureAction() { }`         |
| **Action Filters**   | Run code before and after action methods.                        | `[LogActionFilter] public ActionResult Index() { }`                |
| **Result Filters**   | Run code before and after action results.                        | `[CustomResultFilter] public ActionResult Index() { }`             |
| **Exception Filters**| Handle exceptions thrown by action methods.                     | `[CustomExceptionFilter] public ActionResult Index() { }`          |

Action filters provide a powerful way to apply cross-cutting concerns across your ASP.NET MVC application. By leveraging these filters, you can keep your controllers and actions clean and focused on their primary responsibilities.

## **Action Selector Vs Action Filter:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-)

Here's a detailed comparison between Action Selectors and Action Filters in ASP.NET MVC, presented in tabular form:

| **Aspect**                | **Action Selector**                                      | **Action Filter**                                             |
|---------------------------|----------------------------------------------------------|---------------------------------------------------------------|
| **Purpose**               | Determines which action method to execute.               | Executes code before and after an action method is invoked.   |
| **Usage**                 | Used to select the appropriate action method based on custom logic. | Used to implement cross-cutting concerns like logging, authentication, authorization, etc. |
| **Implementation**        | Implemented by creating custom attributes derived from `ActionMethodSelectorAttribute`. | Implemented by creating custom attributes derived from `ActionFilterAttribute`, `IActionFilter`, `IResultFilter`, etc. |
| **Execution Time**        | Executed before the action method is selected.           | Can be executed before and after the action method, and before and after the action result. |
| **Scope**                 | Affects which action method gets executed.               | Does not affect which action method gets executed but can modify its behavior or the result. |
| **Common Uses**           | Overloading actions, selecting actions based on request headers or parameters. | Logging, authentication, authorization, caching, error handling, etc. |
| **Example**               | `[NonAction]` to mark a method that should not be treated as an action method. | `[Authorize]` to restrict access to certain users or roles.   |
| **Attributes**            | Uses attributes derived from `ActionMethodSelectorAttribute`. | Uses attributes derived from `ActionFilterAttribute`, `IActionFilter`, `IResultFilter`, etc. |
| **Custom Implementation** | Requires overriding `IsValidForRequest` method.          | Requires overriding methods like `OnActionExecuting`, `OnActionExecuted`, `OnResultExecuting`, and `OnResultExecuted`. |
| **Effect on Result**      | Does not directly affect the result of the action method. | Can modify the result of the action method and the action result. |

### Examples

#### Action Selector Example

**Custom Action Selector:**
```csharp
public class CustomActionSelectorAttribute : ActionMethodSelectorAttribute
{
    public override bool IsValidForRequest(ControllerContext controllerContext, MethodInfo methodInfo)
    {
        // Custom logic to determine if the action method should be selected
        return controllerContext.HttpContext.Request.QueryString["action"] == methodInfo.Name;
    }
}
```

**Usage:**
```csharp
public class HomeController : Controller
{
    [CustomActionSelector]
    public ActionResult Index()
    {
        return View();
    }

    [CustomActionSelector]
    public ActionResult About()
    {
        return View();
    }
}
```

#### Action Filter Example

**Custom Action Filter:**
```csharp
public class LogActionFilter : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext filterContext)
    {
        // Code to run before the action method execution
        Log("OnActionExecuting", filterContext.RouteData);
    }

    public override void OnActionExecuted(ActionExecutedContext filterContext)
    {
        // Code to run after the action method execution
        Log("OnActionExecuted", filterContext.RouteData);
    }

    private void Log(string methodName, RouteData routeData)
    {
        // Logging logic
        var controllerName = routeData.Values["controller"];
        var actionName = routeData.Values["action"];
        // Log the details
    }
}
```

**Usage:**
```csharp
[LogActionFilter]
public class HomeController : Controller
{
    public ActionResult Index()
    {
        return View();
    }
}
```

### Summary

While both Action Selectors and Action Filters are used to influence the behavior of action methods, they serve different purposes and are used in different contexts. Action Selectors are primarily for choosing which action method to invoke, whereas Action Filters are for adding additional processing around action method execution. Understanding the distinction between these two concepts is crucial for effectively leveraging ASP.NET MVC to build robust and maintainable web applications.

## **Register Filter at different level:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-)

Filters can be applied at different levels:

1. **Global Filters:**
   - Applied to all controllers and action methods.
   ```csharp
   public class FilterConfig
   {
       public static void RegisterGlobalFilters(GlobalFilterCollection filters)
       {
           filters.Add(new HandleErrorAttribute());
       }
   }
   ```

2. **Controller Level:**
   - Applied to all action methods within a controller.
   ```csharp
   [LogActionFilter]
   public class HomeController : Controller
   {
       // All actions in this controller will use the LogActionFilter
   }
   ```

3. **Action Method Level:**
   - Applied to a specific action method.
   ```csharp
   public class HomeController : Controller
   {
       [LogActionFilter]
       public ActionResult Index()
       {
           return View();
       }
   }
   ```

### Summary

ASP.NET MVC filters provide a powerful way to handle cross-cutting concerns in your application. By understanding and leveraging these filters, you can keep your code clean, modular, and maintainable. Each type of filter has a specific role, and they can be used together to address a wide range of scenarios in a structured manner.

## **Action Filter Vs Result Filter:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/4%20-%20Action.md#action-)

Here's a comparison of Action Filters and Result Filters in MVC .NET in a tabular form:

| Feature/Aspect          | Action Filters                                   | Result Filters                                    |
|-------------------------|--------------------------------------------------|--------------------------------------------------|
| **Purpose**             | Used to execute code before and after an action method executes. | Used to execute code before and after the action result executes. |
| **Common Use Cases**    | - Logging<br>- Authorization<br>- Exception handling<br>- Caching  | - Modifying the result<br>- Processing the result before rendering<br>- Caching |
| **Execution Timing**    | - Before the action method (OnActionExecuting)<br>- After the action method (OnActionExecuted) | - Before the result execution (OnResultExecuting)<br>- After the result execution (OnResultExecuted) |
| **Access to Controller Context** | Yes, access to the action method context, controller, and parameters. | Yes, access to the result context and controller. |
| **Access to Action Result** | No, as they run before the action result is produced. | Yes, since they run just before and after the result is processed. |
| **Usage**               | `[ActionFilter]` attribute or implement `IActionFilter` interface. | `[ResultFilter]` attribute or implement `IResultFilter` interface. |
| **Order of Execution**  | Executed in the order they are defined before the action method, and in reverse order after the action method. | Executed in the order they are defined before the result processing, and in reverse order after the result processing. |
| **Example**             | ```csharp [ActionFilter] public class LogActionFilter : IActionFilter { public void OnActionExecuting(ActionExecutingContext context) { // Pre-action logic } public void OnActionExecuted(ActionExecutedContext context) { // Post-action logic } } ``` | ```csharp [ResultFilter] public class ModifyResultFilter : IResultFilter { public void OnResultExecuting(ResultExecutingContext context) { // Pre-result logic } public void OnResultExecuted(ResultExecutedContext context) { // Post-result logic } } ``` |

In summary, Action Filters are focused on the execution flow around the action methods, providing hooks to execute code before and after an action method runs. Result Filters, on the other hand, deal with the execution flow around the action results, providing hooks to execute code before and after the action result is processed. Both are essential for different scenarios in the request processing pipeline of MVC .NET.
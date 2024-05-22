# Action ❤🚀

## **Action Method:**

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

## **Action Result:**

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

## **Action Selector:**

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
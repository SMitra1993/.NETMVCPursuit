# Exception Handling ❤🚀

**Links:**

- [Exception Handling](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/7%20-%20Exception%20Handling.md#exception-handling--1)

## **Exception Handling:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/7%20-%20Exception%20Handling.md#exception-handling-)

Exception handling in ASP.NET MVC is crucial for maintaining the stability and reliability of your application. It involves capturing and gracefully managing errors that occur during the execution of your application. Here's an overview of different approaches to handle exceptions in ASP.NET MVC, along with examples for each:

### 1. Using `<customErrors>` Element in web.config:
The `<customErrors>` element in the web.config file allows you to specify how errors should be handled and displayed to users.

```xml
<system.web>
  <customErrors mode="On" defaultRedirect="~/Error">
    <error statusCode="404" redirect="~/NotFound"/>
  </customErrors>
</system.web>
```

In this example:
- `mode="On"`: Specifies that custom error pages should be displayed for all errors.
- `defaultRedirect="~/Error"`: Redirects to the "Error" view for unhandled exceptions.
- `<error statusCode="404" redirect="~/NotFound"/>`: Specifies a custom page for 404 errors.

### 2. Using `HandleErrorAttribute`:
The `HandleErrorAttribute` is an attribute that you can apply at the controller or action level to handle exceptions.

```csharp
[HandleError(View = "Error")]
public class HomeController : Controller
{
    public ActionResult Index()
    {
        throw new Exception("An error occurred!");
    }
}
```

In this example:
- `HandleErrorAttribute` is applied to the `HomeController` to handle exceptions.
- The `View` property specifies the name of the view to display when an exception occurs.

### 3. Overriding `Controller.OnException` Method:
You can override the `OnException` method in your controller to handle exceptions globally or for specific controllers.

```csharp
protected override void OnException(ExceptionContext filterContext)
{
    filterContext.ExceptionHandled = true;
    ViewBag.ErrorMessage = filterContext.Exception.Message;
    filterContext.Result = new ViewResult
    {
        ViewName = "Error"
    };
}
```

In this example:
- The `OnException` method is overridden in the controller.
- `ExceptionHandled` is set to true to indicate that the exception has been handled.
- The `ViewBag` is used to pass error information to the view.
- The `Result` property is set to a `ViewResult` to display the "Error" view.

### 4. Using `Application_Error` Event of `HttpApplication`:
You can handle exceptions globally by subscribing to the `Application_Error` event in the Global.asax file.

```csharp
protected void Application_Error()
{
    Exception exception = Server.GetLastError();
    // Log the exception
    // Redirect or display a custom error page
    Response.Redirect("~/Error");
}
```

In this example:
- The `Application_Error` event is used to handle unhandled exceptions.
- The `Server.GetLastError()` method retrieves the last error that occurred.
- You can log the exception and redirect the user to a custom error page.

These are some of the common approaches to handle exceptions in ASP.NET MVC applications. Choose the one that best fits your application's requirements and error handling strategy.
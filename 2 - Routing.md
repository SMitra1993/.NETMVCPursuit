# Routing ❤🚀

**Links:**

- [ASP.NET MVC Routing](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/2%20-%20Routing.md#aspnet-mvc-routing-)

## **ASP.NET MVC Routing:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/2%20-%20Routing.md#routing-)

ASP.NET MVC Routing is a feature that enables the mapping of incoming HTTP requests to particular controller actions based on URL patterns. This is crucial for defining how URL paths correspond to the logic in your application. 

### Key Concepts in ASP.NET MVC Routing:

1. **Route Table**: A collection of routes defined for the application.
2. **Route**: An entry in the route table that defines URL pattern and associated information.
3. **Route Constraints**: Restrictions on the values a URL segment can contain.
4. **Route Defaults**: Default values for route parameters if they are not supplied in the URL.
5. **Route Data**: The values extracted from the URL based on the route definitions.

### Configuration File Example (Global.asax.cs):

Routes are typically configured in the `Global.asax.cs` file within the `Application_Start` method. Here's how you can set it up:

```csharp
using System.Web.Mvc;
using System.Web.Routing;

public class MvcApplication : System.Web.HttpApplication
{
    protected void Application_Start()
    {
        AreaRegistration.RegisterAllAreas();
        RegisterRoutes(RouteTable.Routes);
    }

    public static void RegisterRoutes(RouteCollection routes)
    {
        routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
        );

        routes.MapRoute(
            name: "Product",
            url: "Product/{action}/{id}",
            defaults: new { controller = "Product", action = "List", id = UrlParameter.Optional }
        );

        routes.MapRoute(
            name: "Custom",
            url: "Custom/{category}/{id}",
            defaults: new { controller = "Custom", action = "Index", id = UrlParameter.Optional },
            constraints: new { id = @"\d+" } // id must be numeric
        );
    }
}
```

### Code Explanation:

1. **`routes.IgnoreRoute`**: Ignores requests for certain paths, usually for resources like images, scripts, etc.

2. **`routes.MapRoute`**: Adds a new route to the route table.
   - **name**: The name of the route.
   - **url**: The URL pattern the route will match.
   - **defaults**: Default values if URL parameters are not provided.
   - **constraints**: Optional constraints for URL parameters.

### Detailed Example:

#### Controller and Actions:

Here's an example of how controller actions correspond to routes.

**HomeController.cs**:
```csharp
using System.Web.Mvc;

public class HomeController : Controller
{
    public ActionResult Index()
    {
        return View();
    }

    public ActionResult About()
    {
        return View();
    }
}
```

**ProductController.cs**:
```csharp
using System.Web.Mvc;

public class ProductController : Controller
{
    public ActionResult List()
    {
        // Fetch list of products
        return View();
    }

    public ActionResult Details(int id)
    {
        // Fetch product details by id
        return View();
    }
}
```

**CustomController.cs**:
```csharp
using System.Web.Mvc;

public class CustomController : Controller
{
    public ActionResult Index(string category, int id)
    {
        // Fetch data based on category and id
        return View();
    }
}
```

#### View Files:

These view files (`.cshtml` or `.aspx` depending on your view engine) should be placed in the corresponding folders in the `Views` directory:

- `Views/Home/Index.cshtml`
- `Views/Home/About.cshtml`
- `Views/Product/List.cshtml`
- `Views/Product/Details.cshtml`
- `Views/Custom/Index.cshtml`

#### URL to Action Mapping:

- **Default Route**: 
  - URL: `http://localhost/Home/Index` 
  - Maps to `HomeController.Index()`
  
- **Product Route**: 
  - URL: `http://localhost/Product/List` 
  - Maps to `ProductController.List()`
  
- **Custom Route**: 
  - URL: `http://localhost/Custom/electronics/5` 
  - Maps to `CustomController.Index(category: "electronics", id: 5)`

### Advanced Routing Configuration in `RouteConfig.cs`:

For more advanced scenarios, you can place the route configurations in a separate file like `RouteConfig.cs`.

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

        routes.MapRoute(
            name: "Product",
            url: "Product/{action}/{id}",
            defaults: new { controller = "Product", action = "List", id = UrlParameter.Optional }
        );

        routes.MapRoute(
            name: "Custom",
            url: "Custom/{category}/{id}",
            defaults: new { controller = "Custom", action = "Index", id = UrlParameter.Optional },
            constraints: new { id = @"\d+" } // id must be numeric
        );
    }
}
```

**Global.asax.cs**:
```csharp
using System.Web.Mvc;
using System.Web.Routing;

public class MvcApplication : System.Web.HttpApplication
{
    protected void Application_Start()
    {
        AreaRegistration.RegisterAllAreas();
        RouteConfig.RegisterRoutes(RouteTable.Routes);
    }
}
```

### Summary

Routing in ASP.NET MVC is a powerful feature that maps incoming requests to controller actions. By defining routes in `Global.asax.cs` or a separate `RouteConfig.cs` file, you can control how URLs are matched to your controllers and actions. This allows for clean, readable URLs and a more organized code structure.

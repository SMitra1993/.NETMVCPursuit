# Bundling ❤🚀

**Links:**

- [Script Bundle](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/10%20-%20Bundling.md#script-bundle-)
- [Stye Bundle](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/10%20-%20Bundling.md#stye-bundle-)

## **Script Bundle:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/10%20-%20Bundling.md#bundling-)

In ASP.NET MVC, bundling and minification improve the performance of your web application by reducing the number of HTTP requests and the size of requested resources. The `ScriptBundle` class is part of this feature and is used to bundle multiple JavaScript files into a single file. This reduces the number of HTTP requests required to load a page, resulting in faster page load times.

### Benefits of Using ScriptBundle

1. **Reduced HTTP Requests**: Combines multiple script files into a single bundle, which reduces the number of HTTP requests.
2. **Minification**: Removes unnecessary characters (like spaces and comments) from script files to reduce their size.
3. **Caching**: Generates a versioned URL for each bundle, allowing for better caching and automatic invalidation when the content changes.

### How to Use ScriptBundle

#### Step-by-Step Example

1. **Install the necessary package**: Make sure you have the `Microsoft.AspNet.Web.Optimization` package installed. You can install it via NuGet Package Manager:

   ```bash
   Install-Package Microsoft.AspNet.Web.Optimization
   ```

2. **Create BundleConfig Class**: Define your bundles in a `BundleConfig` class.

   ```csharp
   using System.Web;
   using System.Web.Optimization;

   public class BundleConfig
   {
       public static void RegisterBundles(BundleCollection bundles)
       {
           // Creating a ScriptBundle for your JavaScript files
           bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                       "~/Scripts/jquery-{version}.js"));

           bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
                       "~/Scripts/bootstrap.js"));

           bundles.Add(new ScriptBundle("~/bundles/custom").Include(
                       "~/Scripts/custom.js",
                       "~/Scripts/another-custom-script.js"));

           // Enable bundling and minification
           BundleTable.EnableOptimizations = true;
       }
   }
   ```

   In this example:
   - The `ScriptBundle` for jQuery will include all files matching the pattern `jquery-{version}.js`.
   - The `ScriptBundle` for Bootstrap will include `bootstrap.js`.
   - A custom `ScriptBundle` includes `custom.js` and `another-custom-script.js`.

3. **Register Bundles in Global.asax**: Call the `RegisterBundles` method during application start.

   ```csharp
   protected void Application_Start()
   {
       AreaRegistration.RegisterAllAreas();
       FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
       RouteConfig.RegisterRoutes(RouteTable.Routes);
       BundleConfig.RegisterBundles(BundleTable.Bundles);
   }
   ```

4. **Include Bundles in Views**: Use the `Scripts.Render` method to include the bundles in your views.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My ASP.NET MVC Application</title>
       @Scripts.Render("~/bundles/jquery")
       @Scripts.Render("~/bundles/bootstrap")
       @Scripts.Render("~/bundles/custom")
   </head>
   <body>
       <!-- Your content here -->
   </body>
   </html>
   ```

#### Example with Custom JavaScript Files

Assume you have two custom JavaScript files: `custom1.js` and `custom2.js`, located in the `Scripts` directory.

1. **Scripts/custom1.js**:

   ```javascript
   // custom1.js
   function customFunction1() {
       alert('Custom Function 1 executed');
   }
   ```

2. **Scripts/custom2.js**:

   ```javascript
   // custom2.js
   function customFunction2() {
       alert('Custom Function 2 executed');
   }
   ```

3. **Update BundleConfig**:

   ```csharp
   public class BundleConfig
   {
       public static void RegisterBundles(BundleCollection bundles)
       {
           // Existing bundles...

           // Add a new ScriptBundle for custom scripts
           bundles.Add(new ScriptBundle("~/bundles/customscripts").Include(
                       "~/Scripts/custom1.js",
                       "~/Scripts/custom2.js"));

           BundleTable.EnableOptimizations = true;
       }
   }
   ```

4. **Include the Custom Bundle in Your View**:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Custom Scripts Example</title>
       @Scripts.Render("~/bundles/customscripts")
   </head>
   <body>
       <button onclick="customFunction1()">Run Custom Function 1</button>
       <button onclick="customFunction2()">Run Custom Function 2</button>
   </body>
   </html>
   ```

### Summary

Using `ScriptBundle` in ASP.NET MVC helps improve web application performance by combining multiple JavaScript files into a single file and applying minification to reduce file size. This reduces the number of HTTP requests and speeds up page load times. The process involves defining the bundles in a `BundleConfig` class, registering them in `Global.asax`, and including them in your views using `Scripts.Render`. This approach not only optimizes the loading of scripts but also simplifies script management in your application.

## **Stye Bundle:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/10%20-%20Bundling.md#bundling-)

In ASP.NET MVC, the `StyleBundle` class is used to bundle and minify CSS (Cascading Style Sheets) files. This improves the performance of web applications by reducing the number of HTTP requests and the size of the requested resources. The `StyleBundle` class is part of the bundling and minification feature provided by the `Microsoft.AspNet.Web.Optimization` package.

### Benefits of Using StyleBundle

1. **Reduced HTTP Requests**: Combines multiple CSS files into a single bundle, which reduces the number of HTTP requests needed to load a page.
2. **Minification**: Removes unnecessary characters (like spaces, comments, and new lines) from CSS files to reduce their size.
3. **Caching**: Generates a versioned URL for each bundle, allowing for better caching and automatic invalidation when the content changes.

### How to Use StyleBundle

#### Step-by-Step Example

1. **Install the Necessary Package**: Ensure you have the `Microsoft.AspNet.Web.Optimization` package installed. You can install it via NuGet Package Manager:

   ```bash
   Install-Package Microsoft.AspNet.Web.Optimization
   ```

2. **Create BundleConfig Class**: Define your bundles in a `BundleConfig` class.

   ```csharp
   using System.Web.Optimization;

   public class BundleConfig
   {
       public static void RegisterBundles(BundleCollection bundles)
       {
           // Creating a StyleBundle for your CSS files
           bundles.Add(new StyleBundle("~/Content/css").Include(
                       "~/Content/bootstrap.css",
                       "~/Content/site.css"));

           // Enable bundling and minification
           BundleTable.EnableOptimizations = true;
       }
   }
   ```

   In this example:
   - The `StyleBundle` for CSS files will include `bootstrap.css` and `site.css`.

3. **Register Bundles in Global.asax**: Call the `RegisterBundles` method during application start.

   ```csharp
   protected void Application_Start()
   {
       AreaRegistration.RegisterAllAreas();
       FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
       RouteConfig.RegisterRoutes(RouteTable.Routes);
       BundleConfig.RegisterBundles(BundleTable.Bundles);
   }
   ```

4. **Include Bundles in Views**: Use the `Styles.Render` method to include the bundles in your views.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My ASP.NET MVC Application</title>
       @Styles.Render("~/Content/css")
   </head>
   <body>
       <!-- Your content here -->
   </body>
   </html>
   ```

#### Example with Custom CSS Files

Assume you have two custom CSS files: `custom1.css` and `custom2.css`, located in the `Content` directory.

1. **Content/custom1.css**:

   ```css
   /* custom1.css */
   body {
       background-color: #f0f0f0;
   }
   ```

2. **Content/custom2.css**:

   ```css
   /* custom2.css */
   h1 {
       color: blue;
   }
   ```

3. **Update BundleConfig**:

   ```csharp
   public class BundleConfig
   {
       public static void RegisterBundles(BundleCollection bundles)
       {
           // Existing bundles...

           // Add a new StyleBundle for custom styles
           bundles.Add(new StyleBundle("~/Content/customstyles").Include(
                       "~/Content/custom1.css",
                       "~/Content/custom2.css"));

           BundleTable.EnableOptimizations = true;
       }
   }
   ```

4. **Include the Custom Bundle in Your View**:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Custom Styles Example</title>
       @Styles.Render("~/Content/customstyles")
   </head>
   <body>
       <h1>Welcome to My Custom Styles Example</h1>
   </body>
   </html>
   ```

### Summary

Using `StyleBundle` in ASP.NET MVC helps improve web application performance by combining multiple CSS files into a single bundle and applying minification to reduce file size. This reduces the number of HTTP requests and speeds up page load times. The process involves defining the bundles in a `BundleConfig` class, registering them in `Global.asax`, and including them in your views using `Styles.Render`. This approach not only optimizes the loading of styles but also simplifies style management in your application.
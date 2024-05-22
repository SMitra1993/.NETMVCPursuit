# View ❤🚀

## **MVC View:**

In ASP.NET MVC, the View is responsible for rendering the user interface. It uses data provided by the Controller and presents it to the user. The View in MVC is typically an HTML file with embedded Razor markup, which allows you to create dynamic web pages using C# code.

### Key Features of the View in ASP.NET MVC
- **Separation of Concerns:** The View focuses on how data is displayed to the user, not how it is processed.
- **Razor Syntax:** Views use Razor syntax to embed C# code within HTML.
- **Model Binding:** Views can be strongly typed, meaning they are associated with a specific data model.
- **Templated Helpers:** ASP.NET MVC provides various HTML helpers to simplify form creation and data binding.

### Example Scenario
Let's consider a simple scenario where we have an application that displays a list of students and allows users to add new students. We will focus on creating the Views for listing and creating students.

### Step-by-Step Implementation

1. **Create the Student Model**

First, ensure you have a `Student` model similar to this:

```csharp
// Models/Student.cs
using System.ComponentModel.DataAnnotations;

namespace YourNamespace.Models
{
    public class Student
    {
        public int Id { get; set; }

        [Required]
        [StringLength(50)]
        public string FirstName { get; set; }

        [Required]
        [StringLength(50)]
        public string LastName { get; set; }

        [Range(1, 100)]
        public int Age { get; set; }
    }
}
```

2. **Create the Student Controller**

Create a controller to handle the logic for listing and creating students.

```csharp
// Controllers/StudentController.cs
using System.Linq;
using System.Web.Mvc;
using YourNamespace.Models;

namespace YourNamespace.Controllers
{
    public class StudentController : Controller
    {
        private ApplicationDbContext db = new ApplicationDbContext();

        public ActionResult Index()
        {
            var students = db.Students.ToList();
            return View(students);
        }

        public ActionResult Create()
        {
            return View();
        }

        [HttpPost]
        public ActionResult Create(Student student)
        {
            if (ModelState.IsValid)
            {
                db.Students.Add(student);
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(student);
        }
    }
}
```

3. **Create the Index View**

Create a view to display the list of students.

```html
<!-- Views/Student/Index.cshtml -->
@model IEnumerable<YourNamespace.Models.Student>

@{
    ViewBag.Title = "Students";
}

<h2>Students</h2>

<table class="table">
    <tr>
        <th>First Name</th>
        <th>Last Name</th>
        <th>Age</th>
    </tr>

    @foreach (var student in Model)
    {
        <tr>
            <td>@student.FirstName</td>
            <td>@student.LastName</td>
            <td>@student.Age</td>
        </tr>
    }
</table>

<p>
    @Html.ActionLink("Create New", "Create", null, new { @class = "btn btn-primary" })
</p>
```

In this view:
- The model is an `IEnumerable<Student>` representing the list of students.
- A table is used to display the list of students.
- An `ActionLink` is provided to navigate to the `Create` view.

4. **Create the Create View**

Create a view to add a new student.

```html
<!-- Views/Student/Create.cshtml -->
@model YourNamespace.Models.Student

@{
    ViewBag.Title = "Create Student";
}

<h2>Create Student</h2>

@using (Html.BeginForm())
{
    @Html.ValidationSummary(true, "", new { @class = "text-danger" })

    <div class="form-group">
        @Html.LabelFor(model => model.FirstName)
        @Html.TextBoxFor(model => model.FirstName, new { @class = "form-control" })
        @Html.ValidationMessageFor(model => model.FirstName, "", new { @class = "text-danger" })
    </div>

    <div class="form-group">
        @Html.LabelFor(model => model.LastName)
        @Html.TextBoxFor(model => model.LastName, new { @class = "form-control" })
        @Html.ValidationMessageFor(model => model.LastName, "", new { @class = "text-danger" })
    </div>

    <div class="form-group">
        @Html.LabelFor(model => model.Age)
        @Html.TextBoxFor(model => model.Age, new { @class = "form-control" })
        @Html.ValidationMessageFor(model => model.Age, "", new { @class = "text-danger" })
    </div>

    <button type="submit" class="btn btn-primary">Create</button>
}

<p>
    @Html.ActionLink("Back to List", "Index", null, new { @class = "btn btn-secondary" })
</p>
```

In this view:
- The model is a `Student`.
- A form is created using `Html.BeginForm()` to post the data back to the server.
- Form fields are generated using `Html.TextBoxFor` and validation messages using `Html.ValidationMessageFor`.

### Summary

In this example, we created Views for listing and creating students in an ASP.NET MVC application. We:
- Created a `Student` model with validation attributes.
- Created a controller to handle the logic for listing and creating students.
- Created an `Index` view to display the list of students.
- Created a `Create` view to add new students.

This example demonstrates how to use Views in ASP.NET MVC to present data to the user and collect user input. By separating the presentation logic into Views, you maintain a clean separation of concerns and make your application more maintainable and testable.

## **Razor View Engine:**

The Razor View Engine is a key component of ASP.NET MVC that provides a streamlined syntax for combining HTML markup with server-side code, such as C# or VB.NET. It was introduced as part of ASP.NET MVC 3 to offer a more efficient and readable way to write dynamic web pages compared to its predecessor, the ASPX view engine.

### Key Features of Razor View Engine

1. **Clean Syntax:**
   - Razor uses the `@` symbol to transition from HTML to C#.
   - This results in less cluttered code compared to the <% %> syntax used in ASPX view engine.

2. **Intellisense Support:**
   - Razor views benefit from full IntelliSense support in Visual Studio, providing auto-completion, syntax highlighting, and debugging capabilities.

3. **Server-Side Logic Integration:**
   - Razor allows for the seamless integration of server-side logic within HTML, making it easier to bind data to UI elements.

4. **Code Blocks and Expressions:**
   - Razor differentiates between code blocks (e.g., `@{ ... }`) and inline expressions (e.g., `@Model.Name`), making the syntax intuitive and easy to follow.

5. **HTML Encoding:**
   - Razor automatically HTML encodes output, which helps prevent Cross-Site Scripting (XSS) attacks.

### Razor Syntax Basics

- **Inline Expressions:**
  ```html
  <p>Hello, @Model.Name!</p>
  ```
  This renders the `Name` property of the `Model`.

- **Code Blocks:**
  ```html
  @{
      var greeting = "Hello, World!";
  }
  <p>@greeting</p>
  ```
  Code blocks allow for more complex logic within the view.

- **Control Structures:**
  ```html
  @if (Model.IsAdmin)
  {
      <p>Welcome, Admin!</p>
  }
  else
  {
      <p>Welcome, User!</p>
  }

  @foreach (var item in Model.Items)
  {
      <li>@item.Name</li>
  }
  ```

### Example Scenario

Consider a scenario where you want to display a list of products on a webpage. Each product has a name, price, and description. We'll use Razor to create a clean, dynamic view.

1. **Create the Product Model**

```csharp
// Models/Product.cs
namespace YourNamespace.Models
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
        public string Description { get; set; }
    }
}
```

2. **Create the Product Controller**

```csharp
// Controllers/ProductController.cs
using System.Collections.Generic;
using System.Web.Mvc;
using YourNamespace.Models;

namespace YourNamespace.Controllers
{
    public class ProductController : Controller
    {
        public ActionResult Index()
        {
            var products = new List<Product>
            {
                new Product { Id = 1, Name = "Laptop", Price = 999.99m, Description = "A high-performance laptop." },
                new Product { Id = 2, Name = "Smartphone", Price = 499.99m, Description = "A latest-gen smartphone." },
                new Product { Id = 3, Name = "Tablet", Price = 299.99m, Description = "A versatile tablet." }
            };

            return View(products);
        }
    }
}
```

3. **Create the Razor View**

```html
<!-- Views/Product/Index.cshtml -->
@model IEnumerable<YourNamespace.Models.Product>

@{
    ViewBag.Title = "Product List";
}

<h2>Product List</h2>

<table class="table">
    <thead>
        <tr>
            <th>Name</th>
            <th>Price</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var product in Model)
        {
            <tr>
                <td>@product.Name</td>
                <td>@product.Price.ToString("C")</td>
                <td>@product.Description</td>
            </tr>
        }
    </tbody>
</table>
```

### Explanation of the Example

- **Model Declaration:**
  - The `@model IEnumerable<YourNamespace.Models.Product>` directive specifies the type of data the view will use.

- **HTML Markup with Razor:**
  - Razor syntax is used to loop through the list of products and generate table rows for each product.
  - The `@` symbol precedes C# code within the HTML. For instance, `@product.Name` embeds the product's name in the table cell.
  - The `ToString("C")` method is used to format the price as currency.

### Advantages of Razor View Engine

- **Compact Syntax:**
  - Razor's concise syntax reduces the amount of code needed, making views easier to read and maintain.

- **Seamless Integration:**
  - The `@` symbol simplifies embedding server-side code within HTML, allowing for more natural and fluid coding.

- **Automatic HTML Encoding:**
  - By default, Razor encodes output to prevent XSS attacks, improving security.

- **Tooling Support:**
  - Full IntelliSense and debugging support in Visual Studio enhances developer productivity.

### Conclusion

The Razor View Engine is a powerful and efficient tool for creating dynamic web pages in ASP.NET MVC. It combines the clarity of HTML with the power of C# to produce a streamlined and secure development experience. The example provided demonstrates how Razor can be used to build a simple yet dynamic product listing page, highlighting its ease of use and flexibility.

## **Layout View:**

In ASP.NET MVC, Layout View is a feature that allows you to define a common layout structure for multiple views within your application. This layout typically includes elements such as headers, footers, navigation menus, and other common UI components. Using a layout view helps maintain a consistent look and feel across your application and simplifies the process of updating the overall design.

Here's an example to illustrate how Layout View works in ASP.NET MVC:

1. **Create a Layout View**: First, you create a layout view file (typically named "_Layout.cshtml") in the "Views/Shared" directory of your MVC project. This file will define the common layout structure for your application.

```html
<!-- _Layout.cshtml -->
<!DOCTYPE html>
<html>
<head>
    <title>@ViewBag.Title - My Application</title>
    <!-- CSS and other common resources -->
</head>
<body>
    <div id="header">
        <!-- Header content, such as logo, navigation menu -->
        @Html.Partial("_Header")
    </div>

    <div id="main-content">
        <!-- This is where individual views will be inserted -->
        @RenderBody()
    </div>

    <div id="footer">
        <!-- Footer content -->
        @Html.Partial("_Footer")
    </div>
    <!-- JavaScript and other common resources -->
</body>
</html>
```

2. **Create Partial Views**: You can also create partial views for reusable components like headers, footers, or any other section of your layout that you want to separate out for better maintainability. These partial views can be stored in the "Views/Shared" directory as well.

```html
<!-- _Header.cshtml -->
<header>
    <h1>Welcome to My Application</h1>
    <!-- Navigation menu -->
    <nav>
        <ul>
            <li>@Html.ActionLink("Home", "Index", "Home")</li>
            <li>@Html.ActionLink("About", "About", "Home")</li>
            <!-- Other navigation links -->
        </ul>
    </nav>
</header>
```

```html
<!-- _Footer.cshtml -->
<footer>
    <p>&copy; 2024 My Application</p>
</footer>
```

3. **Use the Layout in Views**: In your individual views (e.g., "Index.cshtml", "About.cshtml", etc.), you specify that they should use the layout view you created by setting the Layout property.

```html
<!-- Index.cshtml -->
@{
    ViewBag.Title = "Home";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<!-- Page content -->
<h2>Welcome to our homepage!</h2>
<p>This is the home page content.</p>
```

4. **Render Content**: Within each individual view, you only define the specific content unique to that page. The layout view takes care of rendering the common elements like headers, footers, etc.

By using Layout Views in ASP.NET MVC, you can ensure consistency across your application's UI while also making it easier to maintain and update common layout elements.

## **Partial Views:**

Partial Views in ASP.NET MVC allow you to create reusable components that can be shared across multiple views within your application. They are similar to regular views but are intended to represent smaller, self-contained parts of a web page, such as a sidebar, a login form, or a product listing.

Here's how Partial Views work in ASP.NET MVC:

1. **Create a Partial View**: Just like regular views, Partial Views are typically stored in the "Views" folder of your MVC project. However, unlike regular views, Partial Views are often stored in a subfolder named "Shared" to indicate that they are intended for reuse.

For example, let's create a Partial View for a simple login form:

```html
<!-- _LoginPartial.cshtml -->
@using (Html.BeginForm("Login", "Account", FormMethod.Post))
{
    <input type="text" name="username" placeholder="Username" />
    <input type="password" name="password" placeholder="Password" />
    <button type="submit">Login</button>
}
```

2. **Render the Partial View in a View**: To use a Partial View within another view, you can use the `Html.Partial` helper method. This method renders the specified Partial View inline within the parent view.

For example, let's render the `_LoginPartial.cshtml` Partial View within the `_Layout.cshtml` layout view:

```html
<!-- _Layout.cshtml -->
<!DOCTYPE html>
<html>
<head>
    <title>@ViewBag.Title - My Application</title>
    <!-- CSS and other common resources -->
</head>
<body>
    <div id="header">
        <!-- Header content -->
        <h1>Welcome to My Application</h1>
        <!-- Render the login partial view -->
        @Html.Partial("_LoginPartial")
    </div>

    <div id="main-content">
        <!-- This is where individual views will be inserted -->
        @RenderBody()
    </div>

    <div id="footer">
        <!-- Footer content -->
        <p>&copy; 2024 My Application</p>
    </div>
    <!-- JavaScript and other common resources -->
</body>
</html>
```

3. **Passing Data to Partial Views**: Partial Views can also accept data from their parent views. This can be achieved by passing a model or other parameters when rendering the Partial View using the `Html.Partial` method.

For example, if your Partial View requires some data to render, you can pass it like this:

```html
<!-- Parent View -->
@{
    var productList = GetProductList(); // Assuming you have a method to retrieve product data
}

<!-- Render the partial view and pass the product list -->
@Html.Partial("_ProductListPartial", productList)
```

```html
<!-- _ProductListPartial.cshtml -->
@model List<Product>

<ul>
    @foreach (var product in Model)
    {
        <li>@product.Name - @product.Price</li>
    }
</ul>
```

By using Partial Views in ASP.NET MVC, you can modularize your application's UI components, promoting code reuse and making your views more maintainable. They are particularly useful for creating reusable widgets, forms, or any other components that appear on multiple pages throughout your application.

## **Layout View Vs Partial View:**

Here's a detailed comparison between Layout Views and Partial Views in tabular form:

| Feature            | Layout Views                                                  | Partial Views                                               |
|--------------------|---------------------------------------------------------------|-------------------------------------------------------------|
| Purpose            | Defines the overall structure and common elements of a page.  | Represents smaller, reusable components of a page.          |
| Location           | Typically stored in the "Views/Shared" directory.            | Also stored in the "Views" directory, often in "Shared".    |
| Rendering          | Used to render the overall layout of a page.                 | Used to render specific components within a page.          |
| Usage              | Applied to entire pages to provide a consistent layout.      | Embedded within other views to provide specific features.  |
| Content            | Contains the main structure of the page (e.g., headers, footers). | Contains smaller, self-contained components (e.g., forms, widgets). |
| Inheritance        | Can be nested within other Layout Views.                     | Can be nested within Layout Views or other Partial Views.  |
| Model Binding      | Typically does not directly bind to models.                  | Can bind to models passed from parent views.              |
| Passing Data       | Usually, data is not passed directly to Layout Views.        | Can accept data from parent views.                         |
| Extensibility      | Can be extended or overridden by individual views.           | Can be extended or overridden by other Partial Views.      |
| Example            | Defining the structure of a website's header, navigation, and footer. | Creating a reusable login form or product listing component. |

Both Layout Views and Partial Views are essential components of ASP.NET MVC applications. Layout Views provide a consistent structure for your application's pages, while Partial Views enable the creation of reusable components that can be shared across multiple views. Understanding when and how to use each type of view is key to building maintainable and modular MVC applications.

## **ViewBag Vs ViewData Vs ViewData:**

Certainly! Here's a comparison between `ViewBag`, `ViewData`, and `TempData` in tabular form, along with examples illustrating scenarios for each:

| Feature          | ViewBag                                                | ViewData                                                  | TempData                                               |
|------------------|--------------------------------------------------------|-----------------------------------------------------------|--------------------------------------------------------|
| Purpose          | Dynamic object to pass data from controllers to views. | Dictionary-like container to pass data from controllers to views. | Holds data across requests and redirects within the same session. |
| Data Storage     | Stored as dynamic properties accessible by string keys. | Stored as key-value pairs within the ViewData dictionary.   | Similar to ViewData but intended for temporary storage during redirects. |
| Type Safety      | Not type-safe; data can be of any type.                | Not type-safe; data can be of any type.                  | Not type-safe; data can be of any type.               |
| Null Handling    | May throw a runtime error if accessed with a non-existent key or null value. | Can handle null values more gracefully than ViewBag.       | Can handle null values more gracefully than ViewBag.   |
| Performance      | Slightly faster than ViewData due to dynamic access.    | Slightly slower than ViewBag due to dictionary access.    | Slightly slower than ViewBag due to dictionary access. |
| Example Scenario | Passing a message to the view for display purposes.      | Passing a complex object or multiple values to the view.  | Storing form data during a redirect after a failed form submission. |

Now, let's illustrate each scenario with an example:

### Example Scenario 1: Using ViewBag
In this scenario, you want to pass a simple message from the controller to the view for display purposes.

**Controller:**
```csharp
public class HomeController : Controller
{
    public ActionResult Index()
    {
        ViewBag.Message = "Welcome to my website!";
        return View();
    }
}
```

**View:**
```html
@{
    ViewBag.Title = "Home";
}

<h2>@ViewBag.Message</h2>
```

### Example Scenario 2: Using ViewData
In this scenario, you want to pass a complex object or multiple values from the controller to the view.

**Controller:**
```csharp
public class ProductController : Controller
{
    public ActionResult Details(int id)
    {
        // Get product details from database
        Product product = _productService.GetProductById(id);

        // Pass product details to the view using ViewData
        ViewData["Product"] = product;

        return View();
    }
}
```

**View:**
```html
@model Product

@{
    ViewData["Title"] = "Product Details";
}

<h2>@ViewData["Title"]</h2>
<p>Name: @Model.Name</p>
<p>Price: @Model.Price</p>
<!-- Other product details -->
```

### Example Scenario 3: Using TempData
In this scenario, you want to store form data during a redirect after a failed form submission.

**Controller (Post Action):**
```csharp
[HttpPost]
public ActionResult Create(ProductViewModel model)
{
    if (!ModelState.IsValid)
    {
        // Store model data in TempData for the next request
        TempData["FormData"] = model;
        return RedirectToAction("Error");
    }

    // Process valid form submission
    return RedirectToAction("Index");
}
```

**Controller (Error Action):**
```csharp
public ActionResult Error()
{
    // Retrieve stored form data from TempData
    ProductViewModel model = TempData["FormData"] as ProductViewModel;
    return View(model);
}
```

**Error View:**
```html
@model ProductViewModel

@{
    ViewBag.Title = "Error";
}

<h2>Error</h2>
@if (Model != null)
{
    <p>Please correct the following errors:</p>
    <!-- Display form errors -->
}
```

In each scenario, the choice between `ViewBag`, `ViewData`, and `TempData` depends on the specific requirements of the application, such as the type of data being passed, its lifespan, and whether it needs to persist across redirects.

## **HTML Helpers:**

Sure, let's delve a bit deeper into each HTML Helper:

1. **TextBox**: The `TextBox` helper generates an HTML input element of type text. It typically takes the name of the property as its first parameter and optionally accepts additional parameters like HTML attributes.
   ```html
   @Html.TextBox("Name")
   ```

2. **TextArea**: The `TextArea` helper generates an HTML textarea element for multi-line text input. It's useful when you need to accept longer text inputs, such as comments or descriptions.
   ```html
   @Html.TextArea("Description")
   ```

3. **CheckBox**: The `CheckBox` helper generates an HTML input element of type checkbox. It's used when you want to present a binary choice, such as enabling or disabling a feature.
   ```html
   @Html.CheckBox("IsEnabled")
   ```

4. **RadioButton**: The `RadioButton` helper generates an HTML input element of type radio. It's used when you have multiple options, but only one can be selected. You typically use multiple `RadioButton` helpers with the same name to create a radio button group.
   ```html
   @Html.RadioButton("Gender", "Male")
   @Html.RadioButton("Gender", "Female")
   ```

5. **DropDownList**: The `DropDownList` helper generates an HTML select element for a dropdown list. It's used when you have a list of options and the user can select one.
   ```html
   @Html.DropDownList("Country", new SelectList(Model.Countries))
   ```

6. **HiddenField**: The `Hidden` helper generates an HTML input element of type hidden. It's used to store data that shouldn't be visible to the user but needs to be sent to the server.
   ```html
   @Html.Hidden("Id")
   ```

7. **Password**: The `Password` helper generates an HTML input element of type password. It's similar to `TextBox` but hides the characters entered by the user, typically used for sensitive information like passwords.
   ```html
   @Html.Password("Password")
   ```

8. **Display**: The `Display` helper generates a display-only HTML element for a model property. It's used to render the value of a property in read-only mode.
   ```html
   @Html.Display("Name")
   ```

9. **Label**: The `Label` helper generates an HTML label element for a form element. It's used to associate a label with an input element, improving accessibility and user experience.
   ```html
   @Html.Label("Name", "Full Name:")
   ```

10. **Editor**: The `Editor` helper generates an HTML textarea element with rich text editing capabilities. It's used when you need to allow users to enter formatted text, such as in a content editing scenario.
   ```html
   @Html.Editor("Content")
   ```

Each HTML Helper provides a convenient way to generate HTML elements while leveraging the features of ASP.NET MVC, such as model binding, validation, and localization. They help streamline the development process and ensure consistency in generating HTML markup across your application.
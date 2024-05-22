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

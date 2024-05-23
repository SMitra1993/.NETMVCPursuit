# Model ❤🚀

**Links:**

- [MVC Model](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/5%20-%20Model.md#mvc-model-)
- [Model Binding](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/5%20-%20Model.md#model-binding-)

## **MVC Model:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/5%20-%20Model.md#model-)

The Model in ASP.NET MVC represents the application's data and business logic. It is the part of the MVC architecture that is responsible for retrieving data from the database, performing operations on it, and providing it to the View and Controller.

### Key Features of the Model in ASP.NET MVC
- **Encapsulation of Data:** Models encapsulate the data of the application.
- **Validation Logic:** Models often contain validation logic to ensure data integrity.
- **Business Logic:** Models can contain business logic that manipulates data.
- **Data Access:** Models interact with the database to perform CRUD (Create, Read, Update, Delete) operations.

### Example Scenario
Let's consider a simple scenario where we have an application that manages a list of students. We will create a Model to represent the student data, a Controller to handle the logic, and a View to display the data.

### Step-by-Step Implementation

1. **Create the Student Model**

First, create a new class in the `Models` folder to represent a student.

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

In this model:
- `Id` is the primary key.
- `FirstName` and `LastName` are required fields with a maximum length of 50 characters.
- `Age` is a required field with a range constraint between 1 and 100.

2. **Create the Database Context**

Create a class that inherits from `DbContext` to interact with the database.

```csharp
// Models/ApplicationDbContext.cs
using System.Data.Entity;

namespace YourNamespace.Models
{
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext() : base("DefaultConnection") { }

        public DbSet<Student> Students { get; set; }
    }
}
```

In this context:
- `DbSet<Student>` represents the collection of `Student` entities in the database.

3. **Create the Student Controller**

Create a controller to handle the logic for student operations.

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

In this controller:
- The `Index` action retrieves the list of students from the database and passes it to the view.
- The `Create` actions handle the creation of new students. The `[HttpPost]` attribute specifies that the second `Create` method handles POST requests.

4. **Create the Views**

Create views to display and create students.

**Index View**

Create a view to display the list of students.

```html
<!-- Views/Student/Index.cshtml -->
@model IEnumerable<YourNamespace.Models.Student>

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
    @Html.ActionLink("Create New", "Create")
</p>
```

In this view:
- The model is an `IEnumerable<Student>` representing the list of students.
- A table is used to display the list of students.
- An `ActionLink` is provided to navigate to the `Create` view.

**Create View**

Create a view to create a new student.

```html
<!-- Views/Student/Create.cshtml -->
@model YourNamespace.Models.Student

<h2>Create Student</h2>

@using (Html.BeginForm())
{
    @Html.ValidationSummary(true)

    <div class="form-group">
        @Html.LabelFor(model => model.FirstName)
        @Html.TextBoxFor(model => model.FirstName, new { @class = "form-control" })
        @Html.ValidationMessageFor(model => model.FirstName)
    </div>

    <div class="form-group">
        @Html.LabelFor(model => model.LastName)
        @Html.TextBoxFor(model => model.LastName, new { @class = "form-control" })
        @Html.ValidationMessageFor(model => model.LastName)
    </div>

    <div class="form-group">
        @Html.LabelFor(model => model.Age)
        @Html.TextBoxFor(model => model.Age, new { @class = "form-control" })
        @Html.ValidationMessageFor(model => model.Age)
    </div>

    <button type="submit" class="btn btn-primary">Create</button>
}

<p>
    @Html.ActionLink("Back to List", "Index")
</p>
```

In this view:
- The model is a `Student`.
- A form is created using `Html.BeginForm()` to post the data back to the server.
- Form fields are generated using `Html.TextBoxFor` and validation messages using `Html.ValidationMessageFor`.

### Summary

In this example, we created a simple ASP.NET MVC application to manage a list of students. We:
- Created a `Student` model with validation attributes.
- Created a database context to interact with the database.
- Created a controller to handle the logic for listing and creating students.
- Created views to display and create students.

This example demonstrates the core concepts of the Model in ASP.NET MVC, including data encapsulation, validation, business logic, and data access. By using these concepts, you can build robust and maintainable web applications.

## **Model Binding:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/5%20-%20Model.md#model-)

Model binding in ASP.NET MVC is a powerful feature that allows the framework to automatically map HTTP request data (such as form values, route data, and query string parameters) to the parameters of your action methods and to the properties of your model classes. This makes it easier to work with incoming data in a strongly-typed manner, avoiding the need to manually parse request data.

### How Model Binding Works

When an HTTP request is received, the MVC framework attempts to bind the request data to the parameters of the action method being called. This process involves:

1. **Gathering Input Data:**
   - The framework collects data from various parts of the HTTP request, such as route values, query string parameters, form data, and request headers.

2. **Identifying Targets:**
   - It identifies the target parameters and model properties that need to be populated with this data.

3. **Converting Data:**
   - It converts the gathered data into the appropriate types required by the parameters and model properties.

4. **Setting Values:**
   - The converted values are then set to the corresponding parameters and properties.

### Types of Model Binding

1. **Simple Types:**
   - Binding simple types like integers, strings, and booleans directly from route data, query strings, and form data.

2. **Complex Types:**
   - Binding complex types, which are typically instances of classes with properties. The framework recursively sets properties on these objects using the input data.

### Example Scenarios

#### Binding Simple Types

Suppose you have a controller action that takes two parameters:

```csharp
public class HomeController : Controller
{
    public ActionResult Index(int id, string name)
    {
        ViewBag.Id = id;
        ViewBag.Name = name;
        return View();
    }
}
```

If you navigate to `http://localhost/Home/Index/1?name=John`, the model binder will:

- Extract `1` from the route and bind it to the `id` parameter.
- Extract `John` from the query string and bind it to the `name` parameter.

#### Binding Complex Types

Consider a scenario where you have a form to create a new product:

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

public class ProductController : Controller
{
    [HttpPost]
    public ActionResult Create(Product product)
    {
        if (ModelState.IsValid)
        {
            // Save product to the database
            return RedirectToAction("Index");
        }
        return View(product);
    }
}
```

Your form might look like this:

```html
@model YourNamespace.Models.Product

@using (Html.BeginForm())
{
    <div>
        @Html.LabelFor(m => m.Name)
        @Html.TextBoxFor(m => m.Name)
    </div>
    <div>
        @Html.LabelFor(m => m.Price)
        @Html.TextBoxFor(m => m.Price)
    </div>
    <button type="submit">Create</button>
}
```

When the form is submitted, the model binder will:

- Read the form values.
- Create a new `Product` instance.
- Set the `Name` and `Price` properties of the `Product` instance based on the form values.
- Pass the `Product` instance to the `Create` action method.

### Custom Model Binding

In some cases, you might need more control over the model binding process. You can create custom model binders by implementing the `IModelBinder` interface.

#### Custom Model Binder Example

Suppose you have a `User` class and you want to bind it from a custom format in the query string.

```csharp
public class User
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

public class UserModelBinder : IModelBinder
{
    public object BindModel(ControllerContext controllerContext, ModelBindingContext bindingContext)
    {
        var request = controllerContext.HttpContext.Request;
        var firstName = request.QueryString["firstName"];
        var lastName = request.QueryString["lastName"];
        
        return new User
        {
            FirstName = firstName,
            LastName = lastName
        };
    }
}
```

You would then register this custom binder in `Global.asax`:

```csharp
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    RouteConfig.RegisterRoutes(RouteTable.Routes);

    ModelBinders.Binders.Add(typeof(User), new UserModelBinder());
}
```

### Model Binding Attributes

ASP.NET MVC provides several attributes to help control model binding:

- `[Bind]`: Specifies which properties should be included or excluded in model binding.
- `[FromBody]`, `[FromUri]`, `[FromRoute]`: Used to specify the source of data for action parameters (more common in ASP.NET Web API).
- `[ModelBinder]`: Specifies a custom model binder to use for a parameter or property.

#### Example Using `[Bind]`

```csharp
public class Product
{
    public int Id { get; set; }
    
    [Bind(Exclude = "Name")]
    public string Name { get; set; }
    
    public decimal Price { get; set; }
}

public class ProductController : Controller
{
    [HttpPost]
    public ActionResult Edit([Bind(Include = "Id,Price")] Product product)
    {
        if (ModelState.IsValid)
        {
            // Save changes to the database
            return RedirectToAction("Index");
        }
        return View(product);
    }
}
```

In this example, only the `Id` and `Price` properties will be bound, and `Name` will be excluded.

### Benefits of Model Binding

- **Simplifies Data Handling:** Reduces the need for manual parsing of request data.
- **Enhances Code Readability:** Promotes clean and maintainable code by separating data handling logic from business logic.
- **Supports Complex Types:** Facilitates the binding of complex types, which can include nested objects.
- **Customizable:** Allows for the creation of custom model binders to handle special scenarios.

### Conclusion

Model binding in ASP.NET MVC is a powerful mechanism that streamlines the process of handling incoming HTTP request data. By automatically mapping request data to action method parameters and model properties, it simplifies data processing and enhances code maintainability. Understanding how to leverage model binding, including the creation of custom model binders and the use of model binding attributes, is crucial for developing robust and efficient MVC applications.

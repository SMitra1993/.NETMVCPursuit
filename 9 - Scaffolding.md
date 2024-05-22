# Scaffloding

## **MVC Scaffolding:**

ASP.NET MVC scaffolding is a code generation framework that automates the process of creating basic CRUD (Create, Read, Update, Delete) operations for your application's data models. It generates the necessary code files, such as controllers, views, and data access code, based on your model classes. Scaffolding simplifies the development process by eliminating repetitive tasks and reducing the amount of boilerplate code you need to write.

Here's a detailed explanation of ASP.NET MVC scaffolding:

### 1. Model:
Scaffolding starts with your data model, which typically represents the structure of your application's data entities. You define your model classes using plain C# (or VB.NET) classes with properties that map to your database tables.

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

### 2. Controller:
Scaffolding generates controller classes that handle HTTP requests and implement CRUD operations for your model classes. The generated controller includes actions for listing, creating, editing, and deleting records.

```csharp
public class ProductsController : Controller
{
    private readonly ApplicationDbContext _context;

    public ProductsController(ApplicationDbContext context)
    {
        _context = context;
    }

    // GET: Products
    public async Task<IActionResult> Index()
    {
        return View(await _context.Products.ToListAsync());
    }

    // Other CRUD actions (Create, Edit, Delete) are also generated
}
```

### 3. Views:
Scaffolding generates Razor views that provide the user interface for interacting with your data. These views include forms for creating and editing records, as well as views for listing and viewing details of individual records.

```html
@model IEnumerable<Product>

@foreach (var product in Model)
{
    <div>
        <h4>@product.Name</h4>
        <p>@product.Price</p>
    </div>
}
```

### 4. Data Access Code:
If you're using Entity Framework or another ORM (Object-Relational Mapping) framework, scaffolding can generate data access code to interact with your database. This includes code for querying, inserting, updating, and deleting records in your database.

```csharp
public class ApplicationDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
}
```

### Benefits of Scaffolding:
- **Increased Productivity**: Scaffolding automates repetitive tasks, reducing the amount of manual coding required.
- **Consistency**: Generated code follows consistent patterns and conventions, improving maintainability.
- **Rapid Prototyping**: Scaffolding allows you to quickly create a functional CRUD interface for your data models, making it easier to prototype and iterate on your application.

In summary, ASP.NET MVC scaffolding is a powerful tool for accelerating the development of CRUD functionality in your web applications. It automates the process of creating controllers, views, and data access code, allowing you to focus on implementing your application's business logic rather than writing boilerplate code.

## **Step-by-step process:**

**Step 1: Create New Project:**

![Create new project](image-2.png)

**Step 2: Configure the project:**

![Configure the project](image-1.png)

**Step 3: Details of the Project Structure:**

![Project Structure](image-3.png)

**Step 4: Hosted application in portal:**

![Application](image-31.png)

**Step 5: Create a model:**

![Create a model](image-5.png)

**Step 6: Create a DB Context:**

![Create a DB Context](image-15.png)

**Step 7: Use Scaffolding:**

![Use Scaffolding](image-7.png)

**Step 8: Add Scaffold Item:**

![Add Scaffold Item](image-9.png)

**Step 9: Add MVC Controller:**

![Add MVC Controller](image-12.png)

**Step 10: Install packages:**

![Install packages](image-10.png)

**Step 11: Controller added:**

![Controller added](image-13.png)

**Step 12: StudentModels Folder inside View Folder:**

![StudentModels Folder](image-14.png)

**Step 13: Update DB Connection String:**

![Update DB Connection String](image-17.png)

**Step 14: Register Program.cs with Db Context:**

![Register Program.cs with Db Context](image-18.png)

**Step 15: Create Db Initilizer Class to initialize default list:**

![Create Db Initilizer Class](image-16.png)

**Step 16: Do Database Migration in Package Manager Console:**

![Database Migration](image-19.png)

**Step 17: Run and host the application in Portal:**

![Host in Portal](image-21.png)

**Step 18: Create new details in portal:**

![Create new details in portal](image-22.png)

**Step 19: After creating new details in portal:**

![After creating new details in portal](image-23.png)

**Step 20: View details in portal:**

![View details in portal](image-24.png)

**Step 21: Update details in portal:**

![Update details in portal](image-25.png)

**Step 22: After update in portal:**

![After update in portal](image-26.png)

**Step 23: Delete details in portal:**

![Delete details in portal](image-27.png)

**Step 24: After delete in portal:**

![After delete in portal](image-28.png)

**Step 25: View Database table:**

![View Database table](image-29.png)

**Step 26: Get details in DB using query:**

![Get details in DB](image-30.png)
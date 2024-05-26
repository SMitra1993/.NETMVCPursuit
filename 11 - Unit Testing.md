# Unit Testing

## **Workflow of Unit test case:**

### ASP.NET MVC Unit Testing

Unit testing in ASP.NET MVC involves testing the individual components of your MVC application, such as controllers, actions, and services, in isolation from the rest of the application. This ensures that each component behaves as expected under various conditions. Unit testing helps in identifying bugs early, improving code quality, and making the codebase more maintainable.

### Key Concepts for Unit Testing in ASP.NET MVC
1. **Isolation**: Tests should be isolated from each other and the environment.
2. **Mocking**: Use mocking frameworks like Moq to simulate the behavior of dependencies.
3. **Assertions**: Verify that the expected outcomes are met using assertions.
4. **Test Frameworks**: Use frameworks like xUnit, NUnit, or MSTest for writing and running tests.

### Tools Needed
- **Test Framework**: xUnit, NUnit, MSTest
- **Mocking Framework**: Moq

### Example Scenario: Testing an MVC Controller

#### Step 1: Set up the MVC Project and Add Dependencies
1. **Create an ASP.NET MVC Project**.
2. **Add References**:
   - xUnit: `dotnet add package xunit`
   - xUnit Runner: `dotnet add package xunit.runner.visualstudio`
   - Moq: `dotnet add package Moq`

#### Step 2: Create the MVC Controller
```csharp
// Models/Product.cs
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// Services/IProductService.cs
public interface IProductService
{
    IEnumerable<Product> GetAllProducts();
    Product GetProductById(int id);
    void CreateProduct(Product product);
    void DeleteProduct(int id);
}

// Services/ProductService.cs
public class ProductService : IProductService
{
    private readonly List<Product> _products = new List<Product>
    {
        new Product { Id = 1, Name = "Product1", Price = 10.0m },
        new Product { Id = 2, Name = "Product2", Price = 20.0m }
    };

    public IEnumerable<Product> GetAllProducts() => _products;

    public Product GetProductById(int id) => _products.FirstOrDefault(p => p.Id == id);

    public void CreateProduct(Product product) => _products.Add(product);

    public void DeleteProduct(int id) => _products.RemoveAll(p => p.Id == id);
}

// Controllers/ProductController.cs
public class ProductController : Controller
{
    private readonly IProductService _productService;

    public ProductController(IProductService productService)
    {
        _productService = productService;
    }

    public IActionResult Index()
    {
        var products = _productService.GetAllProducts();
        return View(products);
    }

    public IActionResult Details(int id)
    {
        var product = _productService.GetProductById(id);
        if (product == null)
        {
            return NotFound();
        }
        return View(product);
    }

    [HttpPost]
    public IActionResult Create(Product product)
    {
        if (ModelState.IsValid)
        {
            _productService.CreateProduct(product);
            return RedirectToAction(nameof(Index));
        }
        return View(product);
    }

    [HttpPost]
    public IActionResult Delete(int id)
    {
        _productService.DeleteProduct(id);
        return RedirectToAction(nameof(Index));
    }
}
```

#### Step 3: Create the Unit Test Project
1. **Create a new xUnit Test Project**.
2. **Add References**:
   - Moq
   - The MVC Project to access the controllers and models.

#### Step 4: Write Unit Tests
```csharp
// ProductControllerTests.cs
using Moq;
using Xunit;
using System.Collections.Generic;
using Microsoft.AspNetCore.Mvc;
using MyMvcApp.Controllers;
using MyMvcApp.Models;
using MyMvcApp.Services;

public class ProductControllerTests
{
    private readonly Mock<IProductService> _mockProductService;
    private readonly ProductController _controller;

    public ProductControllerTests()
    {
        _mockProductService = new Mock<IProductService>();
        _controller = new ProductController(_mockProductService.Object);
    }

    [Fact]
    public void Index_ReturnsAViewResult_WithAListOfProducts()
    {
        // Arrange
        var products = new List<Product> { new Product { Id = 1, Name = "Test Product" } };
        _mockProductService.Setup(service => service.GetAllProducts()).Returns(products);

        // Act
        var result = _controller.Index();

        // Assert
        var viewResult = Assert.IsType<ViewResult>(result);
        var model = Assert.IsAssignableFrom<IEnumerable<Product>>(viewResult.ViewData.Model);
        Assert.Single(model);
    }

    [Fact]
    public void Details_ReturnsNotFoundResult_WhenProductIsNotFound()
    {
        // Arrange
        _mockProductService.Setup(service => service.GetProductById(It.IsAny<int>())).Returns<Product>(null);

        // Act
        var result = _controller.Details(1);

        // Assert
        Assert.IsType<NotFoundResult>(result);
    }

    [Fact]
    public void Create_RedirectsToIndex_WhenModelStateIsValid()
    {
        // Arrange
        var product = new Product { Id = 1, Name = "Test Product" };

        // Act
        var result = _controller.Create(product);

        // Assert
        var redirectToActionResult = Assert.IsType<RedirectToActionResult>(result);
        Assert.Equal("Index", redirectToActionResult.ActionName);
    }

    [Fact]
    public void Delete_RedirectsToIndex_WhenCalled()
    {
        // Act
        var result = _controller.Delete(1);

        // Assert
        var redirectToActionResult = Assert.IsType<RedirectToActionResult>(result);
        Assert.Equal("Index", redirectToActionResult.ActionName);
    }
}
```

### Detailed Explanation

1. **Index_ReturnsAViewResult_WithAListOfProducts**:
   - Sets up the mock service to return a list of products.
   - Calls the `Index` action.
   - Asserts that the result is a `ViewResult` and the model is a list of products.

2. **Details_ReturnsNotFoundResult_WhenProductIsNotFound**:
   - Sets up the mock service to return null for any product ID.
   - Calls the `Details` action with a sample ID.
   - Asserts that the result is a `NotFoundResult`.

3. **Create_RedirectsToIndex_WhenModelStateIsValid**:
   - Creates a sample product.
   - Calls the `Create` action.
   - Asserts that the result is a `RedirectToActionResult` redirecting to the `Index` action.

4. **Delete_RedirectsToIndex_WhenCalled**:
   - Calls the `Delete` action with a sample ID.
   - Asserts that the result is a `RedirectToActionResult` redirecting to the `Index` action.

### Summary
Unit testing in ASP.NET MVC involves creating isolated tests for your controllers and services. By using mocking frameworks like Moq and test frameworks like xUnit, you can ensure that your components behave as expected. This improves the overall quality and maintainability of your codebase.
# Data Validation ❤🚀

**Links:**

- [Data Validation](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation--1)
- [ValidationMessageFor](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#validationmessagefor-)
- [ValidationSummary](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#validationsummary-)
- [Validation Controls](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#validation-controls-)
- [ValidateAntiForgeryToken](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#validationantiforgerytoken-)
- [`[Bind]` Attribute](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#validationantiforgerytoken-)
- [MVC Tag Helpers](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#mvc-tag-helpers-)
- [Data Annotations For Properties / Columns](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-annotations-for-properties--columns-)
- [Data Annotations For Database Schema](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-annotations-for-database-schema-)

## **Data Validation:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation-)

Data validation in ASP.NET MVC is vital for ensuring that the data submitted by users is accurate, complete, and meets the requirements of the application. It helps maintain data integrity and prevents potential security vulnerabilities. Here's a detailed explanation of how to achieve data validation in MVC with examples:

### 1. Using Data Annotations:
Data Annotations provide a declarative way to apply validation rules to model properties directly within the model class.

```csharp
using System.ComponentModel.DataAnnotations;

public class Product
{
    public int Id { get; set; }

    [Required(ErrorMessage = "Name is required.")]
    public string Name { get; set; }

    [Range(1, 1000, ErrorMessage = "Price must be between 1 and 1000.")]
    public decimal Price { get; set; }

    [EmailAddress(ErrorMessage = "Invalid email address.")]
    public string Email { get; set; }
}
```

In this example:
- The `[Required]` attribute specifies that the `Name` property is required.
- The `[Range]` attribute specifies that the `Price` property must be between 1 and 1000.
- The `[EmailAddress]` attribute specifies that the `Email` property must be a valid email address.

### 2. Using ModelState Validation:
In the controller, you can check the ModelState.IsValid property to determine if the model passed validation rules.

```csharp
public class ProductController : Controller
{
    [HttpPost]
    public ActionResult Create(Product product)
    {
        if (ModelState.IsValid)
        {
            // Save product to database
            return RedirectToAction("Index");
        }
        return View(product);
    }
}
```

In this example:
- The `Create` action method receives a `Product` object submitted from a form.
- `ModelState.IsValid` checks if the model passed validation rules specified by data annotations.

### 3. Custom Validation Attributes:
You can create custom validation attributes by deriving from `ValidationAttribute` class and implementing the `IsValid` method.

```csharp
public class CustomValidationAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        // Custom validation logic
        if (/* validation fails */)
        {
            return new ValidationResult("Validation failed.");
        }
        return ValidationResult.Success;
    }
}
```

```csharp
public class Product
{
    [CustomValidation]
    public string CustomProperty { get; set; }
}
```

In this example:
- `CustomValidationAttribute` implements custom validation logic in the `IsValid` method.
- The attribute is applied to the `CustomProperty` of the `Product` class.

### 4. Client-Side Validation:
Enable client-side validation by including the necessary JavaScript libraries, such as jQuery Validation, and enabling unobtrusive JavaScript.

```html
@Scripts.Render("~/bundles/jquery")
@Scripts.Render("~/bundles/jqueryval")
@{ Html.EnableClientValidation(); }
@{ Html.EnableUnobtrusiveJavaScript(); }
```

In this example:
- `Scripts.Render` includes jQuery and jQuery Validation libraries.
- `Html.EnableClientValidation()` and `Html.EnableUnobtrusiveJavaScript()` enable client-side validation.

### 5. Displaying Validation Errors:
Display validation errors in the view using the `ValidationMessageFor` HTML Helper.

```html
@Html.TextBoxFor(m => m.Name)
@Html.ValidationMessageFor(m => m.Name)
```

In this example:
- `Html.TextBoxFor` generates an input field for the `Name` property.
- `Html.ValidationMessageFor` displays validation errors for the `Name` property.

By combining these techniques, you can implement comprehensive data validation in your ASP.NET MVC application, ensuring data integrity and providing a seamless user experience.

## **ValidationMessageFor:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation-)

In ASP.NET MVC, `ValidationMessageFor` is an HTML Helper method that is used to display validation error messages for a specific model property. It is commonly used alongside `TextBoxFor`, `TextAreaFor`, and other input-related HTML Helper methods. `ValidationMessageFor` generates an HTML span element with the validation error message associated with the specified model property.

Here's how you can use `ValidationMessageFor` with an example:

Suppose you have a model class named `User` with a property called `Email`, and you want to validate this property to ensure it is not empty and is a valid email address.

```csharp
using System.ComponentModel.DataAnnotations;

public class User
{
    [Required(ErrorMessage = "Email is required.")]
    [EmailAddress(ErrorMessage = "Invalid email address.")]
    public string Email { get; set; }
}
```

In the corresponding view (`Create.cshtml`), you can use the `TextBoxFor` HTML Helper to create an input field for the `Email` property and `ValidationMessageFor` to display any validation error messages associated with it.

```html
@model User

@using (Html.BeginForm("Create", "User", FormMethod.Post))
{
    <div>
        @Html.LabelFor(m => m.Email)
        @Html.TextBoxFor(m => m.Email)
        @Html.ValidationMessageFor(m => m.Email)
    </div>
    <input type="submit" value="Submit" />
}
```

In this example:
- `Html.LabelFor` generates a label for the `Email` property.
- `Html.TextBoxFor` generates a text box input for the `Email` property.
- `Html.ValidationMessageFor` generates an HTML span element to display any validation error messages associated with the `Email` property.
- If the `Email` property fails validation (e.g., it is empty or not a valid email address), the appropriate error message specified in the data annotations (`[Required]` and `[EmailAddress]`) will be displayed.

When the form is submitted, ASP.NET MVC will automatically perform validation based on the data annotations applied to the model properties. If any validation errors occur, they will be displayed next to the corresponding input fields using `ValidationMessageFor`.

By using `ValidationMessageFor`, you can provide users with immediate feedback about any validation errors, improving the user experience and guiding them to enter valid data.

## **ValidationSummary:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation-)

In ASP.NET MVC, `ValidationSummary` is an HTML Helper method used to display a summary of validation errors that have occurred during form submission. It consolidates all validation error messages into a single location, making it convenient for users to identify and address any issues with their input.

Here's how you can use `ValidationSummary` with an example:

Suppose you have a model class named `User` with properties `FirstName` and `LastName`, and you want to validate these properties to ensure they are not empty.

```csharp
using System.ComponentModel.DataAnnotations;

public class User
{
    [Required(ErrorMessage = "First name is required.")]
    public string FirstName { get; set; }

    [Required(ErrorMessage = "Last name is required.")]
    public string LastName { get; set; }
}
```

In the corresponding view (`Create.cshtml`), you can use `ValidationSummary` to display a summary of validation errors that occur during form submission.

```html
@model User

@using (Html.BeginForm("Create", "User", FormMethod.Post))
{
    @Html.ValidationSummary()

    <div>
        @Html.LabelFor(m => m.FirstName)
        @Html.TextBoxFor(m => m.FirstName)
        @Html.ValidationMessageFor(m => m.FirstName)
    </div>
    
    <div>
        @Html.LabelFor(m => m.LastName)
        @Html.TextBoxFor(m => m.LastName)
        @Html.ValidationMessageFor(m => m.LastName)
    </div>

    <input type="submit" value="Submit" />
}
```

In this example:
- `Html.ValidationSummary` generates an HTML unordered list (`<ul>`) containing validation error messages for all model properties that failed validation.
- `Html.LabelFor`, `Html.TextBoxFor`, and `Html.ValidationMessageFor` are used to create input fields for the `FirstName` and `LastName` properties, as well as display any validation error messages associated with them.

When the form is submitted, ASP.NET MVC will automatically perform validation based on the data annotations applied to the model properties. If any validation errors occur, they will be displayed in the `ValidationSummary` section at the top of the form.

By using `ValidationSummary`, you provide users with a clear overview of any validation errors that occurred during form submission, allowing them to quickly identify and correct their input. This enhances the user experience and helps ensure that valid data is submitted to the server.

## **Validation Controls:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation-)

ASP.NET provides a set of validation controls that enable you to perform client-side and server-side validation of user input. These controls help ensure that data entered into web forms meets specific criteria or constraints. Let's explore each of these validation controls in detail along with examples:

### 1. CompareValidator:
The `CompareValidator` compares the value of an input control against the value of another control or a constant value.

**Example:**
Suppose you have two textboxes for password and confirm password fields, and you want to ensure that the passwords match.

```html
<asp:TextBox ID="txtPassword" runat="server" TextMode="Password"></asp:TextBox>
<asp:TextBox ID="txtConfirmPassword" runat="server" TextMode="Password"></asp:TextBox>
<asp:CompareValidator ID="cvPassword" runat="server" ControlToValidate="txtConfirmPassword" ControlToCompare="txtPassword" ErrorMessage="Passwords do not match"></asp:CompareValidator>
```

### 2. RangeValidator:
The `RangeValidator` checks whether the value entered into an input control falls within a specified range of values.

**Example:**
Suppose you have a textbox for entering a user's age, and you want to ensure that the age entered is between 18 and 60.

```html
<asp:TextBox ID="txtAge" runat="server"></asp:TextBox>
<asp:RangeValidator ID="rvAge" runat="server" ControlToValidate="txtAge" Type="Integer" MinimumValue="18" MaximumValue="60" ErrorMessage="Age must be between 18 and 60"></asp:RangeValidator>
```

### 3. RegularExpressionValidator:
The `RegularExpressionValidator` checks whether the value entered into an input control matches a specified pattern defined by a regular expression.

**Example:**
Suppose you have a textbox for entering an email address, and you want to ensure that the email address entered is in a valid format.

```html
<asp:TextBox ID="txtEmail" runat="server"></asp:TextBox>
<asp:RegularExpressionValidator ID="revEmail" runat="server" ControlToValidate="txtEmail" ValidationExpression="\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*" ErrorMessage="Invalid email address"></asp:RegularExpressionValidator>
```

### 4. RequiredFieldValidator:
The `RequiredFieldValidator` checks whether an input control has a value entered into it.

**Example:**
Suppose you have a textbox for entering a user's name, and you want to ensure that a name is provided.

```html
<asp:TextBox ID="txtName" runat="server"></asp:TextBox>
<asp:RequiredFieldValidator ID="rfvName" runat="server" ControlToValidate="txtName" ErrorMessage="Name is required"></asp:RequiredFieldValidator>
```

### 5. ValidationSummary:
The `ValidationSummary` control displays a summary of all validation errors that occurred on a web form.

**Example:**
Suppose you want to display a summary of all validation errors at the top of your form.

```html
<asp:ValidationSummary ID="vsSummary" runat="server" DisplayMode="BulletList" ShowMessageBox="true" ShowSummary="false" />
```

In summary, ASP.NET validation controls provide a powerful way to validate user input in web forms. By using these controls, you can enforce data integrity and ensure that users provide valid data before submitting forms to the server. They help improve the overall user experience by providing immediate feedback on any validation errors that occur.

## **ValidateAntiForgeryToken:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation-)

The `ValidateAntiForgeryToken` attribute in ASP.NET MVC is used to prevent Cross-Site Request Forgery (CSRF) attacks. CSRF is an attack where a malicious website, email, or program causes a user's web browser to perform an unwanted action on a trusted site for which the user is currently authenticated. The `ValidateAntiForgeryToken` attribute helps to ensure that the requests received by an MVC application are genuine and made by the authenticated user.

### How `ValidateAntiForgeryToken` Works

1. **Token Generation**:
   - When a form is rendered, a unique token is generated and embedded in the form as a hidden field.
   - This token is also stored in the user's cookies.

2. **Token Validation**:
   - When the form is submitted, the token from the form and the token in the user's cookies are sent to the server.
   - The server then validates that these two tokens match, ensuring the request is legitimate and originates from the authenticated user.

### Implementation

#### 1. Enabling Anti-Forgery Token

To use `ValidateAntiForgeryToken`, you need to include the anti-forgery token in your forms. You can use the `@Html.AntiForgeryToken()` method in your Razor views to generate the token.

**View (Razor):**
```html
@using (Html.BeginForm("Submit", "Home", FormMethod.Post))
{
    @Html.AntiForgeryToken()

    <div>
        @Html.LabelFor(m => m.Name)
        @Html.TextBoxFor(m => m.Name)
    </div>

    <button type="submit">Submit</button>
}
```

#### 2. Validating the Token

On the server side, you use the `[ValidateAntiForgeryToken]` attribute on your action methods to validate the token.

**Controller:**
```csharp
public class HomeController : Controller
{
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Submit(MyModel model)
    {
        if (ModelState.IsValid)
        {
            // Process the form submission
            return RedirectToAction("Success");
        }
        
        return View(model);
    }
}
```

### Example Scenario

Let's consider a scenario where a user submits a feedback form on a website. The website needs to ensure that the feedback is submitted by the user and not by a malicious script.

**Step-by-Step Example:**

1. **Model:**
   ```csharp
   public class FeedbackModel
   {
       [Required]
       public string Name { get; set; }

       [Required]
       [EmailAddress]
       public string Email { get; set; }

       [Required]
       public string Message { get; set; }
   }
   ```

2. **View:**
   ```html
   @model FeedbackModel

   @using (Html.BeginForm("SubmitFeedback", "Feedback", FormMethod.Post))
   {
       @Html.AntiForgeryToken()

       <div>
           @Html.LabelFor(m => m.Name)
           @Html.TextBoxFor(m => m.Name)
           @Html.ValidationMessageFor(m => m.Name)
       </div>
       <div>
           @Html.LabelFor(m => m.Email)
           @Html.TextBoxFor(m => m.Email)
           @Html.ValidationMessageFor(m => m.Email)
       </div>
       <div>
           @Html.LabelFor(m => m.Message)
           @Html.TextAreaFor(m => m.Message)
           @Html.ValidationMessageFor(m => m.Message)
       </div>

       <button type="submit">Submit Feedback</button>
   }
   ```

3. **Controller:**
   ```csharp
   public class FeedbackController : Controller
   {
       [HttpPost]
       [ValidateAntiForgeryToken]
       public ActionResult SubmitFeedback(FeedbackModel model)
       {
           if (ModelState.IsValid)
           {
               // Save feedback to the database or process it accordingly
               return RedirectToAction("ThankYou");
           }

           return View(model);
       }

       public ActionResult ThankYou()
       {
           return View();
       }
   }
   ```

### Important Points to Remember

1. **Form Submission**: Always include `@Html.AntiForgeryToken()` in your form views to generate the anti-forgery token.
2. **Validation**: Use the `[ValidateAntiForgeryToken]` attribute on your controller action methods that handle form submissions to validate the token.
3. **Token Matching**: The tokens embedded in the form and stored in cookies must match for the request to be considered valid.
4. **Security**: This mechanism helps mitigate CSRF attacks, ensuring that the requests processed by your application are legitimate.

By implementing anti-forgery tokens, you significantly improve the security of your ASP.NET MVC applications, protecting them from a common type of web vulnerability.

## **`[Bind]` Attribute:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation-)

The `[Bind]` attribute in ASP.NET MVC is used to specify which properties of a model should be included or excluded when model binding occurs. This helps to prevent overposting attacks and ensures that only the intended properties of a model are updated by user input.

### Using the `[Bind]` Attribute

The `[Bind]` attribute can be applied to action method parameters to control which properties are bound to incoming request data. You can include or exclude properties by specifying their names.

### Example Scenario

Let's consider a scenario where we have a `Student` model with several properties. We want to create an action method that allows users to create a new student, but we want to ensure that only specific properties can be set by the user.

#### Student Model
```csharp
public class Student
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
    public string Phone { get; set; }
    public DateTime CreatedAt { get; set; }  // Automatically set by the server
    public bool IsAdmin { get; set; }  // Sensitive property
}
```

#### Controller Using `[Bind]` Attribute

In this example, we use the `[Bind]` attribute to ensure that only `FirstName`, `LastName`, `Email`, and `Phone` properties are bound from the incoming request.

```csharp
public class StudentsController : Controller
{
    private readonly ApplicationDbContext _context;

    public StudentsController(ApplicationDbContext context)
    {
        _context = context;
    }

    // GET: Students/Create
    public IActionResult Create()
    {
        return View();
    }

    // POST: Students/Create
    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> Create([Bind("FirstName,LastName,Email,Phone")] Student student)
    {
        if (ModelState.IsValid)
        {
            student.CreatedAt = DateTime.Now;  // Server sets CreatedAt
            _context.Add(student);
            await _context.SaveChangesAsync();
            return RedirectToAction(nameof(Index));
        }
        return View(student);
    }

    // GET: Students
    public async Task<IActionResult> Index()
    {
        return View(await _context.Students.ToListAsync());
    }
}
```

#### View (Razor)

Here's the form in the view that allows users to create a new student. Only the included properties (`FirstName`, `LastName`, `Email`, and `Phone`) are displayed and can be set by the user.

```html
@model Student

@using (Html.BeginForm("Create", "Students", FormMethod.Post))
{
    @Html.AntiForgeryToken()
    
    <div class="form-group">
        @Html.LabelFor(model => model.FirstName)
        @Html.EditorFor(model => model.FirstName)
        @Html.ValidationMessageFor(model => model.FirstName)
    </div>
    
    <div class="form-group">
        @Html.LabelFor(model => model.LastName)
        @Html.EditorFor(model => model.LastName)
        @Html.ValidationMessageFor(model => model.LastName)
    </div>
    
    <div class="form-group">
        @Html.LabelFor(model => model.Email)
        @Html.EditorFor(model => model.Email)
        @Html.ValidationMessageFor(model => model.Email)
    </div>
    
    <div class="form-group">
        @Html.LabelFor(model => model.Phone)
        @Html.EditorFor(model => model.Phone)
        @Html.ValidationMessageFor(model => model.Phone)
    </div>
    
    <button type="submit" class="btn btn-primary">Create</button>
}
```

### Explanation

1. **Model Definition**: The `Student` model contains several properties, including some that should not be set by the user, such as `CreatedAt` and `IsAdmin`.

2. **Controller Action**:
   - **GET**: The `Create` action displays the form to create a new student.
   - **POST**: The `Create` action uses the `[Bind]` attribute to specify which properties can be set by the user. Only `FirstName`, `LastName`, `Email`, and `Phone` are included in the binding.
   - **ModelState Validation**: The action checks if the model state is valid. If it is, the `CreatedAt` property is set by the server, and the new student is added to the database.

3. **View**: The form only includes fields for the properties specified in the `[Bind]` attribute.

### Security Benefits

By using the `[Bind]` attribute, you protect against overposting attacks. For instance, even if an attacker tries to include `IsAdmin` in the form data, it won't be bound to the `Student` object because it's not included in the `[Bind]` attribute.

#### Example of Overposting Attempt

If an attacker tries to manipulate the form data:

```html
<form method="post" action="/Students/Create">
    <input type="text" name="FirstName" value="John" />
    <input type="text" name="LastName" value="Doe" />
    <input type="email" name="Email" value="john.doe@example.com" />
    <input type="text" name="Phone" value="1234567890" />
    <input type="hidden" name="IsAdmin" value="true" />  <!-- Malicious field -->
    <button type="submit">Create</button>
</form>
```

Without the `[Bind]` attribute, this could potentially set the `IsAdmin` property. With the `[Bind]` attribute in place, the `IsAdmin` property remains unaffected, and the attempt to overpost is thwarted.

### Conclusion

The `[Bind]` attribute is a powerful tool in ASP.NET MVC for controlling model binding and enhancing security. It allows you to specify which properties should be bound to incoming request data, protecting against overposting attacks and ensuring that only the intended properties are updated.

## **MVC Tag Helpers:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation-)

Sure, I'll merge both tables and answers together for a comprehensive overview of MVC Tag Helpers in ASP.NET Core, including detailed descriptions, examples, and their generated HTML.

### MVC Tag Helpers in ASP.NET Core

| Tag Helper                     | Description                                              | Example                                                            | Generated HTML                                                    |
|--------------------------------|----------------------------------------------------------|--------------------------------------------------------------------|-------------------------------------------------------------------|
| `AnchorTagHelper`              | Generates an anchor (`<a>`) element                      | `<a asp-controller="Home" asp-action="Index">Home</a>`             | `<a href="/Home/Index">Home</a>`                                  |
| `ButtonTagHelper`              | Generates a button element                               | `<button asp-controller="Home" asp-action="Index">Home</button>`   | `<button type="submit" formaction="/Home/Index">Home</button>`    |
| `CacheTagHelper`               | Caches the rendered content                              | `<cache vary-by-route="*" expires-after="60"></cache>`             | Caches the content for 60 seconds.                                |
| `DistributedCacheTagHelper`    | Caches the rendered content using a distributed cache    | `<distributed-cache vary-by-route="*" expires-after="60"></distributed-cache>` | Uses a distributed cache to store the content.                    |
| `EnvironmentTagHelper`         | Includes content only for specific environments          | `<environment names="Development"><link rel="stylesheet" href="~/css/dev-only.css" /></environment>` | Only includes the content if the environment is Development.      |
| `FormTagHelper`                | Generates a form element                                 | `<form asp-controller="Home" asp-action="Submit" method="post"><input asp-for="UserName" /><button type="submit">Submit</button></form>` | `<form action="/Home/Submit" method="post"><input id="UserName" name="UserName" type="text" /><button type="submit">Submit</button></form>` |
| `ImageTagHelper`               | Generates an image element                               | `<img src="~/images/logo.png" asp-append-version="true" />`        | `<img src="/images/logo.png?v=XYZ" />`                            |
| `InputTagHelper`               | Generates an input element                               | `<input asp-for="UserName" />`                                     | `<input id="UserName" name="UserName" type="text" />`             |
| `LabelTagHelper`               | Generates a label element                                | `<label asp-for="UserName"></label>`                               | `<label for="UserName">UserName</label>`                          |
| `LinkTagHelper`                | Generates a link element for stylesheets                 | `<link rel="stylesheet" href="~/css/site.css" asp-append-version="true" />` | `<link rel="stylesheet" href="/css/site.css?v=XYZ" />`            |
| `OptionTagHelper`              | Generates an option element                              | `<option asp-for="Country" asp-value="Value">Text</option>`        | `<option value="Value">Text</option>`                             |
| `PartialTagHelper`             | Renders a partial view                                   | `<partial name="_LoginPartial" />`                                 | Renders the `_LoginPartial.cshtml` partial view.                  |
| `RenderAtEndOfBodyTagHelper`   | Renders scripts at the end of the body                   | `<script src="~/lib/jquery/dist/jquery.js" asp-append-version="true"></script>` | `<script src="/lib/jquery/dist/jquery.js?v=XYZ"></script>`        |
| `ScriptTagHelper`              | Generates a script element                               | `<script src="~/js/site.js" asp-append-version="true"></script>`   | `<script src="/js/site.js?v=XYZ"></script>`                       |
| `SectionTagHelper`             | Defines a section in the layout or view                  | `<section name="Scripts">@RenderSection("Scripts", required: false)</section>` | Defines a section that can be rendered in the layout.             |
| `SelectTagHelper`              | Generates a select element                               | `<select asp-for="Country" asp-items="ViewBag.Countries"></select>` | `<select id="Country" name="Country"><!-- options from ViewBag.Countries --></select>` |
| `StyleTagHelper`               | Generates a style element                                | `<style src="~/css/site.css" asp-append-version="true"></style>`   | `<link href="/css/site.css?v=XYZ" rel="stylesheet" />`            |
| `TextareaTagHelper`            | Generates a textarea element                             | `<textarea asp-for="Description"></textarea>`                      | `<textarea id="Description" name="Description"></textarea>`       |
| `ValidationMessageTagHelper`   | Displays a validation message for a specific field       | `<span asp-validation-for="UserName"></span>`                      | `<span class="field-validation-valid" data-valmsg-for="UserName" data-valmsg-replace="true"></span>` |
| `ValidationSummaryTagHelper`   | Displays a summary of validation errors                  | `<div asp-validation-summary="ModelOnly"></div>`                   | `<div class="validation-summary-valid" data-valmsg-summary="true"></div>` |

### Detailed Examples and Descriptions

#### AnchorTagHelper
Generates an anchor (`<a>`) element that links to a specific action and controller.
```html
<a asp-controller="Home" asp-action="Index">Home</a>
```
Generated HTML:
```html
<a href="/Home/Index">Home</a>
```

#### ButtonTagHelper
Generates a button element that can trigger an action.
```html
<button asp-controller="Home" asp-action="Index">Home</button>
```
Generated HTML:
```html
<button type="submit" formaction="/Home/Index">Home</button>
```

#### CacheTagHelper
Caches the rendered content to improve performance.
```html
<cache vary-by-route="*" expires-after="60">
    <p>@DateTime.Now</p>
</cache>
```
This caches the content for 60 seconds.

#### DistributedCacheTagHelper
Caches the rendered content using a distributed cache.
```html
<distributed-cache vary-by-route="*" expires-after="60">
    <p>@DateTime.Now</p>
</distributed-cache>
```
This uses a distributed cache to store the content.

#### EnvironmentTagHelper
Includes content only for specific environments.
```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/dev-only.css" />
</environment>
```
This only includes the content if the environment is set to Development.

#### FormTagHelper
Generates a form element.
```html
<form asp-controller="Home" asp-action="Submit" method="post">
    <input asp-for="UserName" />
    <button type="submit">Submit</button>
</form>
```
Generated HTML:
```html
<form action="/Home/Submit" method="post">
    <input id="UserName" name="UserName" type="text" />
    <button type="submit">Submit</button>
</form>
```

#### ImageTagHelper
Generates an image element with versioned URLs for cache busting.
```html
<img src="~/images/logo.png" asp-append-version="true" />
```
Generated HTML:
```html
<img src="/images/logo.png?v=XYZ" />
```

#### InputTagHelper
Generates an input element.
```html
<input asp-for="UserName" />
```
Generated HTML:
```html
<input id="UserName" name="UserName" type="text" />
```

#### LabelTagHelper
Generates a label element.
```html
<label asp-for="UserName"></label>
```
Generated HTML:
```html
<label for="UserName">UserName</label>
```

#### LinkTagHelper
Generates a link element for stylesheets.
```html
<link rel="stylesheet" href="~/css/site.css" asp-append-version="true" />
```
Generated HTML:
```html
<link rel="stylesheet" href="/css/site.css?v=XYZ" />
```

#### OptionTagHelper
Generates an option element for a select list.
```html
<option asp-for="Country" asp-value="US">United States</option>
```
Generated HTML:
```html
<option value="US">United States</option>
```

#### PartialTagHelper
Renders a partial view.
```html
<partial name="_LoginPartial" />
```
Renders the `_LoginPartial.cshtml` partial view.

#### RenderAtEndOfBodyTagHelper
Renders scripts at the end of the body for better performance.
```html
<script src="~/lib/jquery/dist/jquery.js" asp-append-version="true"></script>
```
Generated HTML:
```html
<script src="/lib/jquery/dist/jquery.js?v=XYZ"></script>
```

#### ScriptTagHelper
Generates a script element, appending a version parameter to the URL.
```html
<script src="~/js/site.js" asp-append-version="true"></script>
```
Generated HTML:
```html
<script src="/js/site.js?v=XYZ"></script>
```

#### SectionTagHelper
Defines a section in the layout or view.
```html
<section name="Scripts">
    @RenderSection("Scripts", required: false)
</section>
```
Defines a section that can be rendered in the layout.

#### SelectTagHelper
Generates a select element.
```html
<select asp-for="Country" asp-items="ViewBag.Countries"></select>
```
Generated HTML:
```html
<select id="Country" name="Country">
    <!-- options from ViewBag.Countries -->
</select>
```

#### StyleTagHelper
Generates a style element with version

ed URLs for cache busting.
```html
<style src="~/css/site.css" asp-append-version="true"></style>
```
Generated HTML:
```html
<link href="/css/site.css?v=XYZ" rel="stylesheet" />
```

#### TextareaTagHelper
Generates a textarea element.
```html
<textarea asp-for="Description"></textarea>
```
Generated HTML:
```html
<textarea id="Description" name="Description"></textarea>
```

#### ValidationMessageTagHelper
Displays a validation message for a specific field.
```html
<span asp-validation-for="UserName"></span>
```
Generated HTML:
```html
<span class="field-validation-valid" data-valmsg-for="UserName" data-valmsg-replace="true"></span>
```

#### ValidationSummaryTagHelper
Displays a summary of validation errors.
```html
<div asp-validation-summary="ModelOnly"></div>
```
Generated HTML:
```html
<div class="validation-summary-valid" data-valmsg-summary="true"></div>
```

### Conclusion

MVC Tag Helpers in ASP.NET Core significantly streamline the process of binding server-side code with HTML elements, making Razor views cleaner and more maintainable. They assist in generating dynamic, data-driven web content efficiently, providing better performance and easier debugging through the use of cache helpers and versioned URLs. By leveraging these Tag Helpers, developers can build robust and interactive web applications more effectively.

## **Data Annotations For Properties / Columns:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation-)

### Data Annotations in Model Binding

Data annotations in ASP.NET MVC are used to configure the model classes by adding metadata that can control how data is displayed and validated. They provide a way to enforce rules and constraints directly in the model, ensuring data integrity and consistency. These annotations can be used for validation, formatting, and specifying database schema details.

Here's a detailed explanation of some commonly used data annotations:

#### Key
The `[Key]` attribute is used to denote the primary key of an entity.

```csharp
public class Product
{
    [Key]
    public int ProductId { get; set; }
    
    public string Name { get; set; }
}
```

#### Timestamp
The `[Timestamp]` attribute is used for concurrency checking. It generates a timestamp column in the database to handle concurrency conflicts.

```csharp
public class Product
{
    public int ProductId { get; set; }

    [Timestamp]
    public byte[] RowVersion { get; set; }
}
```

#### ConcurrencyCheck
The `[ConcurrencyCheck]` attribute specifies that a property should be included in the concurrency check.

```csharp
public class Product
{
    public int ProductId { get; set; }

    [ConcurrencyCheck]
    public string Name { get; set; }
}
```

#### Required
The `[Required]` attribute specifies that a property must have a value. It is used for validating that the field is not empty.

```csharp
public class Product
{
    public int ProductId { get; set; }

    [Required(ErrorMessage = "Name is required")]
    public string Name { get; set; }
}
```

#### MinLength
The `[MinLength]` attribute specifies the minimum length of a string or array data field.

```csharp
public class Product
{
    public int ProductId { get; set; }

    [MinLength(3, ErrorMessage = "Name must be at least 3 characters long")]
    public string Name { get; set; }
}
```

#### MaxLength
The `[MaxLength]` attribute specifies the maximum length of a string or array data field.

```csharp
public class Product
{
    public int ProductId { get; set; }

    [MaxLength(100, ErrorMessage = "Name cannot be more than 100 characters long")]
    public string Name { get; set; }
}
```

#### StringLength
The `[StringLength]` attribute specifies both the maximum and optional minimum length of a string data field.

```csharp
public class Product
{
    public int ProductId { get; set; }

    [StringLength(100, MinimumLength = 3, ErrorMessage = "Name must be between 3 and 100 characters long")]
    public string Name { get; set; }
}
```

### Example Scenario

Consider a `Product` model where you want to enforce specific rules for data validation and database schema constraints. Here's how you can apply these data annotations:

```csharp
public class Product
{
    [Key]
    public int ProductId { get; set; }

    [Required(ErrorMessage = "Name is required")]
    [StringLength(100, MinimumLength = 3, ErrorMessage = "Name must be between 3 and 100 characters long")]
    public string Name { get; set; }

    [ConcurrencyCheck]
    public decimal Price { get; set; }

    [Required]
    [MinLength(3, ErrorMessage = "Category must be at least 3 characters long")]
    [MaxLength(50, ErrorMessage = "Category cannot be more than 50 characters long")]
    public string Category { get; set; }

    [Timestamp]
    public byte[] RowVersion { get; set; }
}
```

### Applying Data Annotations in an ASP.NET MVC Application

1. **Create a Model:**

   ```csharp
   public class Product
   {
       [Key]
       public int ProductId { get; set; }

       [Required(ErrorMessage = "Name is required")]
       [StringLength(100, MinimumLength = 3, ErrorMessage = "Name must be between 3 and 100 characters long")]
       public string Name { get; set; }

       [ConcurrencyCheck]
       public decimal Price { get; set; }

       [Required]
       [MinLength(3, ErrorMessage = "Category must be at least 3 characters long")]
       [MaxLength(50, ErrorMessage = "Category cannot be more than 50 characters long")]
       public string Category { get; set; }

       [Timestamp]
       public byte[] RowVersion { get; set; }
   }
   ```

2. **Create a Controller:**

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

       // GET: Products/Create
       public IActionResult Create()
       {
           return View();
       }

       // POST: Products/Create
       [HttpPost]
       [ValidateAntiForgeryToken]
       public async Task<IActionResult> Create([Bind("ProductId,Name,Price,Category,RowVersion")] Product product)
       {
           if (ModelState.IsValid)
           {
               _context.Add(product);
               await _context.SaveChangesAsync();
               return RedirectToAction(nameof(Index));
           }
           return View(product);
       }
   }
   ```

3. **Create a View:**

   Create a `Create.cshtml` view to add a new product.

   ```html
   @model YourNamespace.Models.Product

   <h2>Create Product</h2>

   <form asp-action="Create">
       <div class="form-group">
           <label asp-for="Name" class="control-label"></label>
           <input asp-for="Name" class="form-control" />
           <span asp-validation-for="Name" class="text-danger"></span>
       </div>
       <div class="form-group">
           <label asp-for="Price" class="control-label"></label>
           <input asp-for="Price" class="form-control" />
           <span asp-validation-for="Price" class="text-danger"></span>
       </div>
       <div class="form-group">
           <label asp-for="Category" class="control-label"></label>
           <input asp-for="Category" class="form-control" />
           <span asp-validation-for="Category" class="text-danger"></span>
       </div>
       <div class="form-group">
           <input type="submit" value="Create" class="btn btn-primary" />
       </div>
   </form>
   ```

### Summary

Using data annotations in ASP.NET MVC, you can ensure that your models adhere to specific validation and database schema rules, thus improving data integrity and reducing the likelihood of errors. The annotations like `[Key]`, `[Required]`, `[MinLength]`, `[MaxLength]`, `[StringLength]`, `[ConcurrencyCheck]`, and `[Timestamp]` allow you to declaratively specify these rules directly in your model classes.

## **Data Annotations For Database Schema:** [🏠](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation-)

### Data Annotations in Model Binding

Data annotations in ASP.NET MVC and Entity Framework (EF) are used to add metadata to the model classes. This metadata can control how data is displayed, validated, and mapped to the database. These annotations are used for a variety of purposes including configuring database schema details and setting validation rules.

Below are the descriptions of specific data annotations related to database schema configuration:

#### Table
The `[Table]` attribute is used to specify the database table name that a class should map to.

```csharp
[Table("Products")]
public class Product
{
    [Key]
    public int ProductId { get; set; }
    
    public string Name { get; set; }
}
```
In this example, the `Product` class will be mapped to the `Products` table in the database.

#### Column
The `[Column]` attribute is used to specify the database column that a property should map to.

```csharp
public class Product
{
    [Key]
    public int ProductId { get; set; }

    [Column("ProductName")]
    public string Name { get; set; }
}
```
In this example, the `Name` property will be mapped to the `ProductName` column in the database.

#### Index
The `[Index]` attribute is used to create an index on a specific column or columns in the database. Note that this attribute is not natively supported in EF Core, but you can use the fluent API for index creation.

```csharp
public class Product
{
    [Key]
    public int ProductId { get; set; }

    [Index(IsUnique = true)]
    public string Name { get; set; }
}
```
In this example, an index will be created on the `Name` column, ensuring that the values are unique.

#### ForeignKey
The `[ForeignKey]` attribute is used to specify a foreign key relationship between two tables.

```csharp
public class Order
{
    [Key]
    public int OrderId { get; set; }

    public int ProductId { get; set; }

    [ForeignKey("ProductId")]
    public Product Product { get; set; }
}
```
In this example, the `ProductId` property in the `Order` class is a foreign key that references the `Product` table.

#### NotMapped
The `[NotMapped]` attribute is used to specify that a property should not be mapped to a column in the database.

```csharp
public class Product
{
    [Key]
    public int ProductId { get; set; }

    [NotMapped]
    public string TemporaryData { get; set; }
}
```
In this example, the `TemporaryData` property will not be mapped to any column in the database.

#### InverseProperty
The `[InverseProperty]` attribute is used to configure the inverse relationship between two navigation properties.

```csharp
public class Order
{
    [Key]
    public int OrderId { get; set; }

    public int ProductId { get; set; }

    [ForeignKey("ProductId")]
    public Product Product { get; set; }
}

public class Product
{
    [Key]
    public int ProductId { get; set; }

    public string Name { get; set; }

    [InverseProperty("Product")]
    public ICollection<Order> Orders { get; set; }
}
```
In this example, the `Orders` collection in the `Product` class is the inverse of the `Product` navigation property in the `Order` class.

### Example Scenario

Consider an e-commerce application where you need to manage products and orders. You can use the data annotations to configure the database schema details.

```csharp
[Table("Products")]
public class Product
{
    [Key]
    public int ProductId { get; set; }

    [Column("ProductName")]
    [Index(IsUnique = true)]
    public string Name { get; set; }

    [NotMapped]
    public string TemporaryData { get; set; }

    [InverseProperty("Product")]
    public ICollection<Order> Orders { get; set; }
}

[Table("Orders")]
public class Order
{
    [Key]
    public int OrderId { get; set; }

    public int ProductId { get; set; }

    [ForeignKey("ProductId")]
    public Product Product { get; set; }
}
```

In this example:
- The `Product` class is mapped to the `Products` table.
- The `Name` property in the `Product` class is mapped to the `ProductName` column and has a unique index.
- The `TemporaryData` property in the `Product` class is not mapped to the database.
- The `Order` class is mapped to the `Orders` table.
- The `ProductId` property in the `Order` class is a foreign key to the `Products` table.
- The `Orders` collection in the `Product` class is the inverse property of the `Product` navigation property in the `Order` class.

### Summary

Data annotations in ASP.NET MVC and EF provide a powerful way to configure your models with metadata. By using annotations such as `[Table]`, `[Column]`, `[Index]`, `[ForeignKey]`, `[NotMapped]`, and `[InverseProperty]`, you can control how your models map to the database schema and enforce validation rules directly within your model classes. This approach ensures consistency, data integrity, and simplifies the overall development process by keeping configuration close to the data it pertains to.
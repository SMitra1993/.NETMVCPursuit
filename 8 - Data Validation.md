# Data Validation ❤🚀

**Links:**

- [Data Validation](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#data-validation--1)
- [ValidationMessageFor](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#validationmessagefor-)
- [ValidationSummary](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#validationsummary-)
- [Validation Controls](https://github.com/SMitra1993/.NETMVCPursuit/blob/master/8%20-%20Data%20Validation.md#validation-controls-)

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


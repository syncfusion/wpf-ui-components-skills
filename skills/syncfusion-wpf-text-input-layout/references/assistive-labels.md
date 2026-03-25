# Assistive Labels in WPF TextInputLayout

Assistive labels provide additional information, guidance, and feedback about the text entered in the input view. They include helper text, error messages, and character counters.

## Table of Contents
- [Helper Text](#helper-text)
- [Error Messages](#error-messages)
- [Character Counter](#character-counter)
- [Validation Patterns](#validation-patterns)
- [Best Practices](#best-practices)

## Helper Text

Helper text provides additional guidance about the input field, explaining how to use it or what format is expected.

### HelperText Property

Set helper text to guide users:

**XAML:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Name"
    HelperText="Enter your full name">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.HelperText = "Enter your full name";
inputLayout.InputView = new TextBox();
```

### Visual Appearance

```
Name
┌─────────────────┐
│                 │
└─────────────────┘
Enter your full name  ← Helper text below input
```

### Helper Text Visibility

Control helper text visibility with the `HelperTextVisibility` property:

**XAML:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Email"
    HelperText="We'll never share your email"
    HelperTextVisibility="Visible">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**Options:**
- `Visible`: Helper text is displayed (default when HelperText is set)
- `Collapsed`: Helper text is hidden and space is not reserved
- `Hidden`: Helper text is hidden but space is reserved

### When to Use Helper Text

Use helper text to:
- **Explain format requirements**: "Use format: MM/DD/YYYY"
- **Provide examples**: "e.g., john.doe@example.com"
- **Clarify purpose**: "This will be displayed publicly"
- **Give constraints**: "Minimum 8 characters required"
- **Offer reassurance**: "Your information is secure"

### Helper Text Examples

**Password Requirements:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Password"
    HelperText="Must contain at least 8 characters, including uppercase, lowercase, and numbers">
    <PasswordBox />
</inputLayout:SfTextInputLayout>
```

**Format Guidance:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Phone Number"
    HelperText="Format: (555) 123-4567">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**Privacy Notice:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Bio"
    HelperText="This information will be visible to other users">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

## Error Messages

Error messages provide feedback when the input doesn't meet validation requirements.

### ErrorText Property

Set error text for validation feedback:

**XAML:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Email" 
    HelperText="Enter your email address"
    ErrorText="Invalid email format"
    HasError="True">
    <TextBox Text="invalid@"/>
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Email";
inputLayout.HelperText = "Enter your email address";
inputLayout.ErrorText = "Invalid email format";
inputLayout.HasError = true;
inputLayout.InputView = new TextBox() { Text = "invalid@" };
```

### HasError Property

The `HasError` property controls whether the error state is active.

**Key Points:**
- ✅ When `true`: Error text is shown, error colors are applied
- ❌ When `false`: Helper text is shown (if set), normal colors are applied
- **Default:** `false`

### Visual Appearance with Error

```
Unfocused with Error:
Email
┌─────────────────┐
│ invalid@        │  ← Red border
└─────────────────┘
✕ Invalid email format  ← Red error text

Focused with Error:
Email
╔═════════════════╗
║ invalid@        ║  ← Red border (thicker)
╚═════════════════╝
✕ Invalid email format
```

### Error State Behavior

When `HasError = true`:
- Hint label becomes red
- Border or base line becomes red
- Error text replaces helper text
- Error color is applied from `ErrorForeground` property

### Important Notes

> **Note:** Error validation logic must be implemented at the application level. The `HasError` property only controls the visual state; you need to write validation code.

> **Note:** Error is not considered a "state" for the `ActiveForeground` property. The `ActiveForeground` reflects only focused/unfocused states.

## Character Counter

The character counter displays the current and maximum character count, helping users stay within limits.

### CharMaxLength Property

Set the maximum character limit:

**XAML:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    CharMaxLength="20"
    CharCountVisibility="Visible"
    HelperText="Enter 5 to 20 characters">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.CharMaxLength = 20;
inputLayout.CharCountVisibility = Visibility.Visible;
inputLayout.HelperText = "Enter 5 to 20 characters";
inputLayout.InputView = new TextBox();
```

### Visual Appearance

```
Name
┌─────────────────┐
│ John            │
└─────────────────┘
Enter 5 to 20 characters    4/20  ← Character count
```

### CharCountVisibility Property

Control character counter visibility:

**Options:**
- `Visible`: Counter is displayed
- `Collapsed`: Counter is hidden (default)
- `Hidden`: Counter is hidden but space is reserved

### Exceeding Character Limit

When the character count exceeds `CharMaxLength`:
- **Counter turns red** (uses `ErrorForeground`)
- **Hint label turns red**
- **Border turns red**
- **Helper text remains visible** (unlike error state)

**Example when limit exceeded:**
```
Name
┌─────────────────────────┐
│ ThisIsAVeryLongName     │  ← Red border
└─────────────────────────┘
Enter 5 to 20 characters    23/20  ← Red counter
```

### Character Counter Without Helper Text

You can use character counter alone:

```xml
<inputLayout:SfTextInputLayout
    Hint="Tweet" 
    CharMaxLength="280"
    CharCountVisibility="Visible">
    <TextBox AcceptsReturn="True" TextWrapping="Wrap" Height="100"/>
</inputLayout:SfTextInputLayout>
```

## Validation Patterns

### Real-Time Email Validation

Validate email format as the user types:

**XAML:**
```xml
<inputLayout:SfTextInputLayout 
    x:Name="emailLayout"
    Hint="Email Address"
    HelperText="Enter a valid email"
    ErrorText="Invalid email format">
    <TextBox TextChanged="OnEmailTextChanged"/>
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
using System.Text.RegularExpressions;

private void OnEmailTextChanged(object sender, TextChangedEventArgs e)
{
    var textBox = sender as TextBox;
    string email = textBox.Text;
    
    if (string.IsNullOrEmpty(email))
    {
        emailLayout.HasError = false;
        return;
    }
    
    // Simple email regex pattern
    string pattern = @"^[^@\s]+@[^@\s]+\.[^@\s]+$";
    bool isValid = Regex.IsMatch(email, pattern);
    
    emailLayout.HasError = !isValid;
}
```

### Password Strength Validation

Validate password complexity:

**XAML:**
```xml
<inputLayout:SfTextInputLayout 
    x:Name="passwordLayout"
    Hint="Password"
    HelperText="Minimum 8 characters with uppercase, lowercase, and number"
    ErrorText="Password does not meet requirements">
    <PasswordBox PasswordChanged="OnPasswordChanged"/>
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
private void OnPasswordChanged(object sender, RoutedEventArgs e)
{
    var passwordBox = sender as PasswordBox;
    string password = passwordBox.Password;
    
    if (string.IsNullOrEmpty(password))
    {
        passwordLayout.HasError = false;
        return;
    }
    
    bool hasMinLength = password.Length >= 8;
    bool hasUpper = password.Any(char.IsUpper);
    bool hasLower = password.Any(char.IsLower);
    bool hasDigit = password.Any(char.IsDigit);
    
    bool isValid = hasMinLength && hasUpper && hasLower && hasDigit;
    passwordLayout.HasError = !isValid;
}
```

### Required Field Validation

Validate that a field is not empty:

**XAML:**
```xml
<inputLayout:SfTextInputLayout 
    x:Name="nameLayout"
    Hint="Full Name"
    HelperText="Required field"
    ErrorText="Name is required">
    <TextBox LostFocus="OnNameLostFocus"/>
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
private void OnNameLostFocus(object sender, RoutedEventArgs e)
{
    var textBox = sender as TextBox;
    string name = textBox.Text;
    
    nameLayout.HasError = string.IsNullOrWhiteSpace(name);
}
```

### Numeric Range Validation

Validate numeric input within a range:

**XAML:**
```xml
<inputLayout:SfTextInputLayout 
    x:Name="ageLayout"
    Hint="Age"
    HelperText="Enter age between 18 and 100"
    ErrorText="Age must be between 18 and 100">
    <TextBox TextChanged="OnAgeTextChanged"/>
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
private void OnAgeTextChanged(object sender, TextChangedEventArgs e)
{
    var textBox = sender as TextBox;
    string ageText = textBox.Text;
    
    if (string.IsNullOrEmpty(ageText))
    {
        ageLayout.HasError = false;
        return;
    }
    
    if (int.TryParse(ageText, out int age))
    {
        ageLayout.HasError = age < 18 || age > 100;
    }
    else
    {
        ageLayout.HasError = true;
        ageLayout.ErrorText = "Please enter a valid number";
    }
}
```

### Form Validation Pattern

Validate entire form before submission:

```csharp
private bool ValidateForm()
{
    bool isValid = true;
    
    // Validate name
    if (string.IsNullOrWhiteSpace(nameTextBox.Text))
    {
        nameLayout.HasError = true;
        nameLayout.ErrorText = "Name is required";
        isValid = false;
    }
    else
    {
        nameLayout.HasError = false;
    }
    
    // Validate email
    if (!IsValidEmail(emailTextBox.Text))
    {
        emailLayout.HasError = true;
        emailLayout.ErrorText = "Invalid email format";
        isValid = false;
    }
    else
    {
        emailLayout.HasError = false;
    }
    
    // Validate password
    if (!IsValidPassword(passwordBox.Password))
    {
        passwordLayout.HasError = true;
        passwordLayout.ErrorText = "Password does not meet requirements";
        isValid = false;
    }
    else
    {
        passwordLayout.HasError = false;
    }
    
    return isValid;
}

private void OnSubmitClicked(object sender, RoutedEventArgs e)
{
    if (ValidateForm())
    {
        // Process form submission
        MessageBox.Show("Form submitted successfully!");
    }
}
```

## Best Practices

### Helper Text Guidelines

**Do:**
- ✅ Keep it concise (one line if possible)
- ✅ Use clear, simple language
- ✅ Explain what to do, not what not to do
- ✅ Provide examples when format is complex
- ✅ Update helper text when requirements change

**Don't:**
- ❌ Use jargon or technical terms users won't understand
- ❌ Write long paragraphs
- ❌ State the obvious ("Enter your name" for a field labeled "Name")
- ❌ Use helper text for error messages (use ErrorText instead)

### Error Message Guidelines

**Do:**
- ✅ Be specific about what's wrong
- ✅ Suggest how to fix it
- ✅ Use friendly, helpful tone
- ✅ Show errors immediately for real-time validation
- ✅ Clear errors when input becomes valid

**Don't:**
- ❌ Blame the user ("You entered an invalid...")
- ❌ Use technical error codes
- ❌ Be vague ("Invalid input")
- ❌ Show errors before user interacts with field
- ❌ Keep errors showing after issue is resolved

### Character Counter Guidelines

**When to Use:**
- ✅ Fields with strict length limits (Twitter-style posts)
- ✅ Form fields with database constraints
- ✅ Text areas where brevity is encouraged
- ✅ Username/handle fields with max length

**When Not to Use:**
- ❌ Fields with very high limits (500+ characters)
- ❌ Fields where length doesn't matter
- ❌ Password fields (use helper text instead)

### Validation Timing

**Real-Time (onChange):**
- ✅ Format validation (email, phone)
- ✅ Character limits
- ✅ Restricted characters

**On Blur (onLostFocus):**
- ✅ Required fields
- ✅ Complex validation rules
- ✅ Server-side validation

**On Submit:**
- ✅ Final form validation
- ✅ Cross-field validation
- ✅ Business logic validation

## Complete Example: Registration Form

```xml
<StackPanel Margin="40" Width="400">
    
    <!-- Username -->
    <inputLayout:SfTextInputLayout 
        x:Name="usernameLayout"
        Hint="Username"
        HelperText="3-20 characters, letters and numbers only"
        ErrorText="Invalid username"
        CharMaxLength="20"
        CharCountVisibility="Visible"
        Margin="0,0,0,20">
        <TextBox x:Name="usernameTextBox" TextChanged="OnUsernameChanged"/>
    </inputLayout:SfTextInputLayout>
    
    <!-- Email -->
    <inputLayout:SfTextInputLayout 
        x:Name="emailLayout"
        Hint="Email"
        HelperText="We'll never share your email"
        ErrorText="Invalid email format"
        Margin="0,0,0,20">
        <TextBox x:Name="emailTextBox" TextChanged="OnEmailChanged"/>
    </inputLayout:SfTextInputLayout>
    
    <!-- Password -->
    <inputLayout:SfTextInputLayout 
        x:Name="passwordLayout"
        Hint="Password"
        HelperText="Minimum 8 characters with uppercase, lowercase, and number"
        ErrorText="Password does not meet requirements"
        Margin="0,0,0,20">
        <PasswordBox x:Name="passwordBox" PasswordChanged="OnPasswordChanged"/>
    </inputLayout:SfTextInputLayout>
    
    <!-- Bio -->
    <inputLayout:SfTextInputLayout 
        x:Name="bioLayout"
        Hint="Bio"
        HelperText="Tell us about yourself"
        CharMaxLength="200"
        CharCountVisibility="Visible"
        Margin="0,0,0,30">
        <TextBox AcceptsReturn="True" TextWrapping="Wrap" Height="80"/>
    </inputLayout:SfTextInputLayout>
    
    <Button Content="Create Account" Click="OnSubmitClicked"/>
    
</StackPanel>
```

## Related Topics

- **Customization**: Style error colors and appearance → [customization.md](customization.md)
- **Container Types**: Choose the right container style → [container-types.md](container-types.md)
- **Getting Started**: Basic setup and configuration → [getting-started.md](getting-started.md)

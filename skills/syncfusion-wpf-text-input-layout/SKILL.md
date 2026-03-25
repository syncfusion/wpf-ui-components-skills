---
name: syncfusion-wpf-text-input-layout
description: Implements the Syncfusion WPF SfTextInputLayout control to provide floating labels, assistive labels, and input validation UI for WPF text inputs. Use when adding floating labels, customizing input container styles, or showing validation/helper text.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF TextInputLayout 

The TextInputLayout control for WPF adds decorative elements and enhanced user experience to input controls by providing floating labels, icons, assistive labels, and various container styles on top of TextBox and other input views.

## When to Use This Skill

Use this skill when user need to:
- **Add floating labels** to TextBox or other input controls in WPF
- **Implement input validation UI** with error messages and visual feedback
- **Customize input containers** with outlined, filled, or minimal styles
- **Add helper text** and character counters to input fields
- **Include icons** (leading/trailing) in input layouts
- **Configure hint positioning** and float behaviors
- **Support multiple input types** (TextBox, PasswordBox, ComboBox, etc.)
- **Implement Material Design-style** input fields in WPF
- **Add assistive labels** for accessibility and usability
- **Customize colors and styling** for focused/unfocused/error states

## Component Overview

The **SfTextInputLayout** control enhances standard WPF input controls by wrapping them with decorative elements:

### Key Features
- **Floating Labels**: Animated hint labels that float above input when focused
- **Container Types**: Outlined (default), Filled, and None styles
- **Assistive Labels**: Helper text, error messages, and character counters
- **Custom Icons**: Leading and trailing views with inside/outside positioning
- **Multiple Input Views**: TextBox, PasswordBox, ComboBox, ComboBoxAdv, SfTextBoxExt
- **State-Based Styling**: Different colors for focused, unfocused, and error states
- **RTL Support**: Right-to-left text flow for internationalization
- **Extensive Customization**: Border thickness, corner radius, padding, and more

### Typical Use Cases
- Form input fields with validation
- Material Design-style interfaces
- User registration and login forms
- Data entry applications
- Search boxes with icons
- Password fields with visibility toggles
- Dropdown inputs with decorative containers

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly references
- Namespace imports and initialization
- Basic TextInputLayout setup
- Adding hint labels
- HintVisibility configuration
- Theme support with SfSkinManager

### Container Types and Layout
📄 **Read:** [references/container-types.md](references/container-types.md)
- ContainerType property overview
- Outlined container (default with rounded border)
- Filled container (with background and base line)
- None container (minimal styling)
- ContainerBackground customization
- OutlineCornerRadius configuration

### Assistive Labels and Validation
📄 **Read:** [references/assistive-labels.md](references/assistive-labels.md)
- Helper text with HelperText property
- HelperTextVisibility configuration
- Error messages with ErrorText property
- HasError property for validation state
- Character counter with CharMaxLength
- CharCountVisibility configuration
- Validation patterns and best practices

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)
- FocusedForeground for focused state colors
- Foreground for unfocused state colors
- ErrorForeground for error state colors
- ActiveForeground property
- ContainerBackground customization
- FocusedBorderBrush for independent border colors
- StrokeThickness and FocusedStrokeThickness
- InputViewPadding configuration
- Color patterns and styling approaches

### Icons and Input Views
📄 **Read:** [references/icons-and-views.md](references/icons-and-views.md)
- LeadingView and LeadingViewPosition configuration
- TrailingView and TrailingViewPosition configuration
- Icon implementation with font icons
- Supported input views: TextBox, PasswordBox, ComboBox
- ComboBoxAdv integration
- SfTextBoxExt (Autocomplete) support
- DropDownBorder customization
- InputView property usage

### Hint Configuration
📄 **Read:** [references/hint-configuration.md](references/hint-configuration.md)
- HintFloatMode property options
- Float mode (default behavior)
- AlwaysFloat mode (always at top)
- None mode (hidden when focused)
- Hint animation and positioning
- Best practices for hint labels

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- RTL support with FlowDirection property
- Right-to-left text flow configuration
- Dropdown border customization
- Complete API reference summary
- Performance considerations
- Best practices and patterns
- Troubleshooting common issues

## Quick Start Example

### Basic TextInputLayout with Hint

**XAML:**
```xml
<Window xmlns:inputLayout="clr-namespace:Syncfusion.UI.Xaml.TextInputLayout;assembly=Syncfusion.SfTextInputLayout.WPF">
    
    <inputLayout:SfTextInputLayout 
        Hint="Name" 
        HelperText="Enter your full name">
        <TextBox />
    </inputLayout:SfTextInputLayout>
    
</Window>
```

**C#:**
```csharp
using Syncfusion.UI.Xaml.TextInputLayout;

var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.HelperText = "Enter your full name";
inputLayout.InputView = new TextBox();
```

### With Validation and Error State

**XAML:**
```xml
<inputLayout:SfTextInputLayout 
    Hint="Email" 
    HelperText="Enter a valid email address"
    ErrorText="Invalid email format"
    HasError="True"
    ErrorForeground="Red">
    <TextBox Text="invalid@"/>
</inputLayout:SfTextInputLayout>
```

### Outlined Container with Icon

**XAML:**
```xml
<inputLayout:SfTextInputLayout 
    Hint="Search" 
    ContainerType="Outlined"
    FocusedForeground="Blue">
    <TextBox />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Content="🔍" FontSize="16"/>
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

## Common Patterns

### Form Field with Validation Pattern

Use this pattern for validated form inputs:

```xml
<StackPanel>
    <!-- Text Input with Helper -->
    <inputLayout:SfTextInputLayout 
        Hint="Username" 
        HelperText="At least 5 characters"
        CharMaxLength="20"
        CharCountVisibility="Visible">
        <TextBox TextChanged="ValidateUsername"/>
    </inputLayout:SfTextInputLayout>
    
    <!-- Password Input -->
    <inputLayout:SfTextInputLayout 
        Hint="Password" 
        HelperText="Must contain letters and numbers">
        <PasswordBox PasswordChanged="ValidatePassword"/>
    </inputLayout:SfTextInputLayout>
</StackPanel>
```

```csharp
private void ValidateUsername(object sender, TextChangedEventArgs e)
{
    var textBox = sender as TextBox;
    var layout = FindParent<SfTextInputLayout>(textBox);
    
    if (textBox.Text.Length < 5)
    {
        layout.HasError = true;
        layout.ErrorText = "Username too short";
    }
    else
    {
        layout.HasError = false;
    }
}
```

### Material Design Style Pattern

Create Material Design-style inputs:

```xml
<inputLayout:SfTextInputLayout 
    Hint="Email Address" 
    ContainerType="Filled"
    FocusedForeground="#6200EE"
    ContainerBackground="#F5F5F5"
    HelperText="We'll never share your email">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

### Search Box with Icons Pattern

Implement search inputs with leading/trailing icons:

```xml
<inputLayout:SfTextInputLayout 
    Hint="Search..." 
    ContainerType="Outlined"
    OutlineCornerRadius="20">
    <TextBox TextChanged="OnSearchTextChanged"/>
    
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Content="🔍" FontSize="18"/>
    </inputLayout:SfTextInputLayout.LeadingView>
    
    <inputLayout:SfTextInputLayout.TrailingView>
        <Button Content="✕" Click="ClearSearch" 
                Background="Transparent" BorderThickness="0"/>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

### Dropdown with Outlined Container Pattern

Enhance ComboBox with TextInputLayout:

```xml
<inputLayout:SfTextInputLayout 
    Hint="Country" 
    ContainerType="Outlined"
    FocusedBorderBrush="Blue">
    <ComboBox ItemsSource="{Binding Countries}" 
              SelectedItem="{Binding SelectedCountry}"/>
</inputLayout:SfTextInputLayout>
```

## Key Properties

### Core Configuration
- **InputView**: The input control (TextBox, PasswordBox, etc.)
- **Hint**: Floating label text
- **HintVisibility**: Control hint label visibility
- **HintFloatMode**: Float, AlwaysFloat, or None
- **ContainerType**: Outlined, Filled, or None

### Assistive Labels
- **HelperText**: Additional guidance text
- **ErrorText**: Error message text
- **HasError**: Boolean to show error state
- **CharMaxLength**: Maximum character limit
- **CharCountVisibility**: Show/hide character counter

### Styling Properties
- **FocusedForeground**: Color when focused
- **Foreground**: Color when unfocused
- **ErrorForeground**: Color in error state
- **ContainerBackground**: Background color
- **FocusedBorderBrush**: Border color when focused

### Border and Stroke
- **StrokeThickness**: Unfocused border thickness (default: 1)
- **FocusedStrokeThickness**: Focused border thickness (default: 2)
- **OutlineCornerRadius**: Corner radius for outlined borders

### Icons and Views
- **LeadingView**: Icon/view on the left
- **LeadingViewPosition**: Inside or Outside
- **TrailingView**: Icon/view on the right
- **TrailingViewPosition**: Inside or Outside

## Common Use Cases

### User Registration Form
Combine multiple TextInputLayout controls with validation for username, email, password, and other fields with real-time validation feedback.

### Login Screen
Create clean, Material Design-style login forms with email/username and password inputs with floating labels and error states.

### Search Interfaces
Implement search boxes with icons, placeholder hints, and clear buttons using leading/trailing views.

### Data Entry Forms
Build comprehensive forms with helper text, character limits, and validation messages for improved user experience.

### Settings Panels
Use TextInputLayout with various input types (TextBox, ComboBox, PasswordBox) for consistent styling in settings interfaces.

### Profile Editing
Create profile edit forms with validation, character counters for bio fields, and proper error handling.

## Related Components

- **TextBox**: Primary input view for text
- **PasswordBox**: For password inputs
- **ComboBox/ComboBoxAdv**: For dropdown selections
- **SfTextBoxExt**: For autocomplete functionality

## Notes

- The hint label automatically animates based on focus state
- Error validation logic must be implemented at application level
- Character counter shows error color when limit exceeded
- FocusedBorderBrush overrides FocusedForeground for borders
- DropDownBorder only applies to ComboBox/ComboBoxAdv
- OutlineCornerRadius only applies to Outlined container type

---

**Next Steps:** Navigate to the reference documentation above based on your implementation needs. Start with `getting-started.md` for initial setup, then explore specific features as needed.

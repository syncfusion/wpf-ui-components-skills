# Getting Started with WPF TextInputLayout

This guide covers the initial setup and basic implementation of the Syncfusion WPF TextInputLayout (SfTextInputLayout) control.

## Installation and Assembly References

### Adding TextInputLayout Reference

**Method 1: NuGet Package Manager**

Install the package using the Package Manager Console:

```powershell
Install-Package Syncfusion.SfTextInputLayout.WPF
```

**Method 2: Visual Studio NuGet Package Manager**

1. Right-click on your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.SfTextInputLayout.WPF"
4. Click Install

**Method 3: Syncfusion Control Panel**

Install the complete Syncfusion WPF suite using the Syncfusion Control Panel installer.

### Required Assemblies

The TextInputLayout control requires the following assembly:
- `Syncfusion.SfTextInputLayout.WPF.dll`

This assembly will be automatically referenced when you install the NuGet package.

## Namespace Imports

### XAML Namespace

Add the namespace to your XAML file:

```xml
<Window xmlns:inputLayout="clr-namespace:Syncfusion.UI.Xaml.TextInputLayout;assembly=Syncfusion.SfTextInputLayout.WPF">
    <!-- Your content here -->
</Window>
```

**Alternative using Syncfusion schema:**

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <!-- Your content here -->
</Window>
```

### C# Namespace

Add the using directive in your C# code:

```csharp
using Syncfusion.UI.Xaml.TextInputLayout;
```

## Initialize TextInputLayout

### Basic Initialization in XAML

Create a basic TextInputLayout with a TextBox:

```xml
<inputLayout:SfTextInputLayout>
    <TextBox/>
</inputLayout:SfTextInputLayout>
```

### Basic Initialization in C#

Create a TextInputLayout programmatically:

```csharp
SfTextInputLayout inputLayout = new SfTextInputLayout();
inputLayout.InputView = new TextBox();

// Add to your container (e.g., Grid, StackPanel)
myContainer.Children.Add(inputLayout);
```

## Adding Hint Labels

The hint (floating label) is the most common feature of TextInputLayout.

### Setting the Hint Property

**XAML:**
```xml
<inputLayout:SfTextInputLayout Hint="Name">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.InputView = new TextBox();
```

### Hint Behavior

When the user focuses on the input view:
1. The hint label animates and moves to the top
2. It remains at the top while the input has focus
3. If the user unfocuses without entering text, the hint returns to its original position
4. If text is entered, the hint stays at the top even when unfocused

**Visual Example:**

```
Unfocused (empty):
┌─────────────────┐
│ Name            │  ← Hint in center
└─────────────────┘

Focused (empty):
Name                ← Hint floated to top
┌─────────────────┐
│ |               │  ← Cursor visible
└─────────────────┘

With Text:
Name                ← Hint stays at top
┌─────────────────┐
│ John            │  ← Text entered
└─────────────────┘
```

## HintVisibility Configuration

Control the visibility of the hint label using the `HintVisibility` property.

### Setting Hint Visibility

```xml
<inputLayout:SfTextInputLayout 
    Hint="Name"
    HintVisibility="Visible">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**Options:**
- `Visible`: Hint is displayed (default)
- `Collapsed`: Hint is hidden
- `Hidden`: Hint is hidden but space is reserved

### When to Hide Hints

You might hide the hint when:
- Using only helper text for guidance
- Implementing custom label solutions
- Creating minimal UI designs

## Complete Basic Example

Here's a complete example with multiple input layouts:

**XAML:**
```xml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:inputLayout="clr-namespace:Syncfusion.UI.Xaml.TextInputLayout;assembly=Syncfusion.SfTextInputLayout.WPF"
        Title="TextInputLayout Demo" Height="300" Width="400">
    
    <StackPanel Margin="20" VerticalAlignment="Center">
        
        <!-- Name Input -->
        <inputLayout:SfTextInputLayout 
            Hint="Name"
            Margin="0,0,0,20">
            <TextBox />
        </inputLayout:SfTextInputLayout>
        
        <!-- Email Input -->
        <inputLayout:SfTextInputLayout 
            Hint="Email"
            Margin="0,0,0,20">
            <TextBox />
        </inputLayout:SfTextInputLayout>
        
        <!-- Password Input -->
        <inputLayout:SfTextInputLayout 
            Hint="Password">
            <PasswordBox />
        </inputLayout:SfTextInputLayout>
        
    </StackPanel>
    
</Window>
```

**C# Code-Behind:**
```csharp
using System.Windows;
using Syncfusion.UI.Xaml.TextInputLayout;
using System.Windows.Controls;

namespace MyApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

## Theme Support

TextInputLayout supports Syncfusion's theming system for consistent styling across your application.

### Applying Themes with SfSkinManager

**Install the theme assembly:**
```powershell
Install-Package Syncfusion.Themes.MaterialLight.WPF
```

**Apply theme in XAML:**
```xml
<Window xmlns:syncfusionThemes="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionThemes:SfSkinManager.Theme="{syncfusionThemes:SkinManagerExtension ThemeName=MaterialLight}">
    
    <inputLayout:SfTextInputLayout Hint="Name">
        <TextBox />
    </inputLayout:SfTextInputLayout>
    
</Window>
```

**Apply theme in C#:**
```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

### Available Themes

Syncfusion provides multiple built-in themes:
- **MaterialLight** / **MaterialDark**
- **FluentLight** / **FluentDark**
- **Office2019Colorful** / **Office2019White** / **Office2019Black**
- **Office2016Colorful** / **Office2016White** / **Office2016DarkGray**
- And more...

### Creating Custom Themes

Use Syncfusion Theme Studio to create custom color schemes:
https://help.syncfusion.com/wpf/themes/theme-studio

## Next Steps

Now that you have the basic TextInputLayout set up, explore these features:

1. **Container Types**: Learn about Outlined, Filled, and None containers → [container-types.md](container-types.md)
2. **Assistive Labels**: Add helper text, error messages, and character counters → [assistive-labels.md](assistive-labels.md)
3. **Customization**: Customize colors, borders, and styling → [customization.md](customization.md)
4. **Icons**: Add leading and trailing icons → [icons-and-views.md](icons-and-views.md)

## Common Issues

### Issue: Namespace not recognized

**Problem:** XAML shows error for `inputLayout` namespace.

**Solution:** 
- Verify the NuGet package is installed
- Check that the assembly name in the namespace declaration matches your installed version
- Clean and rebuild the solution

### Issue: Hint doesn't float

**Problem:** Hint label doesn't animate to the top when focused.

**Solution:**
- Ensure `HintFloatMode` is set to `Float` (default)
- Check that `HintVisibility` is set to `Visible`
- Verify the InputView is properly set

### Issue: Theme not applying

**Problem:** Theme doesn't affect the TextInputLayout appearance.

**Solution:**
- Install the appropriate theme assembly via NuGet
- Apply the theme to the Window or parent container
- Ensure SfSkinManager assembly is referenced

## Related Resources

- **API Reference**: [SfTextInputLayout Class Documentation](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.TextInputLayout.SfTextInputLayout.html)
- **Syncfusion WPF Documentation**: https://help.syncfusion.com/wpf/
- **Sample Browser**: Install via Syncfusion Control Panel

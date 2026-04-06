# Getting Started with WPF Rating (SfRating)

This guide covers the complete setup and basic implementation of the Syncfusion WPF Rating control, including installation, adding the control to your application, and configuring essential properties.

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Creating Application with SfRating Control](#creating-application-with-sfrating-control)
- [Basic Configuration](#basic-configuration)
- [ValueChanged Event](#valuechanged-event)
- [Methods](#methods)
- [Theme Support](#theme-support)
- [Complete Example](#complete-example)
- [Troubleshooting](#troubleshooting)

## Assembly Deployment

The SfRating control requires the following assemblies:

- **Syncfusion.SfInput.WPF** - Contains the SfRating control
- **Syncfusion.SfShared.WPF** - Shared components and utilities

### NuGet Package Installation

Install the SfRating control using NuGet Package Manager:

1. Open your WPF project in Visual Studio
2. Navigate to **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
3. Search for `Syncfusion.SfInput.WPF`
4. Select the package and click **Install**

Alternatively, use the Package Manager Console:

```powershell
Install-Package Syncfusion.SfInput.WPF
```

The NuGet package automatically includes all required dependencies.

For more details on NuGet package installation, see: [How to install nuget packages](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages)

## Creating Application with SfRating Control

You can add the SfRating control to your WPF application using three methods:
1. Via Visual Studio Designer
2. Manually in XAML
3. Manually in C# code

### Method 1: Adding Control via Designer

The easiest way to add SfRating to your application:

1. Open your WPF window or user control in the Visual Studio Designer
2. Open the **Toolbox** (View → Toolbox or Ctrl+Alt+X)
3. Locate **SfRating** in the Syncfusion Controls section
4. Drag and drop the control onto your design surface
5. Visual Studio automatically adds the required assembly references and namespace declarations

The designer automatically generates the XAML code with the proper namespace:

```xml
<syncfusion:SfRating ItemsCount="5" Width="150"/>
```

### Method 2: Adding Control Manually in XAML

For manual XAML implementation:

1. **Add assembly references** to your project:
   - Syncfusion.SfShared.WPF
   - Syncfusion.SfInput.WPF

2. **Import the Syncfusion WPF schema** in your XAML file:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

3. **Declare the SfRating control**:

```xml
<Window x:Class="RatingApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="SfRating Application" Height="450" Width="800">
    
    <Grid>
        <syncfusion:SfRating ItemsCount="5" Width="150"/>
    </Grid>
</Window>
```

### Method 3: Adding Control Manually in C#

To create and configure SfRating programmatically:

1. **Add assembly references** to your project (same as XAML method)

2. **Import the namespace** in your code-behind file:

```csharp
using Syncfusion.Windows.Controls.Input;
```

3. **Create and add the SfRating instance**:

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace RatingApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create an instance of SfRating control
            SfRating rating = new SfRating()
            {
                ItemsCount = 5,
                Width = 150
            };
            
            // Add SfRating as window content
            this.Content = rating;
        }
    }
}
```

## Basic Configuration

### Customize Number of Rating Items

Use the `ItemsCount` property to set how many rating items (stars) to display. The default value is 0, so you must set this property.

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" Width="150"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Width = 150
};
```

Common configurations:
- **5 items** - Standard star rating (most common)
- **10 items** - Extended rating scale
- **3 items** - Simple satisfaction scale (poor/average/good)

### Set Rating Value

The `Value` property sets the current rating. The default value is 0.

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" Value="3" Width="150"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 3,
    Width = 150
};
```

The Value property:
- Accepts any double value from 0 to ItemsCount
- Can be set programmatically or through data binding
- Updates automatically when user clicks rating items
- Respects the Precision mode (Standard, Half, or Exact)

### Basic Precision Configuration

The `Precision` property determines the rating accuracy. The default is `Standard` (full item increments).

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     Precision="Exact" 
                     Width="150"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Precision = Syncfusion.Windows.Primitives.Precision.Standard,
    Width = 150
};
```

Available precision modes:
- **Standard** - Full item increments (1, 2, 3, 4, 5)
- **Half** - Half item increments (0.5, 1.0, 1.5, 2.0, etc.)
- **Exact** - Any value (precise to decimal places)

For detailed information on precision modes, see [precision.md](precision.md).

## ValueChanged Event

The `ValueChanged` event fires whenever the rating value changes, providing both the old and new values.

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="3"
                     Width="150"
                     ValueChanged="SfRating_ValueChanged"/>
```

**C# Event Handler:**
```csharp
private void SfRating_ValueChanged(object sender, ValueChangedEventArgs e)
{
    var oldValue = e.OldValue;
    var newValue = e.NewValue;
    MessageBox.Show($"Rating changed from {oldValue} to {newValue}");
}
```

**Programmatic Event Subscription:**
```csharp
SfRating rating = new SfRating();
rating.ItemsCount = 5;
rating.Value = 3;
rating.Width = 150;
rating.ValueChanged += SfRating_ValueChanged;
this.Content = rating;
```

Use the ValueChanged event for:
- Saving rating values to a database
- Updating bound data models
- Triggering validation or business logic
- Providing user feedback
- Logging rating changes

## Methods

The SfRating control provides several methods for advanced scenarios:

### Dispose()

Releases all resources used by the SfRating control. Call this when the control is no longer needed.

```csharp
SfRating rating = new SfRating();
rating.ItemsCount = 5;
this.Content = rating;

// Release resources when no longer needed
rating.Dispose();
```

### GetContainerForItemOverride()

Creates and returns a new `SfRatingItem` container. This method is called internally when items are generated. Override in custom implementations:

```csharp
public class CustomSfRating : SfRating
{
    protected override DependencyObject GetContainerForItemOverride()
    {
        return base.GetContainerForItemOverride();
    }
}
```

### IsItemItsOwnContainerOverride(Object)

Determines whether the specified item is already a `SfRatingItem` container:

```csharp
public class CustomSfRating : SfRating
{
    protected override bool IsItemItsOwnContainerOverride(object item)
    {
        return base.IsItemItsOwnContainerOverride(item);
    }
}
```

### OnApplyTemplate()

Called when the control template is applied. Override to access and customize template parts:

```csharp
public class CustomSfRating : SfRating
{
    public override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        // Access and customize template parts here
    }
}
```

### PrepareContainerForItemOverride(DependencyObject, Object)

Prepares the `SfRatingItem` container by applying styles and bindings:

```csharp
public class CustomSfRating : SfRating
{
    protected override void PrepareContainerForItemOverride(
        DependencyObject element, object item)
    {
        base.PrepareContainerForItemOverride(element, item);
        
        // Custom container preparation
        if (element is SfRatingItem ratingItem)
        {
            ratingItem.RatedFill = new SolidColorBrush(Colors.Gold);
        }
    }
}
```

### Event Handling Methods

The following methods handle user interactions. Override for custom behavior:

- **OnKeyDown(KeyEventArgs)** - Handles keyboard input
- **OnManipulationDelta(ManipulationDeltaEventArgs)** - Handles touch gestures
- **OnMouseEnter(MouseEventArgs)** - Mouse enters control
- **OnMouseLeave(MouseEventArgs)** - Mouse leaves control
- **OnMouseLeftButtonDown(MouseButtonEventArgs)** - Mouse button pressed
- **OnMouseLeftButtonUp(MouseButtonEventArgs)** - Mouse button released

## Theme Support

The SfRating control supports Syncfusion's built-in themes for consistent styling across your application.

### Applying Themes with SfSkinManager

Use the SfSkinManager to apply themes:

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
    
    <Grid>
        <syncfusion:SfRating ItemsCount="5" Value="3" Width="150"/>
    </Grid>
</Window>
```

Available themes include:
- MaterialLight, MaterialDark
- Office2019Colorful, Office2019Black, Office2019White
- FluentLight, FluentDark
- And many more

For complete theme documentation, see: [Apply theme using SfSkinManager](https://help.syncfusion.com/wpf/themes/skin-manager)

### Creating Custom Themes

Create custom themes using ThemeStudio to match your brand:

1. Visit [ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio)
2. Customize colors, fonts, and styles
3. Export and apply to your application

## Complete Example

Here's a complete working example combining multiple features:

**MainWindow.xaml:**
```xml
<Window x:Class="RatingApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Rating Demo" Height="300" Width="400">
    
    <StackPanel Margin="20">
        <TextBlock Text="Rate this product:" FontSize="16" Margin="0,0,0,10"/>
        
        <syncfusion:SfRating x:Name="productRating"
                             ItemsCount="5"
                             Value="0"
                             Width="150"
                             ValueChanged="ProductRating_ValueChanged"/>
        
        <TextBlock x:Name="feedbackText" 
                   Margin="0,20,0,0"
                   FontSize="14"
                   Foreground="Green"/>
    </StackPanel>
</Window>
```

**MainWindow.xaml.cs:**
```csharp
using System.Windows;
using Syncfusion.UI.Xaml.Controls.Input;

namespace RatingApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void ProductRating_ValueChanged(object sender, ValueChangedEventArgs e)
        {
            feedbackText.Text = $"Thank you for rating: {e.NewValue} stars";
        }
    }
}
```

## Troubleshooting

### Control Not Appearing

**Problem:** SfRating doesn't show up in the application.

**Solutions:**
- Verify assembly references are added correctly
- Check that ItemsCount is set (default is 0)
- Ensure Width or HorizontalAlignment allows the control to render
- Verify the namespace declaration in XAML

### Designer Shows Error

**Problem:** Visual Studio designer shows an error.

**Solutions:**
- Rebuild the project
- Close and reopen the XAML file
- Check that the correct version of Syncfusion assemblies is referenced
- Try cleaning the solution (Build → Clean Solution)

### ValueChanged Not Firing

**Problem:** The ValueChanged event doesn't trigger.

**Solutions:**
- Ensure IsReadOnly is not set to True
- Verify the event handler is properly attached
- Check that the Value actually changed (same value won't trigger event)

---
name: syncfusion-wpf-busy-indicator
description: Comprehensive guide for implementing the Syncfusion WPF Busy Indicator (SfBusyIndicator) control in Windows Presentation Foundation applications. Use this skill when working with loading indicators, progress animations, or visual feedback during async and long-running operations in WPF. Covers IsBusy binding, AnimationType configuration, header customization, MVVM integration, and sizing with ViewboxHeight/ViewboxWidth.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Busy Indicator

Comprehensive guide for implementing the Syncfusion® WPF Busy Indicator control. The SfBusyIndicator provides visual feedback during long-running operations with over 37 built-in animations, customizable headers, and full MVVM support.

## When to Use This Skill

Use this skill when user need to:
- **Show loading indicators** during data fetching or processing operations
- **Display progress animations** while waiting for async operations to complete
- **Provide visual feedback** during long-running background tasks
- **Implement wait screens** for file operations, API calls, or database queries
- **Indicate busy status** in WPF applications with animated indicators
- **Customize loading UI** with headers, templates, and animation styles
- **Integrate with MVVM** patterns for command execution feedback
- **Configure animation types** from 37+ built-in animation options

The SfBusyIndicator is essential for improving user experience by providing clear visual feedback during operations that require users to wait.

## Component Overview

The **SfBusyIndicator** control provides a flexible and customizable way to display loading indicators in WPF applications:

### Key Features
- **37+ Built-in Animations** - Multiple animation styles including Flight, Spin, Ball, Box, and more
- **Animation Speed Control** - Adjustable animation speed for all animation types (except Fluent)
- **Custom Headers** - Display informative text below the animation
- **Header Templates** - Full template customization for header content
- **Flexible Sizing** - Control animation size with ViewboxHeight and ViewboxWidth
- **IsBusy Property** - Simple boolean toggle to show/hide the indicator
- **Theme Support** - Built-in theme integration with Syncfusion theme system
- **MVVM Ready** - Full data binding support for properties and commands
- **Extensible Methods** - Override methods for custom control behaviors

### Assembly and Namespace
- **Assembly:** `Syncfusion.SfBusyIndicator.WPF`
- **Namespace:** `Syncfusion.Windows.Controls.Notification`
- **NuGet Package:** `Syncfusion.SfBusyIndicator.WPF`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation via NuGet Package Manager
- Assembly references and namespace imports
- License configuration and registration
- Basic implementation in XAML and code-behind
- Theme setup and configuration
- Minimal working example
- GitHub sample repository links

### Animation Configuration
📄 **Read:** [references/animation-types.md](references/animation-types.md)
- AnimationType property overview
- Complete list of 37+ built-in animations
- AnimationSpeed property configuration
- Speed adjustment examples (normal, faster, slower)
- Platform-specific notes (Fluent animation behavior)
- Visual animation gallery reference
- XAML, C#, and VB code examples

### Display Control
📄 **Read:** [references/isbusy-property.md](references/isbusy-property.md)
- IsBusy property purpose and usage
- Controlling animation execution on/off
- Data binding to async operations
- MVVM pattern integration with commands
- RelayCommand and ICommand examples
- Triggering busy state from view models
- Common display control scenarios

### Header Customization
📄 **Read:** [references/header-customization.md](references/header-customization.md)
- Header property for simple text
- HeaderTemplate for advanced customization
- DataTemplate examples with styles
- Dynamic content binding
- Multi-line headers and formatting
- Foreground and font customization
- Layout and positioning options

### Sizing and Layout
📄 **Read:** [references/sizing.md](references/sizing.md)
- ViewboxHeight property configuration
- ViewboxWidth property configuration
- Responsive sizing strategies
- Container and layout considerations
- Maintaining aspect ratios
- Examples for different screen sizes
- Best practices for sizing

### Advanced Usage - Methods
📄 **Read:** [references/methods.md](references/methods.md)
- Dispose() method for resource cleanup
- Dispose(Boolean) protected overload
- OnApplyTemplate() for template initialization
- OnAnimationTypeChanged() for animation changes
- OnPropertyChanged() for property monitoring
- Custom control extension scenarios
- Template part access patterns
- Advanced customization examples

## Quick Start Example

### Basic Busy Indicator

```xml
<Window xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Controls.Notification;assembly=Syncfusion.SfBusyIndicator.WPF"
        x:Class="BusyIndicatorApp.MainWindow">
    <Grid Background="CornflowerBlue">
        <syncfusion:SfBusyIndicator IsBusy="True"
                                     AnimationType="Flight"
                                     Header="Loading..."
                                     Foreground="White"/>
    </Grid>
</Window>
```

### With Data Binding (MVVM)

```xml
<Grid>
    <syncfusion:SfBusyIndicator IsBusy="{Binding IsLoading}"
                                 AnimationType="DoubleCircle"
                                 Header="{Binding LoadingMessage}"/>
</Grid>
```

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    private bool _isLoading;
    public bool IsLoading
    {
        get => _isLoading;
        set { _isLoading = value; OnPropertyChanged(); }
    }
    
    private string _loadingMessage = "Please wait...";
    public string LoadingMessage
    {
        get => _loadingMessage;
        set { _loadingMessage = value; OnPropertyChanged(); }
    }
    
    public async Task LoadDataAsync()
    {
        IsLoading = true;
        LoadingMessage = "Loading data...";
        
        await Task.Delay(3000); // Simulate operation
        
        IsLoading = false;
    }
}
```

## Common Patterns

### Pattern 1: Async Operation with Busy Indicator

```csharp
public class DataViewModel : ViewModelBase
{
    private bool _isBusy;
    public bool IsBusy
    {
        get => _isBusy;
        set => SetProperty(ref _isBusy, value);
    }
    
    public ICommand LoadCommand { get; }
    
    public DataViewModel()
    {
        LoadCommand = new RelayCommand(async () => await LoadDataAsync());
    }
    
    private async Task LoadDataAsync()
    {
        IsBusy = true;
        try
        {
            var data = await _dataService.GetDataAsync();
            // Process data
        }
        finally
        {
            IsBusy = false;
        }
    }
}
```

### Pattern 2: Custom Animation Selection

```xml
<StackPanel>
    <ComboBox x:Name="AnimationSelector" SelectedIndex="0">
        <ComboBoxItem Content="Flight"/>
        <ComboBoxItem Content="DoubleCircle"/>
        <ComboBoxItem Content="Spin"/>
    </ComboBox>
    
    <syncfusion:SfBusyIndicator IsBusy="True"
                                 AnimationType="{Binding ElementName=AnimationSelector, 
                                                Path=SelectedItem.Content}"/>
</StackPanel>
```

### Pattern 3: Overlay Busy Indicator

```xml
<Grid>
    <!-- Main content -->
    <ContentControl Content="{Binding MainContent}"/>
    
    <!-- Overlay busy indicator -->
    <Grid Background="#80000000" 
          Visibility="{Binding IsBusy, Converter={StaticResource BoolToVisibilityConverter}}">
        <syncfusion:SfBusyIndicator IsBusy="{Binding IsBusy}"
                                     AnimationType="Ball"
                                     Header="Processing..."
                                     Foreground="White"
                                     ViewboxHeight="150"
                                     ViewboxWidth="150"/>
    </Grid>
</Grid>
```

### Pattern 4: Conditional Headers

```xml
<syncfusion:SfBusyIndicator IsBusy="{Binding IsProcessing}">
    <syncfusion:SfBusyIndicator.HeaderTemplate>
        <DataTemplate>
            <StackPanel>
                <TextBlock Text="{Binding CurrentOperation}" 
                          FontSize="14" FontWeight="Bold"/>
                <TextBlock Text="{Binding ProgressPercentage, StringFormat='Progress: {0}%'}"
                          FontSize="11" Margin="0,5,0,0"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfBusyIndicator.HeaderTemplate>
</syncfusion:SfBusyIndicator>
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **IsBusy** | bool | Controls whether the animation is displayed |
| **AnimationType** | AnimationTypes | Sets the animation style (Flight, Spin, etc.) |
| **AnimationSpeed** | double | Controls animation speed (not for Fluent type) |
| **Header** | object | Text or content displayed below animation |
| **HeaderTemplate** | DataTemplate | Custom template for header content |
| **ViewboxHeight** | double | Height of the animation viewbox |
| **ViewboxWidth** | double | Width of the animation viewbox |

## Key Methods

| Method | Description |
|--------|-------------|
| **Dispose()** | Releases managed and unmanaged resources |
| **Dispose(Boolean)** | Protected overload for resource cleanup |
| **OnApplyTemplate()** | Override to access template parts |
| **OnAnimationTypeChanged()** | Override to handle animation type changes |
| **OnPropertyChanged()** | Override to monitor property changes |

## Common Use Cases

### Data Loading Operations
Display a busy indicator while fetching data from APIs, databases, or file systems. Bind IsBusy to the loading state in your view model.

### File Operations
Show loading feedback during file uploads, downloads, or processing operations with informative headers indicating the current operation.

### Background Processing
Indicate that background tasks are running (calculations, image processing, batch operations) with appropriate animation types and messages.

### Application Initialization
Display a busy indicator during application startup while loading configuration, user settings, or initial data.

### Search and Filter Operations
Provide visual feedback while performing complex search or filter operations on large datasets.

### Navigation and Page Loading
Show busy indicators when navigating between views or loading page content in navigation-based applications.

## Best Practices

1. **Always use with async operations** - Ensure IsBusy is set before starting and cleared after completing operations
2. **Provide meaningful headers** - Use descriptive text to inform users what's happening
3. **Choose appropriate animations** - Select animations that match your application's theme and purpose
4. **Consider overlay approach** - Use semi-transparent overlays to prevent user interaction during busy states
5. **Handle exceptions properly** - Always reset IsBusy in finally blocks to avoid stuck indicators
6. **Use MVVM pattern** - Bind IsBusy to view model properties for better testability and maintainability
7. **Adjust sizing appropriately** - Size the indicator based on the importance and screen space available
8. **Theme consistently** - Use Syncfusion themes for consistent appearance across your application

## Performance Tips

- Avoid complex HeaderTemplates that may impact rendering performance
- Use appropriate animation types for your use case (simpler animations for frequent operations)
- Don't nest multiple busy indicators unnecessarily
- Consider caching animation resources when using many indicators

## Related Components

- **ProgressBar** - For determinate progress indication with percentage
- **Toast** - For brief notification messages
- **Message** - For user notifications with actions
- **Skeleton** - For content placeholder animations

**Next Steps:** Navigate to the reference files above based on your specific implementation needs. Start with `getting-started.md` for initial setup or jump directly to feature-specific references for advanced scenarios.

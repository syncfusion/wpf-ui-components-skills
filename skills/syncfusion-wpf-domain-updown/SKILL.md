---
name: syncfusion-wpf-domain-updown
description: Implements the Syncfusion WPF DomainUpDown (SfDomainUpDown) control for cycling through predefined item lists using spin buttons. Use this when adding domain selectors, item selectors with up/down buttons, or controls that navigate through collections using increment/decrement buttons in WPF applications. Covers item population, spin button alignment, styling, auto-reverse, and data binding with custom templates.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF DomainUpDown

Guide for implementing the Syncfusion® WPF DomainUpDown (SfDomainUpDown) control - a versatile input control that allows users to cycle through a predefined list of items using spin buttons (increment/decrement arrows).

## When to Use This Skill

Use this skill when you need to:
- **Implement a DomainUpDown control** for item selection with spin buttons
- **Populate predefined item lists** that users can cycle through
- **Customize spin button positioning** (left, right, or both sides)
- **Bind complex data objects** with custom display templates
- **Style and theme** the control appearance
- **Enable auto-reverse** cycling from max to min values
- **Handle mouse wheel** and keyboard navigation gestures
- **Create data-driven selectors** with MVVM pattern support

## Component Overview

The `SfDomainUpDown` control provides an elegant way for users to select values from a predefined list by clicking up/down buttons or using mouse wheel/keyboard navigation. Unlike numeric up-down controls, DomainUpDown works with any type of data - strings, objects, or custom types.

**Key Capabilities:**
- Data binding support with `ItemsSource`
- Custom content templates for complex objects
- Configurable spin button alignment (left, right, both)
- Smooth spin animations
- Auto-reverse cycling behavior
- Mouse wheel and keyboard gestures
- Extensive styling and theming options
- MVVM-friendly architecture

**Assemblies Required:**
- `Syncfusion.SfInput.WPF`
- `Syncfusion.SfShared.WPF`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install and set up the DomainUpDown control
- Add the control via designer or code
- Understand assembly dependencies
- Create your first basic implementation
- Configure the `Value` property
- Import necessary namespaces

### Data Population and Binding
📄 **Read:** [references/data-population.md](references/data-population.md)

When you need to:
- Populate the control with a list of items
- Bind data using `ItemsSource` property
- Create data models and ViewModels
- Use `ContentTemplate` for custom display
- Display complex objects with multiple properties
- Implement MVVM data binding patterns

### Spin Button Configuration
📄 **Read:** [references/spin-button-alignment.md](references/spin-button-alignment.md)

When you need to:
- Customize spin button positioning
- Use `SpinButtonsAlignment` property
- Place buttons on the right (default)
- Place buttons on the left
- Split buttons on both sides (decrement left, increment right)
- Choose appropriate alignment for your UI

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)

When you need to:
- Enable or disable spin animations
- Customize accent colors with `AccentBrush`
- Style up/down buttons with `UpDownStyle`
- Create custom control templates
- Modify borders, backgrounds, and foregrounds
- Apply fonts and layout customization
- Build comprehensive custom themes

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When you need to:
- Enable auto-reverse cycling behavior
- Handle mouse wheel scrolling
- Implement keyboard navigation
- Understand edge cases
- Optimize performance

## Quick Start Example

### Basic DomainUpDown with String Values

**XAML:**
```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation">
    <Grid>
        <syncfusion:SfDomainUpDown x:Name="domainUpDown" 
                                   Height="30" 
                                   Width="150" 
                                   Value="James">
            <syncfusion:SfDomainUpDown.ItemsSource>
                <x:Array Type="sys:String" xmlns:sys="clr-namespace:System;assembly=mscorlib">
                    <sys:String>Lucas</sys:String>
                    <sys:String>James</sys:String>
                    <sys:String>Jacob</sys:String>
                    <sys:String>Michael</sys:String>
                </x:Array>
            </syncfusion:SfDomainUpDown.ItemsSource>
        </syncfusion:SfDomainUpDown>
    </Grid>
</Window>
```

**C#:**
```csharp
using Syncfusion.Windows.Controls.Input;

SfDomainUpDown domainUpDown = new SfDomainUpDown();
domainUpDown.Height = 30;
domainUpDown.Width = 150;
domainUpDown.ItemsSource = new List<string> { "Lucas", "James", "Jacob", "Michael" };
domainUpDown.Value = "James";
this.Content = domainUpDown;
```

### Data-Bound DomainUpDown with Custom Template

**Model:**
```csharp
public class Employee
{
    public string Name { get; set; }
    public string Email { get; set; }
}
```

**ViewModel:**
```csharp
public class ViewModel
{
    public List<Employee> Employees { get; set; }
    
    public ViewModel()
    {
        Employees = new List<Employee>
        {
            new Employee { Name = "Lucas", Email = "lucas@syncfusion.com" },
            new Employee { Name = "James", Email = "james@syncfusion.com" },
            new Employee { Name = "Jacob", Email = "jacob@syncfusion.com" }
        };
    }
}
```

**XAML:**
```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown" 
                           Width="200" 
                           ItemsSource="{Binding Employees}">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="{Binding Name}" FontWeight="Bold" Margin="0,0,5,0"/>
                <TextBlock Text="{Binding Email}" Foreground="Gray"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

## Common Patterns

### Pattern 1: Simple Item Selection

**When:** User needs to select from a fixed list of simple values (strings, enums).

**Approach:**
1. Define items directly in XAML or populate `ItemsSource` in code
2. Set initial `Value` property
3. Handle value changes if needed

### Pattern 2: Data-Bound Object Selection with Display Template

**When:** User needs to select from a collection of complex objects with custom display.

**Approach:**
1. Create data model class
2. Populate collection in ViewModel
3. Bind `ItemsSource` to collection
4. Define `ContentTemplate` to display desired properties
5. Use data binding for MVVM compliance

### Pattern 3: Styled DomainUpDown with Custom Buttons

**When:** User needs a fully customized appearance matching application theme.

**Approach:**
1. Define custom styles in Resources
2. Use `UpDownStyle` property for button customization
3. Customize borders, backgrounds, and accents
4. Apply control template for comprehensive theming

### Pattern 4: Auto-Reverse List Cycling

**When:** User wants continuous cycling (max → min, min → max).

**Approach:**
1. Set `AutoReverse="True"`
2. Populate items normally
3. Cycling automatically wraps around boundaries

## Key Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `ItemsSource` | `IEnumerable` | Collection of items to display | Populate control with data |
| `Value` | `object` | Currently selected item | Get/set selected value |
| `ContentTemplate` | `DataTemplate` | Template for displaying items | Customize item appearance |
| `SpinButtonsAlignment` | `SpinButtonsAlignment` | Position of spin buttons (Left, Right, Both) | Control button placement |
| `AccentBrush` | `Brush` | Color for control hotspots | Theme customization |
| `EnableSpinAnimation` | `bool` | Enable/disable spin transitions | Control animation behavior |
| `AutoReverse` | `bool` | Enable cycling from max to min | Continuous list navigation |
| `UpDownStyle` | `Style` | Custom style for up/down buttons | Advanced button styling |

## Common Use Cases

### 1. Day of Week Selector
Select day names with cycling support:
```xml
<syncfusion:SfDomainUpDown AutoReverse="True" Value="Monday">
    <syncfusion:SfDomainUpDown.ItemsSource>
        <x:Array Type="sys:String">
            <sys:String>Monday</sys:String>
            <sys:String>Tuesday</sys:String>
            <!-- ... other days ... -->
        </x:Array>
    </syncfusion:SfDomainUpDown.ItemsSource>
</syncfusion:SfDomainUpDown>
```

### 2. Employee/Contact Selector
Display employee names from data source:
```xml
<syncfusion:SfDomainUpDown ItemsSource="{Binding Employees}">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}"/>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

### 3. Status/State Selector
Allow selection from predefined status values:
```csharp
domainUpDown.ItemsSource = new List<string> { "Draft", "Pending", "Approved", "Rejected" };
domainUpDown.Value = "Draft";
```

### 4. Custom Alignment Layout
Split increment/decrement buttons:
```xml
<syncfusion:SfDomainUpDown SpinButtonsAlignment="Both" 
                           ItemsSource="{Binding Items}"/>
```

## Best Practices

1. **Data Binding:** Use `ItemsSource` with ViewModels for MVVM compliance
2. **Templates:** Define `ContentTemplate` when displaying complex objects
3. **Initial Value:** Always set an initial `Value` from your `ItemsSource`
4. **AutoReverse:** Enable for seamless cycling in fixed lists
5. **Styling:** Use `AccentBrush` for simple theming, `UpDownStyle` for comprehensive customization
6. **Performance:** For large lists, consider virtualization alternatives (ComboBox may be better)
7. **Accessibility:** Ensure button contrast and provide keyboard navigation

## Troubleshooting

**Issue:** Items display as object type names instead of values  
**Solution:** Define a `ContentTemplate` to specify how items should be displayed

**Issue:** Spin buttons don't appear  
**Solution:** Verify assemblies are referenced and namespace is imported correctly

**Issue:** Value doesn't update on spin  
**Solution:** Ensure items exist in `ItemsSource` and `Value` is a valid item

**Issue:** Animation is jumpy or slow  
**Solution:** Set `EnableSpinAnimation="False"` or adjust system performance settings

For more detailed troubleshooting, refer to the specific reference documentation sections above.

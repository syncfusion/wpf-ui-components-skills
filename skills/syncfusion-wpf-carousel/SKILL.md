---
name: syncfusion-wpf-carousel
description: Implements the Syncfusion WPF Carousel for displaying items in rotating or custom path interfaces with navigation support. Use when building image carousels, rotating galleries, or custom-path item displays with templates and data binding.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Carousels

The Syncfusion WPF Carousel control provides a 3D circular interface for displaying and rotating through items with interactive navigation, data binding, custom paths, scaling, skewing, and opacity effects. It supports both standard circular paths and custom geometric paths for unique visual experiences.

## When to Use This Skill

Use this skill when you need to:
- Display items in a rotating circular 3D interface
- Create image galleries with carousel navigation
- Build rotating product viewers or portfolios
- Implement item browsing with keyboard/mouse navigation
- Create custom path animations for item display
- Apply visual effects (scaling, opacity, skewing) to carousel items
- Display data collections in an interactive rotating format
- Implement page-based item display with custom paths
- Create visual navigation experiences in WPF applications

## Component Overview

The Carousel control provides:
- **Standard Path Mode**: Circular 3D rotation with configurable radius
- **Custom Path Mode**: Define any geometric path for item positioning
- **Data Binding**: Bind to collections with full template support
- **Navigation**: Keyboard, mouse, scroll bar, and command-based navigation
- **Visual Effects**: Scaling, opacity, skewing, and rotation animations
- **Item Selection**: Single selection with programmatic control
- **Virtualization**: Efficient loading for large item collections
- **Customization**: Full template and style customization support

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet packages
- Adding Carousel via designer, XAML, or C#
- Basic configuration and setup
- First working example
- Required namespaces and assemblies

### Populating Items
📄 **Read:** [references/populating-items.md](references/populating-items.md)
- Adding items using CarouselItem
- Binding to collections with ItemsSource
- Customizing item appearance with ItemTemplate
- Using ItemContainerStyle and StyleSelector
- Virtualization for large datasets
- Item selection and selection events
- Flow direction (RTL support)

### Navigation Features
📄 **Read:** [references/navigation.md](references/navigation.md)
- Keyboard navigation (arrow keys, Home, End, Page Up/Down)
- Mouse wheel navigation
- Scroll bar navigation
- Navigation commands (SelectFirstItem, SelectNextItem, etc.)
- Programmatic navigation methods
- Looping items in custom path mode
- Navigation patterns and best practices

### Standard Path Mode
📄 **Read:** [references/standard-path.md](references/standard-path.md)
- Circular 3D path display (default mode)
- Configuring radius (RadiusX, RadiusY)
- Rotation speed and animation
- Scaling items (ScaleFraction, ScalingEnabled)
- Opacity effects (OpacityFraction, OpacityEnabled)
- Skewing items (SkewAngleX/Y, enabled properties)
- Standard path best practices

### Custom Path Mode
📄 **Read:** [references/custom-path.md](references/custom-path.md)
- Defining custom geometric paths
- Items per page configuration
- Global scaling with ScaleFraction
- Individual item scaling with ScaleFractions
- Global opacity with OpacityFraction
- Individual item opacity with OpacityFractions
- Global skewing effects
- Individual item skewing with fractions
- Complex path examples

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- TopItemPosition for selected item placement
- Rotation angle configuration
- Path geometry patterns and examples
- Performance optimization techniques
- Common pitfalls and solutions
- Troubleshooting guide

## Quick Start Example

### Basic Carousel with Data Binding

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <syncfusion:Carousel Name="carousel" 
                         Height="400" 
                         Width="600"
                         ItemsSource="{Binding Items}">
        <syncfusion:Carousel.ItemTemplate>
            <DataTemplate>
                <Border Height="100" 
                        Width="150" 
                        BorderBrush="Purple" 
                        BorderThickness="3"
                        Background="LightBlue"
                        CornerRadius="5">
                    <StackPanel HorizontalAlignment="Center" 
                                VerticalAlignment="Center">
                        <Image Source="{Binding ImageSource}" 
                               Height="60" 
                               Width="80"/>
                        <TextBlock Text="{Binding Name}" 
                                   FontWeight="Bold"
                                   Margin="0,5,0,0"/>
                    </StackPanel>
                </Border>
            </DataTemplate>
        </syncfusion:Carousel.ItemTemplate>
    </syncfusion:Carousel>
</Window>
```

```csharp
using Syncfusion.Windows.Shared;
using System.Collections.ObjectModel;

// ViewModel
public class ViewModel
{
    public ObservableCollection<CarouselItem> Items { get; set; }
    
    public ViewModel()
    {
        Items = new ObservableCollection<CarouselItem>
        {
            new CarouselItem { Name = "Item 1", ImageSource = "/Images/img1.png" },
            new CarouselItem { Name = "Item 2", ImageSource = "/Images/img2.png" },
            new CarouselItem { Name = "Item 3", ImageSource = "/Images/img3.png" }
        };
    }
}

// Model
public class CarouselItem
{
    public string Name { get; set; }
    public string ImageSource { get; set; }
}
```

## Common Patterns

### Pattern 1: Carousel with Scaling and Opacity Effects

```xml
<syncfusion:Carousel ItemsSource="{Binding Products}"
                     ScaleFraction="0.7"
                     ScalingEnabled="True"
                     OpacityFraction="0.5"
                     OpacityEnabled="True"
                     RotationSpeed="300">
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <Border Height="120" Width="150" 
                    Background="White" 
                    BorderBrush="Gray" 
                    BorderThickness="2">
                <Grid>
                    <Image Source="{Binding ProductImage}" Stretch="Uniform"/>
                    <TextBlock Text="{Binding ProductName}" 
                               VerticalAlignment="Bottom"
                               Background="#CC000000"
                               Foreground="White"
                               Padding="5"/>
                </Grid>
            </Border>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

### Pattern 2: Custom Linear Path with Paging

```xml
<syncfusion:Carousel VisualMode="CustomPath"
                     ItemsPerPage="5"
                     ItemsSource="{Binding Gallery}"
                     ScaleFraction="0.8"
                     OpacityFraction="0.3">
    <syncfusion:Carousel.Path>
        <Path Data="M0,100 L500,100" 
              Stroke="Blue" 
              StrokeThickness="2"/>
    </syncfusion:Carousel.Path>
    
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding}" 
                   Height="100" 
                   Width="100" 
                   Stretch="UniformToFill"/>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

### Pattern 3: Programmatic Navigation

```csharp
// Navigate to specific items
carousel.SelectFirstItem();
carousel.SelectLastItem();
carousel.SelectNextItem();
carousel.SelectPreviousItem();

// Navigate by pages
carousel.SelectNextPage();
carousel.SelectPreviousPage();

// Set selected item programmatically
carousel.SelectedIndex = 2;

// Using commands in XAML
<Button Content="Next" 
        Command="{Binding ElementName=carousel, Path=SelectNextItemCommand}"/>
<Button Content="Previous" 
        Command="{Binding ElementName=carousel, Path=SelectPreviousItemCommand}"/>
```

### Pattern 4: Selection Changed Event

```csharp
carousel.SelectionChanged += Carousel_SelectionChanged;

private void Carousel_SelectionChanged(DependencyObject d, 
                                       DependencyPropertyChangedEventArgs e)
{
    var oldItem = e.OldValue;
    var newItem = e.NewValue;
    
    // Get selected item details
    var selectedIndex = carousel.SelectedIndex;
    var selectedItem = carousel.SelectedItem;
    var selectedValue = carousel.SelectedValue;
    
    // Perform actions based on selection
    Debug.WriteLine($"Selected: {selectedItem}");
}
```

### Pattern 5: Custom Path with Individual Item Effects

```xml
<syncfusion:Carousel VisualMode="CustomPath"
                     ScalingEnabled="True"
                     OpacityEnabled="True"
                     ItemsSource="{Binding Items}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 Q250,200 500,0" 
              Stroke="Red" 
              StrokeThickness="1"/>
    </syncfusion:Carousel.Path>
    
    <!-- Individual scaling per position -->
    <syncfusion:Carousel.ScaleFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="0.5"/>
            <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
            <syncfusion:FractionValue Fraction="1" Value="0.5"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.ScaleFractions>
    
    <!-- Individual opacity per position -->
    <syncfusion:Carousel.OpacityFractions>
        <syncfusion:PathFractionCollection>
            <syncfusion:FractionValue Fraction="0" Value="0.3"/>
            <syncfusion:FractionValue Fraction="0.5" Value="1.0"/>
            <syncfusion:FractionValue Fraction="1" Value="0.3"/>
        </syncfusion:PathFractionCollection>
    </syncfusion:Carousel.OpacityFractions>
</syncfusion:Carousel>
```

## Key Properties

| Property | Description | Default |
|----------|-------------|---------|
| `VisualMode` | Display mode: `Standard` (circular) or `CustomPath` | `Standard` |
| `ItemsSource` | Collection to bind carousel items | `null` |
| `SelectedIndex` | Index of selected item | `-1` |
| `SelectedItem` | Currently selected item object | `null` |
| `RadiusX` | Horizontal radius (Standard mode) | `250` |
| `RadiusY` | Vertical radius (Standard mode) | `150` |
| `RotationSpeed` | Animation speed in milliseconds | `200` |
| `ScaleFraction` | Scale factor for non-selected items (0-1) | `0` |
| `ScalingEnabled` | Enable/disable scaling effects | `true` |
| `OpacityFraction` | Opacity for non-selected items (0-1) | `0` |
| `OpacityEnabled` | Enable/disable opacity effects | `true` |
| `ItemsPerPage` | Items visible per page (CustomPath only) | `-1` (all) |
| `EnableLooping` | Loop items in CustomPath mode | `false` |
| `EnableVirtualization` | Enable UI virtualization for performance | `false` |
| `Path` | Custom path geometry for CustomPath mode | `null` |

## Common Use Cases

1. **Image Gallery Viewer**: Display photos in rotating carousel with thumbnails
2. **Product Showcase**: E-commerce product browsing with 3D rotation
3. **Media Player**: Album art or video thumbnail carousel
4. **Dashboard Navigation**: Navigate through different dashboard views
5. **Portfolio Display**: Showcase work samples in artistic layouts
6. **Document Viewer**: Browse through document pages or previews
7. **Team Member Gallery**: Display team photos with biographical info
8. **Settings Panels**: Rotate through configuration options
9. **Tutorial Steps**: Navigate through step-by-step instructions
10. **Timeline Display**: Show chronological events in circular format

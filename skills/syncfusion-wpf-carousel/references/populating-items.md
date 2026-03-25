# Populating Items in WPF Carousel

## Table of Contents
- [Overview](#overview)
- [Populating Using CarouselItem](#populating-using-carouselitem)
- [Populating Using Collection Binding](#populating-using-collection-binding)
- [Custom UI with ItemTemplate](#custom-ui-with-itemtemplate)
- [Custom UI with ItemContainerStyle](#custom-ui-with-itemcontainerstyle)
- [Custom UI with ItemContainerStyleSelector](#custom-ui-with-itemcontainerstyleselector)
- [Virtualization Support](#virtualization-support)
- [Selecting Carousel Items](#selecting-carousel-items)
- [Selection Changed Notification](#selection-changed-notification)
- [Flow Direction](#flow-direction)

## Overview

The Carousel control provides multiple ways to populate and display items. You can add items directly using CarouselItem elements, bind to data collections, or use templates for custom item appearance. This guide covers all populating methods and item customization techniques.

## Populating Using CarouselItem

You can add carousel items directly using the `CarouselItem` element. This approach is suitable for static content or when you need fine-grained control over individual items.

### Basic CarouselItem Usage

```xml
<syncfusion:Carousel x:Name="carousel" 
                     Height="400" 
                     Width="600">
    <syncfusion:CarouselItem>
        <syncfusion:CarouselItem.Content>
            <Viewbox Height="100" Width="100">
                <Image Source="Images/Buchanan.png"/>
            </Viewbox>
        </syncfusion:CarouselItem.Content>
    </syncfusion:CarouselItem>
    
    <syncfusion:CarouselItem>
        <syncfusion:CarouselItem.Content>
            <Viewbox Height="100" Width="100">
                <Image Source="Images/Callahan.png"/>
            </Viewbox>
        </syncfusion:CarouselItem.Content>
    </syncfusion:CarouselItem>
    
    <syncfusion:CarouselItem>
        <syncfusion:CarouselItem.Content>
            <Viewbox Height="100" Width="100">
                <Image Source="Images/Davolio.png"/>
            </Viewbox>
        </syncfusion:CarouselItem.Content>
    </syncfusion:CarouselItem>
</syncfusion:Carousel>
```

### Adding CarouselItem in Code-Behind

```csharp
using System;
using System.Windows.Controls;
using System.Windows.Media.Imaging;
using Syncfusion.Windows.Shared;

Carousel carousel = new Carousel()
{ 
    Width = 600, 
    Height = 400
};

// Create images
Image image1 = new Image();
Image image2 = new Image();
Image image3 = new Image();

BitmapImage bitimg1 = new BitmapImage(new Uri("/Images/1.png", UriKind.RelativeOrAbsolute));
BitmapImage bitimg2 = new BitmapImage(new Uri("/Images/2.png", UriKind.RelativeOrAbsolute));
BitmapImage bitimg3 = new BitmapImage(new Uri("/Images/3.png", UriKind.RelativeOrAbsolute));

image1.Source = bitimg1;
image2.Source = bitimg2;
image3.Source = bitimg3;

// Add items to carousel
carousel.Items.Add(new CarouselItem() { Content = new Viewbox() { Child = image1 } });
carousel.Items.Add(new CarouselItem() { Content = new Viewbox() { Child = image2 } });
carousel.Items.Add(new CarouselItem() { Content = new Viewbox() { Child = image3 } });

// Add carousel to window
grid.Children.Add(carousel);
```

**When to use CarouselItem:**
- Static content that doesn't change
- Small number of items (< 10)
- Different visual appearance for each item
- Need direct control over individual item properties

## Populating Using Collection Binding

For dynamic content, bind the Carousel to a data collection using the `ItemsSource` property. This is the recommended approach for data-driven scenarios.

### Define Model and ViewModel

```csharp
// Model.cs
public class CarouselModel
{
    public string Header { get; set; }
    public string Description { get; set; }
    public string ImageSource { get; set; }
}

// ViewModel.cs
using System.Collections.ObjectModel;

public class ViewModel
{
    private ObservableCollection<CarouselModel> collection;
    
    public ObservableCollection<CarouselModel> HeaderCollection
    {
        get { return collection; }
        set { collection = value; }
    }
    
    public ViewModel()
    {
        HeaderCollection = new ObservableCollection<CarouselModel>();
        HeaderCollection.Add(new CarouselModel() 
        { 
            Header = "Buchanan",
            Description = "Sales Manager",
            ImageSource = "/Images/Buchanan.png"
        });
        HeaderCollection.Add(new CarouselModel() 
        { 
            Header = "Callahan",
            Description = "Inside Sales Coordinator",
            ImageSource = "/Images/Callahan.png"
        });
        HeaderCollection.Add(new CarouselModel() 
        { 
            Header = "Davolio",
            Description = "Sales Representative",
            ImageSource = "/Images/Davolio.png"
        });
        // Add more items...
    }
}
```

### Bind to Collection in XAML

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <syncfusion:Carousel Name="carousel"
                         ItemsSource="{Binding HeaderCollection}">
        <syncfusion:Carousel.ItemTemplate>
            <DataTemplate>
                <Border Height="50" 
                        Width="100" 
                        BorderBrush="Purple" 
                        BorderThickness="5"
                        Background="LightBlue">
                    <TextBlock HorizontalAlignment="Center" 
                               VerticalAlignment="Center" 
                               Text="{Binding Header}"/>
                </Border>
            </DataTemplate>
        </syncfusion:Carousel.ItemTemplate>
    </syncfusion:Carousel>
</Grid>
```

**Benefits of Collection Binding:**
- Dynamic data updates (add/remove items at runtime)
- Cleaner XAML code
- Separation of data and presentation
- MVVM pattern support
- Easy to populate from databases or APIs

## Custom UI with ItemTemplate

Use `ItemTemplate` to define how each carousel item should appear. The DataContext of the template is the bound data object.

### Simple Template Example

```xml
<syncfusion:Carousel ItemsSource="{Binding Products}">
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <Border Height="150" 
                    Width="200" 
                    Background="White"
                    BorderBrush="Gray" 
                    BorderThickness="2"
                    CornerRadius="5">
                <StackPanel Margin="10">
                    <Image Source="{Binding ProductImage}" 
                           Height="80" 
                           Width="80"
                           Stretch="Uniform"/>
                    <TextBlock Text="{Binding ProductName}" 
                               FontWeight="Bold"
                               FontSize="14"
                               HorizontalAlignment="Center"
                               Margin="0,5,0,0"/>
                    <TextBlock Text="{Binding Price, StringFormat=C}" 
                               Foreground="Green"
                               HorizontalAlignment="Center"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

### Complex Template with Multiple Elements

```xml
<syncfusion:Carousel ItemsSource="{Binding HeaderCollection}"
                     ScaleFraction="0.5">
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <Grid Width="180" Height="200">
                <!-- Background ellipse -->
                <Ellipse Width="200" 
                         Height="100" 
                         Stroke="Green" 
                         StrokeThickness="4"
                         Fill="Yellow"/>
                
                <!-- Content -->
                <StackPanel VerticalAlignment="Center">
                    <TextBlock Text="{Binding Header}" 
                               FontSize="18"
                               FontWeight="Bold"
                               HorizontalAlignment="Center"/>
                    <TextBlock Text="{Binding Description}" 
                               FontSize="12"
                               HorizontalAlignment="Center"
                               Margin="0,5,0,0"/>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

## Custom UI with ItemContainerStyle

Use `ItemContainerStyle` to customize the container element generated for each carousel item. This allows styling the CarouselItem itself rather than just its content.

### Basic Container Style

```xml
<Window.Resources>
    <Style x:Key="carouselItemStyle" TargetType="syncfusion:CarouselItem">
        <Setter Property="Background" Value="LightBlue"/>
        <Setter Property="BorderBrush" Value="Navy"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Padding" Value="5"/>
    </Style>
</Window.Resources>

<syncfusion:Carousel ItemsSource="{Binding Items}"
                     ItemContainerStyle="{StaticResource carouselItemStyle}">
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}" FontSize="16"/>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

### Advanced Container Style with Template

```xml
<Window.Resources>
    <Style x:Key="customItemStyle" TargetType="syncfusion:CarouselItem">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="syncfusion:CarouselItem">
                    <Grid>
                        <Border x:Name="border">
                            <Grid>
                                <!-- Custom background -->
                                <Ellipse Stroke="Red" 
                                         StrokeThickness="4"
                                         Fill="Yellow"/>
                                
                                <!-- Content area -->
                                <Border Margin="20">
                                    <Grid>
                                        <Grid.RowDefinitions>
                                            <RowDefinition Height="*"/>
                                            <RowDefinition Height="Auto"/>
                                        </Grid.RowDefinitions>
                                        
                                        <Image Source="{Binding ImageSource}" 
                                               Height="100" 
                                               Width="100"
                                               Grid.Row="0"/>
                                        
                                        <TextBlock Text="{Binding Header}"
                                                   FontWeight="Bold" 
                                                   Grid.Row="1"
                                                   HorizontalAlignment="Center"/>
                                    </Grid>
                                </Border>
                            </Grid>
                        </Border>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<syncfusion:Carousel ScaleFraction="0.5" 
                     ItemsSource="{Binding Persons}"
                     ItemContainerStyle="{StaticResource customItemStyle}"/>
```

## Custom UI with ItemContainerStyleSelector

Use `ItemContainerStyleSelector` to apply different styles to items based on custom logic. Useful when items should look different based on their data properties.

### Define StyleSelector Class

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Shared;

public class PersonStyleSelector : StyleSelector
{
    public Style ElderStyle { get; set; }
    public Style YoungerStyle { get; set; }
    
    public override Style SelectStyle(object item, DependencyObject container)
    {
        var model = item as CarouselModel;
        if (model != null && model.Age > 50)
            return ElderStyle;
        else
            return YoungerStyle;
    }
}
```

### Define Styles and Apply Selector

```xml
<Window.Resources>
    <!-- Style for elder persons -->
    <Style TargetType="syncfusion:CarouselItem" x:Key="elderStyle">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate>
                    <Grid>
                        <Ellipse Width="100" Height="50" 
                                 Stroke="Purple" 
                                 StrokeThickness="4"
                                 Fill="LightBlue"/>
                        <TextBlock HorizontalAlignment="Center" 
                                   VerticalAlignment="Center" 
                                   Text="{Binding Name}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    
    <!-- Style for younger persons -->
    <Style TargetType="syncfusion:CarouselItem" x:Key="youngerStyle">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate>
                    <Border Height="50" 
                            Width="100" 
                            BorderBrush="Yellow" 
                            BorderThickness="5"
                            Background="LightGreen">
                        <TextBlock HorizontalAlignment="Center" 
                                   VerticalAlignment="Center" 
                                   Text="{Binding Name}"/>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    
    <!-- StyleSelector -->
    <local:PersonStyleSelector x:Key="personStyleSelector"
                               ElderStyle="{StaticResource elderStyle}" 
                               YoungerStyle="{StaticResource youngerStyle}"/>
</Window.Resources>

<syncfusion:Carousel ItemContainerStyleSelector="{StaticResource personStyleSelector}"
                     ItemsSource="{Binding Persons}"
                     ScaleFraction="0.5"/>
```

## Virtualization Support

Enable UI virtualization to improve performance when loading large datasets. Virtualization loads only visible items instead of creating UI elements for all items.

### Enable Virtualization

```xml
<syncfusion:Carousel EnableVirtualization="True"
                     ItemsSource="{Binding LargeCollection}"
                     Name="carousel"/>
```

```csharp
carousel.EnableVirtualization = true;
```

**Benefits:**
- Faster initial load time
- Reduced memory consumption
- Smooth scrolling even with thousands of items
- Better application performance

**When to use:**
- Collections with 100+ items
- Performance-critical scenarios
- Memory-constrained environments
- Frequently changing large datasets

**Default Value:** `false`

## Selecting Carousel Items

The Carousel control supports single item selection through mouse clicks or programmatic methods.

### Get Selected Item Information

```csharp
// Get selected item object
var selectedItem = carousel.SelectedItem;

// Get selected item index
int selectedIndex = carousel.SelectedIndex;

// Get selected item value
var selectedValue = carousel.SelectedValue;
```

### Select Item Programmatically Using Property

```xml
<Window.Resources>
    <Style x:Key="selecteditemStyle" TargetType="syncfusion:CarouselItem">
        <Style.Triggers>
            <Trigger Property="IsSelected" Value="True">
                <Setter Property="Foreground" Value="Red"/>
                <Setter Property="FontWeight" Value="Bold"/>
            </Trigger>
        </Style.Triggers>
    </Style>
</Window.Resources>

<syncfusion:Carousel ItemContainerStyle="{StaticResource selecteditemStyle}" 
                     x:Name="carousel">
    <syncfusion:CarouselItem Content="Item1"/>
    <syncfusion:CarouselItem Content="Item2"/>
    <syncfusion:CarouselItem Content="Item3" IsSelected="True"/>
    <syncfusion:CarouselItem Content="Item4"/>
    <syncfusion:CarouselItem Content="Item5"/>
</syncfusion:Carousel>
```

### Select Using Commands and Methods

```csharp
// Select first item
carousel.SelectFirstItem();
// OR
carousel.SelectFirstItemCommand.Execute(null);

// Select last item
carousel.SelectLastItem();
// OR
carousel.SelectLastItemCommand.Execute(null);

// Select next item
carousel.SelectNextItem();
// OR
carousel.SelectNextItemCommand.Execute(null);

// Select previous item
carousel.SelectPreviousItem();
// OR
carousel.SelectPreviousItemCommand.Execute(null);

// Select by index
carousel.SelectedIndex = 2;
```

### Using Commands in XAML

```xml
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" Margin="10">
    <Button Content="First" 
            Command="{Binding ElementName=carousel, Path=SelectFirstItemCommand}"
            Margin="5"/>
    <Button Content="Previous" 
            Command="{Binding ElementName=carousel, Path=SelectPreviousItemCommand}"
            Margin="5"/>
    <Button Content="Next" 
            Command="{Binding ElementName=carousel, Path=SelectNextItemCommand}"
            Margin="5"/>
    <Button Content="Last" 
            Command="{Binding ElementName=carousel, Path=SelectLastItemCommand}"
            Margin="5"/>
</StackPanel>
```

## Selection Changed Notification

Handle the `SelectionChanged` event to respond when the selected item changes.

### XAML Event Handler

```xml
<syncfusion:Carousel SelectionChanged="Carousel_SelectionChanged"
                     ItemsSource="{Binding HeaderCollection}"
                     Name="carousel"/>
```

### Code-Behind Event Handler

```csharp
using System.Windows;
using Syncfusion.Windows.Shared;

// Subscribe to event
carousel.SelectionChanged += Carousel_SelectionChanged;

// Event handler
private void Carousel_SelectionChanged(DependencyObject d, 
                                       DependencyPropertyChangedEventArgs e)
{
    // Get old and new selected items
    var oldValue = e.OldValue;
    var newValue = e.NewValue;
    
    // Get selected item details
    var selectedIndex = carousel.SelectedIndex;
    var selectedItem = carousel.SelectedItem;
    
    // Perform actions based on selection
    if (newValue != null)
    {
        System.Diagnostics.Debug.WriteLine($"Selected: {selectedItem}");
        // Update UI, load details, etc.
    }
}
```

## Flow Direction

Change the carousel's layout direction for right-to-left (RTL) language support.

### Set Flow Direction

```xml
<syncfusion:Carousel FlowDirection="RightToLeft"
                     ItemsSource="{Binding Items}"
                     Name="carousel"/>
```

```csharp
carousel.FlowDirection = FlowDirection.RightToLeft;
```

**Options:**
- `LeftToRight` (default): Standard left-to-right layout
- `RightToLeft`: Reversed layout for RTL languages

**Default Value:** `LeftToRight`

**Use cases:**
- Applications supporting RTL languages (Arabic, Hebrew, etc.)
- Mirrored UI layouts
- Localization requirements

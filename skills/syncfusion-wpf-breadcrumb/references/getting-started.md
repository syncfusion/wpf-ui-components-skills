# Getting Started with WPF HierarchyNavigator

Setup and basic implementation of the `HierarchyNavigator` breadcrumb control.

## Table of Contents
- [Assembly Setup](#assembly-setup)
- [Adding the Control](#adding-the-control)
- [Adding Items Declaratively](#adding-items-declaratively)
- [Basic Data Binding](#basic-data-binding)
- [Applying Themes](#applying-themes)

---

## Assembly Setup

### Required Assemblies
Add these references to your WPF project:
- `Syncfusion.Tools.WPF`
- `Syncfusion.Shared.WPF`

### NuGet Package
```
Install-Package Syncfusion.Tools.WPF
```

### XAML Namespace
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### C# Namespace
```csharp
using Syncfusion.Windows.Tools.Controls;
```

---

## Adding the Control

### Via Designer
Drag `HierarchyNavigator` from the Toolbox onto the designer canvas. The required assemblies (`Syncfusion.Tools.WPF`, `Syncfusion.Shared.WPF`) are added automatically and this XAML is generated:

```xml
<syncfusion:HierarchyNavigator x:Name="hierarchyNavigator"
                               Width="100" Height="100"
                               VerticalAlignment="Center"
                               HorizontalAlignment="Center"/>
```

### Via XAML (Manual)
```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="MyApp.MainWindow"
        Title="HierarchyNavigator Sample" Height="350" Width="525">
  <Grid>
    <syncfusion:HierarchyNavigator x:Name="hierarchyNavigator"
                                   Width="600" Height="30"
                                   VerticalAlignment="Top"/>
  </Grid>
</Window>
```

### Via C#
```csharp
using Syncfusion.Windows.Tools.Controls;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        HierarchyNavigator navigator = new HierarchyNavigator();
        this.Content = navigator;
    }
}
```

---

## Adding Items Declaratively

Use `HierarchyNavigatorItem` elements to build a static hierarchy directly in XAML. Each item can have nested `.Items` for child levels.

### XAML
```xml
<syncfusion:HierarchyNavigator x:Name="navigator"
                               VerticalAlignment="Top"
                               Height="30" Width="600">
  <syncfusion:HierarchyNavigator.Items>
    <syncfusion:HierarchyNavigatorItem Content="Syncfusion">
      <syncfusion:HierarchyNavigatorItem.Items>
        <syncfusion:HierarchyNavigatorItem Content="User Interface"/>
        <syncfusion:HierarchyNavigatorItem Content="Silverlight">
          <syncfusion:HierarchyNavigatorItem.Items>
            <syncfusion:HierarchyNavigatorItem Content="Tools"/>
          </syncfusion:HierarchyNavigatorItem.Items>
        </syncfusion:HierarchyNavigatorItem>
      </syncfusion:HierarchyNavigatorItem.Items>
    </syncfusion:HierarchyNavigatorItem>
  </syncfusion:HierarchyNavigator.Items>
</syncfusion:HierarchyNavigator>
```

### C#
```csharp
HierarchyNavigator navigator = new HierarchyNavigator { Height = 30 };

HierarchyNavigatorItem root = new HierarchyNavigatorItem { Content = "Syncfusion" };

HierarchyNavigatorItem userInterface = new HierarchyNavigatorItem { Content = "User Interface" };
userInterface.Items.Add(new HierarchyNavigatorItem { Content = "Silverlight" });
userInterface.Items.Add(new HierarchyNavigatorItem { Content = "WPF" });
userInterface.Items.Add(new HierarchyNavigatorItem { Content = "ASP .Net" });
userInterface.Items.Add(new HierarchyNavigatorItem { Content = "MVC" });

root.Items.Add(userInterface);
navigator.Items.Add(root);
this.Content = navigator;
```

> **When to use declarative items:** Best for small, static hierarchies where the structure is known at design time. For dynamic or data-driven scenarios, use `ItemsSource` binding instead.

---

## Basic Data Binding

Bind an `ObservableCollection` to `ItemsSource` and use `HierarchicalDataTemplate` to map each level.

### Model
```csharp
public class HierarchyItem
{
    public string Name { get; set; }
    public ObservableCollection<HierarchyItem> Children { get; set; }

    public HierarchyItem(string name, params HierarchyItem[] children)
    {
        Name = name;
        Children = new ObservableCollection<HierarchyItem>(children);
    }
}
```

### Collection (acts as ItemsSource)
```csharp
public class HierarchyItemsSource : ObservableCollection<HierarchyItem>
{
    public HierarchyItemsSource()
    {
        this.Add(new HierarchyItem("Syncfusion",
            new HierarchyItem("User Interface",
                new HierarchyItem("Silverlight"),
                new HierarchyItem("WPF"),
                new HierarchyItem("ASP .Net"),
                new HierarchyItem("MVC")),
            new HierarchyItem("Reporting",
                new HierarchyItem("PDF Generator"),
                new HierarchyItem("IO"))));
    }
}
```

### XAML
```xml
<syncfusion:HierarchyNavigator Height="30" Width="600">
  <syncfusion:HierarchyNavigator.ItemsSource>
    <local:HierarchyItemsSource/>
  </syncfusion:HierarchyNavigator.ItemsSource>
  <syncfusion:HierarchyNavigator.ItemTemplate>
    <HierarchicalDataTemplate ItemsSource="{Binding Children}">
      <TextBlock Text="{Binding Name}" Margin="2,0"/>
    </HierarchicalDataTemplate>
  </syncfusion:HierarchyNavigator.ItemTemplate>
</syncfusion:HierarchyNavigator>
```

> **Key requirement:** Always use `HierarchicalDataTemplate` (not plain `DataTemplate`) when binding hierarchical data. The `ItemsSource` binding on the template tells the control how to retrieve child items for each level.

For more complex data binding scenarios (XML data, WCF services, on-demand loading), see [`references/populating-data.md`](populating-data.md).

---

## Applying Themes

`HierarchyNavigator` supports Syncfusion's `SfSkinManager` theme system.

### Apply theme to the whole window
```xml
<Window xmlns:skin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        skin:SfSkinManager.Theme="{skin:SkinManagerExtension ThemeName=FluentDark}">
  <Grid>
    <syncfusion:HierarchyNavigator Height="30" Width="600" .../>
  </Grid>
</Window>
```

For full appearance customization (item templates, Expression Blend customization, custom themes), see [`references/appearance-and-themes.md`](appearance-and-themes.md).

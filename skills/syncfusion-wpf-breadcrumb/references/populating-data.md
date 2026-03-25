# Populating Data in HierarchyNavigator

## Table of Contents
- [Binding Business Object Collections](#binding-business-object-collections)
- [HierarchicalDataTemplate](#hierarchicaldatatemplate)
- [Binding XML Data](#binding-xml-data)
- [Binding via WCF Service](#binding-via-wcf-service)
- [On-Demand Item Loading](#on-demand-item-loading)

---

## Binding Business Object Collections

The most common approach: create a model class with a children collection, build an `ObservableCollection`, and bind it to `ItemsSource`.

### Step 1 — Model class

```csharp
public class HierarchyItem
{
    public string ContentString { get; set; }

    public HierarchyItem(string content, params HierarchyItem[] children)
    {
        ContentString = content;
        HierarchyItems = new ObservableCollection<HierarchyItem>(children);
    }

    public ObservableCollection<HierarchyItem> HierarchyItems { get; set; }
}
```

### Step 2 — Collection (ItemsSource)

```csharp
public class HierarchicalItemsSource : ObservableCollection<HierarchyItem>
{
    public HierarchicalItemsSource()
    {
        this.Add(new HierarchyItem("Syncfusion",
            new HierarchyItem("User Interface",
                new HierarchyItem("Silverlight"),
                new HierarchyItem("WPF"),
                new HierarchyItem("ASP .Net"),
                new HierarchyItem("MVC")),
            new HierarchyItem("Reporting Edition",
                new HierarchyItem("IO"),
                new HierarchyItem("PDF Generator"),
                new HierarchyItem("WPF"))));
    }
}
```

### Step 3 — Bind in XAML

```xml
<syncfusion:HierarchyNavigator Name="hierarchyNavigator" Width="600" Height="30">
  <syncfusion:HierarchyNavigator.ItemsSource>
    <local:HierarchicalItemsSource/>
  </syncfusion:HierarchyNavigator.ItemsSource>
  <syncfusion:HierarchyNavigator.ItemTemplate>
    <HierarchicalDataTemplate ItemsSource="{Binding HierarchyItems}">
      <TextBlock Text="{Binding ContentString}" Margin="2,0"/>
    </HierarchicalDataTemplate>
  </syncfusion:HierarchyNavigator.ItemTemplate>
</syncfusion:HierarchyNavigator>
```

> **Key:** The `ItemsSource` binding on `HierarchicalDataTemplate` must point to the property that holds child items (here `HierarchyItems`). This tells the control how to drill down into each level.

---

## HierarchicalDataTemplate

`HierarchicalDataTemplate` is the required template type for hierarchical data in `HierarchyNavigator`. A plain `DataTemplate` will not expose child levels.

### Defining as a resource

```xml
<Window.Resources>
  <local:HierarchicalItemsSource x:Key="hierarchicalItemsSource"/>
  <HierarchicalDataTemplate x:Key="myTemplate" ItemsSource="{Binding HierarchyItems}">
    <TextBlock Text="{Binding ContentString}" Margin="2,0"/>
  </HierarchicalDataTemplate>
</Window.Resources>

<syncfusion:HierarchyNavigator ItemTemplate="{StaticResource myTemplate}"
                               ItemsSource="{StaticResource hierarchicalItemsSource}"
                               Height="30" Width="600"/>
```

### Custom item layout

Any `UIElement` can be used inside the template — use this to add icons, colors, or multi-column layouts:

```xml
<HierarchicalDataTemplate ItemsSource="{Binding Children}">
  <StackPanel Orientation="Horizontal">
    <Image Source="{Binding IconPath}" Width="16" Height="16" Margin="0,0,4,0"/>
    <TextBlock Text="{Binding Name}" VerticalAlignment="Center"/>
  </StackPanel>
</HierarchicalDataTemplate>
```

---

## Binding XML Data

When data comes from an XML file, parse it into an `ObservableCollection` first, then bind to `ItemsSource`.

### XML structure (HierarchyItems.xml)

```xml
<categories>
  <category name="Syncfusion">
    <category name="User Interface">
      <category name="Silverlight">
        <category name="Essential Tools"/>
        <category name="Essential Grid"/>
      </category>
      <category name="WPF"/>
      <category name="MVC"/>
    </category>
  </category>
</categories>
```

### Model class

```csharp
public class HierarchyItem
{
    public string ContentStr { get; set; }
    public ObservableCollection<HierarchyItem> HierarchyItems { get; set; }
        = new ObservableCollection<HierarchyItem>();
}
```

### Parse XML and bind

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        BindXmlData();
    }

    private void BindXmlData()
    {
        XDocument xmlSource = XDocument.Load("HierarchyItems.xml");
        var categories = GetCategories(xmlSource.Element("categories"));
        hierarchyNavigator1.ItemsSource = categories;
    }

    private ObservableCollection<HierarchyItem> GetCategories(XElement element)
    {
        var result = new ObservableCollection<HierarchyItem>();
        foreach (var category in element.Elements("category"))
        {
            result.Add(new HierarchyItem
            {
                ContentStr = category.Attribute("name").Value,
                HierarchyItems = GetCategories(category)
            });
        }
        return result;
    }
}
```

### XAML binding for XML data

```xml
<syncfusion:HierarchyNavigator x:Name="hierarchyNavigator1"
                               VerticalAlignment="Center" Height="30">
  <syncfusion:HierarchyNavigator.ItemTemplate>
    <HierarchicalDataTemplate ItemsSource="{Binding HierarchyItems}">
      <TextBlock Text="{Binding ContentStr}" Margin="2,0"/>
    </HierarchicalDataTemplate>
  </syncfusion:HierarchyNavigator.ItemTemplate>
</syncfusion:HierarchyNavigator>
```

---

## Binding via WCF Service

For server-side data, expose an `ObservableCollection<HierarchyItem>` from a WCF service operation and bind it asynchronously.

### WCF service contract

```csharp
[ServiceContract(Namespace = "")]
[AspNetCompatibilityRequirements(RequirementsMode = AspNetCompatibilityRequirementsMode.Allowed)]
public class DataService
{
    [OperationContract]
    public ObservableCollection<HierarchyItem> GetHierarchyItems()
    {
        XDocument xmlSource = XDocument.Load("HierarchyItems.xml");
        return GetCategories(xmlSource.Element("categories"));
    }

    private ObservableCollection<HierarchyItem> GetCategories(XElement element)
    {
        var result = new ObservableCollection<HierarchyItem>();
        foreach (var item in element.Elements("category"))
        {
            result.Add(new HierarchyItem
            {
                ContentStr = item.Attribute("name").Value,
                HierarchyItems = GetCategories(item)
            });
        }
        return result;
    }
}
```

### Client-side ViewModel

```csharp
public class CustomSource
{
    public ObservableCollection<HierarchyItem> Categories { get; set; }
        = new ObservableCollection<HierarchyItem>();

    public CustomSource()
    {
        var client = new DataServiceClient();
        client.GetHierarchyItemsCompleted += (s, e) =>
        {
            if (e.Error == null && e.Result != null)
                foreach (var item in e.Result)
                    Categories.Add(item);
        };
        client.GetHierarchyItemsAsync();
    }
}
```

### XAML

```xml
<Window.DataContext>
  <local:CustomSource/>
</Window.DataContext>
<Grid>
  <syncfusion:HierarchyNavigator ItemsSource="{Binding Categories}"
                                 VerticalAlignment="Center" Height="30">
    <syncfusion:HierarchyNavigator.ItemTemplate>
      <HierarchicalDataTemplate ItemsSource="{Binding HierarchyItems}">
        <TextBlock Text="{Binding ContentStr}" Margin="2,0"/>
      </HierarchicalDataTemplate>
    </syncfusion:HierarchyNavigator.ItemTemplate>
  </syncfusion:HierarchyNavigator>
</Grid>
```

---

## On-Demand Item Loading

For large hierarchies, load children lazily when a node is expanded rather than all upfront.

```csharp
public class LazyHierarchyItem
{
    public string Name { get; set; }
    public bool HasChildren { get; set; }

    private ObservableCollection<LazyHierarchyItem> _children;
    public ObservableCollection<LazyHierarchyItem> Children
    {
        get
        {
            if (_children == null && HasChildren)
                _children = LoadChildren();
            return _children;
        }
    }

    private ObservableCollection<LazyHierarchyItem> LoadChildren()
    {
        // Fetch from database, service, file system, etc.
        return new ObservableCollection<LazyHierarchyItem>
        {
            new LazyHierarchyItem { Name = "Child 1" },
            new LazyHierarchyItem { Name = "Child 2" }
        };
    }
}
```

Bind the same way as eager loading — `HierarchyNavigator` will call the `Children` getter only when the user navigates into that node.

> **Reference:** [How to load items on demand in WPF HierarchyNavigator](https://support.syncfusion.com/kb/article/9834/how-to-load-items-in-wpf-hierarchynavigator-breadcrumb-control-in-on-demand)

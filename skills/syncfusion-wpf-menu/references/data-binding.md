# Data Binding in MenuAdv

## Table of Contents
- [Binding to Objects (ObservableCollection)](#binding-to-objects-observablecollection)
- [Binding to XML via XmlDataProvider](#binding-to-xml-via-xmldataprovider)
- [Gotchas](#gotchas)

---

## Binding to Objects (ObservableCollection)

Use `ItemsSource` + `HierarchicalDataTemplate` to populate a menu from a nested data model.

### Step 1: Define the Model

The model must have a `Header` (display text) and a child collection property — the same property name is used in `HierarchicalDataTemplate.ItemsSource`:

```csharp
using System.Collections.ObjectModel;

public class MenuModel
{
    public MenuModel()
    {
        SubItems = new ObservableCollection<MenuModel>();
    }

    public string Header { get; set; }

    // Child items — property name MUST match HierarchicalDataTemplate ItemsSource binding
    public ObservableCollection<MenuModel> SubItems { get; set; }
}
```

### Step 2: Define the ViewModel

```csharp
using System.Collections.ObjectModel;

public class MenuViewModel
{
    public ObservableCollection<MenuModel> MenuItems { get; set; }

    public MenuViewModel()
    {
        MenuItems = new ObservableCollection<MenuModel>();
        PopulateData();
    }

    private void PopulateData()
    {
        MenuModel product = new MenuModel { Header = "Products" };

        MenuModel bi = new MenuModel { Header = "Business Intelligence" };
        MenuModel ui = new MenuModel { Header = "User Interface" };

        MenuModel wpf     = new MenuModel { Header = "WPF" };
        MenuModel tools   = new MenuModel { Header = "Tools" };
        MenuModel chart   = new MenuModel { Header = "Chart" };
        MenuModel grid    = new MenuModel { Header = "Grid" };
        MenuModel diagram = new MenuModel { Header = "Diagram" };

        wpf.SubItems.Add(tools);
        wpf.SubItems.Add(chart);
        wpf.SubItems.Add(grid);
        wpf.SubItems.Add(diagram);

        MenuModel sl = new MenuModel { Header = "Silverlight" };
        ui.SubItems.Add(wpf);
        ui.SubItems.Add(sl);

        MenuModel reporting = new MenuModel { Header = "Reporting" };

        product.SubItems.Add(bi);
        product.SubItems.Add(ui);
        product.SubItems.Add(reporting);

        MenuItems.Add(product);
    }
}
```

### Step 3: Set DataContext

```xaml
<Window.DataContext>
    <local:MenuViewModel/>
</Window.DataContext>
```

Or in code-behind:
```csharp
this.DataContext = new MenuViewModel();
```

### Step 4: Bind in XAML

```xaml
<syncfusion:MenuAdv ItemsSource="{Binding MenuItems}">
    <syncfusion:MenuAdv.ItemTemplate>
        <HierarchicalDataTemplate ItemsSource="{Binding SubItems}">
            <TextBlock Text="{Binding Header}"/>
        </HierarchicalDataTemplate>
    </syncfusion:MenuAdv.ItemTemplate>
</syncfusion:MenuAdv>
```

**Key rule:** `HierarchicalDataTemplate.ItemsSource="{Binding SubItems}"` — the property name **must exactly match** the child collection name on your model.

---

## Binding to XML via XmlDataProvider

### Step 1: Create XML file (`Data.xml`)

```xml
<?xml version="1.0" encoding="utf-8"?>
<Categories>
    <Root Name="Products">
        <SubItem Name="User Interface">
            <SubItem Name="WPF">
                <SubItem Name="Tools"/>
                <SubItem Name="Chart"/>
                <SubItem Name="Grid"/>
            </SubItem>
            <SubItem Name="Silverlight"/>
        </SubItem>
        <SubItem Name="Business Intelligence">
            <SubItem Name="WPF"/>
            <SubItem Name="ASP.NET"/>
        </SubItem>
        <SubItem Name="Reporting">
            <SubItem Name="WPF"/>
        </SubItem>
    </Root>
</Categories>
```

### Step 2: Add XmlDataProvider in XAML Resources

```xaml
<Window.Resources>
    <XmlDataProvider x:Key="xmlSource"
                     Source="Data.xml"
                     XPath="Categories"/>
</Window.Resources>
```

### Step 3: Bind MenuAdv to XML

```xaml
<syncfusion:MenuAdv
    ItemsSource="{Binding Source={StaticResource xmlSource}, XPath=Root}">
    <syncfusion:MenuAdv.ItemTemplate>
        <HierarchicalDataTemplate ItemsSource="{Binding XPath=SubItem}">
            <TextBlock Text="{Binding XPath=@Name}"/>
        </HierarchicalDataTemplate>
    </syncfusion:MenuAdv.ItemTemplate>
</syncfusion:MenuAdv>
```

**XPath patterns:**
- `XPath=SubItem` — selects child elements named `SubItem`
- `XPath=@Name` — selects the `Name` attribute of the element

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Menu items show but no submenus | `HierarchicalDataTemplate.ItemsSource` property name mismatch | Match exactly: `{Binding SubItems}` must match model property |
| Child collection is null | Model constructor missing `SubItems` initialization | Initialize in model constructor: `SubItems = new ObservableCollection<MenuModel>()` |
| XML binding shows no items | Incorrect `XPath` value | Check element names in XML: `XPath=Root` selects `<Root>` elements |
| `@Name` binding empty | Attribute name mismatch | Use `XPath=@Name` only if XML attribute is literally `Name="..."` |
| Menu empty after setting DataContext | `MenuItems` populated before DataContext set | Set DataContext before or in constructor after population |

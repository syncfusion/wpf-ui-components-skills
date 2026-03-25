# Getting Started with SfMultiColumnDropDownControl

## Table of Contents
- [Overview](#overview)
- [Assembly Deployment](#assembly-deployment)
- [Creating Application](#creating-application)
  - [Adding Control via Designer](#adding-control-via-designer)
  - [Adding Control in XAML](#adding-control-in-xaml)
  - [Adding Control in C#](#adding-control-in-c)
- [Creating Data Model](#creating-data-model)
- [Binding to Data](#binding-to-data)
- [DisplayMember and ValueMember](#displaymember-and-valuemember)
- [Defining Columns](#defining-columns)
- [Theme Support](#theme-support)
- [Troubleshooting](#troubleshooting)

## Overview

The SfMultiColumnDropDownControl displays multiple columns in a dropdown by embedding the SfDataGrid control for rich look-up selection. It combines a TextBox editor with a powerful data grid to provide:

- Autocomplete support
- Customizable columns display
- Text-based filtering
- Runtime popup resizing
- Read-only editor option

## Assembly Deployment

To use SfMultiColumnDropDownControl, add the following assemblies as references to your WPF application:

### Required Assemblies

| Assembly | Description |
|----------|-------------|
| **Syncfusion.Data.WPF** | Contains fundamental classes for CollectionViewAdv responsible for data processing in SfDataGrid |
| **Syncfusion.SfGrid.WPF** | Contains UI classes for SfMultiColumnDropDownControl and DropDownGrid |

### Namespace

The SfMultiColumnDropDownControl is in the `Syncfusion.UI.Xaml.Grid` namespace, available in the Syncfusion WPF schema:
- **Schema URI:** `http://schemas.syncfusion.com/wpf`
- **Namespace:** `Syncfusion.UI.Xaml.Grid`

## Creating Application

### Adding Control via Designer

1. Open your WPF project in Visual Studio
2. Locate the **SfMultiColumnDropDownControl** in the Toolbox
3. Drag and drop it onto the Designer view
4. Required assembly references are added automatically

This is the quickest method for adding the control as Visual Studio handles assembly references and namespace imports.

### Adding Control in XAML

**Step 1:** Add required assembly references:
- Syncfusion.Data.WPF
- Syncfusion.SfGrid.WPF

**Step 2:** Import the Syncfusion WPF schema or namespace in your XAML:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="WpfApplication1.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <syncfusion:SfMultiColumnDropDownControl 
            x:Name="sfmultiColumn"
            Width="200"
            Height="30"/>
    </Grid>
</Window>
```

**Alternative:** Import specific namespace:
```xml
xmlns:grid="clr-namespace:Syncfusion.UI.Xaml.Grid;assembly=Syncfusion.SfGrid.WPF"
```

Then use:
```xml
<grid:SfMultiColumnDropDownControl x:Name="sfmultiColumn"/>
```

### Adding Control in C#

**Step 1:** Add assembly references (same as XAML)

**Step 2:** Import namespace and create control:

```csharp
using Syncfusion.UI.Xaml.Grid;

namespace WpfApplication1
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create control instance
            SfMultiColumnDropDownControl sfMultiColumn = new SfMultiColumnDropDownControl();
            sfMultiColumn.Width = 200;
            sfMultiColumn.Height = 30;
            
            // Add to layout
            Root_Grid.Children.Add(sfMultiColumn);
        }
    }
}
```

## Creating Data Model

Before binding data, create a data model class representing your data structure.

### Simple Data Model

```csharp
public class OrderInfo
{
    private int orderID;
    private string customerId;
    private string country;
    private string customerName;
    private string shippingCity;

    public int OrderID
    {
        get { return orderID; }
        set { orderID = value; }
    }

    public string CustomerID
    {
        get { return customerId; }
        set { customerId = value; }
    }

    public string CustomerName
    {
        get { return customerName; }
        set { customerName = value; }
    }

    public string Country
    {
        get { return country; }
        set { country = value; }
    }

    public string ShipCity
    {
        get { return shippingCity; }
        set { shippingCity = value; }
    }

    public OrderInfo(int orderId, string customerName, string country, string customerId, string shipCity)
    {
        this.OrderID = orderId;
        this.CustomerName = customerName;
        this.Country = country;
        this.CustomerID = customerId;
        this.ShipCity = shipCity;
    }
}
```

### ViewModel with Data Collection

```csharp
using System.Collections.ObjectModel;

public class ViewModel
{
    private ObservableCollection<OrderInfo> _orders;

    public ObservableCollection<OrderInfo> Orders
    {
        get { return _orders; }
        set { _orders = value; }
    }

    public ViewModel()
    {
        _orders = new ObservableCollection<OrderInfo>();
        this.GenerateOrders();
    }

    private void GenerateOrders()
    {
        _orders.Add(new OrderInfo(1001, "Maria Anders", "Germany", "ALFKI", "Berlin"));
        _orders.Add(new OrderInfo(1002, "Ana Trujilo", "Mexico", "ANATR", "Mexico D.F."));
        _orders.Add(new OrderInfo(1003, "Antonio Moreno", "Mexico", "ANTON", "Mexico D.F."));
        _orders.Add(new OrderInfo(1004, "Thomas Hardy", "UK", "AROUT", "London"));
        _orders.Add(new OrderInfo(1005, "Christina Berglund", "Sweden", "BERGS", "Lula"));
        _orders.Add(new OrderInfo(1006, "Hanna Moos", "Germany", "BLAUS", "Mannheim"));
        _orders.Add(new OrderInfo(1007, "Frederique Citeaux", "France", "BLONP", "Strasbourg"));
        _orders.Add(new OrderInfo(1008, "Martin Sommer", "Spain", "BOLID", "Madrid"));
        _orders.Add(new OrderInfo(1009, "Laurence Lebihan", "France", "BONAP", "Marseille"));
        _orders.Add(new OrderInfo(1010, "Elizabeth Lincoln", "Canada", "BOTTM", "Tsawassen"));
    }
}
```

## Binding to Data

Set the `ItemsSource` property to populate the dropdown list.

### XAML Data Binding

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:WpfApplication1"
        x:Class="WpfApplication1.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <Grid x:Name="Root_Grid">
        <syncfusion:SfMultiColumnDropDownControl 
            x:Name="sfMultiColumn"
            ItemsSource="{Binding Orders}"
            DisplayMember="OrderID"
            Width="175"
            Height="30"
            SelectedIndex="2"/>
    </Grid>
</Window>
```

### Code-Behind Data Binding

```csharp
ViewModel viewModel = new ViewModel();
sfMultiColumn.ItemsSource = viewModel.Orders;
sfMultiColumn.DisplayMember = "CustomerName";
sfMultiColumn.SelectedIndex = 0;
```

## DisplayMember and ValueMember

These properties control what is displayed and what value is retrieved from selections.

### DisplayMember

- **Purpose:** Specifies the property path displayed in the TextBox editor
- **Type:** String (property name from data object)
- **Default:** null (displays ToString() of object)

### ValueMember

- **Purpose:** Specifies the property path for the SelectedValue property
- **Type:** String (property name from data object)
- **Default:** null

### Example

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    ValueMember="OrderID"
    SelectedIndex="0"/>
```

With this configuration:
- **TextBox displays:** CustomerName value (e.g., "Maria Anders")
- **SelectedValue contains:** OrderID value (e.g., 1001)
- **SelectedItem contains:** The entire OrderInfo object

### Accessing Values

```csharp
// Get displayed text
string displayedName = sfMultiColumn.Text;

// Get selected value based on ValueMember
int orderId = (int)sfMultiColumn.SelectedValue;

// Get full selected object
OrderInfo selectedOrder = sfMultiColumn.SelectedItem as OrderInfo;
```

## Defining Columns

By default, columns are auto-generated from the data source. You can control this behavior.

### Auto-Generated Columns

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AutoGenerateColumns="True"
    SelectedIndex="0"/>
```

All public properties from the data object become columns automatically.

### Manually Defined Columns

To have precise control over which columns appear:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    Width="175"
    Height="30"
    SelectedIndex="0"
    AutoGenerateColumns="False"
    DisplayMember="OrderID"
    ItemsSource="{Binding Orders}">
    
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" HeaderText="Order ID"/>
        <syncfusion:GridTextColumn MappingName="CustomerID" HeaderText="Customer ID"/>
        <syncfusion:GridTextColumn MappingName="Country"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

### Code-Behind Column Definition

```csharp
sfMultiColumn.AutoGenerateColumns = false;
sfMultiColumn.Columns.Add(new GridTextColumn() { MappingName = "OrderID", HeaderText = "Order ID" });
sfMultiColumn.Columns.Add(new GridTextColumn() { MappingName = "CustomerID", HeaderText = "Customer ID" });
sfMultiColumn.Columns.Add(new GridTextColumn() { MappingName = "Country" });
```

## Theme Support

SfMultiColumnDropDownControl supports various built-in themes for consistent appearance.

### Applying Theme with SfSkinManager

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    
    <Grid syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
        <syncfusion:SfMultiColumnDropDownControl 
            x:Name="sfMultiColumn"
            ItemsSource="{Binding Orders}"
            DisplayMember="CustomerName"/>
    </Grid>
</Window>
```

### Available Themes

- MaterialLight
- MaterialDark
- Office2019Colorful
- Office2019Black
- Office2019White
- FluentLight
- FluentDark
- And more...

## Troubleshooting

### Control Not Visible

**Problem:** Control added but not showing in UI

**Solutions:**
- Verify Width and Height are set (not 0 or NaN)
- Check that parent container has proper sizing
- Ensure control is added to visual tree

### No Data Displaying

**Problem:** ItemsSource bound but dropdown is empty

**Solutions:**
- Verify ItemsSource collection has data (not empty)
- Check DataContext is properly set
- Ensure property names in binding are correct
- Use Snoop or Visual Studio Live Visual Tree to inspect bindings

### Assembly Reference Errors

**Problem:** Cannot find SfMultiColumnDropDownControl type

**Solutions:**
- Verify Syncfusion.Data.WPF and Syncfusion.SfGrid.WPF are referenced
- Check assembly version compatibility
- Ensure correct namespace import in XAML or using statement in C#

### DisplayMember Not Working

**Problem:** Wrong property showing in editor

**Solutions:**
- Verify property name spelling (case-sensitive)
- Ensure property is public
- Check that data objects have the property
- Try setting DisplayMember in code-behind to verify binding

### Theme Not Applying

**Problem:** Control doesn't match application theme

**Solutions:**
- Add Syncfusion.SfSkinManager.WPF reference
- Apply theme to parent container or window
- Verify theme name spelling
- Check SfSkinManager is properly referenced in XAML

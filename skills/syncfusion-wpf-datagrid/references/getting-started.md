# Getting Started with WPF DataGrid

This guide covers the initial setup and basic configuration of the Syncfusion WPF DataGrid (SfDataGrid) component in your Windows Presentation Foundation application.

## Installation

### NuGet Package Manager

Install the SfDataGrid package via NuGet Package Manager:

```powershell
Install-Package Syncfusion.SfGrid.WPF
```

Or use the Package Manager UI in Visual Studio:
1. Right-click on your project → Manage NuGet Packages
2. Search for "Syncfusion.SfGrid.WPF"
3. Click Install

### Assembly References

After installation, the following assemblies are automatically referenced:
- **Syncfusion.SfGrid.WPF** - Main DataGrid control
- **Syncfusion.Data.WPF** - Data management and operations
- **Syncfusion.Shared.WPF** - Shared utilities and helpers

## Namespace Imports

Add the necessary namespace in your XAML:

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

Or in C#:

```csharp
using Syncfusion.UI.Xaml.Grid;
using Syncfusion.Data;
```

## License Configuration

Syncfusion components require a license key. Register it in your application startup:

```csharp
// In App.xaml.cs
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // Register Syncfusion license
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        base.OnStartup(e);
    }
}
```

**License Options:**
- **Community License**: Free for qualifying companies and individuals
- **Trial License**: 30-day evaluation
- **Commercial License**: For production use

Get your license key from: https://www.syncfusion.com/account/downloads

## Basic DataGrid Setup

### Simple XAML Setup

Create a basic DataGrid with auto-generated columns:

```xml
<Window x:Class="DataGridDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:SfDataGrid x:Name="dataGrid"
                               AutoGenerateColumns="True"
                               ItemsSource="{Binding Orders}"/>
    </Grid>
</Window>
```

### Data Binding with ViewModel

Create a data model class:

```csharp
using System.ComponentModel;

public class OrderInfo : INotifyPropertyChanged
{
    private int orderID;
    private string customerID;
    private string customerName;
    private string country;
    private double unitPrice;

    public int OrderID
    {
        get { return orderID; }
        set
        {
            orderID = value;
            OnPropertyChanged(nameof(OrderID));
        }
    }

    public string CustomerID
    {
        get { return customerID; }
        set
        {
            customerID = value;
            OnPropertyChanged(nameof(CustomerID));
        }
    }

    public string CustomerName
    {
        get { return customerName; }
        set
        {
            customerName = value;
            OnPropertyChanged(nameof(CustomerName));
        }
    }

    public string Country
    {
        get { return country; }
        set
        {
            country = value;
            OnPropertyChanged(nameof(Country));
        }
    }

    public double UnitPrice
    {
        get { return unitPrice; }
        set
        {
            unitPrice = value;
            OnPropertyChanged(nameof(UnitPrice));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Create a ViewModel:

```csharp
using System.Collections.ObjectModel;

public class ViewModel
{
    public ObservableCollection<OrderInfo> Orders { get; set; }

    public ViewModel()
    {
        Orders = new ObservableCollection<OrderInfo>();
        GenerateOrders();
    }

    private void GenerateOrders()
    {
        Orders.Add(new OrderInfo
        {
            OrderID = 1001,
            CustomerID = "ALFKI",
            CustomerName = "Maria Anders",
            Country = "Germany",
            UnitPrice = 250.50
        });

        Orders.Add(new OrderInfo
        {
            OrderID = 1002,
            CustomerID = "ANATR",
            CustomerName = "Ana Trujillo",
            Country = "Mexico",
            UnitPrice = 320.75
        });

        Orders.Add(new OrderInfo
        {
            OrderID = 1003,
            CustomerID = "ANTON",
            CustomerName = "Antonio Moreno",
            Country = "Spain",
            UnitPrice = 180.25
        });
    }
}
```

Set DataContext in MainWindow:

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.DataContext = new ViewModel();
    }
}
```

Or in XAML:

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>
```

## Auto-Generating Columns

By default, `AutoGenerateColumns` is set to `true`, which automatically creates columns based on the properties of your data model.

### Column Type Mapping

The DataGrid automatically selects column types based on property data types:

| Data Type | Column Type | Usage |
|-----------|-------------|-------|
| `string`, `object` | `GridTextColumn` | String data |
| `int`, `float`, `double`, `decimal` | `GridNumericColumn` | Numeric data |
| `DateTime`, `DateTimeOffset` | `GridDateTimeColumn` | Date/time values |
| `bool` | `GridCheckBoxColumn` | Boolean values |
| `TimeSpan` | `GridTimeSpanColumn` | Time spans |
| `Uri` | `GridHyperlinkColumn` | URLs |

### Disabling Auto-Generation

To manually define columns, set `AutoGenerateColumns` to `false`:

```xml
<syncfusion:SfDataGrid AutoGenerateColumns="False"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID" HeaderText="Order ID"/>
        <syncfusion:GridTextColumn MappingName="CustomerName" HeaderText="Customer"/>
        <syncfusion:GridTextColumn MappingName="Country" HeaderText="Country"/>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

## Auto-Generate Modes

Control column generation behavior with `AutoGenerateColumnsMode`:

```xml
<syncfusion:SfDataGrid AutoGenerateColumnsMode="Reset" 
                       ItemsSource="{Binding Orders}"/>
```

**Available Modes:**

| Mode | Behavior | On ItemsSource Change |
|------|----------|----------------------|
| `Reset` | Generates columns from data properties | Keeps manual columns, regenerates auto columns |
| `RetainOld` | Generates if not explicitly defined | Maintains existing columns and settings |
| `ResetAll` | Generates columns from data properties | Clears all columns and regenerates |
| `None` | No auto-generation | Keeps existing columns |

## Customizing Auto-Generated Columns

Handle the `AutoGeneratingColumn` event to customize columns during auto-generation:

```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new ViewModel();
    this.dataGrid.AutoGeneratingColumn += DataGrid_AutoGeneratingColumn;
}

private void DataGrid_AutoGeneratingColumn(object sender, AutoGeneratingColumnArgs e)
{
    // Change header text
    if (e.Column.MappingName == "OrderID")
    {
        e.Column.HeaderText = "Order Number";
    }

    // Disable editing for specific column
    if (e.Column.MappingName == "CustomerID")
    {
        e.Column.AllowEditing = false;
    }

    // Cancel column generation
    if (e.Column.MappingName == "InternalField")
    {
        e.Cancel = true;
    }

    // Change column type
    if (e.Column.MappingName == "UnitPrice" && e.Column is GridNumericColumn)
    {
        e.Column = new GridCurrencyColumn
        {
            MappingName = "UnitPrice",
            HeaderText = "Price",
            CurrencySymbol = "$"
        };
    }
}
```

## Complete Example

Here's a complete working example:

### MainWindow.xaml

```xml
<Window x:Class="DataGridDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:DataGridDemo"
        Title="WPF DataGrid Demo" Height="450" Width="800">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <Grid Margin="10">
        <syncfusion:SfDataGrid x:Name="dataGrid"
                               AutoGenerateColumns="True"
                               AllowSorting="True"
                               AllowFiltering="False"
                               AllowEditing="False"
                               ItemsSource="{Binding Orders}"/>
    </Grid>
</Window>
```

### MainWindow.xaml.cs

```csharp
using System.Windows;
using Syncfusion.UI.Xaml.Grid;

namespace DataGridDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Customize auto-generated columns
            this.dataGrid.AutoGeneratingColumn += (s, e) =>
            {
                if (e.Column.MappingName == "UnitPrice")
                {
                    e.Column.HeaderText = "Price ($)";
                    e.Column.TextAlignment = TextAlignment.Right;
                }
            };
        }
    }
}
```

## Next Steps

Now that you have a basic DataGrid set up, explore these topics:

- **Columns**: Learn about column types and manual configuration
- **Sorting**: Enable and configure data sorting
- **Filtering**: Add filtering capabilities
- **Editing**: Enable in-line data editing
- **Styling**: Customize the visual appearance

## Common Issues

### License Key Error

**Problem**: Application shows license error popup.

**Solution**: Ensure `RegisterLicense()` is called before any Syncfusion control is initialized, typically in `App.xaml.cs` `OnStartup` method.

### Columns Not Showing

**Problem**: DataGrid displays but no columns appear.

**Solution**: 
- Verify `ItemsSource` is bound correctly
- Check that data model properties are public
- Ensure `AutoGenerateColumns="True"` or columns are manually defined

### Data Not Updating

**Problem**: Changes to data source don't reflect in grid.

**Solution**:
- Use `ObservableCollection<T>` for ItemsSource
- Implement `INotifyPropertyChanged` in data model
- Call `OnPropertyChanged` when property values change

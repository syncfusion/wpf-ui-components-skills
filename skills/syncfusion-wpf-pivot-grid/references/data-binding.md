# Data Binding in WPF PivotGrid

## Table of Contents
- [Overview](#overview)
- [Binding to List Collections](#binding-to-list-collections)
- [Binding to DataTable](#binding-to-datatable)
- [Asynchronous Data Loading](#asynchronous-data-loading)
- [ItemSource Configuration](#itemsource-configuration)
- [Best Practices](#best-practices)

## Overview

The WPF PivotGrid supports binding to various data sources including List collections, DataTables, and DataViews. The control uses the `ItemSource` property to bind data, which can be set through XAML or code-behind.

## Binding to List Collections

The PivotGrid can bind to any IEnumerable collection, including List<T>, ObservableCollection<T>, and custom collections.

### Creating a Data Model

```csharp
public class ProductSales
{
    public string Product { get; set; }
    public string Date { get; set; }
    public string Country { get; set; }
    public string State { get; set; }
    public int Quantity { get; set; }
    public double Amount { get; set; }
}
```

### Generating Sample Data

```csharp
public static class SalesDataGenerator
{
    public static List<ProductSales> GetSalesData()
    {
        string[] countries = new string[] { "Canada", "USA", "UK" };
        string[] states = new string[] { "Alberta", "British Columbia", "Ontario" };
        string[] dates = new string[] { "FY 2005", "FY 2006", "FY 2007" };
        string[] products = new string[] { "Bike", "Car", "Truck" };
        
        Random r = new Random(123345345);
        int numberOfRecords = 2000;
        List<ProductSales> salesData = new List<ProductSales>();
        
        for (int i = 0; i < numberOfRecords; i++)
        {
            ProductSales sale = new ProductSales
            {
                Country = countries[r.Next(countries.Length)],
                State = states[r.Next(states.Length)],
                Date = dates[r.Next(dates.Length)],
                Product = products[r.Next(products.Length)],
                Quantity = r.Next(1, 12),
            };
            
            // Calculate amount with discount
            double discount = (30000 * sale.Quantity) * (sale.Quantity / 100.0);
            sale.Amount = (30000 * sale.Quantity) - discount;
            
            salesData.Add(sale);
        }
        
        return salesData;
    }
}
```

### Binding in XAML

Use `ObjectDataProvider` to bind the data source:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourNamespace"
        Title="PivotGrid Data Binding" Height="600" Width="800">
    
    <Window.Resources>
        <ObjectDataProvider x:Key="salesData" 
                          ObjectType="{x:Type local:SalesDataGenerator}" 
                          MethodName="GetSalesData" />
    </Window.Resources>
    
    <Grid>
        <syncfusion:PivotGridControl Name="pivotGrid" 
                                    ItemSource="{Binding Source={StaticResource salesData}}">
            <!-- PivotRows, PivotColumns, PivotCalculations -->
        </syncfusion:PivotGridControl>
    </Grid>
</Window>
```

### Binding in Code-Behind

```csharp
using Syncfusion.Windows.Controls.PivotGrid;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.Loaded += MainWindow_Loaded;
    }

    void MainWindow_Loaded(object sender, RoutedEventArgs e)
    {
        // Bind List collection
        pivotGrid.ItemSource = SalesDataGenerator.GetSalesData();
    }
}
```

## Binding to DataTable

The PivotGrid can bind to DataTable and DataView objects from ADO.NET datasets.

### Retrieving Data from Database

```csharp
using System.Data;
using System.Data.SqlServerCe;

public static class OrderDataProvider
{
    public static DataView GetOrderDetails()
    {
        try
        {
            DataSet ds = new DataSet();
            string connectionString = @"DataSource=Data\Northwind.sdf";
            
            using (SqlCeConnection con = new SqlCeConnection(connectionString))
            {
                con.Open();
                
                string query = @"SELECT Top(100) [Order ID], [Product ID], 
                                [Unit Price], Quantity 
                                FROM [Order Details]";
                
                SqlCeCommand cmd = new SqlCeCommand(query, con);
                SqlCeDataAdapter da = new SqlCeDataAdapter(cmd);
                da.Fill(ds);
                
                // Create DataView with filter
                DataView dataView = new DataView(
                    ds.Tables[0], 
                    "[Unit Price] >= 20", 
                    "[Order ID]", 
                    DataViewRowState.CurrentRows
                );
                
                return dataView;
            }
        }
        catch (Exception ex)
        {
            // Handle exception
            throw;
        }
    }
}
```

### Binding DataView in XAML

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourNamespace"
        Title="DataTable Binding" Height="600" Width="800">
    
    <Window.Resources>
        <ObjectDataProvider x:Key="orderData" 
                          ObjectType="{x:Type local:OrderDataProvider}" 
                          MethodName="GetOrderDetails" />
    </Window.Resources>
    
    <Grid>
        <syncfusion:PivotGridControl Name="pivotGrid1" 
                                    ItemSource="{Binding Source={StaticResource orderData}}">
            
            <syncfusion:PivotGridControl.PivotRows>
                <syncfusion:PivotItem FieldHeader="OID" 
                                     FieldMappingName="Order ID" 
                                     TotalHeader="Total" />
            </syncfusion:PivotGridControl.PivotRows>
            
            <syncfusion:PivotGridControl.PivotColumns>
                <syncfusion:PivotItem FieldHeader="PID" 
                                     FieldMappingName="Product ID" 
                                     TotalHeader="Total" />
            </syncfusion:PivotGridControl.PivotColumns>
            
            <syncfusion:PivotGridControl.PivotCalculations>
                <syncfusion:PivotComputationInfo FieldName="Unit Price" 
                                                CalculationName="UnitTotal" 
                                                SummaryType="DoubleTotalSum" 
                                                Format="#.00" />
                <syncfusion:PivotComputationInfo FieldName="Quantity" 
                                                CalculationName="QuantityCalc" 
                                                SummaryType="Count" 
                                                Format="C" />
            </syncfusion:PivotGridControl.PivotCalculations>
        </syncfusion:PivotGridControl>
    </Grid>
</Window>
```

### Binding DataView in Code-Behind

```csharp
using Syncfusion.PivotAnalysis.Base;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.Loaded += MainWindow_Loaded;
    }

    void MainWindow_Loaded(object sender, RoutedEventArgs e)
    {
        // Bind DataView to ItemSource
        this.pivotGrid1.ItemSource = OrderDataProvider.GetOrderDetails();
        
        // Configure PivotItems
        this.pivotGrid1.PivotRows.Add(new PivotItem
        {
            FieldHeader = "OrderID",
            FieldMappingName = "Order ID",
            TotalHeader = "Total"
        });
        
        this.pivotGrid1.PivotColumns.Add(new PivotItem
        {
            FieldHeader = "ProductID",
            FieldMappingName = "Product ID",
            TotalHeader = "Total"
        });
        
        // Configure Calculations
        this.pivotGrid1.PivotCalculations.Add(new PivotComputationInfo
        {
            CalculationName = "UnitPrice",
            FieldName = "Unit Price",
            Format = "C",
            SummaryType = SummaryType.DecimalTotalSum
        });
        
        this.pivotGrid1.PivotCalculations.Add(new PivotComputationInfo
        {
            CalculationName = "Quantity",
            FieldName = "Quantity",
            SummaryType = SummaryType.Count
        });
    }
}
```

## Asynchronous Data Loading

For large datasets, use asynchronous data loading to keep the UI responsive during data operations.

### Enabling Async Loading

```csharp
using System.Threading.Tasks;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.Loaded += MainWindow_Loaded;
    }

    async void MainWindow_Loaded(object sender, RoutedEventArgs e)
    {
        // Show loading indicator
        loadingIndicator.Visibility = Visibility.Visible;
        
        // Load data asynchronously
        var data = await Task.Run(() => SalesDataGenerator.GetSalesData());
        
        // Bind data on UI thread
        pivotGrid.ItemSource = data;
        
        // Hide loading indicator
        loadingIndicator.Visibility = Visibility.Collapsed;
    }
}
```

### Async Data Provider Pattern

```csharp
public static class AsyncDataProvider
{
    public static async Task<List<ProductSales>> GetSalesDataAsync()
    {
        return await Task.Run(() =>
        {
            // Simulate database query or API call
            System.Threading.Thread.Sleep(2000);
            return SalesDataGenerator.GetSalesData();
        });
    }
    
    public static async Task<DataView> GetOrderDetailsAsync()
    {
        return await Task.Run(() =>
        {
            return OrderDataProvider.GetOrderDetails();
        });
    }
}
```

### Using Async/Await Pattern

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        InitializeAsync();
    }

    private async void InitializeAsync()
    {
        try
        {
            // Load data without blocking UI
            var salesData = await AsyncDataProvider.GetSalesDataAsync();
            
            // Configure PivotGrid after data loads
            ConfigurePivotGrid();
            
            // Bind data
            pivotGrid.ItemSource = salesData;
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error loading data: {ex.Message}");
        }
    }
    
    private void ConfigurePivotGrid()
    {
        // Add PivotItems and Calculations
        pivotGrid.PivotRows.Add(new PivotItem 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product" 
        });
        // ... more configuration
    }
}
```

## ItemSource Configuration

### Changing Data Source at Runtime

```csharp
private void LoadNewData_Click(object sender, RoutedEventArgs e)
{
    // Clear existing data
    pivotGrid.ItemSource = null;
    
    // Load and bind new data
    var newData = GetNewDataSource();
    pivotGrid.ItemSource = newData;
    
    // Refresh the PivotGrid
    pivotGrid.InternalGrid.Model.ResetRowHeights();
}
```

### Refreshing Data

```csharp
private void RefreshData_Click(object sender, RoutedEventArgs e)
{
    // Get current data source
    var currentData = pivotGrid.ItemSource;
    
    // Reload or update data
    var updatedData = ReloadDataSource();
    
    // Re-bind to trigger refresh
    pivotGrid.ItemSource = null;
    pivotGrid.ItemSource = updatedData;
}
```

## Best Practices

### 1. Use Appropriate Data Types

Match your data model properties to PivotGrid expectations:
- **Dimensions (Rows/Columns):** string, int, DateTime
- **Values (Calculations):** int, double, decimal, float

### 2. Optimize Large Datasets

For datasets with >10,000 records:
- Use asynchronous loading
- Enable virtualization (on by default)
- Consider data paging or filtering at source
- Use appropriate SummaryTypes to reduce calculations

### 3. Handle Null Values

```csharp
public class ProductSales
{
    private string _product = string.Empty;
    
    public string Product
    {
        get => _product ?? string.Empty;
        set => _product = value;
    }
    
    // Nullable for optional fields
    public double? Discount { get; set; }
}
```

### 4. Use ObservableCollection for Dynamic Data

For data that changes at runtime:

```csharp
using System.Collections.ObjectModel;

public class SalesViewModel : INotifyPropertyChanged
{
    public ObservableCollection<ProductSales> SalesData { get; set; }
    
    public SalesViewModel()
    {
        SalesData = new ObservableCollection<ProductSales>();
        LoadData();
    }
    
    private void LoadData()
    {
        var data = SalesDataGenerator.GetSalesData();
        foreach (var item in data)
        {
            SalesData.Add(item);
        }
    }
}
```

### 5. Validate Field Mappings

Ensure `FieldMappingName` matches property names exactly:

```csharp
// Data Model
public class Sales
{
    public string ProductName { get; set; }  // Property name
}

// PivotItem - must match exactly
var item = new PivotItem
{
    FieldMappingName = "ProductName"  // Matches property
};
```

### 6. Connection String Management

Store connection strings in configuration:

```xml
<!-- App.config -->
<configuration>
  <connectionStrings>
    <add name="NorthwindDB" 
         connectionString="DataSource=Data\Northwind.sdf" 
         providerName="System.Data.SqlServerCe" />
  </connectionStrings>
</configuration>
```

```csharp
using System.Configuration;

string connectionString = ConfigurationManager
    .ConnectionStrings["NorthwindDB"]
    .ConnectionString;
```

## Common Issues

### Issue: PivotGrid Shows No Data

**Solution:** Verify:
1. ItemSource is not null
2. FieldMappingName matches property names (case-sensitive)
3. Data collection is not empty
4. PivotRows, PivotColumns, and PivotCalculations are defined

### Issue: Performance Degradation with Large Data

**Solution:**
- Use async loading
- Reduce data at source with SQL filtering
- Use appropriate aggregations
- Consider enabling data virtualization

### Issue: Data Not Refreshing

**Solution:**
- Set ItemSource to null before rebinding
- Use ObservableCollection for automatic updates
- Call grid refresh methods after data changes

## Related Topics

- **PivotItems and Calculations:** Learn how to define dimensions and aggregations
- **Filtering:** Narrow down data display
- **Performance:** Optimize for large datasets with virtualization

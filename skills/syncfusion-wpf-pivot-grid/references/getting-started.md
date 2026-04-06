# Getting Started with WPF PivotGrid

This guide covers installation, setup, and creating your first PivotGrid control in a WPF application.

### NuGet Package Installation

**Important:** The latest version should be installed.

Install via NuGet Package Manager:

```powershell
Install-Package Syncfusion.PivotTable.wpf
```

Or via .NET CLI:

```bash
dotnet add package Syncfusion.PivotTable.wpf
```

The NuGet package automatically includes all required dependencies.


## License Configuration

**Important:** Starting with v16.2.0.x, if you reference Syncfusion assemblies from trial setup or NuGet feed, you must include a license key in your projects.

Register the license key in your application startup:

```csharp
// In App.xaml.cs
protected override void OnStartup(StartupEventArgs e)
{
    Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
    base.OnStartup(e);
}
```

Learn more about [Syncfusion licensing](https://help.syncfusion.com/common/essential-studio/licensing/overview).


## Adding PivotGrid to Your Application

The PivotGrid can be added through Designer, XAML, or code-behind. Choose the method that fits your workflow.

### Method 1: Adding Through Designer

1. Open Visual Studio IDE
2. Navigate to **File > New > Project > WPF Application** (Visual C# Templates)
3. After creating the WPF application, go to **View** menu and select **Toolbox**
4. Locate the PivotGrid control in the **Syncfusion BI WPF** category
5. Drag the control from toolbox to the designer page
6. The PivotGrid control will be added along with dependency assemblies automatically

### Method 2: Adding Through XAML

**Step 1:** Create a new WPF Application in Visual Studio

**Step 2:** Add assembly references (see Assembly References section above)

**Step 3:** Define the PivotGrid in MainWindow.xaml:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="WpfApplication1.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <syncfusion:PivotGridControl Name="pivotGrid" 
                                    HorizontalAlignment="Left" 
                                    Margin="5,0,0,0" 
                                    VerticalAlignment="Top" 
                                    Height="50" 
                                    Width="100"/>
    </Grid>
</Window>
```

### Method 3: Adding Through Code-Behind

**Step 1:** Create a new WPF Application

**Step 2:** Add assembly references

**Step 3:** Add a grid container in MainWindow.xaml:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="WpfApplication1.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    <Grid Name="grid1">
    </Grid>
</Window>
```

**Step 4:** Include the namespace in MainWindow.xaml.cs:

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
```

**Step 5:** Create and add PivotGrid in the Loaded event:

```csharp
public MainWindow()
{
    InitializeComponent();
    this.Loaded += MainWindow_Loaded;
}

void MainWindow_Loaded(object sender, RoutedEventArgs e)
{
    PivotGridControl pivotGrid = new PivotGridControl();
    this.grid1.Children.Add(pivotGrid);
}
```

### Method 4: Adding Through Expression Blend

1. Open Blend for Visual Studio
2. Navigate to **File > New Project > WPF > WPF Application**
3. Select the **Projects** tab (top-left corner)
4. Right-click **References** and select **Add Reference**
5. Browse and add the required assemblies
6. After adding assemblies, the PivotGrid control appears under the **Assets** tab
7. Choose the **Assets** tab and drag PivotGrid from toolbox to designer

## Binding a Data Source

To display data, you need to bind a data source to the PivotGrid's `ItemSource` property.

### Step 1: Create a Data Model

Create a new class file (e.g., ProductSales.cs):

```csharp
using System;
using System.Collections.Generic;

public class ProductSales
{
    public string Product { get; set; }
    public string Date { get; set; }
    public string Country { get; set; }
    public string State { get; set; }
    public int Quantity { get; set; }
    public double Amount { get; set; }

    public static ProductSalesCollection GetSalesData()
    {
        // Geography
        string[] countries = new string[] { "Canada" };
        string[] canadaStates = new string[] { "Alberta", "British Columbia", "Ontario" };
        
        // Time
        string[] dates = new string[] { "FY 2005", "FY 2006", "FY 2007" };
        
        // Products
        string[] products = new string[] { "Bike", "Car" };
        
        Random r = new Random(123345345);
        int numberOfRecords = 2000;
        ProductSalesCollection listOfProductSales = new ProductSalesCollection();
        
        for (int i = 0; i < numberOfRecords; i++)
        {
            ProductSales sales = new ProductSales();
            sales.Country = countries[r.Next(0, countries.GetLength(0))];
            sales.Quantity = r.Next(1, 12);
            
            // 1 percent discount for 1 quantity
            double discount = (30000 * sales.Quantity) * (double.Parse(sales.Quantity.ToString()) / 100);
            sales.Amount = (30000 * sales.Quantity) - discount;
            sales.Date = dates[r.Next(r.Next(dates.GetLength(0) + 1))];
            sales.Product = products[r.Next(r.Next(products.GetLength(0) + 1))];
            sales.State = canadaStates[r.Next(canadaStates.GetLength(0))];
            
            listOfProductSales.Add(sales);
        }
        
        return listOfProductSales;
    }

    public override string ToString()
    {
        return string.Format("{0}-{1}-{2}", this.Country, this.State, this.Product);
    }

    public class ProductSalesCollection : List<ProductSales> { }
}
```

### Step 2: Bind Data in XAML

Use `ObjectDataProvider` to bind the data source:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:WpfApplication1"
        x:Class="WpfApplication1.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    <Window.Resources>
        <ResourceDictionary>
            <ObjectDataProvider x:Key="data" 
                              ObjectType="{x:Type local:ProductSales}" 
                              MethodName="GetSalesData" />
        </ResourceDictionary>
    </Window.Resources>
    
    <Grid>
        <syncfusion:PivotGridControl Name="pivotGrid" 
                                    HorizontalAlignment="Left" 
                                    VerticalAlignment="Top" 
                                    ItemSource="{Binding Source={StaticResource data}}">
        </syncfusion:PivotGridControl>
    </Grid>
</Window>
```

### Step 3: Bind Data in Code-Behind

```csharp
public MainWindow()
{
    InitializeComponent();
    this.Loaded += MainWindow_Loaded;
}

void MainWindow_Loaded(object sender, RoutedEventArgs e)
{
    PivotGridControl pivotGrid = new PivotGridControl();
    this.grid1.Children.Add(pivotGrid);
    
    // Bind data source
    pivotGrid.ItemSource = ProductSales.GetSalesData();
}
```

## Defining PivotItems and PivotCalculations

To display meaningful pivot data, you must define:
- **PivotItems**: Row and column dimensions
- **PivotCalculations**: Value fields with aggregation

### Defining in XAML

```xml
<syncfusion:PivotGridControl Name="pivotGrid" 
                            ItemSource="{Binding Source={StaticResource data}}">
    <!-- Define Pivot Rows -->
    <syncfusion:PivotGridControl.PivotRows>
        <syncfusion:PivotItem FieldHeader="Product" 
                             FieldMappingName="Product" 
                             TotalHeader="Total" />
        <syncfusion:PivotItem FieldHeader="Date" 
                             FieldMappingName="Date" 
                             TotalHeader="Total" />
    </syncfusion:PivotGridControl.PivotRows>
    
    <!-- Define Pivot Columns -->
    <syncfusion:PivotGridControl.PivotColumns>
        <syncfusion:PivotItem FieldHeader="Country" 
                             FieldMappingName="Country" 
                             TotalHeader="Total" />
        <syncfusion:PivotItem FieldHeader="State" 
                             FieldMappingName="State" 
                             TotalHeader="Total" />
    </syncfusion:PivotGridControl.PivotColumns>
    
    <!-- Define Calculations -->
    <syncfusion:PivotGridControl.PivotCalculations>
        <syncfusion:PivotComputationInfo CalculationName="Total" 
                                        FieldName="Amount" 
                                        Format="C" />
        <syncfusion:PivotComputationInfo CalculationName="Total" 
                                        FieldName="Quantity" 
                                        SummaryType="Count" />
    </syncfusion:PivotGridControl.PivotCalculations>
</syncfusion:PivotGridControl>
```

### Defining in Code-Behind

Include the required namespace:

```csharp
using Syncfusion.Windows.Controls.PivotGrid;
using Syncfusion.PivotAnalysis.Base;
```

Define PivotItems and calculations:

```csharp
public MainWindow()
{
    InitializeComponent();
    this.Loaded += MainWindow_Loaded;
}

void MainWindow_Loaded(object sender, RoutedEventArgs e)
{
    PivotGridControl pivotGrid = new PivotGridControl();
    grid1.Children.Add(pivotGrid);
    
    // Define Pivot Rows
    PivotItem productItem = new PivotItem
    {
        FieldHeader = "Product",
        FieldMappingName = "Product",
        TotalHeader = "Total"
    };
    
    PivotItem dateItem = new PivotItem
    {
        FieldHeader = "Date",
        FieldMappingName = "Date",
        TotalHeader = "Total"
    };
    
    // Define Pivot Columns
    PivotItem countryItem = new PivotItem
    {
        FieldHeader = "Country",
        FieldMappingName = "Country",
        TotalHeader = "Total"
    };
    
    PivotItem stateItem = new PivotItem
    {
        FieldHeader = "State",
        FieldMappingName = "State",
        TotalHeader = "Total"
    };
    
    // Add PivotItems to PivotRows and PivotColumns
    pivotGrid.PivotRows.Add(productItem);
    pivotGrid.PivotRows.Add(dateItem);
    pivotGrid.PivotColumns.Add(countryItem);
    pivotGrid.PivotColumns.Add(stateItem);
    
    // Define Calculations
    PivotComputationInfo amountCalc = new PivotComputationInfo
    {
        CalculationName = "Amount",
        FieldName = "Amount",
        SummaryType = SummaryType.Count
    };
    
    PivotComputationInfo quantityCalc = new PivotComputationInfo
    {
        CalculationName = "Quantity",
        FieldName = "Quantity",
        SummaryType = SummaryType.Count
    };
    
    pivotGrid.PivotCalculations.Add(amountCalc);
    pivotGrid.PivotCalculations.Add(quantityCalc);
    
    // Bind data source
    pivotGrid.ItemSource = ProductSales.GetSalesData();
}
```

## Running Your First PivotGrid

Run the application and you'll see a cross-tabulated pivot table displaying:
- **Rows:** Product and Date dimensions
- **Columns:** Country and State dimensions
- **Values:** Aggregated Amount and Quantity calculations

The PivotGrid automatically:
- Aggregates data based on SummaryType
- Displays totals for each dimension
- Provides an interactive cross-tabular view

## Next Steps

Now that you have a basic PivotGrid working:
- Learn about **data binding** options for different data sources
- Explore **PivotItems and PivotCalculations** in depth
- Add **filtering** to narrow down data display
- Implement **sorting** for better data organization
- Enable **Grouping Bar** for interactive pivoting

Refer to the other reference files in this skill for detailed guidance on each feature.

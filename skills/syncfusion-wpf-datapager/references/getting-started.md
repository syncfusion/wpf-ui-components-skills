# Getting Started with WPF DataPager

## Table of Contents
- [Assembly Requirements](#assembly-requirements)
- [Control Structure](#control-structure)
- [Creating Simple Application](#creating-simple-application)
- [Integration with SfDataGrid](#integration-with-sfdatagrid)
- [Theme Support](#theme-support)

## Assembly Requirements

The following assemblies are required to use SfDataPager in your WPF application:

### Basic Assembly

| Assembly | Description |
|----------|-------------|
| `Syncfusion.SfGrid.WPF` | Contains SfDataPager and related elements |

### Additional Assemblies for DataGrid Integration

When using SfDataPager with SfDataGrid, add these assemblies:

| Assembly | Description |
|----------|-------------|
| `Syncfusion.Data.WPF` | Provides data processing (sorting, grouping, filtering) |
| `Syncfusion.Shared.WPF` | Contains editors like CurrencyTextBox, PercentEdit, DateTimeEdit |

## Control Structure

The SfDataPager control consists of the following elements:

### Control Elements

- **FirstPageButton**: Moves the current page index to the first page and displays the first page data
- **PreviousPageButton**: Moves the current page index to the previous page and displays the previous page data
- **LastPageButton**: Moves the current page index to the last page and displays the last page data
- **NextPageButton**: Moves the current page index to the next page and displays the next page data
- **NumericButtons**: Denotes the available pages. Click any button to navigate directly to that page
- **EllipsisButton**: Displayed when AutoEllipsis mode is set. Shows the next set of numeric page buttons when clicked

## Creating Simple Application

Follow these steps to create a simple application with SfDataPager:

### Step 1: Create New WPF Application

Create a new WPF application in Visual Studio.

### Step 2: Add SfDataPager from Toolbox

1. Open Visual Studio toolbox
2. Navigate to SyncfusionControls tab
3. Drag the SfDataPager control to the design window

When you drag SfDataPager to the window, it automatically adds required references to your application.

### Step 3: Add Required Assemblies (Manual Method)

If adding the control manually without toolbox, add these assemblies to your project:
- `Syncfusion.Data.WPF`
- `Syncfusion.SfGrid.WPF`

### Step 4: Add Namespace

Add the SfDataPager namespace to your XAML:

```xml
<Window x:Class="SfDataPagerDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:datapager="clr-namespace:Syncfusion.UI.Xaml.Controls.DataPager;assembly=Syncfusion.SfGrid.WPF"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <datapager:SfDataPager x:Name="sfDataPager"/>
    </Grid>
</Window>
```

### Step 5: Create Business Object Class

Create a data model class (e.g., `OrderInfo`):

```csharp
public class OrderInfo
{
    public int OrderID { get; set; }
    public string CustomerID { get; set; }
    public string CustomerName { get; set; }
    public string Country { get; set; }
    public string ShipCity { get; set; }

    public OrderInfo(int orderId, string customerName, string country, 
                     string customerId, string shipCity)
    {
        this.OrderID = orderId;
        this.CustomerName = customerName;
        this.Country = country;
        this.CustomerID = customerId;
        this.ShipCity = shipCity;
    }
}
```

### Step 6: Create ViewModel

Add a ViewModel with sample data:

```csharp
public class ViewModel
{
    private ObservableCollection<OrderInfo> orderCollection;

    public ObservableCollection<OrderInfo> OrderInfoCollection
    {
        get { return orderCollection; }
        set { orderCollection = value; }
    }

    public ViewModel()
    {
        orderCollection = new ObservableCollection<OrderInfo>();
        this.GenerateOrders();
    }

    private void GenerateOrders()
    {
        orderCollection.Add(new OrderInfo(1001, "Maria Anders", "Germany", "ALFKI", "Berlin"));
        orderCollection.Add(new OrderInfo(1002, "Ana Trujilo", "Mexico", "ANATR", "Mexico D.F."));
        orderCollection.Add(new OrderInfo(1003, "Antonio Moreno", "Mexico", "ANTON", "Mexico D.F."));
        orderCollection.Add(new OrderInfo(1004, "Thomas Hardy", "UK", "AROUT", "London"));
        orderCollection.Add(new OrderInfo(1005, "Christina Berglund", "Sweden", "BERGS", "Lula"));
        orderCollection.Add(new OrderInfo(1006, "Hanna Moos", "Germany", "BLAUS", "Mannheim"));
        orderCollection.Add(new OrderInfo(1007, "Frederique Citeaux", "France", "BLONP", "Strasbourg"));
        orderCollection.Add(new OrderInfo(1008, "Martin Sommer", "Spain", "BOLID", "Madrid"));
        orderCollection.Add(new OrderInfo(1009, "Laurence Lebihan", "France", "BONAP", "Marseille"));
        orderCollection.Add(new OrderInfo(1010, "Elizabeth Lincoln", "Canada", "BOTTM", "Tsawassen"));
    }
}
```

### Step 7: Bind Data to SfDataPager

Set the ViewModel as DataContext and bind the data collection:

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <datapager:SfDataPager x:Name="sfDataPager"
                           AccentBackground="DodgerBlue"
                           NumericButtonCount="5"
                           PageSize="5"
                           Source="{Binding OrderInfoCollection}"/>
</Grid>
```

## Integration with SfDataGrid

To display paginated data in SfDataGrid, bind the PagedSource property:

### Complete Example

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    
    <!-- DataGrid binds to PagedSource -->
    <sfGrid:SfDataGrid AutoGenerateColumns="True"
                       ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <!-- DataPager binds to data collection -->
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           NumericButtonCount="10"
                           PageSize="10"
                           Source="{Binding OrdersDetails}"/>
</Grid>
```

### Key Binding Pattern

The binding flow works as follows:
1. **Source**: Bind to your data collection in ViewModel
2. **PagedSource**: SfDataPager wraps the collection in PagedCollectionView
3. **ItemsSource**: Bind DataGrid (or any ItemsControl) to PagedSource

This creates a clean separation where:
- SfDataPager manages pagination logic
- SfDataGrid displays only the current page's data

### Result

The output displays the SfDataPager integrated with SfDataGrid, showing paginated data with navigation controls.

## Theme Support

SfDataPager supports various built-in themes for consistent visual appearance.

### Applying Themes

**Option 1: Using SfSkinManager**

Apply themes dynamically using SfSkinManager:

```csharp
SfSkinManager.SetTheme(this, new Theme() { ThemeName = "MaterialLight" });
```

Available themes:
- MaterialLight
- MaterialDark
- Office2019Colorful
- Office2019Black
- Office2019White
- FluentLight
- FluentDark

**Option 2: Using ThemeStudio**

Create custom themes using Syncfusion ThemeStudio:
1. Visit Syncfusion ThemeStudio online
2. Customize colors and styles
3. Export theme package
4. Apply to your application

### Theme Example

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    <syncfusionskin:SfSkinManager.Theme>
        <syncfusionskin:FluentTheme />
    </syncfusionskin:SfSkinManager.Theme>
    
    <Grid>
        <datapager:SfDataPager x:Name="sfDataPager"
                               PageSize="10"
                               Source="{Binding DataCollection}"/>
    </Grid>
</Window>
```

## Best Practices

### Assembly Management
- Add only required assemblies to minimize application size
- Use Syncfusion.Data.WPF only when needed for advanced data operations

### Data Binding
- Always bind Source to your data collection
- Always bind ItemsControl.ItemsSource to PagedSource (not Source)
- Use ElementName binding for PagedSource: `{Binding ElementName=sfDataPager, Path=PagedSource}`

### Performance
- Set appropriate PageSize based on screen real estate and data complexity
- For very large datasets, consider on-demand paging (see data-binding.md)
- Test with realistic data volumes

### Design-Time Support
- Use Visual Studio or Expression Blend designer for visual editing
- Drag-and-drop from toolbox automatically adds required references
- Set sample data in ViewModel for design-time preview

## Troubleshooting

### No Data Displayed
- Verify Source is bound to data collection
- Check that ItemsControl binds to PagedSource (not Source)
- Ensure ViewModel is set as DataContext
- Confirm PageSize > 0

### Navigation Buttons Not Working
- Check that Source collection has more items than PageSize
- Verify assemblies are properly referenced
- Ensure NumericButtonCount is set appropriately

### Assembly Reference Errors
- Clean and rebuild solution
- Verify assembly versions match
- Check that target framework is compatible (typically .NET Framework 4.5+)

# Master-Details View in WPF DataGrid

## Overview

DataGrid supports hierarchical data representation through Master-Details View, allowing nested tables with expand/collapse functionality. The nesting level is unlimited.

## Generating from IEnumerable

### Create Data Model with Relations

```csharp
public class Employee : INotifyPropertyChanged
{
    private int _employeeID;
    private ObservableCollection<SalesInfo> _sales;
    private ObservableCollection<OrderInfo> _orders;

    public int EmployeeID
    {
        get { return _employeeID; }
        set { _employeeID = value; OnPropertyChanged("EmployeeID"); }
    }

    public ObservableCollection<SalesInfo> Sales
    {
        get { return _sales; }
        set { _sales = value; OnPropertyChanged("Sales"); }
    }

    public ObservableCollection<OrderInfo> Orders
    {
        get { return _orders; }
        set { _orders = value; OnPropertyChanged("Orders"); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    private void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

### Auto-Generating Relations

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="True"
                       ItemsSource="{Binding Employees}" />
```

```csharp
dataGrid.AutoGenerateRelations = true;
```

Handle `AutoGeneratingRelations` event to customize:

```csharp
this.dataGrid.AutoGeneratingRelations += dataGrid_AutoGeneratingRelations;

void dataGrid_AutoGeneratingRelations(object sender, AutoGeneratingRelationsArgs e)
{
    e.GridViewDefinition.DataGrid.AllowEditing = true;
    e.GridViewDefinition.DataGrid.AllowFiltering = true;
}
```

### Manually Defining Relations

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="False"
                       ItemsSource="{Binding Employees}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:GridViewDefinition RelationalColumn="Sales">
            <syncfusion:GridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid x:Name="FirstLevelNestedGrid1"
                                       AutoGenerateColumns="True"/>
            </syncfusion:GridViewDefinition.DataGrid>
        </syncfusion:GridViewDefinition>
        <syncfusion:GridViewDefinition RelationalColumn="Orders">
            <syncfusion:GridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid x:Name="FirstLevelNestedGrid2"
                                       AutoGenerateColumns="True"/>
            </syncfusion:GridViewDefinition.DataGrid>
        </syncfusion:GridViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

```csharp
dataGrid.AutoGenerateRelations = false;

var gridViewDefinition1 = new GridViewDefinition();
gridViewDefinition1.RelationalColumn = "Sales";
gridViewDefinition1.DataGrid = new SfDataGrid() { Name = "FirstLevelNestedGrid1", AutoGenerateColumns = true };

var gridViewDefinition2 = new GridViewDefinition();
gridViewDefinition2.RelationalColumn = "Orders";
gridViewDefinition2.DataGrid = new SfDataGrid() { Name = "FirstLevelNestedGrid2", AutoGenerateColumns = true };

dataGrid.DetailsViewDefinition.Add(gridViewDefinition1);
dataGrid.DetailsViewDefinition.Add(gridViewDefinition2);
```

### Nested Relations

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="False"                        
                       ItemsSource="{Binding Employees}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:GridViewDefinition RelationalColumn="Orders">
            <syncfusion:GridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid x:Name="FirstLevelNestedGrid"
                                       AutoGenerateColumns="True"
                                       AutoGenerateRelations="False">
                    <syncfusion:SfDataGrid.DetailsViewDefinition>
                        <syncfusion:GridViewDefinition RelationalColumn="Products">
                            <syncfusion:GridViewDefinition.DataGrid>
                                <syncfusion:SfDataGrid x:Name="SecondLevelNestedGrid" 
                                                       AutoGenerateColumns="True" />
                            </syncfusion:GridViewDefinition.DataGrid>
                        </syncfusion:GridViewDefinition>
                    </syncfusion:SfDataGrid.DetailsViewDefinition>                   
                </syncfusion:SfDataGrid>
            </syncfusion:GridViewDefinition.DataGrid>
        </syncfusion:GridViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

## Generating from DataTable

### Create DataTable with Relations

```csharp
public class ViewModel 
{        
    private DataTable _suppliers;
    
    public DataTable Suppliers
    {
        get { return _suppliers; }
        set { _suppliers = value; }
    }

    public ViewModel()
    {
        _suppliers = GetDataTable();
    }
       
    public DataTable GetDataTable()
    {
        DataSet ds = new DataSet();
        
        // Load Suppliers table
        using (SqlConnection con = new SqlConnection(connectionString))
        {
            con.Open();
            SqlDataAdapter sda = new SqlDataAdapter("SELECT * FROM Suppliers", con);
            sda.Fill(ds, "Suppliers");
        }
        
        // Load Products table
        using (SqlConnection con = new SqlConnection(connectionString))
        {
            con.Open();
            SqlDataAdapter sda = new SqlDataAdapter("SELECT * FROM Products", con);
            sda.Fill(ds, "Products");
        }
        
        // Create relation
        ds.Relations.Add(new DataRelation("Supplier_Product", 
                                         ds.Tables[0].Columns["Supplier ID"], 
                                         ds.Tables[1].Columns["Supplier ID"]));
            
        return ds.Tables[0];
    }
}
```

### Auto-Generating Relations

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="True"
                       ItemsSource="{Binding Suppliers}" />
```

### Manually Defining Relations

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="False"
                       ItemsSource="{Binding Suppliers}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:GridViewDefinition RelationalColumn="Supplier_Product">
            <syncfusion:GridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid x:Name="FirstLevelNestedGrid" AutoGenerateColumns="True" />
            </syncfusion:GridViewDefinition.DataGrid>
        </syncfusion:GridViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

## Populating Through Events

Load ItemsSource asynchronously using `DetailsViewExpanding` event:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="False"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:GridViewDefinition RelationalColumn="ProductDetails">
            <syncfusion:GridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid x:Name="FirstLevelNestedGrid"  
                                       AutoGenerateColumns="True" />
            </syncfusion:GridViewDefinition.DataGrid>
        </syncfusion:GridViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

```csharp
this.dataGrid.DetailsViewExpanding += dataGrid_DetailsViewExpanding;

void dataGrid_DetailsViewExpanding(object sender, GridDetailsViewExpandingEventArgs e)
{
    e.DetailsViewItemsSource.Clear();      
    var itemsSource = GetItemSource();
    e.DetailsViewItemsSource.Add("ProductDetails", itemsSource);   
}

private ObservableCollection<ProductInfo> GetItemSource()
{
    var products = new ObservableCollection<ProductInfo>();    
    products.Add(new ProductInfo() { OrderID = 1001, ProductName = "Bike" });
    products.Add(new ProductInfo() { OrderID = 1002, ProductName = "Car" });
    return products;
}
```

### Loading Asynchronously

```csharp
this.dataGrid.DetailsViewExpanding += dataGrid_DetailsViewExpanding;

async void dataGrid_DetailsViewExpanding(object sender, GridDetailsViewExpandingEventArgs e)
{
    e.DetailsViewItemsSource.Clear();
    var itemsSource = await GetItemSourceAsync();
    e.DetailsViewItemsSource.Add("ProductDetails", itemsSource);
}

private async Task<ObservableCollection<ProductInfo>> GetItemSourceAsync()
{
    var products = new ObservableCollection<ProductInfo>();
    await Task.Run(() =>
    {
        // Simulate data loading delay
        Thread.Sleep(2000);
        products.Add(new ProductInfo() { OrderID = 1001, ProductName = "Bike" });
        products.Add(new ProductInfo() { OrderID = 1002, ProductName = "Car" });
    });
    return products;
}
```

## Defining Properties for DetailsViewDataGrid

### When AutoGenerateRelations is False

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="False"
                       ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:GridViewDefinition RelationalColumn="ProductDetails">
            <syncfusion:GridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid x:Name="FirstLevelNestedGrid"
                                       AllowEditing="True"
                                       AllowFiltering="True"
                                       AllowResizingColumns="True"
                                       AllowSorting="True"
                                       AutoGenerateColumns="False" />
            </syncfusion:GridViewDefinition.DataGrid>
        </syncfusion:GridViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

### When AutoGenerateRelations is True

```csharp
this.dataGrid.AutoGeneratingRelations += dataGrid_AutoGeneratingRelations;

void dataGrid_AutoGeneratingRelations(object sender, AutoGeneratingRelationsArgs e)
{
    e.GridViewDefinition.DataGrid.AllowEditing = true;
    e.GridViewDefinition.DataGrid.AllowFiltering = true;
    e.GridViewDefinition.DataGrid.AllowSorting = true;
    e.GridViewDefinition.DataGrid.AllowResizingColumns = true;        
}
```

## ExpandAllDetailsView and CollapseAllDetailsView

```csharp
// Expand all details views
this.dataGrid.ExpandAllDetailsView();

// Collapse all details views
this.dataGrid.CollapseAllDetailsView();

// Expand details views up to specific level
this.dataGrid.ExpandAllDetailsView(2);
```

## ExpandDetailsViewAt and CollapseDetailsViewAt

```csharp
// Expand details view at specific record index
this.dataGrid.ExpandDetailsViewAt(0);

// Collapse details view at specific record index
this.dataGrid.CollapseDetailsViewAt(0);
```

## Custom Detail View Templates

Define custom template for details view:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Employees}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:GridViewDefinition RelationalColumn="Orders">
            <syncfusion:GridViewDefinition.DetailsViewTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Vertical" Margin="20">
                        <TextBlock Text="{Binding OrderID}" FontWeight="Bold"/>
                        <TextBlock Text="{Binding ProductName}"/>
                        <TextBlock Text="{Binding Quantity}"/>
                    </StackPanel>
                </DataTemplate>
            </syncfusion:GridViewDefinition.DetailsViewTemplate>
        </syncfusion:GridViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

## Detail View Events

### DetailsViewLoading

```csharp
this.dataGrid.DetailsViewLoading += dataGrid_DetailsViewLoading;

void dataGrid_DetailsViewLoading(object sender, DetailsViewLoadingAndUnloadingEventArgs e)
{
    // Customize DetailsViewDataGrid as it loads
    if (!(e.DetailsViewDataGrid.SelectionController is CustomSelectionController))
        e.DetailsViewDataGrid.SelectionController = new CustomSelectionController(e.DetailsViewDataGrid);
}
```

### DetailsViewUnloading

```csharp
this.dataGrid.DetailsViewUnloading += dataGrid_DetailsViewUnloading;

void dataGrid_DetailsViewUnloading(object sender, DetailsViewLoadingAndUnloadingEventArgs e)
{
    // Cleanup when DetailsViewDataGrid unloads
}
```

### DetailsViewExpanding

```csharp
this.dataGrid.DetailsViewExpanding += dataGrid_DetailsViewExpanding;

void dataGrid_DetailsViewExpanding(object sender, GridDetailsViewExpandingEventArgs e)
{
    // Handle before expansion
}
```

### DetailsViewExpanded

```csharp
this.dataGrid.DetailsViewExpanded += dataGrid_DetailsViewExpanded;

void dataGrid_DetailsViewExpanded(object sender, GridDetailsViewExpandedEventArgs e)
{
    // Handle after expansion
}
```

### DetailsViewCollapsing

```csharp
this.dataGrid.DetailsViewCollapsing += dataGrid_DetailsViewCollapsing;

void dataGrid_DetailsViewCollapsing(object sender, GridDetailsViewCollapsingEventArgs e)
{
    // Handle before collapse
}
```

### DetailsViewCollapsed

```csharp
this.dataGrid.DetailsViewCollapsed += dataGrid_DetailsViewCollapsed;

void dataGrid_DetailsViewCollapsed(object sender, GridDetailsViewCollapsedEventArgs e)
{
    // Handle after collapse
}
```

## Getting DetailsViewDataGrid

```csharp
// Get selected DetailsViewDataGrid
var detailsViewDataGrid = this.dataGrid.SelectedDetailsViewGrid;

// Get DetailsViewDataGrid at specific row index
var detailsViewDataGrid = this.dataGrid.GetDetailsViewGrid(2);

// Get DetailsViewDataGrid by record index and relation name
var detailsViewDataGrid = this.dataGrid.GetDetailsViewGrid(0, "ProductDetails");
```

## Hiding Empty DetailsViewDataGrid

Hide expander when RelationalColumn has empty collection:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="True"
                       HideEmptyGridViewDefinition="True"
                       ItemsSource="{Binding Orders}" />
```

## DetailsViewPadding

Customize padding of DetailsViewDataGrid:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="False"
                       DetailsViewPadding="15"
                       ItemsSource="{Binding Orders}">
```

```csharp
this.dataGrid.DetailsViewPadding = new Thickness(15);
```

## Multiple Detail View Levels

Create multiple levels of nested grids:

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding Level1Data}">
    <syncfusion:SfDataGrid.DetailsViewDefinition>
        <syncfusion:GridViewDefinition RelationalColumn="Level2Data">
            <syncfusion:GridViewDefinition.DataGrid>
                <syncfusion:SfDataGrid AutoGenerateColumns="True">
                    <syncfusion:SfDataGrid.DetailsViewDefinition>
                        <syncfusion:GridViewDefinition RelationalColumn="Level3Data">
                            <syncfusion:GridViewDefinition.DataGrid>
                                <syncfusion:SfDataGrid AutoGenerateColumns="True" />
                            </syncfusion:GridViewDefinition.DataGrid>
                        </syncfusion:GridViewDefinition>
                    </syncfusion:SfDataGrid.DetailsViewDefinition>
                </syncfusion:SfDataGrid>
            </syncfusion:GridViewDefinition.DataGrid>
        </syncfusion:GridViewDefinition>
    </syncfusion:SfDataGrid.DetailsViewDefinition>
</syncfusion:SfDataGrid>
```

## Customize Expander Column Width

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"                               
                       ExpanderColumnWidth="50"
                       AutoGenerateRelations="True"
                       ItemsSource="{Binding OrderInfoCollection}">
```

```csharp
this.dataGrid.ExpanderColumnWidth = 50;
```

## Hiding GridDetailsViewIndentCell

```xaml
<syncfusion:SfDataGrid x:Name="dataGrid"
                       AutoGenerateColumns="True"
                       AutoGenerateRelations="True"
                       ShowDetailsViewIndentCell="False"
                       ItemsSource="{Binding Orders}">
```

```csharp
dataGrid.ShowDetailsViewIndentCell = false;
```

## Troubleshooting

### Issue: DetailsView not expanding
**Solution**: Check that `RelationalColumn` matches property name. Verify data object contains the collection property. Ensure `AutoGenerateRelations` is set correctly.

### Issue: Empty expander showing
**Solution**: Set `HideEmptyGridViewDefinition="True"` to hide expanders for empty collections.

### Issue: DetailsView columns not showing
**Solution**: Verify `AutoGenerateColumns` is set. Check column definitions if manually defined. Ensure data objects have properties.

### Issue: Events not firing
**Solution**: Verify event handlers are wired correctly. Check that `NotifyEventsToParentDataGrid` is set if using parent events.

### Issue: Performance issues with many levels
**Solution**: Avoid too many nesting levels. Use virtualization. Load data on demand using `DetailsViewExpanding` event.

### Issue: Styling not applied to DetailsView
**Solution**: Apply styles in `DetailsViewLoading` event or set properties in `AutoGeneratingRelations` event.

### Issue: Selection not working in DetailsView
**Solution**: Verify `SelectionUnit` is set. Check that selection controller is properly initialized.

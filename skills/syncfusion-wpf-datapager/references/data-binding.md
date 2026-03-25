# Data Binding in WPF DataPager

## Table of Contents
- [Overview](#overview)
- [Source and PagedSource Properties](#source-and-pagedsource-properties)
- [PageIndex Property](#pageindex-property)
- [Events](#events)
- [Binding to Other Controls](#binding-to-other-controls)
- [On-Demand Paging](#on-demand-paging)
- [PageSize Configuration](#pagesize-configuration)
- [Runtime PageSize Changes](#runtime-pagesize-changes)

## Overview

Data binding is the core feature of SfDataPager. The control binds to an external data source to display paginated data, automatically wrapping collections in PagedCollectionView for efficient page management.

## Source and PagedSource Properties

### Source Property

The `Source` property is where you pass your data collection for paging. This can be any IEnumerable collection:

- ObservableCollection
- List
- Array
- IEnumerable from database queries
- LINQ query results

### PagedSource Property

SfDataPager automatically wraps the Source collection in a `PagedCollectionView` and exposes it through the `PagedSource` property. This PagedSource is what you bind to your ItemsControl's ItemsSource.

### Basic Data Binding Example

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    
    <!-- DataGrid displays current page data -->
    <sfgrid:SfDataGrid AutoGenerateColumns="True"
                       ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <!-- DataPager manages pagination -->
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           NumericButtonCount="10"
                           PageSize="16"
                           Source="{Binding OrdersDetails}"/>
</Grid>
```

### Key Concepts

1. **Source receives the full collection**: `Source="{Binding OrdersDetails}"`
2. **PagedSource provides current page**: SfDataPager wraps Source in PagedCollectionView
3. **ItemsControl binds to PagedSource**: `ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"`

This pattern ensures:
- SfDataPager controls which items are visible
- ItemsControl displays only the current page
- Page changes automatically update the display

## PageIndex Property

The `PageIndex` property contains the index of the currently selected page (0-based).

### Using PageIndex

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       PageIndex="2"
                       PageSize="10"
                       Source="{Binding DataCollection}"/>
```

### Programmatic Navigation

```csharp
// Get current page
int currentPage = sfDataPager.PageIndex;

// Set page programmatically
sfDataPager.PageIndex = 5; // Navigate to page 6 (0-based)
```

### Binding PageIndex

You can bind PageIndex to a ViewModel property for two-way synchronization:

```xml
<datapager:SfDataPager PageIndex="{Binding CurrentPageIndex, Mode=TwoWay}"/>
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private int currentPageIndex;
    public int CurrentPageIndex
    {
        get { return currentPageIndex; }
        set
        {
            currentPageIndex = value;
            OnPropertyChanged(nameof(CurrentPageIndex));
        }
    }
}
```

## Events

SfDataPager provides two events for page navigation tracking and validation:

### PageIndexChanging Event

Triggered **before** the page index changes. This event is cancelable, allowing you to validate or prevent page navigation.

**Event Arguments:**
- `OldPageIndex`: Current page index
- `NewPageIndex`: Target page index
- `Cancel`: Set to true to prevent navigation

**Example - Validation Before Page Change:**

```xml
<datapager:SfDataPager PageIndexChanging="sfDataPager_PageIndexChanging"/>
```

```csharp
private void sfDataPager_PageIndexChanging(object sender, PageIndexChangingEventArgs e)
{
    // Check for unsaved changes
    if (HasUnsavedChanges())
    {
        MessageBoxResult result = MessageBox.Show(
            "You have unsaved changes. Do you want to change the page?",
            "Confirm",
            MessageBoxButton.YesNo);
        
        if (result == MessageBoxResult.No)
        {
            e.Cancel = true; // Cancel page navigation
        }
    }
}
```

### PageIndexChanged Event

Triggered **after** the page index has changed. Use this for logging, analytics, or updating UI state.

**Event Arguments:**
- `OldPageIndex`: Previous page index
- `NewPageIndex`: Current page index

**Example - Logging Page Changes:**

```csharp
private void sfDataPager_PageIndexChanged(object sender, PageIndexChangedEventArgs e)
{
    Console.WriteLine($"Navigated from page {e.OldPageIndex} to page {e.NewPageIndex}");
    
    // Update status bar
    statusTextBlock.Text = $"Page {e.NewPageIndex + 1} of {sfDataPager.PageCount}";
}
```

## Binding to Other Controls

The PagedSource property can bind to any ItemsControl, not just SfDataGrid.

### ListBox Example

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    
    <ListBox ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}">
        <ListBox.ItemTemplate>
            <DataTemplate>
                <StackPanel Orientation="Horizontal" Margin="5">
                    <TextBlock Text="{Binding OrderID}" Width="100"/>
                    <TextBlock Text="{Binding CustomerName}" Width="150"/>
                    <TextBlock Text="{Binding Country}" Width="100"/>
                    <TextBlock Text="{Binding ShipCity}" Width="100"/>
                </StackPanel>
            </DataTemplate>
        </ListBox.ItemTemplate>
    </ListBox>
    
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           NumericButtonCount="10"
                           PageSize="10"
                           Source="{Binding OrdersDetails}"/>
</Grid>
```

### ListView Example

```xml
<ListView ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}">
    <ListView.View>
        <GridView>
            <GridViewColumn Header="Order ID" DisplayMemberBinding="{Binding OrderID}"/>
            <GridViewColumn Header="Customer" DisplayMemberBinding="{Binding CustomerName}"/>
            <GridViewColumn Header="Country" DisplayMemberBinding="{Binding Country}"/>
        </GridView>
    </ListView.View>
</ListView>

<datapager:SfDataPager x:Name="sfDataPager"
                       PageSize="15"
                       Source="{Binding OrdersDetails}"/>
```

### ItemsControl Example

Any ItemsControl can use PagedSource:

```xml
<ItemsControl ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}">
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapPanel/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
    <ItemsControl.ItemTemplate>
        <DataTemplate>
            <Border BorderBrush="Gray" BorderThickness="1" Margin="5" Padding="10">
                <StackPanel>
                    <TextBlock Text="{Binding CustomerName}" FontWeight="Bold"/>
                    <TextBlock Text="{Binding Country}"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ItemsControl>
```

## On-Demand Paging

For large datasets (thousands or millions of records), loading all data at once is inefficient. On-demand paging loads data only for the current page.

### Benefits

- **Reduced memory usage**: Load only visible page data
- **Faster initial load**: No need to load entire dataset
- **Better performance**: Especially for database queries
- **Scalability**: Works with datasets of any size

### Enabling On-Demand Paging

Set `UseOnDemandPaging="True"` and handle the `OnDemandLoading` event:

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    
    <sfgrid:SfDataGrid x:Name="dataGrid"
                       ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           UseOnDemandPaging="True"
                           PageCount="50"
                           PageSize="16"
                           OnDemandLoading="OnDemandDataLoading"/>
</Grid>
```

### OnDemandLoading Event Handler

The OnDemandLoading event provides:
- `StartIndex`: First item index for current page
- `PageSize`: Number of items to load

```csharp
private void OnDemandDataLoading(object sender, OnDemandLoadingEventArgs args)
{
    // Load data for current page
    var pageData = dataSource.Skip(args.StartIndex).Take(args.PageSize);
    
    // Load into DataPager
    sfDataPager.LoadDynamicItems(args.StartIndex, pageData);
}
```

### Complete On-Demand Example

```csharp
public class ViewModel
{
    private List<OrderInfo> fullDataSource;
    
    public ViewModel()
    {
        // Initialize with large dataset
        fullDataSource = GenerateLargeDataset(100000); // 100k records
    }
    
    private List<OrderInfo> GenerateLargeDataset(int count)
    {
        var data = new List<OrderInfo>();
        for (int i = 0; i < count; i++)
        {
            data.Add(new OrderInfo(
                i, 
                $"Customer {i}", 
                "Country", 
                $"ID{i}", 
                "City"));
        }
        return data;
    }
}
```

```csharp
// In code-behind
private void OnDemandDataLoading(object sender, OnDemandLoadingEventArgs args)
{
    var viewModel = (ViewModel)DataContext;
    
    // Simulate database query - only fetch requested page
    var pageData = GetPageDataFromDatabase(args.StartIndex, args.PageSize);
    
    // Or use in-memory data
    // var pageData = fullDataSource.Skip(args.StartIndex).Take(args.PageSize);
    
    sfDataPager.LoadDynamicItems(args.StartIndex, pageData);
}

private IEnumerable<OrderInfo> GetPageDataFromDatabase(int startIndex, int pageSize)
{
    // Database query example (pseudo-code)
    // return dbContext.Orders
    //     .OrderBy(o => o.OrderID)
    //     .Skip(startIndex)
    //     .Take(pageSize)
    //     .ToList();
    
    return fullDataSource.Skip(startIndex).Take(pageSize);
}
```

### Important Notes

**When using on-demand paging:**
- Do NOT set the `Source` property
- DO set the `PageCount` property (total number of pages)
- DO set the `PageSize` property
- DO implement the `OnDemandLoading` event
- DO call `LoadDynamicItems()` in the event handler

## PageSize Configuration

The `PageSize` property determines how many items are displayed per page.

### Setting PageSize

```xml
<datapager:SfDataPager PageSize="10" Source="{Binding DataCollection}"/>
```

### Default Behavior

- **PageSize = 0** (default): All data displayed on single page, no pagination
- **PageSize > 0**: Data split into pages of specified size

### Calculating PageCount

PageCount is automatically calculated:
```
PageCount = Ceiling(TotalItems / PageSize)
```

Examples:
- 100 items, PageSize=10 → 10 pages
- 95 items, PageSize=10 → 10 pages (last page has 5 items)
- 100 items, PageSize=15 → 7 pages (last page has 10 items)

## Runtime PageSize Changes

Allow users to change page size dynamically using a ComboBox or other input control.

### ComboBox PageSize Selector

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>
    
    <sfgrid:SfDataGrid Grid.ColumnSpan="2"
                       ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <!-- PageSize selector -->
    <StackPanel Grid.Column="1" Grid.Row="1" 
                Orientation="Horizontal" 
                Margin="5">
        <TextBlock Text="Items per page:" VerticalAlignment="Center" Margin="0,0,5,0"/>
        <ComboBox Name="pageSizeComboBox" 
                  SelectedIndex="0" 
                  ItemsSource="{Binding PageSizeOptions}"
                  Width="60"/>
    </StackPanel>
    
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           AccentBackground="DodgerBlue"
                           NumericButtonCount="10"
                           PageSize="{Binding SelectedValue, ElementName=pageSizeComboBox}"
                           Source="{Binding OrdersDetails}"/>
</Grid>
```

### ViewModel with PageSize Options

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<int> pageSizeOptions;
    
    public ObservableCollection<int> PageSizeOptions
    {
        get { return pageSizeOptions; }
        set
        {
            pageSizeOptions = value;
            OnPropertyChanged(nameof(PageSizeOptions));
        }
    }
    
    public ViewModel()
    {
        PageSizeOptions = new ObservableCollection<int> { 5, 10, 20, 50, 100 };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Custom PageSize Input

Allow users to enter custom page size:

```xml
<StackPanel Orientation="Horizontal">
    <TextBlock Text="Items per page:" VerticalAlignment="Center"/>
    <TextBox Name="pageSizeTextBox" 
             Text="10" 
             Width="50" 
             Margin="5,0"/>
    <Button Content="Apply" Click="ApplyPageSize_Click"/>
</StackPanel>

<datapager:SfDataPager x:Name="sfDataPager" Source="{Binding DataCollection}"/>
```

```csharp
private void ApplyPageSize_Click(object sender, RoutedEventArgs e)
{
    if (int.TryParse(pageSizeTextBox.Text, out int pageSize) && pageSize > 0)
    {
        sfDataPager.PageSize = pageSize;
    }
    else
    {
        MessageBox.Show("Please enter a valid page size (positive integer).");
    }
}
```

## Best Practices

### Data Binding
- Always bind ItemsControl to PagedSource, never to Source directly
- Use ElementName binding for PagedSource: `{Binding ElementName=sfDataPager, Path=PagedSource}`
- Set ViewModel as DataContext at Window/UserControl level

### Performance
- Use on-demand paging for datasets > 10,000 items
- Choose appropriate PageSize (10-50 items typically optimal)
- Implement efficient database queries with Skip/Take for on-demand paging

### Event Handling
- Use PageIndexChanging for validation (can cancel)
- Use PageIndexChanged for logging and UI updates (cannot cancel)
- Always check for unsaved changes before allowing navigation

### PageSize
- Provide PageSize options based on screen size and data complexity
- Common options: 10, 20, 50, 100
- Consider data grid row height when choosing default PageSize
- Remember user's PageSize preference (save to settings)

## Troubleshooting

### PagedSource is null
- Ensure Source is bound before accessing PagedSource
- Check that data collection is not null
- Verify ViewModel is set as DataContext

### Page navigation not working
- Confirm PageSize > 0
- Check that Source collection has data
- Verify collection has more items than PageSize

### OnDemandLoading not firing
- Ensure UseOnDemandPaging="True"
- Check that PageCount is set
- Verify Source is NOT set (incompatible with on-demand paging)

### PageIndexChanging Cancel not working
- Set e.Cancel = true (not false)
- Check event is wired up correctly
- Ensure event handler is called before PageIndexChanged

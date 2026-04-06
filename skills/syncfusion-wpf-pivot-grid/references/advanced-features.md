# Advanced Features in WPF PivotGrid

This guide covers advanced features including hyperlink cells, inner-most computations, raw data access, performance optimization with virtualization, and advanced scenarios for the Syncfusion WPF PivotGrid control.

## Table of Contents

- [Hyperlink Cells](#hyperlink-cells)
- [Inner-Most Computations](#inner-most-computations)
- [GetRawItem Method](#getrawitem-method)
- [Performance with Virtualization](#performance-with-virtualization)
- [Advanced Scenarios](#advanced-scenarios)
- [Best Practices](#best-practices)

## Hyperlink Cells

Hyperlink cells allow users to click cells to retrieve detailed information. The PivotGrid generates a `HyperLinkCellClick` event, and `HyperLinkCellClickEventArgs` returns the clicked `PivotCellInfo`.

### Cell Types Supporting Hyperlinks

- Column header cells
- Row header cells
- Summary header cells
- Summary cells
- Value cells

### IsHyperlinkCell Property

The `IsHyperlinkCell` property enables or disables hyperlink functionality for specific cell types.

### Enable Column Header Hyperlinks

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Enable Column Header cells as Hyperlink cells
        pivotGrid.ColumnHeaderCellStyle.IsHyperlinkCell = true;
    }
}
```

### Enable Row Header Hyperlinks

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Enable Row Header cells as Hyperlink cells
        pivotGrid.RowHeaderCellStyle.IsHyperlinkCell = true;
    }
}
```

### Enable Summary Header Hyperlinks

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Enable both Row and Column summary Header cells as Hyperlink cells
        pivotGrid.SummaryHeaderStyle.IsHyperlinkCell = true;
    }
}
```

### Enable Summary Cell Hyperlinks

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Enable both Row and Column summary cells as Hyperlink cells
        pivotGrid.SummaryCellStyle.IsHyperlinkCell = true;
    }
}
```

### Enable Value Cell Hyperlinks

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Enable Value cells as Hyperlink cells
        pivotGrid.ValueCellStyle.IsHyperlinkCell = true;
    }
}
```

### Complete Hyperlink Implementation

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        ConfigureHyperlinks();
        pivotGrid.HyperlinkCellClick += PivotGrid_HyperlinkCellClick;
    }
    
    private void ConfigureHyperlinks()
    {
        // Enable hyperlinks for all cell types
        pivotGrid.ColumnHeaderCellStyle.IsHyperlinkCell = true;
        pivotGrid.RowHeaderCellStyle.IsHyperlinkCell = true;
        pivotGrid.SummaryHeaderStyle.IsHyperlinkCell = true;
        pivotGrid.SummaryCellStyle.IsHyperlinkCell = true;
        pivotGrid.ValueCellStyle.IsHyperlinkCell = true;
    }
    
    private void PivotGrid_HyperlinkCellClick(object sender, HyperlinkCellClickEventArgs e)
    {
        // Get clicked cell information
        var cellInfo = e.CellInfo;
        var rowIndex = e.RowColumnIndex.RowIndex;
        var colIndex = e.RowColumnIndex.ColumnIndex;
        
        // Display cell details
        MessageBox.Show(
            $"Cell Clicked:\n" +
            $"Row: {rowIndex}\n" +
            $"Column: {colIndex}\n" +
            $"Value: {cellInfo.FormattedText}\n" +
            $"Type: {cellInfo.CellType}",
            "Hyperlink Cell Info",
            MessageBoxButton.OK,
            MessageBoxImage.Information);
    }
}
```

### Advanced Hyperlink Event Handling

```csharp
private void PivotGrid_HyperlinkCellClick(object sender, HyperlinkCellClickEventArgs e)
{
    var cellInfo = e.CellInfo;
    
    switch (cellInfo.CellType)
    {
        case PivotCellType.ColumnHeaderCell:
            ShowColumnDetails(cellInfo);
            break;
            
        case PivotCellType.RowHeaderCell:
            ShowRowDetails(cellInfo);
            break;
            
        case PivotCellType.ValueCell:
            ShowValueDetails(cellInfo);
            break;
            
        case PivotCellType.SummaryCell:
            ShowSummaryDetails(cellInfo);
            break;
    }
}

private void ShowColumnDetails(PivotCellInfo cellInfo)
{
    // Show column-specific details
    var detailsWindow = new DetailsWindow(cellInfo);
    detailsWindow.ShowDialog();
}

private void ShowRowDetails(PivotCellInfo cellInfo)
{
    // Show row-specific details
    var detailsWindow = new DetailsWindow(cellInfo);
    detailsWindow.ShowDialog();
}

private void ShowValueDetails(PivotCellInfo cellInfo)
{
    // Show value cell details with drill-down capability
    var drillDownWindow = new DrillDownWindow(cellInfo);
    drillDownWindow.ShowDialog();
}

private void ShowSummaryDetails(PivotCellInfo cellInfo)
{
    // Show summary calculation details
    var summaryWindow = new SummaryWindow(cellInfo);
    summaryWindow.ShowDialog();
}
```

## Inner-Most Computations

Display the grid with inner-most computations only, without displaying total values.

### XAML Configuration

```xml
<syncfusion:PivotGridControl 
    Name="pivotGrid" 
    RowPivotsOnly="True">

    <syncfusion:PivotGridControl.PivotRows>
        <syncfusion:PivotItem 
            AllowFilter="False" 
            FieldHeader="PID" 
            FieldMappingName="PID" 
            TotalHeader="Total" />
        <syncfusion:PivotItem 
            AllowFilter="False" 
            FieldHeader="Location" 
            FieldMappingName="Location" 
            TotalHeader="Total" />
    </syncfusion:PivotGridControl.PivotRows>

    <syncfusion:PivotGridControl.PivotCalculations>
        <syncfusion:PivotComputationInfo 
            FieldHeader="Color" 
            FieldName="Color" 
            Format="0.0" 
            SummaryType="DoubleTotalSum" 
            InnerMostComputationsOnly="InnerMostOnly" />
        <syncfusion:PivotComputationInfo 
            FieldHeader="Class" 
            FieldName="Class" 
            Format="0.0" 
            SummaryType="DoubleTotalSum" 
            InnerMostComputationsOnly="InnerMostOnly" />
        <syncfusion:PivotComputationInfo 
            FieldHeader="Units" 
            FieldName="Units" 
            Format="0.0" 
            SummaryType="DoubleTotalSum" 
            InnerMostComputationsOnly="InnerMostOnly" />
    </syncfusion:PivotGridControl.PivotCalculations>

</syncfusion:PivotGridControl>
```

### Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        pivotGrid.Loaded += pivotGrid_Loaded;
    }

    void pivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Set InnerMostComputationsOnly for all PivotCalculations
        for (int i = 0; i < pivotGrid.PivotCalculations.Count; i++)
        {
            pivotGrid.PivotCalculations[i].InnerMostComputationsOnly = 
                SummaryDisplayLevel.InnerMostOnly;
        }
    }
}
```

### Selective Inner-Most Configuration

```csharp
public void ConfigureInnerMostComputations()
{
    foreach (var calculation in pivotGrid.PivotCalculations)
    {
        // Apply based on calculation name or type
        if (calculation.FieldName == "DetailedMetric")
        {
            calculation.InnerMostComputationsOnly = SummaryDisplayLevel.InnerMostOnly;
        }
        else
        {
            calculation.InnerMostComputationsOnly = SummaryDisplayLevel.All;
        }
    }
}
```

## GetRawItem Method

The `GetRawItemFor` method obtains the list of raw items for value cells, total cells, or grand total cells.

### Basic Implementation

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Enable hyperlinks for value and summary cells
        pivotGrid.ValueCellStyle.IsHyperlinkCell = true;
        pivotGrid.SummaryCellStyle.IsHyperlinkCell = true;
        
        // Handle hyperlink click
        pivotGrid.HyperlinkCellClick += pivotGrid_HyperlinkCellClick;
    }

    void pivotGrid_HyperlinkCellClick(object sender, HyperlinkCellClickEventArgs e)
    {
        // Get raw items for the clicked cell
        var rawItems = pivotGrid.PivotEngine.GetRawItemsFor(
            e.RowColumnIndex.RowIndex, 
            e.RowColumnIndex.ColumnIndex);
        
        // Display raw items
        ShowRawItemsWindow(rawItems);
    }
}
```

### Advanced Raw Item Processing

```csharp
private void pivotGrid_HyperlinkCellClick(object sender, HyperlinkCellClickEventArgs e)
{
    var rowIndex = e.RowColumnIndex.RowIndex;
    var colIndex = e.RowColumnIndex.ColumnIndex;
    
    // Get raw items
    var rawItems = pivotGrid.PivotEngine.GetRawItemsFor(rowIndex, colIndex);
    
    if (rawItems != null && rawItems.Count > 0)
    {
        // Create detailed view
        var detailWindow = new RawItemsDetailWindow();
        detailWindow.Title = $"Details for Cell [{rowIndex}, {colIndex}]";
        
        // Process raw items
        var processedData = ProcessRawItems(rawItems);
        detailWindow.DataGrid.ItemsSource = processedData;
        
        detailWindow.ShowDialog();
    }
    else
    {
        MessageBox.Show("No raw data available for this cell.", 
            "No Data", MessageBoxButton.OK, MessageBoxImage.Information);
    }
}

private List<ProcessedItem> ProcessRawItems(IEnumerable rawItems)
{
    var processed = new List<ProcessedItem>();
    
    foreach (var item in rawItems)
    {
        // Convert to strongly typed object
        var processedItem = ConvertToProcessedItem(item);
        processed.Add(processedItem);
    }
    
    return processed;
}

private ProcessedItem ConvertToProcessedItem(object rawItem)
{
    // Your conversion logic
    return new ProcessedItem();
}

public class ProcessedItem
{
    public string Field1 { get; set; }
    public double Field2 { get; set; }
    public DateTime Field3 { get; set; }
}
```

### Export Raw Items

```csharp
private void ExportRawItems_Click(object sender, RoutedEventArgs e)
{
    var selectedCell = GetSelectedCell();
    
    if (selectedCell != null)
    {
        var rawItems = pivotGrid.PivotEngine.GetRawItemsFor(
            selectedCell.Row, 
            selectedCell.Column);
        
        // Export to CSV
        ExportToCsv(rawItems, "RawItemsExport.csv");
    }
}

private void ExportToCsv(IEnumerable rawItems, string fileName)
{
    using (var writer = new StreamWriter(fileName))
    {
        // Write header
        writer.WriteLine("Field1,Field2,Field3");
        
        // Write data
        foreach (var item in rawItems)
        {
            // Extract fields and write
            writer.WriteLine($"{GetField1(item)},{GetField2(item)},{GetField3(item)}");
        }
    }
}
```

## Performance with Virtualization

Optimize performance for large datasets using the Index Engine with on-demand calculations.

### Key Properties

- **UseIndexedEngine**: Use optimized algorithm with data indexing
- **EnableOnDemandCalculations**: Postpone calculations until requested

### Implementation

```csharp
public partial class MainWindow : Window
{
    DateTime _startIndex = DateTime.Now;
    
    public MainWindow()
    {
        InitializeComponent();
        
        // Enable performance optimization
        pivotGrid.PivotEngine.EnableOnDemandCalculations = true;
        pivotGrid.PivotEngine.UseIndexedEngine = true;
        pivotGrid.PivotEngine.GetValue = ItemObjectLookup;
        
        ObservableCollection<ItemObject> itemsSourceObject = 
            (pivotGrid.DataContext as ViewModel.ViewModel).ItemObjectCollection;
        
        pivotGrid.PivotEngine.PivotSchemaChanged += PivotEngine_PivotSchemaChanged;
        pivotGrid.ItemSource = itemsSourceObject;
    }

    private void PivotEngine_PivotSchemaChanged(object sender, PivotSchemaChangedArgs e)
    {
        _startIndex = DateTime.Now;
        ScrollViewer _scrollViewer = e.OriginalSource as ScrollViewer;
        
        pivotGrid.Dispatcher.BeginInvoke(DispatcherPriority.SystemIdle, new Action(() =>
        {
            if (!pivotGrid.IgnoreRefresh)
            {
                if ((_scrollViewer.Content as TextBlock) != null)
                    (_scrollViewer.Content as TextBlock).Text = string.Empty;

                CheckTime(_startIndex, "Initial part done in");
                ContinueLoadingAsynchronously(
                    pivotGrid.PivotEngine.IndexEngine, _startIndex);
            }
        }));
    }

    private void ContinueLoadingAsynchronously(IndexEngine engine, DateTime startIndex)
    {
        pivotGrid.Dispatcher.BeginInvoke(new Action(() =>
        {
            if (engine != null && engine.HighRowLevel < engine.RowCount - 1)
            {
                int cutOff = Math.Min(engine.HighRowLevel + 800, engine.RowCount - 1);
                
                // Get 800 more rows from pivot engine (on-demand calculation)
                object o = engine[cutOff, 0];
                
                if ((_scrollViewer.Content as TextBlock) != null)
                {
                    (_scrollViewer.Content as TextBlock).Text +=
                        string.Format("\n{0}/{1}", engine.HighRowLevel, engine.RowCount - 1);
                    
                    // Recursive call to update rows until high row level is reached
                    ContinueLoadingAsynchronously(engine, startIndex);
                }
            }
            else
            {
                CheckTime(startIndex, "Load Completed");
            }
        }), DispatcherPriority.SystemIdle);
    }

    private void CheckTime(DateTime start, string label)
    {
        if (_textBlock != null)
        {
            _textBlock.Text += string.Format("\n{0} {1:0.0000} seconds.", 
                label, DateTime.Now.Subtract(start).TotalSeconds);
        }
    }

    public IComparable ItemObjectLookup(object o, string name)
    {
        IComparable c = null;
        var io = o as ItemObject;
        
        if (io != null)
        {
            switch (name)
            {
                case "Date":
                    c = io.Date;
                    break;
                case "Client":
                    c = io.Client;
                    break;
                case "Campaign":
                    c = io.Campaign;
                    break;
                case "Color":
                    c = io.Color;
                    break;
                case "Shape":
                    c = io.Shape;
                    break;
                case "Price":
                    c = io.Price;
                    break;
                case "Spend":
                    c = io.Spend;
                    break;
            }
        }
        
        return c;
    }
}
```

### Optimized Data Loading

```csharp
public class OptimizedDataLoader
{
    private readonly PivotGridControl pivotGrid;
    private readonly int batchSize;
    
    public OptimizedDataLoader(PivotGridControl grid, int batch = 1000)
    {
        pivotGrid = grid;
        batchSize = batch;
    }
    
    public async Task LoadDataAsync(IEnumerable<object> data)
    {
        // Enable optimization
        pivotGrid.PivotEngine.EnableOnDemandCalculations = true;
        pivotGrid.PivotEngine.UseIndexedEngine = true;
        
        // Load data in batches
        var batches = data.Batch(batchSize);
        
        foreach (var batch in batches)
        {
            await Task.Run(() => 
            {
                // Process batch
                ProcessBatch(batch);
            });
            
            // Allow UI to update
            await Task.Delay(10);
        }
    }
    
    private void ProcessBatch(IEnumerable<object> batch)
    {
        // Batch processing logic
    }
}

public static class EnumerableExtensions
{
    public static IEnumerable<IEnumerable<T>> Batch<T>(this IEnumerable<T> source, int size)
    {
        using (var enumerator = source.GetEnumerator())
        {
            while (enumerator.MoveNext())
                yield return YieldBatchElements(enumerator, size - 1);
        }
    }
    
    private static IEnumerable<T> YieldBatchElements<T>(
        IEnumerator<T> source, int size)
    {
        yield return source.Current;
        for (int i = 0; i < size && source.MoveNext(); i++)
            yield return source.Current;
    }
}
```

## Advanced Scenarios

### Custom Drill-Down

```csharp
public class DrillDownManager
{
    private readonly PivotGridControl pivotGrid;
    
    public DrillDownManager(PivotGridControl grid)
    {
        pivotGrid = grid;
        pivotGrid.ValueCellStyle.IsHyperlinkCell = true;
        pivotGrid.HyperlinkCellClick += OnHyperlinkCellClick;
    }
    
    private void OnHyperlinkCellClick(object sender, HyperlinkCellClickEventArgs e)
    {
        var rawItems = pivotGrid.PivotEngine.GetRawItemsFor(
            e.RowColumnIndex.RowIndex, 
            e.RowColumnIndex.ColumnIndex);
        
        ShowDrillDownWindow(rawItems, e.CellInfo);
    }
    
    private void ShowDrillDownWindow(IEnumerable rawItems, PivotCellInfo cellInfo)
    {
        var window = new DrillDownWindow
        {
            Title = $"Details: {cellInfo.FormattedText}",
            DataContext = new DrillDownViewModel(rawItems)
        };
        window.ShowDialog();
    }
}
```

### Dynamic Hyperlink Styling

```csharp
public class DynamicHyperlinkStyler
{
    private readonly PivotGridControl pivotGrid;
    
    public DynamicHyperlinkStyler(PivotGridControl grid)
    {
        pivotGrid = grid;
        pivotGrid.QueryCellStyle += OnQueryCellStyle;
    }
    
    private void OnQueryCellStyle(object sender, PivotGridQueryCellStyleEventArgs e)
    {
        if (e.Cell.CellType == PivotCellType.ValueCell)
        {
            // Apply hyperlink style based on value
            double value;
            if (double.TryParse(e.Cell.FormattedText, out value))
            {
                if (value > 10000)
                {
                    e.Style.Foreground = new SolidColorBrush(Colors.Blue);
                    e.Style.TextDecorations = TextDecorations.Underline;
                }
            }
        }
    }
}
```

### Progressive Loading Indicator

```csharp
public class ProgressiveLoadingIndicator
{
    private readonly PivotGridControl pivotGrid;
    private readonly TextBlock statusTextBlock;
    
    public ProgressiveLoadingIndicator(
        PivotGridControl grid, 
        TextBlock statusBlock)
    {
        pivotGrid = grid;
        statusTextBlock = statusBlock;
        
        pivotGrid.PivotEngine.PivotSchemaChanged += OnSchemaChanged;
    }
    
    private void OnSchemaChanged(object sender, PivotSchemaChangedArgs e)
    {
        var engine = pivotGrid.PivotEngine.IndexEngine;
        
        if (engine != null)
        {
            UpdateProgress(engine.HighRowLevel, engine.RowCount - 1);
        }
    }
    
    private void UpdateProgress(int current, int total)
    {
        double percentage = (double)current / total * 100;
        
        statusTextBlock.Text = $"Loading: {percentage:F1}% ({current}/{total} rows)";
    }
}
```

## Best Practices

### Hyperlink Configuration

1. **Selective Enabling**: Only enable hyperlinks on cells that provide drill-down
2. **Visual Feedback**: Provide clear visual indication of hyperlink cells
3. **Performance**: Limit hyperlink processing for large grids
4. **User Guidance**: Provide tooltips explaining hyperlink functionality

### Inner-Most Computations

1. **Use Case**: Apply when totals/subtotals are not needed
2. **Performance**: Improves rendering speed by reducing calculations
3. **User Experience**: Simplifies view for detail-focused analysis
4. **Documentation**: Clearly indicate when totals are hidden

### Raw Data Access

1. **Drill-Down**: Implement comprehensive drill-down windows
2. **Export Options**: Allow users to export raw data
3. **Filtering**: Provide filtering on raw data views
4. **Performance**: Cache raw data for frequently accessed cells

### Virtualization and Performance

1. **Large Datasets**: Always enable for >10,000 records
2. **Batch Size**: Adjust based on available memory
3. **Progress Indicators**: Show loading progress for large datasets
4. **Lazy Loading**: Load data on-demand as users scroll

### Memory Management

```csharp
public void OptimizeMemory()
{
    // Enable virtualization
    pivotGrid.PivotEngine.EnableOnDemandCalculations = true;
    pivotGrid.PivotEngine.UseIndexedEngine = true;
    
    // Disable features not needed
    pivotGrid.ShowGroupingBar = false;
    pivotGrid.ShowFieldList = false;
    
    // Limit visible area
    pivotGrid.MaxHeight = 600;
    pivotGrid.MaxWidth = 1000;
}
```

## Summary

Advanced features in the WPF PivotGrid provide:

- **Hyperlink Cells**: Interactive cells with drill-down capability
- **Inner-Most Computations**: Display detail data without totals
- **GetRawItem Method**: Access underlying data for any cell
- **Virtualized Binding**: High performance for large datasets
- **Index Engine**: On-demand calculation for optimal loading
- **Advanced Scenarios**: Drill-down, dynamic styling, and progressive loading
- **Best Practices**: Performance optimization and user experience guidelines

Implement these advanced features strategically based on your data volume, user requirements, and performance needs to create a powerful and responsive PivotGrid application.

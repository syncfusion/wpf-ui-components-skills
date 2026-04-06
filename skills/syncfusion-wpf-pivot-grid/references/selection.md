# Cell Selection in WPF PivotGrid

This guide covers cell selection, range selection, row/column selection, and programmatic selection capabilities in the Syncfusion WPF PivotGrid control.

## Selection Overview

The WPF PivotGrid supports selecting entire rows, columns, and groups of cells with or without headers, similar to cell selection in Microsoft Excel.

## Basic Cell Selection

The `AllowSelection` property enables selection behavior in the PivotGrid.

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        AllowSelection="True" 
        ItemSource="{Binding Source={StaticResource data}}">

        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                FieldHeader="Product" 
                FieldMappingName="Product" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="Date" 
                FieldMappingName="Date" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotColumns>
            <syncfusion:PivotItem 
                FieldHeader="Country" 
                FieldMappingName="Country" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="State" 
                FieldMappingName="State" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotColumns>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Amount" 
                Format="C" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Quantity" 
                SummaryType="Count" />
        </syncfusion:PivotGridControl.PivotCalculations>

    </syncfusion:PivotGridControl>
</Grid>
```

### Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        PivotItem m_PivotItem = new PivotItem() 
        { 
            FieldHeader = "Product", 
            FieldMappingName = "Product", 
            TotalHeader = "Total" 
        };
        PivotItem m_PivotItem1 = new PivotItem() 
        { 
            FieldHeader = "Date", 
            FieldMappingName = "Date", 
            TotalHeader = "Total" 
        };
        PivotItem n_PivotItem = new PivotItem() 
        { 
            FieldHeader = "Country", 
            FieldMappingName = "Country", 
            TotalHeader = "Total" 
        };
        PivotItem n_PivotItem1 = new PivotItem() 
        { 
            FieldHeader = "State", 
            FieldMappingName = "State", 
            TotalHeader = "Total" 
        };
        
        // Adding PivotItem to PivotRows
        pivotGrid.PivotRows.Add(m_PivotItem);
        pivotGrid.PivotRows.Add(m_PivotItem1);
        
        // Adding PivotItem to PivotColumns
        pivotGrid.PivotColumns.Add(n_PivotItem);
        pivotGrid.PivotColumns.Add(n_PivotItem1);
        
        PivotComputationInfo m_PivotComputationInfo = new PivotComputationInfo() 
        { 
            CalculationName = "Amount", 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.DoubleTotalSum 
        };
        PivotComputationInfo m_PivotComputationInfo1 = new PivotComputationInfo() 
        { 
            CalculationName = "Quantity", 
            FieldName = "Quantity", 
            SummaryType = SummaryType.Count 
        };
        
        pivotGrid.PivotCalculations.Add(m_PivotComputationInfo);
        pivotGrid.PivotCalculations.Add(m_PivotComputationInfo1);

        // Enable cell selection
        pivotGrid.AllowSelection = true;
    }
}
```

### Selection Behavior

- **Single Cell**: Click a cell to select it
- **Range Selection**: Click and drag to select multiple cells
- **Keyboard Navigation**: Use arrow keys to move selection
- **Extended Selection**: Hold Shift and click to select range

## Row Selection

Select entire rows by clicking row headers.

### Single Row Selection

Click a row header to select all cells in that row.

### Multiple Row Selection

1. Click a row header
2. Drag through other row headers to extend selection
3. Release to complete selection

### Implementation Example

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        
        // Configure PivotGrid...
        
        // Enable selection
        pivotGrid.AllowSelection = true;
        
        // Add selection changed event handler
        pivotGrid.SelectionChanged += PivotGrid_SelectionChanged;
    }
    
    private void PivotGrid_SelectionChanged(object sender, GridRangeInfoEventArgs e)
    {
        // Handle selection change
        var selectedRows = e.Range.Top + " to " + e.Range.Bottom;
        Console.WriteLine($"Selected Rows: {selectedRows}");
    }
}
```

## Column Selection

Select entire columns by clicking column headers.

### Single Column Selection

Click a column header to select all cells in that column.

### Multiple Column Selection

1. Click a column header
2. Drag through other column headers to extend selection
3. Release to complete selection

### Implementation Example

```csharp
private void PivotGrid_SelectionChanged(object sender, GridRangeInfoEventArgs e)
{
    // Get selected column range
    var selectedColumns = e.Range.Left + " to " + e.Range.Right;
    Console.WriteLine($"Selected Columns: {selectedColumns}");
    
    // Access selected cell values
    for (int row = e.Range.Top; row <= e.Range.Bottom; row++)
    {
        for (int col = e.Range.Left; col <= e.Range.Right; col++)
        {
            var cellInfo = pivotGrid.InternalGrid.Model[row, col];
            Console.WriteLine($"Cell [{row},{col}]: {cellInfo.Text}");
        }
    }
}
```

## Selection with Headers

The `AllowSelectionWithHeaders` property enables selection that includes header cells.

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        AllowSelectionWithHeaders="True" 
        ItemSource="{Binding Source={StaticResource data}}">

        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                FieldHeader="Product" 
                FieldMappingName="Product" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="Date" 
                FieldMappingName="Date" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotColumns>
            <syncfusion:PivotItem 
                FieldHeader="Country" 
                FieldMappingName="Country" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="State" 
                FieldMappingName="State" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotColumns>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Amount" 
                Format="C" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Quantity" 
                SummaryType="Count" />
        </syncfusion:PivotGridControl.PivotCalculations>

    </syncfusion:PivotGridControl>
</Grid>
```

### Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure PivotRows, PivotColumns, and PivotCalculations...
        
        // Enable selection with headers
        pivotGrid.AllowSelectionWithHeaders = true;
    }
}
```

### Selection Behavior with Headers

- **Row Selection**: Includes row header cells in selection
- **Column Selection**: Includes column header cells in selection
- **Range Selection**: Headers are part of the selected range

## Programmatic Selection

Control selection through code for automated scenarios.

### Select Specific Cell

```csharp
public void SelectCell(int row, int col)
{
    var gridModel = pivotGrid.InternalGrid.Model;
    pivotGrid.InternalGrid.SelectCells(
        GridRangeInfo.Cell(row, col));
}
```

### Select Cell Range

```csharp
public void SelectRange(int startRow, int startCol, int endRow, int endCol)
{
    pivotGrid.InternalGrid.SelectCells(
        GridRangeInfo.Cells(startRow, startCol, endRow, endCol));
}
```

### Select Entire Row

```csharp
public void SelectRow(int rowIndex)
{
    pivotGrid.InternalGrid.SelectCells(
        GridRangeInfo.Row(rowIndex));
}
```

### Select Entire Column

```csharp
public void SelectColumn(int columnIndex)
{
    pivotGrid.InternalGrid.SelectCells(
        GridRangeInfo.Col(columnIndex));
}
```

### Clear Selection

```csharp
public void ClearSelection()
{
    pivotGrid.InternalGrid.ClearSelection();
}
```

## Selection Events

Handle selection changes to respond to user interactions.

### SelectionChanged Event

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        
        // Configure PivotGrid...
        
        pivotGrid.AllowSelection = true;
        pivotGrid.SelectionChanged += PivotGrid_SelectionChanged;
    }
    
    private void PivotGrid_SelectionChanged(object sender, GridRangeInfoEventArgs e)
    {
        Console.WriteLine($"Selection Changed:");
        Console.WriteLine($"  Top: {e.Range.Top}");
        Console.WriteLine($"  Left: {e.Range.Left}");
        Console.WriteLine($"  Bottom: {e.Range.Bottom}");
        Console.WriteLine($"  Right: {e.Range.Right}");
        Console.WriteLine($"  Cell Count: {e.Range.Height * e.Range.Width}");
    }
}
```

### SelectionChanging Event

```csharp
pivotGrid.SelectionChanging += PivotGrid_SelectionChanging;

private void PivotGrid_SelectionChanging(object sender, GridRangeInfoCancelEventArgs e)
{
    // Cancel selection based on condition
    if (e.Range.Height > 100 || e.Range.Width > 100)
    {
        e.Cancel = true;
        MessageBox.Show("Selection too large");
    }
}
```

## Advanced Selection Patterns

### Multi-Range Selection

```csharp
public void SelectMultipleRanges()
{
    var selections = new List<GridRangeInfo>
    {
        GridRangeInfo.Cells(2, 2, 5, 5),
        GridRangeInfo.Cells(10, 10, 15, 15),
        GridRangeInfo.Cells(20, 2, 25, 5)
    };
    
    foreach (var range in selections)
    {
        pivotGrid.InternalGrid.SelectCells(range, true); // true = add to selection
    }
}
```

### Conditional Selection

```csharp
public void SelectCellsWithCondition(Func<string, bool> condition)
{
    var gridModel = pivotGrid.InternalGrid.Model;
    
    for (int row = 1; row <= gridModel.RowCount; row++)
    {
        for (int col = 1; col <= gridModel.ColumnCount; col++)
        {
            var cellValue = gridModel[row, col].Text;
            if (condition(cellValue))
            {
                pivotGrid.InternalGrid.SelectCells(
                    GridRangeInfo.Cell(row, col), true);
            }
        }
    }
}

// Usage:
SelectCellsWithCondition(value => 
{
    double num;
    return double.TryParse(value, out num) && num > 1000;
});
```

### Get Selected Cell Values

```csharp
public List<string> GetSelectedValues()
{
    var values = new List<string>();
    var selection = pivotGrid.InternalGrid.SelectedRanges;
    
    foreach (var range in selection)
    {
        for (int row = range.Top; row <= range.Bottom; row++)
        {
            for (int col = range.Left; col <= range.Right; col++)
            {
                var cellInfo = pivotGrid.InternalGrid.Model[row, col];
                values.Add(cellInfo.Text);
            }
        }
    }
    
    return values;
}
```

### Export Selected Data

```csharp
public void ExportSelectedToClipboard()
{
    var selectedValues = GetSelectedValues();
    var data = string.Join(Environment.NewLine, selectedValues);
    Clipboard.SetText(data);
}
```

## Selection Customization

### Custom Selection Styling

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Configure PivotGrid...
        
        pivotGrid.Loaded += PivotGrid_Loaded;
    }
    
    private void PivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Customize selection appearance
        var grid = pivotGrid.InternalGrid;
        grid.Properties.AlphaBlendSelectionColor = Color.FromArgb(100, 135, 206, 250);
        grid.Properties.ShowCurrentCellBorderBehavior = 
            GridShowCurrentCellBorder.GrayWhenLostFocus;
    }
}
```

### Selection Validation

```csharp
public class SelectionValidator
{
    private PivotGridControl pivotGrid;
    
    public SelectionValidator(PivotGridControl grid)
    {
        pivotGrid = grid;
        pivotGrid.SelectionChanging += OnSelectionChanging;
    }
    
    private void OnSelectionChanging(object sender, GridRangeInfoCancelEventArgs e)
    {
        // Validate selection
        if (!IsValidSelection(e.Range))
        {
            e.Cancel = true;
            MessageBox.Show("Invalid selection");
        }
    }
    
    private bool IsValidSelection(GridRangeInfo range)
    {
        // Only allow selection of value cells, not headers
        return range.Top > pivotGrid.PivotEngine.RowCount &&
               range.Left > pivotGrid.PivotEngine.ColumnCount;
    }
}
```

## Best Practices

### Enable Selection Appropriately

```csharp
// For interactive reports
pivotGrid.AllowSelection = true;

// For read-only displays
pivotGrid.AllowSelection = false;
```

### Handle Large Selections

```csharp
private void PivotGrid_SelectionChanging(object sender, GridRangeInfoCancelEventArgs e)
{
    // Limit selection size for performance
    const int MaxCells = 10000;
    int cellCount = e.Range.Height * e.Range.Width;
    
    if (cellCount > MaxCells)
    {
        e.Cancel = true;
        MessageBox.Show($"Selection limited to {MaxCells} cells");
    }
}
```

### Provide User Feedback

```csharp
private void PivotGrid_SelectionChanged(object sender, GridRangeInfoEventArgs e)
{
    int cellCount = e.Range.Height * e.Range.Width;
    statusLabel.Text = $"{cellCount} cells selected";
    
    // Calculate sum of selected numeric values
    double sum = CalculateSelectionSum(e.Range);
    sumLabel.Text = $"Sum: {sum:C}";
}
```

### Performance Optimization

```csharp
public void OptimizeSelectionPerformance()
{
    // Disable visual updates during batch selection
    pivotGrid.InternalGrid.BeginUpdate();
    
    try
    {
        // Perform multiple selections
        SelectMultipleRanges();
    }
    finally
    {
        pivotGrid.InternalGrid.EndUpdate();
    }
}
```

### Keyboard Shortcuts

```csharp
public void SetupKeyboardShortcuts()
{
    pivotGrid.KeyDown += (s, e) =>
    {
        if (e.Key == Key.A && Keyboard.Modifiers == ModifierKeys.Control)
        {
            // Ctrl+A: Select all
            SelectAll();
            e.Handled = true;
        }
        else if (e.Key == Key.Escape)
        {
            // Esc: Clear selection
            ClearSelection();
            e.Handled = true;
        }
    };
}

private void SelectAll()
{
    var model = pivotGrid.InternalGrid.Model;
    pivotGrid.InternalGrid.SelectCells(
        GridRangeInfo.Cells(1, 1, model.RowCount, model.ColumnCount));
}
```

## Summary

Cell selection in the WPF PivotGrid provides:

- **Basic Cell Selection** through `AllowSelection` property
- **Row Selection** by clicking row headers
- **Column Selection** by clicking column headers  
- **Selection with Headers** via `AllowSelectionWithHeaders` property
- **Programmatic Selection** for automated control
- **Selection Events** for responding to user interactions
- **Advanced Patterns** including multi-range and conditional selection
- **Customization Options** for styling and validation
- **Best Practices** for performance, usability, and user experience

Implement selection features based on your application requirements, user needs, and performance considerations to create an intuitive and responsive PivotGrid interface.

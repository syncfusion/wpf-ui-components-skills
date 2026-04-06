# Editing and Updating in WPF PivotGrid

This guide covers cell editing, value updating, data modification, and real-time refresh capabilities in the Syncfusion WPF PivotGrid control.

## Editing Overview

The WPF PivotGrid supports editing value and total cells, allowing users to modify data directly in the grid. When editing is enabled, values are calculated automatically, and total values adjust accordingly.

## Enable Value Cell Editing

The `EnableValueEditing` property enables editing for value cells in the PivotGrid.

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        EnableValueEditing="True" 
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

        // Enable value cell editing
        pivotGrid.EnableValueEditing = true;
    }
}
```

## Enable Total Cell Editing

The `AllowEditingOfTotalCells` property of `PivotEditingManager` enables editing for total cells.

### Implementation

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

        pivotGrid.Loaded += pivotGrid_Loaded;
    }
    
    void pivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Enable editing for total cells
        pivotGrid.EditManager.AllowEditingOfTotalCells = true;
    }
}
```

## Custom Editing Manager

Create a custom editing manager to further customize value formatting and cell behavior after editing.

### Creating Custom EditManager

```csharp
public class CustomEditManager : PivotEditingManager
{
    public CustomEditManager(PivotGridControl pg) : base(pg) {}
    
    protected override void ChangeValue(object oldValue, object newValue, 
                                       int row1, int col1, PivotCellInfo pi)
    {
        // Execute base change operation
        base.ChangeValue(oldValue, newValue, row1, col1, pi);
        
        // Mark all adjusted cell contents
        pi.FormattedText += "*";
    }
}
```

### Implementing Custom EditManager

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

        pivotGrid.Loaded += pivotGrid_Loaded;
    }

    void pivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Dispose existing edit manager and set custom manager
        pivotGrid.EditManager.Dispose();
        pivotGrid.EditManager = new CustomEditManager(pivotGrid);
    }
}
```

## Real-Time Updating

The PivotGrid supports real-time value updates through the `EnableUpdating` property.

### Key Properties

- **EnableUpdating**: Enables/disables real-time updating
- **ThrottleUpdateRate**: Time interval (in milliseconds) between UI refreshes

### XAML Configuration

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        EnableUpdating="True" 
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

        // Enable real-time updating
        pivotGrid.EnableUpdating = true;
    }
}
```

### Throttling Update Rate

Control the UI refresh rate to optimize CPU usage:

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
        
        pivotGrid.EnableUpdating = true;
        pivotGrid.Loaded += pivotGrid_Loaded;
    }

    void pivotGrid_Loaded(object sender, RoutedEventArgs e)
    {
        // Set throttle rate to 300 milliseconds
        pivotGrid.UpdateManager.ThrottleUpdateRate = 300;
    }
}
```

**Throttle Rate Guidelines**:
- **0 ms**: Immediate refresh (default) - highest CPU usage
- **300-500 ms**: Recommended for high-frequency updates
- **Higher values**: For slower update rates, lower CPU usage

## Data Modification Patterns

### Validating Edits

```csharp
public class ValidatingEditManager : PivotEditingManager
{
    public ValidatingEditManager(PivotGridControl pg) : base(pg) {}
    
    protected override void ChangeValue(object oldValue, object newValue, 
                                       int row1, int col1, PivotCellInfo pi)
    {
        // Validate new value
        if (newValue != null)
        {
            double value;
            if (double.TryParse(newValue.ToString(), out value))
            {
                if (value >= 0)
                {
                    // Only allow non-negative values
                    base.ChangeValue(oldValue, newValue, row1, col1, pi);
                }
                else
                {
                    MessageBox.Show("Value must be non-negative");
                }
            }
            else
            {
                MessageBox.Show("Invalid numeric value");
            }
        }
    }
}
```

### Tracking Changes

```csharp
public class ChangeTrackingEditManager : PivotEditingManager
{
    private List<EditChange> changes = new List<EditChange>();
    
    public ChangeTrackingEditManager(PivotGridControl pg) : base(pg) {}
    
    protected override void ChangeValue(object oldValue, object newValue, 
                                       int row1, int col1, PivotCellInfo pi)
    {
        // Track the change
        changes.Add(new EditChange
        {
            Row = row1,
            Column = col1,
            OldValue = oldValue,
            NewValue = newValue,
            Timestamp = DateTime.Now
        });
        
        base.ChangeValue(oldValue, newValue, row1, col1, pi);
    }
    
    public List<EditChange> GetChanges()
    {
        return new List<EditChange>(changes);
    }
}

public class EditChange
{
    public int Row { get; set; }
    public int Column { get; set; }
    public object OldValue { get; set; }
    public object NewValue { get; set; }
    public DateTime Timestamp { get; set; }
}
```

### Undo/Redo Support

```csharp
public class UndoableEditManager : PivotEditingManager
{
    private Stack<EditAction> undoStack = new Stack<EditAction>();
    private Stack<EditAction> redoStack = new Stack<EditAction>();
    
    public UndoableEditManager(PivotGridControl pg) : base(pg) {}
    
    protected override void ChangeValue(object oldValue, object newValue, 
                                       int row1, int col1, PivotCellInfo pi)
    {
        // Save action for undo
        undoStack.Push(new EditAction
        {
            Row = row1,
            Column = col1,
            OldValue = oldValue,
            NewValue = newValue,
            CellInfo = pi
        });
        
        redoStack.Clear(); // Clear redo stack on new edit
        
        base.ChangeValue(oldValue, newValue, row1, col1, pi);
    }
    
    public void Undo()
    {
        if (undoStack.Count > 0)
        {
            var action = undoStack.Pop();
            redoStack.Push(action);
            
            // Revert the change
            base.ChangeValue(action.NewValue, action.OldValue, 
                           action.Row, action.Column, action.CellInfo);
        }
    }
    
    public void Redo()
    {
        if (redoStack.Count > 0)
        {
            var action = redoStack.Pop();
            undoStack.Push(action);
            
            // Reapply the change
            base.ChangeValue(action.OldValue, action.NewValue, 
                           action.Row, action.Column, action.CellInfo);
        }
    }
}

public class EditAction
{
    public int Row { get; set; }
    public int Column { get; set; }
    public object OldValue { get; set; }
    public object NewValue { get; set; }
    public PivotCellInfo CellInfo { get; set; }
}
```

## Best Practices

### Performance Optimization

1. **Use Throttling**: Set `ThrottleUpdateRate` for high-frequency updates
2. **Batch Updates**: Group multiple changes when possible
3. **Disable Updates During Bulk Operations**: Toggle `EnableUpdating` off during large data changes

### User Experience

1. **Visual Feedback**: Mark edited cells with indicators (e.g., asterisk)
2. **Validation**: Implement input validation in custom edit managers
3. **Confirmation**: Prompt users before applying changes to total cells
4. **Undo Support**: Provide undo/redo functionality for data changes

### Data Integrity

1. **Validate Inputs**: Check data types and value ranges
2. **Track Changes**: Maintain change history for auditing
3. **Preserve Original Data**: Keep backups of unmodified data
4. **Transaction Support**: Consider implementing transaction-like behavior

### Error Handling

```csharp
protected override void ChangeValue(object oldValue, object newValue, 
                                   int row1, int col1, PivotCellInfo pi)
{
    try
    {
        // Validation logic
        if (IsValidValue(newValue))
        {
            base.ChangeValue(oldValue, newValue, row1, col1, pi);
        }
        else
        {
            throw new ArgumentException("Invalid value");
        }
    }
    catch (Exception ex)
    {
        // Log error
        LogError(ex);
        
        // Notify user
        MessageBox.Show($"Edit failed: {ex.Message}");
        
        // Optionally revert
        RevertChange(row1, col1, oldValue);
    }
}
```

## Summary

Editing and updating in the WPF PivotGrid provides:

- **Value Cell Editing** through `EnableValueEditing` property
- **Total Cell Editing** via `AllowEditingOfTotalCells` property
- **Custom Edit Managers** for specialized editing behavior
- **Real-Time Updates** with `EnableUpdating` property
- **Throttled Refresh** using `ThrottleUpdateRate` for performance
- **Data Modification Patterns** for validation, tracking, and undo/redo
- **Best Practices** for performance, user experience, and data integrity

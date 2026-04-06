# Drill-Through in WPF OLAP Client

## Overview

Drill-through retrieves the raw data that was used to create a specified cell in a cube. This powerful feature allows users to explore the detailed transactional records underlying aggregated OLAP data, providing deep insights into data composition.

## Enabling Drill-Through

Drill-through is controlled by the `EnableDrillThrough` property on the OLAP Grid. When enabled, value cells become hyperlinks that users can click to view underlying detail data.

### Configuration

```csharp
// Set display mode to Grid only (drill-through works on grid)
this.olapClient1.DisplayMode = Syncfusion.Windows.Client.Olap.DisplayModes.GridOnly;

// Make value cells appear as hyperlinks
this.olapClient1.OlapGrid.ValueCellStyle.IsHyperlinkCell = true;

// Enable drill-through functionality
this.olapClient1.OlapGrid.EnableDrillThrough = true;

// Handle LinkClick event to process drill-through data
this.olapClient1.OlapGrid.LinkClick += new Syncfusion.Windows.Grid.Olap.LinkLabelClickEventHander(OlapGrid_LinkClick);
```

### LinkClick Event Handler

The `LinkClick` event fires when a user clicks a hyperlinked cell. The event provides access to the underlying detail data:

```csharp
void OlapGrid_LinkClick(object sender, Syncfusion.Windows.Grid.Olap.LinkLabelEventArgs e)
{
    // Retrieve the drill-through data as a DataTable
    DataTable drillThroughData = e.DrillThroughData;
    
    // Process or display the data
    // Examples: Show in dialog, export to Excel, bind to another grid
    DisplayDrillThroughDialog(drillThroughData);
}
```

**VB.NET:**

```vbnet
' Set display mode to Grid only
Me.olapClient1.DisplayMode = Syncfusion.Windows.Client.Olap.DisplayModes.GridOnly

' Make value cells appear as hyperlinks
Me.olapClient1.OlapGrid.ValueCellStyle.IsHyperlinkCell = True

' Enable drill-through functionality
Me.olapClient1.OlapGrid.EnableDrillThrough = True

' Handle LinkClick event
AddHandler Me.olapClient1.OlapGrid.LinkClick, AddressOf OlapGrid_LinkClick

Private Sub OlapGrid_LinkClick(sender As Object, e As Syncfusion.Windows.Grid.Olap.LinkLabelEventArgs)
    ' Retrieve the drill-through data as a DataTable
    Dim drillThroughData As DataTable = e.DrillThroughData
    
    ' Process or display the data
    DisplayDrillThroughDialog(drillThroughData)
End Sub
```

## Drill-Through Workflow

### Step 1: Hyperlink Cell Click

When drill-through is enabled, value cells in the OLAP Grid display as hyperlinks. User clicks on a hyperlinked value cell to initiate drill-through.

### Step 2: Attribute Hierarchy Selector

An Attribute Hierarchy Selector dialog appears, allowing users to choose which attributes to include in the detail view.

**Features:**
- **Select Attributes**: Check which dimension attributes to display
- **Customize View**: Include only relevant attributes
- **Apply Selection**: Click OK to proceed

### Step 3: Drill-Through Data Grid

The detailed data is displayed in a grid showing the raw transactions that contributed to the aggregated cell value.

**Information Displayed:**
- Individual transaction records
- Selected dimension attributes
- Measure values for each record
- Complete detail context

## Use Cases

### Transaction-Level Analysis

**Scenario:** Executive sees high sales figure for a region/month and wants to see actual orders

**Workflow:**
1. View aggregated sales in OLAP Grid
2. Click on high-value cell
3. Select attributes (Customer, Product, Date)
4. View individual transactions making up the total

### Data Quality Investigation

**Scenario:** Analyst notices unusual aggregated value and needs to verify underlying data

**Workflow:**
1. Identify suspicious cell in OLAP Grid
2. Drill through to see detail records
3. Examine individual transactions
4. Identify data quality issues or anomalies

### Audit and Compliance

**Scenario:** Compliance officer needs to trace aggregated figures back to source transactions

**Workflow:**
1. Navigate to specific period/category in OLAP Grid
2. Drill through to retrieve transaction list
3. Export detail data for audit trail
4. Verify against source systems

### Customer Behavior Analysis

**Scenario:** Marketing team wants to understand which customers contributed to regional sales

**Workflow:**
1. View sales by region in OLAP Grid
2. Click on target region cell
3. Select Customer attribute in hierarchy selector
4. Analyze customer-level purchase details

## Display Modes

Drill-through is specifically designed for grid-based analysis. Set the appropriate display mode:

### Grid Only Mode

```csharp
// Recommended for drill-through
this.olapClient1.DisplayMode = Syncfusion.Windows.Client.Olap.DisplayModes.GridOnly;
```

### Other Display Modes

- **ChartOnly**: Chart-only view (drill-through not applicable)
- **GridAndChart**: Both grid and chart visible
- **GridThenChart**: Grid above, chart below

**Note:** Drill-through clicks occur on the grid, regardless of display mode.

## Processing Drill-Through Data

The `DrillThroughData` property returns a `DataTable` with the detail records. You can process this data in various ways:

### Display in Dialog Window

```csharp
void OlapGrid_LinkClick(object sender, Syncfusion.Windows.Grid.Olap.LinkLabelEventArgs e)
{
    DataTable drillThroughData = e.DrillThroughData;
    
    // Create a window to display the data
    Window detailWindow = new Window
    {
        Title = "Drill-Through Details",
        Width = 800,
        Height = 600
    };
    
    DataGrid dataGrid = new DataGrid
    {
        ItemsSource = drillThroughData.DefaultView,
        IsReadOnly = true,
        AutoGenerateColumns = true
    };
    
    detailWindow.Content = dataGrid;
    detailWindow.ShowDialog();
}
```

### Export to Excel

```csharp
void OlapGrid_LinkClick(object sender, Syncfusion.Windows.Grid.Olap.LinkLabelEventArgs e)
{
    DataTable drillThroughData = e.DrillThroughData;
    
    SaveFileDialog saveDialog = new SaveFileDialog
    {
        Filter = "Excel Files (*.xlsx)|*.xlsx",
        FileName = "DrillThroughData.xlsx"
    };
    
    if (saveDialog.ShowDialog() == true)
    {
        // Export DataTable to Excel using Syncfusion.XlsIO
        ExportToExcel(drillThroughData, saveDialog.FileName);
    }
}
```

### Filter and Analyze

```csharp
void OlapGrid_LinkClick(object sender, Syncfusion.Windows.Grid.Olap.LinkLabelEventArgs e)
{
    DataTable drillThroughData = e.DrillThroughData;
    
    // Filter data programmatically
    DataView filteredView = drillThroughData.DefaultView;
    filteredView.RowFilter = "SalesAmount > 1000";
    
    // Sort data
    filteredView.Sort = "SalesAmount DESC";
    
    // Display filtered results
    DisplayData(filteredView);
}
```

### Export to CSV

```csharp
void OlapGrid_LinkClick(object sender, Syncfusion.Windows.Grid.Olap.LinkLabelEventArgs e)
{
    DataTable drillThroughData = e.DrillThroughData;
    
    SaveFileDialog saveDialog = new SaveFileDialog
    {
        Filter = "CSV Files (*.csv)|*.csv",
        FileName = "DrillThroughData.csv"
    };
    
    if (saveDialog.ShowDialog() == true)
    {
        ExportToCsv(drillThroughData, saveDialog.FileName);
    }
}

private void ExportToCsv(DataTable dataTable, string filePath)
{
    StringBuilder csv = new StringBuilder();
    
    // Write headers
    csv.AppendLine(string.Join(",", dataTable.Columns.Cast<DataColumn>().Select(c => c.ColumnName)));
    
    // Write rows
    foreach (DataRow row in dataTable.Rows)
    {
        csv.AppendLine(string.Join(",", row.ItemArray));
    }
    
    File.WriteAllText(filePath, csv.ToString());
}
```

## Best Practices

### Enable Visual Indicators

Make it obvious that cells are clickable:

```csharp
// Hyperlink styling makes cells clearly clickable
this.olapClient1.OlapGrid.ValueCellStyle.IsHyperlinkCell = true;

// Optionally customize hyperlink appearance
this.olapClient1.OlapGrid.ValueCellStyle.HyperlinkCellStyle.Foreground = Brushes.Blue;
this.olapClient1.OlapGrid.ValueCellStyle.HyperlinkCellStyle.TextDecorations = TextDecorations.Underline;
```

### Provide User Guidance

Inform users about drill-through capability:
- Add tooltip explaining click functionality
- Provide help text or instructions
- Show example in user training

### Handle Large Detail Sets

Drill-through can return many records. Handle large datasets appropriately:

```csharp
void OlapGrid_LinkClick(object sender, Syncfusion.Windows.Grid.Olap.LinkLabelEventArgs e)
{
    DataTable drillThroughData = e.DrillThroughData;
    
    if (drillThroughData.Rows.Count > 10000)
    {
        MessageBoxResult result = MessageBox.Show(
            $"This cell contains {drillThroughData.Rows.Count} detail records. " +
            "Would you like to export to Excel instead of viewing?",
            "Large Dataset",
            MessageBoxButton.YesNo
        );
        
        if (result == MessageBoxResult.Yes)
        {
            ExportToExcel(drillThroughData);
        }
    }
    else
    {
        DisplayInDialog(drillThroughData);
    }
}
```

### Select Relevant Attributes

In the Attribute Hierarchy Selector:
- Include only attributes relevant to analysis
- Too many attributes = cluttered display
- Too few attributes = insufficient context
- Balance detail with readability

### Combine with Filtering

Filter aggregated data before drilling through:
1. Apply filters to focus on specific segments
2. Drill through on filtered cells
3. Underlying data is contextually relevant

## Performance Considerations

### Large Cubes

Drill-through queries large cubes for detail data. Consider:
- **Cube design**: Ensure appropriate indexing
- **Network**: Online cubes may have latency
- **Local caching**: Offline cubes perform faster

### Query Optimization

- Drill-through executes MDX queries against the cube
- Query performance depends on cube design and SSAS configuration
- Test drill-through with realistic data volumes

### UI Responsiveness

For large detail sets:
- Show progress indicator during drill-through
- Load data asynchronously if possible
- Provide cancel option for long-running queries

## Troubleshooting

### Hyperlinks Not Appearing

**Possible Causes:**
- `IsHyperlinkCell` property not set to `true`
- `EnableDrillThrough` property not set to `true`

**Solutions:**
```csharp
this.olapClient1.OlapGrid.ValueCellStyle.IsHyperlinkCell = true;
this.olapClient1.OlapGrid.EnableDrillThrough = true;
```

### LinkClick Event Not Firing

**Possible Causes:**
- Event handler not registered
- DisplayMode incompatible

**Solutions:**
- Verify event handler is connected
- Ensure DisplayMode includes Grid

### No Data Returned

**Possible Causes:**
- Cell has no underlying detail data
- Cube doesn't support drill-through
- Insufficient permissions

**Solutions:**
- Verify cube configuration supports drill-through
- Check user permissions on cube
- Test with different cells

### Performance Issues

**Possible Causes:**
- Very large detail datasets
- Slow cube queries
- Network latency

**Solutions:**
- Optimize cube design
- Use offline cube for testing
- Implement paging or export for large results

## Next Steps

After implementing drill-through:

1. Explore exporting detail data - see [exporting.md](exporting.md)
2. Learn about filtering for focused analysis - see [filtering.md](filtering.md)
3. Understand paging for large datasets - see [paging.md](paging.md)

# Advanced Features

## Table of Contents
- [Paging Configuration](#paging-configuration)
- [Hyperlink Cells](#hyperlink-cells)
- [Freeze Headers](#freeze-headers)
- [Tooltip Customization](#tooltip-customization)
- [Grid Style Dialog](#grid-style-dialog)
- [Additional Features](#additional-features)

## Paging Configuration

Paging helps manage large OLAP datasets by loading data in chunks, improving performance and user experience.

### Enabling Paging

```csharp
// Enable paging through OlapDataManager
olapDataManager.CurrentReport.PagerOptions.PagingEnabled = true;
olapDataManager.CurrentReport.PagerOptions.CategoricalPageSize = 10;
olapDataManager.CurrentReport.PagerOptions.SeriesPageSize = 10;
```

### Configuration Properties

- **PagingEnabled** - Turns paging on/off
- **CategoricalPageSize** - Number of column groups per page
- **SeriesPageSize** - Number of row groups per page

### When to Use Paging

- Cube returns thousands of members
- Initial load time is slow
- Users typically analyze subsets of data
- Memory constraints exist

## Hyperlink Cells

Add clickable hyperlinks to cells for navigation to related reports or external resources.

### Basic Implementation

```csharp
// Configure hyperlink cells
olapGrid1.CellClick += OlapGrid1_CellClick;

private void OlapGrid1_CellClick(object sender, CellClickEventArgs e)
{
    if (e.Cell.IsDataCell)
    {
        // Navigate to detailed report or URL
        string url = GetDrillThroughUrl(e.Cell);
        System.Diagnostics.Process.Start(url);
    }
}
```

### Use Cases

- Drill through to transaction details
- Link to external dashboards
- Navigate to related reports
- Open documentation or help

## Freeze Headers

Freeze row or column headers to keep them visible while scrolling through large datasets.

### Implementation

```csharp
// Freeze columns (headers)
olapGrid1.InternalGrid.FrozenColumns = numberOfColumnsToFreeze;

// Freeze rows (headers)
olapGrid1.InternalGrid.FrozenRows = numberOfRowsToFreeze;
```

### Benefits

- Headers remain visible during scroll
- Easier navigation in large grids
- Better context for data cells
- Improved user experience

## Tooltip Customization

Customize tooltips to show additional information when hovering over cells.

### Basic Tooltip

```csharp
// Enable tooltips
olapGrid1.ShowToolTip = true;
```

### Custom Tooltip Content

```csharp
olapGrid1.QueryCellInfo += OlapGrid1_QueryCellInfo;

private void OlapGrid1_QueryCellInfo(object sender, QueryCellInfoEventArgs e)
{
    if (e.Cell.IsDataCell)
    {
        // Set custom tooltip
        e.Style.CellTipText = $"Value: {e.Cell.Value}\nDetails: {GetCellDetails(e.Cell)}";
    }
}
```

### Tooltip Best Practices

- Keep tooltip text concise
- Show additional context not visible in cell
- Include formatting hints or explanations
- Don't duplicate visible information

## Grid Style Dialog

The Grid Style Dialog provides a UI for users to customize grid appearance at runtime.

### Enabling Style Dialog

```csharp
// Provide menu option to open style dialog
private void OpenStyleDialog_Click(object sender, RoutedEventArgs e)
{
    // Open grid style customization dialog
    // Implementation depends on Syncfusion API version
}
```

### Customizable Elements

- Cell backgrounds and foregrounds
- Font styles and sizes
- Border styles
- Header styles
- Summary cell appearance

## Additional Features

### RTL (Right-to-Left) Support

For languages like Arabic and Hebrew:

```csharp
olapGrid1.FlowDirection = FlowDirection.RightToLeft;
```

### Accessibility

- Support keyboard navigation
- Provide screen reader compatibility
- Ensure sufficient color contrast
- Include ARIA attributes where applicable

### Performance Tips

- Use paging for large datasets
- Limit initial drill depth
- Consider offline cubes for better performance
- Implement lazy loading where possible

### Localization

- Support multiple languages for UI elements
- Format numbers and dates based on culture
- Translate dimension and measure names if needed

```csharp
// Set culture
System.Threading.Thread.CurrentThread.CurrentCulture = 
    new System.Globalization.CultureInfo("de-DE");
```

## Best Practices

**Paging:**
- Set appropriate page sizes (10-20 items)
- Provide clear navigation controls
- Show total page count
- Remember user's page position

**Hyperlinks:**
- Make links visually distinct
- Provide hover indication
- Handle navigation errors gracefully
- Consider security for external links

**Freeze Headers:**
- Freeze enough headers for context
- Don't freeze too many (reduces visible data area)
- Test with different screen sizes

**Tooltips:**
- Show on hover with slight delay
- Auto-hide when mouse moves away
- Don't block important UI elements
- Test tooltip performance with large grids

## Troubleshooting

**Paging not working:**
- Verify PagingEnabled is true
- Check page sizes are set appropriately
- Ensure OlapReport is configured correctly

**Frozen headers not staying fixed:**
- Check FrozenColumns/FrozenRows values
- Verify grid has scrollbars
- Test with sufficient data to require scrolling

**Tooltips not appearing:**
- Confirm ShowToolTip is enabled
- Check QueryCellInfo event is wired
- Verify tooltip text is not empty

**Performance issues:**
- Enable paging for large datasets
- Reduce initial drill depth
- Consider offline cube
- Profile and optimize queries

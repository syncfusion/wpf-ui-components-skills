# Grid Layouts

## Table of Contents
- [Grid Layout Overview](#grid-layout-overview)
- [Layout Types](#layout-types)
- [Normal Layout](#normal-layout)
- [Excel-Like Layout](#excel-like-layout)
- [Excel-Like Layout with Member Properties](#excel-like-layout-with-member-properties)
- [Normal Top Summary Layout](#normal-top-summary-layout)
- [No Summaries Layout](#no-summaries-layout)
- [Layout Configuration](#layout-configuration)
- [Layout Selection Guidelines](#layout-selection-guidelines)

## Grid Layout Overview

The OLAP Grid supports five different layout types that control how summary cells and child members are positioned. Choosing the right layout improves readability and matches user expectations based on their familiarity with tools like Excel or traditional OLAP clients.

**Layout determines:**
- Position of summary/total cells relative to parent members
- Indentation and visual hierarchy of child members
- Display of member properties alongside dimensions
- Overall grid appearance and navigation pattern

**Available Layouts:**
1. **Normal** - Summaries at bottom, children adjacent (default)
2. **Excel-Like** - Summaries at bottom, children indented below parent
3. **Excel-Like with Member Properties** - Excel-like + member properties displayed
4. **Normal Top Summary** - Summaries at top, children adjacent
5. **No Summaries** - Hides all summary cells

## Layout Types

### Comparison Table

| Layout | Summary Position | Child Position | Member Properties | Best For |
|--------|------------------|----------------|-------------------|----------|
| Normal | Bottom | Adjacent | No | Traditional OLAP users |
| Excel-Like | Bottom | Indented below | No | Excel-familiar users |
| Excel-Like with Properties | Bottom | Indented below | Yes | Detailed analysis with properties |
| Normal Top Summary | Top | Adjacent | No | Summary-first analysis |
| No Summaries | Hidden | Adjacent | No | Detailed data without totals |

## Normal Layout

The default layout where summary cells appear at the bottom of each parent member, and child members are positioned adjacent to the parent.

### Characteristics

- **Summary Position:** Bottom of parent member group
- **Child Position:** Adjacent to parent (same level visually)
- **Visual Style:** Traditional OLAP/pivot grid appearance
- **Hierarchy Indication:** Subtle, based on grouping

### Configuration

**C# Code:**
```csharp
olapGrid1.Layout = GridLayout.Normal;
```

**VB.NET Code:**
```vbnet
Me.OlapGrid1.Layout = GridLayout.Normal
```

**XAML:**
```xaml
<syncfusion:OlapGrid Name="olapGrid1" Layout="Normal" />
```

### When to Use

Use Normal layout when:
- Users are familiar with traditional OLAP tools
- You want a compact view with minimal vertical space
- Summary totals should appear after detail rows
- The application follows classic BI tool patterns

### Visual Structure Example

```
Customer Geography
├─ Australia          [values]
├─ Canada             [values]
├─ France             [values]
└─ Total              [summary values]  ← Summary at bottom
```

## Excel-Like Layout

Summary cells positioned at the bottom alone, with child members displayed below the parent with indentation, mimicking Microsoft Excel pivot table style.

### Characteristics

- **Summary Position:** Bottom (aggregate level)
- **Child Position:** Indented below parent member
- **Visual Style:** Excel pivot table appearance
- **Hierarchy Indication:** Clear visual hierarchy with indentation

### Configuration

**C# Code:**
```csharp
olapGrid1.Layout = GridLayout.ExcelLikeLayout;
```

**VB.NET Code:**
```vbnet
Me.OlapGrid1.Layout = GridLayout.ExcelLikeLayout
```

**XAML:**
```xaml
<syncfusion:OlapGrid Name="olapGrid1" Layout="ExcelLikeLayout" />
```

### When to Use

Use Excel-Like layout when:
- Users are most familiar with Excel pivot tables
- Clear visual hierarchy is important
- You want intuitive drill-down navigation
- The application targets business analysts who use Excel frequently

### Visual Structure Example

```
Customer Geography
  Australia           [values]
    New South Wales   [values]
    Queensland        [values]
  Canada              [values]
    British Columbia  [values]
    Ontario           [values]
  Total               [summary values]  ← Summary at bottom
```

Notice the indentation showing parent-child relationships clearly.

## Excel-Like Layout with Member Properties

Extends the Excel-Like layout by displaying member properties adjacent to each dimension member. Properties must be defined in the OLAP cube and bound through the OlapReport.

### Characteristics

- **Summary Position:** Bottom (aggregate level)
- **Child Position:** Indented below parent
- **Member Properties:** Displayed in adjacent columns
- **Visual Style:** Excel pivot table with extra property columns

### Configuration

**C# Code:**
```csharp
olapGrid1.Layout = GridLayout.ExcelLikeLayoutWithMemberProperties;
```

**VB.NET Code:**
```vbnet
Me.OlapGrid1.Layout = GridLayout.ExcelLikeLayoutWithMemberProperties
```

**XAML:**
```xaml
<syncfusion:OlapGrid Name="olapGrid1" Layout="ExcelLikeLayoutWithMemberProperties" />
```

### Prerequisites

This layout only works when:
1. Member properties are defined in the OLAP cube
2. Properties are bound to dimension members in the OlapReport
3. The dimension has properties like Name, Address, Phone, Email, etc.

### When to Use

Use Excel-Like with Member Properties when:
- Dimension members have important properties (customer names, addresses, product codes)
- Users need contextual information alongside dimension values
- Detailed analysis requires property-level data
- Report consumers need to identify members by properties not just keys

### Visual Structure Example

```
Customer            | Customer Name        | City      | Country    | [Measures]
  ALFKI            | Alfreds Futterkiste  | Berlin    | Germany    | [values]
  ANATR            | Ana Trujillo         | México    | Mexico     | [values]
  ANTON            | Antonio Moreno       | México    | Mexico     | [values]
  Total            |                      |           |            | [summary]
```

Properties (Customer Name, City, Country) displayed alongside dimension members.

### OlapReport Configuration for Properties

```csharp
// Configure dimension with member properties
DimensionElement customerDimension = new DimensionElement();
customerDimension.Name = "Customer";
customerDimension.AddLevel("Customer", "Customer ID");

// Member properties will be automatically displayed
// if they exist in the cube and the layout is set correctly
```

See `kpi-and-member-properties.md` for detailed member property configuration.

## Normal Top Summary Layout

Similar to Normal layout, but summary cells appear at the top of each parent member group instead of the bottom.

### Characteristics

- **Summary Position:** Top of parent member group
- **Child Position:** Adjacent to parent (below the summary)
- **Visual Style:** Summary-first presentation
- **Use Case:** When totals are more important than details

### Configuration

**C# Code:**
```csharp
olapGrid1.Layout = GridLayout.NormalTopSummary;
```

**VB.NET Code:**
```vbnet
Me.OlapGrid1.Layout = GridLayout.NormalTopSummary
```

**XAML:**
```xaml
<syncfusion:OlapGrid Name="olapGrid1" Layout="NormalTopSummary" />
```

### When to Use

Use Normal Top Summary when:
- Summary/total values are the primary focus
- Users want to see aggregates before diving into details
- Executive dashboards where totals matter most
- Quick scanning of summary values is critical

### Visual Structure Example

```
Customer Geography
├─ Total              [summary values]  ← Summary at top
├─ Australia          [values]
├─ Canada             [values]
└─ France             [values]
```

## No Summaries Layout

Hides all summary/total cells, displaying only the leaf-level detail data. Child members appear adjacent to parent members.

### Characteristics

- **Summary Position:** Hidden (not displayed)
- **Child Position:** Adjacent to parent
- **Visual Style:** Detail-only view
- **Data Focus:** Raw dimensional data without aggregations

### Configuration

**C# Code:**
```csharp
olapGrid1.Layout = GridLayout.NoSummaries;
```

**VB.NET Code:**
```vbnet
Me.OlapGrid1.Layout = GridLayout.NoSummaries
```

**XAML:**
```xaml
<syncfusion:OlapGrid Name="olapGrid1" Layout="NoSummaries" />
```

### When to Use

Use No Summaries layout when:
- Users only need detailed data (no aggregates)
- Summary calculations are misleading or inappropriate
- Raw data export/analysis is the goal
- Screen space should be maximized for details
- The cube already calculates needed aggregates at leaf level

### Visual Structure Example

```
Customer Geography
├─ Australia          [values]
├─ Canada             [values]
└─ France             [values]

(No Total row)
```

### Important Considerations

- Drill-down still works (expanding shows child members)
- Calculations at measure level still occur
- Only summary rows are hidden, not measure calculations
- Good for "flat" analysis without hierarchy totals

## Layout Configuration

### Setting Layout Programmatically

```csharp
public void ConfigureGridLayout()
{
    // Set layout based on user preference or business requirement
    olapGrid1.Layout = GridLayout.ExcelLikeLayout;
    
    // Rebind if data is already loaded
    olapGrid1.DataBind();
}
```

### Dynamic Layout Switching

Allow users to switch layouts at runtime:

```csharp
private void LayoutComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string selectedLayout = (e.AddedItems[0] as ComboBoxItem).Content.ToString();
    
    switch (selectedLayout)
    {
        case "Normal":
            olapGrid1.Layout = GridLayout.Normal;
            break;
        case "Excel-Like":
            olapGrid1.Layout = GridLayout.ExcelLikeLayout;
            break;
        case "Excel-Like with Properties":
            olapGrid1.Layout = GridLayout.ExcelLikeLayoutWithMemberProperties;
            break;
        case "Top Summary":
            olapGrid1.Layout = GridLayout.NormalTopSummary;
            break;
        case "No Summaries":
            olapGrid1.Layout = GridLayout.NoSummaries;
            break;
    }
    
    olapGrid1.DataBind();
}
```

### XAML with Data Binding

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <!-- Layout Selector -->
        <ComboBox Grid.Row="0" SelectionChanged="LayoutComboBox_SelectionChanged"
                  SelectedIndex="0" Width="200" HorizontalAlignment="Left" Margin="10">
            <ComboBoxItem Content="Normal"/>
            <ComboBoxItem Content="Excel-Like"/>
            <ComboBoxItem Content="Excel-Like with Properties"/>
            <ComboBoxItem Content="Top Summary"/>
            <ComboBoxItem Content="No Summaries"/>
        </ComboBox>
        
        <!-- OLAP Grid -->
        <syncfusion:OlapGrid Grid.Row="1" Name="olapGrid1" Layout="Normal" />
    </Grid>
</Window>
```

## Layout Selection Guidelines

### Decision Matrix

**Choose Normal when:**
- ✓ Traditional OLAP/BI tool users
- ✓ Compact vertical layout needed
- ✓ Classic pivot table behavior expected

**Choose Excel-Like when:**
- ✓ Business analysts familiar with Excel
- ✓ Clear visual hierarchy important
- ✓ Intuitive parent-child relationships needed

**Choose Excel-Like with Properties when:**
- ✓ Member properties exist in cube
- ✓ Properties provide essential context
- ✓ Detailed member identification required

**Choose Normal Top Summary when:**
- ✓ Summary values are primary focus
- ✓ Executive/management reports
- ✓ Quick total scanning is critical

**Choose No Summaries when:**
- ✓ Detail-only analysis required
- ✓ Summaries are meaningless or wrong
- ✓ Raw data export is the goal

### Performance Considerations

All layouts have similar performance characteristics. Choose based on user experience, not performance.

**However:**
- **Excel-Like with Properties** may load slightly slower if many properties exist
- **No Summaries** may improve rendering speed for very large datasets (fewer cells to calculate)

### Accessibility

- **Excel-Like** layouts provide better screen reader support due to clear hierarchy
- **Normal** and **Top Summary** are more compact for keyboard navigation
- **No Summaries** reduces complexity for assistive technologies

## Sample Locations

Explore layout examples in the Syncfusion sample browser:

```
{System Drive}:\Users\<User Name>\AppData\Local\Syncfusion\EssentialStudio\<Version>\WPF\OlapGrid.WPF\Samples\Appearance\Grid Layout
```

## Common Patterns

### User-Configurable Layout

```csharp
// Save user preference
Properties.Settings.Default.OlapGridLayout = olapGrid1.Layout.ToString();
Properties.Settings.Default.Save();

// Restore on load
public void LoadUserPreferences()
{
    string savedLayout = Properties.Settings.Default.OlapGridLayout;
    if (!string.IsNullOrEmpty(savedLayout))
    {
        olapGrid1.Layout = (GridLayout)Enum.Parse(typeof(GridLayout), savedLayout);
    }
}
```

### Context-Sensitive Layout

```csharp
// Different layouts for different report types
public void ApplyReportLayout(string reportType)
{
    switch (reportType)
    {
        case "Executive":
            olapGrid1.Layout = GridLayout.NormalTopSummary;
            break;
        case "Detailed":
            olapGrid1.Layout = GridLayout.ExcelLikeLayoutWithMemberProperties;
            break;
        case "Export":
            olapGrid1.Layout = GridLayout.NoSummaries;
            break;
        default:
            olapGrid1.Layout = GridLayout.ExcelLikeLayout;
            break;
    }
}
```

## Troubleshooting

**Member properties not appearing:**
- Verify cube has member properties defined
- Confirm layout is set to `ExcelLikeLayoutWithMemberProperties`
- Check that dimension includes levels with properties

**Layout doesn't change:**
- Call `DataBind()` after changing layout
- Verify `OlapDataManager` is set and has data
- Check that layout enum value is valid

**Summary cells missing:**
- If using `NoSummaries`, this is expected
- Verify data exists for the dimension
- Check if drill state affects summary visibility

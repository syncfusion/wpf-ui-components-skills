# Drill Operations

## Table of Contents
- [Drill Operations Overview](#drill-operations-overview)
- [Multi-Level Drilling](#multi-level-drilling)
- [Drill Position Configuration](#drill-position-configuration)
- [All Level Type Member Support](#all-level-type-member-support)
- [Expander Control](#expander-control)
- [Drill Events and Programmatic Control](#drill-events-and-programmatic-control)
- [Drill State Management](#drill-state-management)
- [Best Practices](#best-practices)

## Drill Operations Overview

Drill operations are fundamental OLAP Grid capabilities that allow users to navigate through hierarchical dimension data. Drilling controls the level of detail displayed, enabling users to:

- **Drill Down:** Expand a member to view its child members (more detail)
- **Drill Up:** Collapse child members back to parent level (less detail)
- **Navigate hierarchies:** Move through levels like Year → Quarter → Month → Day

**Visual Indicators:**
- **Plus (+) icon:** Indicates member can be drilled down (has children)
- **Minus (-) icon:** Indicates member is expanded and can be collapsed
- **Arrow icon:** Alternative expander style
- **No icon:** Leaf-level member (no children)

**Benefits:**
- Manage information complexity by showing only relevant detail
- Progressive disclosure of data
- Improved performance (load only expanded data)
- User-controlled analysis depth

## Multi-Level Drilling

Users can drill through multiple hierarchy levels to reach desired detail.

### Hierarchy Example

**Date Dimension Hierarchy:**
```
All Periods (Level 0)
└─ FY 2023 (Level 1 - Year)
    ├─ Q1 2023 (Level 2 - Quarter)
    │   ├─ Jan 2023 (Level 3 - Month)
    │   │   ├─ 2023-01-01 (Level 4 - Day)
    │   │   ├─ 2023-01-02
    │   │   └─ ...
    │   ├─ Feb 2023
    │   └─ Mar 2023
    ├─ Q2 2023
    └─ ...
```

### User Interaction

**Initial State (Level 1):**
```
[+] FY 2022    [values]
[+] FY 2023    [values]
[+] FY 2024    [values]
```

**After Drilling Down FY 2023 (Level 2):**
```
[+] FY 2022         [values]
[-] FY 2023         [values]
    [+] Q1 2023     [values]
    [+] Q2 2023     [values]
    [+] Q3 2023     [values]
    [+] Q4 2023     [values]
[+] FY 2024         [values]
```

**After Further Drilling Q1 2023 (Level 3):**
```
[+] FY 2022             [values]
[-] FY 2023             [values]
    [-] Q1 2023         [values]
        [+] Jan 2023    [values]
        [+] Feb 2023    [values]
        [+] Mar 2023    [values]
    [+] Q2 2023         [values]
    [+] Q3 2023         [values]
    [+] Q4 2023         [values]
[+] FY 2024             [values]
```

### Configuration

Drilling is enabled by default. Users simply click the expander icons to drill down or up.

**Initial Drill State:**

```csharp
// Start with specific members already drilled
olapDataManager.CurrentReport.ShowExpanders = true; // Default
```

## Drill Position Configuration

Drill Position is an advanced drilling mode that affects only the clicked member's position, without expanding the same member in other locations.

### Standard Drilling Behavior

By default, drilling a member expands that member everywhere it appears in the grid.

**Example:**
- User drills "FY 2023" in the first product category row
- "FY 2023" automatically expands in ALL product category rows

### Drill Position Behavior

With DrillPosition enabled, drilling affects only the specific cell clicked.

**Example:**
- User drills "FY 2023" in the "Bikes" row
- "FY 2023" expands ONLY for "Bikes"
- "FY 2023" remains collapsed for "Clothing", "Accessories", etc.

### Enabling Drill Position

**C# Code:**
```csharp
olapDataManager.CurrentReport.DrillType = DrillType.DrillPosition;
```

**VB.NET Code:**
```vbnet
olapDataManager.CurrentReport.DrillType = DrillType.DrillPosition
```

### When to Use Drill Position

Use Drill Position when:
- Users need granular control over what expands
- Different rows require different drill depths
- Comparative analysis at different detail levels
- Large datasets where expanding all instances would be overwhelming

**Example Scenario:**
Analyzing sales by product and time, where you want to see monthly detail for "Bikes" but only quarterly detail for "Accessories."

### DrillType Enumeration

```csharp
public enum DrillType
{
    DrillDownOnly,    // Standard drilling (expands all instances)
    DrillPosition     // Position-specific drilling
}
```

**Default:** `DrillDownOnly`

### Sample Location

```
{System Drive}:\Users\<User Name>\AppData\Local\Syncfusion\EssentialStudio\<Version>\WPF\OlapGrid.WPF\Samples\Data Relation\Drill Types
```

## All Level Type Member Support

The "All" level member is the topmost member in a hierarchy that acts as parent to all other members. Displaying this member provides a complete hierarchy view and an additional aggregation level.

### What is "All" Level?

In OLAP hierarchies, the "All" level represents the complete aggregate:

```
[All Products]           ← "All" level
├─ Bikes
│   ├─ Mountain Bikes
│   └─ Road Bikes
├─ Clothing
└─ Accessories
```

### Enabling "All" Level Member

**C# Code:**
```csharp
OlapDataManager dataManager = new OlapDataManager(connectionString)
{
    ShowLevelTypeAll = true
};
```

**VB.NET Code:**
```vbnet
Dim dataManager As OlapDataManager = New OlapDataManager(connectionString) With {
    .ShowLevelTypeAll = True
}
```

**Default:** `false` (All level is hidden)

### Benefits of Showing "All" Level

**Hierarchy Clarity:**
- Clear parent-child relationships
- Users see complete hierarchy structure
- Easier understanding of drill navigation

**Additional Aggregation:**
- Grand total level
- Comparison point for drill-down analysis
- Validates that child members sum to parent

**Control:**
- Single expander controls visibility of entire hierarchy
- Collapse all members at once
- Simplifies navigation in complex hierarchies

### Visual Example

**With ShowLevelTypeAll = false:**
```
Products
├─ [+] Bikes            [values]
├─ [+] Clothing         [values]
└─ [+] Accessories      [values]
```

**With ShowLevelTypeAll = true:**
```
[-] All Products        [values]  ← New "All" level
    ├─ [+] Bikes        [values]
    ├─ [+] Clothing     [values]
    └─ [+] Accessories  [values]
```

### When to Use "All" Level

Enable "All" level when:
- ✓ Users need to see complete hierarchy structure
- ✓ Grand totals at highest level are important
- ✓ Collapsing entire hierarchy with one click is desired
- ✓ Training users on hierarchy navigation
- ✓ Verifying data integrity (children sum to All level)

Disable "All" level when:
- ✓ Screen space is limited
- ✓ Users are experienced and don't need top-level
- ✓ "All" level doesn't provide meaningful business context
- ✓ Initial view should show more specific categories

### Sample Location

```
{System Drive}:\Users\<User Name>\AppData\Local\Syncfusion\EssentialStudio\<Version>\WPF\OlapGrid.WPF\Samples\Defining Reports\Reports-in-code
```

## Expander Control

Expanders are the UI elements (+ / - icons) that enable drilling. You can control their visibility.

### Hiding Expanders

**C# Code:**
```csharp
olapGrid1.OlapDataManager.CurrentReport.ShowExpanders = false;
```

**VB.NET Code:**
```vbnet
Me.OlapGrid1.OlapDataManager.CurrentReport.ShowExpanders = False
```

**Effect:**
- All + / - icons are hidden
- Users cannot drill interactively
- Grid shows static, non-interactive hierarchy

### When to Hide Expanders

Hide expanders when:
- Creating static reports for printing/export
- Data is fully expanded to desired level programmatically
- Users should not modify drill state
- Simplified UI is needed without drill capability

**Note:** Hiding expanders doesn't prevent programmatic drilling (see below).

## Drill Events and Programmatic Control

While interactive drilling is common, you can also control drill state programmatically.

### Drill Events

Monitor when users drill to respond with custom logic:

```csharp
// Subscribe to drill events (if available in API)
olapGrid1.DrillSuccess += OlapGrid1_DrillSuccess;

private void OlapGrid1_DrillSuccess(object sender, DrillEventArgs e)
{
    // e.DrillInfo contains information about drilled member
    string memberName = e.DrillInfo.MemberUniqueName;
    bool isExpanded = e.DrillInfo.IsExpanded;
    
    // Custom logic: logging, analytics, conditional actions
    LogDrillAction(memberName, isExpanded);
}
```

### Programmatic Drilling

Control drill state through code:

```csharp
// Example: Expand specific members on load
public void ExpandFiscalYear2023()
{
    // Access internal grid and trigger drill operation
    // Implementation depends on OlapGrid API for programmatic drill
    // Consult Syncfusion documentation for exact methods
}
```

### Initial Expanded State

Configure which members start expanded:

```csharp
// In OlapReport configuration
private OlapReport CreateReportWithExpandedMembers()
{
    OlapReport report = new OlapReport();
    report.CurrentCubeName = "Adventure Works";
    
    // Configure dimensions
    DimensionElement date = new DimensionElement();
    date.Name = "Date";
    date.AddLevel("Fiscal", "Fiscal Year");
    
    // Set initial drill state (if supported)
    // Exact implementation depends on API version
    
    return report;
}
```

## Drill State Management

### Preserving Drill State

When users navigate away and return, preserve their drill state:

```csharp
// Save drill state
private Dictionary<string, bool> SaveDrillState()
{
    // Extract current expanded/collapsed state
    // Implementation depends on OlapGrid API
    Dictionary<string, bool> drillState = new Dictionary<string, bool>();
    
    // Store member unique names and their expanded state
    // drillState["[Date].[Fiscal].[Fiscal Year].&[2023]"] = true;
    
    return drillState;
}

// Restore drill state
private void RestoreDrillState(Dictionary<string, bool> drillState)
{
    foreach (var member in drillState)
    {
        if (member.Value) // If was expanded
        {
            // Programmatically expand this member
        }
    }
}
```

### Session Persistence

```csharp
// Save to session storage
Session["OlapDrillState"] = SaveDrillState();

// Restore from session
var savedState = Session["OlapDrillState"] as Dictionary<string, bool>;
if (savedState != null)
{
    RestoreDrillState(savedState);
}
```

## Best Practices

### Performance Optimization

**Limit Initial Drill Depth:**
```csharp
// Start at year level, let users drill to month if needed
dimensionElement.AddLevel("Fiscal", "Fiscal Year"); // Not "Month"
```

**Why:** Less initial data load = faster rendering

**Use Drill Position for Large Datasets:**
```csharp
olapDataManager.CurrentReport.DrillType = DrillType.DrillPosition;
```

**Why:** Prevents expanding too much data at once

### User Experience

**Show "All" Level for New Users:**
```csharp
dataManager.ShowLevelTypeAll = true;
```

**Why:** Helps users understand hierarchy structure

**Keep Expanders Visible:**
```csharp
olapDataManager.CurrentReport.ShowExpanders = true; // Default
```

**Why:** Enables interactive exploration

**Provide Expand/Collapse All Buttons:**
```xaml
<Button Content="Expand All" Click="ExpandAll_Click" />
<Button Content="Collapse All" Click="CollapseAll_Click" />
```

**Why:** Quick navigation for power users

### Data Design

**Logical Hierarchy Levels:**
- Geography: Country → State → City
- Time: Year → Quarter → Month → Day
- Product: Category → Subcategory → Product

**Avoid:**
- Too many levels (>5 makes navigation tedious)
- Illogical progressions
- Levels with single child (redundant)

### Testing

**Test Drill Scenarios:**
- Drill down to leaf level
- Drill up from leaf to top
- Mixed drill states (some expanded, some collapsed)
- Drill Position behavior
- Performance with fully expanded hierarchy

## Common Patterns

### Pattern 1: Expand First Level Only

```csharp
// Initial load shows first level expanded, rest collapsed
public void SetupInitialDrillState()
{
    // Configure report to show first level
    dimensionElement.AddLevel("Fiscal", "Fiscal Year");
    
    // Users can drill to Quarter, Month as needed
}
```

### Pattern 2: Context-Sensitive Drilling

```csharp
// Different drill behavior based on data volume
public void ConfigureDrillingBasedOnData(int rowCount)
{
    if (rowCount > 1000)
    {
        // Large dataset: use drill position
        olapDataManager.CurrentReport.DrillType = DrillType.DrillPosition;
    }
    else
    {
        // Small dataset: normal drilling is fine
        olapDataManager.CurrentReport.DrillType = DrillType.DrillDownOnly;
    }
}
```

### Pattern 3: Guided Navigation

```csharp
// Provide hints to users
private void ShowDrillInstructions()
{
    MessageBox.Show(
        "Click the + icons to expand and view more detail.\n" +
        "Click the - icons to collapse and see less detail.",
        "Drill Navigation Help",
        MessageBoxButton.OK,
        MessageBoxImage.Information
    );
}
```

## Troubleshooting

**Expanders not appearing:**
- Verify `ShowExpanders = true`
- Check that members have children to expand
- Ensure hierarchy has multiple levels
- Confirm cube data includes child members

**Drilling doesn't work:**
- Check OlapDataManager is bound to grid
- Verify data is loaded (`DataBind()` called)
- Ensure member has children in cube
- Check for JavaScript/event errors (if web-based)

**All level not showing:**
- Set `ShowLevelTypeAll = true` on OlapDataManager
- Verify cube defines "All" level
- Check hierarchy configuration in cube

**Drill Position not working as expected:**
- Confirm `DrillType = DrillPosition`
- Verify version supports this feature
- Check that drill is triggered correctly

**Performance issues when drilling:**
- Reduce initial drill depth (higher level start)
- Use Drill Position mode
- Implement paging (see `advanced-features.md`)
- Consider offline cube for faster local access

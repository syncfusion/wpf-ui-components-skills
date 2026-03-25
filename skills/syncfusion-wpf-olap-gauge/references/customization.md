# Customization in OLAP Gauge

Comprehensive guide for customizing OLAP Gauge layout, visibility settings, and XAML-based configuration.

## Table of Contents

- [Overview](#overview)
- [Layout Customization](#layout-customization)
- [Visibility Control](#visibility-control)
- [XAML Configuration](#xaml-configuration)
- [DataSource Properties](#datasource-properties)
- [Report Configuration in XAML](#report-configuration-in-xaml)
- [Dimension Configuration in XAML](#dimension-configuration-in-xaml)
- [KPI Configuration in XAML](#kpi-configuration-in-xaml)
- [Complete XAML Examples](#complete-xaml-examples)
- [Design-Time vs Runtime Configuration](#design-time-vs-runtime-configuration)
- [Best Practices](#best-practices)

## Overview

The OLAP Gauge provides extensive customization options for:
- **Layout**: Control rows and columns for multiple gauge display
- **Visibility**: Show/hide headers, labels, and factors
- **XAML Configuration**: Define entire control setup in XAML without code-behind

## Layout Customization

### Multiple Gauge Arrangement

When displaying multiple KPIs or dimension members, control the gauge layout with `RowsCount` and `ColumnsCount` properties.

### ColumnsCount Property

Specifies the number of columns for displaying gauges.

**XAML:**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" ColumnsCount="2"/>
```

**C#:**

```csharp
this.OlapGauge1.ColumnsCount = 2;
```

**VB.NET:**

```vb
Me.OlapGauge1.ColumnsCount = 2
```

### RowsCount Property

Specifies the number of rows for displaying gauges.

**XAML:**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" RowsCount="2"/>
```

**C#:**

```csharp
this.OlapGauge1.RowsCount = 2;
```

**VB.NET:**

```vb
Me.OlapGauge1.RowsCount = 2
```

### Combined Layout Configuration

**2x2 Grid (4 gauges):**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" 
                      RowsCount="2" 
                      ColumnsCount="2"/>
```

```csharp
this.OlapGauge1.ColumnsCount = 2;
this.OlapGauge1.RowsCount = 2;
```

Result: Displays up to 4 gauges in a 2x2 grid layout.

![Multiple gauges in 2x2 layout](Gauge-Customization_images/Gauge-customization.png)

### Layout Patterns

**Horizontal strip (3 gauges):**

```csharp
this.OlapGauge1.ColumnsCount = 3;
this.OlapGauge1.RowsCount = 1;
```

**Vertical stack (4 gauges):**

```csharp
this.OlapGauge1.ColumnsCount = 1;
this.OlapGauge1.RowsCount = 4;
```

**3x3 Grid (9 gauges):**

```csharp
this.OlapGauge1.ColumnsCount = 3;
this.OlapGauge1.RowsCount = 3;
```

### Auto-Arrangement

If you have 6 KPIs but set ColumnsCount=3 and RowsCount=2:
- First 6 gauges fill the 3×2 grid
- Additional gauges (if any) are not displayed
- Consider increasing grid size if all gauges must be visible

### Dynamic Layout Calculation

```csharp
private void ConfigureLayoutForGaugeCount(int gaugeCount)
{
    // Calculate optimal layout
    int columns = (int)Math.Ceiling(Math.Sqrt(gaugeCount));
    int rows = (int)Math.Ceiling((double)gaugeCount / columns);
    
    this.OlapGauge1.ColumnsCount = columns;
    this.OlapGauge1.RowsCount = rows;
}

// Example: 6 gauges → 3 columns × 2 rows
ConfigureLayoutForGaugeCount(6);
```

## Visibility Control

### Gauge Header Visibility

The gauge header displays the combination of measure and KPI details.

**Property:** `ShowGaugeHeaders`

**Hide headers:**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" ShowGaugeHeaders="False"/>
```

```csharp
OlapGauge1.ShowGaugeHeaders = false;
```

```vb
OlapGauge1.ShowGaugeHeaders = False
```

![Hidden gauge headers](Gauge-Customization_images/Gauge-customization-header.png)

**Use case:** Clean, minimalist dashboards where context is obvious

### Gauge Label Visibility

Gauge labels are displayed inside the gauge showing scale values.

**Property:** `ShowGaugeLabels`

**Hide labels:**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" ShowGaugeLabels="False"/>
```

```csharp
OlapGauge1.ShowGaugeLabels = false;
```

```vb
OlapGauge1.ShowGaugeLabels = False
```

![Hidden gauge labels](Gauge-Customization_images/Gauge-customization-label.png)

**Use case:** High-level executive dashboards focusing on status indicators

### Gauge Factor Visibility

The gauge factor component provides additional gauge information.

**Property:** `ShowGaugeFactors`

**Hide factors:**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" ShowGaugeFactors="False"/>
```

```csharp
OlapGauge1.ShowGaugeFactors = false;
```

```vb
OlapGauge1.ShowGaugeFactors = False
```

![Hidden gauge factors](Gauge-Customization_images/Gauge-customization-factor.png)

**Use case:** Simplified display when factor information is not needed

### Combined Visibility Configuration

**Minimal gauge (only value and goal):**

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" 
                      ShowGaugeHeaders="False"
                      ShowGaugeLabels="False"
                      ShowGaugeFactors="False"/>
```

```csharp
this.OlapGauge1.ShowGaugeHeaders = false;
this.OlapGauge1.ShowGaugeLabels = false;
this.OlapGauge1.ShowGaugeFactors = false;
```

**Standard display (all visible - default):**

```csharp
this.OlapGauge1.ShowGaugeHeaders = true;  // Default
this.OlapGauge1.ShowGaugeLabels = true;   // Default
this.OlapGauge1.ShowGaugeFactors = true;  // Default
```

## XAML Configuration

XAML configuration allows you to define the OLAP Gauge entirely in design-time without code-behind.

### Key Benefits

- **Design-time visibility**: See gauge configuration in XAML designer
- **No code-behind required**: Complete setup in markup
- **Maintainability**: Easier to review and modify configuration
- **Separation of concerns**: UI definition separate from business logic

### Required Namespace

```xml
xmlns:olapshared="clr-namespace:Syncfusion.Windows.Shared;assembly=Syncfusion.OlapShared.WPF"
```

### Basic XAML Structure

```xml
<syncfusion:OlapGauge x:Name="olapGauge"
                      CurrentCubeName="Adventure Works"
                      ReportName="SalesReport">
    <!-- Configuration here -->
</syncfusion:OlapGauge>
```

## DataSource Properties

### DataSource.ConnectionString

Specifies the OLAP connection string directly in XAML.

**Attached property syntax:**

```xml
<syncfusion:OlapGauge x:Name="olapGauge"
                      olapshared:DataSource.ConnectionString="datasource=localhost; initial catalog=adventure works dw"/>
```

**Example with online XML/A:**

```xml
<syncfusion:OlapGauge x:Name="olapGauge"
                      olapshared:DataSource.ConnectionString="Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; Initial Catalog=Adventure Works DW 2008 SE"/>
```

### DataSource.ConnectionName

References a connection string defined in App.Config.

**App.Config:**

```xml
<configuration>
  <connectionStrings>
    <add name="OlapConnection" 
         connectionString="datasource=localhost; initial catalog=adventure works dw"/>
  </connectionStrings>
</configuration>
```

**XAML:**

```xml
<syncfusion:OlapGauge x:Name="olapGauge"
                      olapshared:DataSource.ConnectionName="OlapConnection"/>
```

**Benefits:**
- Centralized connection management
- Easy environment-specific configuration
- Secure credential storage

### DataSource.DataManagerName

Specifies the data manager name for identification.

```xml
<syncfusion:OlapGauge x:Name="olapGauge"
                      olapshared:DataSource.DataManagerName="localManager"/>
```

### SharedDataManagerName

References a shared data manager from a shared collection (advanced scenario).

```xml
<syncfusion:OlapGauge x:Name="olapGauge"
                      SharedDataManagerName="localManager"/>
```

## Report Configuration in XAML

### ReportName Property

Specifies the name of the OLAP report.

```xml
<syncfusion:OlapGauge x:Name="olapGauge"
                      ReportName="SalesReport"/>
```

### CurrentCubeName Property

Specifies the OLAP cube to use from the database.

```xml
<syncfusion:OlapGauge x:Name="olapGauge"
                      CurrentCubeName="Adventure Works"/>
```

### Report Axes in XAML

OLAP reports have three axes that can be configured in XAML:

1. **CategoricalAxis**: Primary axis for dimensions and KPIs
2. **SeriesAxis**: Secondary axis for additional dimensions
3. **SlicerAxis**: Filter axis (rarely used in gauges)

## Dimension Configuration in XAML

### Basic Dimension

```xml
<syncfusion:OlapGauge.CategoricalAxis>
    <syncfusion:Dimension Name="Date" 
                          HierarchyName="Fiscal" 
                          LevelName="Fiscal Year"/>
</syncfusion:OlapGauge.CategoricalAxis>
```

### Dimension Properties

| Property | Description | Example |
|----------|-------------|---------|
| `Name` | Dimension name from cube | "Date", "Product", "Geography" |
| `HierarchyName` | Hierarchy within dimension | "Fiscal", "Calendar", "Product Categories" |
| `LevelName` | Level within hierarchy | "Fiscal Year", "Month", "Product Line" |
| `IncludeMembers` | Specific members to include | "FY 2002, FY 2003" |

### Dimension with Member Filtering

**Include specific members (comma-separated):**

```xml
<syncfusion:Dimension Name="Date" 
                      HierarchyName="Fiscal" 
                      LevelName="Fiscal Year" 
                      IncludeMembers="FY 2002, FY 2003"/>
```

**Multiple members:**

```xml
<syncfusion:Dimension Name="Product" 
                      HierarchyName="Product Model Lines" 
                      LevelName="Product Line" 
                      IncludeMembers="Road, Mountain, Touring"/>
```

### Multiple Dimensions in Axis

```xml
<syncfusion:OlapGauge.CategoricalAxis>
    <!-- Dimension 1 -->
    <syncfusion:Dimension Name="Date" 
                          HierarchyName="Fiscal" 
                          LevelName="Fiscal Year"/>
    
    <!-- Dimension 2 -->
    <syncfusion:Dimension Name="Sales Channel" 
                          HierarchyName="Sales Channel" 
                          LevelName="Sales Channel"/>
</syncfusion:OlapGauge.CategoricalAxis>
```

## KPI Configuration in XAML

### Basic KPI

```xml
<syncfusion:OlapGauge.CategoricalAxis>
    <syncfusion:Kpi Name="Revenue"/>
</syncfusion:OlapGauge.CategoricalAxis>
```

### KPI with Display Options

```xml
<syncfusion:Kpi Name="Revenue" 
                ShowGoal="True" 
                ShowStatus="True" 
                ShowValue="True" 
                ShowTrend="True"/>
```

### KPI Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | string | KPI name from cube |
| `ShowGoal` | bool | Display goal marker |
| `ShowStatus` | bool | Display status indicator |
| `ShowValue` | bool | Display value pointer |
| `ShowTrend` | bool | Display trend arrow |

### Multiple KPIs

```xml
<syncfusion:OlapGauge.CategoricalAxis>
    <syncfusion:Kpi Name="Revenue" 
                    ShowGoal="True" 
                    ShowStatus="True" 
                    ShowValue="True"/>
    
    <syncfusion:Kpi Name="Growth" 
                    ShowGoal="True" 
                    ShowValue="True"/>
    
    <syncfusion:Kpi Name="Profit" 
                    ShowStatus="True" 
                    ShowValue="True"/>
</syncfusion:OlapGauge.CategoricalAxis>
```

## Complete XAML Examples

### Example 1: Basic XAML Configuration

```xml
<Window x:Class="OlapGaugeApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:olapshared="clr-namespace:Syncfusion.Windows.Shared;assembly=Syncfusion.OlapShared.WPF"
        Title="OLAP Gauge - XAML Config" Height="450" Width="800">
    <Grid>
        <syncfusion:OlapGauge x:Name="olapGauge" 
                              CurrentCubeName="Adventure Works" 
                              ReportName="SalesReport"
                              olapshared:DataSource.ConnectionString="datasource=localhost; initial catalog=adventure works dw">
            
            <!-- Categorical Axis: Dimensions and KPIs -->
            <syncfusion:OlapGauge.CategoricalAxis>
                <syncfusion:Dimension Name="Date" 
                                      HierarchyName="Fiscal" 
                                      LevelName="Fiscal Year"
                                      IncludeMembers="FY 2002, FY 2003"/>
                <syncfusion:Kpi Name="Revenue" 
                                ShowGoal="True" 
                                ShowStatus="True" 
                                ShowValue="True"/>
            </syncfusion:OlapGauge.CategoricalAxis>
            
        </syncfusion:OlapGauge>
    </Grid>
</Window>
```

### Example 2: Multi-Dimensional XAML Configuration

```xml
<syncfusion:OlapGauge x:Name="olapGauge" 
                      CurrentCubeName="Adventure Works" 
                      ReportName="SalesReport"
                      SharedDataManagerName="localManager"
                      olapshared:DataSource.DataManagerName="localManager"
                      olapshared:DataSource.ConnectionString="datasource=localhost; initial catalog=adventure works dw">
    
    <!-- Categorical Axis -->
    <syncfusion:OlapGauge.CategoricalAxis>
        <!-- Date Dimension -->
        <syncfusion:Dimension Name="Date" 
                              HierarchyName="Fiscal" 
                              LevelName="Fiscal Year" 
                              IncludeMembers="FY 2002, FY 2003"/>
        
        <!-- KPI Element -->
        <syncfusion:Kpi Name="Revenue" 
                        ShowGoal="True" 
                        ShowStatus="True" 
                        ShowValue="True" 
                        ShowTrend="True"/>
    </syncfusion:OlapGauge.CategoricalAxis>
    
    <!-- Series Axis -->
    <syncfusion:OlapGauge.SeriesAxis>
        <!-- Sales Channel Dimension -->
        <syncfusion:Dimension Name="Sales Channel" 
                              HierarchyName="Sales Channel" 
                              LevelName="Sales Channel"/>
        
        <!-- Product Dimension -->
        <syncfusion:Dimension Name="Product" 
                              HierarchyName="Product Model Lines" 
                              LevelName="Product Line" 
                              IncludeMembers="Road"/>
    </syncfusion:OlapGauge.SeriesAxis>
    
</syncfusion:OlapGauge>
```

![XAML Configuration Result](XAML-Configuration_images/XAML-Configuration_img1.png)

### Example 3: Multiple KPIs with Layout

```xml
<syncfusion:OlapGauge x:Name="olapGauge" 
                      CurrentCubeName="Adventure Works"
                      RowsCount="2"
                      ColumnsCount="2"
                      olapshared:DataSource.ConnectionString="Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; Initial Catalog=Adventure Works DW 2008 SE">
    
    <syncfusion:OlapGauge.CategoricalAxis>
        <!-- Multiple KPIs in 2x2 layout -->
        <syncfusion:Kpi Name="Revenue" ShowGoal="True" ShowStatus="True" ShowValue="True"/>
        <syncfusion:Kpi Name="Growth" ShowGoal="True" ShowStatus="True" ShowValue="True"/>
        <syncfusion:Kpi Name="Profit" ShowGoal="True" ShowStatus="True" ShowValue="True"/>
        <syncfusion:Kpi Name="Order Quantity" ShowGoal="True" ShowValue="True"/>
    </syncfusion:OlapGauge.CategoricalAxis>
    
</syncfusion:OlapGauge>
```

### Example 4: Configuration from App.Config

**App.Config:**

```xml
<configuration>
  <connectionStrings>
    <add name="OlapConnection" 
         connectionString="Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; Initial Catalog=Adventure Works DW 2008 SE"/>
  </connectionStrings>
</configuration>
```

**MainWindow.xaml:**

```xml
<syncfusion:OlapGauge x:Name="olapGauge" 
                      CurrentCubeName="Adventure Works"
                      olapshared:DataSource.ConnectionName="OlapConnection">
    
    <syncfusion:OlapGauge.CategoricalAxis>
        <syncfusion:Dimension Name="Date" HierarchyName="Fiscal" LevelName="Fiscal Year"/>
        <syncfusion:Kpi Name="Revenue" ShowGoal="True" ShowStatus="True" ShowValue="True"/>
    </syncfusion:OlapGauge.CategoricalAxis>
    
</syncfusion:OlapGauge>
```

## Design-Time vs Runtime Configuration

### Design-Time (XAML)

**Advantages:**
- Visual designer support
- Declarative and readable
- Easy to review configuration
- No compilation needed for changes

**Limitations:**
- Static configuration (no dynamic logic)
- Can't respond to runtime conditions
- Connection strings visible in XAML

**Best for:**
- Fixed reports
- Prototyping
- Simple dashboards

### Runtime (Code-Behind)

**Advantages:**
- Dynamic report generation
- Conditional logic
- User-driven configuration
- Secure credential handling

**Limitations:**
- More verbose
- Less visual
- Requires recompilation

**Best for:**
- User-configurable reports
- Dynamic dimension selection
- Complex business logic

### Hybrid Approach

Combine both for flexibility:

**XAML (structure and defaults):**

```xml
<syncfusion:OlapGauge x:Name="olapGauge" 
                      CurrentCubeName="Adventure Works"
                      RowsCount="2"
                      ColumnsCount="2"/>
```

**Code-Behind (dynamic report):**

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Connection string from secure storage
    string connectionString = GetSecureConnectionString();
    OlapDataManager dataManager = new OlapDataManager(connectionString);
    
    // Report based on user selection
    dataManager.SetCurrentReport(CreateDynamicReport());
    
    this.olapGauge.OlapDataManager = dataManager;
}
```

## Best Practices

### 1. Use XAML for Static Dashboards

When the report structure doesn't change, XAML configuration provides clarity:

```xml
<!-- Clear, declarative, easy to maintain -->
<syncfusion:OlapGauge.CategoricalAxis>
    <syncfusion:Dimension Name="Date" HierarchyName="Fiscal" LevelName="Fiscal Year"/>
    <syncfusion:Kpi Name="Revenue" ShowGoal="True" ShowStatus="True" ShowValue="True"/>
</syncfusion:OlapGauge.CategoricalAxis>
```

### 2. Use Code-Behind for Dynamic Scenarios

When users select dimensions or KPIs at runtime:

```csharp
private OlapReport BuildReportFromUserSelection(string selectedKpi, List<string> selectedDimensions)
{
    // Dynamic report creation based on user choices
}
```

### 3. Connection String Management

**Development:**

```xml
<!-- Direct connection string for dev -->
<syncfusion:OlapGauge olapshared:DataSource.ConnectionString="datasource=localhost; initial catalog=DevDB"/>
```

**Production:**

```xml
<!-- Reference from App.Config -->
<syncfusion:OlapGauge olapshared:DataSource.ConnectionName="ProductionOlap"/>
```

### 4. Layout Considerations

Match layout to screen size and gauge count:

```csharp
// Responsive layout
if (gaugeCount <= 4)
{
    OlapGauge1.ColumnsCount = 2;
    OlapGauge1.RowsCount = 2;
}
else if (gaugeCount <= 9)
{
    OlapGauge1.ColumnsCount = 3;
    OlapGauge1.RowsCount = 3;
}
```

### 5. Visibility Settings Based on Context

**Executive dashboard:**

```xml
<syncfusion:OlapGauge ShowGaugeLabels="False" ShowGaugeFactors="False"/>
```

**Operational dashboard:**

```xml
<syncfusion:OlapGauge ShowGaugeHeaders="True" ShowGaugeLabels="True"/>
```

### 6. Sample Location Reference

Syncfusion provides XAML configuration samples:

```
{system drive}:\Users\<User Name>\AppData\Local\Syncfusion\
EssentialStudio\<Version Number>\WPF\OlapGauge.WPF\Samples\
Defining Reports\XAML Configuration\
```

Study these samples for advanced XAML configuration patterns.

---
name: syncfusion-wpf-olap-gauge
description: Guide for implementing Syncfusion WPF OLAP Gauge control for displaying Key Performance Indicators (KPI) from OLAP data sources. Use this when working with OLAP KPI visualization, business intelligence dashboards, or XML/A data binding in WPF. This skill covers connecting to OLAP cubes, SQL Server Analysis Services, Mondrian servers, and configuring KPI displays for executive dashboards and BI applications.
metadata:
    author: "Syncfusion Inc"
    version: "33.1.44"
---

# Implementing OLAP Gauges

Guide for implementing the Syncfusion® WPF OLAP Gauge control for displaying Key Performance Indicators (KPI) from OLAP data sources. The OLAP Gauge is ideal for highlighting business-critical KPI information in executive dashboards and report cards, presenting values against goals in an intuitive manner.

## When to Use This Skill

Use this skill when you need to:
- **Display KPI data** from OLAP cubes in gauge format
- **Connect to OLAP data sources** (XML/A, SQL Server Analysis Services, Mondrian, ActivePivot)
- **Visualize business metrics** with goals, status, and trend indicators
- **Create executive dashboards** with KPI visualizations
- **Bind OLAP gauge to offline cubes** or local/online servers
- **Customize gauge appearance** with frames, themes, and layouts
- **Configure multiple gauges** in structured layouts
- **Implement OLAP reports** with dimensions and KPI elements
- **Localize OLAP gauge** for international applications
- **Add tooltips** to display value and goal information

## Component Overview

The **OlapGauge** control for WPF provides sophisticated KPI visualization capabilities:

### Key Capabilities

- **OLAP Data Binding:** Built-in support for XML/A data sources, offline cubes, SQL Server Analysis Services, Mondrian, and ActivePivot servers
- **KPI Visualization:** Displays KPI values (pointers), goals (markers), status, and trend indicators
- **Visual Indicators:** Traffic lights, road signs, and standard arrow images for status and trend
- **Multiple Gauge Layouts:** Customizable row and column layouts for displaying multiple gauges
- **Frame Types:** Various built-in frame types (circular, half-circle, full-circle) for rich appearance
- **Theming:** Supports Office 2010/2013, Metro, Blend, and VisualStudio themes
- **Tooltips:** Interactive tooltips for pointers and markers
- **Customization:** Control visibility of headers, labels, factors, and adjust gauge radius
- **Localization:** Multi-language support with RTL (Right-to-Left) capability
- **XAML Configuration:** Complete design-time control configuration without code-behind

### Architecture

```
OlapGauge Control
├── OlapDataManager (manages OLAP connections)
├── OlapReport (defines cube, dimensions, KPI elements)
├── Gauge Visualization (renders KPI data)
│   ├── Pointers (KPI values)
│   ├── Markers (KPI goals)
│   ├── Status Indicators (visual feedback)
│   └── Trend Indicators (directional arrows)
└── Layout (rows/columns for multiple gauges)
```

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this reference when setting up the OLAP Gauge control for the first time:
- Installing and adding control through Visual Studio
- Installing and adding control through Expression Blend
- Adding control programmatically through code-behind
- Required assembly references (Syncfusion.OlapGauge.WPF, Syncfusion.Olap.Base, etc.)
- Setting up OlapDataManager with connection strings
- Creating basic OlapReport with KpiElements and DimensionElements
- Defining cube names and configuring dimensions
- Required namespaces (Syncfusion.Olap.Reports, Syncfusion.Olap.Manager, Syncfusion.Windows.Gauge.Olap)
- Initial rendering and sample output

### Data Binding

📄 **Read:** [references/data-binding.md](references/data-binding.md)

Read this reference when connecting OLAP Gauge to data sources:
- Binding to offline cube files (.cub) with physical path
- Binding to cube in local SQL Server Analysis Services
- Binding to cube in online SQL Server via XML/A
- Binding to Mondrian server OLAP cubes
- Binding to ActivePivot server OLAP cubes
- Connection string formats for different providers
- Authentication and credential configuration
- Setting DataProvider.ProviderName for different servers
- Troubleshooting connection issues

### KPI Visualization

📄 **Read:** [references/kpi-visualization.md](references/kpi-visualization.md)

Read this reference when working with KPI display features:
- Understanding KPI elements and structure
- KPI value representation using pointers
- KPI goal representation using markers
- KPI status indicators (traffic lights, road signs)
- KPI trend indicators (directional arrows)
- Configuring KpiElement properties (ShowKPIGoal, ShowKPIStatus, ShowKPIValue, ShowKPITrend)
- Display options for each KPI component
- Member-KPI combination display patterns
- Visual feedback for performance metrics

### Customization

📄 **Read:** [references/customization.md](references/customization.md)

Read this reference when customizing gauge layout and behavior:
- Layout customization with ColumnsCount and RowsCount properties
- Controlling multiple gauge arrangements
- Gauge header visibility (ShowGaugeHeaders)
- Gauge label visibility (ShowGaugeLabels)
- Gauge factor visibility (ShowGaugeFactors)
- Complete XAML configuration without code-behind
- DataSource.ConnectionString and DataSource.ConnectionName properties
- ReportName and CurrentCubeName configuration in XAML
- CategoricalAxis, SeriesAxis, and SlicerAxis definition
- Dimension and KPI elements in XAML markup
- Calculated members configuration

### Appearance

📄 **Read:** [references/appearance.md](references/appearance.md)

Read this reference when styling the gauge appearance:
- Adjusting gauge radius for size control
- Built-in frame types (CircularCenterGradient, CircularWithDarkOuterFrames, FullCircle, HalfCircle)
- Applying themes with SkinStorage.VisualStyle
- Available themes: Metro, Blend, Office2010Black, Office2010Blue, Office2010Silver
- Office 2013 themes: LightGray, DarkGray, White
- VisualStudio2013 theme
- Setting themes in XAML vs code-behind
- SfSkinManager.SetVisualStyle usage

### Tooltips and Localization

📄 **Read:** [references/tooltips-localization.md](references/tooltips-localization.md)

Read this reference when implementing tooltips or multi-language support:
- Enabling pointer tooltips (ShowPointersTooltip)
- Enabling marker tooltips (ShowMarkersTooltip)
- Tooltip content and formatting
- Localization overview and translation process
- Creating resource files (.resx) with proper naming conventions
- File naming format: Syncfusion.OlapGauge.wpf.<Culture Code>.resx
- Setting CurrentUICulture for culture specification
- RTL (Right-to-Left) support with FlowDirection property
- Combining localization with RTL for Arabic, Hebrew, etc.

## Quick Start Example

### Basic OLAP Gauge with KPI

```xml
<Window x:Class="OlapGaugeApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:OlapGauge x:Name="OlapGauge1"/>
    </Grid>
</Window>
```

```csharp
using Syncfusion.Olap.Manager;
using Syncfusion.Olap.Reports;
using System.Windows;

namespace OlapGaugeApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create data manager with connection string
            var olapDataManager = new OlapDataManager(
                "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " +
                "Initial Catalog=Adventure Works DW 2008 SE;");
            
            // Set OLAP report
            olapDataManager.SetCurrentReport(CreateOlapReport());
            
            // Bind to gauge
            this.OlapGauge1.OlapDataManager = olapDataManager;
        }

        private OlapReport CreateOlapReport()
        {
            OlapReport report = new OlapReport();
            report.CurrentCubeName = "Adventure Works";

            // Configure KPI element
            KpiElements kpiElement = new KpiElements();
            kpiElement.Elements.Add(new KpiElement 
            { 
                Name = "Revenue", 
                ShowKPIGoal = true, 
                ShowKPIStatus = true, 
                ShowKPIValue = true, 
                ShowKPITrend = true 
            });

            // Configure dimensions
            DimensionElement dimensionElement = new DimensionElement();
            dimensionElement.Name = "Date";
            dimensionElement.AddLevel("Fiscal Year", "Fiscal Year");

            // Add elements to report
            report.CategoricalElements.Add(new Item { ElementValue = kpiElement });
            report.CategoricalElements.Add(new Item { ElementValue = dimensionElement });
            
            return report;
        }
    }
}
```

## Common Use Cases

### Executive Dashboard with Multiple KPIs

Display multiple KPI gauges in a structured layout:

```csharp
// Configure 2x2 layout for 4 KPIs
this.OlapGauge1.RowsCount = 2;
this.OlapGauge1.ColumnsCount = 2;
```

### Offline Cube Binding

Connect to a local cube file:

```csharp
string connectionString = @"DataSource = C:\Cubes\Adventure_Works_Ext.cub; Provider = MSOLAP;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

### Complete XAML Configuration

Define entire OLAP Gauge in XAML without code-behind:

```xml
<syncfusion:OlapGauge x:Name="olapGauge" 
                      CurrentCubeName="Adventure Works" 
                      ReportName="SalesReport"
                      olapshared:DataSource.ConnectionString="datasource=localhost; initial catalog=adventure works dw">
    <syncfusion:OlapGauge.CategoricalAxis>
        <syncfusion:Dimension Name="Date" HierarchyName="Fiscal" LevelName="Fiscal Year"/>
        <syncfusion:Kpi Name="Revenue" ShowGoal="True" ShowStatus="True" ShowValue="True"/>
    </syncfusion:OlapGauge.CategoricalAxis>
</syncfusion:OlapGauge>
```

### Themed Gauge with Tooltips

Apply theme and enable interactive tooltips:

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1" 
                      ShowPointersTooltip="True" 
                      ShowMarkersTooltip="True"
                      sfshared:SkinStorage.VisualStyle="Metro"/>
```

## Key Properties

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| `OlapDataManager` | OlapDataManager | Manages OLAP connection and data binding |
| `RowsCount` | int | Number of rows for multiple gauge layout |
| `ColumnsCount` | int | Number of columns for multiple gauge layout |
| `CurrentCubeName` | string | Name of the OLAP cube to use |
| `ReportName` | string | Name of the OLAP report |

### Visibility Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ShowGaugeHeaders` | bool | true | Show/hide gauge headers |
| `ShowGaugeLabels` | bool | true | Show/hide gauge labels |
| `ShowGaugeFactors` | bool | true | Show/hide gauge factors |
| `ShowPointersTooltip` | bool | false | Enable tooltips for pointers (values) |
| `ShowMarkersTooltip` | bool | false | Enable tooltips for markers (goals) |

### Appearance Properties

| Property | Type | Description |
|----------|------|-------------|
| `Radius` | double | Gauge radius size |
| `FrameType` | GaugeFrameType | Frame style (Circular, HalfCircle, etc.) |
| `FlowDirection` | FlowDirection | Layout direction (LeftToRight/RightToLeft) |

### XAML Configuration Properties

| Property | Description |
|----------|-------------|
| `DataSource.ConnectionString` | OLAP connection string |
| `DataSource.ConnectionName` | Connection name from App.Config |
| `SharedDataManagerName` | Shared data manager reference |
| `CategoricalAxis` | Categorical axis dimensions and KPIs |
| `SeriesAxis` | Series axis dimensions |
| `SlicerAxis` | Slicer axis dimensions |

## Related Components

### Within Data Visualization Category

- **[Circular Gauges](../implementing-circular-gauges/)** - For non-OLAP radial gauge visualizations with pointers and ranges
- **[Charts](../implementing-charts/)** - For detailed OLAP data visualization with multiple chart types

### Other Syncfusion WPF Components

For other WPF components, refer to the main library:
- **[Main WPF Library](../../../)** - Complete Syncfusion WPF component documentation

## Assembly References

Required assemblies for OLAP Gauge:
- `Syncfusion.OlapGauge.WPF`
- `Syncfusion.Gauge.WPF`
- `Syncfusion.Olap.Base`
- `Syncfusion.OlapShared.WPF`
- `Syncfusion.Core`
- `Syncfusion.Shared.WPF`

Location: `{Drive}:\Program Files (x86)\Syncfusion\Essential Studio\{version}\precompiledassemblies\{version}\{framework}\`

## Licensing

Starting with v16.2.0.x, Syncfusion license key registration is required. Include the license key in your application. Refer to [Syncfusion licensing documentation](https://help.syncfusion.com/common/essential-studio/licensing/license-key) for details.

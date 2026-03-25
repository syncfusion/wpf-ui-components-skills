# Getting Started with OLAP Gauge

Complete guide for setting up and initializing the Syncfusion WPF OLAP Gauge control in your application.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Assembly References](#assembly-references)
- [Adding Control Through Visual Studio](#adding-control-through-visual-studio)
- [Adding Control Through Expression Blend](#adding-control-through-expression-blend)
- [Adding Control Through Code-Behind](#adding-control-through-code-behind)
- [OlapReport and OlapDataManager Setup](#olapreport-and-olapdatamanager-setup)
- [Creating Basic OLAP Reports](#creating-basic-olap-reports)
- [Understanding KpiElements](#understanding-kpielements)
- [Understanding DimensionElements](#understanding-dimensionelements)
- [Required Namespaces](#required-namespaces)
- [Complete Working Example](#complete-working-example)
- [Troubleshooting Initial Setup](#troubleshooting-initial-setup)

## Overview

The OLAP Gauge control can be initialized and added to a WPF application through three methods:
1. Visual Studio designer (drag-and-drop)
2. Expression Blend designer
3. Code-behind (programmatic creation)

All three methods require proper assembly references and OlapDataManager configuration.

## Prerequisites

### Licensing Requirement

**Important:** Starting with v16.2.0.x, if you reference Syncfusion assemblies from trial setup or NuGet feed, you must include a license key in your projects.

Register the license key in your WPF application:

```csharp
// In App.xaml.cs constructor or Main method
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

Refer to [Syncfusion licensing documentation](https://help.syncfusion.com/common/essential-studio/licensing/license-key) for obtaining and registering your key.

### System Requirements

- .NET Framework 4.5 or later / .NET Core 3.1 or later
- Visual Studio 2017 or later
- Syncfusion Essential Studio WPF installation or NuGet packages

## Assembly References

The OLAP Gauge requires the following Syncfusion assemblies:

### Required Assemblies

- `Syncfusion.OlapGauge.WPF.dll` - Main OLAP Gauge control
- `Syncfusion.Gauge.WPF.dll` - Base gauge functionality
- `Syncfusion.Olap.Base.dll` - OLAP data handling
- `Syncfusion.OlapShared.WPF.dll` - Shared OLAP components
- `Syncfusion.Core.dll` - Core Syncfusion utilities
- `Syncfusion.Shared.WPF.dll` - Shared WPF utilities

### Assembly Location

Default installation path:
```
{Drive}:\Program Files (x86)\Syncfusion\Essential Studio\{version}\precompiledassemblies\{version}\{framework}\
```

Example:
```
C:\Program Files (x86)\Syncfusion\Essential Studio\25.1.35\precompiledassemblies\25.1.35\4.6\
```

### Adding via NuGet

```powershell
Install-Package Syncfusion.OlapGauge.WPF
```

This installs the OLAP Gauge package and all dependencies automatically.

## Adding Control Through Visual Studio

### Step 1: Create WPF Application

1. Open Visual Studio
2. Navigate to **File > New > Project**
3. Select **WPF Application** under Visual C# Templates
4. Name your project and click **Create**

### Step 2: Add Control from Toolbox

1. Open the **Toolbox** (View > Toolbox or Ctrl+Alt+X)
2. Locate the **OLAP Gauge** control under "Syncfusion BI WPF" group
3. Drag the control to the MainWindow.xaml designer

![Visual Studio Toolbox](Getting-Started_images/Getting-Started_img1.png)

### Step 3: Name the Control

In MainWindow.xaml, name the control for code-behind reference:

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

### Step 4: Configure OlapDataManager

Proceed to [OlapReport and OlapDataManager Setup](#olapreport-and-olapdatamanager-setup) to complete the configuration.

## Adding Control Through Expression Blend

### Step 1: Create Project in Blend

1. Open **Blend for Visual Studio**
2. Navigate to **File > New project > WPF > WPF Application**
3. Name your project and click **OK**

### Step 2: Add Assembly References

1. In the **Project** tab (left panel), right-click **References**
2. Select **Add Reference**
3. Browse and add the required Syncfusion assemblies (listed in [Assembly References](#assembly-references))

### Step 3: Add Control from Assets

1. Open the **Assets** tab (usually docked on left side)
2. The OLAP Gauge control appears automatically after adding assemblies
3. Search for "OlapGauge" if needed
4. Drag the control to the designer

![Expression Blend Assets](Getting-Started_images/Getting-Started_img3.png)

### Step 4: Name the Control

```xml
<syncfusion:OlapGauge x:Name="OlapGauge1"/>
```

### Step 5: Configure OlapDataManager

Proceed to [OlapReport and OlapDataManager Setup](#olapreport-and-olapdatamanager-setup).

## Adding Control Through Code-Behind

### Step 1: Create WPF Application

1. Open Visual Studio
2. Create a new **WPF Application** project

### Step 2: Add Assembly References

1. Right-click **References** in Solution Explorer
2. Select **Add Reference**
3. Manually add all required Syncfusion assemblies (see [Assembly References](#assembly-references))

### Step 3: Prepare the Layout

In MainWindow.xaml, name the root Grid:

```xml
<Window x:Class="OlapGaugeApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Grid x:Name="RootGrid"/>
</Window>
```

### Step 4: Create Control Programmatically

In MainWindow.xaml.cs:

```csharp
using System.Windows;
using Syncfusion.Olap.Manager;
using Syncfusion.Olap.Reports;
using Syncfusion.Windows.Gauge.Olap;

namespace OlapGaugeApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create OLAP Gauge instance
            var olapGauge = new OlapGauge();
            
            // Create and configure data manager
            var olapDataManager = new OlapDataManager(
                "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " +
                "Initial Catalog=Adventure Works DW 2008 SE;");
            
            olapDataManager.SetCurrentReport(CreateOlapReport());
            
            // Bind data manager to gauge
            olapGauge.OlapDataManager = olapDataManager;
            
            // Add to visual tree
            this.RootGrid.Children.Add(olapGauge);
        }

        private OlapReport CreateOlapReport()
        {
            // See "Creating Basic OLAP Reports" section
        }
    }
}
```

VB.NET equivalent:

```vb
Imports System.Windows
Imports Syncfusion.Olap.Manager
Imports Syncfusion.Olap.Reports
Imports Syncfusion.Windows.Gauge.Olap

Namespace OlapGaugeApp
    Partial Public Class MainWindow
        Inherits Window
        
        Public Sub New()
            InitializeComponent()
            
            ' Create OLAP Gauge instance
            Dim olapGauge = New OlapGauge()
            
            ' Create and configure data manager
            Dim olapDataManager = New OlapDataManager(
                "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " & _
                "Initial Catalog=Adventure Works DW 2008 SE;")
            
            olapDataManager.SetCurrentReport(CreateOlapReport())
            
            ' Bind data manager to gauge
            olapGauge.OlapDataManager = olapDataManager
            
            ' Add to visual tree
            Me.RootGrid.Children.Add(olapGauge)
        End Sub
        
        Private Function CreateOlapReport() As OlapReport
            ' See "Creating Basic OLAP Reports" section
        End Function
    End Class
End Namespace
```

## OlapReport and OlapDataManager Setup

### Required Namespaces

Include these namespaces in your code-behind file:

```csharp
using Syncfusion.Olap.Manager;   // For OlapDataManager
using Syncfusion.Olap.Reports;   // For OlapReport, KpiElements, DimensionElement
using Syncfusion.Windows.Gauge.Olap;  // For OlapGauge (if adding via code)
```

VB.NET:

```vb
Imports Syncfusion.Olap.Manager
Imports Syncfusion.Olap.Reports
Imports Syncfusion.Windows.Gauge.Olap
```

### OlapDataManager Overview

The `OlapDataManager` manages the connection to your OLAP data source and executes queries defined in the `OlapReport`.

**Constructor:**

```csharp
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

**Connection string format:**

```csharp
"Data Source=<server_url>; Initial Catalog=<database_name>;"
```

### Setting the Report

```csharp
dataManager.SetCurrentReport(CreateOlapReport());
this.OlapGauge1.OlapDataManager = dataManager;
```

## Creating Basic OLAP Reports

An `OlapReport` defines what data to retrieve from the OLAP cube.

### Basic Report Structure

```csharp
private OlapReport CreateOlapReport()
{
    OlapReport report = new OlapReport();
    
    // 1. Specify the cube name
    report.CurrentCubeName = "Adventure Works";
    
    // 2. Add KPI elements
    KpiElements kpiElement = new KpiElements();
    kpiElement.Elements.Add(new KpiElement 
    { 
        Name = "Revenue", 
        ShowKPIGoal = true, 
        ShowKPIStatus = true, 
        ShowKPIValue = true, 
        ShowKPITrend = true 
    });
    
    // 3. Add dimension elements
    DimensionElement dimensionElement1 = new DimensionElement();
    dimensionElement1.Name = "Date";
    dimensionElement1.AddLevel("Fiscal Year", "Fiscal Year");
    
    // 4. Add elements to report axes
    report.CategoricalElements.Add(new Item { ElementValue = dimensionElement1 });
    report.CategoricalElements.Add(new Item { ElementValue = kpiElement });
    
    return report;
}
```

### Report Axes

OLAP reports have three axes for organizing data:

1. **CategoricalElements** - Categories (typically dimensions and KPIs)
2. **SeriesElements** - Series data (additional dimensions)
3. **SlicerElements** - Filters (not commonly used in gauges)

## Understanding KpiElements

KPI (Key Performance Indicator) elements display business metrics.

### KpiElement Properties

```csharp
KpiElement kpiElement = new KpiElement
{
    Name = "Revenue",              // KPI name from cube
    ShowKPIGoal = true,            // Display goal marker
    ShowKPIStatus = true,          // Display status indicator
    ShowKPIValue = true,           // Display value pointer
    ShowKPITrend = true            // Display trend indicator
};
```

### Adding Multiple KPIs

```csharp
KpiElements kpiElements = new KpiElements();
kpiElements.Elements.Add(new KpiElement { Name = "Revenue", ShowKPIGoal = true, ShowKPIStatus = true, ShowKPIValue = true });
kpiElements.Elements.Add(new KpiElement { Name = "Growth", ShowKPIGoal = true, ShowKPIValue = true });
kpiElements.Elements.Add(new KpiElement { Name = "Profit", ShowKPIStatus = true, ShowKPIValue = true });
```

### KPI Display Components

- **Value (Pointer):** The actual KPI value
- **Goal (Marker):** The target value to achieve
- **Status (Icon):** Visual indicator of performance (e.g., traffic light)
- **Trend (Arrow):** Direction indicator (up/down/stable)

## Understanding DimensionElements

Dimensions organize data into hierarchies and levels.

### Basic Dimension

```csharp
DimensionElement dimensionElement = new DimensionElement();
dimensionElement.Name = "Date";                          // Dimension name from cube
dimensionElement.AddLevel("Fiscal Year", "Fiscal Year"); // Hierarchy and level
```

### Dimension with Member Filtering

```csharp
DimensionElement dimensionElement = new DimensionElement();
dimensionElement.Name = "Sales Channel";
dimensionElement.AddLevel("Sales Channel", "Sales Channel");

// Add specific member
dimensionElement.Hierarchy.LevelElements["Sales Channel"].Add("Reseller");

// Include other available members
dimensionElement.Hierarchy.LevelElements["Sales Channel"].IncludeAvailableMembers = true;
```

### Multiple Dimensions

```csharp
// Date dimension
DimensionElement dateElement = new DimensionElement();
dateElement.Name = "Date";
dateElement.AddLevel("Fiscal Year", "Fiscal Year");

// Product dimension
DimensionElement productElement = new DimensionElement();
productElement.Name = "Product";
productElement.AddLevel("Product Model Lines", "Product Line");
productElement.Hierarchy.LevelElements["Product Line"].Add("Road");
productElement.Hierarchy.LevelElements["Product Line"].IncludeAvailableMembers = true;

// Geography dimension
DimensionElement geoElement = new DimensionElement();
geoElement.Name = "Customer";
geoElement.AddLevel("Customer Geography", "Country");
```

## Required Namespaces

Complete namespace list for OLAP Gauge development:

```csharp
using System.Windows;                        // Window, FrameworkElement
using Syncfusion.Olap.Manager;               // OlapDataManager
using Syncfusion.Olap.Reports;               // OlapReport, KpiElements, DimensionElement, Item
using Syncfusion.Windows.Gauge.Olap;         // OlapGauge
```

For theming:

```csharp
using Syncfusion.SfSkinManager;              // SfSkinManager for themes
```

XAML namespace declarations:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
xmlns:olapshared="clr-namespace:Syncfusion.Windows.Shared;assembly=Syncfusion.OlapShared.WPF"
```

## Complete Working Example

### MainWindow.xaml

```xml
<Window x:Class="OlapGaugeApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="OLAP Gauge Example" Height="450" Width="800">
    <Grid>
        <syncfusion:OlapGauge x:Name="OlapGauge1"/>
    </Grid>
</Window>
```

### MainWindow.xaml.cs

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
            
            // Initialize data manager with connection string
            var olapDataManager = new OlapDataManager(
                "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " +
                "Initial Catalog=Adventure Works DW 2008 SE;");
            
            // Set the OLAP report
            olapDataManager.SetCurrentReport(CreateOlapReport());
            
            // Bind to OLAP Gauge
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
            DimensionElement dimensionElement1 = new DimensionElement();
            dimensionElement1.Name = "Date";
            dimensionElement1.AddLevel("Fiscal Year", "Fiscal Year");

            DimensionElement dimensionElement2 = new DimensionElement();
            dimensionElement2.Name = "Sales Channel";
            dimensionElement2.AddLevel("Sales Channel", "Sales Channel");
            dimensionElement2.Hierarchy.LevelElements["Sales Channel"].Add("Reseller");
            dimensionElement2.Hierarchy.LevelElements["Sales Channel"].IncludeAvailableMembers = true;

            DimensionElement dimensionElement3 = new DimensionElement();
            dimensionElement3.Name = "Product";
            dimensionElement3.AddLevel("Product Model Lines", "Product Line");
            dimensionElement3.Hierarchy.LevelElements["Product Line"].Add("Road");
            dimensionElement3.Hierarchy.LevelElements["Product Line"].IncludeAvailableMembers = true;

            // Add to report axes
            report.CategoricalElements.Add(new Item { ElementValue = dimensionElement2 });
            report.CategoricalElements.Add(new Item { ElementValue = dimensionElement1 });
            report.CategoricalElements.Add(new Item { ElementValue = kpiElement });
            report.SeriesElements.Add(new Item { ElementValue = dimensionElement3 });
            
            return report;
        }
    }
}
```

### Expected Output

![OLAP Gauge Sample Output](Getting-Started_images/Getting-Started_img5.png)

The gauge displays:
- KPI value as a pointer
- KPI goal as a marker
- Status indicator (traffic light icon)
- Trend indicator (arrow)
- Multiple gauges for different dimension combinations

## Troubleshooting Initial Setup

### Control Not Appearing in Toolbox

**Solution:**
1. Ensure Syncfusion assemblies are properly referenced
2. Rebuild the solution
3. Close and reopen Visual Studio
4. Manually add assemblies via Add Reference dialog

### "Could not load file or assembly" Error

**Solution:**
1. Verify all required assemblies are referenced (see [Assembly References](#assembly-references))
2. Check that assembly versions match across all Syncfusion references
3. Ensure correct .NET Framework version compatibility
4. Try cleaning solution and rebuilding

### Connection String Errors

**Solution:**
1. Verify the data source URL is accessible
2. Check catalog name matches your OLAP database
3. Ensure XML/A service is running on the server
4. Add credentials if required: `User ID=username; Password=password;`

### Cube Name Not Found

**Solution:**
1. Verify the cube name exactly matches the name in your OLAP database
2. Cube names are case-sensitive
3. Use SSMS or another tool to confirm the cube exists and is accessible

### No Data Displayed

**Solution:**
1. Verify KPI exists in the specified cube
2. Check dimension and level names match the cube schema
3. Ensure connection string points to correct server and catalog
4. Verify members specified in filters actually exist in dimensions

### License Key Warning

**Solution:**
1. Register license key before InitializeComponent():
   ```csharp
   Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_KEY");
   ```
2. Obtain license key from [Syncfusion account](https://www.syncfusion.com/account/manage-trials/downloads)
3. For trial users, generate trial key from Syncfusion website

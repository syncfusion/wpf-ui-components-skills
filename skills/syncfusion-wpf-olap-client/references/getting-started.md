# Getting Started with WPF OLAP Client

## Table of Contents
- [Overview](#overview)
- [License Key Registration](#license-key-registration)
- [Setup Method 1: Through Visual Studio Designer](#setup-method-1-through-visual-studio-designer)
- [Setup Method 2: Through Expression Blend](#setup-method-2-through-expression-blend)
- [Setup Method 3: Through Code-Behind](#setup-method-3-through-code-behind)
- [Required Assemblies](#required-assemblies)
- [Required Namespaces](#required-namespaces)
- [Troubleshooting](#troubleshooting)

## Overview

This guide covers the information required to create a simple OLAP Client bound to an OLAP data source. The OLAP Client control provides three different setup methods to suit your development workflow.


### Install the NuGet Package

To use the OLAP Client in your WPF application, install the following NuGet package:

```powershell
Install-Package Syncfusion.OlapClient.WPF
```

### Assembly References

The OLAP Client requires the following assemblies:

Assembly Reference

* `Syncfusion.Chart.WPF`
* `Syncfusion.Grid.WPF`
* `Syncfusion.GridCommon.WPF`
* `Syncfusion.Linq.Base`
* `Syncfusion.Olap.Base`
* `Syncfusion.OlapChart.WPF`
* `Syncfusion.OlapChartConverter.WPF`
* `Syncfusion.OlapGrid.WPF`
* `Syncfusion.OlapGridCommon.WPF`
* `Syncfusion.OlapGridConverter.WPF`
* `Syncfusion.OlapShared.WPF`
* `Syncfusion.OlapTools.WPF`
* `Syncfusion.Shared.WPF`
* `Syncfusion.Tools.WPF`

NuGet Package

* `Syncfusion.OlapClient.WPF`

## Setup Method 1: Through Visual Studio Designer

### Step 1: Create WPF Application

1. Open Visual Studio IDE
2. Navigate to **File > New > Project**
3. Select **WPF Application** (inside Visual C# templates)
4. Create the new WPF application

### Step 2: Add OLAP Client from Toolbox

1. Go to **View** menu and select **Toolbox** option
2. The toolbox will appear inside Visual Studio IDE
3. From the Visual Studio Toolbox, locate **Syncfusion BI WPF** section
4. Drag the **OLAP Client** control to your designer

This automatically adds the required assemblies to your application.

### Step 3: Add Name to OLAP Client

Add a **Name** attribute to the OLAP Client for accessing it through code-behind:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="WpfApplication5.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <syncfusion:OlapClient Name="olapClient1" 
                               HorizontalAlignment="Stretch" 
                               VerticalAlignment="Stretch" 
                               Height="600" 
                               Width="700"/>
    </Grid>
</Window>
```

### Step 4: Configure OlapDataManager in Code-Behind

Include the required namespace:

```csharp
using Syncfusion.Olap.Manager;
```

Configure the OLAP Client with connection details:

```csharp
using Syncfusion.Olap.Manager;

namespace WpfApplication
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Replace with your actual connection string
            string connectionString = "Enter a valid connection string";
            
            // Connection string is passed to the OlapDataManager as an argument
            OlapDataManager olapDataManager = new OlapDataManager(connectionString);
            
            // OlapClient gets information from the OlapDataManager
            this.olapClient1.OlapDataManager = olapDataManager;
            this.olapClient1.DataBind();
        }
    }
}
```

**VB.NET:**

```vbnet
Imports Syncfusion.Olap.Manager

Namespace WpfApplication
    Partial Public Class MainWindow
        Inherits Window
        Public Sub New()
            InitializeComponent()
            
            Dim connectionString As String = "Enter a valid connection string"
            
            ' Connection string is passed to the OlapDataManager as an argument
            Dim olapDataManager As OlapDataManager = New OlapDataManager(connectionString)
            
            ' OlapClient gets information from the OlapDataManager
            Me.olapClient1.OlapDataManager = olapDataManager
            Me.olapClient1.DataBind()
        End Sub
    End Class
End Namespace
```

### Step 5: Run the Application

Run the application to see the OLAP Client in action.

## Setup Method 2: Through Expression Blend

### Step 1: Create WPF Application

1. Open Blend for Visual Studio
2. Navigate to **File > New project > WPF > WPF application**
3. Create the new WPF application

### Step 2: Add Assembly References

1. Select the **Project** tab (left corner of Blend IDE)
2. Right-click **References** and select **Add Reference**
3. Browse and add the following Syncfusion assemblies:
   - Syncfusion.Grid.Wpf
   - Syncfusion.Olap.Base
   - Syncfusion.Chart.Wpf
   - Syncfusion.OlapChart.Wpf
   - Syncfusion.OlapGrid.Wpf
   - Syncfusion.OlapGridCommon.Wpf
   - Syncfusion.OlapClient.Wpf
   - Syncfusion.OlapShared.Wpf
   - Syncfusion.OlapTools.Wpf

**Assembly Location:**  
`{System Drive}:\Program Files (x86)\Syncfusion\Essential Studio\<version number>\precompiledassemblies\<version number>\<framework version>\`

### Step 3: Add OLAP Client from Assets

After adding assemblies, the OLAP Client control appears under the **Assets** tab:

1. Choose the **Assets** tab
2. Drag the **OLAP Client** to the designer

### Step 4: Configure XAML

Add a name to the OLAP Client:

```xaml
<Window x:Class="WpfApplication.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Client.Olap;assembly=Syncfusion.OlapClient.WPF"
        Width="900" Height="630">
    <Grid>
        <syncfusion:OlapClient Name="olapClient1" 
                               HorizontalAlignment="Stretch" 
                               VerticalAlignment="Stretch"/>
    </Grid>
</Window>
```

### Step 5: Configure Code-Behind

Use the same code-behind configuration as shown in Method 1, Step 4.

## Setup Method 3: Through Code-Behind

This method creates the OLAP Client entirely through C# code without XAML designer.

### Step 1: Create WPF Application

1. Open Visual Studio IDE
2. Navigate to **File > New > Project**
3. Select **WPF application** (inside Visual C# Templates)

### Step 2: Add Assembly References Manually

Right-click **References** and select **Add Reference**. Add these assemblies:

- Syncfusion.Chart.WPF
- Syncfusion.Shared.WPF
- Syncfusion.Olap.Base
- Syncfusion.OlapChart.WPF
- Syncfusion.OlapChartConverter.WPF
- Syncfusion.OlapClient.WPF
- Syncfusion.OlapGrid.WPF
- Syncfusion.OlapGridCommon.WPF
- Syncfusion.OlapGridConverter.WPF
- Syncfusion.OlapSampleUtils
- Syncfusion.OlapShared.WPF
- Syncfusion.OlapTools.WPF
- Syncfusion.Tools.WPF

### Step 3: Create OLAP Client Programmatically

Include required namespaces:

```csharp
using Syncfusion.Windows.Client.Olap;
using Syncfusion.Olap.Manager;
```

Create and configure OLAP Client:

```csharp
using Syncfusion.Windows.Client.Olap;
using Syncfusion.Olap.Manager;

namespace WpfApplication
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // OlapClient instantiation
            OlapClient olapClient1 = new OlapClient();
            
            // Connection string
            string connectionString = "Enter a valid connection string";
            
            // Connection string is passed to the OlapDataManager as an argument
            OlapDataManager olapDataManager = new OlapDataManager(connectionString);
            
            // OlapClient gets information from the OlapDataManager
            olapClient1.OlapDataManager = olapDataManager;
            olapClient1.DataBind();
            
            // OlapClient added to the Main Window Grid region
            grid.Children.Add(olapClient1);
        }
    }
}
```

**VB.NET:**

```vbnet
Imports Syncfusion.Olap.Manager
Imports Syncfusion.Windows.Client.Olap

Namespace WpfApplication
    Partial Public Class MainWindow
        Inherits Window
        Public Sub New()
            InitializeComponent()
            
            ' OlapClient instantiation
            Dim olapClient1 As OlapClient = New OlapClient()
            
            Dim connectionString As String = "Enter a valid connection string"
            
            ' Connection string is passed to the OlapDataManager as an argument
            Dim olapDataManager As OlapDataManager = New OlapDataManager(connectionString)
            
            ' OlapClient gets information from the OlapDataManager
            olapClient1.OlapDataManager = olapDataManager
            olapClient1.DataBind()
            
            ' OlapClient added to the Main Window Grid region
            grid.Children.Add(olapClient1)
        End Sub
    End Class
End Namespace
```

## Required Assemblies

All three setup methods require these Syncfusion assemblies:

| Assembly | Purpose |
|----------|---------|
| Syncfusion.Grid.Wpf | Grid rendering |
| Syncfusion.Olap.Base | OLAP core functionality |
| Syncfusion.Chart.Wpf | Chart rendering |
| Syncfusion.OlapChart.Wpf | OLAP-specific charting |
| Syncfusion.OlapGrid.Wpf | OLAP grid component |
| Syncfusion.OlapGridCommon.Wpf | OLAP grid common utilities |
| Syncfusion.OlapClient.Wpf | Main OLAP Client control |
| Syncfusion.OlapShared.Wpf | Shared OLAP utilities |
| Syncfusion.OlapTools.Wpf | OLAP tools and editors |

## Required Namespaces

Include these namespaces in your code files:

```csharp
using Syncfusion.Olap.Manager;           // For OlapDataManager
using Syncfusion.Windows.Client.Olap;    // For OlapClient control
```

## Troubleshooting

### OLAP Client Not Appearing in Toolbox

**Solution:** Ensure you have installed Syncfusion Essential Studio for WPF. The OLAP Client appears under the **Syncfusion BI WPF** category.

### Assembly Reference Errors

**Solution:** Verify all required assemblies are referenced and that they match your project's target framework version.

### Connection String Errors

**Solution:** Replace "Enter a valid connection string" with an actual connection string. See the data-binding.md reference for connection string examples for different data sources.

### DataBind() Not Loading Data

**Solution:** Ensure:
1. Connection string is valid
2. OlapDataManager is properly assigned to OlapClient.OlapDataManager
3. DataBind() is called after setting the OlapDataManager
4. The target cube/database is accessible

### License Key Errors

**Solution:** For v16.2.0.x and later, register your Syncfusion license key before using the control. See the [licensing documentation](https://help.syncfusion.com/common/essential-studio/licensing/overview).

## Next Steps

Now that you have a basic OLAP Client setup:

1. Configure a proper connection string - see [data-binding.md](data-binding.md)
2. Learn about OLAP Client UI elements - see [olap-client-elements.md](olap-client-elements.md)
3. Explore filtering capabilities - see [filtering.md](filtering.md)
4. Learn about report management - see [report-management.md](report-management.md)

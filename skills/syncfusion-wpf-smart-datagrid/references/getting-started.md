# Getting Started with WPF Smart DataGrid

## Table of Contents
- [Overview](#overview)
- [Assembly Deployment](#assembly-deployment)
- [Creating a Simple Application](#creating-a-simple-application)
- [Adding the Control](#adding-the-control)
- [Installation and Setup Steps](#installation-and-setup-steps)

## Overview

This guide provides a quick overview for working with the WPF SfSmartDataGrid (SfSmartDataGrid) control. The Smart DataGrid is an AI-assisted data grid that enhances how users interact with data through natural language-driven operations.

The SfSmartDataGrid enables:
- Natural language sorting, filtering, grouping, and highlighting
- Context-aware data operations
- Intuitive user interactions without manual configuration
- AI-powered data exploration

## Assembly Deployment

Before using the SfSmartDataGrid control, you need to add the required assemblies as references to your project.

### Required Assemblies

| Assembly | Description |
|----------|-------------|
| **Syncfusion.Data.WPF** | Contains fundamental and base classes for `CollectionViewAdv` which is responsible for data processing operations handled in SfSmartDataGrid. |
| **Syncfusion.SfGrid.WPF** | Contains the core grid engine and UI components for the WPF data grid. Provides the `Syncfusion.UI.Xaml.Grid` namespace and implements essential features such as virtualization, row/column generation, selection, editing, sorting, filtering, grouping, and performance optimizations for large data sets. |
| **Syncfusion.SfSmartComponents.WPF** | Contains the Smart extensions and higher-level controls that enable AI-assisted and smart behaviors. Exposes the `Syncfusion.UI.Xaml.SmartComponents` namespace and includes `SfSmartDataGrid` with smart features, suggestions, and natural-language action hooks for AI-driven sorting, filtering, grouping, and highlighting. |
| **Syncfusion.SfChat.WPF** | Contains UI controls and services for building chat and messaging experiences. Provides chat windows, message templates, typing indicators, message grouping, and utilities for loading and persisting message history. Useful for integrating conversational interfaces alongside data-grid scenarios. |
| **Syncfusion.Shared.WPF** | Contains various editor controls (such as IntegerTextBox, DoubleTextBox, etc.) which are used in SfSmartDataGrid. |

### Key Namespaces

When working with SfSmartDataGrid, you'll primarily use:
- `Syncfusion.UI.Xaml.SmartComponents` – Smart DataGrid control
- `Syncfusion.UI.Xaml.Grid` – Core grid features and column types

## Creating a Simple Application

Follow these steps to create a WPF application with SfSmartDataGrid:

1. Create a new WPF project
2. Install required Syncfusion NuGet packages
3. Add the control via Designer, XAML, or C#
4. Create the data model
5. Bind data
6. Register the AI service
7. Run the application

### Step 1: Create a New WPF Project

Create a new WPF Project in Visual Studio to display SfSmartDataGrid with data objects.

**In Visual Studio:**
1. File → New → Project
2. Select "WPF App (.NET)" or "WPF Application"
3. Name your project (e.g., "SmartDataGridDemo")
4. Click Create

### Step 2: Install the Syncfusion WPF NuGet Packages

Install the required NuGet package to get all necessary assemblies:

**Steps:**
1. In **Solution Explorer**, right-click the project and choose **Manage NuGet Packages**
2. Search for `Syncfusion.SfSmartComponents.WPF`
3. Install the latest version (use `*` for the version if no specific version is mentioned)
4. Ensure the necessary dependencies are installed correctly and the project is restored

**Package Manager Console:**
```powershell
# Install latest version (default - use when no specific version is mentioned)
Install-Package Syncfusion.SfSmartComponents.WPF

# Or explicitly specify latest version using wildcard
Install-Package Syncfusion.SfSmartComponents.WPF -Version *

# Or install a specific version if customer specifies one
Install-Package Syncfusion.SfSmartComponents.WPF -Version 33.1.44
```

> **Note:** When no specific version is mentioned by the customer, always use the latest version (`*`). This ensures you get the most recent features, bug fixes, and improvements.

This package includes all required dependencies:
- Syncfusion.Data.WPF
- Syncfusion.SfGrid.WPF
- Syncfusion.SfChat.WPF
- Syncfusion.Shared.WPF

## Adding the Control

There are three ways to add the SfSmartDataGrid control to your application:

### Option 1: Adding Control via Designer

The SfSmartDataGrid control can be added by dragging it from the Toolbox and dropping it in the Designer view. The required assembly references will be added automatically.

**Steps:**
1. Open MainWindow.xaml in Designer view
2. Locate the Toolbox (View → Toolbox if not visible)
3. Find "SfSmartDataGrid" in the Syncfusion controls section
4. Drag and drop it onto the design surface

### Option 2: Adding Control Manually in XAML

To add the control manually in XAML:

**Steps:**

1. **Add assembly references** (if not already added):
   - Syncfusion.Data.WPF
   - Syncfusion.SfGrid.WPF
   - Syncfusion.Shared.WPF
   - Syncfusion.SfChat.WPF
   - Syncfusion.SfSmartComponents.WPF

2. **Import the Syncfusion WPF schema** or namespace in XAML:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

Or use the full namespace:
```xml
xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.SmartComponents;assembly=Syncfusion.SfSmartComponents.WPF"
```

3. **Declare the SfSmartDataGrid control**:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="WpfApplication1.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <syncfusion:SfSmartDataGrid x:Name="smartDataGrid"/>
    </Grid>
</Window>
```

### Option 3: Adding Control Manually in C#

To add the control manually in C#:

**Steps:**

1. **Add assembly references** (same as XAML approach)

2. **Import the namespace** in your code-behind:

```csharp
using Syncfusion.UI.Xaml.SmartComponents;
```

3. **Create and add the control instance**:

```csharp
using Syncfusion.UI.Xaml.SmartComponents;

namespace WpfApplication1
{ 
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create the Smart DataGrid
            SfSmartDataGrid smartDataGrid = new SfSmartDataGrid();
            
            // Add to the root Grid (assuming it's named Root_Grid)
            Root_Grid.Children.Add(smartDataGrid);
        }
    }
}
```

## Installation and Setup Steps

### Complete Setup Checklist

After installing the NuGet package and adding the control, verify:

1. **Assembly References Added:**
   - ✓ Syncfusion.Data.WPF
   - ✓ Syncfusion.SfGrid.WPF
   - ✓ Syncfusion.SfSmartComponents.WPF
   - ✓ Syncfusion.SfChat.WPF
   - ✓ Syncfusion.Shared.WPF

2. **Namespace Imported in XAML:**
   ```xml
   xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
   ```

3. **Control Declared:**
   ```xml
   <syncfusion:SfSmartDataGrid x:Name="smartDataGrid"/>
   ```

4. **Project Builds Successfully** – No assembly reference errors

### Next Steps

After adding the control:
1. **Create the data model** – Define classes for your data (see data-model-binding.md)
2. **Bind data** – Set ItemsSource property
3. **Register AI service** – Configure Azure OpenAI integration (see ai-service-setup.md)
4. **Configure columns** – Define GridTextColumn, GridNumericColumn, etc.
5. **Enable smart actions** – Set `EnableSmartActions="True"`

## Common Issues and Solutions

### Issue 1: Control Not Appearing in Toolbox
**Solution:** Ensure the NuGet package is installed correctly. Rebuild the solution and restart Visual Studio if necessary.

### Issue 2: Namespace Not Found Error
**Solution:** Verify that `Syncfusion.SfSmartComponents.WPF` is installed and the correct namespace is imported:
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Issue 3: Assembly Reference Missing
**Solution:** Install the `Syncfusion.SfSmartComponents.WPF` NuGet package, which includes all required dependencies.

### Issue 4: Grid Not Displaying Data
**Solution:** Ensure:
- ItemsSource is bound to a valid data collection
- Columns are defined (either AutoGenerateColumns="True" or manual column definitions)
- Data model implements INotifyPropertyChanged for dynamic updates

## Minimal Working Example

Here's a complete minimal example showing the control added via XAML:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="WpfApplication1.MainWindow"
        Title="Smart DataGrid Demo" Height="450" Width="800">
    <Grid>
        <syncfusion:SfSmartDataGrid x:Name="smartDataGrid"
                                    AutoGenerateColumns="True"
                                    EnableSmartActions="True"/>
    </Grid>
</Window>
```

This creates a basic Smart DataGrid with auto-generated columns and smart actions enabled. The next step is to create a data model and bind data to the grid.

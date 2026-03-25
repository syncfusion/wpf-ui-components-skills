# Getting Started with WPF TreeGrid

## Table of Contents
- [Overview](#overview)
- [Assembly Requirements](#assembly-requirements)
- [Adding TreeGrid to Your Application](#adding-treegrid-to-your-application)
- [Data Binding Basics](#data-binding-basics)
- [Initial Configuration](#initial-configuration)
- [Troubleshooting](#troubleshooting)

## Overview

The WPF TreeGrid (SfTreeGrid) is a data-oriented control that displays self-relational and hierarchical data in a tree structure with columns. It combines the functionality of a tree view with the data display capabilities of a grid control.

**Use TreeGrid when you need to:**
- Display parent-child hierarchical data
- Show self-relational data (where parent-child relationships exist in the same collection)
- Present nested collections with expandable/collapsible nodes
- Load large hierarchical datasets on-demand

## Assembly Requirements

### Required Assemblies

Add these assemblies to your WPF project:

| Assembly | Description |
|----------|-------------|
| **Syncfusion.Data.WPF** | Dependent assembly for data operations |
| **Syncfusion.SfGrid.WPF** | Contains UI operations for SfTreeGrid |
| **Syncfusion.Shared.WPF** | Editor controls (IntegerTextBox, DoubleTextBox, etc.) |

The SfTreeGrid control is in the `Syncfusion.UI.Xaml.TreeGrid` namespace.

### Optional Assemblies (for Export Features)

| Assembly | Description |
|----------|-------------|
| **Syncfusion.SfGridConverter.WPF** | Export to Excel and PDF functionality |
| **Syncfusion.XlsIO.Base** | Excel file manipulation |
| **Syncfusion.Pdf.Base** | PDF creation and manipulation |

### Installation Methods

**Method 1: NuGet Package Manager**
```powershell
Install-Package Syncfusion.SfGrid.WPF
```

**Method 2: Visual Studio Designer**
- Drag SfTreeGrid from Toolbox to Designer
- Assemblies are added automatically

**Method 3: Manual Assembly References**
- Right-click project → Add Reference
- Browse to Syncfusion installation folder
- Add required assemblies

## Adding TreeGrid to Your Application

### Method 1: XAML Declaration

```xml
<Window x:Class="TreeGridSample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="TreeGrid Sample"
        Width="800"
        Height="600">
    <Grid>
        <syncfusion:SfTreeGrid x:Name="treeGrid" />
    </Grid>
</Window>
```

**Namespace Options:**
- Schema-based: `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`
- CLR namespace: `xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.TreeGrid;assembly=Syncfusion.SfGrid.WPF"`

### Method 2: Code-Behind (C#)

```csharp
using Syncfusion.UI.Xaml.TreeGrid;

namespace TreeGridSample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            SfTreeGrid treeGrid = new SfTreeGrid();
            Root_Grid.Children.Add(treeGrid);
        }
    }
}
```

### Method 3: Designer (Drag-Drop)

1. Open WPF Designer
2. Locate SfTreeGrid in Toolbox
3. Drag and drop onto design surface
4. Required assemblies are added automatically

## Data Binding Basics

TreeGrid supports two primary data binding approaches:

### 1. Self-Relational Data Binding

For collections where parent-child relationships exist within the same data structure.

#### Data Model

```csharp
public class EmployeeInfo : INotifyPropertyChanged
{
    public int ID { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Title { get; set; }
    public double? Salary { get; set; }
    public int ReportsTo { get; set; }  // References parent's ID
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    
    private void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

#### ViewModel

```csharp
public class EmployeeViewModel
{
    public ObservableCollection<EmployeeInfo> Employees { get; set; }
    
    public EmployeeViewModel()
    {
        Employees = new ObservableCollection<EmployeeInfo>
        {
            // Root nodes (ReportsTo = -1)
            new EmployeeInfo { ID = 1, FirstName = "Robert", LastName = "King", 
                              Title = "CEO", Salary = 2000000, ReportsTo = -1 },
            
            // Child nodes
            new EmployeeInfo { ID = 2, FirstName = "David", LastName = "Smith", 
                              Title = "VP of Sales", Salary = 1500000, ReportsTo = 1 },
            new EmployeeInfo { ID = 3, FirstName = "Nancy", LastName = "Davolio", 
                              Title = "Sales Manager", Salary = 900000, ReportsTo = 2 },
            new EmployeeInfo { ID = 4, FirstName = "Andrew", LastName = "Fuller", 
                              Title = "Sales Rep", Salary = 650000, ReportsTo = 2 }
        };
    }
}
```

#### XAML Binding

```xml
<Window.DataContext>
    <local:EmployeeViewModel />
</Window.DataContext>

<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding Employees}"
                       ParentPropertyName="ID"
                       ChildPropertyName="ReportsTo"
                       SelfRelationRootValue="-1"
                       AutoExpandMode="RootNodesExpanded"
                       AutoGenerateColumns="True">
</syncfusion:SfTreeGrid>
```

**Key Properties:**
- `ParentPropertyName`: Property that uniquely identifies each record (e.g., "ID")
- `ChildPropertyName`: Property that references the parent (e.g., "ReportsTo")
- `SelfRelationRootValue`: Value indicating root-level nodes (e.g., -1, null, 0)

### 2. Nested Collection Binding

For data models with explicit child collections.

#### Data Model

```csharp
public class PersonInfo : INotifyPropertyChanged
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public double Salary { get; set; }
    public bool Availability { get; set; }
    
    // Child collection
    public ObservableCollection<PersonInfo> Children { get; set; }
    
    public PersonInfo()
    {
        Children = new ObservableCollection<PersonInfo>();
    }
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
}
```

#### ViewModel

```csharp
public class PersonViewModel
{
    public ObservableCollection<PersonInfo> PersonDetails { get; set; }
    
    public PersonViewModel()
    {
        PersonDetails = new ObservableCollection<PersonInfo>();
        
        var ceo = new PersonInfo 
        { 
            FirstName = "John", 
            LastName = "Adams", 
            Salary = 2000000,
            Availability = true
        };
        
        // Add children
        ceo.Children.Add(new PersonInfo 
        { 
            FirstName = "Andrew", 
            LastName = "Fuller", 
            Salary = 1200000,
            Availability = true
        });
        
        ceo.Children.Add(new PersonInfo 
        { 
            FirstName = "Janet", 
            LastName = "Leverling", 
            Salary = 1000000,
            Availability = false
        });
        
        PersonDetails.Add(ceo);
    }
}
```

#### XAML Binding

```xml
<Window.DataContext>
    <local:PersonViewModel />
</Window.DataContext>

<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding PersonDetails}"
                       ChildPropertyName="Children"
                       AutoExpandMode="AllNodesExpanded"
                       AutoGenerateColumns="True">
</syncfusion:SfTreeGrid>
```

**Key Property:**
- `ChildPropertyName`: Property containing the child collection (e.g., "Children")

## Initial Configuration

### AutoGenerateColumns

By default, TreeGrid auto-generates columns based on data object properties.

```xml
<!-- Auto-generate columns (default) -->
<syncfusion:SfTreeGrid AutoGenerateColumns="True"
                       ItemsSource="{Binding Employees}"/>
```

To manually define columns:

```xml
<syncfusion:SfTreeGrid AutoGenerateColumns="False"
                       ItemsSource="{Binding Employees}">
    <syncfusion:SfTreeGrid.Columns>
        <syncfusion:TreeGridTextColumn HeaderText="First Name" 
                                       MappingName="FirstName" 
                                       Width="150"/>
        <syncfusion:TreeGridTextColumn HeaderText="Last Name" 
                                       MappingName="LastName" 
                                       Width="150"/>
        <syncfusion:TreeGridNumericColumn HeaderText="Salary" 
                                          MappingName="Salary"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

### Auto-Expand Modes

Control initial node expansion:

```xml
<!-- Expand only root nodes -->
<syncfusion:SfTreeGrid AutoExpandMode="RootNodesExpanded" />

<!-- Expand all nodes -->
<syncfusion:SfTreeGrid AutoExpandMode="AllNodesExpanded" />

<!-- All nodes collapsed (default) -->
<syncfusion:SfTreeGrid AutoExpandMode="None" />
```

### Basic Feature Enablement

```xml
<syncfusion:SfTreeGrid ItemsSource="{Binding Employees}"
                       ChildPropertyName="Children"
                       AllowSorting="True"
                       AllowFiltering="True"
                       AllowEditing="True"
                       SelectionMode="Single">
</syncfusion:SfTreeGrid>
```

## Troubleshooting

### Issue: Tree structure not forming

**Symptoms:** All records appear at root level, no hierarchy

**Solutions:**
1. **Self-relational binding:** Verify `ParentPropertyName` and `ChildPropertyName` are set correctly
   ```xml
   <syncfusion:SfTreeGrid ParentPropertyName="ID" 
                          ChildPropertyName="ReportsTo"/>
   ```

2. **Check SelfRelationRootValue:** Ensure it matches root node indicator
   ```xml
   <!-- If root nodes have ReportsTo = -1 -->
   <syncfusion:SfTreeGrid SelfRelationRootValue="-1"/>
   
   <!-- If root nodes have ReportsTo = null -->
   <syncfusion:SfTreeGrid SelfRelationRootValue="{x:Null}"/>
   ```

3. **Nested collections:** Verify `ChildPropertyName` matches collection property name
   ```csharp
   // Property name must match
   public ObservableCollection<Employee> Children { get; set; }
   ```
   ```xml
   <syncfusion:SfTreeGrid ChildPropertyName="Children"/>
   ```

### Issue: No data displaying

**Symptoms:** Empty TreeGrid, no rows

**Solutions:**
1. Verify `ItemsSource` binding
2. Check DataContext is set
3. Ensure collection is not empty
4. Verify namespace imports

```xml
<!-- Correct binding -->
<Window.DataContext>
    <local:ViewModel />
</Window.DataContext>

<syncfusion:SfTreeGrid ItemsSource="{Binding Employees}"/>
```

### Issue: Changes not reflecting in UI

**Symptoms:** Data updates don't appear in TreeGrid

**Solutions:**
1. Use `ObservableCollection<T>` for ItemsSource
2. Implement `INotifyPropertyChanged` in data models
3. Raise `PropertyChanged` events on property updates

```csharp
public class Employee : INotifyPropertyChanged
{
    private string _firstName;
    
    public string FirstName
    {
        get => _firstName;
        set
        {
            if (_firstName != value)
            {
                _firstName = value;
                OnPropertyChanged(nameof(FirstName));
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Issue: Assemblies not found

**Symptoms:** "Type 'SfTreeGrid' not found" error

**Solutions:**
1. Verify all required assemblies are referenced
2. Check assembly version matches installed Syncfusion version
3. Clean and rebuild solution
4. Verify namespace import in XAML:
   ```xml
   xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
   ```

### Issue: License key errors

**Symptoms:** "License key not registered" or watermark appearing

**Solution:** Register license key in App.xaml.cs:
```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
        base.OnStartup(e);
    }
}
```

---

**Next Steps:**
- For detailed data binding scenarios, read `data-binding.md`
- For load-on-demand implementation, read `load-on-demand.md`
- For column configuration, read `columns.md`

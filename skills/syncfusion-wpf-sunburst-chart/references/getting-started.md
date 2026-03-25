# Getting Started with Sunburst Chart

Complete guide for setting up and configuring the Syncfusion WPF Sunburst Chart (SfSunburstChart) control in your WPF application. This reference covers installation, assembly references, data binding, and creating your first working sunburst chart.

## Required Dependencies

The SfSunburstChart requires:
- **Syncfusion.SfSunburstChart.WPF** - Main sunburst chart assembly
- **.NET Framework 4.5 or higher** - Minimum framework version

### XAML Namespace

Add the sunburst namespace to your XAML Window or UserControl:

```xml
<Window x:Class="SunburstDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:sunburst="clr-namespace:Syncfusion.UI.Xaml.SunburstChart;assembly=Syncfusion.SfSunburstChart.WPF"
        Title="Sunburst Chart Demo" Height="600" Width="800">
    <!-- Chart content here -->
</Window>
```

### C# Using Statements

Add these using directives to your code-behind:

```csharp
using Syncfusion.UI.Xaml.SunburstChart;
```

## Data Model Structure

Sunburst charts require hierarchical data. Create a model class with properties for each hierarchy level and a numeric value.

### Example: Employee Distribution Model

```csharp
public class EmployeeModel
{
    public string Country { get; set; }
    public string JobDescription { get; set; }
    public string JobGroup { get; set; }
    public string JobRole { get; set; }
    public double EmployeesCount { get; set; }
}
```

**Key Points:**
- Each hierarchy level needs a string property (Country, JobDescription, etc.)
- Include a numeric property for segment sizing (EmployeesCount)
- Properties can be nullable for optional levels
- Use meaningful names that reflect your data structure

## ViewModel Creation

Create a ViewModel with an observable collection of your data model:

```csharp
public class ViewModel
{
    public ObservableCollection<EmployeeModel> Data { get; set; }
    
    public ViewModel()
    {
        Data = new ObservableCollection<EmployeeModel>
        {
            new EmployeeModel
            {
                Country = "America", 
                JobDescription = "Sales",
                EmployeesCount = 70
            },
            new EmployeeModel
            {
                Country = "America", 
                JobDescription = "Technical",
                JobGroup = "Testers", 
                EmployeesCount = 35
            },
            new EmployeeModel
            {
                Country = "America", 
                JobDescription = "Technical",
                JobGroup = "Developers", 
                JobRole = "Windows", 
                EmployeesCount = 105
            },
            new EmployeeModel
            {
                Country = "America", 
                JobDescription = "Technical",
                JobGroup = "Developers", 
                JobRole = "Web", 
                EmployeesCount = 40
            },
            new EmployeeModel
            {
                Country = "America", 
                JobDescription = "Management",
                EmployeesCount = 40
            },
            new EmployeeModel
            {
                Country = "America", 
                JobDescription = "Accounts",
                EmployeesCount = 60
            },
            new EmployeeModel
            {
                Country = "India", 
                JobDescription = "Technical",
                JobGroup = "Testers", 
                EmployeesCount = 25
            },
            new EmployeeModel
            {
                Country = "India", 
                JobDescription = "Technical", 
                JobGroup = "Developers",
                JobRole = "Windows", 
                EmployeesCount = 155
            },
            new EmployeeModel
            {
                Country = "India", 
                JobDescription = "Technical", 
                JobGroup = "Developers",
                JobRole = "Web", 
                EmployeesCount = 60
            },
            new EmployeeModel
            {
                Country = "Germany", 
                JobDescription = "Sales", 
                JobGroup = "Executive",
                EmployeesCount = 30
            },
            new EmployeeModel
            {
                Country = "Germany", 
                JobDescription = "Sales", 
                JobGroup = "Analyst",
                EmployeesCount = 40
            },
            new EmployeeModel
            {
                Country = "UK", 
                JobDescription = "Technical", 
                JobGroup = "Developers",
                JobRole = "Windows", 
                EmployeesCount = 100
            },
            new EmployeeModel
            {
                Country = "UK", 
                JobDescription = "Technical", 
                JobGroup = "Developers",
                JobRole = "Web", 
                EmployeesCount = 30
            },
            new EmployeeModel
            {
                Country = "UK", 
                JobDescription = "HR Executives", 
                EmployeesCount = 60
            },
            new EmployeeModel
            {
                Country = "UK", 
                JobDescription = "Marketing", 
                EmployeesCount = 40
            }
        };
    }
}
```

**Important:** Notice that not all records need all hierarchy levels. Records can have 2, 3, or 4 levels depending on the data structure.

## Basic Chart Configuration

### XAML Implementation

```xml
<Window x:Class="SunburstDemo.MainWindow"
        xmlns:sunburst="clr-namespace:Syncfusion.UI.Xaml.SunburstChart;assembly=Syncfusion.SfSunburstChart.WPF">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <Grid>
        <sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                                  ValueMemberPath="EmployeesCount">
            
            <sunburst:SfSunburstChart.Levels>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobDescription"/>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobGroup"/>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobRole"/>
            </sunburst:SfSunburstChart.Levels>
            
        </sunburst:SfSunburstChart>
    </Grid>
</Window>
```

### C# Code-Behind Implementation

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Set DataContext
        this.DataContext = new ViewModel();
        
        // Create chart programmatically (optional)
        SfSunburstChart sunburst = new SfSunburstChart();
        sunburst.ValueMemberPath = "EmployeesCount";
        sunburst.SetBinding(SfSunburstChart.ItemsSourceProperty, "Data");
        
        // Add hierarchy levels
        sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "Country" });
        sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobDescription" });
        sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobGroup" });
        sunburst.Levels.Add(new SunburstHierarchicalLevel() { GroupMemberPath = "JobRole" });
        
        // Add to visual tree
        this.Content = sunburst;
    }
}
```

**Key Properties:**
- **ItemsSource** – Binds to your data collection (ViewModel.Data)
- **ValueMemberPath** – Property name for segment sizing ("EmployeesCount")
- **Levels** – Collection defining each hierarchy level
- **GroupMemberPath** – Property name for each level's grouping

## Adding Visual Elements

### Adding a Header

Display a title above the chart:

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="EmployeesCount"
                          Header="Employees Count" 
                          FontSize="22">
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
sunburst.Header = "Employees Count";
sunburst.FontSize = 22;
```

### Adding a Legend

Enable legend to show first-level categories:

**XAML:**
```xml
<sunburst:SfSunburstChart.Legend>
    <sunburst:SunburstLegend DockPosition="Left"/>
</sunburst:SfSunburstChart.Legend>
```

**C#:**
```csharp
SunburstLegend legend = new SunburstLegend();
legend.DockPosition = ChartDock.Left;
sunburst.Legend = legend;
```

**DockPosition options:**
- `Left` – Legend on left side
- `Right` – Legend on right side
- `Top` – Legend above chart
- `Bottom` – Legend below chart

### Adding Data Labels

Display category names and values on segments:

**XAML:**
```xml
<sunburst:SfSunburstChart.DataLabelInfo>
    <sunburst:SunburstDataLabelInfo ShowLabel="True"/>
</sunburst:SfSunburstChart.DataLabelInfo>
```

**C#:**
```csharp
SunburstDataLabelInfo dataLabel = new SunburstDataLabelInfo();
dataLabel.ShowLabel = true;
sunburst.DataLabelInfo = dataLabel;
```

## Complete Working Example

Here's a full XAML example combining all elements:

```xml
<Window x:Class="SunburstDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:SunburstDemo"
        xmlns:sunburst="clr-namespace:Syncfusion.UI.Xaml.SunburstChart;assembly=Syncfusion.SfSunburstChart.WPF"
        Title="Sunburst Chart Demo" Height="600" Width="800">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <Grid>
        <sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                                  ValueMemberPath="EmployeesCount" 
                                  Header="Employees Count" 
                                  FontSize="22">
            
            <!-- Legend -->
            <sunburst:SfSunburstChart.Legend>
                <sunburst:SunburstLegend DockPosition="Left"/>
            </sunburst:SfSunburstChart.Legend>
            
            <!-- Hierarchy Levels -->
            <sunburst:SfSunburstChart.Levels>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobDescription"/>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobGroup"/>
                <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobRole"/>
            </sunburst:SfSunburstChart.Levels>
            
            <!-- Data Labels -->
            <sunburst:SfSunburstChart.DataLabelInfo>
                <sunburst:SunburstDataLabelInfo ShowLabel="True"/>
            </sunburst:SfSunburstChart.DataLabelInfo>
            
        </sunburst:SfSunburstChart>
    </Grid>
</Window>
```

And the corresponding C# code-behind:

```csharp
using System.Collections.ObjectModel;
using System.Windows;
using Syncfusion.UI.Xaml.SunburstChart;

namespace SunburstDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            // DataContext set in XAML
        }
    }
    
    public class EmployeeModel
    {
        public string Country { get; set; }
        public string JobDescription { get; set; }
        public string JobGroup { get; set; }
        public string JobRole { get; set; }
        public double EmployeesCount { get; set; }
    }
    
    public class ViewModel
    {
        public ObservableCollection<EmployeeModel> Data { get; set; }
        
        public ViewModel()
        {
            // Initialize data as shown earlier
            Data = new ObservableCollection<EmployeeModel>
            {
                // Add all employee records here...
            };
        }
    }
}
```

## Theme Integration

Apply consistent styling using Syncfusion's theme support:

### Using SfSkinManager

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    
    <syncfusionskin:SfSkinManager.Theme>
        <syncfusionskin:Theme ThemeName="MaterialLight"/>
    </syncfusionskin:SfSkinManager.Theme>
    
    <!-- Your sunburst chart here -->
</Window>
```

**Available Themes:**
- MaterialLight / MaterialDark
- Office2019Colorful / Office2019Black
- FluentLight / FluentDark
- And more...

### Theme Studio

Create custom themes using Syncfusion's Theme Studio tool for brand-specific color schemes.

## Troubleshooting

**Chart not appearing:**
- Verify assembly reference is added correctly
- Check namespace declaration in XAML
- Ensure DataContext is set (ViewModel assigned)
- Confirm ItemsSource binding path is correct

**No data displaying:**
- Verify Data collection is not empty
- Check property names match exactly (case-sensitive)
- Ensure ValueMemberPath points to a numeric property
- Confirm at least one level is defined

**Binding errors:**
- Check Output window for binding errors
- Verify ViewModel is public and accessible
- Ensure properties have public getters
- Use correct binding syntax: `{Binding Data}`

## Next Steps

Now that you have a basic sunburst chart working:

1. **Customize hierarchy levels** → Read [hierarchical-levels.md](hierarchical-levels.md)
2. **Style data labels** → Read [data-labels.md](data-labels.md)
3. **Configure legend** → Read [legend.md](legend.md)
4. **Add animations** → Read [animation.md](animation.md)
5. **Apply color palettes** → Read [visual-customization.md](visual-customization.md)
6. **Enable interactivity** → Read [interactivity.md](interactivity.md)
7. **Handle events** → Read [events.md](events.md)

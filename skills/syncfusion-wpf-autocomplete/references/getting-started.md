# Getting Started with WPF Autocomplete (SfTextBoxExt)

Complete guide for setting up and creating your first WPF Autocomplete control implementation with SfTextBoxExt.

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Creating a Simple Application](#creating-a-simple-application)
- [Populating AutoComplete with Data](#populating-autocomplete-with-data)
- [AutoComplete Modes](#autocomplete-modes)
- [Basic Selection](#basic-selection)
- [Theme Support](#theme-support)

## Assembly Deployment

### Required Assemblies

To use the SfTextBoxExt control in your WPF application, add references to the following assemblies:

- **Syncfusion.SfInput.WPF**
- **Syncfusion.SfShared.WPF**

### NuGet Package Installation

Install the Syncfusion WPF AutoComplete package via NuGet:

```powershell
Install-Package Syncfusion.SfInput.WPF
```

Or use the NuGet Package Manager UI in Visual Studio:
1. Right-click on your project → Manage NuGet Packages
2. Search for "Syncfusion.SfInput.WPF"
3. Click Install

For detailed NuGet installation instructions, refer to the [Syncfusion NuGet documentation](https://help.syncfusion.com/wpf/welcome-to-syncfusion-essential-wpf).

### Control Dependencies

For a complete list of assemblies and dependencies, refer to the [Control Dependencies documentation](https://help.syncfusion.com/wpf/control-dependencies#sftextboxext).

## Creating a Simple Application

### Step 1: Create a WPF Project

1. Open Visual Studio
2. Create a new WPF Application (.NET Framework or .NET Core/.NET 5+)
3. Name your project (e.g., "WpfAutoCompleteApp")

### Step 2: Add Control Using Designer

The easiest way to add SfTextBoxExt is through the Visual Studio designer:

1. Build your project to load Syncfusion controls in the toolbox
2. Open MainWindow.xaml in designer view
3. Drag **SfTextBoxExt** from the toolbox onto your window
4. Required assembly references will be added automatically

![Designer View](../../../assets/autocomplete-designer.png)

### Step 3: Add Control Manually in XAML

**Method 1: Using Syncfusion Schema**

```xaml
<Window x:Class="WpfAutoCompleteApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="http://schemas.syncfusion.com/wpf"
        Title="AutoComplete Demo" Height="450" Width="800">
    
    <editors:SfTextBoxExt 
        HorizontalAlignment="Center" 
        VerticalAlignment="Center" 
        HorizontalContentAlignment="Center"
        Height="40"
        Width="300" 
        Text="Hello! World..."/>
</Window>
```

**Method 2: Using Full Namespace**

```xaml
<Window x:Class="WpfAutoCompleteApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.WPF"
        Title="AutoComplete Demo" Height="450" Width="800">
    
    <syncfusion:SfTextBoxExt 
        HorizontalAlignment="Center" 
        VerticalAlignment="Center"
        Width="300"
        Height="40"
        Text="Hello! World..."/>
</Window>
```

### Step 4: Add Control Manually in C#

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace WpfAutoCompleteApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create SfTextBoxExt instance
            SfTextBoxExt textBoxExt = new SfTextBoxExt
            {
                HorizontalContentAlignment = HorizontalAlignment.Center,
                HorizontalAlignment = HorizontalAlignment.Center,
                VerticalAlignment = VerticalAlignment.Center,
                Width = 300,
                Height = 40,
                Text = "Hello! World..."
            };
            
            // Add to window
            this.Content = textBoxExt;
        }
    }
}
```

## Populating AutoComplete with Data

AutoComplete is a data-bound control that displays suggestions based on a data source. Follow these steps to bind data:

### Step 1: Create Data Model

Create a class to represent your data:

```csharp
public class Employee
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string Department { get; set; }
}
```

### Step 2: Create ViewModel

Create a ViewModel class with your data collection:

```csharp
using System.Collections.Generic;

public class EmployeeViewModel
{
    public List<Employee> Employees { get; set; }

    public EmployeeViewModel()
    {
        Employees = new List<Employee>
        {
            new Employee { Name = "Eric", Email = "Eric@syncfusion.com", Department = "Engineering" },
            new Employee { Name = "James", Email = "James@syncfusion.com", Department = "Sales" },
            new Employee { Name = "Jacob", Email = "Jacob@syncfusion.com", Department = "Marketing" },
            new Employee { Name = "Lucas", Email = "Lucas@syncfusion.com", Department = "Engineering" },
            new Employee { Name = "Mark", Email = "Mark@syncfusion.com", Department = "Support" },
            new Employee { Name = "Aldan", Email = "Aldan@syncfusion.com", Department = "Engineering" },
            new Employee { Name = "Aldrin", Email = "Aldrin@syncfusion.com", Department = "Sales" },
            new Employee { Name = "Alan", Email = "Alan@syncfusion.com", Department = "Marketing" },
            new Employee { Name = "Aaron", Email = "Aaron@syncfusion.com", Department = "Support" }
        };
    }
}
```

### Step 3: Bind Data in XAML

```xaml
<Window x:Class="WpfAutoCompleteApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:WpfAutoCompleteApp"
        Title="AutoComplete Demo" Height="450" Width="800">

    <Window.DataContext>
        <local:EmployeeViewModel/>
    </Window.DataContext>
    
    <editors:SfTextBoxExt 
        HorizontalAlignment="Center" 
        VerticalAlignment="Center" 
        Width="300"
        Height="40"
        SearchItemPath="Name"
        AutoCompleteMode="Suggest"
        AutoCompleteSource="{Binding Employees}" />
</Window>
```

**Key Properties:**
- `AutoCompleteSource`: Binds to your data collection
- `SearchItemPath`: Specifies which property to filter on ("Name" in this case)
- `AutoCompleteMode`: Determines how suggestions are displayed

### Step 4: Bind Data in C#

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Create ViewModel
    EmployeeViewModel viewModel = new EmployeeViewModel();
    this.DataContext = viewModel;
    
    // Create and configure SfTextBoxExt
    SfTextBoxExt textBoxExt = new SfTextBoxExt
    {
        HorizontalAlignment = HorizontalAlignment.Center,
        VerticalAlignment = VerticalAlignment.Center,
        Width = 300,
        Height = 40,
        SearchItemPath = "Name",
        AutoCompleteMode = AutoCompleteMode.Suggest,
        AutoCompleteSource = viewModel.Employees
    };
    
    this.Content = textBoxExt;
}
```

### Simple String Collection

For simple scenarios, you can bind to a string collection:

```csharp
public class SimpleViewModel
{
    public List<string> Countries { get; set; }

    public SimpleViewModel()
    {
        Countries = new List<string>
        {
            "United States", "United Kingdom", "Canada", 
            "Australia", "Germany", "France", "Italy", "Spain"
        };
    }
}
```

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Countries}" />
```

## AutoComplete Modes

SfTextBoxExt supports four AutoComplete modes that determine how suggestions are displayed:

### 1. Suggest Mode

Displays suggestions in a dropdown list.

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}" />
```

### 2. Append Mode

Automatically appends the first matching suggestion to the entered text.

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Append"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}" />
```

### 3. SuggestAppend Mode

Combines both Suggest and Append modes - shows dropdown and appends first match.

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="SuggestAppend"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}" />
```

### 4. None Mode

Disables AutoComplete functionality. Use this when you want only the TextBox features without suggestions.

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="None"
    Text="Regular TextBox" />
```

### Mode Comparison Table

| Mode | Dropdown Display | Text Append | Use Case |
|------|-----------------|-------------|-----------|
| **Suggest** | ✓ | ✗ | Standard autocomplete with visible suggestions |
| **Append** | ✗ | ✓ | Quick entry with auto-completion |
| **SuggestAppend** | ✓ | ✓ | Maximum assistance for users |
| **None** | ✗ | ✗ | Regular textbox behavior |

**Note:** The default value is `None`, so you must explicitly set `AutoCompleteMode` to enable suggestions.

### Setting Mode in Code

```csharp
// Suggest mode
textBoxExt.AutoCompleteMode = AutoCompleteMode.Suggest;

// SuggestAppend mode
textBoxExt.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
```

## Basic Selection

### Single Selection (Default)

By default, SfTextBoxExt operates in single-selection mode:

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}" />
```

**Retrieving Selected Item:**

```csharp
// Get selected item
var selectedEmployee = textBoxExt.SelectedItem as Employee;
if (selectedEmployee != null)
{
    MessageBox.Show($"Selected: {selectedEmployee.Name}");
}
```

### Selection Properties

| Property | Type | Description |
|----------|------|-------------|
| `SelectedItem` | `object` | Currently selected item |
| `SelectedValue` | `object` | Value of selected item (use with ValueMemberPath) |
| `SuggestionIndex` | `int` | Index of selected item in suggestions |

### Retrieving Selected Value

Use `ValueMemberPath` to specify which property value to retrieve:

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    ValueMemberPath="Email"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
// Get selected email
string selectedEmail = textBoxExt.SelectedValue?.ToString();
```

### Multi-Selection

Enable multi-selection with `MultiSelectMode`:

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    MultiSelectMode="Token"
    AutoCompleteSource="{Binding Employees}" />
```

**For detailed multi-selection information, see:** [references/selection.md](selection.md)

## Theme Support

SfTextBoxExt supports Syncfusion's built-in themes for consistent UI styling.

### Applying Theme with SfSkinManager

```csharp
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    
    // Apply Material Light theme
    SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
}
```

### Available Themes

- **MaterialLight** / **MaterialDark**
- **FluentLight** / **FluentDark**
- **Office2019Colorful** / **Office2019Black** / **Office2019White**
- **Office2016Colorful** / **Office2016White** / **Office2016DarkGray**
- **VisualStudio2015**
- **VisualStudio2013**

### Applying Theme in XAML

```xaml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
    
    <editors:SfTextBoxExt Width="300" Height="40" />
</Window>
```

### Creating Custom Themes

Use Syncfusion ThemeStudio to create custom themes:
1. Visit https://help.syncfusion.com/wpf/themes/theme-studio
2. Customize colors and styles
3. Export and apply to your application

## Complete Example

Here's a complete working example combining all concepts:

```xaml
<Window x:Class="WpfAutoCompleteApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:WpfAutoCompleteApp"
        Title="Employee Search" Height="450" Width="600">

    <Window.DataContext>
        <local:EmployeeViewModel/>
    </Window.DataContext>
    
    <StackPanel Margin="20" VerticalAlignment="Center">
        <TextBlock Text="Search Employees:" FontSize="16" Margin="0,0,0,10"/>
        
        <editors:SfTextBoxExt 
            Width="400"
            Height="40"
            Watermark="Type employee name..."
            SearchItemPath="Name"
            ValueMemberPath="Email"
            AutoCompleteMode="Suggest"
            SuggestionMode="Contains"
            IgnoreCase="True"
            AutoCompleteSource="{Binding Employees}"
            SelectedItemChanged="OnEmployeeSelected" />
        
        <TextBlock x:Name="ResultText" 
                   FontSize="14" 
                   Margin="0,20,0,0" 
                   TextWrapping="Wrap"/>
    </StackPanel>
</Window>
```

```csharp
using System.Windows;
using Syncfusion.Windows.Controls.Input;

namespace WpfAutoCompleteApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void OnEmployeeSelected(object sender, SelectedItemChangedEventArgs e)
        {
            var textBox = sender as SfTextBoxExt;
            var employee = textBox?.SelectedItem as Employee;
            
            if (employee != null)
            {
                ResultText.Text = $"Selected: {employee.Name}\n" +
                                 $"Email: {employee.Email}\n" +
                                 $"Department: {employee.Department}";
            }
        }
    }
}
```

## Next Steps

- **Advanced Filtering**: Learn about 18+ filter modes → [autocomplete-filtering.md](autocomplete-filtering.md)
- **Multi-Selection**: Implement token and delimiter modes → [selection.md](selection.md)
- **Dropdown Customization**: Style and configure the suggestion popup → [dropdown-customization.md](dropdown-customization.md)
- **TextBox Customization**: Add watermarks and visual enhancements → [textbox-customization.md](textbox-customization.md)
- **Advanced Features**: Diacritic search, highlighting, custom filters → [advanced-features.md](advanced-features.md)

## Resources

- **GitHub Sample**: https://github.com/SyncfusionExamples/wpf-textboxext-examples/tree/master/Samples/Getting-Started
- **API Documentation**: https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Controls.Input.SfTextBoxExt.html
- **Online Documentation**: https://help.syncfusion.com/wpf/autocomplete/getting-started

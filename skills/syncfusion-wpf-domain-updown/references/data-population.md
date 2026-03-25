# Data Population and Binding

Comprehensive guide to populating the SfDomainUpDown control with data using ItemsSource, custom templates, and MVVM patterns.

## Overview

The `SfDomainUpDown` control can be populated with various types of data - from simple strings to complex objects. This guide covers all approaches to data population, from basic lists to advanced data binding with custom display templates.

## Basic Data Population

### Using Simple String Lists

The simplest way to populate the control is with a list of strings.

**XAML (Inline Array):**
```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown" Value="James">
    <syncfusion:SfDomainUpDown.ItemsSource>
        <x:Array Type="sys:String" xmlns:sys="clr-namespace:System;assembly=mscorlib">
            <sys:String>Lucas</sys:String>
            <sys:String>James</sys:String>
            <sys:String>Jacob</sys:String>
        </x:Array>
    </syncfusion:SfDomainUpDown.ItemsSource>
</syncfusion:SfDomainUpDown>
```

**C# (List<string>):**
```csharp
domainUpDown.ItemsSource = new List<string> 
{ 
    "Lucas", 
    "James", 
    "Jacob" 
};
domainUpDown.Value = "James";
```

### Using Enum Values

Populate with enum values for type-safe selections:

```csharp
public enum Priority
{
    Low,
    Medium,
    High,
    Critical
}

// In your code
domainUpDown.ItemsSource = Enum.GetValues(typeof(Priority));
domainUpDown.Value = Priority.Medium;
```

## ItemsSource Property

The `ItemsSource` property accepts any `IEnumerable` collection, making it highly flexible for data binding.

### Supported Collection Types

- `List<T>`
- `ObservableCollection<T>` (recommended for MVVM)
- `Array`
- `IEnumerable<T>`
- Any collection implementing `IEnumerable`

### Binding to ItemsSource

**XAML:**
```xml
<syncfusion:SfDomainUpDown ItemsSource="{Binding Employees}"/>
```

**C#:**
```csharp
domainUpDown.ItemsSource = viewModel.Employees;
```

## Working with Complex Objects

### Creating a Data Model

Define a model class for your data:

```csharp
public class Employee
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string Department { get; set; }
    public string Position { get; set; }
}
```

### Creating a ViewModel

Build a ViewModel to manage your data collection:

```csharp
using System.Collections.Generic;
using System.ComponentModel;

public class ViewModel : INotifyPropertyChanged
{
    private List<Employee> employees;
    
    public List<Employee> Employees
    {
        get { return employees; }
        set 
        { 
            employees = value;
            OnPropertyChanged(nameof(Employees));
        }
    }
    
    public ViewModel()
    {
        Employees = new List<Employee>();
        PopulateEmployees();
    }
    
    private void PopulateEmployees()
    {
        Employees.Add(new Employee 
        { 
            Name = "Lucas", 
            Email = "lucas@syncfusion.com",
            Department = "Engineering",
            Position = "Developer"
        });
        
        Employees.Add(new Employee 
        { 
            Name = "James", 
            Email = "james@syncfusion.com",
            Department = "Design",
            Position = "Designer"
        });
        
        Employees.Add(new Employee 
        { 
            Name = "Jacob", 
            Email = "jacob@syncfusion.com",
            Department = "Marketing",
            Position = "Manager"
        });
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Setting DataContext

**MainWindow.xaml.cs:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.DataContext = new ViewModel();
    }
}
```

### Binding in XAML

```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown" 
                           HorizontalAlignment="Center" 
                           VerticalAlignment="Center" 
                           Width="200"
                           ItemsSource="{Binding Employees}">
</syncfusion:SfDomainUpDown>
```

**Note:** Without a `ContentTemplate`, the control displays the object's `ToString()` representation (typically the class name). To display specific properties, use a ContentTemplate.

## ContentTemplate - Custom Display

The `ContentTemplate` property allows you to define how items are displayed in the control.

### Basic Text Display

Display just the employee name:

```xml
<syncfusion:SfDomainUpDown ItemsSource="{Binding Employees}">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}"/>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

### Rich Content Display

Display multiple properties with formatting:

```xml
<syncfusion:SfDomainUpDown ItemsSource="{Binding Employees}" Width="300">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="{Binding Name}" 
                           FontWeight="Bold" 
                           Margin="0,0,10,0"/>
                <TextBlock Text="{Binding Email}" 
                           Foreground="Gray" 
                           FontStyle="Italic"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

### Template with Icons

Add visual elements like images:

```xml
<syncfusion:SfDomainUpDown ItemsSource="{Binding Employees}" Width="250">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Source="/Images/user.png" 
                       Height="24" 
                       Width="24" 
                       Margin="0,0,8,0"/>
                <TextBlock Text="{Binding Name}" 
                           VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

### Multi-Line Template

Display information across multiple lines:

```xml
<syncfusion:SfDomainUpDown ItemsSource="{Binding Employees}" Width="300" Height="60">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <StackPanel>
                <TextBlock Text="{Binding Name}" 
                           FontWeight="Bold" 
                           FontSize="14"/>
                <TextBlock Text="{Binding Position}" 
                           FontSize="11" 
                           Foreground="Gray"/>
                <TextBlock Text="{Binding Department}" 
                           FontSize="10" 
                           Foreground="DarkGray"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

## Complete MVVM Example

Here's a complete working example using MVVM pattern:

**Employee.cs:**
```csharp
public class Employee
{
    public string Name { get; set; }
    public string Email { get; set; }
}
```

**ViewModel.cs:**
```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Employee> employees;
    private Employee selectedEmployee;
    
    public ObservableCollection<Employee> Employees
    {
        get { return employees; }
        set 
        { 
            employees = value;
            OnPropertyChanged(nameof(Employees));
        }
    }
    
    public Employee SelectedEmployee
    {
        get { return selectedEmployee; }
        set 
        { 
            selectedEmployee = value;
            OnPropertyChanged(nameof(SelectedEmployee));
        }
    }
    
    public ViewModel()
    {
        Employees = new ObservableCollection<Employee>
        {
            new Employee { Name = "Lucas", Email = "lucas@syncfusion.com" },
            new Employee { Name = "James", Email = "james@syncfusion.com" },
            new Employee { Name = "Jacob", Email = "jacob@syncfusion.com" }
        };
        
        SelectedEmployee = Employees[0];
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**MainWindow.xaml:**
```xml
<Window x:Class="DomainUpDownDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:DomainUpDownDemo"
        Title="Employee Selector" Height="300" Width="400">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <Grid>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <TextBlock Text="Select Employee:" FontWeight="Bold" Margin="0,0,0,10"/>
            
            <syncfusion:SfDomainUpDown x:Name="domainUpDown"
                                       Width="250"
                                       Height="40"
                                       ItemsSource="{Binding Employees}"
                                       Value="{Binding SelectedEmployee, Mode=TwoWay}">
                <syncfusion:SfDomainUpDown.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                            <TextBlock Text="{Binding Name}" 
                                       FontWeight="Bold" 
                                       Margin="0,0,10,0"/>
                            <TextBlock Text="{Binding Email}" 
                                       Foreground="Gray"/>
                        </StackPanel>
                    </DataTemplate>
                </syncfusion:SfDomainUpDown.ContentTemplate>
            </syncfusion:SfDomainUpDown>
            
            <TextBlock Margin="0,20,0,0" TextAlignment="Center">
                <Run Text="Selected: "/>
                <Run Text="{Binding SelectedEmployee.Name, Mode=OneWay}" FontWeight="Bold"/>
            </TextBlock>
        </StackPanel>
    </Grid>
</Window>
```

## Best Practices

### 1. Use ObservableCollection for Dynamic Data

When items can be added/removed at runtime:

```csharp
public ObservableCollection<Employee> Employees { get; set; }
```

### 2. Implement INotifyPropertyChanged

Ensure UI updates when data changes:

```csharp
public class Employee : INotifyPropertyChanged
{
    private string name;
    
    public string Name
    {
        get { return name; }
        set 
        { 
            name = value;
            OnPropertyChanged(nameof(Name));
        }
    }
    
    // INotifyPropertyChanged implementation
}
```

### 3. Set Initial Value After Populating Items

```csharp
domainUpDown.ItemsSource = employees;
domainUpDown.Value = employees[0]; // Set after ItemsSource
```

### 4. Keep ContentTemplate Simple

Avoid complex layouts in templates for better performance.

## Common Scenarios

### Scenario 1: Country Selector

```csharp
public class Country
{
    public string Name { get; set; }
    public string Code { get; set; }
}

// Usage
domainUpDown.ItemsSource = new List<Country>
{
    new Country { Name = "United States", Code = "US" },
    new Country { Name = "Canada", Code = "CA" },
    new Country { Name = "United Kingdom", Code = "UK" }
};
```

### Scenario 2: Status Selector with Colors

```xml
<syncfusion:SfDomainUpDown ItemsSource="{Binding StatusList}">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Ellipse Width="10" Height="10" 
                         Fill="{Binding Color}" 
                         Margin="0,0,8,0"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

## Troubleshooting

**Issue:** Objects display as type names instead of property values  
**Solution:** Define a `ContentTemplate` to specify which properties to display

**Issue:** Value binding doesn't update ViewModel  
**Solution:** Use `Mode=TwoWay` in the Value binding

**Issue:** Collection changes don't reflect in UI  
**Solution:** Use `ObservableCollection<T>` instead of `List<T>`

**Issue:** Performance issues with large collections  
**Solution:** Consider using a different control (ComboBox) for large datasets; DomainUpDown is optimized for small-to-medium lists

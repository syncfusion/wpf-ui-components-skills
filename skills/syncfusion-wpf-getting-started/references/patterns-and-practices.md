# Pattern and Practices

## Table of Contents
- [Overview](#overview)
- [Getting Started with MVVM](#getting-started-with-mvvm)
- [MVVM Commands](#mvvm-commands)
- [CommandParameter Usage](#commandparameter-usage)
- [Command Target](#command-target)
- [DataContext and Data Binding](#datacontext-and-data-binding)
- [NotificationObject Base Class](#notificationobject-base-class)
- [Complete MVVM Example](#complete-mvvm-example)
- [Theme Management](#theme-management)
- [Performance Best Practices](#performance-best-practices)
- [Common MVVM Patterns](#common-mvvm-patterns)

## Overview

Syncfusion® Essential® WPF controls are designed to work seamlessly with the MVVM (Model-View-ViewModel) pattern. This reference covers best practices, patterns, and implementation techniques for building maintainable WPF applications with Syncfusion controls.

**Key Benefits of MVVM with Syncfusion:**
- Built-in command support for common control events
- Two-way data binding support
- INotifyPropertyChanged integration
- Testable business logic
- Clean separation of concerns

## Getting Started with MVVM

MVVM separates application into three components:
- **Model**: Data and business logic
- **View**: XAML UI definition
- **ViewModel**: Presentation logic and commands

### Basic MVVM Structure

**View (MainWindow.xaml):**
```xml
<Window x:Class="YourApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourApp">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <syncfusion:TabControlExt ItemsSource="{Binding TabCollection}">
        <syncfusion:TabControlExt.ItemTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding HeaderName}"/>
            </DataTemplate>
        </syncfusion:TabControlExt.ItemTemplate>
    </syncfusion:TabControlExt>
</Window>
```

**ViewModel:**
```csharp
public class ViewModel : NotificationObject
{
    private ObservableCollection<TabItemModel> _tabCollection;
    
    public ObservableCollection<TabItemModel> TabCollection
    {
        get { return _tabCollection; }
        set
        {
            _tabCollection = value;
            RaisePropertyChanged(nameof(TabCollection));
        }
    }
    
    public ViewModel()
    {
        TabCollection = new ObservableCollection<TabItemModel>();
        InitializeCollection();
    }
    
    private void InitializeCollection()
    {
        TabCollection.Add(new TabItemModel { HeaderName = "item1" });
        TabCollection.Add(new TabItemModel { HeaderName = "item2" });
        TabCollection.Add(new TabItemModel { HeaderName = "item3" });
    }
}
```

**Model:**
```csharp
public class TabItemModel : NotificationObject
{
    private string _headerName;
    
    public string HeaderName
    {
        get { return _headerName; }
        set
        {
            _headerName = value;
            RaisePropertyChanged(nameof(HeaderName));
        }
    }
}
```

**VB.NET Version (ViewModel):**
```vb
Public Class ViewModel
    Inherits NotificationObject
    
    Private _tabcollection As ObservableCollection(Of TabItemModel)
    
    Public Property TabCollection() As ObservableCollection(Of TabItemModel)
        Get
            Return _tabcollection
        End Get
        Set(ByVal value As ObservableCollection(Of TabItemModel))
            _tabcollection = value
            RaisePropertyChanged("TabCollection")
        End Set
    End Property
    
    Public Sub New()
        TabCollection = New ObservableCollection(Of TabItemModel)()
        InitializeCollection()
    End Sub
    
    Private Sub InitializeCollection()
        TabCollection.Add(New TabItemModel() With {.HeaderName = "item1"})
        TabCollection.Add(New TabItemModel() With {.HeaderName = "item2"})
        TabCollection.Add(New TabItemModel() With {.HeaderName = "item3"})
    End Sub
End Class
```

### Setting DataContext

The `DataContext` property specifies the default source for data binding in MVVM.

**Method 1: XAML Declaration**
```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>
```

**Method 2: Code-Behind**
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

**Method 3: App.xaml.cs (Dependency Injection)**
```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        var mainWindow = new MainWindow
        {
            DataContext = new ViewModel()
        };
        mainWindow.Show();
    }
}
```

## MVVM Commands

Syncfusion provides built-in commands to handle control events in a MVVM-friendly way.

### Required Assemblies

To use Syncfusion MVVM commands:
- `Syncfusion.Tools.MVVM.WPF.dll` (for TabControlExt commands)
- `Syncfusion.Shared.MVVM.WPF.dll` (dependency assembly)

### TabControlExt SelectionChanged Command

Handle the `SelectionChanged` event using `TabControlExtSelectionChangedCommand`.

**XAML:**
```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:mvvm="clr-namespace:Syncfusion.UI.Xaml.Tools.MVVM;assembly=Syncfusion.Shared.MVVM.WPF">
    
    <syncfusion:TabControlExt 
        ItemsSource="{Binding TabCollection}"
        mvvm:TabControlExtSelectionChangedCommand.Command="{Binding SelectionChangedCommand}">
        <syncfusion:TabControlExt.ItemTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding HeaderName}"/>
            </DataTemplate>
        </syncfusion:TabControlExt.ItemTemplate>
    </syncfusion:TabControlExt>
</Window>
```

**ViewModel Implementation:**
```csharp
public class ViewModel : NotificationObject
{
    private ICommand _selectionChangedCommand;
    
    public ICommand SelectionChangedCommand
    {
        get { return _selectionChangedCommand; }
    }
    
    public ViewModel()
    {
        _selectionChangedCommand = new DelegateCommand<object>(OnSelectionChanged);
    }
    
    private void OnSelectionChanged(object parameter)
    {
        MessageBox.Show("Selection changed!");
        
        // Access selected item if needed
        if (parameter is TabItemModel selectedItem)
        {
            MessageBox.Show($"Selected: {selectedItem.HeaderName}");
        }
    }
}
```

**VB.NET Version:**
```vb
Public Class ViewModel
    Inherits NotificationObject
    
    Private selectionChangedCommand_Renamed As ICommand
    
    Public ReadOnly Property SelectionChangedCommand() As ICommand
        Get
            Return selectionChangedCommand_Renamed
        End Get
    End Property
    
    Public Sub New()
        selectionChangedCommand_Renamed = New DelegateCommand(Of Object)(AddressOf OnSelectionChanged)
    End Sub
    
    Private Sub OnSelectionChanged(ByVal obj As Object)
        MessageBox.Show("Selection changed!")
    End Sub
End Class
```

### Other Control Commands

Many Syncfusion controls provide similar command patterns:

```xml
<!-- Button Click Command -->
<syncfusion:ButtonAdv Command="{Binding ClickCommand}"/>

<!-- Checkbox Checked/Unchecked -->
<CheckBox Command="{Binding CheckedCommand}" 
          IsChecked="{Binding IsChecked, Mode=TwoWay}"/>
```

## CommandParameter Usage

`CommandParameter` allows you to pass data to command handlers.

### Passing String Values

**XAML:**
```xml
<syncfusion:TabControlExt 
    ItemsSource="{Binding TabCollection}"
    mvvm:TabControlExtSelectionChangedCommand.Command="{Binding SelectionChangedCommand}"
    mvvm:TabControlExtSelectionChangedCommand.CommandParameter="SelectedItem Command Parameter">
</syncfusion:TabControlExt>
```

**ViewModel:**
```csharp
private void OnSelectionChanged(object parameter)
{
    // parameter will contain the string "SelectedItem Command Parameter"
    MessageBox.Show(parameter.ToString());
}
```

**VB.NET:**
```vb
Private Sub OnSelectionChanged(ByVal obj As Object)
    ' obj will contain the string "SelectedItem Command Parameter"
    MessageBox.Show(obj.ToString())
End Sub
```

### Passing Property Values Through CommandParameter

Bind control properties to CommandParameter to pass dynamic values.

**XAML - Pass SelectedItem:**
```xml
<syncfusion:TabControlExt 
    x:Name="tabControl"
    ItemsSource="{Binding TabCollection}"
    mvvm:TabControlExtSelectionChangedCommand.Command="{Binding SelectionChangedCommand}"
    mvvm:TabControlExtSelectionChangedCommand.CommandParameter="{Binding Path=SelectedItem.HeaderName, RelativeSource={RelativeSource Self}}">
</syncfusion:TabControlExt>
```

**ViewModel:**
```csharp
private void OnSelectionChanged(object parameter)
{
    if (parameter is string headerName)
    {
        MessageBox.Show($"Selected item: {headerName}");
    }
}
```

### Passing Multiple Values

Use a multi-binding converter or pass the entire control:

**XAML - Pass Entire Control:**
```xml
<syncfusion:TabControlExt 
    x:Name="tabControl"
    ItemsSource="{Binding TabCollection}"
    mvvm:TabControlExtSelectionChangedCommand.Command="{Binding SelectionChangedCommand}"
    mvvm:TabControlExtSelectionChangedCommand.CommandParameter="{Binding ElementName=tabControl}">
</syncfusion:TabControlExt>
```

**ViewModel:**
```csharp
private void OnSelectionChanged(object parameter)
{
    if (parameter is TabControlExt tabControl)
    {
        var selectedItem = tabControl.SelectedItem as TabItemModel;
        var selectedIndex = tabControl.SelectedIndex;
        MessageBox.Show($"Item: {selectedItem?.HeaderName}, Index: {selectedIndex}");
    }
}
```

### Passing ViewModel Properties

**XAML:**
```xml
<syncfusion:TabControlExt 
    ItemsSource="{Binding TabCollection}"
    mvvm:TabControlExtSelectionChangedCommand.Command="{Binding SelectionChangedCommand}"
    mvvm:TabControlExtSelectionChangedCommand.CommandParameter="{Binding CurrentUser}">
</syncfusion:TabControlExt>
```

**ViewModel:**
```csharp
public string CurrentUser { get; set; } = "John Doe";

private void OnSelectionChanged(object parameter)
{
    string user = parameter as string;
    MessageBox.Show($"User {user} changed tab");
}
```

## Command Target

The `CommandTarget` property specifies the element where the command exists.

**Use Case:** When command is defined on a different element or needs specific focus.

**XAML Example:**
```xml
<Grid>
    <syncfusion:TabControlExt x:Name="tabControl" 
                              ItemsSource="{Binding TabCollection}"/>
    
    <Button Content="Perform Action"
            Command="{Binding ActionCommand}"
            CommandTarget="{Binding ElementName=tabControl}"/>
</Grid>
```

**Reference:** [CommandTarget Property Documentation](https://docs.microsoft.com/en-us/dotnet/api/system.windows.input.icommandsource.commandtarget)

## DataContext and Data Binding

### Binding ItemsSource

**XAML:**
```xml
<syncfusion:SfDataGrid ItemsSource="{Binding Employees}" 
                       SelectedItem="{Binding SelectedEmployee, Mode=TwoWay}"
                       AutoGenerateColumns="True"/>
```

**ViewModel:**
```csharp
public class ViewModel : NotificationObject
{
    private ObservableCollection<Employee> _employees;
    private Employee _selectedEmployee;
    
    public ObservableCollection<Employee> Employees
    {
        get { return _employees; }
        set
        {
            _employees = value;
            RaisePropertyChanged(nameof(Employees));
        }
    }
    
    public Employee SelectedEmployee
    {
        get { return _selectedEmployee; }
        set
        {
            _selectedEmployee = value;
            RaisePropertyChanged(nameof(SelectedEmployee));
            OnEmployeeSelected();
        }
    }
    
    private void OnEmployeeSelected()
    {
        // Handle selection change
        if (SelectedEmployee != null)
        {
            // Do something with selected employee
        }
    }
}
```

### Two-Way Binding

**XAML:**
```xml
<syncfusion:SfTextBoxExt Text="{Binding Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
```

**ViewModel:**
```csharp
private string _name;

public string Name
{
    get { return _name; }
    set
    {
        _name = value;
        RaisePropertyChanged(nameof(Name));
        ValidateName();
    }
}

private void ValidateName()
{
    // Validation logic runs on every change
}
```

## NotificationObject Base Class

Syncfusion provides `NotificationObject` to simplify `INotifyPropertyChanged` implementation.

### Basic Usage

```csharp
using Syncfusion.Windows.Shared;

public class ViewModel : NotificationObject
{
    private string _title;
    
    public string Title
    {
        get { return _title; }
        set
        {
            _title = value;
            RaisePropertyChanged(nameof(Title));
        }
    }
}
```

### With Property Validation

```csharp
public class ViewModel : NotificationObject
{
    private int _age;
    
    public int Age
    {
        get { return _age; }
        set
        {
            if (value >= 0 && value <= 150)
            {
                _age = value;
                RaisePropertyChanged(nameof(Age));
                RaisePropertyChanged(nameof(IsAdult));
            }
        }
    }
    
    public bool IsAdult => Age >= 18;
}
```

### Alternative: INotifyPropertyChanged

If not using `NotificationObject`, implement manually:

```csharp
using System.ComponentModel;

public class ViewModel : INotifyPropertyChanged
{
    private string _title;
    
    public string Title
    {
        get { return _title; }
        set
        {
            _title = value;
            OnPropertyChanged(nameof(Title));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Complete MVVM Example

### Scenario: Employee Management with SfDataGrid

**Model:**
```csharp
public class Employee : NotificationObject
{
    private int _id;
    private string _name;
    private string _department;
    private decimal _salary;
    
    public int Id
    {
        get { return _id; }
        set { _id = value; RaisePropertyChanged(nameof(Id)); }
    }
    
    public string Name
    {
        get { return _name; }
        set { _name = value; RaisePropertyChanged(nameof(Name)); }
    }
    
    public string Department
    {
        get { return _department; }
        set { _department = value; RaisePropertyChanged(nameof(Department)); }
    }
    
    public decimal Salary
    {
        get { return _salary; }
        set { _salary = value; RaisePropertyChanged(nameof(Salary)); }
    }
}
```

**ViewModel:**
```csharp
public class EmployeeViewModel : NotificationObject
{
    private ObservableCollection<Employee> _employees;
    private Employee _selectedEmployee;
    
    public ObservableCollection<Employee> Employees
    {
        get { return _employees; }
        set
        {
            _employees = value;
            RaisePropertyChanged(nameof(Employees));
        }
    }
    
    public Employee SelectedEmployee
    {
        get { return _selectedEmployee; }
        set
        {
            _selectedEmployee = value;
            RaisePropertyChanged(nameof(SelectedEmployee));
        }
    }
    
    public ICommand AddEmployeeCommand { get; }
    public ICommand DeleteEmployeeCommand { get; }
    
    public EmployeeViewModel()
    {
        LoadEmployees();
        AddEmployeeCommand = new DelegateCommand<object>(OnAddEmployee);
        DeleteEmployeeCommand = new DelegateCommand<object>(OnDeleteEmployee, CanDeleteEmployee);
    }
    
    private void LoadEmployees()
    {
        Employees = new ObservableCollection<Employee>
        {
            new Employee { Id = 1, Name = "John Doe", Department = "IT", Salary = 50000 },
            new Employee { Id = 2, Name = "Jane Smith", Department = "HR", Salary = 55000 },
            new Employee { Id = 3, Name = "Bob Johnson", Department = "Sales", Salary = 48000 }
        };
    }
    
    private void OnAddEmployee(object parameter)
    {
        var newEmployee = new Employee
        {
            Id = Employees.Count + 1,
            Name = "New Employee",
            Department = "Unknown",
            Salary = 40000
        };
        Employees.Add(newEmployee);
    }
    
    private void OnDeleteEmployee(object parameter)
    {
        if (SelectedEmployee != null)
        {
            Employees.Remove(SelectedEmployee);
        }
    }
    
    private bool CanDeleteEmployee(object parameter)
    {
        return SelectedEmployee != null;
    }
}
```

**View (MainWindow.xaml):**
```xml
<Window x:Class="YourApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourApp">
    
    <Window.DataContext>
        <local:EmployeeViewModel/>
    </Window.DataContext>
    
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        
        <syncfusion:SfDataGrid Grid.Row="0"
                               ItemsSource="{Binding Employees}"
                               SelectedItem="{Binding SelectedEmployee, Mode=TwoWay}"
                               AutoGenerateColumns="True"
                               AllowEditing="True"/>
        
        <StackPanel Grid.Row="1" Orientation="Horizontal" Margin="10">
            <Button Content="Add Employee" 
                    Command="{Binding AddEmployeeCommand}"
                    Margin="5"/>
            <Button Content="Delete Employee" 
                    Command="{Binding DeleteEmployeeCommand}"
                    Margin="5"/>
        </StackPanel>
    </Grid>
</Window>
```

## Theme Management

### Applying Themes via App.xaml

**MaterialLight Theme:**
```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/Syncfusion.Themes.MaterialLight.WPF;component/MSControl/SfDataGrid.xaml"/>
            <ResourceDictionary Source="/Syncfusion.Themes.MaterialLight.WPF;component/MSControl/Button.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

### Using SfSkinManager

**C#:**
```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
    }
}
```

**XAML:**
```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
</Window>
```

### Available Themes

- MaterialLight
- MaterialDark
- FluentLight
- FluentDark
- Office2019White
- Office2019Black
- Windows11Light
- Windows11Dark

## Performance Best Practices

### ✓ DO

1. **Use ObservableCollection for dynamic data**
   ```csharp
   public ObservableCollection<Employee> Employees { get; set; }
   ```

2. **Enable UI virtualization for large datasets**
   ```xml
   <syncfusion:SfDataGrid VirtualizingPanel.IsVirtualizing="True"
                          VirtualizingPanel.VirtualizationMode="Recycling"/>
   ```

3. **Use async/await for long operations**
   ```csharp
   private async Task LoadDataAsync()
   {
       var data = await _service.GetEmployeesAsync();
       Employees = new ObservableCollection<Employee>(data);
   }
   ```

4. **Dispose resources properly**
   ```csharp
   public class ViewModel : NotificationObject, IDisposable
   {
       public void Dispose()
       {
           // Clean up resources
       }
   }
   ```

5. **Batch property changes**
   ```csharp
   public void UpdateEmployee(Employee emp)
   {
       // Batch updates to reduce notifications
       emp.Name = "New Name";
       emp.Department = "New Dept";
       emp.Salary = 60000;
       // Single PropertyChanged notification
   }
   ```

### ✗ DON'T

1. **Don't use List<T> for dynamic data** - Use ObservableCollection<T>
2. **Don't call RaisePropertyChanged unnecessarily**
3. **Don't perform heavy operations in property getters**
4. **Don't create new collections on every get**
5. **Don't forget to unsubscribe from events**

## Common MVVM Patterns

### Lazy Loading

```csharp
private ObservableCollection<Employee> _employees;

public ObservableCollection<Employee> Employees
{
    get
    {
        if (_employees == null)
        {
            LoadEmployees();
        }
        return _employees;
    }
}
```

### Validation

```csharp
public class Employee : NotificationObject, IDataErrorInfo
{
    private string _name;
    
    public string Name
    {
        get { return _name; }
        set
        {
            _name = value;
            RaisePropertyChanged(nameof(Name));
        }
    }
    
    public string Error => null;
    
    public string this[string propertyName]
    {
        get
        {
            if (propertyName == nameof(Name) && string.IsNullOrWhiteSpace(Name))
            {
                return "Name is required";
            }
            return null;
        }
    }
}
```

### Command CanExecute

```csharp
private ICommand _saveCommand;

public ICommand SaveCommand
{
    get
    {
        if (_saveCommand == null)
        {
            _saveCommand = new DelegateCommand<object>(
                OnSave,
                CanSave
            );
        }
        return _saveCommand;
    }
}

private void OnSave(object parameter)
{
    // Save logic
}

private bool CanSave(object parameter)
{
    return !string.IsNullOrEmpty(Name) && Salary > 0;
}
```

## Additional Resources

- **Syncfusion MVVM Documentation:** [Pattern and Practices](https://help.syncfusion.com/wpf/pattern-and-practices)
- **Microsoft MVVM Guide:** [MVVM Pattern](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/advanced/mvvm-pattern)
- **Prism Framework:** [Advanced MVVM](https://prismlibrary.com/)
- **MVVM Light Toolkit:** [Alternative MVVM Framework](http://www.mvvmlight.net/)

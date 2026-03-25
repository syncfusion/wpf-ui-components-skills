# Property Binding in WPF PropertyGrid

This guide covers property binding patterns, data context configuration, and value update modes for the Syncfusion WPF PropertyGrid control.

## Table of Contents
- [Binding with SelectedObject](#binding-with-selectedobject)
- [Built-in Editors](#built-in-editors)
- [Update Source Modes](#update-source-modes)
- [INotifyPropertyChanged Implementation](#inotifypropertychanged-implementation)
- [ObservableCollection Usage](#observablecollection-usage)
- [Binding Multiple Objects](#binding-multiple-objects)

## Binding with SelectedObject

The PropertyGrid displays properties of any object through the `SelectedObject` property. When `SelectedObject` is bound with an object, the properties of that object are automatically parsed and displayed.

### Basic Binding

```csharp
// Employee class to be explored in PropertyGrid
public class Employee 
{
    public string EmployeeName { get; set; }
    public string ID { get; set; }
    public int Age { get; set; }
    public int Experience { get; set; }
}

// ViewModel with SelectedEmployee property
public class ViewModel 
{
    public object SelectedEmployee { get; set; }
    
    public ViewModel() 
    {
        SelectedEmployee = new Employee() 
        {
            EmployeeName = "Johnson",
            Age = 25,
            ID = "1234",
            Experience = 3
        };
    }
}
```

### XAML Binding

```xml
<syncfusion:PropertyGrid SelectedObject="{Binding SelectedEmployee}"
                         Name="propertyGrid1">
    <syncfusion:PropertyGrid.DataContext>
        <local:ViewModel/>
    </syncfusion:PropertyGrid.DataContext>
</syncfusion:PropertyGrid>
```

### Code-Behind Binding

```csharp
PropertyGrid propertyGrid1 = new PropertyGrid();
propertyGrid1.DataContext = new ViewModel();
propertyGrid1.SetBinding(PropertyGrid.SelectedObjectProperty, new Binding("SelectedEmployee"));
```

## Built-in Editors

The PropertyGrid supports several built-in editors that are automatically assigned as value editors based on property type. These editors validate and restrict input according to the property type.

### Default Editor Mapping

| Property Type | Default Editor | Control Used |
|---------------|----------------|--------------|
| int | IntegerTextBoxEditor | IntegerTextBox |
| double | DoubleTextBoxEditor | DoubleTextBox |
| string | TextBoxEditor | TextBox |
| enum | EnumComboEditor | ComboBox |
| DateTime | DateTimeEditor | DateTimeEdit |
| bool | CheckBoxEditor | CheckBox |
| Brush | BrushSelectorEditor | ColorPicker |
| Input Mask | MaskEditor | SfMaskedEdit |
| TimeSpan | TimeSpanEditor | TimeSpanEdit |
| FontFamily | FontComboEditor | ComboBox |

### Example with Multiple Property Types

```csharp
public class Employee 
{
    public string Name { get; set; }        // TextBox editor
    public int Age { get; set; }            // IntegerTextBox editor
    public DateTime DOB { get; set; }       // DateTimeEdit editor
    public bool IsActive { get; set; }      // CheckBox editor
    public Brush Color { get; set; }        // ColorPicker editor
    public Gender Gender { get; set; }      // ComboBox editor (enum)
}

public enum Gender 
{
    Male,
    Female,
    Other
}
```

## Built-in Mask Attributes

You can restrict user input to valid values (alphanumeric, binary, email, IPv4, etc.) using the built-in `MaskAttribute`.

**Note:** Mask attribute applies only to properties of type `Object` or `string`.

### Available Mask Types

| Mask Type | Description | Example |
|-----------|-------------|---------|
| Alphanumeric | Letters and numbers only | ABC123 |
| Binary | Binary digits (0,1) | 01101 |
| CardNumber | Credit/debit card format | 1234-5678-9012-3456 |
| EmailId | Email address format | user@example.com |
| Fraction | Fractional numbers | 3/4 |
| HexaDecimal | Hexadecimal values | A1B2C3 |
| IPv4 | IPv4 address format | 192.168.1.1 |
| IPv6 | IPv6 address format | 2001:0db8:85a3::8a2e |
| MobileNumber | Phone number format | (123) 456-7890 |
| Number | Numeric values | 12345 |
| Octal | Octal values | 01234567 |
| PositiveNumber | Positive numbers only | 123 (no negatives) |
| ProductKey | Product key format | XXXXX-XXXXX-XXXXX |

### Using Built-in Masks

```csharp
using Syncfusion.Windows.PropertyGrid;

// Apply mask to multiple properties
[Mask(MaskAttribute.EmailId, "EmailID_1, EmailID_2")]
[Mask(MaskAttribute.CardNumber, "CardNumberMask")]
public class Masks
{
    [Mask(MaskAttribute.Binary)]
    public string BinaryMask { get; set; }
    
    public string CardNumberMask { get; set; }
    
    public string EmailID_1 { get; set; }
    public string EmailID_2 { get; set; }
    
    [Mask(MaskAttribute.PositiveNumber)]
    public string PositiveNumberMask { get; set; }
    
    [Mask(MaskAttribute.ProductKey)]
    public string ProductKeyMask { get; set; }
}
```

### Custom Regex Masks

You can create custom masks using regular expressions for specific validation patterns:

```csharp
// Email mask for two properties
[MaskAttribute("[A-Za-z0-9._%-]+@[A-Za-z0-9]+.[A-Za-z]{2,3}", "Email_1,Email_2")]
public class Person 
{
    public string Name { get; set; }
    public string Email_1 { get; set; }
    public object Email_2 { get; set; }
    
    // Phone number mask
    [MaskAttribute(@"\(\d{3}\) \d{3} - \d{4}")]
    public string Mobile { get; set; }
    
    // Two-digit age mask
    [MaskAttribute(@"\d{2}")]
    public object Age { get; set; }
}
```

**Note:** Invalid or partial inputs are highlighted with a red border.

### MaskEditor for Custom Editors

You can create custom editors with masks by deriving from `MaskEditor`:

```csharp
// Custom email editor
public class EmailEditor : MaskEditor 
{
    public EmailEditor() 
    {
        Mask = "[A-Za-z0-9._%-]+@[A-Za-z0-9]+.[A-Za-z]{2,3}";
    }
}

// Custom phone editor
public class MobileNoEditor : MaskEditor 
{
    public MobileNoEditor() 
    {
        Mask = @"\(\d{3}\) \d{3} - \d{4}";
    }
}

// Apply custom editors to properties
[Editor("Email_1, Email_2", typeof(EmailEditor))]
[Editor("Mobile", typeof(MobileNoEditor))]
public class Person 
{
    public string Name { get; set; }
    public string Email_1 { get; set; }
    public object Email_2 { get; set; }
    public string Mobile { get; set; }
}
```

## Update Source Modes

Control when property values are updated using the `UpdateSourceMode` property.

### Update Modes

| Mode | Description |
|------|-------------|
| **Immediately** (default) | Updates property value immediately when editor changes |
| **ReturnOrLostFocus** | Updates only when editor loses focus or Enter key is pressed |

### Immediate Update (Default)

```xml
<syncfusion:PropertyGrid UpdateSourceMode="Immediately"
                         SelectedObject="{Binding SelectedEmployee}"/>
```

```csharp
propertyGrid.UpdateSourceMode = UpdateSourceMode.Immediately;
```

**Behavior:** The `SelectedObject` value changes immediately as you type.

### Update on Lost Focus

```xml
<syncfusion:PropertyGrid UpdateSourceMode="ReturnOrLostFocus"
                         SelectedObject="{Binding ElementName=button}"/>
```

```csharp
propertyGrid.UpdateSourceMode = UpdateSourceMode.ReturnOrLostFocus;
```

**Behavior:** The `SelectedObject` value changes only when:
- The editor loses focus (clicking elsewhere)
- Enter key is pressed

### Use Cases

**Use Immediately when:**
- You need real-time synchronization
- Updating live previews
- Simple validation scenarios

**Use ReturnOrLostFocus when:**
- Expensive property updates
- Validation requires complete value
- Avoiding partial value updates
- User should explicitly commit changes

## INotifyPropertyChanged Implementation

Implement property change notification for dynamic property updates:

```csharp
using System.ComponentModel;

public class Employee : INotifyPropertyChanged
{
    private string name;
    private int age;
    
    public string Name
    {
        get => name;
        set
        {
            if (name != value)
            {
                name = value;
                OnPropertyChanged(nameof(Name));
            }
        }
    }
    
    public int Age
    {
        get => age;
        set
        {
            if (age != value)
            {
                age = value;
                OnPropertyChanged(nameof(Age));
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

### Why INotifyPropertyChanged?

- **Two-way binding** - Changes in UI update the object and vice versa
- **Real-time updates** - PropertyGrid reflects external property changes
- **MVVM support** - Essential for proper MVVM pattern implementation

## ObservableCollection Usage

For collection properties, use `ObservableCollection<T>` to enable automatic UI updates:

```csharp
using System.Collections.ObjectModel;

public class Department
{
    public string Name { get; set; }
    
    // Collection automatically notifies on add/remove/clear
    public ObservableCollection<Employee> Employees { get; set; }
    
    public Department()
    {
        Employees = new ObservableCollection<Employee>
        {
            new Employee { Name = "John", Age = 30 },
            new Employee { Name = "Jane", Age = 28 }
        };
    }
}
```

### Collection Editor Support

PropertyGrid automatically provides a collection editor for collection properties:

```csharp
public class Product
{
    public string Name { get; set; }
    
    // Shows collection editor button
    public ObservableCollection<Customer> Customers { get; set; }
    
    // Also works with List<T>
    public List<Supplier> Suppliers { get; set; }
}
```

**Requirements for collection editing:**
- Collection type must derive from `IList`
- Collection must have a setter (or be initialized)
- Item type must have a parameterless constructor

## Binding Multiple Objects

Display common properties of multiple selected objects using `SelectedObjects`:

```csharp
// Multiple employee selection
var employees = new object[]
{
    new Employee { Name = "John", Age = 30, Department = "Sales" },
    new Employee { Name = "Jane", Age = 28, Department = "Sales" }
};

propertyGrid.SelectedObjects = employees;
```

**Behavior:**
- Only **common properties** with the **same value** are shown
- Editing a property updates all selected objects
- Useful for bulk editing scenarios

### Example: Multi-Select Editor

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <ListBox Name="employeeList" 
             SelectionMode="Multiple"
             SelectionChanged="EmployeeList_SelectionChanged">
        <!-- Employee items -->
    </ListBox>
    
    <syncfusion:PropertyGrid Grid.Row="1"
                             Name="propertyGrid1"
                             Height="300"/>
</Grid>
```

```csharp
private void EmployeeList_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (employeeList.SelectedItems.Count > 0)
    {
        var selected = employeeList.SelectedItems.Cast<Employee>().ToArray();
        propertyGrid1.SelectedObjects = selected;
    }
}
```

## Best Practices

### 1. Property Type Selection
- Use appropriate types for properties (`int` for integers, `DateTime` for dates)
- Use `object` or `string` for custom masked properties
- Use `enum` for fixed choice properties

### 2. Validation
- Use mask attributes for input validation
- Implement `IDataErrorInfo` or `INotifyDataErrorInfo` for complex validation
- Provide meaningful error messages

### 3. Performance
- Use `ReturnOrLostFocus` update mode for expensive operations
- Avoid deep nested objects (use FlatMode if needed)
- Filter unnecessary properties early

### 4. Change Notification
- Always implement `INotifyPropertyChanged` for bound objects
- Use `ObservableCollection<T>` for collection properties
- Notify dependent properties when one property changes

### 5. User Experience
- Provide `[Description]` attributes for property tooltips
- Use `[DisplayName]` for user-friendly property names
- Group related properties with `[Category]` attribute

## Related Topics

- [Custom Editors](custom-editors.md) - Create custom value editors
- [Filtering](filtering.md) - Hide and search properties
- [Nested Properties](nested-properties.md) - Work with complex objects
- [Getting Started](getting-started.md) - Basic setup and configuration

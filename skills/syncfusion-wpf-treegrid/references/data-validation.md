# Data Validation for WPF TreeGrid

## Overview

WPF TreeGrid supports comprehensive validation through IDataErrorInfo, INotifyDataErrorInfo, Data Annotations, and custom validation events.

## Validation Modes

```csharp
// Validate during edit
treeGrid.GridValidationMode = GridValidationMode.InEdit;

// Validate on cell focus lost
treeGrid.GridValidationMode = GridValidationMode.InView;

// No automatic validation
treeGrid.GridValidationMode = GridValidationMode.None;
```

## IDataErrorInfo

```csharp
public class Employee : IDataErrorInfo, INotifyPropertyChanged
{
    public string FirstName { get; set; }
    public decimal Salary { get; set; }

    public string this[string columnName]
    {
        get
        {
            switch (columnName)
            {
                case "FirstName":
                    if (string.IsNullOrEmpty(FirstName))
                        return "First Name is required";
                    break;
                case "Salary":
                    if (Salary < 0)
                        return "Salary cannot be negative";
                    break;
            }
            return null;
        }
    }

    public string Error => null;
}
```

## INotifyDataErrorInfo

```csharp
public class Employee : INotifyDataErrorInfo, INotifyPropertyChanged
{
    private Dictionary<string, List<string>> _errors = new Dictionary<string, List<string>>();

    public bool HasErrors => _errors.Count > 0;

    public event EventHandler<DataErrorsChangedEventArgs> ErrorsChanged;

    public IEnumerable GetErrors(string propertyName)
    {
        if (string.IsNullOrEmpty(propertyName) || !_errors.ContainsKey(propertyName))
            return null;
        return _errors[propertyName];
    }

    private void SetErrors(string propertyName, List<string> errors)
    {
        _errors[propertyName] = errors;
        ErrorsChanged?.Invoke(this, new DataErrorsChangedEventArgs(propertyName));
    }

    public string FirstName
    {
        get => _firstName;
        set
        {
            _firstName = value;
            ValidateFirstName();
            OnPropertyChanged();
        }
    }

    private void ValidateFirstName()
    {
        var errors = new List<string>();
        
        if (string.IsNullOrEmpty(FirstName))
            errors.Add("First Name is required");
        else if (FirstName.Length < 2)
            errors.Add("First Name must be at least 2 characters");

        if (errors.Count > 0)
            SetErrors(nameof(FirstName), errors);
        else
            _errors.Remove(nameof(FirstName));
    }
}
```

## Data Annotations

```csharp
using System.ComponentModel.DataAnnotations;

public class Employee : INotifyPropertyChanged
{
    [Required(ErrorMessage = "First Name is required")]
    [StringLength(50, MinimumLength = 2)]
    public string FirstName { get; set; }

    [Required]
    [EmailAddress(ErrorMessage = "Invalid email format")]
    public string Email { get; set; }

    [Range(0, double.MaxValue, ErrorMessage = "Salary must be positive")]
    public decimal Salary { get; set; }
}
```

## Validation Events

```csharp
// Cell validation
treeGrid.CurrentCellValidating += (s, e) =>
{
    if (e.Column.MappingName == "Salary")
    {
        decimal salary;
        if (decimal.TryParse(e.NewValue?.ToString(), out salary))
        {
            if (salary < 0)
            {
                e.IsValid = false;
                e.ErrorMessage = "Salary cannot be negative";
            }
        }
    }
};

// Row validation
treeGrid.RowValidating += (s, e) =>
{
    var employee = e.RowData as Employee;
    if (employee != null)
    {
        if (string.IsNullOrEmpty(employee.FirstName) || string.IsNullOrEmpty(employee.LastName))
        {
            e.IsValid = false;
            e.ErrorMessages.Add("Name", "First and Last names are required");
        }
    }
};
```

## Custom Validation

```csharp
private bool ValidateEmployee(Employee employee, out string errorMessage)
{
    errorMessage = null;

    // Required fields
    if (string.IsNullOrEmpty(employee.FirstName))
    {
        errorMessage = "First Name is required";
        return false;
    }

    // Business rules
    if (employee.Salary > 1000000 && employee.Title != "CEO")
    {
        errorMessage = "Only CEO can have salary > $1M";
        return false;
    }

    // Cross-field validation
    if (employee.JoinDate > DateTime.Now)
    {
        errorMessage = "Join date cannot be in the future";
        return false;
    }

    return true;
}

treeGrid.CurrentCellValidating += (s, e) =>
{
    var node = treeGrid.GetNodeAtRowIndex(e.RowColumnIndex.RowIndex);
    var employee = node?.Item as Employee;
    
    if (employee != null)
    {
        string errorMessage;
        if (!ValidateEmployee(employee, out errorMessage))
        {
            e.IsValid = false;
            e.ErrorMessage = errorMessage;
        }
    }
};
```

## Error Display Customization

```xaml
<!-- Customize error template -->
<syncfusion:SfTreeGrid.RowValidationTemplate>
    <DataTemplate>
        <Grid>
            <TextBlock Text="{Binding ErrorMessage}" 
                      Foreground="Red" 
                      FontWeight="Bold"/>
        </Grid>
    </DataTemplate>
</syncfusion:SfTreeGrid.RowValidationTemplate>
```

## Best Practices

### 1. Choose Appropriate Validation Approach

```csharp
// Simple validation - use IDataErrorInfo
// Complex/async validation - use INotifyDataErrorInfo
// Declarative validation - use Data Annotations
```

### 2. Provide Clear Error Messages

```csharp
public string this[string columnName]
{
    get
    {
        if (columnName == "Email")
        {
            if (string.IsNullOrEmpty(Email))
                return "Email is required";
            if (!IsValidEmail(Email))
                return "Please enter a valid email address (e.g., user@example.com)";
        }
        return null;
    }
}
```

### 3. Validate Related Fields

```csharp
private decimal _salary;
public decimal Salary
{
    get => _salary;
    set
    {
        _salary = value;
        OnPropertyChanged();
        OnPropertyChanged(nameof(TotalCompensation)); // Trigger validation for related field
    }
}
```

## Troubleshooting

### Issue: Validation Not Showing

```csharp
// Check validation mode
treeGrid.GridValidationMode = GridValidationMode.InEdit; // or InView

// Verify IDataErrorInfo implementation
var employee = new Employee { FirstName = "" };
var error = employee["FirstName"];
Debug.WriteLine($"Validation error: {error}");
```

### Issue: Validation Fires Too Early

```csharp
// Use InView mode for validation on focus lost
treeGrid.GridValidationMode = GridValidationMode.InView;

// Or handle in CurrentCellValidating
treeGrid.CurrentCellValidating += (s, e) =>
{
    // Only validate if user actually changed the value
    if (e.NewValue != e.OldValue)
    {
        // Validate here
    }
};
```

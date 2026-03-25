# Editing for WPF TreeGrid

## Table of Contents
- [Overview](#overview)
- [Enable Editing](#enable-editing)
- [Edit Triggers](#edit-triggers)
- [Edit Events](#edit-events)
- [Programmatic Editing](#programmatic-editing)
- [Validation](#validation)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

WPF TreeGrid supports inline editing with various triggers, validation, and programmatic control.

## Enable Editing

```xaml
<!-- Enable for entire grid -->
<syncfusion:SfTreeGrid AllowEditing="True"
                       ItemsSource="{Binding EmployeeDetails}">
    <syncfusion:SfTreeGrid.Columns>
        <!-- Specific column editing -->
        <syncfusion:TreeGridTextColumn MappingName="FirstName" AllowEditing="True"/>
        <syncfusion:TreeGridTextColumn MappingName="LastName" AllowEditing="False"/>
    </syncfusion:SfTreeGrid.Columns>
</syncfusion:SfTreeGrid>
```

```csharp
// Enable editing
treeGrid.AllowEditing = true;

// Per column
treeGrid.Columns["FirstName"].AllowEditing = true;
treeGrid.Columns["LastName"].AllowEditing = false;
```

## Edit Triggers

```csharp
// Single click
treeGrid.EditTrigger = EditTrigger.OnTap;

// Double click (default)
treeGrid.EditTrigger = EditTrigger.OnDoubleTap;

// F2 key only
treeGrid.EditTrigger = EditTrigger.OnF2;
```

## Edit Events

### CurrentCellBeginEdit

Occurs before entering edit mode:

```csharp
treeGrid.CurrentCellBeginEdit += (s, e) =>
{
    // Prevent editing specific cells
    if (e.Column.MappingName == "EmployeeID")
    {
        e.Cancel = true;
    }
};
```

### CurrentCellEndEdit

Occurs when leaving edit mode:

```csharp
treeGrid.CurrentCellEndEdit += (s, e) =>
{
    var newValue = e.NewValue;
    var oldValue = e.OldValue;
    
    Debug.WriteLine($"Value changed from {oldValue} to {newValue}");
};
```

### CurrentCellValueChanged

Occurs when cell value changes:

```csharp
treeGrid.CurrentCellValueChanged += (s, e) =>
{
    var changedColumn = e.Column.MappingName;
    var newValue = e.Record;
    
    // Update related cells
    if (changedColumn == "FirstName" || changedColumn == "LastName")
    {
        UpdateFullName(newValue);
    }
};
```

## Programmatic Editing

```csharp
// Begin edit for current cell
treeGrid.CurrentCellManager.BeginEdit();

// End edit
treeGrid.CurrentCellManager.EndEdit();

// Cancel edit
treeGrid.CurrentCellManager.CancelEdit();

// Move to next cell and edit
var rowIndex = treeGrid.ResolveToRowIndex(employee);
var columnIndex = treeGrid.ResolveToScrollColumnIndex(1);
treeGrid.MoveCurrentCell(new RowColumnIndex(rowIndex, columnIndex));
treeGrid.CurrentCellManager.BeginEdit();
```

## Validation

### Using IDataErrorInfo

```csharp
public class Employee : IDataErrorInfo, INotifyPropertyChanged
{
    private string _email;
    
    public string Email
    {
        get => _email;
        set
        {
            _email = value;
            OnPropertyChanged();
        }
    }

    public string this[string columnName]
    {
        get
        {
            if (columnName == "Email")
            {
                if (string.IsNullOrEmpty(Email))
                    return "Email is required";
                if (!Email.Contains("@"))
                    return "Invalid email format";
            }
            return null;
        }
    }

    public string Error => null;
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Validation Events

```csharp
treeGrid.CurrentCellValidating += (s, e) =>
{
    if (e.Column.MappingName == "Salary")
    {
        if (Convert.ToDecimal(e.NewValue) < 0)
        {
            e.IsValid = false;
            e.ErrorMessage = "Salary cannot be negative";
        }
    }
};
```

## Best Practices

### 1. Use IEditableObject

```csharp
public class Employee : IEditableObject
{
    private Employee _backup;

    public void BeginEdit()
    {
        _backup = (Employee)this.MemberwiseClone();
    }

    public void EndEdit()
    {
        _backup = null;
    }

    public void CancelEdit()
    {
        if (_backup != null)
        {
            // Restore values
            this.FirstName = _backup.FirstName;
            this.LastName = _backup.LastName;
        }
    }
}
```

### 2. Handle Validation Properly

```csharp
treeGrid.GridValidationMode = GridValidationMode.InEdit;

treeGrid.CurrentCellValidating += (s, e) =>
{
    // Validate during editing
    if (!IsValid(e.NewValue))
    {
        e.IsValid = false;
        e.ErrorMessage = "Invalid value";
    }
};
```

### 3. Manage Edit State

```csharp
// Check if editing
bool isEditing = treeGrid.SelectionController.CurrentCellManager.HasCurrentCell 
                 && treeGrid.SelectionController.CurrentCellManager.CurrentCell.IsEditing;

// Commit pending edits
if (isEditing)
{
    treeGrid.CurrentCellManager.EndEdit();
}
```

## Troubleshooting

### Issue: Cannot Enter Edit Mode

```csharp
// Check settings
Debug.WriteLine($"AllowEditing: {treeGrid.AllowEditing}");
Debug.WriteLine($"Column AllowEditing: {treeGrid.Columns["FirstName"].AllowEditing}");
Debug.WriteLine($"EditTrigger: {treeGrid.EditTrigger}");

// Verify no events canceling
treeGrid.CurrentCellBeginEdit += (s, e) =>
{
    Debug.WriteLine($"BeginEdit - Cancel: {e.Cancel}");
};
```

### Issue: Changes Not Saving

```csharp
// Ensure two-way binding
var column = new TreeGridTextColumn
{
    MappingName = "FirstName",
    ValueBinding = new Binding("FirstName")
    {
        Mode = BindingMode.TwoWay,
        UpdateSourceTrigger = UpdateSourceTrigger.PropertyChanged
    }
};

// Check property setter
public string FirstName
{
    get => _firstName;
    set
    {
        _firstName = value;
        OnPropertyChanged(); // Must raise property changed
    }
}
```

### Issue: Validation Not Working

```csharp
// Set validation mode
treeGrid.GridValidationMode = GridValidationMode.InEdit;

// Implement IDataErrorInfo properly
public string this[string columnName]
{
    get
    {
        string result = null;
        
        switch (columnName)
        {
            case "FirstName":
                if (string.IsNullOrEmpty(FirstName))
                    result = "First Name is required";
                break;
            case "Email":
                if (!IsValidEmail(Email))
                    result = "Invalid email format";
                break;
        }
        
        return result;
    }
}
```

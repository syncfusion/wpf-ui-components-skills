# Column Types for WPF TreeGrid

## Table of Contents
- [Overview](#overview)
- [Text Column](#text-column)
- [Numeric Columns](#numeric-columns)
  - [TreeGridNumericColumn](#treegridnumericcolumn)
  - [TreeGridCurrencyColumn](#treegridcurrencycolumn)
  - [TreeGridPercentColumn](#treegridpercentcolumn)
- [DateTime Column](#datetime-column)
- [CheckBox Column](#checkbox-column)
- [ComboBox Column](#combobox-column)
- [Hyperlink Column](#hyperlink-column)
- [Template Column](#template-column)
- [Mask Column](#mask-column)
- [Custom Columns](#custom-columns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

WPF TreeGrid provides various column types for different data representations:
- TreeGridTextColumn - Text display
- TreeGridNumericColumn - Numeric values
- TreeGridCurrencyColumn - Currency formatting
- TreeGridPercentColumn - Percentage values
- TreeGridDateTimeColumn - Date/time values
- TreeGridCheckBoxColumn - Boolean values
- TreeGridComboBoxColumn - Dropdown selection
- TreeGridHyperlinkColumn - Clickable links
- TreeGridTemplateColumn - Custom UI
- TreeGridMaskColumn - Formatted input

## Text Column

Basic text display and editing:

```xaml
<syncfusion:TreeGridTextColumn MappingName="FirstName"
                              HeaderText="First Name"
                              Width="150"
                              TextAlignment="Left"
                              AllowEditing="True"/>
```

**Code-behind:**

```csharp
var textColumn = new TreeGridTextColumn
{
    MappingName = "FirstName",
    HeaderText = "First Name",
    Width = 150,
    TextAlignment = TextAlignment.Left
};
treeGrid.Columns.Add(textColumn);
```

## Numeric Columns

### TreeGridNumericColumn

Display numeric values with formatting:

```xaml
<syncfusion:TreeGridNumericColumn MappingName="EmployeeID"
                                 HeaderText="Employee ID"
                                 NumberDecimalDigits="0"
                                 NumberGroupSeparator=","
                                 MinValue="1"
                                 MaxValue="99999"
                                 AllowScrollingOnCircle="True"/>
```

**Common Properties:**
- `NumberDecimalDigits` - Decimal places
- `NumberGroupSeparator` - Thousands separator
- `NumberGroupSizes` - Grouping pattern
- `MinValue`/`MaxValue` - Value range
- `AllowScrollingOnCircle` - Mouse wheel editing

```csharp
var numericColumn = new TreeGridNumericColumn
{
    MappingName = "EmployeeID",
    HeaderText = "ID",
    NumberDecimalDigits = 0,
    NumberGroupSeparator = ",",
    MinValue = 1,
    MaxValue = 99999
};
```

### TreeGridCurrencyColumn

Format currency values:

```xaml
<syncfusion:TreeGridCurrencyColumn MappingName="Salary"
                                  HeaderText="Salary"
                                  CurrencySymbol="$"
                                  CurrencyDecimalDigits="2"
                                  CurrencyGroupSeparator=","
                                  CurrencyNegativePattern="1"/>
```

**Currency Patterns:**

```csharp
// Negative patterns
// 0: ($n)
// 1: -$n
// 2: $-n
// 3: $n-

var currencyColumn = new TreeGridCurrencyColumn
{
    MappingName = "Salary",
    CurrencySymbol = "$",
    CurrencyDecimalDigits = 2,
    CurrencyGroupSeparator = ",",
    CurrencyNegativePattern = 1, // -$n
    CurrencyPositivePattern = 0  // $n
};
```

### TreeGridPercentColumn

Display percentage values:

```xaml
<syncfusion:TreeGridPercentColumn MappingName="CompletionRate"
                                 HeaderText="Completion %"
                                 PercentDecimalDigits="2"
                                 PercentGroupSeparator=","
                                 PercentSymbol="%"/>
```

```csharp
var percentColumn = new TreeGridPercentColumn
{
    MappingName = "CompletionRate",
    HeaderText = "Completion %",
    PercentDecimalDigits = 2,
    PercentSymbol = "%"
};
```

## DateTime Column

Handle date and time data:

```xaml
<syncfusion:TreeGridDateTimeColumn MappingName="DOB"
                                  HeaderText="Date of Birth"
                                  Pattern="ShortDate"
                                  MinDate="1950-01-01"
                                  MaxDate="2010-12-31"
                                  NullText="Not Specified"
                                  AllowNullValue="True"/>
```

**Date Patterns:**

```csharp
// Available patterns
Pattern = DateTimePattern.ShortDate;      // M/d/yyyy
Pattern = DateTimePattern.LongDate;       // dddd, MMMM dd, yyyy
Pattern = DateTimePattern.ShortTime;      // h:mm tt
Pattern = DateTimePattern.LongTime;       // h:mm:ss tt
Pattern = DateTimePattern.FullDateTime;   // Full date & time
Pattern = DateTimePattern.CustomPattern;  // Use CustomPattern property

var dateColumn = new TreeGridDateTimeColumn
{
    MappingName = "DOB",
    HeaderText = "Date of Birth",
    Pattern = DateTimePattern.ShortDate,
    MinDate = new DateTime(1950, 1, 1),
    MaxDate = new DateTime(2010, 12, 31)
};

// Custom pattern
var customDateColumn = new TreeGridDateTimeColumn
{
    MappingName = "JoinDate",
    Pattern = DateTimePattern.CustomPattern,
    CustomPattern = "MMM dd, yyyy" // Jan 01, 2024
};
```

## CheckBox Column

Boolean value representation:

```xaml
<syncfusion:TreeGridCheckBoxColumn MappingName="IsActive"
                                  HeaderText="Active"
                                  AllowCheckBoxOnHeader="True"
                                  IsThreeState="False"/>
```

```csharp
var checkBoxColumn = new TreeGridCheckBoxColumn
{
    MappingName = "IsActive",
    HeaderText = "Active",
    AllowCheckBoxOnHeader = true, // Header checkbox to toggle all
    IsThreeState = false          // True for Checked/Unchecked/Indeterminate
};

// Header checkbox events
treeGrid.HeaderCheckBoxClick += (s, e) =>
{
    bool isChecked = e.IsChecked;
    Debug.WriteLine($"Header checkbox: {isChecked}");
};
```

## ComboBox Column

Dropdown selection:

```xaml
<syncfusion:TreeGridComboBoxColumn MappingName="Department"
                                  HeaderText="Department"
                                  ItemsSource="{Binding Departments}"
                                  DisplayMemberPath="Name"
                                  SelectedValuePath="ID"
                                  IsEditable="True"
                                  StaysOpenOnEdit="True"/>
```

```csharp
// Setup ComboBox column
var comboBoxColumn = new TreeGridComboBoxColumn
{
    MappingName = "Department",
    HeaderText = "Department",
    DisplayMemberPath = "Name",
    SelectedValuePath = "ID",
    IsEditable = true
};

// Set ItemsSource from collection
comboBoxColumn.ItemsSource = new List<Department>
{
    new Department { ID = 1, Name = "Engineering" },
    new Department { ID = 2, Name = "Sales" },
    new Department { ID = 3, Name = "Marketing" }
};

treeGrid.Columns.Add(comboBoxColumn);

// Custom ItemTemplate
var comboBoxColumn = new TreeGridComboBoxColumn
{
    MappingName = "Department",
    ItemTemplate = (DataTemplate)Resources["DepartmentTemplate"]
};
```

## Hyperlink Column

Clickable links:

```xaml
<syncfusion:TreeGridHyperlinkColumn MappingName="WebSite"
                                   HeaderText="Website"
                                   NavigateText="Visit"
                                   TextDecorations="Underline"/>
```

```csharp
var hyperlinkColumn = new TreeGridHyperlinkColumn
{
    MappingName = "WebSite",
    HeaderText = "Website"
};

// Handle navigation
treeGrid.CurrentCellRequestNavigate += (s, e) =>
{
    if (e.Column.MappingName == "WebSite")
    {
        var url = e.NavigateText;
        Process.Start(new ProcessStartInfo
        {
            FileName = url,
            UseShellExecute = true
        });
    }
};
```

## Template Column

Custom UI templates:

```xaml
<syncfusion:TreeGridTemplateColumn MappingName="Photo" HeaderText="Photo">
    <syncfusion:TreeGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <Image Source="{Binding Photo}" 
                   Width="50" 
                   Height="50" 
                   Stretch="UniformToFill"/>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.CellTemplate>
    
    <syncfusion:TreeGridTemplateColumn.EditTemplate>
        <DataTemplate>
            <Button Content="Upload Photo" Click="UploadPhoto_Click"/>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.EditTemplate>
</syncfusion:TreeGridTemplateColumn>
```

**Complex Template Example:**

```xaml
<syncfusion:TreeGridTemplateColumn MappingName="Status" HeaderText="Status">
    <syncfusion:TreeGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Ellipse Width="10" Height="10" Margin="5,0">
                    <Ellipse.Fill>
                        <SolidColorBrush>
                            <SolidColorBrush.Color>
                                <MultiBinding Converter="{StaticResource StatusColorConverter}">
                                    <Binding Path="Status"/>
                                </MultiBinding>
                            </SolidColorBrush.Color>
                        </SolidColorBrush>
                    </Ellipse.Fill>
                </Ellipse>
                <TextBlock Text="{Binding Status}" VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.CellTemplate>
</syncfusion:TreeGridTemplateColumn>
```

## Mask Column

Formatted input with masks:

```xaml
<syncfusion:TreeGridMaskColumn MappingName="PhoneNumber"
                              HeaderText="Phone"
                              Mask="(999) 000-0000"
                              MaskType="Simple"
                              PromptChar="_"/>
```

**Mask Types:**

```csharp
// Simple mask
var phoneMask = new TreeGridMaskColumn
{
    MappingName = "PhoneNumber",
    Mask = "(999) 000-0000",  // 9=optional, 0=required digit
    MaskType = MaskType.Simple,
    PromptChar = '_'
};

// RegEx mask
var emailMask = new TreeGridMaskColumn
{
    MappingName = "Email",
    Mask = @"[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}",
    MaskType = MaskType.RegEx
};

// Custom mask characters
// 0 = Required digit (0-9)
// 9 = Optional digit (0-9) or space
// # = Optional digit or space, plus and minus signs
// L = Required letter (A-Z, a-z)
// ? = Optional letter (A-Z, a-z)
// & = Required character (any)
// C = Optional character (any)
// A = Required alphanumeric (A-Z, a-z, 0-9)
// a = Optional alphanumeric (A-Z, a-z, 0-9)

// Social Security Number
var ssnMask = new TreeGridMaskColumn
{
    Mapping Name = "SSN",
    Mask = "000-00-0000"
};

// License Plate
var plateMask = new TreeGridMaskColumn
{
    MappingName = "LicensePlate",
    Mask = "AAA-0000"
};
```

## Custom Columns

Create custom column types:

```csharp
// Step 1: Define custom column
public class DatePickerColumn : TreeGridColumn
{
    public DatePickerColumn()
    {
        SetCellType("DatePickerRenderer");
    }

    protected override void SetDisplayBindingConverter()
    {
        (DisplayBinding as Binding).Converter = new CustomDateConverter();
    }

    protected override Freezable CreateInstanceCore()
    {
        return new DatePickerColumn();
    }
}

// Step 2: Create custom renderer
public class DatePickerRenderer : TreeGridVirtualizingCellRenderer<TextBlock, DatePicker>
{
    protected override TextBlock OnCreateDisplayUIElement()
    {
        return new TextBlock();
    }

    protected override DatePicker OnCreateEditUIElement()
    {
        return new DatePicker();
    }

    public override void OnInitializeDisplayElement(TreeDataColumnBase dataColumn, 
                                                    TextBlock uiElement, 
                                                    object dataContext)
    {
        base.OnInitializeDisplayElement(dataColumn, uiElement, dataContext);
        SetDisplayBinding(uiElement, dataColumn.TreeGridColumn, dataContext);
    }

    public override void OnInitializeEditElement(TreeDataColumnBase dataColumn, 
                                                 DatePicker uiElement, 
                                                 object dataContext)
    {
        base.OnInitializeEditElement(dataColumn, uiElement, dataContext);
        SetEditBinding(uiElement, dataColumn.TreeGridColumn, dataContext);
    }

    private static void SetDisplayBinding(TextBlock element, 
                                          TreeGridColumn column, 
                                          object dataContext)
    {
        var binding = new Binding
        {
            Path = new PropertyPath(column.MappingName),
            Mode = BindingMode.TwoWay,
            Converter = (column.DisplayBinding as Binding).Converter
        };
        element.SetBinding(TextBlock.TextProperty, binding);
    }

    private static void SetEditBinding(DatePicker element, 
                                       TreeGridColumn column, 
                                       object dataContext)
    {
        var binding = new Binding
        {
            Source = dataContext,
            Path = new PropertyPath(column.MappingName),
            Mode = BindingMode.TwoWay
        };
        element.SetBinding(DatePicker.SelectedDateProperty, binding);
    }
}

// Step 3: Register renderer
treeGrid.CellRenderers.Add("DatePickerRenderer", new DatePickerRenderer());

// Step 4: Use custom column
treeGrid.Columns.Add(new DatePickerColumn
{
    MappingName = "BirthDate",
    HeaderText = "Birth Date"
});
```

## Best Practices

### 1. Choose Appropriate Column Types

```csharp
// Good - Use specific types
var salaryColumn = new TreeGridCurrencyColumn { MappingName = "Salary" };
var completionColumn = new TreeGridPercentColumn { MappingName = "Progress" };

// Avoid - Using text for specialized data
// var salaryColumn = new TreeGridTextColumn { MappingName = "Salary" }; // Bad
```

### 2. Set Validation Ranges

```csharp
var ageColumn = new TreeGridNumericColumn
{
    MappingName = "Age",
    MinValue = 18,
    MaxValue = 100,
    NumberDecimalDigits = 0
};

var dateColumn = new TreeGridDateTimeColumn
{
    MappingName = "JoinDate",
    MinDate = DateTime.Now.AddYears(-50),
    MaxDate = DateTime.Now
};
```

### 3. Use Null Handling

```csharp
var dateColumn = new TreeGridDateTimeColumn
{
    MappingName = "TerminationDate",
    AllowNullValue = true,
    NullText = "Currently Employed",
    NullValue = null
};
```

### 4. Template Column Performance

```xaml
<!-- Use lightweight elements -->
<syncfusion:TreeGridTemplateColumn.CellTemplate>
    <DataTemplate>
        <!-- Good - Simple element -->
        <TextBlock Text="{Binding Name}" />
        
        <!-- Avoid - Heavy nested layouts -->
        <!--
        <Grid>
            <Border>
                <StackPanel>
                    <TextBlock/>
                </StackPanel>
            </Border>
        </Grid>
        -->
    </DataTemplate>
</syncfusion:TreeGridTemplateColumn.CellTemplate>
```

## Troubleshooting

### Issue: Numeric Column Not Formatting

**Problem**: Numbers display without formatting

**Solutions**:

```csharp
// Ensure proper setup
var numericColumn = new TreeGridNumericColumn
{
    MappingName = "Salary",
    NumberDecimalDigits = 2,
    NumberGroupSeparator = ",",
    NumberGroupSizes = new Int32Collection { 3 }
};

// Check data type
// Property must be numeric (int, double, decimal, etc.)
public decimal Salary { get; set; } // Good
// public string Salary { get; set; } // Won't format
```

### Issue: ComboBox Not Showing Items

**Problem**: Dropdown empty when clicked

**Solutions**:

```csharp
// Verify ItemsSource is set
var comboColumn = new TreeGridComboBoxColumn
{
    MappingName = "Department",
    ItemsSource = viewModel.Departments, // Must be set
    DisplayMemberPath = "Name",
    SelectedValuePath = "ID"
};

// Check binding paths match property names
public class Department
{
    public int ID { get; set; }  // Matches SelectedValuePath
    public string Name { get; set; }  // Matches DisplayMemberPath
}
```

### Issue: DateTime Column Shows Raw Value

**Problem**: Date displays as "1/1/2024 12:00:00 AM"

**Solutions**:

```csharp
// Set appropriate pattern
var dateColumn = new TreeGridDateTimeColumn
{
    MappingName = "DOB",
    Pattern = DateTimePattern.ShortDate, // M/d/yyyy
    // or
    Pattern = DateTimePattern.CustomPattern,
    CustomPattern = "MMM dd, yyyy" // Jan 01, 2024
};
```

### Issue: Template Column Not Updating

**Problem**: Template doesn't reflect data changes

**Solutions**:

```csharp
// Ensure data model implements INotifyPropertyChanged
public class Employee : INotifyPropertyChanged
{
    private string _status;
    public string Status
    {
        get => _status;
        set
        {
            _status = value;
            OnPropertyChanged();
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

// Use proper binding in template
<TextBlock Text="{Binding Status, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
```

### Issue: Mask Column Validation Failing

**Problem**: Mask validation not working correctly

**Solutions**:

```csharp
// Verify mask pattern matches input
var phoneColumn = new TreeGridMaskColumn
{
    MappingName = "Phone",
    Mask = "(999) 000-0000", // Correct format
    MaskType = MaskType.Simple,
    PromptChar = '_',
    ValidationMode = MaskValidationMode.Immediate
};

// Handle validation events
treeGrid.CurrentCellValidating += (s, e) =>
{
    if (e.Column.MappingName == "Phone")
    {
        var phone = e.NewValue?.ToString();
        if (string.IsNullOrWhiteSpace(phone) || phone.Contains("_"))
        {
            e.IsValid = false;
            e.ErrorMessage = "Please enter a complete phone number";
        }
    }
};
```

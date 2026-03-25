# Value and Date Management in SfDatePicker

## Table of Contents
- [Setting Date Values](#setting-date-values)
- [Handling Null Values](#handling-null-values)
- [Watermark and Placeholder Text](#watermark-and-placeholder-text)
- [Inline Editing Mode](#inline-editing-mode)
- [Min and Max Date Range](#min-and-max-date-range)
- [Focus-Based Value Updates](#focus-based-value-updates)
- [Keyboard Input and On-Screen Keyboard](#keyboard-input-and-on-screen-keyboard)

## Setting Date Values

The **Value** property stores the currently selected date. You can set it programmatically or via data binding.

### Default Behavior
If no value is assigned, SfDatePicker automatically displays the **current system date**.

### Via XAML – Static Date
```xaml
<syncfusion:SfDatePicker Value="5/30/2026" 
                         x:Name="sfDatePicker"/>
```

### Via XAML – Binding
```xaml
<syncfusion:SfDatePicker Value="{Binding SelectedDate}" 
                         x:Name="sfDatePicker"/>
```

### Via C# – Set Programmatically
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.Value = new DateTime(2026, 5, 30);
```

### Via C# – Get Current Value
```csharp
if (sfDatePicker.Value.HasValue)
{
    DateTime selectedDate = sfDatePicker.Value.Value;
    Console.WriteLine($"Selected: {selectedDate:d}");
}
```

### MVVM Data Binding Example
```csharp
public class DateViewModel : INotifyPropertyChanged
{
    private DateTime? _selectedDate = DateTime.Now;
    
    public DateTime? SelectedDate
    {
        get => _selectedDate;
        set
        {
            if (_selectedDate != value)
            {
                _selectedDate = value;
                OnPropertyChanged();
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    private void OnPropertyChanged([CallerMemberName] string name = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

```xaml
<syncfusion:SfDatePicker Value="{Binding SelectedDate}" 
                         x:Name="sfDatePicker"/>
```

## Handling Null Values

By default, SfDatePicker must always have a date value (defaults to today). However, you can allow null/empty values for optional date selections.

### Enable Null Values

Set the **AllowNull** property to **true** and Value to **null**:

#### Via XAML
```xaml
<syncfusion:SfDatePicker AllowNull="True" 
                         Value="{x:Null}"
                         x:Name="sfDatePicker"/>
```

#### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.AllowNull = true;
sfDatePicker.Value = null;
```

### Default Behavior When AllowNull is False
If `AllowNull="False"` (default):
- Cannot set Value to null
- Current system date is automatically assigned instead
- Control always displays a date

### Check for Null Value
```csharp
if (sfDatePicker.Value == null)
{
    Console.WriteLine("No date selected");
}
else
{
    DateTime selectedDate = sfDatePicker.Value.Value;
    Console.WriteLine($"Selected: {selectedDate:d}");
}
```

### Use Case: Optional Date Field
```xaml
<StackPanel>
    <TextBlock Text="Birth Date (Optional)"/>
    <syncfusion:SfDatePicker AllowNull="True" 
                             Value="{x:Null}"
                             x:Name="birthDatePicker"/>
    <!-- User can leave blank or select a date -->
</StackPanel>
```

## Watermark and Placeholder Text

The **Watermark** property displays placeholder text when the control is empty (typically when `AllowNull="True"` and Value is null).

### Purpose
- Guides users about what to enter
- Displays only when control is empty
- Disappears when user selects a date

### Via XAML – Basic Watermark
```xaml
<syncfusion:SfDatePicker AllowNull="True"
                         Value="{x:Null}"
                         Watermark="Select a date"
                         x:Name="sfDatePicker"/>
```

### Via C# – Set Watermark
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.AllowNull = true;
sfDatePicker.Value = null;
sfDatePicker.Watermark = "Enter your birth date";
```

### Watermark Examples
```xaml
<!-- Event date selection -->
<syncfusion:SfDatePicker Watermark="Enter event date"/>

<!-- Deadline picker -->
<syncfusion:SfDatePicker Watermark="Project deadline"/>

<!-- Optional field -->
<syncfusion:SfDatePicker Watermark="MM/dd/yyyy (optional)"/>
```

### Customizing Watermark Appearance

Use **WatermarkTemplate** to customize how the watermark text is displayed:

```xaml
<syncfusion:SfDatePicker AllowNull="True"
                         Value="{x:Null}"
                         Watermark="Select the Date"
                         x:Name="sfDatePicker">
    <syncfusion:SfDatePicker.WatermarkTemplate>
        <DataTemplate>
            <Border Background="Yellow" CornerRadius="4" Padding="8">
                <TextBlock Foreground="Blue"
                          FontWeight="Bold"
                          Text="{Binding}"
                          TextAlignment="Center"/>
            </Border>
        </DataTemplate>
    </syncfusion:SfDatePicker.WatermarkTemplate>
</syncfusion:SfDatePicker>
```

### Watermark with Styling
```xaml
<syncfusion:SfDatePicker AllowNull="True"
                         Value="{x:Null}"
                         Watermark="Pick a date"
                         x:Name="sfDatePicker">
    <syncfusion:SfDatePicker.WatermarkTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}"
                      Foreground="Gray"
                      FontStyle="Italic"
                      FontSize="12"/>
        </DataTemplate>
    </syncfusion:SfDatePicker.WatermarkTemplate>
</syncfusion:SfDatePicker>
```

## Inline Editing Mode

The **AllowInlineEditing** property enables users to type dates directly into the control with automatic validation.

### Default Behavior
By default, inline editing is **disabled** (False). Users must use the dropdown selector.

### Enable Inline Editing

#### Via XAML
```xaml
<syncfusion:SfDatePicker AllowInlineEditing="True" 
                         x:Name="sfDatePicker"/>
```

#### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.AllowInlineEditing = true;
```

### How Inline Editing Works

1. User types numeric values into the control
2. Each digit is validated against the FormatString pattern
3. When a complete segment is entered (e.g., a month), focus moves to the next segment
4. Pressing Enter or clicking elsewhere validates the full date
5. Invalid dates are rejected and reverted to previous value

### Inline Editing Example
```csharp
// FormatString is "M/d/yyyy" (default)
// User types: 3 → Move to day
// User types: 19 → Move to year
// User types: 2026 → Complete date entered
// User presses Enter → Date is set to 3/19/2026
```

### Practical Implementation
```xaml
<StackPanel>
    <TextBlock Text="Select a date (type or use dropdown)"/>
    <syncfusion:SfDatePicker AllowInlineEditing="True"
                             FormatString="M/d/yyyy"
                             x:Name="sfDatePicker"/>
    <TextBlock Text="Format: MM/dd/yyyy" FontSize="10" Foreground="Gray"/>
</StackPanel>
```

## Min and Max Date Range

Restrict users from selecting dates outside a specific range using **MinDate** and **MaxDate** properties.

### Purpose
- Ensure dates fall within acceptable bounds
- Prevent future/past date selection
- Enforce business rules (e.g., not before company founding)

### Via XAML – Set Range
```xaml
<syncfusion:SfDatePicker MinDate="1/1/2020"
                         MaxDate="12/31/2030"
                         x:Name="sfDatePicker"/>
```

### Via C# – Set Range
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.MinDate = new DateTime(2020, 1, 1);
sfDatePicker.MaxDate = new DateTime(2030, 12, 31);
```

### Auto-Correction Behavior

If a date is set outside the range:
- **If Value < MinDate** → Value is set to MinDate
- **If Value > MaxDate** → Value is set to MaxDate

```csharp
sfDatePicker.MinDate = new DateTime(2020, 1, 1);
sfDatePicker.MaxDate = new DateTime(2030, 12, 31);

// Try to set a date before MinDate
sfDatePicker.Value = new DateTime(2019, 6, 15);
// Result: Value is automatically set to 1/1/2020 (MinDate)
```

### Practical Examples

**Example 1: Age Verification (18+ Only)**
```csharp
DateTime today = DateTime.Now;
int minAge = 18;

sfDatePicker.MaxDate = today.AddYears(-minAge);  // Max: 18 years ago
sfDatePicker.MinDate = today.AddYears(-120);     // Min: 120 years ago
```

**Example 2: Current Year Transactions**
```csharp
int currentYear = DateTime.Now.Year;

sfDatePicker.MinDate = new DateTime(currentYear, 1, 1);
sfDatePicker.MaxDate = new DateTime(currentYear, 12, 31);
```

**Example 3: Event Date (Next 30 Days)**
```csharp
sfDatePicker.MinDate = DateTime.Now;
sfDatePicker.MaxDate = DateTime.Now.AddDays(30);
```

**Example 4: Project Timeline**
```csharp
DateTime projectStart = new DateTime(2026, 1, 15);
DateTime projectEnd = new DateTime(2026, 12, 31);

sfDatePicker.MinDate = projectStart;
sfDatePicker.MaxDate = projectEnd;
```

## Focus-Based Value Updates

The **SetValueOnLostFocus** property controls when the selected date from the dropdown is applied to the control.

### Default Behavior
- By default (False), date selection is applied only when clicking OK button
- Moving focus away from the dropdown doesn't update the value

### Enable Focus-Based Update

#### Via XAML
```xaml
<syncfusion:SfDatePicker SetValueOnLostFocus="True"
                         x:Name="sfDatePicker"/>
```

#### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.SetValueOnLostFocus = true;
```

### When to Use

**Use True when:**
- You want immediate feedback as user makes selections
- OK/Cancel buttons are hidden or unnecessary
- Selections should be applied automatically

**Use False when:**
- User should confirm before applying (OK button)
- You need optional selection behavior
- Accidental selections should be avoidable

### Practical Example: Real-Time Selection
```xaml
<syncfusion:SfDatePicker SetValueOnLostFocus="True"
                         x:Name="sfDatePicker">
    <!-- As user navigates through date picker and moves focus away,
         the selected date is immediately applied -->
</syncfusion:SfDatePicker>
```

## Keyboard Input and On-Screen Keyboard

The **InputScope** property controls which keyboard appears on touch devices with on-screen keyboards.

### Purpose
- Optimize for touch input on tablets/phones
- Show appropriate keyboard type (numeric, alphanumeric, etc.)
- Works in conjunction with AllowInlineEditing

### Available InputScopes
```csharp
public enum InputScopeNameValue
{
    Default,      // Standard keyboard
    Number,       // Numeric keypad
    Telephone,    // Phone number layout
    Url,          // URL keyboard
    Email,        // Email keyboard
    Date,         // Date input keyboard
    // ... and more
}
```

### Via XAML – Set Input Scope
```xaml
<syncfusion:SfDatePicker AllowInlineEditing="True"
                         InputScope="Number"
                         x:Name="sfDatePicker"/>
```

### Via C# – Set Input Scope
```csharp
using System.Windows.Input;

SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.AllowInlineEditing = true;
sfDatePicker.InputScope = InputScopeNameValue.Number;
```

### Recommended Configurations

**Configuration 1: Numeric Input**
```xaml
<syncfusion:SfDatePicker AllowInlineEditing="True"
                         InputScope="Number"
                         x:Name="sfDatePicker">
    <!-- Shows numeric keypad for touch devices -->
</syncfusion:SfDatePicker>
```

**Configuration 2: Date Input**
```xaml
<syncfusion:SfDatePicker AllowInlineEditing="True"
                         InputScope="Date"
                         x:Name="sfDatePicker">
    <!-- Shows date-optimized keyboard for touch devices -->
</syncfusion:SfDatePicker>
```

### Mobile/Touch Best Practices

```xaml
<syncfusion:SfDatePicker AllowInlineEditing="True"
                         InputScope="Number"
                         SelectorItemWidth="120"
                         SelectorItemHeight="100"
                         x:Name="sfDatePicker">
    <!-- Optimized for touch:
         - Large cells for easy tapping
         - Numeric keyboard for fast input
         - Inline editing support -->
</syncfusion:SfDatePicker>
```

## Complete Date Management Example

Here's a comprehensive example combining multiple date management features:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Date Management Example">
    <StackPanel Padding="20" Spacing="20">
        
        <!-- Required date (always has value) -->
        <StackPanel>
            <TextBlock Text="Event Date (Required)" FontWeight="Bold"/>
            <syncfusion:SfDatePicker MinDate="2026/01/01"
                                     MaxDate="2026/12/31"
                                     AllowInlineEditing="True"
                                     x:Name="eventDate"/>
        </StackPanel>
        
        <!-- Optional date with watermark -->
        <StackPanel>
            <TextBlock Text="Follow-up Date (Optional)" FontWeight="Bold"/>
            <syncfusion:SfDatePicker AllowNull="True"
                                     Watermark="Select follow-up date"
                                     SetValueOnLostFocus="True"
                                     x:Name="followUpDate"/>
        </StackPanel>
        
        <!-- Age verification -->
        <StackPanel>
            <TextBlock Text="Date of Birth (18+ required)" FontWeight="Bold"/>
            <syncfusion:SfDatePicker AllowInlineEditing="True"
                                     InputScope="Number"
                                     x:Name="birthDate"/>
        </StackPanel>
        
    </StackPanel>
</Window>
```

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Configure age verification (18+)
        int minAge = 18;
        DateTime today = DateTime.Now;
        birthDate.MaxDate = today.AddYears(-minAge);
        birthDate.MinDate = today.AddYears(-120);
    }
}
```

## Best Practices

1. **Always validate date ranges** – Set MinDate/MaxDate based on business requirements
2. **Use watermarks for optional dates** – Guide users when null values are allowed
3. **Enable inline editing on mobile** – Pair with appropriate InputScope
4. **Provide visual feedback** – Use events to show selected dates
5. **Document format expectations** – Show users expected date format
6. **Test with edge cases** – Boundary dates (min, max, today)
7. **Consider accessibility** – Ensure keyboard navigation works properly

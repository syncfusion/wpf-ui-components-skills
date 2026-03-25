# Features and Interaction in SfDatePicker

## Table of Contents
- [Dropdown Button Visibility](#dropdown-button-visibility)
- [Dropdown Height Customization](#dropdown-height-customization)
- [Date Range Validation](#date-range-validation)
- [Localization Support](#localization-support)
- [Common Workflows](#common-workflows)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Dropdown Button Visibility

The **ShowDropDownButton** property controls whether the dropdown button (calendar icon) is visible to the user.

### Default Behavior
By default, the dropdown button is **visible** (True).

### Hide the Dropdown Button

#### Via XAML
```xaml
<syncfusion:SfDatePicker ShowDropDownButton="False" 
                         x:Name="sfDatePicker"/>
```

#### Via C#
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.ShowDropDownButton = false;
```

### Use Cases

**Hide the button when:**
- You want users to only use keyboard input or inline editing
- You need a minimal, text-only date input field
- You want to enforce date typing (validation purpose)
- You're building a dense form with limited space

**Show the button when:**
- You want to provide easy visual date selection
- You're targeting touch/mobile devices
- You need to accommodate less tech-savvy users
- You want discoverable date picker functionality

### Practical Example 1: Minimal Text Input
```xaml
<syncfusion:SfDatePicker ShowDropDownButton="False"
                         AllowInlineEditing="True"
                         Watermark="MM/dd/yyyy"
                         x:Name="sfDatePicker"/>
<!-- Users type dates; no visual picker available -->
```

### Practical Example 2: Forced Keyboard Entry
```csharp
sfDatePicker.ShowDropDownButton = false;
sfDatePicker.AllowInlineEditing = true;
// Users must type dates; dropdown forbidden
```

### Practical Example 3: Mobile-Optimized with Button Hidden
```xaml
<syncfusion:SfDatePicker ShowDropDownButton="False"
                         AllowInlineEditing="True"
                         InputScope="Number"
                         x:Name="sfDatePicker"/>
<!-- Mobile keyboard + inline editing, no button clutter -->
```

## Dropdown Height Customization

The **DropDownHeight** property controls the height of the date selector dropdown popup.

### Default Behavior
By default, the height is calculated automatically based on content and available screen space.

### Via XAML – Set Custom Height
```xaml
<syncfusion:SfDatePicker DropDownHeight="300" 
                         x:Name="sfDatePicker"/>
```

### Via C# – Set Custom Height
```csharp
SfDatePicker sfDatePicker = new SfDatePicker();
sfDatePicker.DropDownHeight = 300;
```

### Height Guidelines

| DropDownHeight | Display | Use Case |
|---|---|---|
| 150 | Compact, minimal space | Dense forms, small screens |
| 200 | Standard, balanced | Most desktop applications |
| 250-300 | Comfortable, readable | Large screens, accessibility |
| 400+ | Very tall, spacious | Touch devices, large displays |

### Practical Examples

**Example 1: Desktop Application (Balanced)**
```xaml
<syncfusion:SfDatePicker DropDownHeight="250" x:Name="sfDatePicker"/>
```

**Example 2: Tablet Application (Spacious)**
```xaml
<syncfusion:SfDatePicker DropDownHeight="400" x:Name="sfDatePicker"/>
```

**Example 3: Mobile Application (Compact)**
```xaml
<syncfusion:SfDatePicker DropDownHeight="150" x:Name="sfDatePicker"/>
```

### Dynamic Height Based on Window Size
```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Set dropdown height based on window height
    double windowHeight = SystemParameters.PrimaryScreenHeight;
    sfDatePicker.DropDownHeight = Math.Max(200, windowHeight * 0.3);
}
```

## Date Range Validation

Validating that selected dates fall within acceptable ranges and handling validation errors.

### Automatic Range Enforcement

MinDate and MaxDate automatically constrain selections:

```csharp
sfDatePicker.MinDate = new DateTime(2026, 1, 1);
sfDatePicker.MaxDate = new DateTime(2026, 12, 31);

// If user tries to select 2025-06-15 (before MinDate)
sfDatePicker.Value = new DateTime(2025, 6, 15);
// Result: Value is set to 2026-01-01 (MinDate)
```

### Custom Validation Logic

Implement validation in the ValueChanged event handler:

```csharp
private void SfDatePicker_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (e.NewValue == null) return;
    
    DateTime newDate = (DateTime)e.NewValue;
    DateTime today = DateTime.Now;
    
    // Reject future dates
    if (newDate > today)
    {
        MessageBox.Show("Cannot select future dates");
        sfDatePicker.Value = e.OldValue;
        return;
    }
    
    // Accept the date
    Console.WriteLine($"Valid date selected: {newDate:d}");
}
```

### Complex Validation Example

```csharp
private void ValidateDateSelection(DateTime selectedDate)
{
    bool isValid = true;
    string errorMessage = "";
    
    // Check if date is before today
    if (selectedDate < DateTime.Now.Date)
    {
        isValid = false;
        errorMessage = "Date must be today or later";
    }
    
    // Check if date is a weekend
    if (selectedDate.DayOfWeek == DayOfWeek.Saturday || 
        selectedDate.DayOfWeek == DayOfWeek.Sunday)
    {
        isValid = false;
        errorMessage = "Weekends not allowed";
    }
    
    // Check if within project timeline
    if (selectedDate > new DateTime(2026, 12, 31))
    {
        isValid = false;
        errorMessage = "Date must be within 2026";
    }
    
    if (!isValid)
    {
        MessageBox.Show(errorMessage);
        // Revert to previous value
    }
}
```

### Validation with User Feedback

```xaml
<StackPanel>
    <syncfusion:SfDatePicker x:Name="sfDatePicker"
                             ValueChanged="SfDatePicker_ValueChanged"/>
    <TextBlock x:Name="validationMessage" 
               Foreground="Red" 
               FontSize="12"/>
</StackPanel>
```

```csharp
private void SfDatePicker_ValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    validationMessage.Text = ""; // Clear previous message
    
    if (e.NewValue == null) return;
    
    DateTime newDate = (DateTime)e.NewValue;
    
    // Perform validation
    if (!IsValidDate(newDate))
    {
        validationMessage.Text = "Invalid date. Please select a date within the allowed range.";
    }
}

private bool IsValidDate(DateTime date)
{
    return date >= sfDatePicker.MinDate && date <= sfDatePicker.MaxDate;
}
```

## Localization Support

Localization enables the SfDatePicker to display text in different languages and respect regional settings.

### What Gets Localized
- Button text (OK, Cancel)
- Month and day names
- Date format patterns
- Number formatting

### Regional Settings Impact

Date formatting automatically adapts to system locale:

```
System Locale: en-US → 3/19/2026
System Locale: en-GB → 19/03/2026
System Locale: de-DE → 19.03.2026
System Locale: fr-FR → 19/03/2026
```

### Applying Localization

1. **Use standard format specifiers** – They respect system locale automatically
2. **Set UI language** – Control respects application culture

### Culture-Specific Example

```csharp
using System.Globalization;
using System.Threading;

public MainWindow()
{
    InitializeComponent();
    
    // Set application culture to German
    Thread.CurrentThread.CurrentCulture = new CultureInfo("de-DE");
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
    
    // SfDatePicker now displays in German
    // Buttons: "OK" → "OK" / "Abbrechen" (depending on localization resources)
}
```

### Custom Localization Resource File

Create a resource file for button text:
1. Create `Resources.resx` with translations
2. Add keys: `Button_OK`, `Button_Cancel`
3. Reference in your application

```csharp
// Example: Resources_de.resx
// Button_OK = OK
// Button_Cancel = Abbrechen
```

### Recommended Localization Approach

Use RESX files for maintainability:

```xml
<!-- Resources.resx (English) -->
<data name="OkButton" xml:space="preserve">
    <value>OK</value>
</data>
<data name="CancelButton" xml:space="preserve">
    <value>Cancel</value>
</data>

<!-- Resources.de.resx (German) -->
<data name="OkButton" xml:space="preserve">
    <value>OK</value>
</data>
<data name="CancelButton" xml:space="preserve">
    <value>Abbrechen</value>
</data>
```

## Common Workflows

### Workflow 1: Date Range Picker (From/To)

```xaml
<StackPanel Padding="20">
    <TextBlock Text="Select Date Range" FontSize="14" FontWeight="Bold"/>
    
    <StackPanel Margin="0,10,0,0">
        <TextBlock Text="From:"/>
        <syncfusion:SfDatePicker x:Name="fromDatePicker"
                                 Width="200"/>
    </StackPanel>
    
    <StackPanel Margin="0,10,0,0">
        <TextBlock Text="To:"/>
        <syncfusion:SfDatePicker x:Name="toDatePicker"
                                 Width="200"/>
    </StackPanel>
</StackPanel>
```

```csharp
public MainWindow()
{
    InitializeComponent();
    
    fromDatePicker.ValueChanged += (d, e) =>
    {
        // Ensure 'to' date is after 'from' date
        if (toDatePicker.Value < fromDatePicker.Value)
        {
            toDatePicker.Value = fromDatePicker.Value;
        }
        toDatePicker.MinDate = fromDatePicker.Value;
    };
}
```

### Workflow 2: Age Verification

```csharp
public void ConfigureAgeVerification(int requiredAge)
{
    DateTime today = DateTime.Now;
    DateTime maxBirthDate = today.AddYears(-requiredAge);
    
    sfDatePicker.MaxDate = maxBirthDate;
    sfDatePicker.MinDate = today.AddYears(-150); // Reasonable upper limit
    
    sfDatePicker.ValueChanged += (d, e) =>
    {
        if (e.NewValue != null)
        {
            DateTime birthDate = (DateTime)e.NewValue;
            int age = today.Year - birthDate.Year;
            
            if (birthDate.Date > today.AddYears(-age))
                age--;
                
            Console.WriteLine($"Age: {age}");
        }
    };
}
```

### Workflow 3: Appointment Booking

```xaml
<StackPanel>
    <TextBlock Text="Book Appointment" FontSize="14" FontWeight="Bold"/>
    
    <TextBlock Text="Select Date:" Margin="0,10,0,5"/>
    <syncfusion:SfDatePicker x:Name="appointmentDate"
                             Width="200"/>
    
    <Button Content="Book" Click="Book_Click" Margin="0,10,0,0"/>
</StackPanel>
```

```csharp
private void Book_Click(object sender, RoutedEventArgs e)
{
    if (!appointmentDate.Value.HasValue)
    {
        MessageBox.Show("Please select a date");
        return;
    }
    
    DateTime selectedDate = appointmentDate.Value.Value;
    
    // Check if date is available
    if (IsDateAvailable(selectedDate))
    {
        BookAppointment(selectedDate);
        MessageBox.Show("Appointment booked successfully!");
    }
    else
    {
        MessageBox.Show("Date not available. Please select another date.");
    }
}

private bool IsDateAvailable(DateTime date)
{
    // Check against booked appointments
    return DateTime.Now < date && date.DayOfWeek != DayOfWeek.Sunday;
}

private void BookAppointment(DateTime date)
{
    // Save to database
    Console.WriteLine($"Appointment booked for: {date:d}");
}
```

## Edge Cases and Troubleshooting

### Edge Case 1: Daylight Saving Time

When working with dates near daylight saving time transitions:

```csharp
// Issue: DateTime calculations around DST might produce unexpected results
DateTime march2026 = new DateTime(2026, 3, 8);  // DST starts

// Solution: Use Date, not DateTime with times
DateTime safeDate = march2026.Date;
```

### Edge Case 2: Leap Year Handling

```csharp
// February 29 on a leap year
DateTime leapDate = new DateTime(2024, 2, 29);

// Solution: Validate before setting
if (DateTime.IsLeapYear(leapDate.Year) && leapDate.Month == 2)
{
    // Valid date
    sfDatePicker.Value = leapDate;
}
```

### Edge Case 3: Null Reference When Value is Null

```csharp
// Problem: Accessing Value without null check crashes
int dayOfMonth = sfDatePicker.Value.Day; // Exception if Value is null

// Solution: Check HasValue first
if (sfDatePicker.Value.HasValue)
{
    int dayOfMonth = sfDatePicker.Value.Value.Day;
}
```

### Edge Case 4: Date Binding Issues

```csharp
// Problem: Data binding may not update if property doesn't raise PropertyChanged

public class ViewModel : INotifyPropertyChanged
{
    private DateTime? _selectedDate;
    
    public DateTime? SelectedDate
    {
        get => _selectedDate;
        set
        {
            if (_selectedDate != value)
            {
                _selectedDate = value;
                OnPropertyChanged(); // MUST raise for binding to work
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

### Troubleshooting: Date Not Updating

**Problem:** Date value doesn't change when user selects from dropdown

**Solutions:**
1. Check if `ShowDropDownButton` is False
2. Ensure `ValueChanged` event isn't blocking the update
3. Verify `AllowNull` and `MinDate`/`MaxDate` aren't conflicting
4. Check for data binding errors in Output window

### Troubleshooting: Inline Editing Not Working

**Problem:** User can't type dates directly

**Solutions:**
1. Set `AllowInlineEditing="True"`
2. Verify `InputScope` is appropriate
3. Check `FormatString` matches expected format
4. Ensure `AllowNull` allows empty initial state

### Troubleshooting: Culture/Locale Not Applied

**Problem:** Date formatting doesn't respect system locale

**Solutions:**
1. Use standard format specifiers (d, D, M, y) not custom patterns
2. Verify Thread.CurrentUICulture is set correctly
3. Check System Regional Settings
4. Restart application after changing system locale

## Best Practices Summary

1. **Always validate date ranges** – Use MinDate/MaxDate and custom validation
2. **Provide visual feedback** – Show messages for invalid selections
3. **Consider mobile UX** – Use appropriate InputScope and button visibility
4. **Test edge cases** – Leap years, DST, null values, boundary dates
5. **Localize appropriately** – Use system locale or explicit culture
6. **Handle events safely** – Always null-check before using values
7. **Document your implementation** – Explain validation rules to future developers

# Appearance and Customization in CalendarEdit

## Table of Contents
- [Foreground Customization](#foreground-customization)
- [Background Customization](#background-customization)
- [Border and Mouse Over Effects](#border-and-mouse-over-effects)
- [RTL (Right-to-Left) Support](#rtl-right-to-left-support)
- [Using Calendar Object Methods](#using-calendar-object-methods)
- [Complete Styling Example](#complete-styling-example)
- [Common Patterns](#common-patterns)

---

## Foreground Customization

The `Foreground` property controls the text color of the calendar dates and UI elements.

### Set Foreground Color

```xaml
<!-- Set foreground to blue -->
<syncfusion:CalendarEdit Foreground="Blue"
                         Name="calendarEdit" />
```

```csharp
using System.Windows.Media;

// Set foreground color in C#
calendarEdit.Foreground = Brushes.Blue;

// Using RGB values
calendarEdit.Foreground = new SolidColorBrush(Color.FromRgb(0, 0, 255));  // Blue
```

### Foreground Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Foreground` | Brush | Normal text color | Dark SlateGray |
| `MouseOverForeground` | Brush | Text color on hover | Black |

### Set Mouse Over Foreground

When users hover over dates, the text color changes:

```xaml
<syncfusion:CalendarEdit Foreground="Blue"
                         MouseOverForeground="Red"
                         Name="calendarEdit" />
```

```csharp
calendarEdit.Foreground = Brushes.Blue;
calendarEdit.MouseOverForeground = Brushes.Red;
```

### Foreground Color Examples

```csharp
// Professional blue theme
calendarEdit.Foreground = Brushes.DarkBlue;
calendarEdit.MouseOverForeground = Brushes.CornflowerBlue;

// Dark theme
calendarEdit.Foreground = Brushes.White;
calendarEdit.MouseOverForeground = Brushes.LightGray;

// High contrast for accessibility
calendarEdit.Foreground = Brushes.Black;
calendarEdit.MouseOverForeground = Brushes.White;
```

---

## Background Customization

The `Background` property controls the fill color of the calendar area.

### Set Background Color

```xaml
<!-- Set background to light pink -->
<syncfusion:CalendarEdit Background="Pink"
                         Name="calendarEdit" />
```

```csharp
// Set background in C#
calendarEdit.Background = Brushes.Pink;

// Using RGB values
calendarEdit.Background = new SolidColorBrush(Color.FromRgb(255, 192, 203));  // Light pink
```

### Background Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Background` | Brush | Calendar background color | White |
| `MouseOverBackground` | Brush | Background color on hover | Lavender |

### Set Mouse Over Background

When users hover over dates, the background changes:

```xaml
<syncfusion:CalendarEdit Background="White"
                         MouseOverBackground="LightBlue"
                         Name="calendarEdit" />
```

```csharp
calendarEdit.Background = Brushes.White;
calendarEdit.MouseOverBackground = Brushes.LightBlue;
```

### Background Color Examples

```csharp
// Clean white theme
calendarEdit.Background = Brushes.White;
calendarEdit.MouseOverBackground = Brushes.AliceBlue;

// Dark theme
calendarEdit.Background = Brushes.DarkGray;
calendarEdit.MouseOverBackground = Brushes.Gray;

// Warm theme
calendarEdit.Background = Brushes.Wheat;
calendarEdit.MouseOverBackground = Brushes.NavajoWhite;
```

---

## Border and Mouse Over Effects

### Customize Mouse Over Border Brush

The `MouseOverBorderBrush` property controls the border color when hovering over dates:

```xaml
<syncfusion:CalendarEdit MouseOverBorderBrush="DarkGoldenrod"
                         Name="calendarEdit" />
```

```csharp
calendarEdit.MouseOverBorderBrush = Brushes.DarkGoldenrod;
```

### Complete Hover Styling

```xaml
<syncfusion:CalendarEdit Foreground="Blue"
                         Background="White"
                         MouseOverForeground="Red"
                         MouseOverBackground="LightBlue"
                         MouseOverBorderBrush="Green"
                         Name="calendarEdit" />
```

```csharp
// Professional hover effect
calendarEdit.Foreground = Brushes.DarkBlue;
calendarEdit.Background = Brushes.White;
calendarEdit.MouseOverForeground = Brushes.White;
calendarEdit.MouseOverBackground = Brushes.CornflowerBlue;
calendarEdit.MouseOverBorderBrush = Brushes.Navy;
```

---

## RTL (Right-to-Left) Support

### Enable Right-to-Left Layout

The `FlowDirection` property controls text and UI element direction for languages like Arabic, Hebrew, and Persian.

### RTL in XAML

```xaml
<!-- Enable RTL layout -->
<syncfusion:CalendarEdit FlowDirection="RightToLeft"
                         Name="calendarEdit" />
```

### RTL in C#

```csharp
using System.Windows;

// Enable RTL
calendarEdit.FlowDirection = FlowDirection.RightToLeft;

// Disable RTL (default)
calendarEdit.FlowDirection = FlowDirection.LeftToRight;
```

### FlowDirection Values

| Value | Description | Languages |
|-------|-------------|-----------|
| `LeftToRight` | Default LTR layout | English, French, Spanish, etc. |
| `RightToLeft` | RTL layout | Arabic, Hebrew, Farsi, Urdu, etc. |

### RTL Example with Culture Support

```csharp
public void SetupArabicCalendar()
{
    // Set culture to Arabic
    System.Globalization.CultureInfo culture = 
        new System.Globalization.CultureInfo("ar-SA");
    System.Globalization.CultureInfo.CurrentCulture = culture;
    System.Globalization.CultureInfo.CurrentUICulture = culture;
    
    // Enable RTL layout
    calendarEdit.FlowDirection = FlowDirection.RightToLeft;
    
    // Apply appropriate styling for RTL
    calendarEdit.Foreground = Brushes.Black;
    calendarEdit.Background = Brushes.White;
}
```

---

## Using Calendar Object Methods

The `CalendarEdit` provides access to calendar methods through the `Calendar` property for date arithmetic operations.

### Available Calendar Methods

| Method | Description |
|--------|-------------|
| `AddDays(DateTime, int)` | Returns date + specified days |
| `AddHours(DateTime, int)` | Returns date + specified hours |
| `AddMonths(DateTime, int)` | Returns date + specified months |
| `AddSeconds(DateTime, int)` | Returns date + specified seconds |
| `AddMinutes(DateTime, int)` | Returns date + specified minutes |
| `AddMilliseconds(DateTime, int)` | Returns date + specified milliseconds |
| `AddYears(DateTime, int)` | Returns date + specified years |

### AddDays Example

```csharp
// Add 5 days to current date
DateTime result = calendarEdit.Calendar.AddDays(calendarEdit.Date, 5);
MessageBox.Show($"5 days later: {result:yyyy-MM-dd}");

// Output example: "2024-03-20" (if current date is 2024-03-15)
```

### AddMonths Example

```csharp
// Add 3 months to current date
DateTime result = calendarEdit.Calendar.AddMonths(calendarEdit.Date, 3);
MessageBox.Show($"3 months later: {result:yyyy-MM-dd}");

// Subtract months (negative value)
DateTime previousQuarter = calendarEdit.Calendar.AddMonths(calendarEdit.Date, -3);
MessageBox.Show($"3 months ago: {previousQuarter:yyyy-MM-dd}");
```

### AddYears Example

```csharp
// Add 1 year
DateTime nextYear = calendarEdit.Calendar.AddYears(calendarEdit.Date, 1);
MessageBox.Show($"Next year: {nextYear:yyyy-MM-dd}");

// Add 10 years
DateTime tenYearsLater = calendarEdit.Calendar.AddYears(calendarEdit.Date, 10);
MessageBox.Show($"In 10 years: {tenYearsLater:yyyy-MM-dd}");
```

### AddHours/Minutes/Seconds Example

```csharp
// Add 2 hours to current date
DateTime result = calendarEdit.Calendar.AddHours(calendarEdit.Date, 2);
MessageBox.Show($"In 2 hours: {result:yyyy-MM-dd HH:mm:ss}");

// Add 30 minutes
DateTime inThirtyMinutes = calendarEdit.Calendar.AddMinutes(calendarEdit.Date, 30);
MessageBox.Show($"In 30 minutes: {inThirtyMinutes:yyyy-MM-dd HH:mm:ss}");
```

### Date Arithmetic Pattern

```csharp
private DateTime CalculateDateOffset(int days = 0, int months = 0, int years = 0)
{
    DateTime result = calendarEdit.Date;
    
    if (days != 0)
        result = calendarEdit.Calendar.AddDays(result, days);
    
    if (months != 0)
        result = calendarEdit.Calendar.AddMonths(result, months);
    
    if (years != 0)
        result = calendarEdit.Calendar.AddYears(result, years);
    
    return result;
}

// Usage: Get date 3 months and 5 days from now
DateTime futureDate = CalculateDateOffset(5, 3, 0);
MessageBox.Show($"Future date: {futureDate:yyyy-MM-dd}");
```

---

## Complete Styling Example

### Professional Calendar Theme

```csharp
public void ApplyProfessionalTheme()
{
    // Colors
    Color primaryColor = Color.FromRgb(45, 125, 154);
    Color accentColor = Color.FromRgb(76, 175, 80);
    Color hoverColor = Color.FromRgb(100, 150, 200);
    
    // Apply styling
    calendarEdit.Foreground = new SolidColorBrush(Color.FromRgb(0, 0, 0));
    calendarEdit.Background = new SolidColorBrush(Color.FromRgb(245, 245, 245));
    calendarEdit.MouseOverForeground = new SolidColorBrush(Color.FromRgb(255, 255, 255));
    calendarEdit.MouseOverBackground = new SolidColorBrush(hoverColor);
    calendarEdit.HeaderBackground = new SolidColorBrush(primaryColor);
    calendarEdit.HeaderForeground = Brushes.White;
    
    // Animation timing
    calendarEdit.FrameMovingTime = 200;
    calendarEdit.ChangeModeTime = 250;
}
```

### Dark Theme

```csharp
public void ApplyDarkTheme()
{
    calendarEdit.Foreground = Brushes.White;
    calendarEdit.Background = new SolidColorBrush(Color.FromRgb(30, 30, 30));
    calendarEdit.MouseOverForeground = Brushes.Black;
    calendarEdit.MouseOverBackground = new SolidColorBrush(Color.FromRgb(100, 100, 100));
    calendarEdit.HeaderBackground = new SolidColorBrush(Color.FromRgb(50, 50, 50));
    calendarEdit.HeaderForeground = Brushes.White;
}
```

---

## Common Patterns

### Pattern 1: Gradient Background

```csharp
private void ApplyGradientBackground()
{
    LinearGradientBrush gradient = new LinearGradientBrush();
    gradient.StartPoint = new Point(0, 0);
    gradient.EndPoint = new Point(1, 1);
    gradient.GradientStops.Add(new GradientStop(Colors.LightBlue, 0.0));
    gradient.GradientStops.Add(new GradientStop(Colors.White, 1.0));
    
    calendarEdit.Background = gradient;
}
```

### Pattern 2: Accessibility High Contrast

```csharp
public void ApplyHighContrast()
{
    // High contrast for visually impaired users
    calendarEdit.Foreground = Brushes.Black;
    calendarEdit.Background = Brushes.White;
    calendarEdit.MouseOverForeground = Brushes.White;
    calendarEdit.MouseOverBackground = Brushes.Black;
    calendarEdit.MouseOverBorderBrush = Brushes.Yellow;
}
```

### Pattern 3: Dynamic Theme Based on System Settings

```csharp
public void ApplySystemTheme()
{
    // Check if system is in dark mode (Windows 10+)
    var isDarkMode = IsDarkModeEnabled();
    
    if (isDarkMode)
    {
        ApplyDarkTheme();
    }
    else
    {
        ApplyProfessionalTheme();
    }
}

private bool IsDarkModeEnabled()
{
    // Implementation to detect system dark mode
    // This is a placeholder
    return false;
}
```

### Pattern 4: Holiday Styling

Use `BlackoutDates` and visual styling together to mark holidays. See [restrict-dates.md](restrict-dates.md) for blackout-date patterns and examples.

---

## Troubleshooting

### Issue: Colors not applying

**Solution:** Ensure you're using `Brushes` from `System.Windows.Media` namespace. Verify the control is fully loaded before applying colors.

### Issue: RTL not working

**Solution:** Ensure the culture is set correctly. RTL works with `FlowDirection.RightToLeft` and appropriate culture settings.

### Issue: Mouse over effects not showing

**Solution:** Verify `MouseOverForeground`, `MouseOverBackground`, and `MouseOverBorderBrush` are set. These require hover interaction to display.

### Issue: Calendar methods returning unexpected values

**Solution:** Ensure you're passing valid `DateTime` values. Results are returned; the original `calendarEdit.Date` is not modified.

---

## Related Topics

- [Getting Started](getting-started.md) - Initial calendar setup
- [Date Selection](date-selection.md) - Select dates with custom appearance
- [Restrict Date Selection](restrict-dates.md) - Disable dates visually
- [Navigation Support](navigation.md) - Custom header styling and animation

# Date Navigation in CalendarEdit

## Table of Contents
- [Navigation Modes](#navigation-modes)
- [Header-Based Navigation](#header-based-navigation)
- [Keyboard Navigation](#keyboard-navigation)
- [Previous/Next Month Navigation](#previousnext-month-navigation)
- [Customizing Navigation Animation](#customizing-navigation-animation)
- [Customizing Header Appearance](#customizing-header-appearance)
- [Common Patterns](#common-patterns)

---

## Navigation Modes

The CalendarEdit control supports four navigation modes that allow users to view dates at different levels of granularity:

| Mode | View | Example |
|------|------|---------|
| **Day** | Individual dates in a month | March 2024 with all dates |
| **Month** | All months in a year | January through December 2024 |
| **Year** | All years in a decade | 2020-2029 |
| **Decade** | All decades in a century | 2000-2099 |

---

## Header-Based Navigation

### Navigate by Clicking the Header

Users can click the header to cycle through navigation modes:

1. **Click header in Day mode** → Switches to Month mode (shows all months of the year)
2. **Click header in Month mode** → Switches to Year mode (shows all years of the decade)
3. **Click header in Year mode** → Switches to Decade mode (shows all decades)
4. **Click header in Decade mode** → Returns to Year mode

### Example: Navigate to Year 2025

```
Day Mode (March 2024)
    ↓ [Click Header]
Month Mode (Months of 2024)
    ↓ [Click Header]
Year Mode (Years 2020-2029) → [Click 2025]
    ↓ [Auto-select Jan 2025]
Day Mode (January 2025)
```

---

## Keyboard Navigation

### Navigate Between Modes Using Alt+Arrow Keys

| Shortcut | Action |
|----------|--------|
| **Alt + Up Arrow** | Navigate to parent mode (date → month → year → decade) |
| **Alt + Down Arrow** | Navigate to child mode (decade → year → month → date) |
| **Alt + Left Arrow** | Go to previous month (Day mode only) |
| **Alt + Right Arrow** | Go to next month (Day mode only) |

### Example: Navigate from Month View to Year View

```csharp
// Starting in Month mode
// User presses Alt+Up Arrow → Switches to Year mode
```

### Keyboard Example in XAML with Binding

```xaml
<syncfusion:CalendarEdit KeyDown="CalendarEdit_KeyDown"
                         Name="calendarEdit" />
```

```csharp
private void CalendarEdit_KeyDown(object sender, KeyEventArgs e)
{
    // Custom keyboard handling if needed
    if (e.Key == Key.Up && Keyboard.Modifiers == ModifierKeys.Alt)
    {
        // Alt+Up pressed - navigate to parent mode
        e.Handled = true;
    }
}
```

---

## Previous/Next Month Navigation

### Navigate Between Months

Users can navigate to the previous or next month using multiple methods:

#### Method 1: Click Navigation Buttons
- **Click the left arrow (<)** button to go to the previous month
- **Click the right arrow (>)** button to go to the next month

#### Method 2: Keyboard Shortcuts
- Press **Alt + Left Arrow** to go to the previous month
- Press **Alt + Right Arrow** to go to the next month

#### Method 3: Mouse Scroll (in Day Mode)
- **Scroll up** with mouse wheel to go to the previous month
- **Scroll down** with mouse wheel to go to the next month

### Example Code

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Start with a specific month
    calendarEdit.Date = new DateTime(2024, 3, 1);
}

private void NavigateToPreviousMonth()
{
    // Programmatically navigate
    calendarEdit.Date = calendarEdit.Date.AddMonths(-1);
}

private void NavigateToNextMonth()
{
    calendarEdit.Date = calendarEdit.Date.AddMonths(1);
}
```

---

## Customizing Navigation Animation

### Change Mode Navigation Animation Time

The `ChangeModeTime` property controls the animation duration when switching between day/month/year/decade modes:

```xaml
<!-- Disable animation (0 ms) -->
<syncfusion:CalendarEdit ChangeModeTime="0"
                         Name="calendarEdit" />

<!-- 500 ms animation -->
<syncfusion:CalendarEdit ChangeModeTime="500"
                         Name="calendarEdit" />
```

```csharp
// Disable animation
calendarEdit.ChangeModeTime = 0;

// Set 500ms animation
calendarEdit.ChangeModeTime = 500;
```

### Change Month Navigation Animation Time

The `FrameMovingTime` property controls the animation duration when navigating between months:

```xaml
<!-- Smooth month transitions (default is 300 ms) -->
<syncfusion:CalendarEdit FrameMovingTime="300"
                         Name="calendarEdit" />

<!-- Fast transitions -->
<syncfusion:CalendarEdit FrameMovingTime="100"
                         Name="calendarEdit" />

<!-- Instant (no animation) -->
<syncfusion:CalendarEdit FrameMovingTime="0"
                         Name="calendarEdit" />
```

```csharp
// Set animation timing
calendarEdit.FrameMovingTime = 300;  // 300 milliseconds (default)
calendarEdit.ChangeModeTime = 300;   // 300 milliseconds (default)
```

### Performance Tip

For applications requiring smooth, responsive navigation, reduce animation times:

```csharp
calendarEdit.FrameMovingTime = 100;   // Faster month transitions
calendarEdit.ChangeModeTime = 200;    // Faster mode switching
```

---

## Customizing Header Appearance

### Change Header Colors

The header displays the current month/year and navigation buttons. Customize its appearance:

```xaml
<syncfusion:CalendarEdit HeaderBackground="Green"
                         HeaderForeground="Yellow"
                         Name="calendarEdit" />
```

```csharp
using System.Windows.Media;

// Set header background
calendarEdit.HeaderBackground = Brushes.Green;

// Set header text color
calendarEdit.HeaderForeground = Brushes.Yellow;
```

### Header Property Reference

| Property | Type | Description |
|----------|------|-------------|
| `HeaderBackground` | Brush | Background color of the header (default: Lavender) |
| `HeaderForeground` | Brush | Text color of the header (default: Dark SlateGray) |
| `ChangeModeTime` | int | Animation duration when changing modes (ms) |
| `FrameMovingTime` | int | Animation duration for month transitions (ms) |

### Example: Professional Header Styling

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Create a professional look
    calendarEdit.HeaderBackground = new SolidColorBrush(Color.FromRgb(45, 125, 154));
    calendarEdit.HeaderForeground = Brushes.White;
    calendarEdit.FrameMovingTime = 200;
    calendarEdit.ChangeModeTime = 250;
}
```

---

## Common Patterns

### Pattern 1: Quick Year Selection

```csharp
private void QuickSelectYear()
{
    // Navigate to year selection mode
    // User clicks header twice to reach Year mode
    // Then clicks desired year (e.g., 2025)
    // User can quickly jump to any year
    
    calendarEdit.Date = new DateTime(2025, 1, 1);
    MessageBox.Show("Navigated to January 2025");
}
```

### Pattern 2: Disable Animations for Accessibility

```xaml
<syncfusion:CalendarEdit ChangeModeTime="0"
                         FrameMovingTime="0"
                         Name="calendarEdit" />
```

Users with motion sensitivity appreciate the lack of animation.

### Pattern 3: Navigate to a Specific Date

```csharp
private void NavigateToDate(int year, int month, int day)
{
    // Directly jump to a specific date
    calendarEdit.Date = new DateTime(year, month, day);
}

// Usage
NavigateToDate(2024, 12, 25);  // Navigate to Christmas 2024
```

### Pattern 4: Month Navigation Buttons

```xaml
<StackPanel Orientation="Horizontal">
    <Button Click="PreviousMonth_Click">← Previous</Button>
    <syncfusion:CalendarEdit Name="calendarEdit" />
    <Button Click="NextMonth_Click">Next →</Button>
</StackPanel>
```

```csharp
private void PreviousMonth_Click(object sender, RoutedEventArgs e)
{
    calendarEdit.Date = calendarEdit.Date.AddMonths(-1);
}

private void NextMonth_Click(object sender, RoutedEventArgs e)
{
    calendarEdit.Date = calendarEdit.Date.AddMonths(1);
}
```

### Pattern 5: Navigate to Today

```csharp
private void NavigateToToday()
{
    calendarEdit.Date = DateTime.Today;
}
```

---

## Troubleshooting

### Issue: Keyboard shortcuts not working

**Solution:** Ensure the CalendarEdit control has focus. Click on the calendar before using Alt+arrow keys.

### Issue: Animation too slow or fast

**Solution:** Adjust `FrameMovingTime` and `ChangeModeTime` properties to desired millisecond values.

### Issue: Header not showing custom colors

**Solution:** Verify that `HeaderBackground` and `HeaderForeground` are applied after control initialization and the theme doesn't override these properties.

---

## Related Topics

- [Date Selection](date-selection.md) - Select dates at different granularity levels
- [Restrict Date Selection](restrict-dates.md) - Limit navigation to specific date ranges
- [Appearance Customization](appearance.md) - Customize calendar styling

# Advanced Configuration in WPF NumericUpdown

## Step Interval Management

### Step Property

The Step property controls the increment/decrement value when spin buttons are clicked or keyboard/arrow keys are pressed.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  Height="25" 
                  Width="100" 
                  Value="10" 
                  Step="5" />
```

**C#:**
```csharp
updown.Value = 10;
updown.Step = 5;
```

### Step Value Behavior

When user clicks increment button:
- Current value: 10, Step: 5 → New value: 15
- Current value: 15, Step: 5 → New value: 20

When user clicks decrement button:
- Current value: 20, Step: 5 → New value: 15
- Current value: 15, Step: 5 → New value: 10

### Common Step Intervals

| Use Case | Step Value | Example Range |
|----------|-----------|---|
| Percentage | 1 | 0-100 |
| Price (cents) | 0.01 | 0.00-999.99 |
| Temperature | 0.5 | -40 to 50 |
| Quantity (dozen) | 12 | 0-1000 |
| Time (15 min) | 15 | 0-1440 |
| Volume (mL) | 50 | 0-5000 |

### Step Examples

**Percentage Stepper (1% increments):**
```csharp
updown.MinValue = 0;
updown.MaxValue = 100;
updown.Step = 1;
updown.NumberDecimalDigits = 0;
```

**Price Stepper (cent increments):**
```csharp
updown.Step = 0.01;
updown.NumberDecimalDigits = 2;
```

**Quantity in Dozens:**
```csharp
updown.Step = 12;
updown.NumberDecimalDigits = 0;
updown.MinValue = 12;
```

**Temperature (half-degree steps):**
```csharp
updown.Step = 0.5;
updown.MinValue = -50;
updown.MaxValue = 50;
updown.NumberDecimalDigits = 1;
```

## Keyboard and Mouse Interactions

### Default Interaction Patterns

**Mouse Interactions:**
- **Click increment button**: Add Step value
- **Click decrement button**: Subtract Step value
- **Mouse wheel**: (if control has focus) Increment/decrement by Step

**Keyboard Interactions:**
- **Up arrow**: Add Step value
- **Down arrow**: Subtract Step value
- **Ctrl+Up**: Increment to MaxValue (or by large step)
- **Ctrl+Down**: Decrement to MinValue (or by large step)
- **Type directly**: If AllowEdit = True

### Enabling/Disabling Edit

**Allow direct keyboard input:**
```csharp
updown.AllowEdit = true;  // Default
// User can type: "42"
```

**Keyboard-only via spin buttons:**
```csharp
updown.AllowEdit = false;
// User must use Up/Down arrows or mouse buttons
```

### Mouse Wheel Support

The mouse wheel works when UpDown has keyboard focus:

```csharp
// Works automatically with UpDown
updown.Focus();  // Give focus to enable mouse wheel

// Mouse wheel scroll increments/decrements by Step
```

**Behavior:**
- Mouse wheel forward: Add Step
- Mouse wheel backward: Subtract Step
- Value respects MinValue/MaxValue constraints

### Example: Keyboard-Optimized UpDown

```csharp
public void SetupKeyboardOptimized()
{
    updown.Step = 1;
    updown.AllowEdit = false;  // Keyboard buttons only
    updown.MinValue = 0;
    updown.MaxValue = 100;
    
    // User can now:
    // - Click spin buttons
    // - Press Up/Down arrows
    // - Scroll mouse wheel
    // Cannot directly type
}
```

## Animation Support

### Animation in Value Changes

The UpDown control can display smooth animations when values change via spin buttons.

### Enabling Animation

Animation is typically controlled through styling or by the theme applied:

```csharp
// Animation is enabled by default with most themes
// Apply a theme that includes animation
SfSkinManager.SetVisualStyle(updown, VisualStyles.MaterialLight);
```

### Animation Characteristics

- **Duration**: Typically 300-500ms (theme-dependent)
- **Easing**: Smooth easing function (theme-dependent)
- **Trigger**: Occurs on spin button click
- **Effect**: Value transitions smoothly rather than jumping

### Disabling Animation

To disable animations, apply a theme without animation or use custom styling:

```csharp
// Custom style without animations
var noAnimStyle = new Style();
noAnimStyle.Setters.Add(new Setter(FrameworkElement.HeightProperty, 25.0));

// Or apply minimal theme
SfSkinManager.SetVisualStyle(updown, VisualStyles.Office2019Colorful);
```

## Culture-Specific Behaviors

### Culture Property Effects

Culture affects multiple behaviors beyond just number formatting:

**C#:**
```csharp
// US English
updown.Culture = new CultureInfo("en-US");
// Decimal: . | Separator: , | RTL: false

// Arabic (RTL - Right-to-Left)
updown.Culture = new CultureInfo("ar-SA");
// Decimal: ، | Separator: . | RTL: true

// German
updown.Culture = new CultureInfo("de-DE");
// Decimal: , | Separator: . | RTL: false
```

### Culture Settings Impact

| Culture | Decimal Sep | Group Sep | RTL | Note |
|---------|-------------|-----------|-----|------|
| en-US | . | , | No | Standard Western |
| de-DE | , | . | No | European |
| ar-SA | ، | . | Yes | Right-to-left |
| ja-JP | 。 | , | No | Japanese |
| zh-CN | 。 | , | No | Chinese |

### RTL (Right-to-Left) Cultures

For cultures that read right-to-left, the UpDown automatically adjusts:

```csharp
// Arabic - automatically adjusts layout
updown.Culture = new CultureInfo("ar-SA");

// Spin buttons move to left side
// Text alignment reverses
// Number display direction reverses
```

### Multi-Culture Support Example

```csharp
public void ApplyCulture(string cultureCode)
{
    var culture = new CultureInfo(cultureCode);
    updown.Culture = culture;
    
    // Settings automatically adjust:
    // - Number format
    // - Text alignment
    // - RTL/LTR layout
    // - Decimal/group separators
}

// Usage:
ApplyCulture("en-US");    // English - LTR
ApplyCulture("ar-SA");    // Arabic - RTL
ApplyCulture("de-DE");    // German - LTR
ApplyCulture("ja-JP");    // Japanese - LTR
```

## Complex Use Cases

### Use Case 1: Financial Calculator Input

```csharp
public void ConfigureFinancialInput()
{
    updown.MinValue = 0;
    updown.MaxValue = 999999999;
    updown.Step = 0.01;
    updown.NumberDecimalDigits = 2;
    updown.GroupSeperatorEnabled = true;
    updown.Culture = new CultureInfo("en-US");
    updown.TextAlignment = TextAlignment.Right;
    
    // Styling
    updown.Background = Brushes.LightGray;
    updown.Foreground = Brushes.Black;
    updown.EnableNegativeColors = true;
    updown.NegativeForeground = Brushes.Red;
}

// Result: Professional financial input field
// Display: 1,234,567.89 (positive, formatted)
// Display: -1,234,567.89 (negative, red text)
```

### Use Case 2: Age Validator with Null Support

```csharp
public void ConfigureAgeInput()
{
    updown.MinValue = 0;
    updown.MaxValue = 150;
    updown.NumberDecimalDigits = 0;
    updown.Step = 1;
    updown.AllowEdit = true;
    
    // Null handling
    updown.UseNullOption = true;
    updown.NullValueText = "Not specified";
    updown.Value = null;
    
    // Validation
    updown.MinValidation = MinValidation.OnKeyPress;
    updown.MaxValidation = MaxValidation.OnKeyPress;
    updown.ValueChanging += (s, e) =>
    {
        if (e.NewValue != null && (double)e.NewValue < 18)
        {
            MessageBox.Show("Must be 18 or older");
            e.Cancel = true;
        }
    };
}

// Result: Optional age field, 18+, watermark text
```

### Use Case 3: Temperature Monitor (Negative Values)

```csharp
public void ConfigureTemperatureMonitor()
{
    updown.MinValue = -273;  // Absolute zero
    updown.MaxValue = 1000;
    updown.Step = 0.1;
    updown.NumberDecimalDigits = 1;
    updown.AllowEdit = false;  // Read-only, spin-button only
    
    // Color coding
    updown.Background = Brushes.LightBlue;
    updown.Foreground = Brushes.DarkBlue;
    updown.EnableNegativeColors = true;
    updown.NegativeBackground = Brushes.LightCyan;
    updown.NegativeForeground = Brushes.DarkCyan;
    updown.ApplyZeroColor = true;
    updown.ZeroColor = Brushes.Green;  // Freezing point
    
    // Event handlers
    updown.ValueChanged += (d, e) =>
    {
        double temp = (double)e.NewValue;
        if (temp > 100) warningLabel.Text = "Hot!";
        else if (temp < 0) warningLabel.Text = "Frozen!";
        else warningLabel.Text = "Normal";
    };
}

// Result: Temperature display with visual feedback
```

### Use Case 4: Percentage with Localization

```csharp
public class LocalizedPercentageInput
{
    public void ConfigurePercentage(string culture)
    {
        updown.MinValue = 0;
        updown.MaxValue = 100;
        updown.Step = 1;
        updown.NumberDecimalDigits = 0;
        updown.Culture = new CultureInfo(culture);
        updown.GroupSeperatorEnabled = true;
        updown.TextAlignment = TextAlignment.Right;
    }
    
    public void Demo()
    {
        var updown = new UpDown { Value = 75 };
        
        // English: 75
        ConfigurePercentage("en-US");
        
        // German: 75 (format same for percentage)
        ConfigurePercentage("de-DE");
        
        // Arabic: 75 (displayed RTL)
        ConfigurePercentage("ar-SA");
    }
}

// Result: 75% in any culture, properly formatted
```

### Use Case 5: Time Duration Input (Composite)

```csharp
public class TimeDurationInput
{
    public UpDown HoursUpDown { get; set; }
    public UpDown MinutesUpDown { get; set; }
    
    public void ConfigureDurationInputs()
    {
        // Hours (0-23)
        HoursUpDown.MinValue = 0;
        HoursUpDown.MaxValue = 23;
        HoursUpDown.Step = 1;
        HoursUpDown.NumberDecimalDigits = 0;
        
        // Minutes (0-59)
        MinutesUpDown.MinValue = 0;
        MinutesUpDown.MaxValue = 59;
        MinutesUpDown.Step = 15;  // 15-minute increments
        MinutesUpDown.NumberDecimalDigits = 0;
    }
    
    public TimeSpan GetDuration()
    {
        int hours = (int)(double)HoursUpDown.Value;
        int minutes = (int)(double)MinutesUpDown.Value;
        return new TimeSpan(hours, minutes, 0);
    }
    
    public void SetDuration(TimeSpan duration)
    {
        HoursUpDown.Value = duration.Hours;
        MinutesUpDown.Value = duration.Minutes;
    }
}

// Usage:
var input = new TimeDurationInput();
input.ConfigureDurationInputs();
input.SetDuration(new TimeSpan(2, 30, 0));  // 2 hours 30 minutes
```

### Use Case 6: Scoring System (Restricted Choices)

```csharp
public void ConfigureScoringSystem()
{
    updown.MinValue = 1;
    updown.MaxValue = 5;
    updown.Step = 1;
    updown.NumberDecimalDigits = 0;
    updown.AllowEdit = false;  // Buttons only
    
    // Color gradient: Red (1) to Green (5)
    updown.ValueChanged += (d, e) =>
    {
        int score = (int)(double)e.NewValue;
        switch (score)
        {
            case 1:
                updown.Background = Brushes.Red;
                updown.Foreground = Brushes.White;
                break;
            case 2:
                updown.Background = Brushes.Orange;
                updown.Foreground = Brushes.White;
                break;
            case 3:
                updown.Background = Brushes.Yellow;
                updown.Foreground = Brushes.Black;
                break;
            case 4:
                updown.Background = Brushes.LightGreen;
                updown.Foreground = Brushes.Black;
                break;
            case 5:
                updown.Background = Brushes.Green;
                updown.Foreground = Brushes.White;
                break;
        }
    };
}

// Result: 1-5 star rating display with color feedback
```

---

**Skill Complete:** All major UpDown features covered. For additional scenarios, combine concepts from this and other reference files.

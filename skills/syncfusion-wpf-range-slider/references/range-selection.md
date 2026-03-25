# Range Selection Features

The WPF Range Slider supports both single value selection and range selection with dual thumbs. This guide covers all range selection features including ShowRange, RangeStart, RangeEnd, AllowRangeDrag, and ThumbInterval properties.

## Table of Contents
- [ShowRange Property](#showrange-property)
- [RangeStart and RangeEnd](#rangestart-and-rangeend)
- [AllowRangeDrag Feature](#allowrangedrag-feature)
- [ThumbInterval](#thumbinterval)
- [Single vs Dual Thumb Mode](#single-vs-dual-thumb-mode)
- [Data Binding](#data-binding)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## ShowRange Property

The `ShowRange` property enables dual thumb mode, allowing users to select a range of values instead of a single value.

### Basic Usage

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    RangeStart="40"
    RangeEnd="70"
    ShowRange="True" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 400,
    Minimum = 0,
    Maximum = 100,
    ShowRange = true,
    RangeStart = 40,
    RangeEnd = 70
};
```

**When `ShowRange="True"`:**
- Two thumbs appear on the track
- Left thumb controls `RangeStart`
- Right thumb controls `RangeEnd`
- Both tooltips display their respective values
- The track between thumbs is highlighted

**When `ShowRange="False"` (default):**
- Single thumb for `Value` property
- Standard single-value slider behavior

## RangeStart and RangeEnd

These properties define the selected range when `ShowRange` is enabled.

### RangeStart

Gets or sets the start value of the selected range (left thumb position).

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    RangeStart="30"
    RangeEnd="70"
    ShowRange="True" />
```

```csharp
rangeSlider.RangeStart = 30;
```

**Constraints:**
- Must be >= `Minimum`
- Must be < `RangeEnd` (when ThumbInterval is 0)
- Must be <= `RangeEnd - ThumbInterval` (when ThumbInterval > 0)

### RangeEnd

Gets or sets the end value of the selected range (right thumb position).

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    RangeStart="30"
    RangeEnd="70"
    ShowRange="True" />
```

```csharp
rangeSlider.RangeEnd = 70;
```

**Constraints:**
- Must be <= `Maximum`
- Must be > `RangeStart` (when ThumbInterval is 0)
- Must be >= `RangeStart + ThumbInterval` (when ThumbInterval > 0)

### Setting Both Values

```csharp
// Set range programmatically
rangeSlider.RangeStart = 25;
rangeSlider.RangeEnd = 75;

// Or use object initializer
SfRangeSlider slider = new SfRangeSlider()
{
    Minimum = 0,
    Maximum = 100,
    ShowRange = true,
    RangeStart = 25,
    RangeEnd = 75
};
```

## AllowRangeDrag Feature

The `AllowRangeDrag` property allows users to drag the entire selected range without changing the range size.

### Enabling Range Dragging

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    RangeStart="20"
    RangeEnd="40"
    ShowRange="True"
    AllowRangeDrag="True"
    ShowValueLabels="True"
    TickFrequency="20" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 400,
    Minimum = 0,
    Maximum = 100,
    ShowRange = true,
    RangeStart = 20,
    RangeEnd = 40,
    AllowRangeDrag = true,
    ShowValueLabels = true,
    TickFrequency = 20
};
```

**Behavior:**
- Click and drag anywhere between the two thumbs
- The entire range moves together
- Range size (RangeEnd - RangeStart) remains constant
- Both RangeStart and RangeEnd update simultaneously
- Cannot drag beyond Minimum or Maximum boundaries

**Default:** `false`

### Use Cases for AllowRangeDrag

1. **Time Window Selection** - Move a fixed time window across a timeline
2. **Budget Range** - Shift a budget range while maintaining the same spread
3. **Temperature Range** - Adjust a temperature comfort zone
4. **Date Range Picker** - Move a fixed-duration date range

### Example: Price Range Filter with Dragging

```xaml
<StackPanel>
    <TextBlock Text="Price Range Filter" FontWeight="Bold" Margin="0,0,0,10"/>
    <TextBlock x:Name="PriceRangeText" Margin="0,0,0,10"/>
    
    <editors:SfRangeSlider
        x:Name="PriceRangeSlider"
        Width="400"
        Minimum="0"
        Maximum="1000"
        RangeStart="200"
        RangeEnd="500"
        ShowRange="True"
        AllowRangeDrag="True"
        ToolTipFormat="C0"
        ShowValueLabels="True"
        TickFrequency="100"
        TickPlacement="BottomRight"
        RangeChanged="PriceRangeSlider_RangeChanged" />
</StackPanel>
```

```csharp
private void PriceRangeSlider_RangeChanged(object sender, RangeChangedEventArgs e)
{
    PriceRangeText.Text = $"Showing items from {e.NewStartValue:C0} to {e.NewEndValue:C0}";
}
```

## ThumbInterval

The `ThumbInterval` property sets the minimum spacing between the two thumbs, preventing them from getting too close.

### Basic Usage

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    RangeStart="20"
    RangeEnd="60"
    ShowRange="True"
    ThumbInterval="10" />
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 400,
    Minimum = 0,
    Maximum = 100,
    ShowRange = true,
    RangeStart = 20,
    RangeEnd = 60,
    ThumbInterval = 10  // Thumbs must be at least 10 units apart
};
```

**Behavior:**
- Enforces minimum distance between RangeStart and RangeEnd
- `RangeEnd` must always be >= `RangeStart + ThumbInterval`
- Prevents invalid range selections
- Useful for ensuring meaningful range selections

**Default:** 0 (no minimum spacing)

### Use Cases for ThumbInterval

1. **Age Range Filter** - Ensure at least 5-year age ranges
2. **Date Range** - Require minimum 7-day periods
3. **Price Range** - Maintain minimum $50 spread
4. **Measurement Ranges** - Ensure statistically significant ranges

### Example: Age Range with Minimum Spread

```xaml
<StackPanel>
    <TextBlock Text="Age Range (minimum 10-year spread)" FontWeight="Bold" Margin="0,0,0,10"/>
    
    <editors:SfRangeSlider
        Width="400"
        Minimum="18"
        Maximum="100"
        RangeStart="25"
        RangeEnd="45"
        ShowRange="True"
        ThumbInterval="10"
        ShowValueLabels="True"
        TickFrequency="10"
        TickPlacement="BottomRight" />
</StackPanel>
```

### Combining ThumbInterval with AllowRangeDrag

```xaml
<!-- Fixed 20-unit range that can be dragged -->
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    RangeStart="30"
    RangeEnd="50"
    ShowRange="True"
    ThumbInterval="20"
    AllowRangeDrag="True"
    ShowValueLabels="True" />
```

This creates a fixed 20-unit range that maintains its size while being dragged.

## Single vs Dual Thumb Mode

### Single Thumb Mode (Default)

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    Value="50"
    ShowRange="False" />
```

**Characteristics:**
- One thumb control
- Uses `Value` property
- Standard slider behavior
- Tooltip shows single value

### Dual Thumb Mode

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="100"
    RangeStart="30"
    RangeEnd="70"
    ShowRange="True" />
```

**Characteristics:**
- Two thumb controls
- Uses `RangeStart` and `RangeEnd` properties
- Highlighted track between thumbs
- Two tooltips showing both values

### Switching Between Modes

```csharp
// Switch to dual thumb mode
rangeSlider.ShowRange = true;
rangeSlider.RangeStart = 30;
rangeSlider.RangeEnd = 70;

// Switch back to single thumb mode
rangeSlider.ShowRange = false;
rangeSlider.Value = 50;
```

## Data Binding

### Binding to ViewModel Properties

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="{Binding MinValue}"
    Maximum="{Binding MaxValue}"
    RangeStart="{Binding SelectedStart, Mode=TwoWay}"
    RangeEnd="{Binding SelectedEnd, Mode=TwoWay}"
    ShowRange="True" />
```

```csharp
public class RangeViewModel : INotifyPropertyChanged
{
    private double minValue = 0;
    private double maxValue = 100;
    private double selectedStart = 25;
    private double selectedEnd = 75;

    public double MinValue
    {
        get => minValue;
        set { minValue = value; OnPropertyChanged(); }
    }

    public double MaxValue
    {
        get => maxValue;
        set { maxValue = value; OnPropertyChanged(); }
    }

    public double SelectedStart
    {
        get => selectedStart;
        set { selectedStart = value; OnPropertyChanged(); }
    }

    public double SelectedEnd
    {
        get => selectedEnd;
        set { selectedEnd = value; OnPropertyChanged(); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Binding with Validation

```csharp
private double selectedStart;
public double SelectedStart
{
    get => selectedStart;
    set
    {
        // Validate before setting
        if (value >= MinValue && value < SelectedEnd)
        {
            selectedStart = value;
            OnPropertyChanged();
        }
    }
}

private double selectedEnd;
public double SelectedEnd
{
    get => selectedEnd;
    set
    {
        // Validate before setting
        if (value <= MaxValue && value > SelectedStart)
        {
            selectedEnd = value;
            OnPropertyChanged();
        }
    }
}
```

## Common Scenarios

### Scenario 1: Product Price Filter

```xaml
<editors:SfRangeSlider
    Width="450"
    Minimum="0"
    Maximum="5000"
    RangeStart="500"
    RangeEnd="2000"
    ShowRange="True"
    AllowRangeDrag="True"
    ToolTipFormat="C0"
    ShowValueLabels="True"
    TickFrequency="500"
    TickPlacement="BottomRight"
    ThumbInterval="100" />
```

### Scenario 2: Date Range Selection (using numeric values)

```xaml
<editors:SfRangeSlider
    Width="450"
    Minimum="0"
    Maximum="365"
    RangeStart="90"
    RangeEnd="180"
    ShowRange="True"
    AllowRangeDrag="True"
    ShowCustomLabels="True"
    TickFrequency="30"
    TickPlacement="BottomRight"
    ThumbInterval="7">
    <editors:SfRangeSlider.CustomLabels>
        <editors:LabelItem Content="Jan" Position="0"/>
        <editors:LabelItem Content="Apr" Position="90"/>
        <editors:LabelItem Content="Jul" Position="180"/>
        <editors:LabelItem Content="Oct" Position="270"/>
        <editors:LabelItem Content="Dec" Position="365"/>
    </editors:SfRangeSlider.CustomLabels>
</editors:SfRangeSlider>
```

### Scenario 3: Temperature Range Control

```xaml
<editors:SfRangeSlider
    Width="350"
    Minimum="-20"
    Maximum="50"
    RangeStart="18"
    RangeEnd="26"
    ShowRange="True"
    AllowRangeDrag="True"
    ShowValueLabels="True"
    ToolTipFormat="N0"
    TickFrequency="10"
    TickPlacement="BottomRight"
    ThumbInterval="2" />
```

### Scenario 4: Time Range (Minutes)

```xaml
<editors:SfRangeSlider
    Width="400"
    Minimum="0"
    Maximum="1440"
    RangeStart="480"
    RangeEnd="1020"
    ShowRange="True"
    AllowRangeDrag="True"
    ShowValueLabels="True"
    TickFrequency="240"
    TickPlacement="BottomRight"
    ThumbInterval="30"
    RangeChanged="TimeRangeSlider_RangeChanged" />
```

```csharp
private void TimeRangeSlider_RangeChanged(object sender, RangeChangedEventArgs e)
{
    // Convert minutes to time
    TimeSpan startTime = TimeSpan.FromMinutes(e.NewStartValue);
    TimeSpan endTime = TimeSpan.FromMinutes(e.NewEndValue);
    
    string formattedRange = $"{startTime:hh\\:mm} - {endTime:hh\\:mm}";
    // Display or use the formatted range
}
```

## Best Practices

### 1. Always Set ShowRange for Dual Thumbs

```xaml
<!-- Correct -->
<editors:SfRangeSlider ShowRange="True" RangeStart="30" RangeEnd="70" />

<!-- Won't work as expected -->
<editors:SfRangeSlider RangeStart="30" RangeEnd="70" />
```

### 2. Validate Range Values

Ensure RangeStart < RangeEnd when setting programmatically:

```csharp
public void SetRange(double start, double end)
{
    if (start >= end)
    {
        throw new ArgumentException("RangeStart must be less than RangeEnd");
    }
    
    if (start < rangeSlider.Minimum || end > rangeSlider.Maximum)
    {
        throw new ArgumentException("Range values must be within Minimum and Maximum");
    }
    
    rangeSlider.RangeStart = start;
    rangeSlider.RangeEnd = end;
}
```

### 3. Use ThumbInterval for Meaningful Ranges

Set ThumbInterval to prevent selecting ranges that are too narrow to be useful:

```xaml
<!-- Prevent price ranges less than $50 -->
<editors:SfRangeSlider Minimum="0" Maximum="1000" ThumbInterval="50" ShowRange="True" />
```

### 4. Consider AllowRangeDrag for Fixed-Size Ranges

When users need to shift a fixed-size window, enable range dragging:

```xaml
<editors:SfRangeSlider AllowRangeDrag="True" ThumbInterval="20" ShowRange="True" />
```

### 5. Bind to ViewModel for MVVM

Use two-way binding for reactive UI updates:

```xaml
<editors:SfRangeSlider
    RangeStart="{Binding StartValue, Mode=TwoWay}"
    RangeEnd="{Binding EndValue, Mode=TwoWay}"
    ShowRange="True" />
```

### 6. Handle RangeChanged Events

React to range selections in your application logic:

```csharp
rangeSlider.RangeChanged += (sender, e) =>
{
    // Filter data, update UI, etc.
    FilterData(e.NewStartValue, e.NewEndValue);
};
```

## Troubleshooting

### Issue: RangeStart and RangeEnd Don't Appear

**Solution:** Ensure `ShowRange="True"` is set.

### Issue: Thumbs Won't Move Closer

**Solution:** Check `ThumbInterval` property - it may be preventing closer positioning.

### Issue: Range Can't Be Dragged

**Solution:** Set `AllowRangeDrag="True"` to enable range dragging.

### Issue: Invalid Range Values

**Solution:** Validate that:
- `RangeStart` >= `Minimum`
- `RangeEnd` <= `Maximum`
- `RangeEnd` > `RangeStart` + `ThumbInterval`

## Next Steps

- **Ticks and Snapping** - Read `ticks-and-snapping.md` for tick marks and snap behavior
- **Labels** - Read `labels.md` for custom labels and value display
- **Events** - Read `events.md` for handling range change events
- **Tooltips** - Read `tooltips.md` for tooltip customization

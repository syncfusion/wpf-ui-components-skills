# Precision in WPF Rating (SfRating)

The precision mode defines the accuracy level of the SfRating control, determining how rating values are rounded and displayed. It supports three modes: Standard, Half, and Exact.

## Overview

The `Precision` property controls how precisely users can rate items:

- **Standard** - Full item increments only (1, 2, 3, 4, 5)
- **Half** - Half item increments (0.5, 1.0, 1.5, 2.0, etc.)
- **Exact** - Any decimal value (precise rating)

The default precision mode is `Standard`.

## Standard Precision

When the precision mode is set to `Standard`, the rating item fills completely based on the rating value. Users can only select whole number ratings.

### Use Cases for Standard Precision

- Simple 1-5 star ratings
- Binary or discrete rating scales
- When granular feedback is not needed
- Quick rating interactions

### Implementation

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     Precision="Standard"
                     Value="3"/>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ItemsCount = 5;
rating.Precision = Syncfusion.Windows.Primitives.Precision.Standard;
rating.Value = 3;
this.Content = rating;
```

### Behavior

With Standard precision:
- Clicking on any part of a rating item selects that entire item
- Value is always a whole number (1.0, 2.0, 3.0, etc.)
- Visual feedback shows complete item fill
- No partial fills are displayed

**Example:** Clicking anywhere on the 3rd star sets Value to 3.0

## Half Precision

When the precision mode is set to `Half`, the rating item fills partially based on the rating value. Users can select half-star ratings.

### Use Cases for Half Precision

- Product reviews with more granular feedback
- Displaying average ratings that fall between whole numbers
- When users need moderate precision without overwhelming detail
- Common in e-commerce and review platforms

### Implementation

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     Precision="Half"
                     Value="3.5"/>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ItemsCount = 5;
rating.Precision = Syncfusion.Windows.Primitives.Precision.Half;
rating.Value = 3.5;
this.Content = rating;
```

### Behavior

With Half precision:
- Clicking the left half of an item selects that half (e.g., 3.5)
- Clicking the right half selects the full item (e.g., 4.0)
- Value is rounded to nearest 0.5 (e.g., 2.5, 3.0, 3.5, 4.0)
- Visual feedback shows half-filled items for .5 values

**Example:** 
- Clicking left side of 4th star → Value = 3.5
- Clicking right side of 4th star → Value = 4.0

### Displaying Decimal Averages

Half precision is ideal for showing calculated average ratings:

```csharp
// Calculate average rating from database
double averageRating = CalculateAverageRating(); // Returns 4.3

// Display with half-star precision (rounds to 4.5)
SfRating displayRating = new SfRating()
{
    ItemsCount = 5,
    Precision = Syncfusion.Windows.Primitives.Precision.Half,
    Value = averageRating,
    IsReadOnly = true  // Display only, no user interaction
};
```

## Exact Precision

When the precision mode is set to `Exact`, the rating item fills exactly based on the rating value. Users can select any decimal value by clicking at precise positions.

### Use Cases for Exact Precision

- Displaying calculated ratings with high precision
- Data-driven ratings from analytics
- Scientific or technical rating displays
- When bound to precise decimal values
- Showing aggregated ratings

### Implementation

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     Precision="Exact"
                     Value="3.7"/>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.ItemsCount = 5;
rating.Precision = Syncfusion.Windows.Primitives.Precision.Exact;
rating.Value = 3.7;
this.Content = rating;
```

### Behavior

With Exact precision:
- Clicking anywhere on an item sets the precise value based on position
- Value can be any decimal between 0 and ItemsCount
- Visual feedback shows precise partial fills
- Mouse position determines exact rating value

**Example:** 
- Clicking 70% across the 4th star → Value ≈ 3.7
- Clicking 20% across the 2nd star → Value ≈ 1.2

### Displaying Precise Calculated Values

Exact precision is perfect for analytics dashboards and aggregated data:

```csharp
// Precise calculated rating
double preciseRating = 4.37; // From database or calculation

SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Precision = Syncfusion.Windows.Primitives.Precision.Exact,
    Value = preciseRating,
    IsReadOnly = true,  // Usually read-only for display
    ShowToolTip = true,
    AutoToolTipPrecision = 2  // Show 2 decimal places in tooltip
};
```

## Choosing the Right Precision

| Scenario | Recommended Precision | Reason |
|----------|----------------------|---------|
| User feedback collection | Standard or Half | Easy interaction, clear choices |
| Displaying averages (read-only) | Half or Exact | Shows calculated values accurately |
| Simple yes/no ratings | Standard | Clear, unambiguous selection |
| E-commerce product ratings | Half | Industry standard, familiar to users |
| Data visualization | Exact | Preserves precision of data |
| Quick surveys | Standard | Fastest user interaction |
| Professional reviews | Half | Balanced granularity |

## Precision with Data Binding

Precision modes work seamlessly with data binding:

```xml
<syncfusion:SfRating ItemsCount="5"
                     Precision="Half"
                     Value="{Binding ProductRating, Mode=TwoWay}"/>
```

```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    private double _productRating;
    
    public double ProductRating
    {
        get => _productRating;
        set
        {
            _productRating = value;
            OnPropertyChanged(nameof(ProductRating));
        }
    }
    
    // INotifyPropertyChanged implementation
}
```

The bound value automatically respects the Precision setting:
- Standard: Rounds to whole numbers
- Half: Rounds to nearest 0.5
- Exact: Preserves full decimal value

## Combining Precision with Read-Only Mode

Common pattern for displaying non-editable precise ratings:

```xml
<!-- Show exact average rating, not editable -->
<syncfusion:SfRating ItemsCount="5"
                     Precision="Exact"
                     Value="4.37"
                     IsReadOnly="True"
                     ShowToolTip="True"
                     AutoToolTipPrecision="2"/>
```

This displays the precise value (4.37) visually, shows it in the tooltip with 2 decimal places, and prevents user interaction.

## Best Practices

1. **User Input Ratings**: Use Standard or Half precision for the most intuitive user experience
2. **Displaying Calculated Data**: Use Half or Exact with IsReadOnly="True"
3. **Consistency**: Maintain the same precision mode across similar rating controls in your application
4. **Tooltips with Exact**: Always enable ShowToolTip when using Exact precision to show the numeric value
5. **Performance**: All precision modes have similar performance; choose based on user experience needs
6. **Accessibility**: Standard precision is easiest for keyboard navigation and screen readers

## Edge Cases and Considerations

### Setting Invalid Values

The SfRating control handles values outside the valid range:

```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 7  // Exceeds ItemsCount
};
// Value is capped at 5.0
```

Values are automatically clamped to the range [0, ItemsCount].

### Precision Changes with Existing Values

Changing precision mode automatically adjusts the current value:

```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Precision = Syncfusion.Windows.Primitives.Precision.Exact,
    Value = 3.7
};

// Change to Half precision
rating.Precision = Syncfusion.Windows.Primitives.Precision.Half;
// Value automatically rounds to 3.5
```

### Programmatic Value Setting

You can set any decimal value programmatically, regardless of precision mode:

```csharp
rating.Value = 3.7;  // Sets exactly 3.7
```

The precision mode only affects user interaction; programmatic setting always works.

# Appearance and Styling in WPF Rating (SfRating)

The SfRating control provides extensive customization options for appearance and styling. This guide covers fill colors, stroke properties, sizing, spacing, hover effects, and advanced styling techniques.

## Table of Contents
- [Overview](#overview)
- [Fill Colors](#fill-colors)
  - [Rated Items (Selected)](#rated-items-selected)
  - [Unrated Items (Unselected)](#unrated-items-unselected)
- [Stroke Properties](#stroke-properties)
  - [Stroke Colors](#stroke-colors)
  - [Stroke Thickness](#stroke-thickness)
- [Mouse Hover Effects](#mouse-hover-effects)
  - [PointerOverFill](#pointeroverfill)
  - [PointerOverStroke](#pointeroverstroke)
  - [PointerOverStrokeThickness](#pointeroverstrokethickness)
- [Size Customization](#size-customization)
  - [Item Size](#item-size)
  - [Individual Heights](#individual-heights)
  - [Uniform Heights](#uniform-heights)
- [Spacing and Layout](#spacing-and-layout)
  - [Items Spacing](#items-spacing)
  - [Corner Radius](#corner-radius)
- [Preview Value](#preview-value)
- [ItemContainerStyle](#itemcontainerstyle)
- [Complete Styling Examples](#complete-styling-examples)


## Overview

The SfRating control's styling properties are set through the `ItemContainerStyle` property, which targets individual `SfRatingItem` objects. This pattern allows consistent styling across all rating items.

**Basic styling structure:**
```xml
<syncfusion:SfRating ItemsCount="5" Value="3">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="PropertyName" Value="PropertyValue"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

## Fill Colors

Fill colors determine the appearance of rated (selected) and unrated (unselected) items.

### Rated Items (Selected)

The `RatedFill` property sets the fill color for selected rating items.

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" Value="3">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="Gold"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 3
};

Style itemStyle = new Style(typeof(SfRatingItem));
itemStyle.Setters.Add(new Setter(SfRatingItem.RatedFillProperty, 
    new SolidColorBrush(Colors.Gold)));
rating.ItemContainerStyle = itemStyle;

this.Content = rating;
```

**Common color choices for RatedFill:**
- **Gold** - Classic star rating appearance
- **Orange** - Warm, friendly feedback
- **Green** - Positive reinforcement
- **Blue** - Professional, corporate look
- **Red** - Attention-grabbing, urgent

### Unrated Items (Unselected)

The `UnratedFill` property sets the fill color for unselected rating items.

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" Value="2">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="UnratedFill" Value="LightGray"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**C#:**
```csharp
Style itemStyle = new Style(typeof(SfRatingItem));
itemStyle.Setters.Add(new Setter(SfRatingItem.UnRatedFillProperty, 
    new SolidColorBrush(Colors.LightGray)));
rating.ItemContainerStyle = itemStyle;
```

**Typical uses:**
- **LightGray** - Subtle, non-distracting background
- **White** - Clean, minimal appearance
- **Transparent** - Show only selected items
- **Light shade of rated color** - Cohesive color scheme

## Stroke Properties

Stroke properties define the outline appearance of rating items.

### Stroke Colors

#### RatedStroke

Sets the outline color for selected items:

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" Value="3">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="Gold"/>
            <Setter Property="RatedStroke" Value="Orange"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**C#:**
```csharp
itemStyle.Setters.Add(new Setter(SfRatingItem.RatedStrokeProperty, 
    new SolidColorBrush(Colors.Orange)));
```

#### UnratedStroke

Sets the outline color for unselected items:

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" Value="2">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="UnratedFill" Value="White"/>
            <Setter Property="UnratedStroke" Value="Gray"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**Design tip:** Use contrasting stroke colors to create visual separation between items.

### Stroke Thickness

Control the width of item outlines with stroke thickness properties.

#### RatedStrokeThickness

Sets the outline thickness for selected items:

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" Value="3">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedStroke" Value="DarkGoldenrod"/>
            <Setter Property="RatedStrokeThickness" Value="2"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**C#:**
```csharp
itemStyle.Setters.Add(new Setter(SfRatingItem.RatedStrokeThicknessProperty, 2.0));
```

#### UnratedStrokeThickness

Sets the outline thickness for unselected items:

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" Value="2">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="UnratedStroke" Value="LightGray"/>
            <Setter Property="UnratedStrokeThickness" Value="1"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**Thickness guidelines:**
- **0** - No outline (fill only)
- **1** - Subtle outline
- **2** - Standard, visible outline
- **3+** - Bold, prominent outline

## Mouse Hover Effects

Customize the appearance of rating items when the mouse hovers over them.

### PointerOverFill

Sets the fill color when hovering:

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="PointerOverFill" Value="LightGoldenrodYellow"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**C#:**
```csharp
itemStyle.Setters.Add(new Setter(SfRatingItem.PointerOverFillProperty, 
    new SolidColorBrush(Colors.LightGoldenrodYellow)));
```

**Use cases:**
- Preview what rating will be selected
- Provide visual feedback during interaction
- Guide users to clickable areas

### PointerOverStroke

Sets the stroke color when hovering:

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="PointerOverStroke" Value="DarkOrange"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

### PointerOverStrokeThickness

Sets the stroke thickness when hovering:

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="PointerOverStroke" Value="Orange"/>
            <Setter Property="PointerOverStrokeThickness" Value="2"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**Design pattern:** Increase stroke thickness on hover to create a "glow" or emphasis effect.

## Size Customization

### Item Size

The `ItemSize` property sets a uniform size for all rating items. Default is 20.

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="3" 
                     ItemSize="30"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 3,
    ItemSize = 30
};
```

**Size recommendations by context:**
- **12-16** - Compact, inline with text
- **20** - Default, balanced
- **30-40** - Prominent, easy to click
- **50+** - Large displays, touch interfaces

### Individual Heights

Set different heights for each rating item to create visual effects:

**XAML:**
```xml
<syncfusion:SfRating Value="3">
    <syncfusion:SfRatingItem Height="20"/>
    <syncfusion:SfRatingItem Height="18"/>
    <syncfusion:SfRatingItem Height="16"/>
    <syncfusion:SfRatingItem Height="14"/>
    <syncfusion:SfRatingItem Height="12"/>
</syncfusion:SfRating>
```

**C#:**
```csharp
SfRating rating = new SfRating();
rating.Value = 3;
rating.Items.Add(new SfRatingItem() { Height = 20 });
rating.Items.Add(new SfRatingItem() { Height = 18 });
rating.Items.Add(new SfRatingItem() { Height = 16 });
rating.Items.Add(new SfRatingItem() { Height = 14 });
rating.Items.Add(new SfRatingItem() { Height = 12 });
this.Content = rating;
```

**Creative uses:**
- Descending heights for visual interest
- Emphasis on certain items
- Unique designs for branding

### Uniform Heights

Set the same height for all items using ItemContainerStyle:

**XAML:**
```xml
<syncfusion:SfRating Value="4" ItemsCount="10">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="Height" Value="12"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**When to use:**
- Consistent appearance across all items
- When ItemsCount is set (auto-generated items)
- Standard rating interfaces

## Spacing and Layout

### Items Spacing

The `ItemsSpacing` property sets the gap between rating items. Default is 4.

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="3" 
                     ItemsSpacing="10"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 3,
    ItemsSpacing = 10
};
```

**Spacing guidelines:**
- **0-2** - Compact, minimal space
- **4** - Default, balanced
- **8-12** - Spacious, easy to distinguish
- **15+** - Wide spacing for touch interfaces

### Corner Radius

The `CornerRadius` property rounds the corners of rating items:

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="3" 
                     CornerRadius="5"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 3,
    CornerRadius = new CornerRadius(5)
};
```

**Different corner radius values:**
```xml
<!-- Uniform radius -->
<syncfusion:SfRating CornerRadius="10"/>

<!-- Individual corners (TopLeft, TopRight, BottomRight, BottomLeft) -->
<syncfusion:SfRating CornerRadius="10,5,10,5"/>
```

**Design effects:**
- **0** - Sharp corners (default star shape)
- **3-5** - Slightly rounded, modern
- **10+** - Heavily rounded, soft appearance

## Preview Value

The `PreviewValue` property is a read-only property that shows the rating value when hovering before selection.

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="3"
                     x:Name="sfRating"/>

<TextBlock Text="{Binding ElementName=sfRating, Path=PreviewValue}"/>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 3
};

// Access preview value (updates during hover)
double previewValue = rating.PreviewValue;
```

**Use cases:**
- Display numeric preview while user hovers
- Show preview in a separate label or tooltip
- Provide real-time feedback before selection

## ItemContainerStyle

The `ItemContainerStyle` property applies a consistent style to all auto-generated rating items. This is the primary method for styling when using `ItemsCount`.

### Complete Style Example

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" Value="3">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <!-- Fill colors -->
            <Setter Property="RatedFill" Value="Gold"/>
            <Setter Property="UnratedFill" Value="LightGray"/>
            
            <!-- Stroke properties -->
            <Setter Property="RatedStroke" Value="Orange"/>
            <Setter Property="UnratedStroke" Value="Gray"/>
            <Setter Property="RatedStrokeThickness" Value="2"/>
            <Setter Property="UnratedStrokeThickness" Value="1"/>
            
            <!-- Hover effects -->
            <Setter Property="PointerOverFill" Value="LightGoldenrodYellow"/>
            <Setter Property="PointerOverStroke" Value="DarkOrange"/>
            <Setter Property="PointerOverStrokeThickness" Value="2"/>
            
            <!-- Size -->
            <Setter Property="Height" Value="25"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 3
};

Style itemStyle = new Style(typeof(SfRatingItem));

// Fill colors
itemStyle.Setters.Add(new Setter(SfRatingItem.RatedFillProperty, 
    new SolidColorBrush(Colors.Gold)));
itemStyle.Setters.Add(new Setter(SfRatingItem.UnRatedFillProperty, 
    new SolidColorBrush(Colors.LightGray)));

// Stroke properties
itemStyle.Setters.Add(new Setter(SfRatingItem.RatedStrokeProperty, 
    new SolidColorBrush(Colors.Orange)));
itemStyle.Setters.Add(new Setter(SfRatingItem.RatedStrokeThicknessProperty, 2.0));

rating.ItemContainerStyle = itemStyle;
this.Content = rating;
```

## Complete Styling Examples

### Example 1: E-Commerce Product Rating

```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="4.5" 
                     Precision="Half"
                     IsReadOnly="True"
                     ItemSize="24"
                     ItemsSpacing="6">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="#FFD700"/>
            <Setter Property="UnratedFill" Value="#E0E0E0"/>
            <Setter Property="RatedStroke" Value="#FFA500"/>
            <Setter Property="RatedStrokeThickness" Value="1"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

### Example 2: Professional Review System

```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="3"
                     ItemSize="28"
                     ItemsSpacing="8"
                     CornerRadius="3">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="#2196F3"/>
            <Setter Property="UnratedFill" Value="White"/>
            <Setter Property="UnratedStroke" Value="#BDBDBD"/>
            <Setter Property="UnratedStrokeThickness" Value="2"/>
            <Setter Property="PointerOverFill" Value="#64B5F6"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

### Example 3: Minimal Modern Design

```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="4"
                     ItemSize="32"
                     ItemsSpacing="12">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="Black"/>
            <Setter Property="UnratedFill" Value="Transparent"/>
            <Setter Property="UnratedStroke" Value="#E0E0E0"/>
            <Setter Property="UnratedStrokeThickness" Value="2"/>
            <Setter Property="PointerOverFill" Value="#424242"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

### Example 4: Colorful Feedback System

```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="2"
                     ItemSize="26"
                     ItemsSpacing="5">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="#4CAF50"/>
            <Setter Property="UnratedFill" Value="#F5F5F5"/>
            <Setter Property="RatedStroke" Value="#388E3C"/>
            <Setter Property="RatedStrokeThickness" Value="2"/>
            <Setter Property="PointerOverFill" Value="#81C784"/>
            <Setter Property="PointerOverStroke" Value="#66BB6A"/>
            <Setter Property="PointerOverStrokeThickness" Value="2"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

## Best Practices

1. **Contrast**: Ensure sufficient contrast between rated and unrated items
2. **Consistency**: Use the same styling across similar rating controls in your app
3. **Accessibility**: Consider color-blind users; don't rely solely on color
4. **Hover Feedback**: Always provide hover effects for interactive ratings
5. **Size for Touch**: Use ItemSize ≥ 30 for touch-enabled interfaces
6. **Brand Colors**: Match rating colors to your application's brand palette
7. **Read-Only Styling**: Consider different styling for read-only vs interactive ratings
8. **Performance**: ItemContainerStyle is more efficient than individual item styling

## Troubleshooting

### Styles Not Applying

**Problem:** ItemContainerStyle changes don't appear.

**Solutions:**
- Ensure TargetType is set to `syncfusion:SfRatingItem`
- Verify ItemsCount is set (auto-generates items)
- Check that property names are spelled correctly
- Rebuild the application

### Hover Effects Not Working

**Problem:** PointerOver properties don't take effect.

**Solutions:**
- Verify IsReadOnly is set to False
- Check that all three PointerOver properties are set (Fill, Stroke, StrokeThickness)
- Ensure mouse events are not captured by another control

### Sizing Issues

**Problem:** Rating items appear too large or too small.

**Solutions:**
- Adjust ItemSize property for uniform sizing
- Check container constraints (Width, MaxWidth)
- Verify ItemsSpacing isn't consuming too much space
- Consider the parent container's available space

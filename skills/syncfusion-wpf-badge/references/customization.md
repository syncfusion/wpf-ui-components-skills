# Customization in WPF Badge (SfBadge)

## Table of Contents
- [Predefined Colors and Fill States](#predefined-colors-and-fill-states)
- [Custom Colors](#custom-colors)
- [Badge Shapes](#badge-shapes)
- [Custom SVG Shapes](#custom-svg-shapes)
- [Badge Size Configuration](#badge-size-configuration)
- [Troubleshooting Customization](#troubleshooting-customization)

## Predefined Colors and Fill States

### Purpose

The `Fill` property applies predefined color schemes that represent different states or contexts.

### Available Fill Values

| Fill Value | Background Color | Use Case |
|-----------|-------------------|----------|
| `Accent` | DarkSlateBlue | Default; general notifications |
| `Alt` | DarkSlateGray | Alternative styling |
| `Default` | WhiteSmoke | Neutral, muted state |
| `Error` | OrangeRed | Errors, critical alerts |
| `Information` | RoyalBlue | Info messages, updates |
| `Success` | Green | Successful actions, online status |
| `Warning` | Chocolate | Warnings, caution alerts |

### Basic Usage

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Fill="Success"
            Content="10"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

### C# Implementation

```csharp
badge.Fill = BadgeFill.Success;
badge.Content = "10";
```

### Complete Example: Status Buttons

```xaml
<StackPanel Spacing="10">
    <!-- Online status -->
    <Button Content="User Online">
        <notification:SfBadge.Badge>
            <notification:SfBadge Fill="Success" Content="●"/>
        </notification:SfBadge.Badge>
    </Button>

    <!-- Warning -->
    <Button Content="Settings">
        <notification:SfBadge.Badge>
            <notification:SfBadge Fill="Warning" Content="!"/>
        </notification:SfBadge.Badge>
    </Button>

    <!-- Error -->
    <Button Content="Tasks">
        <notification:SfBadge.Badge>
            <notification:SfBadge Fill="Error" Content="5"/>
        </notification:SfBadge.Badge>
    </Button>

    <!-- Info -->
    <Button Content="Messages">
        <notification:SfBadge.Badge>
            <notification:SfBadge Fill="Information" Content="NEW"/>
        </notification:SfBadge.Badge>
    </Button>
</StackPanel>
```

## Custom Colors

### Purpose

Override the `Fill` property with custom colors using `Background` and `Foreground` properties.

### Properties

- `Background` - Custom badge background color (Brush)
- `Foreground` - Custom badge text/content color (Brush)

### Default Values

- `Background` = **null** (uses Fill property)
- `Foreground` = **null** (default text color)

### Example: Custom Background and Foreground

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Background="Black"
            Foreground="Yellow"
            Content="99+"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

### C# Implementation

```csharp
badge.Background = Brushes.Black;
badge.Foreground = Brushes.Yellow;
badge.Content = "99+";
```

### Example: Gradient Background

```xaml
<notification:SfBadge Content="Pro">
    <notification:SfBadge.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Gold" Offset="0"/>
            <GradientStop Color="Orange" Offset="1"/>
        </LinearGradientBrush>
    </notification:SfBadge.Background>
</notification:SfBadge>
```

### Color Precedence

When both `Fill` and `Background` are set, `Background` takes precedence:

```xaml
<!-- This will use the black background, not Success green -->
<notification:SfBadge 
    Fill="Success"
    Background="Black"
    Content="10"/>
```

## Badge Shapes

### Purpose

The `Shape` property defines the default visual shape of the badge.

### Available Shapes

| Shape | Description |
|-------|-------------|
| `Oval` | Circular/oval container (default for content) |
| `Rectangle` | Square or rectangular shape |
| `Ellipse` | Circular container (taller) |
| `None` | No shape; content only |
| `Custom` | Custom SVG path (use `CustomShape` property) |

### Default Value

- `Shape` = **Oval**

### Examples

#### Oval Badge

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Shape="Oval"
            Content="10"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

#### Rectangle Badge

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Shape="Rectangle"
            Content="NEW"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

#### Ellipse Badge

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Shape="Ellipse"
            Content="●"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

#### No Shape

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Shape="None"
            Content="99+"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

### Shape Selection Guidelines

- **Oval**: Best for short text (1-3 characters) or numbers
- **Rectangle**: Best for longer labels ("NEW", "SALE", "PENDING")
- **Ellipse**: Best for single-character indicators (●, !)
- **None**: For custom content or invisible container

## Custom SVG Shapes

### Purpose

Define custom badge shapes using SVG path data.

### How It Works

1. Set `Shape` property to `Custom`
2. Provide SVG path data in `CustomShape` property

### SVG Path Format

The `CustomShape` property accepts standard SVG path data (M, L, C, etc.):

```xaml
<notification:SfBadge 
    Shape="Custom"
    CustomShape="M16,0C17.3,0.5 18.4,1.6 19.2,3.3..."
    Content="10"
    x:Name="badge"/>
```

### Example: Star-Shaped Badge

```xaml
<Button Width="100" Height="50" Content="Special">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Shape="Custom"
            CustomShape="M12,0 L15,10 L25,10 L18,16 L21,26 L12,20 L3,26 L6,16 L-1,10 L9,10 Z"
            Fill="Warning"
            Content="★"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

### C# Implementation

```csharp
badge.Shape = BadgeShape.Custom;
badge.CustomShape = "M12,0 L15,10 L25,10 L18,16 L21,26 L12,20 L3,26 L6,16 L-1,10 L9,10 Z";
badge.Content = "★";
```

### Generating SVG Paths

Use SVG editors like:
- Inkscape
- Adobe Illustrator
- Online SVG path generators
- Draw.io

Then copy the path data into `CustomShape`.

## Badge Size Configuration

### Purpose

Control the physical dimensions of the badge.

### Size Properties

- `Width` - Badge width in pixels
- `Height` - Badge height in pixels

### Default Values

- `Width` = **40**
- `Height` = **30**

### Examples

#### Large Badge

```xaml
<Button Width="150" Height="80" Content="Notifications">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Width="60"
            Height="50"
            FontSize="24"
            Content="99+"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

#### Small Indicator

```xaml
<Image Source="avatar.png" Width="100" Height="100">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Width="20"
            Height="20"
            Shape="Ellipse"
            Fill="Success"
            x:Name="statusBadge"/>
    </notification:SfBadge.Badge>
</Image>
```

#### Adaptive Size Based on Content

```xaml
<notification:SfBadge 
    Width="Auto"
    Height="Auto"
    Padding="8,4"
    Shape="Rectangle"
    Content="NEW FEATURE"
    FontSize="12"/>
```

### C# Implementation

```csharp
badge.Width = 60;
badge.Height = 50;
badge.FontSize = 24;
badge.Content = "99+";
```

### Size Considerations

- Very small badges (< 16x16): Use `Shape="None"` or custom shapes
- Medium badges (20x20 to 40x40): Standard for notifications
- Large badges (> 50x50): Use for dashboard or emphasized notifications
- Maintain aspect ratio for symmetrical shapes (Oval, Ellipse)

## Troubleshooting Customization

### Issue: Custom Background Color Not Appearing

**Symptoms:** `Background` property set but default color still shows.

**Solution:** Ensure `Background` is set before `Fill`:

```xaml
<!-- Correct: Background overrides Fill -->
<notification:SfBadge 
    Fill="Success"
    Background="Black"
    Content="10"/>
```

### Issue: Custom Shape Not Rendering

**Symptoms:** SVG path set but badge appears as default shape.

**Solution:** 
1. Verify `Shape="Custom"` is set
2. Validate SVG path syntax (no unclosed paths)
3. Ensure path uses absolute coordinates

```xaml
<!-- Correct -->
<notification:SfBadge 
    Shape="Custom"
    CustomShape="M10,10 L20,20 L30,10 Z"
    Content="OK"/>

<!-- Incorrect - missing Shape="Custom" -->
<notification:SfBadge 
    CustomShape="M10,10 L20,20 L30,10 Z"
    Content="OK"/>
```

### Issue: Text Not Centered in Custom Shape

**Solution:** Use `HorizontalContentAlignment` and `VerticalContentAlignment`:

```xaml
<notification:SfBadge 
    Shape="Custom"
    CustomShape="M0,0 L30,0 L30,30 L0,30 Z"
    HorizontalContentAlignment="Center"
    VerticalContentAlignment="Center"
    Content="✓"/>
```

### Issue: Badge Size Changes with Content

**Symptoms:** Badge resizes when content text length changes.

**Solution:** Set explicit `Width` and `Height`:

```xaml
<notification:SfBadge 
    Width="40"
    Height="40"
    Content="{Binding NotificationCount}"/>
```

### Issue: Shape Distortion at Different Sizes

**Solution:** Maintain consistent aspect ratios and test at target size:

```xaml
<!-- Good: Proportional -->
<notification:SfBadge 
    Width="50"
    Height="50"
    Shape="Oval"/>

<!-- Avoid: Stretched -->
<notification:SfBadge 
    Width="80"
    Height="20"
    Shape="Oval"/>
```

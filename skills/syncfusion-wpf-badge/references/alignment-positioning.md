# Alignment and Positioning in WPF Badge (SfBadge)

## Table of Contents
- [Alignment Properties Overview](#alignment-properties-overview)
- [HorizontalAlignment and VerticalAlignment](#horizontalalignment-and-verticalalignment)
- [Anchor Properties for Positioning](#anchor-properties-for-positioning)
- [Precise Positioning with Anchor Position](#precise-positioning-with-anchor-position)
- [Content Alignment Within Badge](#content-alignment-within-badge)
- [Common Positioning Patterns](#common-positioning-patterns)

## Alignment Properties Overview

The WPF Badge provides multiple layers of positioning control:

1. **Alignment properties** - Position the badge on its container (Left, Center, Right, Top, Bottom)
2. **Anchor properties** - Control where the badge attaches (Inside, Center, Outside)
3. **Position properties** - Fine-tune placement using normalized coordinates (0-1 range)
4. **Content alignment** - Align content within the badge boundaries

## HorizontalAlignment and VerticalAlignment

### Definition

The `HorizontalAlignment` and `VerticalAlignment` properties define where the badge is positioned relative to its container.

### Default Values

- `HorizontalAlignment` = **Right**
- `VerticalAlignment` = **Top**

### Available Values

**HorizontalAlignment:**
- `Left` - Position at left edge
- `Center` - Position at horizontal center
- `Right` - Position at right edge
- `Stretch` - Stretch across container width

**VerticalAlignment:**
- `Top` - Position at top edge
- `Center` - Position at vertical center
- `Bottom` - Position at bottom edge
- `Stretch` - Stretch across container height

### Examples

#### Top-Right Corner (Default)

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            HorizontalAlignment="Right"
            VerticalAlignment="Top"
            Content="10"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

#### Bottom-Left Corner

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            HorizontalAlignment="Left"
            VerticalAlignment="Bottom"
            Content="5"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

#### Center Position

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            HorizontalAlignment="Center"
            VerticalAlignment="Center"
            Content="3"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

## Anchor Properties for Positioning

### Purpose

The `HorizontalAnchor` and `VerticalAnchor` properties define the attachment point of the badge relative to the alignment position. They work in conjunction with `HorizontalAlignment` and `VerticalAlignment`.

### Default Values

- `HorizontalAnchor` = **Center**
- `VerticalAnchor` = **Center**

### Available Values

- `Inside` - Badge attached inside the container boundary
- `Center` - Badge straddles the alignment edge (partially inside, partially outside)
- `Outside` - Badge positioned completely outside the container boundary
- `Custom` - Use `HorizontalAnchorPosition` and `VerticalAnchorPosition` for custom placement

### Example: Different Anchor Modes

```xaml
<!-- Inside: Badge inside container -->
<notification:SfBadge 
    HorizontalAlignment="Right"
    VerticalAlignment="Top"
    HorizontalAnchor="Inside"
    VerticalAnchor="Inside"
    Content="In"
    x:Name="badge1"/>

<!-- Center: Badge crosses the edge -->
<notification:SfBadge 
    HorizontalAlignment="Right"
    VerticalAlignment="Top"
    HorizontalAnchor="Center"
    VerticalAnchor="Center"
    Content="Cntr"
    x:Name="badge2"/>

<!-- Outside: Badge outside container -->
<notification:SfBadge 
    HorizontalAlignment="Right"
    VerticalAlignment="Top"
    HorizontalAnchor="Outside"
    VerticalAnchor="Outside"
    Content="Out"
    x:Name="badge3"/>
```

### C# Implementation

```csharp
badge.HorizontalAnchor = BadgeAnchor.Outside;
badge.VerticalAnchor = BadgeAnchor.Center;
badge.Content = "10";
```

## Precise Positioning with Anchor Position

### Purpose

For circular or irregularly-shaped containers, use normalized coordinate positioning (0-1 range).

### Properties

- `HorizontalPosition` - Horizontal placement (0 = left, 1 = right)
- `VerticalPosition` - Vertical placement (0 = top, 1 = bottom)
- `HorizontalAnchorPosition` - Horizontal anchor point when `HorizontalAnchor = "Custom"` (0 = left, 1 = right)
- `VerticalAnchorPosition` - Vertical anchor point when `VerticalAnchor = "Custom"` (0 = top, 1 = bottom)

### Default Values

- `HorizontalPosition` = **1** (right edge)
- `VerticalPosition` = **0** (top edge)
- `HorizontalAnchorPosition` = **0**
- `VerticalAnchorPosition` = **0**

### Example: Badge on Circular Avatar

```xaml
<Image Source="avatar.png" Width="100" Height="100">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Shape="None"
            HorizontalPosition="0.9"
            VerticalPosition="0.8"
            x:Name="badge">
            <notification:SfBadge.Content>
                <Ellipse Width="20" Height="20" Fill="LimeGreen"/>
            </notification:SfBadge.Content>
        </notification:SfBadge>
    </notification:SfBadge.Badge>
</Image>
```

### Example: Custom Anchor Position

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            HorizontalAnchor="Custom"
            VerticalAnchor="Custom"
            HorizontalAnchorPosition="0.2"
            VerticalAnchorPosition="0.4"
            Content="99+"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

### C# Implementation

```csharp
badge.HorizontalPosition = 0.9;
badge.VerticalPosition = 0.8;

// For custom anchor
badge.HorizontalAnchor = BadgeAnchor.Custom;
badge.VerticalAnchor = BadgeAnchor.Custom;
badge.HorizontalAnchorPosition = 0.2;
badge.VerticalAnchorPosition = 0.4;
```

## Content Alignment Within Badge

### Purpose

Control how content (text or custom UI) is positioned within the badge boundaries.

### Properties

- `HorizontalContentAlignment` - Content horizontal alignment (Left, Center, Right, Stretch)
- `VerticalContentAlignment` - Content vertical alignment (Top, Center, Bottom, Stretch)

### Default Values

- `HorizontalContentAlignment` = **Center**
- `VerticalContentAlignment` = **Center**

### Example

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            HorizontalContentAlignment="Right"
            VerticalContentAlignment="Top"
            Content="10"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

### C# Implementation

```csharp
badge.HorizontalContentAlignment = HorizontalAlignment.Right;
badge.VerticalContentAlignment = VerticalAlignment.Top;
badge.Content = "10";
```

## Common Positioning Patterns

### Pattern 1: Standard Notification Badge (Top-Right Outside)

Display badge in top-right corner, overlapping the container:

```xaml
<notification:SfBadge 
    HorizontalAlignment="Right"
    VerticalAlignment="Top"
    HorizontalAnchor="Outside"
    VerticalAnchor="Outside"
    Content="5"
    Fill="Error"/>
```

### Pattern 2: Inside Corner Badge

Display badge inside the top-right corner:

```xaml
<notification:SfBadge 
    HorizontalAlignment="Right"
    VerticalAlignment="Top"
    HorizontalAnchor="Inside"
    VerticalAnchor="Inside"
    Content="3"
    Fill="Success"/>
```

### Pattern 3: Status Indicator on Avatar

Small circular indicator on bottom-right of avatar:

```xaml
<Image Source="user-avatar.png" Width="64" Height="64">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Shape="None"
            HorizontalPosition="0.85"
            VerticalPosition="0.85"
            Width="16"
            Height="16"
            x:Name="statusBadge">
            <notification:SfBadge.Content>
                <Ellipse Fill="LimeGreen"/>
            </notification:SfBadge.Content>
        </notification:SfBadge>
    </notification:SfBadge.Badge>
</Image>
```

### Pattern 4: Centered Badge

Display badge centered on container:

```xaml
<notification:SfBadge 
    HorizontalAlignment="Center"
    VerticalAlignment="Center"
    HorizontalAnchor="Center"
    VerticalAnchor="Center"
    Content="NEW"
    Shape="Rectangle"/>
```

### Pattern 5: Multi-Position Layout

Position multiple badges in different corners:

```xaml
<Grid Width="200" Height="150">
    <!-- Top-left -->
    <notification:SfBadge 
        HorizontalAlignment="Left"
        VerticalAlignment="Top"
        HorizontalAnchor="Outside"
        Content="1"/>
    
    <!-- Top-right -->
    <notification:SfBadge 
        HorizontalAlignment="Right"
        VerticalAlignment="Top"
        HorizontalAnchor="Outside"
        Content="2"/>
    
    <!-- Bottom-left -->
    <notification:SfBadge 
        HorizontalAlignment="Left"
        VerticalAlignment="Bottom"
        VerticalAnchor="Outside"
        Content="3"/>
</Grid>
```

---
name: syncfusion-wpf-badge
description: Implements the Syncfusion WPF Badge (SfBadge) component for notification badges and indicators. Use this when adding notification badges to buttons, displaying counts or status indicators, or customizing badge appearance in WPF applications. Covers badge positioning with alignment and anchor properties, color customization with Fill property, shapes, animations, and attachment using SfBadge.Badge property.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Badge (SfBadge)

## When to Use This Skill

Use this skill when you need to:
- Add notification badges to UI elements (buttons, icons, images)
- Display unread message counts or status indicators
- Customize badge appearance with colors, shapes, and animations
- Control badge positioning and alignment on containers
- Implement data-bound badge content in lists or collections

## Component Overview

The Syncfusion WPF Badge (SfBadge) is a notification component used to display counts, status, or alerts on other UI elements. Common use cases include:

- **Unread message counts** - Display "99+" on mailbox buttons
- **Status indicators** - Show online/offline status with colored badges
- **Alert notifications** - Use different colors for errors, warnings, or success states
- **Custom notifications** - Bind to data properties for dynamic updates

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly setup
- Adding SfBadge via XAML and C#
- Basic badge with button example
- Setting display content
- Common setup issues and solutions

### Alignment and Positioning
📄 **Read:** [references/alignment-positioning.md](references/alignment-positioning.md)
- HorizontalAlignment & VerticalAlignment properties (Left, Center, Right, Stretch)
- HorizontalAnchor & VerticalAnchor positioning (Inside, Center, Outside)
- HorizontalPosition & VerticalPosition for precise placement (0-1 range)
- Custom alignment with HorizontalAnchorPosition & VerticalAnchorPosition
- Content alignment within badge boundaries

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- Predefined colors (Fill property: Accent, Success, Error, Warning, Information)
- Custom background and foreground colors
- Predefined shapes (Rectangle, Oval, Ellipse, None)
- Custom SVG shapes via CustomShape property
- Badge size configuration

### Content and Animation
📄 **Read:** [references/content-animation.md](references/content-animation.md)
- Content property and display formats
- ContentTemplate for custom UI
- AnimationType (Scale, Opacity, None)
- Number formatting and display conventions
- Text formatting (FontFamily, FontSize, FontStyle)

### Appearance and Features
📄 **Read:** [references/appearance-features.md](references/appearance-features.md)
- Stroke and StrokeThickness customization
- Opacity property and transparency
- Visibility property for hiding badges
- Direct badge usage vs SfBadge.Badge container
- Data binding with ListView and ItemsSource
- Integration patterns and edge cases

## Quick Start Example

```xaml
<!-- Basic badge on a button -->
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Content="10" 
            Fill="Success"
            Shape="Oval"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

```csharp
// C# equivalent
SfBadge sfBadge = new SfBadge();
sfBadge.Name = "badge";
sfBadge.Content = "10";
sfBadge.Fill = BadgeFill.Success;
sfBadge.Shape = BadgeShape.Oval;

Button button = new Button();
button.Width = 100;
button.Height = 50;
button.Content = "Inbox";

SfBadge.SetBadge(button, sfBadge);
```

## Common Patterns

### Pattern 1: Status Badges with Position Control
Position badges in specific corners with alignment properties:

```xaml
<!-- Top-right corner (default) -->
<notification:SfBadge 
    HorizontalAlignment="Right" 
    VerticalAlignment="Top" 
    Content="5"/>

<!-- Bottom-left corner -->
<notification:SfBadge 
    HorizontalAlignment="Left" 
    VerticalAlignment="Bottom" 
    Content="3"/>
```

### Pattern 2: Animated Count Updates
Enable animations when badge content changes:

```xaml
<notification:SfBadge 
    AnimationType="Scale" 
    Content="{Binding UnreadCount}"
    Fill="Error"/>
```

### Pattern 3: Custom Styled Badges
Combine colors, shapes, and text formatting:

```xaml
<notification:SfBadge 
    Fill="Information"
    Shape="Ellipse"
    FontSize="16"
    FontStyle="Italic"
    Content="NEW"/>
```

### Pattern 4: Data-Bound Collections
Display badges in ListView with conditional visibility:

```xaml
<ListView ItemsSource="{Binding MailItems}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <TextBlock Text="{Binding ItemName}"/>
                <notification:SfBadge 
                    Content="{Binding UnreadCount}"
                    Visibility="{Binding HasMessages, Converter={StaticResource BoolToVisibilityConverter}}"
                    Shape="Oval"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## Key Properties

| Property | Default | Purpose |
|----------|---------|---------|
| `Content` | null | Badge display text/value |
| `Fill` | Accent | Predefined color state (Success, Error, Warning, etc.) |
| `Shape` | Oval | Badge shape (Oval, Rectangle, Ellipse, None, Custom) |
| `HorizontalAlignment` | Right | Horizontal position (Left, Center, Right, Stretch) |
| `VerticalAlignment` | Top | Vertical position (Top, Center, Bottom, Stretch) |
| `HorizontalAnchor` | Center | Anchor mode (Inside, Center, Outside, Custom) |
| `VerticalAnchor` | Center | Anchor mode (Inside, Center, Outside, Custom) |
| `AnimationType` | None | Animation on content change (Scale, Opacity, None) |
| `Background` | null | Custom background color (overrides Fill) |
| `Foreground` | null | Custom text color |
| `FontSize` | 14 | Text size in pixels |
| `Opacity` | 1 | Transparency (0-1 range) |
| `Visibility` | Visible | Show/hide badge (Visible, Collapsed) |

## Common Use Cases

### Use Case 1: Unread Message Indicator
Display badge showing count of unread messages on an inbox button with animation on update.

### Use Case 2: Online Status Indicator
Small colored badge on user avatar (green for online, red for offline) using custom positioning.

### Use Case 3: Error/Warning Alert
Red or orange badge on form controls to indicate validation errors or warnings.

### Use Case 4: Dynamic Notification Counter
Update badge content from data binding as notifications arrive, with Scale animation effect.

### Use Case 5: Multi-State Status Badge
Show different badge styles (color, shape, content) based on application state or user role.

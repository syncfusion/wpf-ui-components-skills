# Appearance and Features in WPF Badge (SfBadge)

## Table of Contents
- [Stroke Customization](#stroke-customization)
- [Opacity and Transparency](#opacity-and-transparency)
- [Visibility Control](#visibility-control)
- [Badge Container vs Direct Usage](#badge-container-vs-direct-usage)
- [Data Binding Integration](#data-binding-integration)
- [Advanced Features and Edge Cases](#advanced-features-and-edge-cases)

## Stroke Customization

### Purpose

Add border styling to badge shapes using stroke properties.

### Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Stroke` | Brush | null | Border color |
| `StrokeThickness` | Double | 0 | Border width in pixels |

### Basic Stroke Example

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Stroke="Red"
            StrokeThickness="3"
            Content="99+"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

### C# Implementation

```csharp
badge.Stroke = Brushes.Red;
badge.StrokeThickness = 3;
badge.Content = "99+";
```

### Stroke with Gradient Border

```xaml
<notification:SfBadge 
    StrokeThickness="2"
    Content="Premium"
    Shape="Ellipse">
    <notification:SfBadge.Stroke>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Gold" Offset="0"/>
            <GradientStop Color="Orange" Offset="1"/>
        </LinearGradientBrush>
    </notification:SfBadge.Stroke>
</notification:SfBadge>
```

### Colored Border Examples

```xaml
<!-- Accent border -->
<notification:SfBadge 
    Stroke="DarkSlateBlue"
    StrokeThickness="2"
    Content="5"/>

<!-- Thick border for emphasis -->
<notification:SfBadge 
    Stroke="Black"
    StrokeThickness="4"
    Content="!"
    Fill="Warning"/>

<!-- Thin border for subtle definition -->
<notification:SfBadge 
    Stroke="Gray"
    StrokeThickness="0.5"
    Content="●"/>
```

### Use Cases

- **Error highlights:** Red stroke with StrokeThickness="2"
- **Selection indicators:** Thicker stroke to show focus
- **Theme separation:** Matching stroke to app brand color
- **Hierarchy emphasis:** Larger badges use thicker strokes

## Opacity and Transparency

### Purpose

Control badge transparency without changing colors.

### Property Details

- **Property:** `Opacity`
- **Type:** Double
- **Range:** 0 (fully transparent) to 1 (fully opaque)
- **Default:** 1 (fully opaque)

### Examples

#### Semi-Transparent Badge

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Opacity="0.7"
            Content="5"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

#### Faded Notification

```xaml
<notification:SfBadge 
    Opacity="0.5"
    Content="Archived"
    Fill="Default"/>
```

#### Fully Transparent (Hidden)

```csharp
badge.Opacity = 0; // Hidden but still takes up space
badge.Visibility = Visibility.Collapsed; // Preferred for hiding
```

### C# Implementation

```csharp
badge.Opacity = 0.6;
badge.Content = "5";
```

### Opacity Guidelines

| Opacity | Use Case |
|---------|----------|
| 0.3-0.4 | Subtle background indicators |
| 0.5-0.7 | Secondary or archived items |
| 0.8-0.9 | Slightly deemphasized state |
| 1.0 | Full prominence (default) |

### Animated Opacity

```csharp
// Fade in animation
DoubleAnimation fadeIn = new DoubleAnimation();
fadeIn.From = 0.0;
fadeIn.To = 1.0;
fadeIn.Duration = new Duration(TimeSpan.FromSeconds(0.5));

badge.BeginAnimation(UIElement.OpacityProperty, fadeIn);
```

## Visibility Control

### Purpose

Show or hide badges programmatically.

### Property Details

- **Property:** `Visibility`
- **Type:** Visibility enum
- **Values:** `Visible`, `Hidden`, `Collapsed`

### Visibility Options

| Value | Behavior | Use Case |
|-------|----------|----------|
| `Visible` | Badge displayed normally | Default |
| `Hidden` | Badge hidden but layout reserved | Temporary hide |
| `Collapsed` | Badge hidden, no layout space | Permanent removal |

### Examples

#### Hide Badge

```xaml
<notification:SfBadge 
    Visibility="Collapsed"
    Content="5"
    x:Name="badge"/>
```

#### Conditional Visibility via Binding

```xaml
<notification:SfBadge 
    Content="{Binding NotificationCount}"
    Visibility="{Binding HasNotifications, Converter={StaticResource BoolToVisibilityConverter}}"
    x:Name="badge"/>
```

#### Toggle in Code

```csharp
// Hide badge
badge.Visibility = Visibility.Collapsed;

// Show badge
badge.Visibility = Visibility.Visible;

// Toggle
badge.Visibility = badge.Visibility == Visibility.Visible 
    ? Visibility.Collapsed 
    : Visibility.Visible;
```

### Alternative: Null Content

```csharp
// Show badge
badge.Content = "5";

// Hide badge (alternative to setting Visibility)
badge.Content = null;
```

### Use Case: Auto-Hide on Zero Count

```csharp
public class NotificationViewModel : INotifyPropertyChanged
{
    private int _count;
    
    public int NotificationCount
    {
        get { return _count; }
        set
        {
            if (_count != value)
            {
                _count = value;
                OnPropertyChanged(nameof(BadgeVisibility));
            }
        }
    }
    
    public Visibility BadgeVisibility => NotificationCount > 0 
        ? Visibility.Visible 
        : Visibility.Collapsed;
}
```

## Badge Container vs Direct Usage

### SfBadge.Badge Attached Property

**Purpose:** Attach badge to any UI element (button, image, etc.)

**Syntax:** Wraps badge in `<notification:SfBadge.Badge></notification:SfBadge.Badge>`

```xaml
<Button Width="100" Height="50" Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Content="10"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

### Direct Badge Usage

**Purpose:** Badge as standalone element without container

```xaml
<Grid>
    <notification:SfBadge 
        Content="5"
        x:Name="badge"/>
</Grid>
```

### Differences

| Aspect | Attached Property | Direct Usage |
|--------|-------------------|--------------|
| Parent | Any UI element | Panel/Grid only |
| Positioning | On parent bounds | Within parent layout |
| Overlay | Positions over content | Respects layout flow |
| Use Case | Notifications on buttons | Standalone badges |

### When to Use Each

**Use Attached Property when:**
- Attaching badge to existing control (Button, Image)
- Creating notification indicators
- Badge should overlay parent content

**Use Direct Usage when:**
- Badge is primary content element
- Badge positioned in normal layout flow
- Building badge collection lists

### Example: List of Badges (Direct)

```xaml
<ListBox ItemsSource="{Binding Items}">
    <ListBox.ItemTemplate>
        <DataTemplate>
            <notification:SfBadge 
                Content="{Binding Status}"
                Fill="Information"/>
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

## Data Binding Integration

### ListView with Data-Bound Badges

```xaml
<ListView 
    BorderThickness="1"
    BorderBrush="LightGray"
    ItemsSource="{Binding MailItems}" 
    SelectedIndex="0"
    VerticalAlignment="Center" 
    HorizontalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="150"/>
                    <ColumnDefinition Width="100"/>
                </Grid.ColumnDefinitions>
                
                <ContentPresenter 
                    Grid.Column="0" 
                    Content="{Binding ItemName}" 
                    VerticalAlignment="Center"/>
                
                <notification:SfBadge 
                    x:Name="badge"
                    Grid.Column="1" 
                    Height="20" 
                    Width="40" 
                    Background="Orange"
                    Content="{Binding UnreadMessageCount}"
                    Shape="Oval"
                    Visibility="{Binding HasMessages, Converter={StaticResource BoolToVisibilityConverter}}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

### ViewModel Structure

```csharp
public class MailItem
{
    public string ItemName { get; set; }
    public int UnreadMessageCount { get; set; }
    public bool HasMessages => UnreadMessageCount > 0;
}

public class ViewModel : INotifyPropertyChanged
{
    public List<MailItem> MailItems { get; set; } = new()
    {
        new MailItem { ItemName = "Inbox", UnreadMessageCount = 20 },
        new MailItem { ItemName = "Drafts", UnreadMessageCount = 0 },
        new MailItem { ItemName = "Sent", UnreadMessageCount = 5 },
        new MailItem { ItemName = "Junk", UnreadMessageCount = 0 }
    };
}
```

### ItemsControl Pattern

```xaml
<ItemsControl ItemsSource="{Binding Notifications}">
    <ItemsControl.ItemTemplate>
        <DataTemplate>
            <Border BorderBrush="Gray" BorderThickness="1" Padding="10" Margin="5">
                <Grid>
                    <TextBlock Text="{Binding Title}"/>
                    <notification:SfBadge 
                        HorizontalAlignment="Right"
                        VerticalAlignment="Top"
                        Content="{Binding Priority, Converter={StaticResource PriorityToBadgeConverter}}"
                        Fill="{Binding Priority, Converter={StaticResource PriorityToColorConverter}}"/>
                </Grid>
            </Border>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ItemsControl>
```

## Advanced Examples

The advanced examples for appearance, edge cases, and performance patterns have been moved to a separate file to keep this reference concise: `appearance-features-advanced.md` (same folder).

See `appearance-features-advanced.md` for animations, responsive sizing, complex templates, and performance patterns.

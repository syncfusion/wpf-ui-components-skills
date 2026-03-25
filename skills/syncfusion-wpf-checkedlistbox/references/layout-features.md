# Layout Features

## Overview

This guide covers layout-related properties and features of the CheckListBox control, including checkbox alignment, orientation, dimensions, and scrolling behavior.

**Key layout properties:**
- `CheckBoxAlignment` / `CheckBoxPlacement` - Checkbox position
- `FlowDirection` - Left-to-right or right-to-left
- `Width` / `Height` - Control dimensions
- `Margin` / `Padding` - Spacing
- `ScrollViewer` settings - Scroll behavior

## CheckBox Alignment

Position the checkbox on the left or right side of each item.

### CheckBoxAlignment Property

```xml
<!-- Left alignment (default) -->
<syncfusion:CheckListBox CheckBoxAlignment="Left">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
    <syncfusion:CheckListBoxItem Content="Bermuda" />
    <syncfusion:CheckListBoxItem Content="Belize" />
    <syncfusion:CheckListBoxItem Content="Panama" />
</syncfusion:CheckListBox>

<!-- Right alignment -->
<syncfusion:CheckListBox CheckBoxAlignment="Right">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
</syncfusion:CheckListBox>
```

```csharp
// Set checkbox alignment to right
checkListBox.CheckBoxAlignment = CheckBoxAlignment.Right;
```

**Alignment options:**
- **Left** - Checkbox appears on the left side (default)
- **Right** - Checkbox appears on the right side

**When to use right alignment:**
- Right-to-left language applications
- Design consistency with other controls
- User preference for checkbox position

**Note:** Some Syncfusion versions may use `CheckBoxPlacement` instead of `CheckBoxAlignment`. Check your version's documentation.

## Flow Direction

Control the overall layout direction for internationalization support.

### FlowDirection Property

```xml
<!-- Left to right (default) -->
<syncfusion:CheckListBox FlowDirection="LeftToRight">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
</syncfusion:CheckListBox>

<!-- Right to left -->
<syncfusion:CheckListBox FlowDirection="RightToLeft">
    <syncfusion:CheckListBoxItem Content="المكسيك" />
    <syncfusion:CheckListBoxItem Content="كندا" />
</syncfusion:CheckListBox>
```

```csharp
// Set flow direction
checkListBox.FlowDirection = FlowDirection.LeftToRight;  // Default
checkListBox.FlowDirection = FlowDirection.RightToLeft;   // RTL
```

**FlowDirection effects:**
- Text alignment (left vs right)
- Checkbox position (unless explicitly set)
- Scrollbar position
- Overall layout mirroring

**RTL language support:**

For complete RTL support, set FlowDirection on the parent window:

```xml
<Window FlowDirection="RightToLeft">
    <Grid>
        <syncfusion:CheckListBox FlowDirection="RightToLeft"
                                 CheckBoxAlignment="Right">
            <!-- Items -->
        </syncfusion:CheckListBox>
    </Grid>
</Window>
```

## Control Dimensions

### Setting Width and Height

```xml
<!-- Fixed dimensions -->
<syncfusion:CheckListBox Width="300" Height="400">
    <syncfusion:CheckListBoxItem Content="Item 1" />
    <syncfusion:CheckListBoxItem Content="Item 2" />
</syncfusion:CheckListBox>

<!-- Auto width, fixed height -->
<syncfusion:CheckListBox Height="400">
    <syncfusion:CheckListBoxItem Content="Item 1" />
</syncfusion:CheckListBox>

<!-- Fill available space -->
<syncfusion:CheckListBox HorizontalAlignment="Stretch"
                         VerticalAlignment="Stretch">
    <syncfusion:CheckListBoxItem Content="Item 1" />
</syncfusion:CheckListBox>
```

```csharp
// Set dimensions in code
checkListBox.Width = 300;
checkListBox.Height = 400;

// Min/max dimensions
checkListBox.MinWidth = 200;
checkListBox.MaxWidth = 500;
checkListBox.MinHeight = 300;
checkListBox.MaxHeight = 600;
```

### Responsive Sizing

**Stretch to parent:**

```xml
<Grid>
    <syncfusion:CheckListBox HorizontalAlignment="Stretch"
                             VerticalAlignment="Stretch">
    </syncfusion:CheckListBox>
</Grid>
```

**Proportional sizing in Grid:**

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="2*" />
    </Grid.RowDefinitions>
    
    <syncfusion:CheckListBox Grid.Row="0" />
    <syncfusion:CheckListBox Grid.Row="1" />  <!-- Takes twice the space -->
</Grid>
```

## Margin and Padding

### Margin - External Spacing

Space around the outside of the control.

```xml
<!-- Uniform margin -->
<syncfusion:CheckListBox Margin="20">
</syncfusion:CheckListBox>

<!-- Different margins (Left, Top, Right, Bottom) -->
<syncfusion:CheckListBox Margin="10,20,10,20">
</syncfusion:CheckListBox>
```

```csharp
// Uniform margin
checkListBox.Margin = new Thickness(20);

// Different values
checkListBox.Margin = new Thickness(10, 20, 10, 20);  // Left, Top, Right, Bottom
```

### Padding - Internal Spacing

Space between the control border and its content.

```xml
<!-- Uniform padding -->
<syncfusion:CheckListBox Padding="15">
</syncfusion:CheckListBox>

<!-- Different padding values -->
<syncfusion:CheckListBox Padding="10,5,10,5">
</syncfusion:CheckListBox>
```

```csharp
// Set padding
checkListBox.Padding = new Thickness(15);
checkListBox.Padding = new Thickness(10, 5, 10, 5);
```

### Item Spacing

Control spacing between items using ItemContainerStyle:

```xml
<syncfusion:CheckListBox>
    <syncfusion:CheckListBox.ItemContainerStyle>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="Margin" Value="0,4" />  <!-- 4px vertical spacing -->
            <Setter Property="Padding" Value="8" />   <!-- 8px internal padding -->
        </Style>
    </syncfusion:CheckListBox.ItemContainerStyle>
</syncfusion:CheckListBox>
```

## Scrolling Behavior

### ScrollViewer Properties

The CheckListBox uses an internal ScrollViewer for scrolling. Control its behavior through attached properties.

**Vertical scrolling (default):**

```xml
<syncfusion:CheckListBox ScrollViewer.VerticalScrollBarVisibility="Auto"
                         ScrollViewer.HorizontalScrollBarVisibility="Disabled">
</syncfusion:CheckListBox>
```

**ScrollBarVisibility values:**
- **Auto** - Shows scrollbar when content exceeds view (default)
- **Visible** - Always shows scrollbar
- **Hidden** - Scrolling enabled, scrollbar hidden
- **Disabled** - No scrolling in this direction

### Horizontal Scrolling

Enable horizontal scrolling for wide items:

```xml
<syncfusion:CheckListBox ScrollViewer.HorizontalScrollBarVisibility="Auto"
                         ScrollViewer.VerticalScrollBarVisibility="Auto">
</syncfusion:CheckListBox>
```

### Scrolling Performance

**Enable smooth scrolling:**

```xml
<syncfusion:CheckListBox ScrollViewer.CanContentScroll="False">
    <!-- Pixel-based smooth scrolling -->
</syncfusion:CheckListBox>
```

**Enable item-based scrolling (faster for many items):**

```xml
<syncfusion:CheckListBox ScrollViewer.CanContentScroll="True">
    <!-- Scroll by item, not pixel -->
</syncfusion:CheckListBox>
```

## Borders

### Border Properties

```xml
<!-- Border with color and thickness -->
<syncfusion:CheckListBox BorderBrush="#E0E0E0"
                         BorderThickness="1">
</syncfusion:CheckListBox>

<!-- Different border thicknesses -->
<syncfusion:CheckListBox BorderBrush="Gray"
                         BorderThickness="2,0,2,0">  <!-- Left=2, Top=0, Right=2, Bottom=0 -->
</syncfusion:CheckListBox>

<!-- No border -->
<syncfusion:CheckListBox BorderThickness="0">
</syncfusion:CheckListBox>
```

```csharp
// Set border
checkListBox.BorderBrush = new SolidColorBrush(Color.FromRgb(224, 224, 224));
checkListBox.BorderThickness = new Thickness(1);

// Remove border
checkListBox.BorderThickness = new Thickness(0);
```

### Rounded Corners

Use a Border wrapper for rounded corners:

```xml
<Border BorderBrush="#E0E0E0"
        BorderThickness="1"
        CornerRadius="8"
        Background="White">
    <syncfusion:CheckListBox BorderThickness="0"
                             Background="Transparent">
    </syncfusion:CheckListBox>
</Border>
```

## Alignment

### Horizontal and Vertical Alignment

```xml
<!-- Center in parent -->
<Grid>
    <syncfusion:CheckListBox Width="300"
                             Height="400"
                             HorizontalAlignment="Center"
                             VerticalAlignment="Center">
    </syncfusion:CheckListBox>
</Grid>

<!-- Align to specific side -->
<syncfusion:CheckListBox Width="300"
                         HorizontalAlignment="Right"
                         VerticalAlignment="Top">
</syncfusion:CheckListBox>
```

**Alignment values:**
- **Left** / **Top** - Align to left/top
- **Center** - Center in available space
- **Right** / **Bottom** - Align to right/bottom
- **Stretch** - Fill available space (default if Width/Height not set)

### Content Alignment

Align content within the control:

```xml
<syncfusion:CheckListBox HorizontalContentAlignment="Left"
                         VerticalContentAlignment="Top">
</syncfusion:CheckListBox>
```

## Complete Layout Example

```xml
<Window x:Class="LayoutDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="CheckListBox Layout" Height="600" Width="400">
    
    <Grid Background="#F5F5F5">
        <!-- Outer margin for window padding -->
        <Border Margin="20"
                Background="White"
                BorderBrush="#E0E0E0"
                BorderThickness="1"
                CornerRadius="8">
            
            <syncfusion:CheckListBox BorderThickness="0"
                                     Background="Transparent"
                                     Padding="16"
                                     CheckBoxAlignment="Left"
                                     FlowDirection="LeftToRight"
                                     ScrollViewer.VerticalScrollBarVisibility="Auto"
                                     ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                
                <!-- Item styling with spacing -->
                <syncfusion:CheckListBox.ItemContainerStyle>
                    <Style TargetType="syncfusion:CheckListBoxItem">
                        <Setter Property="Margin" Value="0,6" />
                        <Setter Property="Padding" Value="8" />
                        <Setter Property="FontSize" Value="14" />
                    </Style>
                </syncfusion:CheckListBox.ItemContainerStyle>
                
                <!-- Items -->
                <syncfusion:CheckListBoxItem Content="Task 1" />
                <syncfusion:CheckListBoxItem Content="Task 2" />
                <syncfusion:CheckListBoxItem Content="Task 3" />
                <syncfusion:CheckListBoxItem Content="Task 4" />
                <syncfusion:CheckListBoxItem Content="Task 5" />
            </syncfusion:CheckListBox>
        </Border>
    </Grid>
</Window>
```

## Troubleshooting

**Issue: Control not visible**
- Check if Width/Height are set to 0 or very small values
- Verify parent container has space allocated
- Check Visibility property isn't Collapsed

**Issue: Scrollbar not appearing**
- Set appropriate Height to constrain the control
- Verify ScrollViewer.VerticalScrollBarVisibility is not Disabled
- Ensure items exceed visible area

**Issue: Items cut off**
- Increase Width if using fixed width
- Set HorizontalScrollBarVisibility to Auto
- Check Padding and Margin values

**Issue: Too much spacing**
- Adjust Margin in ItemContainerStyle
- Reduce Padding on control
- Check for inherited margins from parent containers

## Next Steps

- **Customize Appearance:** [appearance-customization.md](appearance-customization.md)
- **Optimize Performance:** [virtualization.md](virtualization.md)
- **Handle Selection:** [checking-and-selection.md](checking-and-selection.md)

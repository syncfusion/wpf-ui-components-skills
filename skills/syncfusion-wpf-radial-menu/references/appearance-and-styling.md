# Appearance and Styling in WPF Radial Menu

This guide covers the extensive appearance and styling options available for customizing the visual presentation of the Syncfusion WPF Radial Menu.

## Table of Contents
- [Radius Configuration](#radius-configuration)
- [Center Rim Styling](#center-rim-styling)
- [Outer Rim Styling](#outer-rim-styling)
- [Rim Radius Factor](#rim-radius-factor)
- [Expander Visibility](#expander-visibility)
- [Per-Item Customization](#per-item-customization)
- [Mouse Over Styling](#mouse-over-styling)
- [Navigation Button Styling](#navigation-button-styling)
- [Separator Styling](#separator-styling)
- [Stroke Properties](#stroke-properties)
- [Advanced Styling](#advanced-styling)

---

## Radius Configuration

The `RadiusX` and `RadiusY` properties control the size of the radial menu by defining the horizontal and vertical radii.

### Basic Radius Settings

```xaml
<navigation:SfRadialMenu RadiusX="150" RadiusY="150" IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.RadiusX = 150;
radialMenu.RadiusY = 150;
radialMenu.IsOpen = true;
```

### Circular vs Elliptical Menus

**Circular Menu (Equal RadiusX and RadiusY):**
```xaml
<navigation:SfRadialMenu RadiusX="120" RadiusY="120" IsOpen="True">
    <!-- Items arranged in perfect circle -->
</navigation:SfRadialMenu>
```

**Elliptical Menu (Different RadiusX and RadiusY):**
```xaml
<navigation:SfRadialMenu RadiusX="180" RadiusY="100" IsOpen="True">
    <!-- Items arranged in horizontal ellipse -->
</navigation:SfRadialMenu>
```

### Size Recommendations

| Screen Type | Recommended Radius | Use Case |
|-------------|-------------------|----------|
| Desktop (Mouse) | 100-150px | Standard context menu |
| Tablet (Touch) | 150-200px | Touch-friendly interface |
| Large Display | 180-250px | Presentation mode |
| Compact View | 80-120px | Sidebar or limited space |

---

## Center Rim Styling

### CenterRimRadiusFactor

The `CenterRimRadiusFactor` property defines the radius of the center rim (inner circle) as a factor of the overall radius. Value ranges from 0.0 to 1.0.

```xaml
<navigation:SfRadialMenu CenterRimRadiusFactor="0.3" IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.CenterRimRadiusFactor = 0.3; // 30% of the total radius
```

### Factor Value Effects

| Factor Value | Center Size | Item Area | Use Case |
|--------------|-------------|-----------|----------|
| 0.1 | Very small | Maximum | More space for items |
| 0.3 | Small | Large | Balanced (recommended) |
| 0.5 | Medium | Medium | Prominent center icon |
| 0.7 | Large | Small | Center-focused design |

**Visual Example:**
```xaml
<!-- Small center, more item space -->
<navigation:SfRadialMenu CenterRimRadiusFactor="0.2" IsOpen="True">
    <navigation:SfRadialMenu.Icon>
        <TextBlock Text="A" FontSize="14"/>
    </navigation:SfRadialMenu.Icon>
</navigation:SfRadialMenu>

<!-- Large center, less item space -->
<navigation:SfRadialMenu CenterRimRadiusFactor="0.6" IsOpen="True">
    <navigation:SfRadialMenu.Icon>
        <TextBlock Text="B" FontSize="24"/>
    </navigation:SfRadialMenu.Icon>
</navigation:SfRadialMenu>
```

---

## Outer Rim Styling

### RimBackground

The `RimBackground` property fills the outer rim (outer circle) with a color or brush.

```xaml
<navigation:SfRadialMenu IsOpen="True" RimBackground="Green">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.RimBackground = new SolidColorBrush(Colors.Green);
```

**Gradient Background:**
```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenu.RimBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="LightBlue" Offset="0"/>
            <GradientStop Color="DodgerBlue" Offset="1"/>
        </LinearGradientBrush>
    </navigation:SfRadialMenu.RimBackground>
</navigation:SfRadialMenu>
```

### RimActiveBrush

The `RimActiveBrush` property fills the expander rim segment. This expander is visible only when menu items have sub-items.

```xaml
<navigation:SfRadialMenu RimActiveBrush="Red" 
                         RimBackground="Green" 
                         IsOpen="True">
    <navigation:SfRadialMenuItem Header="Format">
        <navigation:SfRadialMenuItem Header="Bold"/>
        <navigation:SfRadialMenuItem Header="Italic"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Edit"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.RimActiveBrush = new SolidColorBrush(Colors.Red);
radialMenu.RimBackground = new SolidColorBrush(Colors.Green);
```

### RimInactiveBrush

The `RimInactiveBrush` property fills the expander rim when the corresponding menu item doesn't have sub-items.

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Item 1" 
                                 RimActiveBrush="SlateBlue" 
                                 RimInactiveBrush="Red"/>
    <navigation:SfRadialMenuItem Header="Item 2" 
                                 RimActiveBrush="Green">
        <navigation:SfRadialMenuItem Header="Item 21"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Item 3" 
                                 RimActiveBrush="Yellow">
        <navigation:SfRadialMenuItem Header="Item 31"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Item 4" 
                                 RimActiveBrush="Orange">
        <navigation:SfRadialMenuItem Header="Item 41"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Item 5" 
                                 IsExpanderVisible="False" 
                                 RimActiveBrush="Black" 
                                 RimInactiveBrush="GreenYellow"/>
</navigation:SfRadialMenu>
```

### RimHoverBrush

The `RimHoverBrush` property fills the expander rim when the pointer hovers over it.

```xaml
<navigation:SfRadialMenu RimActiveBrush="Red" 
                         RimBackground="Green" 
                         RimHoverBrush="Blue" 
                         IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold">
        <navigation:SfRadialMenuItem Header="Bold 1"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Copy"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.RimHoverBrush = new SolidColorBrush(Colors.Blue);
```

---

## Rim Radius Factor

The `RimRadiusFactor` property sets the radius of the items panel as a factor. Lower values increase outer rim thickness; higher values decrease it.

```xaml
<navigation:SfRadialMenu RimActiveBrush="Red" 
                         RimRadiusFactor="0.7" 
                         RimBackground="Green" 
                         RimHoverBrush="Blue" 
                         IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.RimRadiusFactor = 0.7;
```

### Factor Impact

| Factor Value | Outer Rim | Item Area | Visual Effect |
|--------------|-----------|-----------|---------------|
| 0.5 | Thick | Small | Prominent rim |
| 0.7 | Medium | Medium | Balanced (recommended) |
| 0.9 | Thin | Large | Subtle rim |

---

## Expander Visibility

The `IsExpanderVisible` property controls whether the expander arrow in the outer rim is visible for items with children.

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Item 1" 
                                 RimActiveBrush="SlateBlue">
        <navigation:SfRadialMenuItem Header="Item 21"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Item 2" 
                                 RimActiveBrush="Green">
        <navigation:SfRadialMenuItem Header="Item 21"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Item 3" 
                                 RimActiveBrush="Yellow">
        <navigation:SfRadialMenuItem Header="Item 31"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Item 4" 
                                 RimActiveBrush="Orange">
        <navigation:SfRadialMenuItem Header="Item 41"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Item 5" 
                                 IsExpanderVisible="False" 
                                 RimActiveBrush="Black">
        <navigation:SfRadialMenuItem Header="Item 41"/>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

```csharp
SfRadialMenuItem item = new SfRadialMenuItem() 
{ 
    Header = "Item 5", 
    IsExpanderVisible = false 
};
item.Items.Add(new SfRadialMenuItem() { Header = "Child" });
```

**Use Case:** Hide expander when you want to use custom navigation logic or when the visual indicator isn't desired.

---

## Per-Item Customization

### MenuBackgroundColor

Each `SfRadialMenuItem` can have a different background color using the `MenuBackgroundColor` property.

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold" 
                                 MenuBackgroundColor="Pink">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Copy" 
                                 MenuBackgroundColor="PaleTurquoise">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Undo" 
                                 MenuBackgroundColor="Pink">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Font Size" 
                                 MenuBackgroundColor="PaleTurquoise">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Color" 
                                 MenuBackgroundColor="Lavender">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

```csharp
SfRadialMenuItem boldItem = new SfRadialMenuItem() 
{ 
    Header = "Bold", 
    MenuBackgroundColor = new SolidColorBrush(Colors.Pink) 
};
```

**Use Case:** Color-code menu items by category (e.g., all formatting options in blue, all edit options in green).

---

## Mouse Over Styling

### ShowMouseOverStyle

Enable mouse-over styling by setting `ShowMouseOverStyle` to `true`.

```xaml
<navigation:SfRadialMenuItem Header="Bold" 
                             ShowMouseOverStyle="True" 
                             MenuMouseOverBackgroundColor="Orange"/>
```

### MenuMouseOverBackgroundColor

Set a different background color on mouse hover with the `MenuMouseOverBackgroundColor` property. Requires `ShowMouseOverStyle="True"`.

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold" 
                                 MenuMouseOverBackgroundColor="Pink" 
                                 ShowMouseOverStyle="True">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Copy" 
                                 MenuMouseOverBackgroundColor="PaleTurquoise" 
                                 ShowMouseOverStyle="True">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Undo" 
                                 MenuMouseOverBackgroundColor="Pink" 
                                 ShowMouseOverStyle="True">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Font Size" 
                                 MenuMouseOverBackgroundColor="PaleTurquoise" 
                                 ShowMouseOverStyle="True">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Color" 
                                 MenuMouseOverBackgroundColor="Lavender" 
                                 ShowMouseOverStyle="True">
        <navigation:SfRadialMenuItem/>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

```csharp
SfRadialMenuItem item = new SfRadialMenuItem() 
{ 
    Header = "Bold", 
    ShowMouseOverStyle = true,
    MenuMouseOverBackgroundColor = new SolidColorBrush(Colors.Pink) 
};
```

---

## Navigation Button Styling

### CenterBackButtonForeground

The `CenterBackButtonForeground` property customizes the foreground color of the back button displayed at the center when navigating into sub-levels.

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         CenterBackButtonForeground="White">
    <navigation:SfRadialMenuItem Header="Bold">
        <navigation:SfRadialMenuItem Header="Bold 1"/>
        <navigation:SfRadialMenuItem Header="Bold 2"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.CenterBackButtonForeground = new SolidColorBrush(Colors.White);
```

### NavigationButtonStyle

Apply a custom style to the navigation button using the `NavigationButtonStyle` property.

```xaml
<Window.Resources>
    <Style x:Key="NavigationButtonStyle" TargetType="Button">
        <Setter Property="Background" Value="DodgerBlue"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="FontWeight" Value="Bold"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="Button">
                    <Grid>
                        <Ellipse Fill="{TemplateBinding Background}"/>
                        <ContentPresenter HorizontalAlignment="Center" 
                                        VerticalAlignment="Center"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<navigation:SfRadialMenu NavigationButtonStyle="{StaticResource NavigationButtonStyle}"
                         IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold">
        <navigation:SfRadialMenuItem Header="Bold 1"/>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

---

## Separator Styling

### SeparatorBackground

The `SeparatorBackground` property sets the background color of separator lines drawn between radial menu items.

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         SeparatorBackground="DarkGray">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
    <navigation:SfRadialMenuItem Header="Cut"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.SeparatorBackground = new SolidColorBrush(Colors.DarkGray);
```

### SeparatorWidth

The `SeparatorWidth` property defines the width (thickness) of separator lines between menu items.

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         SeparatorBackground="DarkGray" 
                         SeparatorWidth="3">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
    <navigation:SfRadialMenuItem Header="Cut"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.SeparatorBackground = new SolidColorBrush(Colors.DarkGray);
radialMenu.SeparatorWidth = 3;
```

### Separator Styling Tips

| Width | Visual Impact | Use Case |
|-------|---------------|----------|
| 1 | Subtle | Minimal separation |
| 2-3 | Visible | Standard separation (recommended) |
| 4-5 | Prominent | Strong visual grouping |

---

## Stroke Properties

### StrokeThickness

The `StrokeThickness` property defines the border stroke thickness of the outer rim circle.

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         StrokeThickness="4" 
                         RimBackground="LightBlue">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.StrokeThickness = 4;
radialMenu.RimBackground = new SolidColorBrush(Colors.LightBlue);
```

### MouseOverRimStrokeThickness

The `MouseOverRimStrokeThickness` property defines the stroke thickness of the outer rim when hovering over a menu item.

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         RimBackground="LightBlue"
                         RimHoverBrush="Orange"
                         MouseOverRimStrokeThickness="3">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
radialMenu.RimBackground = new SolidColorBrush(Colors.LightBlue);
radialMenu.RimHoverBrush = new SolidColorBrush(Colors.Orange);
radialMenu.MouseOverRimStrokeThickness = 3;
```

---

## Advanced Styling

### MouseOverRimStyle

The `MouseOverRimStyle` property applies a custom `Style` to the rim segment that appears when hovering over a menu item.

```xaml
<Window.Resources>
    <Style x:Key="CustomMouseOverRimStyle" TargetType="Path">
        <Setter Property="Fill" Value="Orange"/>
        <Setter Property="Opacity" Value="0.7"/>
        <Setter Property="StrokeThickness" Value="2"/>
        <Setter Property="Stroke" Value="DarkOrange"/>
    </Style>
</Window.Resources>

<navigation:SfRadialMenu IsOpen="True"
                         MouseOverRimStyle="{StaticResource CustomMouseOverRimStyle}">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
Style mouseOverStyle = new Style(typeof(Path));
mouseOverStyle.Setters.Add(new Setter(Path.FillProperty, new SolidColorBrush(Colors.Orange)));
mouseOverStyle.Setters.Add(new Setter(Path.OpacityProperty, 0.7));
mouseOverStyle.Setters.Add(new Setter(Path.StrokeThicknessProperty, 2.0));
mouseOverStyle.Setters.Add(new Setter(Path.StrokeProperty, new SolidColorBrush(Colors.DarkOrange)));

radialMenu.MouseOverRimStyle = mouseOverStyle;
```

### Complete Styled Example

```xaml
<navigation:SfRadialMenu IsOpen="True"
                         RadiusX="160"
                         RadiusY="160"
                         CenterRimRadiusFactor="0.35"
                         RimRadiusFactor="0.75"
                         RimBackground="#FF2196F3"
                         RimActiveBrush="#FF4CAF50"
                         RimHoverBrush="#FFFFC107"
                         StrokeThickness="3"
                         SeparatorBackground="White"
                         SeparatorWidth="2"
                         MouseOverRimStrokeThickness="4"
                         CenterBackButtonForeground="White">
    <navigation:SfRadialMenu.Icon>
        <TextBlock Text="✎" FontSize="24" Foreground="White"/>
    </navigation:SfRadialMenu.Icon>
    
    <navigation:SfRadialMenuItem Header="Format"
                                 MenuBackgroundColor="#E3F2FD"
                                 MenuMouseOverBackgroundColor="#BBDEFB"
                                 ShowMouseOverStyle="True">
        <navigation:SfRadialMenuItem Header="Bold"/>
        <navigation:SfRadialMenuItem Header="Italic"/>
    </navigation:SfRadialMenuItem>
    
    <navigation:SfRadialMenuItem Header="Edit"
                                 MenuBackgroundColor="#E8F5E9"
                                 MenuMouseOverBackgroundColor="#C8E6C9"
                                 ShowMouseOverStyle="True">
        <navigation:SfRadialMenuItem Header="Cut"/>
        <navigation:SfRadialMenuItem Header="Copy"/>
    </navigation:SfRadialMenuItem>
    
    <navigation:SfRadialMenuItem Header="Undo"
                                 MenuBackgroundColor="#FFF3E0"
                                 MenuMouseOverBackgroundColor="#FFE0B2"
                                 ShowMouseOverStyle="True"/>
</navigation:SfRadialMenu>
```

---

## Styling Best Practices

### Color Schemes

1. **High Contrast:** Ensure text and icons are readable against backgrounds
2. **Consistent Palette:** Use colors from a unified color scheme
3. **Semantic Colors:** Use colors to convey meaning (red for delete, green for save)

### Interactive States

1. **Hover Feedback:** Always provide visual feedback on hover
2. **Active State:** Distinguish active/expanded items clearly
3. **Disabled State:** Show disabled items with reduced opacity

### Performance

1. **Solid Colors:** Prefer solid colors over gradients for better performance
2. **Simple Styles:** Complex styles can impact rendering performance
3. **Reuse Resources:** Define colors and brushes as resources to reuse

### Accessibility

1. **Color Contrast:** Maintain WCAG contrast ratios (4.5:1 minimum)
2. **Don't Rely on Color Alone:** Use icons or text in addition to color
3. **High Contrast Themes:** Test with Windows High Contrast mode

---

## Troubleshooting

### Issue: Custom Colors Not Appearing

**Problem:** Set colors but menu still uses default styling.

**Solution:** Ensure properties are set correctly and IsOpen is true:
```xaml
<navigation:SfRadialMenu IsOpen="True" RimBackground="Red">
    <!-- Items -->
</navigation:SfRadialMenu>
```

### Issue: Mouse Over Style Not Working

**Problem:** MenuMouseOverBackgroundColor set but no hover effect.

**Solution:** Set `ShowMouseOverStyle="True"`:
```xaml
<navigation:SfRadialMenuItem Header="Item" 
                             ShowMouseOverStyle="True"
                             MenuMouseOverBackgroundColor="Orange"/>
```

### Issue: Separators Not Visible

**Problem:** SeparatorBackground set but separators don't show.

**Solution:** Increase SeparatorWidth and ensure contrasting color:
```xaml
<navigation:SfRadialMenu SeparatorBackground="Black" SeparatorWidth="2">
    <!-- Items -->
</navigation:SfRadialMenu>
```

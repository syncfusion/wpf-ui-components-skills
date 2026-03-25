# Appearance in WPF DataPager

## Table of Contents
- [Overview](#overview)
- [AutoEllipsisMode](#autoellipsismode)
- [AccentBrush Properties](#accentbrush-properties)
- [DisplayMode Options](#displaymode-options)
- [Orientation](#orientation)
- [Best Practices](#best-practices)

## Overview

SfDataPager supports extensive appearance customization through properties that control colors, button visibility, layout orientation, and ellipsis behavior. These properties allow you to match the DataPager's appearance to your application's theme and user experience requirements.

## AutoEllipsisMode

The AutoEllipsis button displays when the total page count exceeds the `NumericButtonCount`. This button allows users to navigate to additional pages not currently visible in the numeric button range.

### AutoEllipsisMode Property

Controls where the ellipsis button appears. This is an enum property.

**Available Values:**
- `None` (default): No ellipsis button displayed
- `After`: Ellipsis button appears after numeric buttons
- `Before`: Ellipsis button appears before numeric buttons
- `Both`: Ellipsis buttons appear before and after numeric buttons

### AutoEllipsisText Property

Customizes the text displayed on the ellipsis button. Default is "...".

### AutoEllipsisMode Examples

#### After Mode

Ellipsis button displayed after numeric buttons:

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       AutoEllipsisMode="After"
                       NumericButtonCount="5"
                       PageSize="10"
                       Source="{Binding OrdersDetails}"/>
```

**Display:** `< 1 2 3 4 5 ... >`

When clicked, shows next set: `< ... 6 7 8 9 10 ... >`

#### Before Mode

Ellipsis button displayed before numeric buttons:

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       AutoEllipsisMode="Before"
                       NumericButtonCount="5"
                       PageSize="10"
                       Source="{Binding OrdersDetails}"/>
```

**Display:** `< ... 6 7 8 9 10 >`

When clicked, shows previous set: `< ... 1 2 3 4 5 ... >`

#### Both Mode

Ellipsis buttons displayed before and after numeric buttons:

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       AutoEllipsisMode="Both"
                       NumericButtonCount="5"
                       PageSize="10"
                       Source="{Binding OrdersDetails}"/>
```

**Display:** `< ... 6 7 8 9 10 ... >`

Allows navigation to both previous and next page sets.

#### None Mode

No ellipsis button displayed (default):

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       AutoEllipsisMode="None"
                       NumericButtonCount="5"
                       PageSize="10"
                       Source="{Binding OrdersDetails}"/>
```

Only currently visible numeric buttons are shown. Users must use navigation arrows to access other pages.

### Custom AutoEllipsisText

Change the ellipsis button text:

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       AutoEllipsisMode="After"
                       AutoEllipsisText="...etc"
                       NumericButtonCount="10"
                       PageSize="16"
                       Source="{Binding OrdersDetails}"/>
```

**Display:** `< 1 2 3 4 5 6 7 8 9 10 ...etc >`

You can use any text:
- "...more" 
- "»"
- "More Pages"
- Custom symbols

### When to Use AutoEllipsis

**Use `After`:**
- Most common pattern for left-to-right interfaces
- Users typically navigate forward through pages
- Standard pagination behavior

**Use `Before`:**
- Right-to-left (RTL) languages
- When emphasizing backward navigation
- Uncommon but useful for specific workflows

**Use `Both`:**
- Large page counts (50+ pages)
- Allow quick navigation in both directions
- Users need to jump between early and late pages

**Use `None`:**
- Small page counts (fits in NumericButtonCount)
- Simple pagination without page jumping
- Minimalist UI requirements

## AccentBrush Properties

AccentBrush properties control the color scheme of the DataPager control, allowing you to match your application's theme.

### AccentBackground Property

Applies background color to:
- Navigation buttons (First, Previous, Next, Last)
- Currently selected numeric page button

**Default:** DarkGray

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       AccentBackground="DodgerBlue"
                       PageSize="16"
                       Source="{Binding OrdersDetails}"/>
```

### AccentForeground Property

Applies foreground (text) color to:
- Currently selected numeric page button text

**Default:** White

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       AccentBackground="#FF8CBF26"
                       AccentForeground="White"
                       PageSize="16"
                       Source="{Binding OrdersDetails}"/>
```

### Combined AccentBrush Example

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <sfgrid:SfDataGrid AutoGenerateColumns="True"
                       ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           AccentBackground="#FF8CBF26"
                           AccentForeground="White"
                           NumericButtonCount="10"
                           PageSize="16"
                           Source="{Binding OrdersDetails}"/>
</Grid>
```

This creates a green theme with white text on the selected page button.

### Color Options

**Common AccentBackground colors:**
- `"DodgerBlue"` - Modern blue
- `"#FF8CBF26"` - Lime green
- `"CornflowerBlue"` - Soft blue
- `"DarkSlateGray"` - Dark theme
- `"OrangeRed"` - Attention-grabbing
- `"MediumPurple"` - Purple theme
- Theme-based: `"{DynamicResource PrimaryBrush}"`

**Common AccentForeground colors:**
- `"White"` - High contrast on dark backgrounds
- `"Black"` - High contrast on light backgrounds
- `"Yellow"` - Highlight on dark backgrounds
- Theme-based: `"{DynamicResource PrimaryTextBrush}"`

### NumericButtonStyle Property

Apply custom styles to numeric buttons for advanced customization.

**Type:** Style  
**Default:** Null

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Window.Resources>
    <Style TargetType="datapager:NumericButton">
        <Setter Property="BorderBrush" Value="Blue"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Margin" Value="2"/>
        <Setter Property="FontWeight" Value="Bold"/>
    </Style>
</Window.Resources>

<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <sfgrid:SfDataGrid AutoGenerateColumns="True"
                       ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           AutoEllipsisMode="Both"
                           AccentBackground="DodgerBlue"
                           NumericButtonCount="10"
                           PageSize="16"
                           Source="{Binding OrdersDetails}"/>
</Grid>
```

## DisplayMode Options

The `DisplayMode` property controls which buttons are visible in the DataPager. This is an enum property with 12 different values.

### DisplayMode Enum Values

#### FirstLastPreviousNextNumeric (Default)

Displays all navigation buttons and numeric page buttons.

```xml
<datapager:SfDataPager DisplayMode="FirstLastPreviousNextNumeric"/>
```

**Display:** `|< < 1 2 3 4 5 > >|`

**Use when:** Full navigation control is needed.

#### FirstLastNumeric

Displays first page, last page navigation buttons, and numeric page buttons.

```xml
<datapager:SfDataPager DisplayMode="FirstLastNumeric"/>
```

**Display:** `|< 1 2 3 4 5 >|`

**Use when:** Incremental navigation (Previous/Next) is not needed.

#### PreviousNextNumeric

Displays previous, next page navigation buttons, and numeric page buttons.

```xml
<datapager:SfDataPager DisplayMode="PreviousNextNumeric"/>
```

**Display:** `< 1 2 3 4 5 >`

**Use when:** Jump-to-end functionality is not needed.

#### FirstLastPreviousNext

Displays only page navigation buttons. No numeric page buttons.

```xml
<datapager:SfDataPager DisplayMode="FirstLastPreviousNext"/>
```

**Display:** `|< < > >|`

**Use when:** Minimal UI, sequential navigation only, limited space.

#### FirstLast

Displays only first and last page navigation buttons.

```xml
<datapager:SfDataPager DisplayMode="FirstLast"/>
```

**Display:** `|< >|`

**Use when:** Jump to start/end only, very limited space.

#### PreviousNext

Displays only previous and next page navigation buttons.

```xml
<datapager:SfDataPager DisplayMode="PreviousNext"/>
```

**Display:** `< >`

**Use when:** Sequential navigation, minimalist design, mobile interfaces.

#### Numeric

Displays only numeric page buttons.

```xml
<datapager:SfDataPager DisplayMode="Numeric"/>
```

**Display:** `1 2 3 4 5`

**Use when:** Direct page selection preferred, clean appearance.

#### First

Displays only the first page navigation button.

```xml
<datapager:SfDataPager DisplayMode="First"/>
```

**Display:** `|<`

**Use when:** Custom navigation UI, only "back to start" needed.

#### Last

Displays only the last page navigation button.

```xml
<datapager:SfDataPager DisplayMode="Last"/>
```

**Display:** `>|`

**Use when:** Custom navigation UI, only "jump to end" needed.

#### Previous

Displays only the previous page navigation button.

```xml
<datapager:SfDataPager DisplayMode="Previous"/>
```

**Display:** `<`

**Use when:** Backward-only navigation, wizard-style interfaces.

#### Next

Displays only the next page navigation button.

```xml
<datapager:SfDataPager DisplayMode="Next"/>
```

**Display:** `>`

**Use when:** Forward-only navigation, tutorial/walkthrough interfaces.

#### None

Displays nothing (programmatic control only).

```xml
<datapager:SfDataPager DisplayMode="None"/>
```

**Use when:** Completely custom UI, page indicator only.

### DisplayMode Selection Guide

| Scenario | Recommended DisplayMode |
|----------|------------------------|
| Standard data grid | FirstLastPreviousNextNumeric |
| Limited horizontal space | PreviousNext or Numeric |
| Mobile/responsive | PreviousNext |
| Few pages (2-10) | Numeric |
| Many pages (50+) | FirstLastPreviousNextNumeric with AutoEllipsis |
| Wizard/tutorial | Next or PreviousNext |
| Custom navigation | None or individual buttons |
| Vertical layout | PreviousNext or FirstLast |

## Orientation

Controls whether buttons are arranged horizontally or vertically. This is an enum property.

### Orientation Values

#### Horizontal (Default)

Arranges all navigation and numeric buttons horizontally.

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       Orientation="Horizontal"
                       DisplayMode="FirstLastPreviousNextNumeric"
                       NumericButtonCount="5"
                       Source="{Binding DataCollection}"/>
```

**Layout:** `|< < 1 2 3 4 5 > >|` (left to right)

**Use when:**
- Standard horizontal layouts
- Bottom or top of data grid
- Wide display area available
- Most common use case

#### Vertical

Arranges all navigation and numeric buttons vertically.

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       Orientation="Vertical"
                       DisplayMode="FirstLastPreviousNextNumeric"
                       NumericButtonCount="5"
                       Source="{Binding DataCollection}"/>
```

**Layout:** Buttons stacked top to bottom:
```
|<
<
1
2
3
4
5
>
>|
```

**Use when:**
- Side panel layouts
- Limited horizontal space
- Vertical scrolling interfaces
- Sidebar navigation patterns

### Orientation with Grid Layout

**Horizontal at bottom:**

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <sfgrid:SfDataGrid ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           Orientation="Horizontal"
                           Source="{Binding DataCollection}"/>
</Grid>
```

**Vertical on right side:**

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*"/>
        <ColumnDefinition Width="Auto"/>
    </Grid.ColumnDefinitions>
    
    <sfgrid:SfDataGrid ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Column="1"
                           Orientation="Vertical"
                           DisplayMode="PreviousNextNumeric"
                           Source="{Binding DataCollection}"/>
</Grid>
```

## Best Practices

### Color Selection
- Choose high-contrast colors for AccentBackground and AccentForeground
- Test accessibility (WCAG AA standard: 4.5:1 contrast ratio minimum)
- Use theme-based brushes for consistency: `{DynamicResource PrimaryBrush}`
- Consider dark mode: provide alternative colors
- Match application's existing color scheme

### AutoEllipsis Configuration
- Use `After` for most scenarios (standard behavior)
- Set `NumericButtonCount` based on available space:
  - Mobile: 3-5 buttons
  - Desktop: 7-10 buttons
  - Large screens: 10-15 buttons
- Enable `Both` mode for large datasets (50+ pages)
- Keep AutoEllipsisText short (1-5 characters)

### DisplayMode Selection
- Start with `FirstLastPreviousNextNumeric` as default
- Simplify for mobile: `PreviousNext` or `Numeric` only
- Match user's mental model (e.g., `Next` only for wizards)
- Consider page count:
  - Few pages (2-10): `Numeric` sufficient
  - Many pages (50+): Need First/Last buttons

### Orientation Choice
- Default to `Horizontal` unless space constrained
- Use `Vertical` for:
  - Sidebar layouts
  - Narrow panels
  - Portrait mobile views
- Adjust `DisplayMode` with `Vertical`:
  - Fewer buttons (avoid long vertical list)
  - `PreviousNext` or `FirstLast` work well vertically

### Responsive Design
- Adapt DisplayMode based on window size
- Hide numeric buttons on small screens
- Provide PageSize selector for user control
- Test on various screen resolutions

### NumericButtonStyle
- Use sparingly (default style usually sufficient)
- Ensure custom styles don't reduce usability
- Maintain clear selected state indication
- Test hover and click states

## Troubleshooting

### Ellipsis button not appearing
- Check that PageCount > NumericButtonCount
- Verify AutoEllipsisMode is not set to None
- Ensure Source has sufficient data

### Colors not applying
- Verify property names are correct (AccentBackground, not Background)
- Check for overriding styles in Window.Resources
- Confirm XAML syntax for color values
- Test with simple color names first ("Red", "Blue")

### Buttons overlapping in Vertical mode
- Reduce NumericButtonCount
- Simplify DisplayMode
- Add margin/padding to buttons
- Ensure container has sufficient height

### Custom style not working
- Verify TargetType="datapager:NumericButton"
- Check namespace declaration
- Ensure style is defined before DataPager
- Remove NumericButtonStyle property to use implicit style

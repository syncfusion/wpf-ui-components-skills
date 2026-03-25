---
name: syncfusion-wpf-radial-menu
description: Implements the Syncfusion WPF SfRadialMenu for circular/contextual menu layouts and hierarchical circular navigation. Use when you need drill-down circular menus, radial item layouts, or compact contextual command sets.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Radial Menu

The **Syncfusion WPF Radial Menu (SfRadialMenu)** displays a hierarchical menu in a circular layout, exposing more menu items in the same space than traditional menus. It's ideal for context menus, tool palettes, and touch-enabled interfaces.

## When to Use This Skill

Use this skill when the user needs to:
- Implement a **radial menu** (circular menu layout) in WPF applications
- Create **context menus** with hierarchical circular navigation
- Display menu items in a **circular layout** around a center point
- Implement **drill-down navigation** in a radial pattern
- Create **color palettes** with radial menu structure
- Build touch-friendly circular menus for kiosk or tablet applications
- Show more menu options in the same space compared to traditional linear menus

---

## Component Overview

**SfRadialMenu** is a circular menu control that arranges menu items radially around a center point. It supports:
- Hierarchical navigation with drill-down capability
- Data binding with business object collections
- Custom and default layout modes
- Icon customization for center and menu items
- Extensive appearance and styling options
- Color palette support with SfRadialColorItem
- Tooltip configuration
- Command binding and event handling

**Namespace:** `Syncfusion.Windows.Controls.Navigation`  
**Assembly:** `Syncfusion.SfRadialMenu.WPF`

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Namespace and assembly setup
- Basic XAML and code-behind implementation
- Opening and closing the menu with Show() and Close() methods
- Event handling: Opened, Closed, Navigating
- IsOpen property for initial state
- Theme support with SfSkinManager

### Populating Items and Data Binding
📄 **Read:** [references/populating-items.md](references/populating-items.md)
- Binding ItemsSource to business object collections
- DisplayMemberPath for header text
- CommandPath for command binding
- ItemTemplate for custom item appearance
- DrillDownItem for tracking navigation
- EnableFreeRotation for manual rotation
- Command and CommandParameter properties
- CloseOnExecute behavior
- CheckMode options (None, CheckBox, Radio)
- GroupName for radio button groups
- IsChecked for checkbox/radio states
- SfRadialMenuItem events (Click, Checked, UnChecked)

### Layout Configuration
📄 **Read:** [references/layout-types.md](references/layout-types.md)
- Default layout type (automatic segmentation)
- Custom layout type with VisibleSegmentsCount
- SegmentIndex for precise item positioning
- Controlling segment distribution
- Layout type selection guide

### Icons and Visual Elements
📄 **Read:** [references/icons.md](references/icons.md)
- Center Icon property for menu center customization
- SfRadialMenuItem Icon property for item icons
- Using Image elements and custom UIElements
- Icon sizing and stretching options

### Appearance and Styling
📄 **Read:** [references/appearance-and-styling.md](references/appearance-and-styling.md)
- RadiusX and RadiusY for menu size
- CenterRimRadiusFactor for inner circle size
- Rim brushes: RimBackground, RimActiveBrush, RimInactiveBrush, RimHoverBrush
- IsExpanderVisible for showing/hiding expander arrows
- RimRadiusFactor for outer rim thickness
- Per-item customization: MenuBackgroundColor, MenuMouseOverBackgroundColor
- ShowMouseOverStyle property
- CenterBackButtonForeground for navigation button
- Separator styling: SeparatorBackground, SeparatorWidth
- Stroke properties: StrokeThickness, MouseOverRimStrokeThickness
- MouseOverRimStyle for advanced hover customization

### Tooltips
📄 **Read:** [references/tooltips.md](references/tooltips.md)
- ToolTip property for item tooltips
- ToolTipPlacement options (None, Left, Top, Right, Bottom)
- Positioning tooltips relative to the menu

### Color Palette
📄 **Read:** [references/color-palette.md](references/color-palette.md)
- SfRadialColorItem for color selection
- Creating hierarchical color palettes
- Nested color items for color variants
- Building color picker interfaces

---

## Quick Start Example

### Basic Radial Menu in XAML

```xaml
<Window xmlns:navigation="clr-namespace:Syncfusion.Windows.Controls.Navigation;assembly=Syncfusion.SfRadialMenu.Wpf">
    <Grid>
        <navigation:SfRadialMenu IsOpen="True">
            <navigation:SfRadialMenuItem Header="Bold"/>
            <navigation:SfRadialMenuItem Header="Cut"/>
            <navigation:SfRadialMenuItem Header="Copy"/>
            <navigation:SfRadialMenuItem Header="Paste"/>
        </navigation:SfRadialMenu>
    </Grid>
</Window>
```

### Basic Radial Menu in Code-Behind

```csharp
using Syncfusion.Windows.Controls.Navigation;

SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.IsOpen = true;

radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Bold" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Cut" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Copy" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Paste" });
```

### Hierarchical Menu with Sub-Items

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Format">
        <navigation:SfRadialMenuItem Header="Bold"/>
        <navigation:SfRadialMenuItem Header="Italic"/>
        <navigation:SfRadialMenuItem Header="Underline"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Edit">
        <navigation:SfRadialMenuItem Header="Cut"/>
        <navigation:SfRadialMenuItem Header="Copy"/>
        <navigation:SfRadialMenuItem Header="Paste"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Undo"/>
    <navigation:SfRadialMenuItem Header="Redo"/>
</navigation:SfRadialMenu>
```

---

## Common Patterns

### Opening Menu on Button Click

```xaml
<Grid>
    <navigation:SfRadialMenu x:Name="radialMenu">
        <navigation:SfRadialMenuItem Header="Bold"/>
        <navigation:SfRadialMenuItem Header="Copy"/>
        <navigation:SfRadialMenuItem Header="Paste"/>
    </navigation:SfRadialMenu>
    
    <Button Content="Open Menu" Click="OpenMenu_Click"/>
</Grid>
```

```csharp
private void OpenMenu_Click(object sender, RoutedEventArgs e)
{
    radialMenu.Show();
}
```

### Data Binding with Business Objects

```csharp
// Model
public class EditorCommand
{
    public string Name { get; set; }
    public string IconPath { get; set; }
    public ICommand Command { get; set; }
}

// ViewModel
public class EditorViewModel
{
    public List<EditorCommand> Commands { get; set; }
    
    public EditorViewModel()
    {
        Commands = new List<EditorCommand>
        {
            new EditorCommand { Name = "Bold", IconPath = "bold.png" },
            new EditorCommand { Name = "Copy", IconPath = "copy.png" },
            new EditorCommand { Name = "Paste", IconPath = "paste.png" }
        };
    }
}
```

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         ItemsSource="{Binding Commands}"
                         DisplayMemberPath="Name"
                         CommandPath="Command"/>
```

### Custom Layout with Specific Positioning

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         LayoutType="Custom" 
                         VisibleSegmentsCount="8">
    <navigation:SfRadialMenuItem Header="Item 1" SegmentIndex="0"/>
    <navigation:SfRadialMenuItem Header="Item 2" SegmentIndex="2"/>
    <navigation:SfRadialMenuItem Header="Item 3" SegmentIndex="4"/>
    <navigation:SfRadialMenuItem Header="Item 4" SegmentIndex="6"/>
</navigation:SfRadialMenu>
```

### Styling with Rim Colors

```xaml
<navigation:SfRadialMenu IsOpen="True"
                         RimBackground="LightBlue"
                         RimActiveBrush="Orange"
                         RimHoverBrush="DarkOrange"
                         CenterRimRadiusFactor="0.3">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

### Menu Items with Commands and Parameters

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold"
                                 Command="{Binding BoldCommand}"
                                 CommandParameter="Bold"
                                 CloseOnExecute="True"/>
    <navigation:SfRadialMenuItem Header="Copy"
                                 Command="{Binding CopyCommand}"
                                 CommandParameter="Copy"
                                 CloseOnExecute="True"/>
</navigation:SfRadialMenu>
```

---

## Key Properties and Events

### SfRadialMenu Properties

| Property | Type | Description |
|----------|------|-------------|
| **IsOpen** | bool | Gets or sets whether the radial menu is open |
| **ItemsSource** | IEnumerable | Business object collection for data binding |
| **DisplayMemberPath** | string | Property path for item headers |
| **CommandPath** | string | Property path for item commands |
| **LayoutType** | LayoutType | Default or Custom layout mode |
| **VisibleSegmentsCount** | int | Number of segments in Custom layout |
| **EnableFreeRotation** | bool | Allows manual rotation of the menu |
| **RadiusX** | double | Horizontal radius of the menu |
| **RadiusY** | double | Vertical radius of the menu |
| **RimBackground** | Brush | Outer rim background color |
| **RimActiveBrush** | Brush | Active expander rim color |
| **RimHoverBrush** | Brush | Hover state rim color |
| **CenterRimRadiusFactor** | double | Inner circle size (0.0 to 1.0) |

### SfRadialMenuItem Properties

| Property | Type | Description |
|----------|------|-------------|
| **Header** | object | Content displayed in the menu item |
| **Icon** | UIElement | Custom icon for the item |
| **Command** | ICommand | Command to execute on click |
| **CommandParameter** | object | Parameter passed to the command |
| **SegmentIndex** | int | Position in Custom layout mode |
| **CloseOnExecute** | bool | Close menu after item click |
| **CheckMode** | CheckMode | None, CheckBox, or Radio behavior |
| **IsChecked** | bool | Checked state for CheckBox/Radio |
| **GroupName** | string | Radio button group name |
| **ToolTip** | string | Tooltip text |
| **ToolTipPlacement** | Placement | Tooltip position |
| **MenuBackgroundColor** | Brush | Item background color |

### Events

| Event | Description |
|-------|-------------|
| **Opened** | Fired when the radial menu opens |
| **Closed** | Fired when the radial menu closes |
| **Navigating** | Fired when navigating into sub-items (cancellable) |
| **Click** (MenuItem) | Fired when a menu item is clicked |
| **Checked** (MenuItem) | Fired when a CheckBox/Radio item is checked |
| **UnChecked** (MenuItem) | Fired when a CheckBox item is unchecked |

---

## Common Use Cases

### 1. Context Menu for Text Editor
Create a radial menu with formatting options (Bold, Italic, Underline, Font Size, Color) that appears on right-click.

### 2. Graphics Editor Tool Palette
Implement a tool selector with drawing tools (Pen, Brush, Eraser, Shapes) in a circular layout.

### 3. Color Picker with Radial Layout
Build a color palette using SfRadialColorItem with primary colors at the first level and shades at the second level.

### 4. Touch-Enabled Kiosk Navigation
Design a touch-friendly navigation menu for kiosk applications where circular layout provides better accessibility.

### 5. Command Palette with Sub-Commands
Create a hierarchical command menu where top-level items represent categories and sub-items are specific commands.

### 6. Quick Action Menu
Implement a floating action button that opens a radial menu with common actions (Edit, Delete, Share, More).

---

## Implementation Guidance

When the user wants to implement a radial menu:

1. **Start with the basics** → Guide them to [getting-started.md](references/getting-started.md) for setup and basic implementation
2. **For data binding** → Direct them to [populating-items.md](references/populating-items.md) for ItemsSource, templates, and commands
3. **For precise layout control** → Show them [layout-types.md](references/layout-types.md) for Custom layout and SegmentIndex
4. **For visual customization** → Point to [icons.md](references/icons.md) and [appearance-and-styling.md](references/appearance-and-styling.md)
5. **For color selection UI** → Guide to [color-palette.md](references/color-palette.md) for SfRadialColorItem usage
6. **For tooltips** → Refer to [tooltips.md](references/tooltips.md) for tooltip configuration

Always consider:
- **Space constraints**: Radial menus expose more items in limited space
- **Touch vs mouse**: Larger radius for touch interfaces
- **Hierarchy depth**: Keep menu levels to 2-3 for usability
- **Item count**: Optimal range is 4-8 items per level
- **Visual feedback**: Use RimHoverBrush and MenuMouseOverBackgroundColor for clear hover states

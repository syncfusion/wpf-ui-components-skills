# Positioning and Layout

This guide covers band positioning, overflow handling, gripper controls, and toolbar orientation for organizing multiple ToolBarAdv controls.

## Table of Contents
- [Band and BandIndex Properties](#band-and-bandindex-properties)
- [Multiple Toolbars in Same Band](#multiple-toolbars-in-same-band)
- [Overflow Mode Control](#overflow-mode-control)
- [Gripper Visibility](#gripper-visibility)
- [ToolBarTrayAdv Orientation](#toolbartrayadv-orientation)

## Band and BandIndex Properties

The `Band` and `BandIndex` properties control where toolbars appear within a ToolBarTrayAdv.

### Band Property

**Purpose:** Specifies which row (horizontal orientation) or column (vertical orientation) the toolbar occupies.

**Type:** `int` (0-based index)

**When to Use:** 
- Organize toolbars into separate rows/columns
- Create logical groupings (File band, Edit band, Format band)
- Control vertical stacking order

**Example - Multiple Bands:**

```xaml
<syncfusion:ToolBarTrayAdv>
    <!-- Band 0 - File operations -->
    <syncfusion:ToolBarAdv Band="0" ToolBarName="File">
        <Button Content="New"/>
        <Button Content="Open"/>
        <Button Content="Save"/>
    </syncfusion:ToolBarAdv>
    
    <!-- Band 1 - Edit operations -->
    <syncfusion:ToolBarAdv Band="1" ToolBarName="Edit">
        <Button Content="Cut"/>
        <Button Content="Copy"/>
        <Button Content="Paste"/>
    </syncfusion:ToolBarAdv>
    
    <!-- Band 2 - Format operations -->
    <syncfusion:ToolBarAdv Band="2" ToolBarName="Format">
        <Button Content="Bold"/>
        <Button Content="Italic"/>
        <Button Content="Underline"/>
    </syncfusion:ToolBarAdv>
</syncfusion:ToolBarTrayAdv>
```

**Result:** Three separate rows of toolbars, stacked vertically.

### BandIndex Property

**Purpose:** Specifies the position within a band when multiple toolbars share the same band.

**Type:** `int` (0-based index)

**When to Use:**
- Place multiple toolbars side-by-side in the same row
- Control left-to-right order (horizontal) or top-to-bottom order (vertical)
- Fine-tune toolbar arrangement

**Example - Multiple Toolbars in Same Band:**

```xaml
<syncfusion:ToolBarTrayAdv>
    <!-- First toolbar on Band 0 -->
    <syncfusion:ToolBarAdv Band="0" BandIndex="0" ToolBarName="File">
        <Button Content="New"/>
        <Button Content="Open"/>
    </syncfusion:ToolBarAdv>
    
    <!-- Second toolbar on same Band 0 -->
    <syncfusion:ToolBarAdv Band="0" BandIndex="1" ToolBarName="Extras">
        <Button Content="Print"/>
        <Button Content="Preview"/>
    </syncfusion:ToolBarAdv>
    
    <!-- Third toolbar also on Band 0 -->
    <syncfusion:ToolBarAdv Band="0" BandIndex="2" ToolBarName="Tools">
        <Button Content="Options"/>
    </syncfusion:ToolBarAdv>
</syncfusion:ToolBarTrayAdv>
```

**Result:** Three toolbars arranged horizontally in a single row.

### Setting Band Properties in C#

```csharp
ToolBarAdv fileToolBar = new ToolBarAdv
{
    ToolBarName = "File",
    Band = 0,
    BandIndex = 0
};

ToolBarAdv editToolBar = new ToolBarAdv
{
    ToolBarName = "Edit",
    Band = 0,
    BandIndex = 1
};

ToolBarAdv formatToolBar = new ToolBarAdv
{
    ToolBarName = "Format",
    Band = 1,
    BandIndex = 0
};

ToolBarTrayAdv tray = new ToolBarTrayAdv();
tray.ToolBars.Add(fileToolBar);
tray.ToolBars.Add(editToolBar);
tray.ToolBars.Add(formatToolBar);
```

## Multiple Toolbars in Same Band

### Horizontal Arrangement

When multiple toolbars share the same `Band` value, they arrange horizontally (left-to-right by default):

```xaml
<syncfusion:ToolBarTrayAdv>
    <syncfusion:ToolBarAdv Band="0" BandIndex="0" ToolBarName="Primary">
        <Button Content="A"/>
        <Button Content="B"/>
        <Button Content="C"/>
    </syncfusion:ToolBarAdv>
    
    <syncfusion:ToolBarAdv Band="0" BandIndex="1" ToolBarName="Secondary">
        <Button Content="D"/>
        <Button Content="E"/>
    </syncfusion:ToolBarAdv>
</syncfusion:ToolBarTrayAdv>
```

**Result:** `[A][B][C] | [D][E]` in one row

### Dynamic Repositioning

Users can drag toolbars by their gripper to reposition them. The `Band` and `BandIndex` properties update automatically when users reposition toolbars.

## Overflow Mode Control

The `OverflowMode` attached property controls how toolbar items behave when space is limited.

### OverflowMode Values

| Mode | Description | When to Use |
|------|-------------|-------------|
| `AsNeeded` (default) | Item moves to overflow when toolbar shrinks | Most toolbar items |
| `Never` | Item always remains visible, toolbar forces scrolling | Critical commands |
| `Always` | Item always appears in overflow panel | Advanced/rare commands |

### Setting OverflowMode in XAML

```xaml
<syncfusion:ToolBarAdv>
    <!-- Critical - always visible -->
    <Button Content="Save" 
            syncfusion:ToolBarAdv.OverflowMode="Never"
            ToolTip="Save (always visible)"/>
    
    <!-- Normal - overflow when needed -->
    <Button Content="Open" 
            syncfusion:ToolBarAdv.OverflowMode="AsNeeded"
            ToolTip="Open"/>
    
    <Button Content="Print" 
            syncfusion:ToolBarAdv.OverflowMode="AsNeeded"
            ToolTip="Print"/>
    
    <!-- Advanced - always in overflow -->
    <Button Content="Options" 
            syncfusion:ToolBarAdv.OverflowMode="Always"
            ToolTip="Advanced options"/>
</syncfusion:ToolBarAdv>
```

### Setting OverflowMode in C#

```csharp
Button saveButton = new Button { Content = "Save" };
ToolBarAdv.SetOverflowMode(saveButton, OverflowMode.Never);

Button openButton = new Button { Content = "Open" };
ToolBarAdv.SetOverflowMode(openButton, OverflowMode.AsNeeded);

Button optionsButton = new Button { Content = "Options" };
ToolBarAdv.SetOverflowMode(optionsButton, OverflowMode.Always);

toolBar.Items.Add(saveButton);
toolBar.Items.Add(openButton);
toolBar.Items.Add(optionsButton);
```

### Overflow Panel Behavior

**Overflow Indicator:** When items overflow, a chevron button (`>>`) appears on the toolbar.

**Clicking Chevron:** Opens a popup showing overflow items.

**Programmatic Control:**

```csharp
// Check if overflow is open
bool isOpen = toolBar.IsOverflowOpen;

// Open overflow programmatically
toolBar.IsOverflowOpen = true;

// Close overflow
toolBar.IsOverflowOpen = false;
```

## Gripper Visibility

The gripper is the handle users drag to reposition toolbars.

### GripperVisibility Property

**Type:** `Visibility` (`Visible`, `Collapsed`, `Hidden`)

**Purpose:** Control whether users can drag and reposition the toolbar.

### Show Gripper (Default)

```xaml
<syncfusion:ToolBarAdv GripperVisibility="Visible">
    <!-- Users can drag this toolbar -->
    <Button Content="Movable"/>
</syncfusion:ToolBarAdv>
```

### Hide Gripper

```xaml
<syncfusion:ToolBarAdv GripperVisibility="Collapsed">
    <!-- Users cannot drag this toolbar -->
    <Button Content="Fixed Position"/>
</syncfusion:ToolBarAdv>
```

**When to Hide Gripper:**
- Toolbar should remain in fixed position
- Preventing user customization
- Application requires consistent layout
- Toolbar is only toolbar in application

### C# Example

```csharp
// Allow repositioning
toolBar.GripperVisibility = Visibility.Visible;

// Prevent repositioning
toolBar.GripperVisibility = Visibility.Collapsed;
```

### Styling the Gripper

Customize gripper appearance through styles:

```xaml
<Window.Resources>
    <Style TargetType="syncfusion:ToolBarAdv">
        <Setter Property="GripperStyle">
            <Setter.Value>
                <Style TargetType="Border">
                    <Setter Property="Background" Value="DarkGray"/>
                    <Setter Property="Width" Value="5"/>
                    <Setter Property="Cursor" Value="SizeAll"/>
                </Style>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>
```

## ToolBarTrayAdv Orientation

Control whether toolbars stack horizontally (side-by-side) or vertically (top-to-bottom).

### Orientation Property

**Type:** `Orientation` (`Horizontal`, `Vertical`)

**Default:** `Horizontal`

### Horizontal Orientation (Default)

```xaml
<syncfusion:ToolBarTrayAdv Orientation="Horizontal">
    <!-- Toolbars stack in rows -->
    <syncfusion:ToolBarAdv Band="0" ToolBarName="Row 1">
        <Button Content="A"/>
    </syncfusion:ToolBarAdv>
    
    <syncfusion:ToolBarAdv Band="1" ToolBarName="Row 2">
        <Button Content="B"/>
    </syncfusion:ToolBarAdv>
</syncfusion:ToolBarTrayAdv>
```

**Result:**
```
[A] [B] [C]  ← Band 0
[D] [E]      ← Band 1
```

### Vertical Orientation

```xaml
<syncfusion:ToolBarTrayAdv Orientation="Vertical">
    <!-- Toolbars stack in columns -->
    <syncfusion:ToolBarAdv Band="0" ToolBarName="Column 1">
        <Button Content="A"/>
        <Button Content="B"/>
    </syncfusion:ToolBarAdv>
    
    <syncfusion:ToolBarAdv Band="1" ToolBarName="Column 2">
        <Button Content="C"/>
        <Button Content="D"/>
    </syncfusion:ToolBarAdv>
</syncfusion:ToolBarTrayAdv>
```

**Result:**
```
[A] [C]  ← Band 0 | Band 1
[B] [D]
```

### Use Cases

**Horizontal Orientation:**
- Traditional menu bar layout (top of window)
- Most common toolbar arrangement
- File/Edit/Format toolbars

**Vertical Orientation:**
- Left or right sidebar toolbars
- Tool palettes (graphics editors)
- Docked side panels

### Combined with ToolBarManager

```xaml
<syncfusion:ToolBarManager>
    <!-- Top: Horizontal orientation -->
    <syncfusion:ToolBarManager.TopToolBarTray>
        <syncfusion:ToolBarTrayAdv Orientation="Horizontal">
            <syncfusion:ToolBarAdv ToolBarName="File">
                <Button Content="New"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.TopToolBarTray>
    
    <!-- Left: Vertical orientation -->
    <syncfusion:ToolBarManager.LeftToolBarTray>
        <syncfusion:ToolBarTrayAdv Orientation="Vertical">
            <syncfusion:ToolBarAdv ToolBarName="Tools">
                <Button Content="Draw" Width="32" Height="32"/>
                <Button Content="Erase" Width="32" Height="32"/>
            </syncfusion:ToolBarAdv>
        </syncfusion:ToolBarTrayAdv>
    </syncfusion:ToolBarManager.LeftToolBarTray>
    
    <syncfusion:ToolBarManager.Content>
        <TextBox/>
    </syncfusion:ToolBarManager.Content>
</syncfusion:ToolBarManager>
```

## Quick Reference

| Property | Values | Purpose |
|----------|--------|---------|
| `Band` | int (0-based) | Row/column the toolbar occupies |
| `BandIndex` | int (0-based) | Position within a band |
| `OverflowMode` | `AsNeeded`, `Never`, `Always` | Item overflow behavior |
| `GripperVisibility` | `Visible`, `Collapsed` | Allow/prevent user repositioning |
| `Orientation` | `Horizontal`, `Vertical` | Tray stacking direction |

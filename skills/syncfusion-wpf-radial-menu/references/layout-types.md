# Layout Types in WPF Radial Menu

The SfRadialMenu supports two different layout types that control how menu items are arranged in the circular panel: **Default** and **Custom**. Both layout types divide the available space among children in the circular panel, but they differ in how segmentation is determined.

---

## Overview

| Layout Type | Segmentation | Item Ordering | Use Case |
|-------------|-------------|---------------|----------|
| **Default** | Automatic (based on item count) | Sequential | Simple menus with dynamic item counts |
| **Custom** | Fixed (via VisibleSegmentsCount) | Positioned (via SegmentIndex) | Precise control over item placement |

---

## Default Layout Type

In Default layout, the number of segments in the panel is automatically determined by the number of children at each level. Each hierarchical level can have a different segment count based on its item count.

### Characteristics

- **Automatic Segmentation:** Segments equal the number of items at that level
- **Sequential Ordering:** Items are arranged in the order they are added
- **Variable Segments:** Each level can have different segment counts
- **Simple Setup:** No additional properties required

### Basic Example

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Cut"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

In this example, the menu has 4 items, so the circle is automatically divided into 4 equal segments.

### Code-Behind Example

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.IsOpen = true;
// LayoutType defaults to Default

radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Item 1" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Item 2" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Item 3" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Item 4" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Item 5" });

// Circle is divided into 5 equal segments
```

### Hierarchical Example

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <!-- Top level: 3 items = 3 segments -->
    <navigation:SfRadialMenuItem Header="Format">
        <!-- Sub-level: 4 items = 4 segments -->
        <navigation:SfRadialMenuItem Header="Bold"/>
        <navigation:SfRadialMenuItem Header="Italic"/>
        <navigation:SfRadialMenuItem Header="Underline"/>
        <navigation:SfRadialMenuItem Header="Strikethrough"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Edit">
        <!-- Sub-level: 3 items = 3 segments -->
        <navigation:SfRadialMenuItem Header="Cut"/>
        <navigation:SfRadialMenuItem Header="Copy"/>
        <navigation:SfRadialMenuItem Header="Paste"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Undo"/>
</navigation:SfRadialMenu>
```

### When to Use Default Layout

**Use Default Layout when:**
- Item count varies dynamically
- Simple sequential arrangement is sufficient
- You don't need precise control over item positions
- Items are evenly distributed by nature
- Implementing MVVM with data binding

---

## Custom Layout Type

In Custom layout, the number of segments is fixed using the `VisibleSegmentsCount` property. Items can be positioned at specific segments using the `SegmentIndex` property.

### Characteristics

- **Fixed Segmentation:** All levels have the same segment count
- **Positioned Ordering:** Items placed at specific indices
- **Consistent Layout:** Same segment count across all levels
- **Precise Control:** Allows gaps and specific positioning

### Setting Custom Layout

```xaml
<navigation:SfRadialMenu LayoutType="Custom" VisibleSegmentsCount="7"/>
```

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.LayoutType = LayoutType.Custom;
radialMenu.VisibleSegmentsCount = 7;
```

---

## VisibleSegmentsCount Property

The `VisibleSegmentsCount` property specifies the total number of segments available in the circular panel when using Custom layout type.

### Behavior Rules

1. **More items than segments:** Overflowing items are not displayed
2. **Fewer items than segments:** Remaining segments are left empty
3. **Applies to all levels:** All hierarchical levels use the same segment count

### Example: Creating Gaps

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

This creates a menu with 8 segments but only 4 items, leaving alternating segments empty.

### Example: Overflow Handling

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         LayoutType="Custom" 
                         VisibleSegmentsCount="4">
    <navigation:SfRadialMenuItem Header="Item 1" SegmentIndex="0"/>
    <navigation:SfRadialMenuItem Header="Item 2" SegmentIndex="1"/>
    <navigation:SfRadialMenuItem Header="Item 3" SegmentIndex="2"/>
    <navigation:SfRadialMenuItem Header="Item 4" SegmentIndex="3"/>
    <navigation:SfRadialMenuItem Header="Item 5" SegmentIndex="4"/>
    <!-- Item 5 is not displayed (index exceeds VisibleSegmentsCount) -->
</navigation:SfRadialMenu>
```

---

## SegmentIndex Property

The `SegmentIndex` property specifies the exact segment position (0-based) where a `SfRadialMenuItem` should be placed in Custom layout mode.

### Index Calculation

Segments are numbered starting from 0, proceeding clockwise:
- Index 0: Top position (12 o'clock)
- Index 1: Next position clockwise
- Index 2: Next position clockwise
- And so on...

### Basic Positioning Example

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         LayoutType="Custom" 
                         VisibleSegmentsCount="7">
    <navigation:SfRadialMenuItem Header="Item 2" SegmentIndex="1"/>
    <navigation:SfRadialMenuItem Header="Item 5" SegmentIndex="4"/>
    <navigation:SfRadialMenuItem Header="Item 1" SegmentIndex="0"/>
    <navigation:SfRadialMenuItem Header="Item 6" SegmentIndex="5"/>
    <navigation:SfRadialMenuItem Header="Item 3" SegmentIndex="2"/>
</navigation:SfRadialMenu>
```

Despite being added in random order, items appear at their specified positions.

### Code-Behind Example

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.IsOpen = true;
radialMenu.LayoutType = LayoutType.Custom;
radialMenu.VisibleSegmentsCount = 7;

SfRadialMenuItem item2 = new SfRadialMenuItem() 
{ 
    Header = "Item 2", 
    SegmentIndex = 1 
};

SfRadialMenuItem item5 = new SfRadialMenuItem() 
{ 
    Header = "Item 5", 
    SegmentIndex = 4 
};

SfRadialMenuItem item1 = new SfRadialMenuItem() 
{ 
    Header = "Item 1", 
    SegmentIndex = 0 
};

SfRadialMenuItem item6 = new SfRadialMenuItem() 
{ 
    Header = "Item 6", 
    SegmentIndex = 5 
};

SfRadialMenuItem item3 = new SfRadialMenuItem() 
{ 
    Header = "Item 3", 
    SegmentIndex = 2 
};

radialMenu.Items.Add(item2);
radialMenu.Items.Add(item5);
radialMenu.Items.Add(item1);
radialMenu.Items.Add(item6);
radialMenu.Items.Add(item3);
```

### Handling Duplicate or Missing Indices

When `SegmentIndex` is not specified or multiple items have the same index:

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         LayoutType="Custom" 
                         VisibleSegmentsCount="8">
    <navigation:SfRadialMenuItem Header="Item 1" SegmentIndex="0"/>
    <navigation:SfRadialMenuItem Header="Item 2" SegmentIndex="2"/>
    <navigation:SfRadialMenuItem Header="Item 3"/>
    <!-- No SegmentIndex: placed in next available segment (3) -->
    <navigation:SfRadialMenuItem Header="Item 4" SegmentIndex="2"/>
    <!-- Duplicate index: placed in next available segment (4) -->
</navigation:SfRadialMenu>
```

**Rule:** Items without a specified index or with duplicate indices are automatically placed in the next available free segment.

---

## Layout Comparison Examples

### Scenario 1: Even Distribution

**Default Layout:**
```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="A"/>
    <navigation:SfRadialMenuItem Header="B"/>
    <navigation:SfRadialMenuItem Header="C"/>
    <navigation:SfRadialMenuItem Header="D"/>
</navigation:SfRadialMenu>
```
Result: 4 segments, items evenly distributed

**Custom Layout (Equivalent):**
```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         LayoutType="Custom" 
                         VisibleSegmentsCount="4">
    <navigation:SfRadialMenuItem Header="A" SegmentIndex="0"/>
    <navigation:SfRadialMenuItem Header="B" SegmentIndex="1"/>
    <navigation:SfRadialMenuItem Header="C" SegmentIndex="2"/>
    <navigation:SfRadialMenuItem Header="D" SegmentIndex="3"/>
</navigation:SfRadialMenu>
```
Result: Identical to Default layout

### Scenario 2: Creating Gaps

**Default Layout:** Cannot create gaps (not possible)

**Custom Layout:**
```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         LayoutType="Custom" 
                         VisibleSegmentsCount="8">
    <navigation:SfRadialMenuItem Header="North" SegmentIndex="0"/>
    <navigation:SfRadialMenuItem Header="East" SegmentIndex="2"/>
    <navigation:SfRadialMenuItem Header="South" SegmentIndex="4"/>
    <navigation:SfRadialMenuItem Header="West" SegmentIndex="6"/>
</navigation:SfRadialMenu>
```
Result: Compass-style layout with gaps between items

### Scenario 3: Asymmetric Placement

**Default Layout:** Always symmetric (not possible)

**Custom Layout:**
```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         LayoutType="Custom" 
                         VisibleSegmentsCount="12">
    <!-- Group items on one side -->
    <navigation:SfRadialMenuItem Header="File" SegmentIndex="0"/>
    <navigation:SfRadialMenuItem Header="Edit" SegmentIndex="1"/>
    <navigation:SfRadialMenuItem Header="View" SegmentIndex="2"/>
    
    <!-- Leave gap, then add more items -->
    <navigation:SfRadialMenuItem Header="Help" SegmentIndex="10"/>
    <navigation:SfRadialMenuItem Header="About" SegmentIndex="11"/>
</navigation:SfRadialMenu>
```
Result: Grouped items with large gap

---

## Choosing the Right Layout Type

### Use Default Layout When:

1. **Dynamic item count:** Number of items changes at runtime
2. **Data binding:** Items populated from collections
3. **Simple requirements:** No need for gaps or specific positioning
4. **MVVM pattern:** ViewModel controls item collection
5. **Uniform spacing:** Items should be evenly distributed

**Example Use Cases:**
- Context menus with varying options
- Data-driven menus
- Simple navigation menus

### Use Custom Layout When:

1. **Fixed design:** Layout is predetermined and stable
2. **Precise positioning:** Items must be at specific angles
3. **Intentional gaps:** Empty spaces are part of the design
4. **Consistent hierarchy:** All levels need same segment count
5. **Clock-like layouts:** Items positioned like clock hours

**Example Use Cases:**
- Compass navigation (N, E, S, W)
- Clock-style time selection
- Game control wheels
- Categorized tool palettes

---

## Advanced Patterns

### Pattern 1: Hierarchical Custom Layout

All levels maintain the same segment count:

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         LayoutType="Custom" 
                         VisibleSegmentsCount="6">
    <navigation:SfRadialMenuItem Header="File" SegmentIndex="0">
        <navigation:SfRadialMenuItem Header="New" SegmentIndex="0"/>
        <navigation:SfRadialMenuItem Header="Open" SegmentIndex="1"/>
        <navigation:SfRadialMenuItem Header="Save" SegmentIndex="2"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Edit" SegmentIndex="2">
        <navigation:SfRadialMenuItem Header="Cut" SegmentIndex="0"/>
        <navigation:SfRadialMenuItem Header="Copy" SegmentIndex="1"/>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

Both parent and child levels have 6 segments.

### Pattern 2: Grouped Items with Separators

Use gaps to visually group related items:

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         LayoutType="Custom" 
                         VisibleSegmentsCount="10">
    <!-- Group 1: Text formatting -->
    <navigation:SfRadialMenuItem Header="Bold" SegmentIndex="0"/>
    <navigation:SfRadialMenuItem Header="Italic" SegmentIndex="1"/>
    
    <!-- Gap (segment 2) -->
    
    <!-- Group 2: Clipboard -->
    <navigation:SfRadialMenuItem Header="Cut" SegmentIndex="3"/>
    <navigation:SfRadialMenuItem Header="Copy" SegmentIndex="4"/>
    <navigation:SfRadialMenuItem Header="Paste" SegmentIndex="5"/>
    
    <!-- Gap (segments 6-7) -->
    
    <!-- Group 3: Undo/Redo -->
    <navigation:SfRadialMenuItem Header="Undo" SegmentIndex="8"/>
    <navigation:SfRadialMenuItem Header="Redo" SegmentIndex="9"/>
</navigation:SfRadialMenu>
```

---

## Troubleshooting

### Issue: Items Don't Appear in Custom Layout

**Problem:** Items are added but not visible.

**Cause:** SegmentIndex exceeds VisibleSegmentsCount.

**Solution:** Ensure all SegmentIndex values are less than VisibleSegmentsCount:
```csharp
// Wrong
radialMenu.VisibleSegmentsCount = 4;
item.SegmentIndex = 5; // Won't display

// Correct
radialMenu.VisibleSegmentsCount = 6;
item.SegmentIndex = 5; // Will display
```

### Issue: Items Overlapping

**Problem:** Multiple items appear at the same position.

**Cause:** Duplicate SegmentIndex values or insufficient VisibleSegmentsCount.

**Solution:** Use unique SegmentIndex values or increase VisibleSegmentsCount.

### Issue: Unexpected Item Order

**Problem:** In Default layout, items appear in wrong order.

**Cause:** Items added in incorrect sequence.

**Solution:** Items appear in the order added. Add them in the desired display order or switch to Custom layout for explicit positioning.

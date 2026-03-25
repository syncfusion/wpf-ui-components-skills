# Layout and Orientation

This reference covers controlling how TaskBarItems are arranged: orientation, spacing (margin/padding), uniform width, and flow direction.

## Table of Contents
- [Group Orientation](#group-orientation)
- [GroupOrientationChanged Event](#grouporientationchanged-event)
- [Group Margin](#group-margin)
- [Group Padding](#group-padding)
- [Group Width](#group-width)
- [Flow Direction](#flow-direction)

---

## Group Orientation

Use `GroupOrientation` to arrange `TaskBarItem` groups horizontally or vertically.

| Value | Layout |
|-------|--------|
| `Vertical` (default) | Groups stacked top-to-bottom |
| `Horizontal` | Groups placed left-to-right |

**XAML — Horizontal layout:**
```xml
<syncfusion:TaskBar Name="taskBar"
                    GroupMargin="5"
                    GroupOrientation="Horizontal">
    <syncfusion:TaskBarItem Header="TaskBarItem1">
        <StackPanel Margin="10">
            <TextBlock TextWrapping="Wrap"
                       Text="This taskbar provides a Windows XP-style UI." />
        </StackPanel>
    </syncfusion:TaskBarItem>
    <syncfusion:TaskBarItem Header="TaskBarItem2">
        <StackPanel Margin="10">
            <TextBlock TextWrapping="Wrap"
                       Text="Specify and customize the group margin." />
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

**C#:**
```csharp
taskBar.GroupOrientation = Orientation.Horizontal;
```

**When to use Horizontal:** Use for wide containers where groups should appear as columns side-by-side rather than stacked vertically.

---

## GroupOrientationChanged Event

When orientation changes, you may need to adjust `GroupMargin` for proper alignment. Handle `GroupOrientationChanged`:

```csharp
private void taskBar_GroupOrientationChanged(DependencyObject d,
    DependencyPropertyChangedEventArgs e)
{
    if (taskBar.GroupOrientation == Orientation.Horizontal)
        taskBar.GroupMargin = new Thickness(5, 0, 0, 0);  // left margin for H
    else
        taskBar.GroupMargin = new Thickness(0, 0, 0, 5);  // bottom margin for V
}
```

**Why:** Horizontal layout needs left-side margin between columns; vertical layout needs bottom margin between rows. Adjusting on orientation change ensures consistent spacing regardless of which orientation is active.

---

## Group Margin

`GroupMargin` sets the outer margin for **all** `TaskBarItem` groups uniformly.

**XAML — Uniform margin:**
```xml
<syncfusion:TaskBar Name="taskBar" GroupMargin="5">
    <syncfusion:TaskBarItem Header="TaskBarItem1">
        <StackPanel Margin="10">
            <TextBlock Text="Content" />
        </StackPanel>
    </syncfusion:TaskBarItem>
    <syncfusion:TaskBarItem Header="TaskBarItem2">
        <StackPanel Margin="10">
            <TextBlock Text="More content" />
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

**XAML — Per-side margin (Left, Top, Right, Bottom):**
```xml
<syncfusion:TaskBar GroupMargin="5,10,5,10">
    ...
</syncfusion:TaskBar>
```

**C#:**
```csharp
// Uniform
taskBar.GroupMargin = new Thickness(5);

// Per-side
taskBar.GroupMargin = new Thickness(5, 10, 5, 10);
```

---

## Group Padding

`GroupPadding` is an **attached property** that sets the internal padding inside each `TaskBarItem`'s content area.

**XAML:**
```xml
<syncfusion:TaskBar Name="taskBar"
                    GroupMargin="5"
                    syncfusion:TaskBar.GroupPadding="20">
    <syncfusion:TaskBarItem Header="TaskBarItem1">
        <StackPanel Margin="10">
            <TextBlock Text="Content with padding." />
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

**C#:**
```csharp
// Uniform padding
TaskBar.SetGroupPadding(taskBar, new Thickness(20));

// Per-side padding
TaskBar.SetGroupPadding(taskBar, new Thickness(10, 5, 10, 5));
```

**Difference from GroupMargin:**
- `GroupMargin` = space *outside* each group (between groups)
- `GroupPadding` = space *inside* each group (between the group border and its content)

---

## Group Width

`GroupWidth` sets a uniform width for all `TaskBarItem` groups in the `TaskBar`.

**XAML:**
```xml
<syncfusion:TaskBar Name="taskBar"
                    GroupMargin="5"
                    GroupWidth="150">
    <syncfusion:TaskBarItem Header="TaskBarItem1">
        <StackPanel Margin="10">
            <TextBlock Text="Fixed-width group." />
        </StackPanel>
    </syncfusion:TaskBarItem>
    <syncfusion:TaskBarItem Header="TaskBarItem2">
        <StackPanel Margin="10">
            <TextBlock Text="Same width." />
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

**C#:**
```csharp
taskBar.GroupWidth = 150;
```

**Use when:** You need all groups to have a consistent width regardless of content, e.g., when using `GroupOrientation="Horizontal"` and want equal-width columns.

---

## Flow Direction

Use `FlowDirection` to switch the reading/layout direction of the TaskBar — useful for RTL language support.

| Value | Effect |
|-------|--------|
| `LeftToRight` (default) | Normal left-to-right layout |
| `RightToLeft` | Mirrored right-to-left layout for RTL locales |

**XAML:**
```xml
<syncfusion:TaskBar Name="taskBar" FlowDirection="RightToLeft">
    <syncfusion:TaskBarItem Header="TaskBarItem1">
        <StackPanel Margin="10">
            <TextBlock Text="RTL content" />
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

**C#:**
```csharp
taskBar.FlowDirection = FlowDirection.RightToLeft;
```

---

## Related References

- [Getting Started](getting-started.md) — Initial setup
- [Items and Content](items-and-content.md) — TaskBarItem content and collapse behavior
- [Appearance](appearance.md) — Visual styles

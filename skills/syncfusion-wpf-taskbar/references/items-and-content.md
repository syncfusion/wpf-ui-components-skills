# Items and Content

This reference covers TaskBarItem content patterns, custom headers with images, collapse/expand behavior, animation speed, and expander button sizing.

## Table of Contents
- [TaskBarItem Content](#taskbaritem-content)
- [Custom Header with Image](#custom-header-with-image)
- [Collapse and Expand](#collapse-and-expand)
- [Animation Speed](#animation-speed)
- [Expander Button Size](#expander-button-size)

---

## TaskBarItem Content

A `TaskBarItem` uses an `Items` collection — add any WPF `UIElement` as content. Wrap multiple elements in a layout panel:

**Single item:**
```xml
<syncfusion:TaskBarItem Header="Actions">
    <Button Content="Click Me" Margin="10" />
</syncfusion:TaskBarItem>
```

**Multiple items with StackPanel:**
```xml
<syncfusion:TaskBarItem Header="File Tasks">
    <StackPanel Margin="10" HorizontalAlignment="Center">
        <Button Content="New File" Margin="0,2" />
        <Button Content="Open File" Margin="0,2" />
        <Button Content="Save File" Margin="0,2" />
    </StackPanel>
</syncfusion:TaskBarItem>
```

**C#:**
```csharp
TaskBarItem item = new TaskBarItem();
item.Header = "File Tasks";

StackPanel panel = new StackPanel() { Margin = new Thickness(10) };
panel.Children.Add(new Button() { Content = "New File" });
panel.Children.Add(new Button() { Content = "Open File" });

item.Items.Add(panel);
taskBar.Items.Add(item);
```

**Tip:** Place a single container panel (e.g., `StackPanel`, `Grid`, `WrapPanel`) inside the `TaskBarItem` rather than multiple top-level elements, for predictable layout.

---

## Custom Header with Image

The `Header` property accepts any `object`, so you can use a template with an icon and label:

```xml
<syncfusion:TaskBarItem>
    <syncfusion:TaskBarItem.Header>
        <DockPanel Margin="0">
            <Image Height="16" Width="16" Source="Assets/folder.ico" />
            <TextBlock Foreground="White"
                       Margin="5,0,0,0"
                       Text="My Documents" />
        </DockPanel>
    </syncfusion:TaskBarItem.Header>

    <StackPanel Margin="10">
        <TextBlock Text="Documents content here..." />
    </StackPanel>
</syncfusion:TaskBarItem>
```

**Key points:**
- The `DockPanel` inside `Header` renders within the TaskBarItem's header strip
- Set `TextBlock.Foreground="White"` for visibility against the default dark header background
- Use any image format: `.ico`, `.png`, `.jpg`, or `BitmapImage` via binding

**Using a resource image:**
```xml
<syncfusion:TaskBarItem.Header>
    <DockPanel>
        <Image Height="16" Width="16">
            <Image.Source>
                <BitmapImage UriSource="/Assets/folder.png" />
            </Image.Source>
        </Image>
        <TextBlock Foreground="White" Margin="5,0,0,0" Text="Documents" />
    </DockPanel>
</syncfusion:TaskBarItem.Header>
```

---

## Collapse and Expand

Use the `IsOpened` attached property to control whether `TaskBarItem` groups are expanded or collapsed.

**Collapse the entire TaskBar on load (XAML):**
```xml
<syncfusion:TaskBar Name="taskBar"
                    GroupMargin="5"
                    syncfusion:TaskBar.IsOpened="False">
    <syncfusion:TaskBarItem Header="TaskBarItem1">
        <StackPanel Margin="10">
            <TextBlock Text="Content here." />
        </StackPanel>
    </syncfusion:TaskBarItem>
    <syncfusion:TaskBarItem Header="TaskBarItem2">
        <StackPanel Margin="10">
            <TextBlock Text="More content." />
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

**Collapse programmatically (C#):**
```csharp
// Collapse all items
TaskBar.SetIsOpened(taskBar, false);

// Expand all items
TaskBar.SetIsOpened(taskBar, true);
```

**Behavior:**
- `IsOpened="True"` (default) — all groups are expanded
- `IsOpened="False"` — all groups are collapsed to their header strip only
- The user can still click individual group headers to toggle them interactively

---

## Animation Speed

Control the duration of the expand/collapse animation using the `Speed` attached property.

**XAML:**
```xml
<syncfusion:TaskBar Name="taskBar"
                    GroupMargin="5"
                    syncfusion:TaskBar.Speed="10">
    <syncfusion:TaskBarItem Header="TaskBarItem1">
        <StackPanel Margin="10">
            <TextBlock Text="Content" />
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

**C# — Set speed:**
```csharp
TaskBar.SetSpeed(taskBar, 10);
```

**C# — Get current speed:**
```csharp
double currentSpeed = TaskBar.GetSpeed(taskBar);
```

**Guidelines:**
- Lower values = faster animation
- Higher values = slower, more deliberate animation
- Set to `0` for instant expand/collapse with no animation

---

## Expander Button Size

Change the height of the expand/collapse button in the TaskBar header strip using the `ButtonSize` attached property.

**XAML:**
```xml
<syncfusion:TaskBar Name="taskBar"
                    GroupMargin="5"
                    syncfusion:TaskBar.ButtonSize="20">
    <syncfusion:TaskBarItem Header="TaskBarItem1">
        <StackPanel Margin="10">
            <TextBlock Text="Content" />
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

**C#:**
```csharp
TaskBar.SetButtonSize(taskBar, 20);
```

**When to use:** Increase `ButtonSize` for touch-friendly UIs or when the default button is too small for the design. Decrease it for compact, dense layouts.

---

## Related References

- [Getting Started](getting-started.md) — Initial setup
- [Layout and Orientation](layout-and-orientation.md) — GroupMargin, GroupPadding, GroupWidth
- [Appearance](appearance.md) — Visual styles

# Getting Started with TaskBar

This guide covers assembly setup, adding the control, and building a basic grouped task panel.

---

## Assembly Setup

**Add assembly references:**
- `Syncfusion.Tools.WPF`
- `Syncfusion.Shared.WPF`

Install via NuGet:
```
Syncfusion.Tools.WPF
```

**C# namespace:**
```csharp
using Syncfusion.Windows.Tools.Controls;
```

**XAML namespace:**
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

---

## Adding TaskBar via XAML

Declare the control in your XAML page:

```xml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="300">
    <Grid Name="grid">
        <syncfusion:TaskBar Name="taskBar" />
    </Grid>
</Window>
```

---

## Adding TaskBar via C#

```csharp
// Create TaskBar instance
TaskBar taskBar = new TaskBar();

// Set as window content (or add to a panel)
this.Content = taskBar;
```

> **Note:** To display the TaskBar via C#, you must add it to an existing panel or set it as the window's `Content`. The control cannot display if it has no parent container.

---

## Adding a TaskBarItem

A `TaskBarItem` is one collapsible group in the TaskBar. Set its `Header` to label the group.

**XAML:**
```xml
<syncfusion:TaskBar Name="taskBar">
    <syncfusion:TaskBarItem Name="taskBarItem1" Header="TaskBarItem1" />
</syncfusion:TaskBar>
```

**C#:**
```csharp
TaskBar taskBar = new TaskBar();

TaskBarItem taskBarItem1 = new TaskBarItem();
taskBarItem1.Header = "TaskBarItem1";

taskBar.Items.Add(taskBarItem1);

this.Content = taskBar;
```

---

## Adding Content to a TaskBarItem

Place any WPF content inside a `TaskBarItem`. Use a `StackPanel` or any container panel to organize items:

**XAML:**
```xml
<syncfusion:TaskBar Name="taskBar">
    <syncfusion:TaskBarItem Name="taskBarItem1" Header="TaskBarItem1">
        <StackPanel Margin="10"
                    HorizontalAlignment="Center"
                    VerticalAlignment="Stretch">
            <TextBlock TextWrapping="Wrap">
                This TaskBar has a TaskBarItem.
            </TextBlock>
        </StackPanel>
    </syncfusion:TaskBarItem>
</syncfusion:TaskBar>
```

**C#:**
```csharp
TaskBar taskBar = new TaskBar();

TaskBarItem taskBarItem1 = new TaskBarItem();
taskBarItem1.Header = "TaskBarItem1";

TextBlock textBlock = new TextBlock();
textBlock.Text = "This TaskBar has a TaskBarItem.";

taskBarItem1.Items.Add(textBlock);
taskBar.Items.Add(taskBarItem1);

this.Content = taskBar;
```

**Content can be any UIElement** — `Button`, `Grid`, `UserControl`, `Image`, custom panels, etc. The `TaskBarItem` is a standard `ItemsControl`.

---

## Related References

- [Items and Content](items-and-content.md) — Headers with images, collapse, animation speed
- [Layout and Orientation](layout-and-orientation.md) — Orientation, margin, padding, width
- [Appearance](appearance.md) — Visual styles

# Getting Started with TimeSpanEdit

## Table of Contents
- [Installation & Adding Control](#installation--adding-control)
- [Setting Initial Values](#setting-initial-values)
- [Example with Event Handling](#example-with-event-handling)

---

## Installation & Adding Control

**Assembly:** `Syncfusion.Shared.WPF`

Install via NuGet: `Install-Package Syncfusion.Shared.WPF`

**Via XAML:**
```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <StackPanel>
        <syncfusion:TimeSpanEdit x:Name="timeSpanEdit"
                                 Value="5.12:30:45"
                                 Width="150" Height="35" />
    </StackPanel>
</Window>
```

**Via C# Code:**
```csharp
using Syncfusion.Windows.Shared;
public partial class MainWindow : Window {
    public MainWindow() {
        TimeSpanEdit editor = new TimeSpanEdit { Value = new TimeSpan(5, 12, 30, 45) };
        StackPanel container = new StackPanel();
        container.Children.Add(editor);
        this.Content = container;
    }
}
```

---

## Setting Initial Values

**XAML:** `<syncfusion:TimeSpanEdit Value="5.12:30:45" />`  (format: d.h:m:s)

**C#:**
```csharp
editor.Value = new TimeSpan(5, 12, 30, 45);      // 5d 12h 30m 45s
editor.Value = TimeSpan.FromHours(24);           // 1 day  
editor.Value = TimeSpan.Zero;                     // Clear
```

---

## Example with Event Handling

**XAML:**
```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" Title="Demo" Height="250" Width="400">
    <StackPanel VerticalAlignment="Center">
        <syncfusion:TimeSpanEdit x:Name="taskDuration" Value="0.08:30:00" Height="35" Margin="10" />
        <Button Content="Submit" Click="Submit_Click" Height="35" Margin="10" />
    </StackPanel>
</Window>
```

**Code:**
```csharp
private void Submit_Click(object sender, RoutedEventArgs e) {
    TimeSpan duration = taskDuration.Value ?? TimeSpan.Zero;
    MessageBox.Show($"Duration: {duration.Days}d {duration.Hours}h {duration.Minutes}m");
}
```



---

## Next Steps

Now that you've added TimeSpanEdit to your application:

1. **Customize the format** — Read [value-and-formats.md](value-and-formats.md) to create custom time displays
2. **Enable interactions** — Check [user-interactions.md](user-interactions.md) for keyboard, mouse, and button controls
3. **Handle events** — Use [constraints-and-events.md](constraints-and-events.md) for ValueChanged and validation
4. **Style it** — Apply colors and themes in [appearance-and-theming.md](appearance-and-theming.md)

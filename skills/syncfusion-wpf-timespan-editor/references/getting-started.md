# Getting Started with TimeSpanEdit

## Table of Contents
- [Installation & Dependencies](#installation--dependencies)
- [Adding via Designer](#adding-via-designer)
- [Adding via XAML](#adding-via-xaml)
- [Adding via C# Code](#adding-via-c-code)
- [Setting Initial Values](#setting-initial-values)

---

## Installation & Dependencies

To use the TimeSpanEdit control in your WPF application, add the Syncfusion assembly reference.

**Required Assembly:**
- `Syncfusion.Shared.WPF`

**Via NuGet Package Manager:**

```powershell
Install-Package Syncfusion.Shared.WPF
```

**Via NuGet Package Manager UI:**
1. Right-click your WPF project → **Manage NuGet Packages**
2. Search for `Syncfusion.Shared.WPF`
3. Click **Install**

**Verify Installation:**
After installing, the assembly should appear in your project's References under "Assemblies".

---

## Adding via Designer

The easiest way to add TimeSpanEdit is through the Visual Studio Designer toolbox.

**Steps:**
1. Open your WPF Designer window (double-click `MainWindow.xaml`)
2. Locate the **Syncfusion Tools** in the Toolbox panel
3. Find and drag the **TimeSpanEdit** control onto your Grid or Panel
4. The `Syncfusion.Shared.WPF` assembly will be added automatically

**Result:**
The control appears in your XAML with default properties:

```xml
<syncfusion:TimeSpanEdit x:Name="timeSpanEdit" 
                         Width="100" 
                         Height="25" />
```

---

## Adding via XAML

**Manual XAML approach for custom layouts:**

**Step 1: Add the Syncfusion namespace to your Window:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="TimeSpanEditDemo.MainWindow"
        Title="TimeSpanEdit Example"
        Height="300"
        Width="500">
```

**Step 2: Declare the TimeSpanEdit control in your layout:**

```xml
<Grid>
    <syncfusion:TimeSpanEdit x:Name="timeSpanEdit"
                             Value="5.12:30:45"
                             Width="150"
                             Height="35"
                             Margin="20" />
</Grid>
```

**Complete XAML Example:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="TimeSpanEditDemo.MainWindow"
        Title="TimeSpanEdit Sample"
        Height="300"
        Width="500">
    <Grid Background="White">
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <TextBlock Text="Enter Time Duration:" 
                       FontSize="14" 
                       Margin="0,0,0,10" />
            
            <syncfusion:TimeSpanEdit x:Name="timeSpanEdit"
                                     Value="5.12:30:45"
                                     Width="150"
                                     Height="35" />
        </StackPanel>
    </Grid>
</Window>
```

---

## Adding via C# Code

**For programmatic control creation:**

**Step 1: Add the using statement:**

```csharp
using Syncfusion.Windows.Tools.Controls;
using System.Windows;
```

**Step 2: Create and configure the TimeSpanEdit instance:**

```csharp
public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        // Create a new TimeSpanEdit instance
        TimeSpanEdit timeSpanEdit = new TimeSpanEdit();
        
        // Configure properties
        timeSpanEdit.Value = new TimeSpan(5, 12, 30, 45);
        timeSpanEdit.Width = 150;
        timeSpanEdit.Height = 35;
        timeSpanEdit.Name = "timeSpanEdit";
        
        // Set as window content or add to a container
        this.Content = timeSpanEdit;
    }
}
```

**Adding to a Container (e.g., Grid or StackPanel):**

```csharp
public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        // Create container
        StackPanel stackPanel = new StackPanel();
        
        // Create TimeSpanEdit
        TimeSpanEdit timeSpanEdit = new TimeSpanEdit();
        timeSpanEdit.Value = new TimeSpan(5, 12, 30, 45);
        timeSpanEdit.Width = 150;
        timeSpanEdit.Height = 35;
        
        // Add to container
        stackPanel.Children.Add(timeSpanEdit);
        
        // Set as window content
        this.Content = stackPanel;
    }
}
```

---

## Setting Initial Values

**Via XAML Attribute:**
```xml
<syncfusion:TimeSpanEdit Value="10.11:32:43" />
```

**Via C# Code:**
```csharp
TimeSpanEdit editor = new TimeSpanEdit();
editor.Value = new TimeSpan(10, 11, 32, 43);  // 10 days, 11 hours, 32 mins, 43 secs
```

**Interpreting TimeSpan Values:**

The `TimeSpan` constructor accepts multiple parameters:
```csharp
new TimeSpan(days, hours, minutes, seconds)
new TimeSpan(hours, minutes, seconds)
new TimeSpan(days, hours, minutes, seconds, milliseconds)
```

**Examples:**
```csharp
new TimeSpan(5, 12, 30, 45)           // 5 days, 12 hours, 30 minutes, 45 seconds
new TimeSpan(12, 30, 45)              // 12 hours, 30 minutes, 45 seconds
new TimeSpan(5, 12, 30, 45, 100)      // 5 days, 12 hours, 30 minutes, 45 seconds, 100 milliseconds
```

---

## Minimal Complete Example

**XAML (MainWindow.xaml):**
```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="TimeSpanEditDemo.MainWindow"
        Title="TimeSpanEdit Demo"
        Height="250"
        Width="400"
        WindowStartupLocation="CenterScreen">
    <Grid Background="#F5F5F5">
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center" Width="200">
            <TextBlock Text="Task Duration" FontSize="16" FontWeight="Bold" Margin="0,0,0,15" />
            
            <TextBlock Text="Days:Hours:Minutes:Seconds" FontSize="12" Margin="0,0,0,5" Foreground="#666" />
            
            <syncfusion:TimeSpanEdit x:Name="taskDuration"
                                     Value="0.08:30:00"
                                     Height="40"
                                     Margin="0,0,0,20" />
            
            <Button Content="Submit" 
                    Click="SubmitButton_Click"
                    Height="35"
                    Background="#0078D4"
                    Foreground="White" />
        </StackPanel>
    </Grid>
</Window>
```

**C# Code-Behind (MainWindow.xaml.cs):**
```csharp
using System.Windows;
using Syncfusion.Windows.Shared;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
    }
    
    private void SubmitButton_Click(object sender, RoutedEventArgs e) {
        TimeSpan duration = taskDuration.Value ?? TimeSpan.Zero;
        MessageBox.Show($"Duration: {duration.Days} days, {duration.Hours} hours, {duration.Minutes} minutes");
    }
}
```

---

## Next Steps

Now that you've added TimeSpanEdit to your application:

1. **Customize the format** — Read [value-and-formats.md](value-and-formats.md) to create custom time displays
2. **Enable interactions** — Check [user-interactions.md](user-interactions.md) for keyboard, mouse, and button controls
3. **Handle events** — Use [constraints-and-events.md](constraints-and-events.md) for ValueChanged and validation
4. **Style it** — Apply colors and themes in [appearance-and-theming.md](appearance-and-theming.md)

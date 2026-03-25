# Getting Started with WPF Badge (SfBadge)

## Table of Contents
- [Installation and Setup](#installation-and-setup)
- [Adding SfBadge via XAML](#adding-sfbadge-via-xaml)
- [Adding SfBadge via C#](#adding-sfbadge-via-c)
- [Basic Badge with Button](#basic-badge-with-button)
- [Setting Display Content](#setting-display-content)
- [Troubleshooting](#troubleshooting)

## Installation and Setup

### Required Assemblies

To use the SfBadge control in a WPF application, add the following assembly references:

- **Syncfusion.Shared.WPF**
- **Syncfusion.Tools.WPF**

### NuGet Package Installation

Install via NuGet Package Manager:

```
Install-Package Syncfusion.Shared.WPF
Install-Package Syncfusion.Tools.WPF
```

Or search for "Syncfusion WPF" in NuGet Package Manager UI and install the appropriate packages.

For more details, refer to the [NuGet installation guide](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages).

## Adding SfBadge via XAML

### Step 1: Create a New WPF Project

1. Open Visual Studio
2. Create a new WPF Application project
3. Name it appropriately (e.g., "BadgeDemo")

### Step 2: Add Assembly References

Add the required Syncfusion assemblies via NuGet or manual reference.

### Step 3: Declare Syncfusion Namespace

In your XAML window, import the Syncfusion namespace:

```xaml
<Window
    x:Class="GettingStarted.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:notification="http://schemas.syncfusion.com/wpf"
    mc:Ignorable="d">
    <Grid>
        <!-- Your content here -->
    </Grid>
</Window>
```

### Step 4: Add SfBadge Control

```xaml
<Grid>
    <notification:SfBadge 
        x:Name="badge"
        Content="5"/>
</Grid>
```

## Adding SfBadge via C#

### Step 1: Include Required Namespace

```csharp
using Syncfusion.Windows.Controls.Notification;
```

### Step 2: Create and Add SfBadge Instance

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        this.InitializeComponent();
        
        // Creating an instance of the Badge control
        SfBadge badge = new SfBadge();
        badge.Name = "badge";
        badge.Content = "5";
        
        // Add to grid
        Grid grid = (Grid)this.Content;
        grid.Children.Add(badge);
    }
}
```

## Basic Badge with Button

The most common use case is attaching a badge to a button using the `SfBadge.Badge` attached property:

### XAML Implementation

```xaml
<Button 
    Width="100" 
    Height="50" 
    Content="Inbox">
    <notification:SfBadge.Badge>
        <notification:SfBadge 
            Content="10"
            x:Name="badge"/>
    </notification:SfBadge.Badge>
</Button>
```

### C# Implementation

```csharp
// Creating Badge control
SfBadge sfBadge = new SfBadge();
sfBadge.Name = "badge";
sfBadge.Content = 10;

// Create button control as container for badge
Button button = new Button();
button.Width = 100;
button.Height = 50;
button.Content = "Inbox";

// Assigning Badge control to button
SfBadge.SetBadge(button, sfBadge);

// Add button to your layout
Grid grid = (Grid)this.Content;
grid.Children.Add(button);
```

### Result

The badge appears in the top-right corner of the button by default, displaying "10".

## Setting Display Content

### Static Content

Set the `Content` property to any value (string or number):

```xaml
<notification:SfBadge 
    Content="99+"
    x:Name="badge"/>
```

```csharp
badge.Content = "99+";
```

### Dynamic Content via Data Binding

Bind the `Content` property to a ViewModel property:

```xaml
<notification:SfBadge 
    Content="{Binding UnreadMessageCount}"
    x:Name="badge"/>
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private int _unreadMessageCount;
    
    public int UnreadMessageCount
    {
        get { return _unreadMessageCount; }
        set
        {
            if (_unreadMessageCount != value)
            {
                _unreadMessageCount = value;
                OnPropertyChanged(nameof(UnreadMessageCount));
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

### Null Content

If `Content` is null, the badge will be hidden:

```csharp
badge.Content = null; // Badge is not displayed
```

## Troubleshooting

### Issue: Badge Control Not Displaying

**Symptoms:** SfBadge appears in XAML but doesn't render in the application.

**Solutions:**
1. Verify assemblies are correctly referenced (Syncfusion.Shared.WPF, Syncfusion.Tools.WPF)
2. Confirm namespace declaration: `xmlns:notification="http://schemas.syncfusion.com/wpf"`
3. Check that the badge has a parent container (grid, stack panel, or button)
4. Ensure `Content` property is not null

### Issue: "Cannot resolve 'http://schemas.syncfusion.com/wpf'"

**Symptoms:** XAML editor shows namespace resolution error.

**Solution:** This is usually an IntelliSense issue. Build the project to verify assemblies are correctly loaded. The application will run despite the editor warning.

### Issue: Badge Not Attached to Button

**Symptoms:** Badge displays but not positioned on the button.

**Solution:** Use the `SfBadge.Badge` attached property:

```xaml
<!-- Incorrect - badge not attached -->
<Button>
    <notification:SfBadge Content="5"/>
</Button>

<!-- Correct - using attached property -->
<Button>
    <notification:SfBadge.Badge>
        <notification:SfBadge Content="5"/>
    </notification:SfBadge.Badge>
</Button>
```

### Issue: Badge Content Not Updating

**Symptoms:** Changing the `Content` property in code doesn't update the display.

**Solution:** Ensure you're modifying the badge instance directly:

```csharp
// Get reference to the badge
SfBadge badge = (SfBadge)SfBadge.GetBadge(button);
badge.Content = newValue; // Now updates correctly
```

For data binding, ensure ViewModel implements `INotifyPropertyChanged`.

### Issue: Assembly Not Found

**Symptoms:** Build error "Could not find assembly 'Syncfusion.Tools.WPF'"

**Solution:** 
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion"
3. Install appropriate WPF packages
4. Verify package version matches your Syncfusion license

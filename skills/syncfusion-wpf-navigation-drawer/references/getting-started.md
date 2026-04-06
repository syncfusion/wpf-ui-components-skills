# Getting Started with WPF Navigation Drawer

This guide covers the basic setup and implementation of the Syncfusion WPF Navigation Drawer control.

## Assembly Deployment

### Required Assemblies
- **Syncfusion.SfNavigationDrawer.WPF** - Core navigation drawer control

Refer to the [Control Dependencies](https://help.syncfusion.com/wpf/control-dependencies#sfnavigationdrawer) documentation for the complete list of assemblies.

### NuGet Package Installation

1. **Package Manager Console:**
   ```powershell
   Install-Package Syncfusion.SfNavigationDrawer.WPF
   ```

2. **NuGet Package Manager UI:**
   - Right-click project → Manage NuGet Packages
   - Search for "Syncfusion.SfNavigationDrawer.WPF"
   - Click Install

## Creating Your First Navigation Drawer

### Step 1: Create WPF Project

Create a new WPF Application project in Visual Studio.

### Step 2: Add Navigation Drawer from Toolbox

1. Drag and drop **SfNavigationDrawer** from the Toolbox to your XAML designer
2. Visual Studio automatically adds:
   - Assembly references
   - XML namespace declaration

The namespace will be added to your XAML:
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Step 3: Add Control Manually in XAML

Alternatively, add the control manually:

```xaml
<Window x:Class="NavigationDrawerWPF.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    
    <syncfusion:SfNavigationDrawer x:Name="navigationDrawer"/>
    
</Window>
```

### Step 4: Add Control in Code-Behind

Create the control programmatically in C#:

```csharp
using Syncfusion.UI.Xaml.NavigationDrawer;
using System.Windows;

namespace NavigationDrawerWPF
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create navigation drawer
            SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
            
            // Set as window content
            this.Content = navigationDrawer;
        }
    }
}
```

## Adding Content to the Control

The Navigation Drawer has two main areas:
1. **Drawer Content** - The sidebar menu
2. **Main Content** - The primary application content

### Adding Main Content View

```xaml
<syncfusion:SfNavigationDrawer>
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Width="150"
               Height="30"
               HorizontalAlignment="Center"
               VerticalAlignment="Center"
               Content="Content View" />
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();

Label label = new Label
{
    Content = "Content View",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center,
    Height = 30,
    Width = 150
};

navigationDrawer.ContentView = label;
this.Content = navigationDrawer;
```

## Adding Sidebar Menu Items

The sidebar supports built-in navigation items with icons and hierarchical structure.

### Basic Navigation Items

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer" 
                               DisplayMode="Expanded">
    <!-- Item 1: Inbox -->
    <syncfusion:NavigationItem Header="Inbox" 
                               IsExpanded="True" 
                               IsSelected="True">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429 L25.410999,23 22.598995,23 15.853724,16.561278 3.4150009,29 44.586115,29z M2.0000003,3.3371553 L2.0000003,27.587 14.406845,15.180154z M46.000002,2.6187388 L33.45355,15.038597 46.000002,27.585888z M3.4950623,2.0000003 L23.399998,21 24.589001,21 43.782564,2.0000003z M0,0 L48.000002,0 48.000002,31 0,31z" 
                  Fill="White" 
                  Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <!-- Item 2: Sent mail -->
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M42.128046,6.7269681 L18.53705,30.317964 25.278003,30.317964 48,7.5950035z M9.9429989,15.162005 L0,25.650993 0,4.2389927z M47.765003,0 L18.570995,0 10.614001,7.9569952 33.309998,30.317964 39.390003,30.317964 47.765003,21.493006z" 
                  Fill="White" 
                  Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <!-- Item 3: Drafts -->
    <syncfusion:NavigationItem Header="Drafts">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M6.9999996,48.353 L19,48.353 19,44.827999 20.686001,44.827999 20.686001,48.353 42.028,48.353 42.028,31.315001 37.811001,31.315001 37.811001,44.118999 23.813999,44.118999 23.813999,31.315001 19.59,31.315001 19.59,44.118999 6.9999996,44.118999z M11.116,27.843998 L19.59,27.843998 19.59,0 30.655998,0 30.655998,27.843998 37.918999,27.843998 24.486,41.472 24.486,41.472z M0,27.843998 L2.8129998,27.843998 2.8129998,51.981998 45.844002,51.981998 45.844002,27.843998 48.656002,27.843998 48.656002,55.610001 0,55.610001z" 
                  Fill="White" 
                  Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <!-- Main Content -->
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Width="150" 
               Height="30"
               HorizontalAlignment="Center"
               VerticalAlignment="Center"
               Content="Content View" />
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Creating Items in Code

```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
navigationDrawer.DisplayMode = DisplayMode.Expanded;

// Create Inbox item
navigationDrawer.Items.Add(new NavigationItem()
{
    Header = "Inbox",
    Icon = new Path()
    {
        Data = Geometry.Parse("M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z"),
        Fill = Brushes.White,
        Stretch = Stretch.Uniform
    },
    IsExpanded = true,
    IsSelected = true
});

// Create Sent mail item
navigationDrawer.Items.Add(new NavigationItem()
{
    Header = "Sent mail",
    Icon = new Path()
    {
        Data = Geometry.Parse("M2,21L23,12L2,3v7l15,2L2,13V21z"),
        Fill = Brushes.White,
        Stretch = Stretch.Uniform
    }
});

// Create Drafts item
navigationDrawer.Items.Add(new NavigationItem()
{
    Header = "Drafts",
    Icon = new Path()
    {
        Data = Geometry.Parse("M3,3h18v14H3V3z M5,5v10h14V5H5z"),
        Fill = Brushes.White,
        Stretch = Stretch.Uniform
    }
});

// Add content view
Label label = new Label();
label.Content = "Content View";
label.HorizontalAlignment = HorizontalAlignment.Center;
label.VerticalAlignment = VerticalAlignment.Center;
label.Height = 30;
label.Width = 150;
navigationDrawer.ContentView = label;

this.Content = navigationDrawer;
```

## Complete Example with Hierarchical Items

This example shows a full email-style navigation with nested items:

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer"
                               DisplayMode="Expanded">
    <!-- Inbox with sub-categories -->
    <syncfusion:NavigationItem Header="Inbox" 
                               IsExpanded="True" 
                               IsSelected="True">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
        
        <!-- Sub-item: Primary -->
        <syncfusion:NavigationItem Header="Primary">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z" Fill="White" Stretch="Uniform"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <!-- Sub-item: Social -->
        <syncfusion:NavigationItem Header="Social">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,2l3,7l7,1l-5,5l1,7l-6-4l-6,4l1-7l-5-5l7-1z" Fill="White" Stretch="Uniform"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <!-- Sub-item: Promotions -->
        <syncfusion:NavigationItem Header="Promotions">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M8,2 L14,14 L2,14 Z" Fill="White" Stretch="Uniform"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M2,21L23,12L2,3v7l15,2L2,13V21z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Important">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Drafts">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M3,3h18v14H3V3z M5,5v10h14V5H5z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <!-- Separator -->
    <syncfusion:NavigationItem ItemType="Separator" />
    
    <!-- Header -->
    <syncfusion:NavigationItem Header="All Labels" ItemType="Header" />
    
    <syncfusion:NavigationItem Header="Starred">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="All mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Trash">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M3,6h18v2H3V6z M6,8v12c0,1.1 0.9,2 2,2h8c1.1,0 2-0.9 2-2V8H6z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Spam">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Width="150" Height="30"
               HorizontalAlignment="Center"
               VerticalAlignment="Center"
               Content="Content View" />
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

## Next Steps

- **Display Modes:** Learn about Default, Compact, and Expanded modes → [display-modes.md](display-modes.md)
- **Data Binding:** Bind to data sources with ItemsSource → [populating-data.md](populating-data.md)
- **Custom Views:** Create custom drawer layouts → [custom-views.md](custom-views.md)
- **Events:** Handle navigation and state change events → [commands-and-events.md](commands-and-events.md)

## Sample Code

View complete sample on GitHub: [Getting Started Sample](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Getting_Started)


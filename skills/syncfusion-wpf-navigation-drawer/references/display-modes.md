# Display Modes in WPF Navigation Drawer

## Table of Contents
- [Overview](#overview)
- [Default Display Mode](#default-display-mode)
- [Compact Display Mode](#compact-display-mode)
- [Expanded Display Mode](#expanded-display-mode)
- [Auto Display Mode Change](#auto-display-mode-change)
- [Width Configuration](#width-configuration)
- [Choosing the Right Mode](#choosing-the-right-mode)

## Overview

The WPF Navigation Drawer provides three display modes to suit different navigation patterns:

| Mode | Description | Use Case |
|------|-------------|----------|
| **Default** | Collapsible drawer with custom views | Mobile-style slide-out menus |
| **Compact** | Icon-only sidebar that expands on toggle | Desktop apps with space constraints |
| **Expanded** | Full-width sidebar always visible | Desktop apps with ample space |

All modes support different item types (Tab, Button, Header, Separator) and hierarchical navigation.

## Default Display Mode

The **Default** mode creates a collapsible drawer menu that overlays the main content when opened.

### Key Features
- Drawer is hidden by default, shown on user interaction
- Can be positioned on any side (Left, Right, Top, Bottom)
- Supports custom views (DrawerHeaderView, DrawerContentView, DrawerFooterView)
- Swipe gestures for opening/closing
- Three transition animations: SlideOnTop, Push, Reveal

### Basic Implementation

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer" 
                               DisplayMode="Default"
                               DrawerWidth="300">
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid Background="White">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            
            <!-- Header with hamburger button -->
            <StackPanel Background="#1aa1d6" Orientation="Horizontal">
                <Button x:Name="hamburgerButton" 
                        Height="50" Width="50"
                        Click="HamburgerButton_Click">
                    <Image Source="hamburger_icon.png" 
                           Height="20" Width="20"/>
                </Button>
                <Label Content="Home" 
                       Height="50" 
                       VerticalContentAlignment="Center"
                       Foreground="White"/>
            </StackPanel>
            
            <!-- Main content -->
            <Label Grid.Row="1" 
                   Content="Content View"
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
    
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9">
            <Label Content="Header View" 
                   HorizontalContentAlignment="Center"
                   VerticalContentAlignment="Center"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
    
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <ListBox ItemsSource="{Binding MenuItems}">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding Name}" Foreground="Black"/>
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void HamburgerButton_Click(object sender, RoutedEventArgs e)
{
    navigationDrawer.ToggleDrawer();
}
```

### When to Use Default Mode
- Mobile-style applications in WPF
- Apps requiring custom drawer layouts beyond standard navigation items
- Scenarios where drawer should overlay content
- Touch-enabled applications with swipe gestures

## Compact Display Mode

The **Compact** mode displays a narrow sidebar showing only icons. When toggled or an item is clicked, it expands to show full content.

### Key Features
- Always visible narrow icon bar (default width: 48px)
- Expands on toggle button click to show full text
- Expansion appears as overlay above main content
- Supports hierarchical navigation with sub-items
- Sub-items shown in popup when collapsed

### Basic Implementation

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer" 
                               DisplayMode="Compact"
                               CompactModeWidth="48"
                               ExpandedModeWidth="320">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
                <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
                <Path Data="M2,21L23,12L2,3v7l15,2L2,13V21z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Drafts">
        <syncfusion:NavigationItem.Icon>
                <Path Data="M3,3h18v14H3V3z M5,5v10h14V5H5z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View" 
               HorizontalAlignment="Center"
               VerticalAlignment="Center"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
navigationDrawer.DisplayMode = DisplayMode.Compact;
navigationDrawer.CompactModeWidth = 48;
navigationDrawer.ExpandedModeWidth = 320;

navigationDrawer.Items.Add(new NavigationItem()
{
    Header = "Inbox",
    Icon = new Path()
    {
        Data = Geometry.Parse("M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z"),
        Fill = Brushes.White,
        Stretch = Stretch.Uniform
    }
});

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

Label label = new Label();
label.Content = "Content View";
label.HorizontalAlignment = HorizontalAlignment.Center;
label.VerticalAlignment = VerticalAlignment.Center;
navigationDrawer.ContentView = label;

this.Content = navigationDrawer;
```

### Compact Mode with Sub-items

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
        
        <!-- Sub-items appear in popup when compact -->
        <syncfusion:NavigationItem Header="Primary">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z" Fill="White" Stretch="Uniform"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="Social">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,2l3,7l7,1l-5,5l1,7l-6-4l-6,4l1-7l-5-5l7-1z" Fill="White" Stretch="Uniform"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:NavigationItem>
</syncfusion:SfNavigationDrawer>
```

### When to Use Compact Mode
- Desktop applications with limited horizontal space
- Apps needing quick icon-based navigation
- Scenarios where screen real estate is premium
- Applications with many navigation items that need compact representation

## Expanded Display Mode

The **Expanded** mode displays a full-width sidebar that is always visible, pushing the main content to the side.

### Key Features
- Always visible full sidebar (default width: 320px)
- Main content appears alongside the drawer
- Toggle button collapses to compact mode (icon-only)
- Best for desktop applications with ample space
- Hierarchical items expanded inline

### Basic Implementation

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer" 
                               DisplayMode="Expanded"
                               ExpandedModeWidth="320">
    <syncfusion:NavigationItem Header="Inbox" IsExpanded="True" IsSelected="True">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
        
        <!-- Sub-items shown inline when expanded -->
        <syncfusion:NavigationItem Header="Primary">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z" Fill="White" Stretch="Uniform"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="Social">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,2l3,7l7,1l-5,5l1,7l-6-4l-6,4l1-7l-5-5l7-1z" Fill="White" Stretch="Uniform"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="Promotions">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z" Fill="White" Stretch="Uniform"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M2,21L23,12L2,3v7l15,2L2,13V21z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Drafts">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M3,3h18v14H3V3z M5,5v10h14V5H5z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View" 
               HorizontalAlignment="Center"
               VerticalAlignment="Center"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
navigationDrawer.DisplayMode = DisplayMode.Expanded;
navigationDrawer.ExpandedModeWidth = 320;

// Create parent item with sub-items
NavigationItemsCollection navigationSubItems = new NavigationItemsCollection();
navigationSubItems.Add(new NavigationItem()
{
    Header = "Primary",
    Icon = new Path()
    {
        Data = Geometry.Parse("M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z"),
        Fill = Brushes.White,
        Stretch = Stretch.Uniform
    }
});

navigationSubItems.Add(new NavigationItem()
{
    Header = "Social",
    Icon = new Path()
    {
        Data = Geometry.Parse("M12,2l3,7l7,1l-5,5l1,7l-6-4l-6,4l1-7l-5-5l7-1z"),
        Fill = Brushes.White,
        Stretch = Stretch.Uniform
    }
});

navigationDrawer.Items.Add(new NavigationItem()
{
    Header = "Inbox",
    Icon = new Path()
    {
        Data = Geometry.Parse("M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z"),
        Fill = Brushes.White,
        Stretch = Stretch.Uniform
    },
    Items = navigationSubItems,
    IsExpanded = true,
    IsSelected = true
});

Label label = new Label();
label.Content = "Content View";
label.HorizontalAlignment = HorizontalAlignment.Center;
label.VerticalAlignment = VerticalAlignment.Center;
navigationDrawer.ContentView = label;

this.Content = navigationDrawer;
```

### When to Use Expanded Mode
- Desktop applications with wide screens
- Apps where navigation is primary (e.g., email clients, file managers)
- Scenarios requiring persistent navigation visibility
- Applications with complex hierarchical navigation

## Auto Display Mode Change

The Navigation Drawer can automatically switch between Compact and Expanded modes based on available window width.

### Configuration

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer"
                               DisplayMode="Expanded"
                               AutoChangeDisplayMode="True"
                               ExpandedModeThresholdWidth="700">
    <!-- Navigation items -->
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
navigationDrawer.DisplayMode = DisplayMode.Expanded;
navigationDrawer.AutoChangeDisplayMode = true;
navigationDrawer.ExpandedModeThresholdWidth = 700;
```

### How It Works
- **Window Width > ExpandedModeThresholdWidth:** Drawer shows in Expanded mode (full width)
- **Window Width ≤ ExpandedModeThresholdWidth:** Drawer switches to Compact mode (icon-only)

### Example: Responsive Navigation

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded"
                               AutoChangeDisplayMode="True"
                               ExpandedModeThresholdWidth="600"
                               CompactModeWidth="48"
                               ExpandedModeWidth="320">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M2,21L23,12L2,3v7l15,2L2,13V21z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Drafts">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M3,3h18v14H3V3z M5,5v10h14V5H5z" Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View" 
               HorizontalAlignment="Center"
               VerticalAlignment="Center"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### When to Use Auto Mode
- Responsive desktop applications
- Apps that need to adapt to different screen sizes
- Multi-monitor setups with varying window sizes
- Applications supporting both maximized and windowed states

## Width Configuration

### CompactModeWidth

Sets the width of the drawer in Compact mode (icon-only view).

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               CompactModeWidth="60">
    <!-- Items -->
</syncfusion:SfNavigationDrawer>
```

**Default:** 48 pixels

### ExpandedModeWidth

Sets the width of the drawer when fully expanded.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded"
                               ExpandedModeWidth="350">
    <!-- Items -->
</syncfusion:SfNavigationDrawer>
```

**Default:** 320 pixels

### ExpandedModeThresholdWidth

Sets the window width threshold for auto mode switching.

```xaml
<syncfusion:SfNavigationDrawer AutoChangeDisplayMode="True"
                               ExpandedModeThresholdWidth="800">
    <!-- Items -->
</syncfusion:SfNavigationDrawer>
```

**Default:** 1007 pixels

## Choosing the Right Mode

| Scenario | Recommended Mode | Reason |
|----------|------------------|--------|
| Mobile-style app | Default | Custom views, overlay behavior |
| Space-constrained desktop app | Compact | Icon-only saves space |
| Wide-screen desktop app | Expanded | Always-visible navigation |
| Responsive desktop app | Expanded + AutoChange | Adapts to window size |
| Touch-enabled app | Default | Swipe gestures support |
| Complex hierarchical navigation | Expanded | Inline sub-item display |
| Quick access navigation | Compact | Fast icon-based access |
| Custom drawer layouts | Default | Full control over drawer content |

## Sample Code

View complete samples on GitHub:
- [Display Mode Sample](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Display_Mode)
- [Custom View Sample](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Custom_View)


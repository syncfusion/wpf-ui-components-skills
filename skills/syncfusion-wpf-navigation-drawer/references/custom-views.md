# Custom Views and Customization in WPF Navigation Drawer

## Table of Contents
- [Overview](#overview)
- [Custom Views in Default Mode](#custom-views-in-default-mode)
- [Drawer Sizing](#drawer-sizing)
- [Drawer Positioning](#drawer-positioning)
- [Swipe Gestures](#swipe-gestures)
- [Transition Animations](#transition-animations)
- [Drawer Background](#drawer-background)
- [Programmatic Control](#programmatic-control)

## Overview

The WPF Navigation Drawer provides extensive customization options:

- **Custom Views:** DrawerHeaderView, DrawerContentView, DrawerFooterView (Default mode)
- **Sizing:** DrawerWidth, DrawerHeight
- **Positioning:** Left, Right, Top, Bottom
- **Gestures:** Swipe to open/close with sensitivity control
- **Animations:** SlideOnTop, Push, Reveal with duration control
- **State Management:** IsOpen property, ToggleDrawer() method

## Custom Views in Default Mode

Default mode allows complete customization of the drawer using three view properties.

### Three View Sections

1. **DrawerHeaderView** - Top section (custom header content)
2. **DrawerContentView** - Body section (main menu/navigation)
3. **DrawerFooterView** - Bottom section (footer actions/info)

### Complete Example

```xaml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:SfNavigationDrawer x:Name="navigationDrawer" 
                               DisplayMode="Default"
                               DrawerWidth="300">
    <!-- Main Content with Hamburger Button -->
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid Background="White">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            
            <!-- Header with hamburger button -->
            <StackPanel Background="#1aa1d6" Orientation="Horizontal">
                <Button x:Name="hamburgerButton" 
                        BorderBrush="Transparent"
                        Height="50" 
                        Width="50"
                        Background="#1aa1d6" 
                        Foreground="White"
                        Click="HamburgerButton_Click">
                    <Image Source="hamburger_icon.png" 
                           Height="20" 
                           Width="20"/>
                </Button>
                <Label x:Name="headerLabel" 
                       Height="50"
                       Content="Home"
                       Foreground="White"
                       Background="#1aa1d6"
                       HorizontalContentAlignment="Center"
                       VerticalContentAlignment="Center"/>
            </StackPanel>
            
            <!-- Main content area -->
            <Label Grid.Row="1" 
                   Content="Content View"
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"
                   Foreground="Black"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
    
    <!-- Custom Header View -->
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9" Height="80">
            <StackPanel Orientation="Horizontal" 
                        VerticalAlignment="Center"
                        Margin="15,0">
                <Ellipse Width="50" 
                         Height="50" 
                         Fill="White">
                    <Ellipse.OpacityMask>
                        <ImageBrush ImageSource="user_avatar.png"/>
                    </Ellipse.OpacityMask>
                </Ellipse>
                <StackPanel Margin="15,0,0,0" 
                            VerticalAlignment="Center">
                    <TextBlock Text="John Doe" 
                               FontWeight="Bold" 
                               Foreground="White"
                               FontSize="16"/>
                    <TextBlock Text="john.doe@email.com" 
                               Foreground="White"
                               FontSize="12"/>
                </StackPanel>
            </StackPanel>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
    
    <!-- Custom Content View (Menu) -->
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <Grid Background="White">
            <ListBox x:Name="list" 
                     ItemsSource="{Binding MenuItems}"
                     BorderThickness="0"
                     Background="White">
                <ListBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal" 
                                    Margin="10">
                            <Image Source="{Binding Icon}" 
                                   Width="24" 
                                   Height="24" 
                                   Margin="0,0,15,0"/>
                            <TextBlock Text="{Binding Name}" 
                                       VerticalAlignment="Center"
                                       Foreground="Black"
                                       FontSize="14"/>
                        </StackPanel>
                    </DataTemplate>
                </ListBox.ItemTemplate>
            </ListBox>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <!-- Custom Footer View -->
    <syncfusion:SfNavigationDrawer.DrawerFooterView>
        <Grid Background="#31ade9" Height="60">
            <Button Content="Sign Out" 
                    Background="Transparent"
                    BorderBrush="White"
                    Foreground="White"
                    Margin="15"
                    Height="35"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerFooterView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void HamburgerButton_Click(object sender, RoutedEventArgs e)
{
    navigationDrawer.ToggleDrawer();
}

public class ViewModel
{
    public ObservableCollection<MenuItem> MenuItems { get; set; }
    
    public ViewModel()
    {
        MenuItems = new ObservableCollection<MenuItem>();
        MenuItems.Add(new MenuItem { Name = "Home", Icon = "home_icon.png" });
        MenuItems.Add(new MenuItem { Name = "Profile", Icon = "profile_icon.png" });
        MenuItems.Add(new MenuItem { Name = "Inbox", Icon = "inbox_icon.png" });
        MenuItems.Add(new MenuItem { Name = "Settings", Icon = "settings_icon.png" });
    }
}

public class MenuItem
{
    public string Name { get; set; }
    public string Icon { get; set; }
}
```

## Drawer Sizing

### DrawerWidth

Controls the width of the drawer when positioned Left or Right (Default mode only).

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               DrawerWidth="350"
                               Position="Left">
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9">
            <Label Content="Wide Drawer Header"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
navigationDrawer.DisplayMode = DisplayMode.Default;
navigationDrawer.DrawerWidth = 350;
navigationDrawer.Position = Position.Left;
```

**Default:** 320 pixels  
**Applies to:** Default mode with Position = Left or Right

### DrawerHeight

Controls the height of the drawer when positioned Top or Bottom (Default mode only).

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               DrawerHeight="250"
                               Position="Top">
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9">
            <TextBlock Text="Top Drawer Content"
                       TextWrapping="Wrap"
                       Padding="15"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
navigationDrawer.DisplayMode = DisplayMode.Default;
navigationDrawer.DrawerHeight = 250;
navigationDrawer.Position = Position.Top;
```

**Default:** 320 pixels  
**Applies to:** Default mode with Position = Top or Bottom

## Drawer Positioning

The drawer can be positioned on any side of the window (Default mode only).

### Position Options

- **Left** (default) - Drawer slides from left
- **Right** - Drawer slides from right
- **Top** - Drawer slides from top
- **Bottom** - Drawer slides from bottom

### Left Position

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               Position="Left"
                               DrawerWidth="300">
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <ListBox ItemsSource="{Binding MenuItems}"/>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid>
            <Button Content="☰ Menu" 
                    HorizontalAlignment="Left"
                    Click="HamburgerButton_Click"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Right Position

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               Position="Right"
                               DrawerWidth="300">
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <ListBox ItemsSource="{Binding MenuItems}"/>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid>
            <Button Content="☰ Menu" 
                    HorizontalAlignment="Right"
                    Click="HamburgerButton_Click"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Top Position

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               Position="Top"
                               DrawerHeight="200">
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <Grid Background="#31ade9">
            <TextBlock Text="Notification Panel" 
                       Foreground="White"
                       Padding="20"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid>
            <Button Content="⬇ Notifications" 
                    VerticalAlignment="Top"
                    Click="HamburgerButton_Click"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Bottom Position

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               Position="Bottom"
                               DrawerHeight="150">
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <Grid Background="#31ade9">
            <TextBlock Text="Quick Actions" 
                       Foreground="White"
                       Padding="20"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid>
            <Button Content="⬆ Actions" 
                    VerticalAlignment="Bottom"
                    Click="HamburgerButton_Click"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

## Swipe Gestures

Enable touch-friendly swipe gestures to open and close the drawer (Default mode only).

### Enabling Swipe Gestures

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               EnableSwipeGesture="True"
                               TouchThreshold="50">
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9">
            <Label Content="Swipe to close"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
navigationDrawer.DisplayMode = DisplayMode.Default;
navigationDrawer.EnableSwipeGesture = true;
navigationDrawer.TouchThreshold = 50;
```

### TouchThreshold

The `TouchThreshold` property defines the swipe-sensitive region from the screen edge (in pixels).

```xaml
<syncfusion:SfNavigationDrawer EnableSwipeGesture="True"
                               TouchThreshold="70">
    <!-- Content -->
</syncfusion:SfNavigationDrawer>
```

**Default:** 40 pixels  
**Use case:** Increase for easier swipe access on smaller screens  
**Range:** Typically 30-100 pixels

## Transition Animations

Choose from three transition effects for drawer open/close animations (Default mode only).

### SlideOnTop (Default)

Drawer slides over the main content.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               Transition="SlideOnTop"
                               AnimationDuration="0:0:0.3">
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <ListBox ItemsSource="{Binding MenuItems}"/>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content stays in place"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Behavior:** Drawer overlays content  
**Best for:** Mobile-style menus, modal navigation

### Push

Drawer and main content move together.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               Transition="Push"
                               AnimationDuration="0:0:0.3">
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <ListBox ItemsSource="{Binding MenuItems}"/>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content moves with drawer"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Behavior:** Content shifts to make room for drawer  
**Best for:** Apps where context should remain visible

### Reveal

Main content slides away to reveal stationary drawer beneath.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default"
                               Transition="Reveal"
                               AnimationDuration="0:0:0.3">
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <ListBox ItemsSource="{Binding MenuItems}"/>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content slides to reveal drawer"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Behavior:** Drawer appears fixed as content moves  
**Best for:** Creating depth effect, modern UI

### AnimationDuration

Control the speed of the open/close animation.

```xaml
<syncfusion:SfNavigationDrawer AnimationDuration="0:0:0.5">
    <!-- Content -->
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
navigationDrawer.AnimationDuration = new TimeSpan(0, 0, 0, 0, 500); // 500ms
```

**Default:** 300 milliseconds  
**Format:** Hours:Minutes:Seconds:Milliseconds (e.g., "0:0:0.25" = 250ms)  
**Recommended range:** 200-500ms

## Drawer Background

Customize the drawer's background color using the `DrawerBackground` property.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded"
                               DrawerBackground="#2c3e50">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
navigationDrawer.DrawerBackground = new SolidColorBrush(
    (Color)ColorConverter.ConvertFromString("#2c3e50")
);
```

**Gradient Background:**
```xaml
<syncfusion:SfNavigationDrawer>
    <syncfusion:SfNavigationDrawer.DrawerBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="#1e3c72" Offset="0"/>
            <GradientStop Color="#2a5298" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:SfNavigationDrawer.DrawerBackground>
</syncfusion:SfNavigationDrawer>
```

## Programmatic Control

Control the drawer state programmatically using properties and methods.

### IsOpen Property

Get or set whether the drawer is open.

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer" 
                               IsOpen="False">
    <!-- Content -->
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
// Open the drawer
navigationDrawer.IsOpen = true;

// Close the drawer
navigationDrawer.IsOpen = false;

// Check drawer state
if (navigationDrawer.IsOpen)
{
    // Drawer is open
}
```

### ToggleDrawer() Method

Toggle between open and closed states.

```csharp
private void MenuButton_Click(object sender, RoutedEventArgs e)
{
    // Toggle drawer state
    navigationDrawer.ToggleDrawer();
}
```

### Example: Open on Startup

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer"
                               DisplayMode="Expanded"
                               IsOpen="True">
    <!-- Content -->
</syncfusion:SfNavigationDrawer>
```

### Example: Close After Item Selection

```csharp
private void NavigationDrawer_ItemClicked(object sender, NavigationItemClickedEventArgs e)
{
    // Close drawer after item click (Default mode)
    if (navigationDrawer.DisplayMode == DisplayMode.Default)
    {
        navigationDrawer.IsOpen = false;
    }
}
```

### Example: Programmatic Navigation

```csharp
private void NavigateToInbox()
{
    // Open drawer
    navigationDrawer.IsOpen = true;
    
    // Wait for animation, then select item
    Task.Delay(300).ContinueWith(_ =>
    {
        Dispatcher.Invoke(() =>
        {
            var inboxItem = navigationDrawer.Items[0];
            navigationDrawer.SelectedItem = inboxItem;
        });
    });
}
```

## Complete Custom View Example

```xaml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:SfNavigationDrawer x:Name="navigationDrawer"
                               DisplayMode="Default"
                               DrawerWidth="300"
                               Position="Left"
                               Transition="SlideOnTop"
                               EnableSwipeGesture="True"
                               TouchThreshold="50"
                               AnimationDuration="0:0:0.3"
                               DrawerBackground="#2c3e50">
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid Background="White">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            
            <StackPanel Background="#3498db" 
                        Orientation="Horizontal"
                        Height="60">
                <Button Content="☰" 
                        Width="60" 
                        Height="60"
                        FontSize="24"
                        Background="Transparent"
                        BorderThickness="0"
                        Foreground="White"
                        Click="HamburgerButton_Click"/>
                <TextBlock Text="My Application" 
                           VerticalAlignment="Center"
                           Foreground="White"
                           FontSize="18"
                           FontWeight="Bold"/>
            </StackPanel>
            
            <Label Grid.Row="1"
                   Content="Main Content Area"
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"
                   FontSize="20"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
    
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#34495e">
            <StackPanel Margin="20" VerticalAlignment="Center">
                <Ellipse Width="60" Height="60" Fill="White"/>
                <TextBlock Text="John Doe" 
                           Foreground="White"
                           FontSize="18"
                           FontWeight="Bold"
                           Margin="0,10,0,0"/>
                <TextBlock Text="john@example.com" 
                           Foreground="#ecf0f1"
                           FontSize="12"/>
            </StackPanel>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
    
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <ListBox ItemsSource="{Binding MenuItems}"
                 Background="Transparent"
                 BorderThickness="0"
                 Foreground="White">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal" 
                                Margin="20,10">
                        <TextBlock Text="{Binding Icon}" 
                                   FontSize="20" 
                                   Width="30"/>
                        <TextBlock Text="{Binding Name}" 
                                   FontSize="16"
                                   VerticalAlignment="Center"/>
                    </StackPanel>
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <syncfusion:SfNavigationDrawer.DrawerFooterView>
        <StackPanel Background="#34495e" Padding="20">
            <Button Content="Settings" 
                    Background="#3498db"
                    BorderThickness="0"
                    Foreground="White"
                    Height="40"
                    Margin="0,0,0,10"/>
            <Button Content="Sign Out" 
                    Background="#e74c3c"
                    BorderThickness="0"
                    Foreground="White"
                    Height="40"/>
        </StackPanel>
    </syncfusion:SfNavigationDrawer.DrawerFooterView>
</syncfusion:SfNavigationDrawer>
```

## Sample Code

View complete sample on GitHub: [Custom View Sample](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Custom_View)


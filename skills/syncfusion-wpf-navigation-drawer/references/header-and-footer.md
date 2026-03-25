# Header and Footer Customization

Complete guide for customizing the header and footer sections of the WPF Navigation Drawer.

## Overview

The Navigation Drawer provides extensive customization for header and footer:
- **Toggle Button:** Customize icon and content templates
- **Header Section:** Adjust height, customize appearance
- **Footer Items:** Add footer navigation items or custom footer view
- **Visibility Control:** Show/hide toggle button

## Customizing the Toggle Button

### ToggleButtonIconTemplate

Customize the toggle button icon (Compact/Expanded modes).

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <syncfusion:SfNavigationDrawer.ToggleButtonIconTemplate>
        <DataTemplate>
            <Image Width="25" Height="25"
                   Source="custom_menu_icon.png"/>
        </DataTemplate>
    </syncfusion:SfNavigationDrawer.ToggleButtonIconTemplate>
    
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### ToggleButtonContentTemplate

Add custom content next to the toggle button icon.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <syncfusion:SfNavigationDrawer.ToggleButtonIconTemplate>
        <DataTemplate>
            <Image Width="25" Height="25" Source="People.png"/>
        </DataTemplate>
    </syncfusion:SfNavigationDrawer.ToggleButtonIconTemplate>
    
    <syncfusion:SfNavigationDrawer.ToggleButtonContentTemplate>
        <DataTemplate>
            <StackPanel Orientation="Vertical" 
                        Margin="10,0,0,0">
                <Label Content="Johnson Martin" 
                       FontWeight="Bold"
                       Padding="0"/>
                <Label Content="Project Manager" 
                       FontSize="10"
                       Foreground="Gray"
                       Padding="0"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfNavigationDrawer.ToggleButtonContentTemplate>
    
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### IsToggleButtonVisible

Show or hide the toggle button.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded"
                               IsToggleButtonVisible="False">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
navigationDrawer.IsToggleButtonVisible = false;
```

**Use case:** When you have a custom hamburger button in the main content

## Footer Items

Add navigation items to the footer section (Compact/Expanded modes).

### Basic Footer Items

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <!-- Main navigation items -->
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M42.128046,6.7269681..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <!-- Footer items -->
    <syncfusion:SfNavigationDrawer.FooterItems>
        <syncfusion:NavigationItem Header="Settings" 
                                   SelectionBackground="Transparent">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M13.999999,3.9500002..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="LogOut" 
                                   ItemType="Button"
                                   SelectionBackground="Transparent">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M13.999999,3.9500002..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:SfNavigationDrawer.FooterItems>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
navigationDrawer.FooterItems.Add(new NavigationItem()
{
    Header = "Settings",
    SelectionBackground = Brushes.Transparent,
    Icon = new Path()
    {
        Data = Geometry.Parse("M13.999999,3.9500002..."),
        Fill = Brushes.White,
        Stretch = Stretch.Uniform
    }
});

navigationDrawer.FooterItems.Add(new NavigationItem()
{
    Header = "LogOut",
    ItemType = ItemType.Button,
    SelectionBackground = Brushes.Transparent,
    Icon = new Path()
    {
        Data = Geometry.Parse("M13.999999,3.9500002..."),
        Fill = Brushes.White,
        Stretch = Stretch.Uniform
    }
});
```

### Footer with Command Binding

```xaml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <syncfusion:NavigationItem Header="Home">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.FooterItems>
        <syncfusion:NavigationItem Header="Sign Out"
                                   ItemType="Button"
                                   Command="{Binding SignOutCommand}">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M13.999999,3.9500002..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:SfNavigationDrawer.FooterItems>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

## Header Height

Adjust the height of the header section containing the toggle button.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded"
                               HeaderHeight="80">
    <syncfusion:SfNavigationDrawer.ToggleButtonContentTemplate>
        <DataTemplate>
            <StackPanel>
                <Label Content="Company Name" 
                       FontSize="16"
                       FontWeight="Bold"/>
                <Label Content="Navigation Menu" 
                       FontSize="10"
                       Foreground="Gray"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfNavigationDrawer.ToggleButtonContentTemplate>
    
    <syncfusion:NavigationItem Header="Dashboard">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
navigationDrawer.HeaderHeight = 80;
```

**Default:** 48 pixels  
**Use case:** Larger headers with user profile, company logo, or multi-line content

## Footer Height

Adjust the height of the footer section.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded"
                               FooterHeight="70">
    <syncfusion:NavigationItem Header="Home">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.FooterItems>
        <syncfusion:NavigationItem Header="Settings">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M13.999999,3.9500002..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:SfNavigationDrawer.FooterItems>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
navigationDrawer.FooterHeight = 70;
```

**Default:** Auto (based on items)  
**Use case:** Consistent spacing, multiple footer items, custom footer content

## Custom Header and Footer Views (Default Mode)

In Default mode, use `DrawerHeaderView` and `DrawerFooterView` for complete customization.

### Complete Example

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default" DrawerWidth="300">
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid Background="White">
            <Button Content="☰ Menu" 
                    HorizontalAlignment="Left"
                    VerticalAlignment="Top"
                    Click="HamburgerButton_Click"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
    
    <!-- Custom Header -->
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9" Height="120">
            <StackPanel VerticalAlignment="Center" Margin="20">
                <Ellipse Width="60" Height="60" Fill="White">
                    <Ellipse.OpacityMask>
                        <ImageBrush ImageSource="user_avatar.png"/>
                    </Ellipse.OpacityMask>
                </Ellipse>
                <TextBlock Text="John Doe" 
                           Foreground="White"
                           FontWeight="Bold"
                           FontSize="18"
                           Margin="0,10,0,0"/>
                <TextBlock Text="john.doe@company.com" 
                           Foreground="White"
                           FontSize="12"/>
            </StackPanel>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
    
    <!-- Menu Content -->
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <ListBox ItemsSource="{Binding MenuItems}">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding Name}" 
                               Foreground="Black"
                               Padding="20,10"/>
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <!-- Custom Footer -->
    <syncfusion:SfNavigationDrawer.DrawerFooterView>
        <Grid Background="#31ade9" Height="80">
            <StackPanel VerticalAlignment="Center" Margin="20">
                <Button Content="Settings" 
                        Background="Transparent"
                        BorderBrush="White"
                        Foreground="White"
                        Margin="0,0,0,10"/>
                <Button Content="Sign Out" 
                        Background="#e74c3c"
                        BorderThickness="0"
                        Foreground="White"/>
            </StackPanel>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerFooterView>
</syncfusion:SfNavigationDrawer>
```

## Common Patterns

### Pattern 1: User Profile Header

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded" HeaderHeight="100">
    <syncfusion:SfNavigationDrawer.ToggleButtonIconTemplate>
        <DataTemplate>
            <Ellipse Width="40" Height="40" Fill="LightBlue">
                <Ellipse.OpacityMask>
                    <ImageBrush ImageSource="avatar.png"/>
                </Ellipse.OpacityMask>
            </Ellipse>
        </DataTemplate>
    </syncfusion:SfNavigationDrawer.ToggleButtonIconTemplate>
    
    <syncfusion:SfNavigationDrawer.ToggleButtonContentTemplate>
        <DataTemplate>
            <StackPanel Margin="10,0,0,0">
                <TextBlock Text="Jane Smith" FontWeight="Bold"/>
                <TextBlock Text="Administrator" 
                           FontSize="10" 
                           Foreground="Gray"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfNavigationDrawer.ToggleButtonContentTemplate>
    
    <syncfusion:NavigationItem Header="Dashboard">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Pattern 2: Footer with Multiple Actions

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <syncfusion:NavigationItem Header="Home">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.FooterItems>
        <syncfusion:NavigationItem ItemType="Separator"/>
        
        <syncfusion:NavigationItem Header="Help" ItemType="Button">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M13.999999,3.9500002..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="Settings">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M13.999999,3.9500002..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="About" ItemType="Button">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M13.999999,3.9500002..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:SfNavigationDrawer.FooterItems>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Pattern 3: Compact Header with Logo

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact">
    <syncfusion:SfNavigationDrawer.ToggleButtonIconTemplate>
        <DataTemplate>
            <Image Source="company_logo.png" 
                   Width="30" 
                   Height="30"/>
        </DataTemplate>
    </syncfusion:SfNavigationDrawer.ToggleButtonIconTemplate>
    
    <syncfusion:NavigationItem Header="Products">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

## Sample Code

View complete sample on GitHub: [Header and Footer Sample](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Header_and_Footer)

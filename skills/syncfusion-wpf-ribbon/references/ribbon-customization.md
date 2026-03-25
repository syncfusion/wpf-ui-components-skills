# Customization & Styling

## Table of Contents
- [StatusBar Configuration](#statusbar-configuration)
- [Styling Basics](#styling-basics)
- [Theme Selection](#theme-selection)
- [Color Customization](#color-customization)
- [Size Groups & Layout](#size-groups--layout)
- [Responsive Behavior](#responsive-behavior)
- [Custom Templates](#custom-templates)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## StatusBar Configuration

### Basic StatusBar

Add a status bar below the ribbon:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
        <RowDefinition Height="30" />
    </Grid.RowDefinitions>
    
    <!-- Ribbon -->
    <syncfusion:Ribbon Grid.Row="0">
        <!-- Ribbon content -->
    </syncfusion:Ribbon>
    
    <!-- Main content -->
    <TextBox Grid.Row="1" Margin="10" />
    
    <!-- StatusBar at bottom -->
    <StatusBar Grid.Row="2" Background="#F0F0F0">
        <TextBlock Text="Ready" Margin="5" />
    </StatusBar>
</Grid>
```

### StatusBar with Multiple Sections

```xml
<StatusBar DockPanel.Dock="Bottom" Height="30">
    <!-- Left section: Status message -->
    <StatusBarItem>
        <TextBlock Text="Ready" Margin="5" />
    </StatusBarItem>
    
    <!-- Spacer -->
    <Separator />
    
    <!-- Middle section: Progress -->
    <StatusBarItem>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="Progress:" Margin="5" />
            <ProgressBar Width="100" Height="15" Value="75" />
        </StackPanel>
    </StatusBarItem>
    
    <!-- Spacer -->
    <Separator />
    
    <!-- Right section: Zoom/Info -->
    <StatusBarItem HorizontalAlignment="Right">
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="Zoom: 100%" Margin="5" />
            <Slider Width="100" />
        </StackPanel>
    </StatusBarItem>
</StatusBar>
```

### StatusBar with Binding

```xml
<StatusBar Height="30">
    <TextBlock Text="{Binding StatusMessage}" Margin="5" />
</StatusBar>
```

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    private string _statusMessage = "Ready";
    public string StatusMessage
    {
        get => _statusMessage;
        set
        {
            if (_statusMessage != value)
            {
                _statusMessage = value;
                OnPropertyChanged(nameof(StatusMessage));
            }
        }
    }

    public void SaveDocument()
    {
        StatusMessage = "Saving...";
        // Save logic
        StatusMessage = "Document saved";
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, 
            new PropertyChangedEventArgs(propertyName));
    }
}
```

## Styling Basics

### Inline Styling

Apply styles directly to ribbon elements:

```xml
<syncfusion:RibbonButton Label="Important"
                         Foreground="Red"
                         FontWeight="Bold"
                         Background="#FFF0F0" />
```

### Resource-Based Styling

Define reusable styles in resources:

```xml
<Window.Resources>
    <!-- Button Style -->
    <Style x:Key="WarningButtonStyle" TargetType="syncfusion:RibbonButton">
        <Setter Property="Foreground" Value="Red" />
        <Setter Property="FontWeight" Value="Bold" />
        <Setter Property="Background" Value="#FFF0F0" />
    </Style>
    
    <!-- Group Style -->
    <Style x:Key="PrimaryGroupStyle" TargetType="syncfusion:RibbonBar">
        <Setter Property="BorderBrush" Value="Blue" />
        <Setter Property="BorderThickness" Value="1" />
    </Style>
</Window.Resources>

<!-- Apply styles -->
<syncfusion:RibbonButton Label="Delete" Style="{StaticResource WarningButtonStyle}" />
<syncfusion:RibbonBar Header="Primary" Style="{StaticResource PrimaryGroupStyle}" />
```

### Dynamic Styling with Triggers

Apply conditional styles:

```xml
<Window.Resources>
    <Style x:Key="DisabledButtonStyle" TargetType="syncfusion:RibbonButton">
        <Setter Property="Foreground" Value="Gray" />
        <Setter Property="Opacity" Value="0.5" />
        
        <Style.Triggers>
            <Trigger Property="IsEnabled" Value="True">
                <Setter Property="Foreground" Value="Black" />
                <Setter Property="Opacity" Value="1" />
            </Trigger>
        </Style.Triggers>
    </Style>
</Window.Resources>

<syncfusion:RibbonButton Label="Save" 
                         Style="{StaticResource DisabledButtonStyle}"
                         IsEnabled="{Binding IsDirty}" />
```

## Theme Selection

### Built-in Themes

Syncfusion ribbon supports multiple themes. Set at application or window level:

```csharp
// App.xaml.cs
public partial class App : Application
{
    public App()
    {
        // Apply Office2019 theme
        SkinStorage.ApplySkinsToMergedDictionaries = true;
        SkinStorage.SetAppliedThemeName("Office2019Blue");
    }
}
```

Available themes:
- Office2007Blue / Office2007Black / Office2007Silver
- Office2010Blue / Office2010Black
- Office2013White / Office2013LightGray / Office2013DarkGray
- Office2016Colorful / Office2016White
- Office2019Blue / Office2019Black / Office2019DarkGray
- HighContrast1 / HighContrast2

### Theme Switching at Runtime

```csharp
public class ThemeManager
{
    public static void ApplyTheme(string themeName)
    {
        SkinStorage.ApplySkinsToMergedDictionaries = true;
        SkinStorage.SetAppliedThemeName(themeName);
    }
}

// Usage
ThemeManager.ApplyTheme("Office2019DarkGray");
```

### Theme Selection UI

Allow users to choose theme:

```xml
<syncfusion:RibbonBar Header="Themes">
    <syncfusion:MenuButton Label="Theme">
        <syncfusion:RibbonMenuItem Header="Office 2019 Blue" 
                                   Click="OnTheme_Office2019Blue" />
        <syncfusion:RibbonMenuItem Header="Office 2019 Dark Gray" 
                                   Click="OnTheme_Office2019DarkGray" />
        <syncfusion:RibbonMenuItem Header="Office 2019 Black" 
                                   Click="OnTheme_Office2019Black" />
    </syncfusion:MenuButton>
</syncfusion:RibbonBar>
```

```csharp
private void OnTheme_Office2019Blue(object sender, EventArgs e)
{
    ThemeManager.ApplyTheme("Office2019Blue");
}

private void OnTheme_Office2019DarkGray(object sender, EventArgs e)
{
    ThemeManager.ApplyTheme("Office2019DarkGray");
}

private void OnTheme_Office2019Black(object sender, EventArgs e)
{
    ThemeManager.ApplyTheme("Office2019Black");
}
```

## Color Customization

### Custom Color Scheme

Create custom colors while keeping theme structure:

```csharp
public static void ApplyCustomColors()
{
    // Access theme resources
    var dictionary = new ResourceDictionary();
    
    // Define custom brushes
    var primaryBrush = new SolidColorBrush(Color.FromRgb(0, 102, 204));
    var accentBrush = new SolidColorBrush(Color.FromRgb(255, 153, 0));
    
    // Apply to resources
    Application.Current.Resources["RibbonPrimaryBrush"] = primaryBrush;
    Application.Current.Resources["RibbonAccentBrush"] = accentBrush;
}
```

### Individual Element Coloring

```xml
<syncfusion:RibbonBar Header="Tools" Background="#E8F4F8">
    <syncfusion:RibbonButton Label="Tool 1" 
                             Foreground="#006699"
                             Background="#E8F4F8" />
    <syncfusion:RibbonButton Label="Tool 2" 
                             Foreground="#006699"
                             Background="#E8F4F8" />
</syncfusion:RibbonBar>
```

## Size Groups & Layout

### Control Button Sizes

Manage button appearance across window sizes:

```xml
<syncfusion:RibbonBar Header="File Operations">
    <!-- Large buttons (always visible) -->
    <syncfusion:RibbonButton Label="New" SizeForm="Large" />
    
    <!-- Normal buttons (visible when space allows) -->
    <syncfusion:RibbonButton Label="Open" SizeForm="Normal" />
    
    <!-- Small buttons (visible only at wider widths) -->
    <syncfusion:RibbonButton Label="Close" SizeForm="Small" />
</syncfusion:RibbonBar>
```

### Size Forms Behavior

- **Large**: Icon + Label, prominent display
- **Normal**: Medium icon with label
- **Small**: Compact text-only display

## Responsive Behavior

### Auto-Collapsing Ribbon

Ribbon automatically reduces at smaller window sizes:

```xml
<syncfusion:RibbonWindow Height="600" Width="1000" MinWidth="600">
    <!-- At 1000px: All buttons visible
         At 800px: Some buttons reduced to normal/small
         At 600px: Most buttons small or hidden -->
</syncfusion:RibbonWindow>
```

### Minimize Ribbon

Allow users to collapse the ribbon:

```csharp
// Programmatically minimize
ribbon.IsMinimized = true;

// User can double-click tab or use UI toggle
```

### Ribbon Height Adjustment

```xml
<syncfusion:Ribbon Height="Auto">
    <!-- Auto height based on content -->
</syncfusion:Ribbon>

<!-- Or fixed height -->
<syncfusion:Ribbon Height="100">
    <!-- Fixed height -->
</syncfusion:Ribbon>
```

## Custom Templates

### Group Template

Customize how groups appear:

```xml
<Window.Resources>
    <DataTemplate x:Key="CustomGroupTemplate" DataType="syncfusion:RibbonBar">
        <Border BorderBrush="Blue" BorderThickness="1" CornerRadius="5" Padding="5">
            <StackPanel>
                <TextBlock Text="{Binding Header}" 
                           FontSize="12"
                           FontWeight="Bold"
                           Foreground="Blue" />
                <ItemsPresenter />
            </StackPanel>
        </Border>
    </DataTemplate>
</Window.Resources>
```

### Button Content Template

Customize button appearance:

```xml
<Window.Resources>
    <DataTemplate x:Key="IconLabelTemplate">
        <StackPanel Orientation="Vertical" 
                    HorizontalAlignment="Center">
            <Image Source="{Binding Icon}" 
                   Width="32" Height="32" />
            <TextBlock Text="{Binding Label}" 
                       FontSize="10"
                       TextAlignment="Center"
                       Margin="0,5,0,0" />
        </StackPanel>
    </DataTemplate>
</Window.Resources>
```

## Common Patterns

### Pattern 1: Dark Mode Support
```csharp
public class AppTheme
{
    public static void SetDarkMode(bool enabled)
    {
        if (enabled)
        {
            ThemeManager.ApplyTheme("Office2019Black");
            Application.Current.Resources["WindowBackground"] = new SolidColorBrush(Colors.DarkGray);
        }
        else
        {
            ThemeManager.ApplyTheme("Office2019Blue");
            Application.Current.Resources["WindowBackground"] = new SolidColorBrush(Colors.White);
        }
    }
}
```

### Pattern 2: Conditional Styling Based on State
```xml
<syncfusion:RibbonButton Label="Status"
                         Foreground="{Binding StatusColor}">
    <syncfusion:RibbonButton.Style>
        <Style TargetType="syncfusion:RibbonButton">
            <Style.Triggers>
                <DataTrigger Binding="{Binding IsError}" Value="True">
                    <Setter Property="Background" Value="#FFE6E6" />
                </DataTrigger>
            </Style.Triggers>
        </Style>
    </syncfusion:RibbonButton.Style>
</syncfusion:RibbonButton>
```

### Pattern 3: Consistent Color Theme
```xml
<Window.Resources>
    <Color x:Key="PrimaryColor">#006699</Color>
    <Color x:Key="AccentColor">#FF9900</Color>
    <Color x:Key="TextColor">#333333</Color>
    
    <SolidColorBrush x:Key="PrimaryBrush" Color="{StaticResource PrimaryColor}" />
    <SolidColorBrush x:Key="AccentBrush" Color="{StaticResource AccentColor}" />
    <SolidColorBrush x:Key="TextBrush" Color="{StaticResource TextColor}" />
</Window.Resources>

<!-- Apply theme colors -->
<syncfusion:RibbonButton Label="Primary" Foreground="{StaticResource PrimaryBrush}" />
<syncfusion:RibbonButton Label="Accent" Foreground="{StaticResource AccentBrush}" />
```

## Troubleshooting

### Issue: Theme not applying
**Solution:** Ensure `SkinStorage.ApplySkinsToMergedDictionaries = true` is set before applying theme.

### Issue: Custom colors not showing
**Solution:** Check that color resources are defined before use. Use `StaticResource` or `DynamicResource` binding.

### Issue: StatusBar disappearing
**Solution:** Ensure `DockPanel.Dock="Bottom"` is set. Check Z-index and visibility properties.

### Issue: Ribbon not responding to resizing
**Solution:** Verify that `SizeForm` properties are set on buttons. Check window `MinWidth` constraints.

### Issue: Template changes not visible
**Solution:** Rebuild application. Clear temporary files if needed. Restart application.

### Issue: Responsive layout not working
**Solution:** Ensure buttons have proper `SizeForm` values (Large, Normal, Small). Check `MaxColumnCount` settings.

# Customization and Styling

Learn how to customize the appearance and behavior of your TabControl with various styling options, orientations, and themes.

## Tab Orientation and Placement

### Basic Placement Options

Control where tab headers appear using the `TabStripPlacement` property:

**Top Placement (Default):**
```xml
<syncfusion:TabControlExt TabStripPlacement="Top" Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1" />
    <syncfusion:TabItemExt Header="Tab 2" />
</syncfusion:TabControlExt>
```

**Bottom Placement:**
```xml
<syncfusion:TabControlExt TabStripPlacement="Bottom" Name="tabControl" />
```

**Left Placement (Vertical):**
```xml
<syncfusion:TabControlExt TabStripPlacement="Left" Name="tabControl" />
```

**Right Placement (Vertical):**
```xml
<syncfusion:TabControlExt TabStripPlacement="Right" Name="tabControl" />
```

**C# Equivalent:**
```csharp
// Using Dock enum
tabControl.TabStripPlacement = Dock.Bottom;  // Top, Bottom, Left, or Right
```

### Responsive Orientation

Change orientation based on available space:

```csharp
public void AdjustTabOrientation(double width)
{
    if (width < 600)
    {
        tabControl.TabStripPlacement = Dock.Left;  // Vertical for narrow windows
    }
    else
    {
        tabControl.TabStripPlacement = Dock.Top;   // Horizontal for wide windows
    }
}
```

## Layout Modes

Configure how tabs are arranged when there are many items using the `TabControlSettings.LayoutMode` property.

### SingleLine Mode
All tabs display on one line with scrolling buttons if needed:

```xml
<syncfusion:TabControlExt Name="tabControl">
    <syncfusion:TabControlExt.TabControlSettings>
        <syncfusion:TabControlSettings TabMode="SingleLine" />
    </syncfusion:TabControlExt.TabControlSettings>
</syncfusion:TabControlExt>
```

**Use case:** Clean interface when space is limited.

### MultiLine Mode
Tabs wrap to multiple rows/columns when space is limited:

```xml
<syncfusion:TabControlExt Name="tabControl">
    <syncfusion:TabControlExt.TabControlSettings>
        <syncfusion:TabControlSettings TabMode="MultiLine" />
    </syncfusion:TabControlExt.TabControlSettings>
</syncfusion:TabControlExt>
```

**Use case:** Display many tabs without horizontal scrolling.

### MultiLineWithFillWidth Mode
Similar to MultiLine but distributes tabs evenly to fill available width:

```xml
<syncfusion:TabControlExt Name="tabControl">
    <syncfusion:TabControlExt.TabControlSettings>
        <syncfusion:TabControlSettings TabMode="MultiLineWithFillWidth" />
    </syncfusion:TabControlExt.TabControlSettings>
</syncfusion:TabControlExt>
```

**Use case:** Professional appearance with balanced tab distribution.

## Editable Tab Headers

Allow users to edit tab headers by double-clicking or pressing F2:

### Enabling Header Editing

```xml
<syncfusion:TabControlExt Name="tabControl" IsHeaderEditingEnabled="True">
    <syncfusion:TabItemExt Header="Tab 1" />
    <syncfusion:TabItemExt Header="Tab 2" />
</syncfusion:TabControlExt>
```

```csharp
tabControl.IsHeaderEditingEnabled = true;
```

### User Interaction
- Double-click a tab header to edit
- Press F2 to edit the selected tab
- Press Enter to confirm
- Press Escape to cancel

### Programmatic Header Editing

```csharp
public void StartEditingHeader(TabItemExt tabItem)
{
    // Note: Direct programmatic editing requires custom implementation
    // Framework provides UI-driven editing through double-click/F2
}
```

## Pin and Unpin Functionality

Enable users to pin frequently used tabs for quick access:

### Configuring Pin/Unpin

```xml
<syncfusion:TabControlExt IsPinningEnabled="True" Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1" />
    <syncfusion:TabItemExt Header="Tab 2" />
</syncfusion:TabControlExt>
```

```csharp
tabControl.IsPinningEnabled = true;
```

### Pin States

- **Unpinned**: Tab appears in normal position, can be closed
- **Pinned**: Tab appears at the start, cannot be closed unless unpinned

### User Interaction
- Right-click tab header to access pin/unpin option
- Click pin icon (if present) to toggle pin state

### Checking Pin State

```csharp
// Get list of pinned tabs
List<TabItemExt> pinnedTabs = new List<TabItemExt>();
foreach (TabItemExt item in tabControl.Items)
{
    if (item.IsPinned)
    {
        pinnedTabs.Add(item);
    }
}
```

```csharp
// Set pin state programmatically
tabItem.IsPinned = true;
```

## Tab Scrolling Configuration

Control how tabs scroll when there are many items and they don't fit on screen:

### Showing Navigation Buttons

```xml
<syncfusion:TabControlExt TabScrollButtonVisibility="Visible" Name="tabControl" />
```

```csharp
tabControl.TabScrollButtonVisibility = Visibility.Visible;
```

### Scroll Button Styles

Configure the appearance and behavior of scroll buttons:

**Standard Style:**
```xml
<syncfusion:TabControlExt TabScrollStyle="Standard" 
                          TabScrollButtonVisibility="Visible" />
```

**Extended Style (with first/last buttons):**
```xml
<syncfusion:TabControlExt TabScrollStyle="Extended" 
                          TabScrollButtonVisibility="Visible" />
```

```csharp
tabControl.TabScrollStyle = TabScrollStyle.Extended;
```

### Navigation Button Types

| Style | Buttons |
|-------|---------|
| Standard | Previous, Next |
| Extended | First, Previous, Next, Last |

## Styling and Themes

### Built-in Themes

Apply Syncfusion themes using SfSkinManager:

```xml
<Window x:Class="TabControlApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        syncfusion:SfSkinManager.VisualStyle="Office2019Colorful">

    <Grid>
        <syncfusion:TabControlExt Name="tabControl" />
    </Grid>
</Window>
```

### Available Themes
- Default
- Office2019Colorful
- Office2019Black
- Office2019HighContrast
- VisualStudio2013
- VisualStudio2015
- Lime
- Saffron
- And many more...

### Applying Theme in C#

```csharp
using Syncfusion.SfSkinManager;

public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        SfSkinManager.SetVisualStyle(this, VisualStyles.Office2019Colorful);
    }
}
```

## Custom Styling

### Style Tab Control

```xml
<Style TargetType="syncfusion:TabControlExt">
    <Setter Property="Background" Value="#F5F5F5" />
    <Setter Property="BorderBrush" Value="#CCCCCC" />
    <Setter Property="BorderThickness" Value="1" />
</Style>
```

### Style Tab Items

```xml
<Style TargetType="syncfusion:TabItemExt">
    <Setter Property="Foreground" Value="#333333" />
    <Setter Property="FontSize" Value="14" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="Padding" Value="10,5,10,5" />
</Style>
```

### Custom Tab Header Content

Create rich tab headers with custom layouts:

```xml
<syncfusion:TabItemExt>
    <syncfusion:TabItemExt.Header>
        <Grid Background="#E8E8E8" Margin="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="20" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="20" />
            </Grid.ColumnDefinitions>
            
            <!-- Icon -->
            <Image Source="icon.png" Grid.Column="0" Width="16" Height="16" />
            
            <!-- Title -->
            <TextBlock Text="Tab Title" Grid.Column="1" Margin="5,0,0,0" VerticalAlignment="Center" />
            
            <!-- Badge -->
            <TextBlock Text="5" Grid.Column="2" Foreground="Red" VerticalAlignment="Center" />
        </Grid>
    </syncfusion:TabItemExt.Header>
    
    <TextBlock Text="Tab content here" Margin="10" />
</syncfusion:TabItemExt>
```

## Visual Customization Examples

### Example 1: Compact Tab Bar
```xml
<syncfusion:TabControlExt CloseButtonType="Individual"
                          TabScrollButtonVisibility="Visible"
                          TabScrollStyle="Extended">
    <syncfusion:TabControlExt.TabControlSettings>
        <syncfusion:TabControlSettings TabMode="SingleLine" />
    </syncfusion:TabControlExt.TabControlSettings>
    
    <Style TargetType="syncfusion:TabItemExt">
        <Setter Property="Padding" Value="8,2,8,2" />
        <Setter Property="FontSize" Value="11" />
    </Style>
</syncfusion:TabControlExt>
```

### Example 2: Wide Tabs with Icons
```xml
<syncfusion:TabControlExt TabScrollButtonVisibility="Hidden">
    <syncfusion:TabControlExt.TabControlSettings>
        <syncfusion:TabControlSettings TabMode="MultiLineWithFillWidth" />
    </syncfusion:TabControlExt.TabControlSettings>
    
    <syncfusion:TabItemExt Padding="15,10,15,10">
        <syncfusion:TabItemExt.Header>
            <StackPanel Orientation="Horizontal">
                <Image Source="icon1.png" Width="24" Height="24" Margin="0,0,8,0" />
                <TextBlock Text="Documents" VerticalAlignment="Center" />
            </StackPanel>
        </syncfusion:TabItemExt.Header>
    </syncfusion:TabItemExt>
</syncfusion:TabControlExt>
```

### Example 3: Vertical Tabs with Pin Support
```xml
<syncfusion:TabControlExt TabStripPlacement="Left"
                          IsPinningEnabled="True"
                          CloseButtonType="Individual"
                          Width="250">
    <syncfusion:TabControlExt.TabControlSettings>
        <syncfusion:TabControlSettings TabMode="MultiLine" />
    </syncfusion:TabControlExt.TabControlSettings>
</syncfusion:TabControlExt>
```

## Responsive Customization

Adjust styling based on container size:

```csharp
public void AdjustStyleForScreenSize(TabControlExt tabControl)
{
    double width = ActualWidth;
    
    if (width < 500)
    {
        // Compact mode
        tabControl.TabScrollButtonVisibility = Visibility.Visible;
        tabControl.TabScrollStyle = TabScrollStyle.Extended;
    }
    else if (width < 1000)
    {
        // Medium mode
        tabControl.TabScrollButtonVisibility = Visibility.Visible;
        tabControl.TabScrollStyle = TabScrollStyle.Standard;
    }
    else
    {
        // Full mode
        tabControl.TabScrollButtonVisibility = Visibility.Hidden;
    }
}
```

## Best Practices

1. **Choose Appropriate Placement**: Use top/bottom for horizontal layouts, left/right for vertical
2. **Consistent Styling**: Apply consistent styles across all tabs
3. **Readable Headers**: Keep tab headers concise but descriptive
4. **Theme Selection**: Choose themes that match your application branding
5. **Accessibility**: Ensure sufficient color contrast in custom styles
6. **Icon Usage**: Use clear, recognizable icons in tab headers when appropriate
7. **Performance**: Avoid excessive custom drawing that could impact performance

## Common Customization Patterns

| Goal | Approach |
|------|----------|
| Clean minimal look | Hide close buttons, use SingleLine mode |
| Document browser | Show close buttons, enable pin/unpin |
| Settings panel | Vertical placement, disable closing |
| Code editor | Dark theme, editable headers |
| Dashboard | MultiLine mode, equal-width tabs |

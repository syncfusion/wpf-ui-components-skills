# Appearance and Visual Customization in WPF TreeView (SfTreeView)

## Table of Contents
- [Item Template](#item-template)
- [Item Template Context](#item-template-context)
- [Item Template Selector](#item-template-selector)
- [Item Height Customization](#item-height-customization)
- [Full Row Select](#full-row-select)
- [Indentation](#indentation)
- [Theming](#theming)
- [Visual States](#visual-states)

## Item Template

Customize the appearance of each tree item using `ItemTemplate`:

### Basic Item Template

```xml
<syncfusion:SfTreeView ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="20"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageIcon}"
                       Height="16" Width="16"
                       VerticalAlignment="Center"/>
                <TextBlock Grid.Column="1"
                           Text="{Binding FolderName}"
                           Margin="5,0,0,0"
                           VerticalAlignment="Center"
                           FontSize="13"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

### Advanced Item Template with Icons and Badges

```xml
<syncfusion:SfTreeView.ItemTemplate>
    <DataTemplate>
        <Grid Height="32">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="24"/>
                <ColumnDefinition Width="*"/>
                <ColumnDefinition Width="Auto"/>
            </Grid.ColumnDefinitions>
            
            <!-- Icon -->
            <Path Data="{Binding IconPath}"
                  Fill="{Binding IconColor}"
                  Stretch="Uniform"
                  Width="16" Height="16"
                  VerticalAlignment="Center"/>
            
            <!-- Name -->
            <TextBlock Grid.Column="1"
                       Text="{Binding Name}"
                       FontWeight="{Binding FontWeight}"
                       Foreground="{Binding Foreground}"
                       Margin="8,0,0,0"
                       VerticalAlignment="Center"/>
            
            <!-- Badge/Count -->
            <Border Grid.Column="2"
                    Background="DodgerBlue"
                    CornerRadius="10"
                    Padding="6,2"
                    Margin="5,0,0,0"
                    Visibility="{Binding Count, Converter={StaticResource CountToVisibilityConverter}}">
                <TextBlock Text="{Binding Count}"
                           Foreground="White"
                           FontSize="10"/>
            </Border>
        </Grid>
    </DataTemplate>
</syncfusion:SfTreeView.ItemTemplate>
```

### Item Template with Multi-Line Content

```xml
<syncfusion:SfTreeView.ItemTemplate>
    <DataTemplate>
        <StackPanel Margin="5">
            <TextBlock Text="{Binding Title}"
                       FontWeight="Bold"
                       FontSize="14"/>
            <TextBlock Text="{Binding Description}"
                       FontSize="11"
                       Foreground="Gray"
                       TextWrapping="Wrap"
                       Margin="0,2,0,0"/>
            <TextBlock Text="{Binding Date, StringFormat='Last modified: {0:d}'}"
                       FontSize="10"
                       Foreground="DarkGray"
                       Margin="0,2,0,0"/>
        </StackPanel>
    </DataTemplate>
</syncfusion:SfTreeView.ItemTemplate>
```

## Item Template Context

Control the data context of item templates using `ItemTemplateDataContextType`:

### Default (Data Model)

By default, `DataContext` is the data model object:

```xml
<syncfusion:SfTreeView ItemTemplateDataContextType="Default">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding FolderName}"/>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

### Node Context

Use `TreeViewNode` as `DataContext` to access node properties:

```xml
<syncfusion:SfTreeView ItemTemplateDataContextType="Node">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                
                <TextBlock Text="{Binding Level}"
                           Foreground="Gray"
                           Margin="0,0,5,0"/>
                <TextBlock Grid.Column="1"
                           Text="{Binding Content.FolderName}"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

**Access Node Properties:**
- `Content` - Data model object
- `Level` - Node depth (0-based)
- `IsExpanded` - Expansion state
- `HasChildNodes` - Has children
- `ParentNode` - Parent node reference

## Item Template Selector

Apply different templates based on node properties:

```csharp
public class FileItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        var node = item as TreeViewNode;
        if (node != null)
        {
            var fileItem = node.Content as FileItem;
            if (fileItem != null)
            {
                return fileItem.IsFolder ? FolderTemplate : FileTemplate;
            }
        }
        return base.SelectTemplate(item, container);
    }
}
```

**XAML Configuration:**

```xml
<Window.Resources>
    <local:FileItemTemplateSelector x:Key="ItemTemplateSelector">
        <local:FileItemTemplateSelector.FolderTemplate>
            <DataTemplate>
                <StackPanel Orientation="Horizontal">
                    <Path Data="M10 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V8c0-1.1-.9-2-2-2h-8l-2-2z"
                          Fill="Orange" Width="16" Height="16"/>
                    <TextBlock Text="{Binding Content.Name}" Margin="5,0,0,0"/>
                </StackPanel>
            </DataTemplate>
        </local:FileItemTemplateSelector.FolderTemplate>
        
        <local:FileItemTemplateSelector.FileTemplate>
            <DataTemplate>
                <StackPanel Orientation="Horizontal">
                    <Path Data="M14 2H6c-1.1 0-2 .9-2 2v16c0 1.1.9 2 2 2h12c1.1 0 2-.9 2-2V8l-6-6zm4 18H6V4h7v5h5v11z"
                          Fill="Gray" Width="16" Height="16"/>
                    <TextBlock Text="{Binding Content.Name}" Margin="5,0,0,0"/>
                </StackPanel>
            </DataTemplate>
        </local:FileItemTemplateSelector.FileTemplate>
    </local:FileItemTemplateSelector>
</Window.Resources>

<syncfusion:SfTreeView ItemTemplateDataContextType="Node"
                       ItemTemplateSelector="{StaticResource ItemTemplateSelector}"/>
```

## Item Height Customization

### Fixed Height

Set uniform height for all items:

```xml
<syncfusion:SfTreeView ItemHeight="40"/>
```

```csharp
treeView.ItemHeight = 40; // Default: 24
```

### Custom Height per Item

Use `QueryNodeSize` event for dynamic heights:

```csharp
treeView.QueryNodeSize += (s, e) =>
{
    // Root nodes: 50px
    if (e.Node.Level == 0)
    {
        e.Height = 50;
        e.Handled = true;
    }
    // Child nodes: 30px
    else
    {
        e.Height = 30;
        e.Handled = true;
    }
};
```

### Auto-Fit Height to Content

Measure content and fit automatically:

```xml
<syncfusion:SfTreeView QueryNodeSize="TreeView_QueryNodeSize">
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <StackPanel Margin="5">
                <TextBlock Text="{Binding Title}" FontWeight="Bold" TextWrapping="Wrap"/>
                <TextBlock Text="{Binding Description}" TextWrapping="Wrap" Foreground="Gray"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
</syncfusion:SfTreeView>
```

```csharp
private void TreeView_QueryNodeSize(object sender, QueryNodeSizeEventArgs e)
{
    if (e.Node.Level == 0)
    {
        e.Height = 60; // Fixed for headers
        e.Handled = true;
    }
    else
    {
        // Auto-fit based on content
        e.Height = e.GetAutoFitNodeHeight();
        e.Handled = true;
    }
}
```

**⚠️ Image Size:** Always specify image dimensions for accurate auto-fit measurement.

## Full Row Select

Extend selection highlight across full width:

```xml
<syncfusion:SfTreeView FullRowSelect="True"/>
```

```csharp
treeView.FullRowSelect = true; // Default: false
```

**Visual Difference:**
- `False`: Only item content highlighted
- `True`: Entire row background highlighted

**Best for:** File managers, navigation trees

## Indentation

Control horizontal spacing for hierarchy levels:

```xml
<syncfusion:SfTreeView Indentation="30"/>
```

```csharp
treeView.Indentation = 30; // Default: 20
```

**Calculation:** Each level adds `Indentation` pixels to left margin.

```
Level 0: 0px
Level 1: 30px
Level 2: 60px
Level 3: 90px
```

**Use Cases:**
- Small indentation (10-15): Compact view, mobile
- Default (20): Balanced visibility
- Large indentation (30-40): Clear hierarchy, presentations

## Theming

Apply built-in themes using `SfSkinManager`:

### Apply Theme

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    <Grid syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
        <syncfusion:SfTreeView ItemsSource="{Binding Folders}"/>
    </Grid>
</Window>
```

```csharp
SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

### Available Themes

- **MaterialLight / MaterialDark**
- **MaterialLightBlue / MaterialDarkBlue**
- **FluentLight / FluentDark**
- **Office2019Colorful / Office2019Black / Office2019White**
- **Office2016Colorful / Office2016White / Office2016DarkGray**
- **VisualStudio2013 / VisualStudio2015**
- **Metro**

### Custom Theme Colors

Override theme resources:

```xml
<syncfusion:SfTreeView>
    <syncfusion:SfTreeView.Resources>
        <SolidColorBrush x:Key="SyncfusionTreeViewItemBackground" Color="White"/>
        <SolidColorBrush x:Key="SyncfusionTreeViewItemHoverBackground" Color="#F0F0F0"/>
        <SolidColorBrush x:Key="SyncfusionTreeViewItemSelectedBackground" Color="DodgerBlue"/>
        <SolidColorBrush x:Key="SyncfusionTreeViewItemSelectedForeground" Color="White"/>
        <SolidColorBrush x:Key="SyncfusionTreeViewItemForeground" Color="Black"/>
    </syncfusion:SfTreeView.Resources>
</syncfusion:SfTreeView>
```

## Visual States

Customize appearance for different interaction states:

### Selection Style

```xml
<syncfusion:SfTreeView>
    <syncfusion:SfTreeView.ItemContainerStyle>
        <Style TargetType="syncfusion:TreeViewItem">
            <Setter Property="Background" Value="Transparent"/>
            <Style.Triggers>
                <Trigger Property="IsSelected" Value="True">
                    <Setter Property="Background" Value="LightBlue"/>
                    <Setter Property="Foreground" Value="DarkBlue"/>
                </Trigger>
            </Style.Triggers>
        </Style>
    </syncfusion:SfTreeView.ItemContainerStyle>
</syncfusion:SfTreeView>
```

### Hover Effect

```xml
<Style TargetType="syncfusion:TreeViewItem">
    <Style.Triggers>
        <Trigger Property="IsMouseOver" Value="True">
            <Setter Property="Background" Value="#E0E0E0"/>
        </Trigger>
    </Style.Triggers>
</Style>
```

### Focus State

```xml
<Style TargetType="syncfusion:TreeViewItem">
    <Style.Triggers>
        <Trigger Property="IsFocused" Value="True">
            <Setter Property="BorderBrush" Value="Blue"/>
            <Setter Property="BorderThickness" Value="2"/>
        </Trigger>
    </Style.Triggers>
</Style>
```

### Disabled State

```xml
<Style TargetType="syncfusion:TreeViewItem">
    <Style.Triggers>
        <Trigger Property="IsEnabled" Value="False">
            <Setter Property="Opacity" Value="0.5"/>
            <Setter Property="Foreground" Value="Gray"/>
        </Trigger>
    </Style.Triggers>
</Style>
```

## Common Patterns

### Alternating Row Colors

```csharp
treeView.QueryNodeSize += (s, e) =>
{
    e.Height = 30;
    e.Handled = true;
};

treeView.ItemPrepared += (s, e) =>
{
    var index = treeView.GetNodeIndex(e.Node);
    e.Item.Background = (index % 2 == 0) 
        ? new SolidColorBrush(Colors.White) 
        : new SolidColorBrush(Color.FromRgb(245, 245, 245));
};
```

### Custom Expander Icon

See tree-structure.md > Expander Template section for full examples.

### Conditional Formatting

```xml
<syncfusion:SfTreeView.ItemTemplate>
    <DataTemplate>
        <TextBlock Text="{Binding Name}">
            <TextBlock.Style>
                <Style TargetType="TextBlock">
                    <Style.Triggers>
                        <DataTrigger Binding="{Binding IsImportant}" Value="True">
                            <Setter Property="FontWeight" Value="Bold"/>
                            <Setter Property="Foreground" Value="Red"/>
                        </DataTrigger>
                    </Style.Triggers>
                </Style>
            </TextBlock.Style>
        </TextBlock>
    </DataTemplate>
</syncfusion:SfTreeView.ItemTemplate>
```

## Best Practices

1. **Always specify image dimensions** when using images in `ItemTemplate`
2. **Use `ItemTemplateDataContextType="Node"`** when accessing node properties (Level, IsExpanded)
3. **Keep ItemHeight ≥24** for touch-friendly targets
4. **Use FullRowSelect** for better click target area
5. **Apply themes consistently** across application
6. **Test auto-fit heights** with long/multi-line content
7. **Cache template selectors** as static resources

## Troubleshooting

**Auto-fit not working:**
- Ensure images have explicit Width/Height
- Check `e.Handled = true` in QueryNodeSize
- Verify ItemTemplate uses proper sizing

**Template not updating:**
- Check DataContext type (Model vs Node)
- Verify binding paths match data structure
- Refresh with `RefreshView()`

**Theme not applying:**
- Add SfSkinManager NuGet package
- Set theme on parent container (Grid/Window)
- Check theme name spelling

**Selection style not visible:**
- Verify FullRowSelect setting
- Check custom ItemContainerStyle triggers
- Ensure background not overridden in ItemTemplate

# Populating Data in WPF Navigation Drawer

## Table of Contents
- [Overview](#overview)
- [Using Built-in NavigationItem](#using-built-in-navigationitem)
- [NavigationItem Properties](#navigationitem-properties)
- [Different Item Types](#different-item-types)
- [Data Binding with ItemsSource](#data-binding-with-itemssource)
- [Hierarchical Data Binding](#hierarchical-data-binding)
- [IconTemplate Customization](#icontemplate-customization)
- [ItemTemplateSelector](#itemtemplateselector)
- [IndentationWidth](#indentationwidth)
- [Popup Support for Sub-items](#popup-support-for-sub-items)

## Overview

The WPF Navigation Drawer provides multiple ways to populate navigation menu items:

1. **Built-in NavigationItem** - Use NavigationItem objects directly (Compact/Expanded modes)
2. **Data Binding** - Bind to custom data sources with ItemsSource
3. **Custom Views** - Use DrawerContentView for fully custom layouts (Default mode)

## Using Built-in NavigationItem

The `NavigationItem` class provides a rich set of properties for creating navigation menus.

### Basic Example

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <syncfusion:NavigationItem Header="Inbox" IsSelected="True">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M42.128046,6.7269681..." Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Hierarchical Navigation

```xaml
<syncfusion:NavigationItem Header="Inbox" IsExpanded="True" IsSelected="True">
    <syncfusion:NavigationItem.Icon>
        <Path Data="M32.032381,16.445429..." Fill="White" Stretch="Uniform"/>
    </syncfusion:NavigationItem.Icon>
    
    <!-- Sub-items -->
    <syncfusion:NavigationItem Header="Primary">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M9.5189972,7.3780194..." Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Social">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M22.133972,14.194015..." Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Promotions">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M9.4614787,7.2521966..." Fill="White" Stretch="Uniform"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
</syncfusion:NavigationItem>
```

## NavigationItem Properties

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| `Header` | string | The text displayed in the navigation item |
| `Icon` | object | The icon displayed (typically a Path or Image) |
| `Items` | NavigationItemsCollection | Collection of sub-items for hierarchical navigation |
| `IsExpanded` | bool | Gets/sets whether the item's sub-items are visible |
| `IsSelected` | bool | Gets/sets whether the item is currently selected |
| `ItemType` | ItemType | Type of item: Tab, Button, Header, or Separator |

### Template Properties

| Property | Type | Description |
|----------|------|-------------|
| `IconTemplate` | DataTemplate | Template for custom icon rendering |
| `ExpanderTemplate` | DataTemplate | Template for the expand/collapse icon |
| `DisplayMemberPath` | string | Path to property for displaying sub-item content |
| `IconMemberPath` | string | Path to property for displaying sub-item icons |

### Command Properties

| Property | Type | Description |
|----------|------|-------------|
| `Command` | ICommand | Command executed when item is clicked |
| `CommandParameter` | object | Parameter passed to the Command |

### Styling Properties

| Property | Type | Description |
|----------|------|-------------|
| `SelectionBackground` | Brush | Background color when item is selected |
| `IsChildSelected` | bool | Gets whether any sub-item is selected |

## Different Item Types

The Navigation Drawer supports four item types:

### 1. Tab (Default)
- Supports interaction and selection
- Can have sub-items with expand/collapse
- Typical navigation menu item

```xaml
<syncfusion:NavigationItem Header="Inbox" ItemType="Tab">
    <syncfusion:NavigationItem.Icon>
        <Path Data="M32.032381,16.445429..." Fill="White"/>
    </syncfusion:NavigationItem.Icon>
</syncfusion:NavigationItem>
```

### 2. Button
- Similar to Tab but without persistent selection
- Acts like a button - triggers action without staying selected
- Can have sub-items

```xaml
<syncfusion:NavigationItem Header="Refresh" ItemType="Button">
    <syncfusion:NavigationItem.Icon>
        <Path Data="M13.999999,3.9500002..." Fill="White"/>
    </syncfusion:NavigationItem.Icon>
</syncfusion:NavigationItem>
```

### 3. Header
- No interaction or selection
- Acts as a section header/label
- Only visible in expanded state
- Cannot have sub-items

```xaml
<syncfusion:NavigationItem Header="All Labels" ItemType="Header"/>
```

### 4. Separator
- Visual separator line
- No interaction or selection
- Cannot have sub-items

```xaml
<syncfusion:NavigationItem ItemType="Separator"/>
```

### Complete Example with All Types

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <!-- Tab items -->
    <syncfusion:NavigationItem Header="Inbox" IsSelected="True">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M42.128046,6.7269681..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Drafts">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M6.9999996,48.353..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <!-- Separator -->
    <syncfusion:NavigationItem ItemType="Separator"/>
    
    <!-- Header -->
    <syncfusion:NavigationItem Header="All Labels" ItemType="Header"/>
    
    <syncfusion:NavigationItem Header="Starred">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M25.085007,5.9780004..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Trash">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M17,12 L19,12..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <!-- Button type (at bottom) -->
    <syncfusion:SfNavigationDrawer.FooterItems>
        <syncfusion:NavigationItem Header="Settings" ItemType="Button">
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

## Data Binding with ItemsSource

Bind the navigation drawer to a collection of custom objects.

### Model Class

```csharp
public class NavigationModel
{
    public string Title { get; set; }
    public object Icon { get; set; }
}
```

### ViewModel

```csharp
public class ViewModel
{
    public ObservableCollection<NavigationModel> Items { get; set; }
    
    public ViewModel()
    {
        Items = new ObservableCollection<NavigationModel>();
        
        Items.Add(new NavigationModel()
        {
            Title = "Explore",
            Icon = new Path()
            {
                Data = Geometry.Parse("M6.0033803,5.705333..."),
                Fill = Brushes.White,
                Stretch = Stretch.Uniform
            }
        });
        
        Items.Add(new NavigationModel()
        {
            Title = "My music",
            Icon = new Path()
            {
                Data = Geometry.Parse("M2.5,10.701 C1.6729736..."),
                Fill = Brushes.White,
                Stretch = Stretch.Uniform
            }
        });
        
        Items.Add(new NavigationModel()
        {
            Title = "Recommended",
            Icon = new Path()
            {
                Data = Geometry.Parse("M11.5,10 C11.224,10..."),
                Fill = Brushes.White,
                Stretch = Stretch.Uniform
            }
        });
    }
}
```

### XAML with Data Binding

```xaml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               ItemsSource="{Binding Items}"
                               IconMemberPath="Icon">
    <syncfusion:SfNavigationDrawer.ItemTemplate>
        <DataTemplate>
            <Label Content="{Binding Title}"/>
        </DataTemplate>
    </syncfusion:SfNavigationDrawer.ItemTemplate>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Using DisplayMemberPath (Simpler Approach)

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               ItemsSource="{Binding Items}"
                               DisplayMemberPath="Title"
                               IconMemberPath="Icon">
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

## Hierarchical Data Binding

Create multi-level navigation from data sources.

### Model with Sub-items

```csharp
public class NavigationModel
{
    public string Title { get; set; }
    public object Icon { get; set; }
    public ObservableCollection<NavigationModel> SubItems { get; set; }
}
```

### ViewModel with Hierarchical Data

```csharp
public class ViewModel
{
    public ObservableCollection<NavigationModel> Items { get; set; }
    
    public ViewModel()
    {
        Items = new ObservableCollection<NavigationModel>();
        
        // Create sub-items
        ObservableCollection<NavigationModel> inboxSubItems = new ObservableCollection<NavigationModel>();
        inboxSubItems.Add(new NavigationModel() { Title = "Primary" });
        inboxSubItems.Add(new NavigationModel() { Title = "Social" });
        inboxSubItems.Add(new NavigationModel() { Title = "Promotions" });
        
        // Add parent item with sub-items
        Items.Add(new NavigationModel()
        {
            Title = "Inbox",
            Icon = new Path()
            {
                Data = Geometry.Parse("M32.032381,16.445429..."),
                Fill = Brushes.White,
                Stretch = Stretch.Uniform
            },
            SubItems = inboxSubItems
        });
        
        Items.Add(new NavigationModel()
        {
            Title = "Sent mail",
            Icon = new Path()
            {
                Data = Geometry.Parse("M42.128046,6.7269681..."),
                Fill = Brushes.White,
                Stretch = Stretch.Uniform
            }
        });
    }
}
```

### XAML with Hierarchical Binding

```xaml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Window.Resources>
    <Style x:Key="ItemStyle" TargetType="syncfusion:NavigationItem">
        <Setter Property="Icon" Value="{Binding Icon}"/>
        <Setter Property="DisplayMemberPath" Value="Title"/>
        <Setter Property="ItemsSource" Value="{Binding SubItems}"/>
    </Style>
</Window.Resources>

<syncfusion:SfNavigationDrawer DisplayMode="Expanded"
                               DisplayMemberPath="Title"
                               ItemContainerStyle="{StaticResource ItemStyle}"
                               ItemsSource="{Binding Items}">
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

## IconTemplate Customization

Use `IconTemplate` to display custom icons from image files or other sources.

### Model with Image Path

```csharp
public class NavigationModel
{
    public string Name { get; set; }
    public string IconPath { get; set; }
}
```

### ViewModel

```csharp
public class ViewModel
{
    public ObservableCollection<NavigationModel> Categories { get; set; }
    
    public ViewModel()
    {
        Categories = new ObservableCollection<NavigationModel>();
        Categories.Add(new NavigationModel() { Name = "Inbox", IconPath = "Inbox.png" });
        Categories.Add(new NavigationModel() { Name = "Sent", IconPath = "Sent.png" });
        Categories.Add(new NavigationModel() { Name = "Draft", IconPath = "Draft.png" });
        Categories.Add(new NavigationModel() { Name = "Spam", IconPath = "Spam.png" });
    }
}
```

### XAML with IconTemplate

```xaml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Window.Resources>
    <Style x:Key="ItemStyle" TargetType="syncfusion:NavigationItem">
        <Setter Property="IconTemplate">
            <Setter.Value>
                <DataTemplate>
                    <Image Width="16" Height="16" Source="{Binding}"/>
                </DataTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               DisplayMemberPath="Name"
                               IconMemberPath="IconPath"
                               ItemContainerStyle="{StaticResource ItemStyle}"
                               ItemsSource="{Binding Categories}">
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="ContentView"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

## ItemTemplateSelector

Use `ItemTemplateSelector` to apply different templates based on item properties.

### Custom TemplateSelector

```csharp
public class NavigationItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate SpecialTemplate { get; set; }
    
    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        var model = item as NavigationModel;
        if (model != null && model.Title == "Important")
        {
            return SpecialTemplate;
        }
        return DefaultTemplate;
    }
}
```

### XAML Implementation

```xaml
<Window.Resources>
    <DataTemplate x:Key="defaultTemplate">
        <TextBlock Text="{Binding Title}" FontSize="14"/>
    </DataTemplate>
    
    <DataTemplate x:Key="specialTemplate">
        <TextBlock Text="{Binding Title}" 
                   FontSize="16" 
                   FontWeight="Bold" 
                   Foreground="Red"/>
    </DataTemplate>
    
    <local:NavigationItemTemplateSelector x:Key="itemTemplateSelector"
                                          DefaultTemplate="{StaticResource defaultTemplate}"
                                          SpecialTemplate="{StaticResource specialTemplate}"/>
</Window.Resources>

<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               ItemsSource="{Binding Items}"
                               IconMemberPath="Icon"
                               ItemTemplateSelector="{StaticResource itemTemplateSelector}">
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

## IndentationWidth

Control the horizontal spacing of sub-items using `IndentationWidth`.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               IndentationWidth="40">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
        
        <!-- Sub-items indented by 40 pixels -->
        <syncfusion:NavigationItem Header="Primary">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M9.5189972,7.3780194..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="Social">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M22.133972,14.194015..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:NavigationItem>
</syncfusion:SfNavigationDrawer>
```

**Default:** 16 pixels  
**Use case:** Adjust visual hierarchy for deeply nested items

## Popup Support for Sub-items

In Compact and Expanded modes, sub-items appear in a popup when the parent item is collapsed or in compact state.

### Example

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact">
    <syncfusion:NavigationItem Header="Important">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M25.085007,5.9780004..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
        
        <!-- These appear in popup when drawer is compact -->
        <syncfusion:NavigationItem Header="Work">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M16.151009,4.5999908..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="Personal">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12.200012,21.899996..." Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:NavigationItem>
</syncfusion:SfNavigationDrawer>
```

**Behavior:**
- **Compact mode:** Sub-items show in popup on hover/click
- **Expanded mode (collapsed parent):** Sub-items show in popup
- **Expanded mode (expanded parent):** Sub-items show inline

## Sample Code

View complete samples on GitHub:
- [Populating with Items](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Populating_With_Items)
- [Hierarchical Data Binding](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Hierarchical_Data_Binding)

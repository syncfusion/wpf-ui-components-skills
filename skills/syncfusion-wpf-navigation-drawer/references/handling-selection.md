# Handling Selection in WPF Navigation Drawer

Complete guide for managing selected items in the Navigation Drawer.

## Overview

The `SelectedItem` property provides access to the currently selected navigation item. The type depends on how the drawer is populated:

- **NavigationItem objects:** SelectedItem is of type `NavigationItem`
- **Custom data binding:** SelectedItem is of your custom object type

## SelectedItem with NavigationItem

When using built-in NavigationItem objects (Compact/Expanded modes).

### Getting Selected Item

```csharp
// Access selected NavigationItem
var selectedItem = navigationDrawer.SelectedItem as NavigationItem;

if (selectedItem != null)
{
    string header = selectedItem.Header;
    bool isExpanded = selectedItem.IsExpanded;
    
    // Use selected item info
    MessageBox.Show($"Selected: {header}");
}
```

### Setting Selected Item Programmatically

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer" 
                               DisplayMode="Expanded">
    <syncfusion:NavigationItem x:Name="inboxItem" 
                               Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem x:Name="sentItem" 
                               Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M2,21L23,12L2,3v7l15,2L2,13V21z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem x:Name="draftsItem" 
                               Header="Drafts">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M3,3h18v14H3V3z M5,5v10h14V5H5z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <StackPanel>
            <Button Content="Select Inbox" Click="SelectInbox_Click"/>
            <Button Content="Select Sent" Click="SelectSent_Click"/>
            <Button Content="Select Drafts" Click="SelectDrafts_Click"/>
        </StackPanel>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void SelectInbox_Click(object sender, RoutedEventArgs e)
{
    navigationDrawer.SelectedItem = inboxItem;
}

private void SelectSent_Click(object sender, RoutedEventArgs e)
{
    navigationDrawer.SelectedItem = sentItem;
}

private void SelectDrafts_Click(object sender, RoutedEventArgs e)
{
    navigationDrawer.SelectedItem = draftsItem;
}
```

### Setting via IsSelected Property

```xaml
<syncfusion:NavigationItem Header="Inbox" IsSelected="True">
    <syncfusion:NavigationItem.Icon>
        <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
    </syncfusion:NavigationItem.Icon>
</syncfusion:NavigationItem>
```

**Code-behind:**
```csharp
NavigationItem inboxItem = new NavigationItem()
{
    Header = "Inbox",
    IsSelected = true,
    Icon = new Path()
    {
        Data = Geometry.Parse("M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z"),
        Fill = Brushes.White
    }
};

navigationDrawer.Items.Add(inboxItem);
```

## SelectedItem with Custom Objects

When binding to custom data objects using ItemsSource.

### Model Class

```csharp
public class NavigationModel
{
    public string Title { get; set; }
    public object Icon { get; set; }
    public string PageName { get; set; }
}
```

### ViewModel

```csharp
public class ViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    public ObservableCollection<NavigationModel> Items { get; set; }
    
    private NavigationModel _selectedItem;
    public NavigationModel SelectedItem
    {
        get { return _selectedItem; }
        set
        {
            _selectedItem = value;
            OnPropertyChanged("SelectedItem");
            
            // Navigate based on selection
            if (_selectedItem != null)
            {
                NavigateToPage(_selectedItem.PageName);
            }
        }
    }
    
    public ViewModel()
    {
        Items = new ObservableCollection<NavigationModel>();
        
        Items.Add(new NavigationModel()
        {
            Title = "Dashboard",
            PageName = "DashboardPage",
            Icon = new Path()
            {
                Data = Geometry.Parse("M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z"),
                Fill = Brushes.White
            }
        });
        
        Items.Add(new NavigationModel()
        {
            Title = "Reports",
            PageName = "ReportsPage",
            Icon = new Path()
            {
                Data = Geometry.Parse("M2,21L23,12L2,3v7l15,2L2,13V21z"),
                Fill = Brushes.White
            }
        });
        
        Items.Add(new NavigationModel()
        {
            Title = "Settings",
            PageName = "SettingsPage",
            Icon = new Path()
            {
                Data = Geometry.Parse("M3,3h18v14H3V3z M5,5v10h14V5H5z"),
                Fill = Brushes.White
            }
        });
        
        // Set initial selection
        SelectedItem = Items[0];
    }
    
    private void NavigateToPage(string pageName)
    {
        // Your navigation logic
        System.Diagnostics.Debug.WriteLine($"Navigating to: {pageName}");
    }
    
    protected void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
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
                               SelectedItem="{Binding SelectedItem, Mode=TwoWay}"
                               DisplayMemberPath="Title"
                               IconMemberPath="Icon">
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid>
            <TextBlock Text="{Binding SelectedItem.Title}" 
                       HorizontalAlignment="Center"
                       VerticalAlignment="Center"
                       FontSize="24"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Getting Selected Custom Object

```csharp
// Access selected custom object
var selectedItem = navigationDrawer.SelectedItem as NavigationModel;

if (selectedItem != null)
{
    string title = selectedItem.Title;
    string pageName = selectedItem.PageName;
    
    // Use selected item
    NavigateToPage(pageName);
}
```

## SelectionBackground Customization

Customize the appearance of the selected item.

### Setting SelectionBackground

```xaml
<syncfusion:NavigationItem Header="Inbox" 
                           IsSelected="True"
                           SelectionBackground="#FF6200EE">
    <syncfusion:NavigationItem.Icon>
        <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
    </syncfusion:NavigationItem.Icon>
</syncfusion:NavigationItem>
```

**Code-behind:**
```csharp
NavigationItem item = new NavigationItem()
{
    Header = "Inbox",
    IsSelected = true,
    SelectionBackground = new SolidColorBrush(
        (Color)ColorConverter.ConvertFromString("#FF6200EE")
    ),
    Icon = new Path()
    {
        Data = Geometry.Parse("M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z"),
        Fill = Brushes.White
    }
};
```

### Transparent Selection (Button-style Items)

```xaml
<syncfusion:SfNavigationDrawer.FooterItems>
    <syncfusion:NavigationItem Header="Settings" 
                               SelectionBackground="Transparent">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M3,6h18v2H3V6z M6,8v12c0,1.1 0.9,2 2,2h8c1.1,0 2-0.9 2-2V8H6z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
</syncfusion:SfNavigationDrawer.FooterItems>
```

### Gradient SelectionBackground

```xaml
<syncfusion:NavigationItem Header="Inbox" IsSelected="True">
    <syncfusion:NavigationItem.SelectionBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="#667eea" Offset="0"/>
            <GradientStop Color="#764ba2" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:NavigationItem.SelectionBackground>
    <syncfusion:NavigationItem.Icon>
        <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
    </syncfusion:NavigationItem.Icon>
</syncfusion:NavigationItem>
```

## IsChildSelected Property

Check if any sub-item is selected within a parent item.

```csharp
NavigationItem parentItem = navigationDrawer.Items[0] as NavigationItem;

if (parentItem != null && parentItem.IsChildSelected)
{
    // One of the sub-items is selected
    MessageBox.Show("A sub-item is selected");
}
```

## Complete Selection Example

```xaml
<Window.DataContext>
    <local:NavigationViewModel/>
</Window.DataContext>

<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    
    <syncfusion:SfNavigationDrawer x:Name="navigationDrawer"
                                   DisplayMode="Expanded"
                                   ItemsSource="{Binding NavigationItems}"
                                   SelectedItem="{Binding SelectedItem, Mode=TwoWay}"
                                   DisplayMemberPath="Title"
                                   IconMemberPath="Icon">
        <syncfusion:SfNavigationDrawer.ContentView>
            <Grid Background="White"/>
        </syncfusion:SfNavigationDrawer.ContentView>
    </syncfusion:SfNavigationDrawer>
    
    <Grid Grid.Column="1" Background="White">
        <StackPanel VerticalAlignment="Center" 
                    HorizontalAlignment="Center">
            <TextBlock Text="{Binding SelectedItem.Title}" 
                       FontSize="32" 
                       FontWeight="Bold"
                       Margin="0,0,0,20"/>
            <TextBlock Text="{Binding SelectedItem.Description}" 
                       FontSize="16"
                       Foreground="Gray"/>
        </StackPanel>
    </Grid>
</Grid>
```

**ViewModel:**
```csharp
public class NavigationViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    public ObservableCollection<PageModel> NavigationItems { get; set; }
    
    private PageModel _selectedItem;
    public PageModel SelectedItem
    {
        get { return _selectedItem; }
        set
        {
            _selectedItem = value;
            OnPropertyChanged("SelectedItem");
            System.Diagnostics.Debug.WriteLine($"Selected: {_selectedItem?.Title}");
        }
    }
    
    public NavigationViewModel()
    {
        NavigationItems = new ObservableCollection<PageModel>
        {
            new PageModel 
            { 
                Title = "Dashboard", 
                Description = "Overview of your data",
                Icon = CreateIcon("M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z")
            },
            new PageModel 
            { 
                Title = "Analytics", 
                Description = "Detailed analytics and reports",
                Icon = CreateIcon("M2,21L23,12L2,3v7l15,2L2,13V21z")
            },
            new PageModel 
            { 
                Title = "Settings", 
                Description = "Configure your preferences",
                Icon = CreateIcon("M3,3h18v14H3V3z M5,5v10h14V5H5z")
            }
        };
        
        // Set initial selection
        SelectedItem = NavigationItems[0];
    }
    
    private Path CreateIcon(string data)
    {
        return new Path
        {
            Data = Geometry.Parse(data),
            Fill = Brushes.White,
            Stretch = Stretch.Uniform
        };
    }
    
    protected void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}

public class PageModel
{
    public string Title { get; set; }
    public string Description { get; set; }
    public Path Icon { get; set; }
}
```

## Best Practices

1. **Two-Way Binding:** Use `Mode=TwoWay` for SelectedItem binding to keep ViewModel in sync
2. **Initial Selection:** Always set an initial SelectedItem to avoid null states
3. **Null Checks:** Always check if SelectedItem is null before accessing properties
4. **Navigation:** Trigger navigation logic when SelectedItem changes
5. **State Persistence:** Save/restore SelectedItem across app sessions if needed

## Sample Code

View complete sample on GitHub: [Handling Selection Sample](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Handling_Selection)


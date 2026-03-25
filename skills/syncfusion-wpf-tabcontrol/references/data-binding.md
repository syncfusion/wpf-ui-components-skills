# Data Binding with TabControl

Learn how to bind TabControl and TabItems to data sources using MVVM patterns and data templates.

## Basic Data Binding

### Binding Items Source

Bind the TabControl items to a collection in your view model:

```xml
<syncfusion:TabControlExt ItemsSource="{Binding TabItems}" 
                          SelectedItem="{Binding SelectedTab}"
                          Name="tabControl" />
```

### View Model Setup

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class MainViewModel : INotifyPropertyChanged
{
    private ObservableCollection<TabItemViewModel> _tabItems;
    private TabItemViewModel _selectedTab;
    
    public ObservableCollection<TabItemViewModel> TabItems
    {
        get { return _tabItems; }
        set
        {
            if (_tabItems != value)
            {
                _tabItems = value;
                OnPropertyChanged(nameof(TabItems));
            }
        }
    }
    
    public TabItemViewModel SelectedTab
    {
        get { return _selectedTab; }
        set
        {
            if (_selectedTab != value)
            {
                _selectedTab = value;
                OnPropertyChanged(nameof(SelectedTab));
            }
        }
    }
    
    public MainViewModel()
    {
        TabItems = new ObservableCollection<TabItemViewModel>();
        LoadTabItems();
    }
    
    private void LoadTabItems()
    {
        TabItems.Add(new TabItemViewModel { Title = "Home", Content = "Home content" });
        TabItems.Add(new TabItemViewModel { Title = "Settings", Content = "Settings content" });
        TabItems.Add(new TabItemViewModel { Title = "About", Content = "About content" });
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class TabItemViewModel
{
    public string Title { get; set; }
    public string Content { get; set; }
}
```

## Using ItemTemplate

Define how each tab item is displayed using `ItemTemplate`:

### Template for Tab Headers

```xml
<syncfusion:TabControlExt ItemsSource="{Binding TabItems}"
                          SelectedItem="{Binding SelectedTab}"
                          Name="tabControl">
    <syncfusion:TabControlExt.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Title}" Margin="10,0" />
        </DataTemplate>
    </syncfusion:TabControlExt.ItemTemplate>
</syncfusion:TabControlExt>
```

### Rich Header Template

Create complex tab headers with images, badges, and buttons:

```xml
<syncfusion:TabControlExt ItemsSource="{Binding TabItems}" Name="tabControl">
    <syncfusion:TabControlExt.ItemTemplate>
        <DataTemplate>
            <Grid Background="White" Margin="2">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="20" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                
                <!-- Icon -->
                <Image Source="{Binding IconPath}" Grid.Column="0" Width="16" Height="16" />
                
                <!-- Title -->
                <TextBlock Text="{Binding Title}" Grid.Column="1" Margin="5,0,0,0" VerticalAlignment="Center" />
                
                <!-- Status Badge -->
                <Ellipse Grid.Column="2" Width="10" Height="10" Margin="5,0,0,0"
                         Fill="{Binding StatusColor}" />
            </Grid>
        </DataTemplate>
    </syncfusion:TabControlExt.ItemTemplate>
</syncfusion:TabControlExt>
```

**Updated View Model:**

```csharp
public class TabItemViewModel
{
    public string Title { get; set; }
    public string IconPath { get; set; }
    public string StatusColor { get; set; }
    public object Content { get; set; }
}
```

## Using ContentTemplate

Define how tab content is displayed:

### Basic Content Template

```xml
<syncfusion:TabControlExt ItemsSource="{Binding TabItems}"
                          SelectedItem="{Binding SelectedTab}"
                          Name="tabControl">
    <syncfusion:TabControlExt.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Title}" />
        </DataTemplate>
    </syncfusion:TabControlExt.ItemTemplate>
    
    <syncfusion:TabControlExt.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Content}" Margin="10" />
        </DataTemplate>
    </syncfusion:TabControlExt.ContentTemplate>
</syncfusion:TabControlExt>
```

### Complex Content Template with Views

Use different views for different tab types:

```xml
<syncfusion:TabControlExt ItemsSource="{Binding TabItems}"
                          SelectedItem="{Binding SelectedTab}"
                          Name="tabControl">
    <syncfusion:TabControlExt.ContentTemplate>
        <DataTemplate>
            <ContentControl Content="{Binding}">
                <ContentControl.ContentTemplateSelector>
                    <local:TabContentTemplateSelector />
                </ContentControl.ContentTemplateSelector>
            </ContentControl>
        </DataTemplate>
    </syncfusion:TabControlExt.ContentTemplate>
</syncfusion:TabControlExt>
```

**Template Selector:**

```csharp
public class TabContentTemplateSelector : DataTemplateSelector
{
    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        FrameworkElement element = container as FrameworkElement;
        TabItemViewModel viewModel = item as TabItemViewModel;
        
        if (viewModel?.Type == "Grid")
        {
            return element?.FindResource("GridTemplate") as DataTemplate;
        }
        else if (viewModel?.Type == "Text")
        {
            return element?.FindResource("TextTemplate") as DataTemplate;
        }
        
        return element?.FindResource("DefaultTemplate") as DataTemplate;
    }
}
```

## Observable Collection Pattern

Use `ObservableCollection<T>` for automatic UI updates when tabs are added/removed:

### Adding Tabs Dynamically

```csharp
public void AddNewTab()
{
    var newTab = new TabItemViewModel 
    { 
        Title = $"Tab {TabItems.Count + 1}",
        Content = "New tab content"
    };
    
    TabItems.Add(newTab);
    SelectedTab = newTab;  // Select the new tab
}

public void RemoveTab(TabItemViewModel tab)
{
    TabItems.Remove(tab);
}

public void ClearAllTabs()
{
    TabItems.Clear();
}
```

### Two-Way Binding for Selection

```xml
<syncfusion:TabControlExt ItemsSource="{Binding TabItems}"
                          SelectedItem="{Binding SelectedTab, Mode=TwoWay}"
                          Name="tabControl" />
```

## Data Binding Scenarios

### Scenario 1: Document Editor with File Tabs

```csharp
public class DocumentViewModel : INotifyPropertyChanged
{
    public ObservableCollection<DocumentTabViewModel> OpenDocuments { get; set; }
    public DocumentTabViewModel SelectedDocument { get; set; }
    
    public class DocumentTabViewModel
    {
        public string FileName { get; set; }
        public string FilePath { get; set; }
        public bool IsModified { get; set; }
        public string DocumentContent { get; set; }
        
        public string DisplayTitle => IsModified ? $"{FileName}*" : FileName;
    }
}
```

**XAML:**

```xml
<syncfusion:TabControlExt ItemsSource="{Binding OpenDocuments}"
                          SelectedItem="{Binding SelectedDocument, Mode=TwoWay}"
                          CloseButtonType="Individual">
    <syncfusion:TabControlExt.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding DisplayTitle}" Margin="5,0" />
        </DataTemplate>
    </syncfusion:TabControlExt.ItemTemplate>
    
    <syncfusion:TabControlExt.ContentTemplate>
        <DataTemplate>
            <TextBox Text="{Binding DocumentContent}" Margin="10" TextWrapping="Wrap" />
        </DataTemplate>
    </syncfusion:TabControlExt.ContentTemplate>
</syncfusion:TabControlExt>
```

### Scenario 2: Settings Panel with Grouped Tabs

```csharp
public class SettingsViewModel
{
    public ObservableCollection<SettingCategory> Categories { get; set; }
    public SettingCategory SelectedCategory { get; set; }
    
    public SettingsViewModel()
    {
        Categories = new ObservableCollection<SettingCategory>
        {
            new SettingCategory 
            { 
                Name = "General",
                Settings = new ObservableCollection<Setting>()
                {
                    new Setting { Name = "Theme", Value = "Dark" },
                    new Setting { Name = "Language", Value = "English" }
                }
            },
            new SettingCategory 
            { 
                Name = "Advanced",
                Settings = new ObservableCollection<Setting>()
                {
                    new Setting { Name = "Debug Mode", Value = "Off" }
                }
            }
        };
        
        SelectedCategory = Categories[0];
    }
}

public class SettingCategory
{
    public string Name { get; set; }
    public ObservableCollection<Setting> Settings { get; set; }
}

public class Setting
{
    public string Name { get; set; }
    public string Value { get; set; }
}
```

### Scenario 3: Dynamic Tab Creation from Data Source

```csharp
public async Task LoadTabsFromDatabase()
{
    var data = await _database.GetAllTabsAsync();
    
    foreach (var item in data)
    {
        TabItems.Add(new TabItemViewModel
        {
            Title = item.Title,
            Content = item.Content,
            IconPath = item.IconPath
        });
    }
}
```

## Handling Selection Changes in MVVM

Monitor selected tab changes in your view model:

```csharp
private TabItemViewModel _selectedTab;

public TabItemViewModel SelectedTab
{
    get { return _selectedTab; }
    set
    {
        if (_selectedTab != value)
        {
            _selectedTab = value;
            OnPropertyChanged(nameof(SelectedTab));
            OnTabSelected(_selectedTab);
        }
    }
}

private void OnTabSelected(TabItemViewModel tab)
{
    // Perform actions when tab is selected
    if (tab != null)
    {
        LoadTabContent(tab);
        UpdateUI(tab);
    }
}
```

## Data Binding Best Practices

1. **Use ObservableCollection**: For automatic collection updates
2. **INotifyPropertyChanged**: Implement for property change notifications
3. **Two-Way Binding**: For selected item binding: `Mode=TwoWay`
4. **Template Selectors**: For complex content display logic
5. **Lazy Loading**: Load tab content only when tab is selected
6. **Weak Event Patterns**: Prevent memory leaks from event subscriptions
7. **Async Data Loading**: Load data asynchronously to avoid UI freezing

## Common Data Binding Issues

**Issue: Items not updating in UI**
- Solution: Use `ObservableCollection<T>` instead of `List<T>`
- Verify `INotifyPropertyChanged` is implemented correctly

**Issue: SelectedItem not binding**
- Solution: Use `Mode=TwoWay` binding
- Ensure selected item is actually in the Items collection

**Issue: Template not being applied**
- Solution: Check data type in template selector
- Verify template resources are properly defined

**Issue: Performance degradation with many tabs**
- Solution: Implement virtualization
- Use lazy loading for tab content
- Consider pagination instead of many tabs

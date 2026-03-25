# Ribbon Gallery Control

## Table of Contents
- [Gallery Basics](#gallery-basics)
- [Gallery Items & Templates](#gallery-items--templates)
- [Selection & Events](#selection--events)
- [Gallery Groups](#gallery-groups)
- [Filtering & Searching](#filtering--searching)
- [Data Binding](#data-binding)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Gallery Basics

### Simple Gallery

A gallery displays a collection of visual items for quick selection:

```xml
<syncfusion:RibbonGallery Label="Colors">
    <syncfusion:RibbonGalleryItem Content="Red" />
    <syncfusion:RibbonGalleryItem Content="Green" />
    <syncfusion:RibbonGalleryItem Content="Blue" />
    <syncfusion:RibbonGalleryItem Content="Yellow" />
    <syncfusion:RibbonGalleryItem Content="Orange" />
    <syncfusion:RibbonGalleryItem Content="Purple" />
</syncfusion:RibbonGallery>
```

### Gallery with Size

Control gallery dimensions:

```xml
<syncfusion:RibbonGallery Label="Shapes">
    <syncfusion:RibbonGalleryItem Content="Circle" />
    <syncfusion:RibbonGalleryItem Content="Rectangle" />
    <syncfusion:RibbonGalleryItem Content="Triangle" />
    <!-- More items -->
</syncfusion:RibbonGallery>
```

### Gallery Sizing Options

```xml
<!-- Compact gallery -->
<syncfusion:RibbonGallery Label="Small Gallery" />

<!-- Large gallery -->
<syncfusion:RibbonGallery Label="Large Gallery" />
```

## Gallery Items & Templates

### Basic Gallery Items

```xml
<syncfusion:RibbonGallery Label="Styles">
    <syncfusion:RibbonGalleryItem Content="Heading 1" />
    <syncfusion:RibbonGalleryItem Content="Heading 2" />
    <syncfusion:RibbonGalleryItem Content="Normal Text" />
</syncfusion:RibbonGallery>
```

### Items with Icons/Images

```xml
<syncfusion:RibbonGallery Label="Shapes">
    <syncfusion:RibbonGalleryItem>
        <Image Source="pack://application:,,,/Resources/circle.png" 
               Width="40" Height="40" />
    </syncfusion:RibbonGalleryItem>
    
    <syncfusion:RibbonGalleryItem>
        <Image Source="pack://application:,,,/Resources/rectangle.png"
               Width="40" Height="40" />
    </syncfusion:RibbonGalleryItem>
    
    <syncfusion:RibbonGalleryItem>
        <Image Source="pack://application:,,,/Resources/triangle.png"
               Width="40" Height="40" />
    </syncfusion:RibbonGalleryItem>
</syncfusion:RibbonGallery>
```

### Custom Item Template

Define a template for gallery items:

```xml
<syncfusion:RibbonGallery Label="Themes">
    <syncfusion:RibbonGallery.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Vertical">
                <Rectangle Width="60" Height="40" 
                           Fill="{Binding Color}" />
                <TextBlock Text="{Binding Name}" 
                           FontSize="10"
                           TextAlignment="Center" />
            </StackPanel>
        </DataTemplate>
    </syncfusion:RibbonGallery.ItemTemplate>
    
    <!-- Items bound to view model -->
</syncfusion:RibbonGallery>
```

## Selection & Events

### Handle Gallery Selection

```xml
<syncfusion:RibbonGallery Label="Colors"
                          ItemSelectionChangedCommand="{Binding ColorSelectedCommand}">
    <syncfusion:RibbonGalleryItem Content="Red" />
    <syncfusion:RibbonGalleryItem Content="Green" />
    <syncfusion:RibbonGalleryItem Content="Blue" />
</syncfusion:RibbonGallery>
```

```csharp
// In ViewModel
public ICommand ColorSelectedCommand { get; }

public MyViewModel()
{
    ColorSelectedCommand = new RelayCommand<object>(
        execute: (param) =>
        {
            if (param is RibbonGalleryItem item)
            {
                ApplyColor(item.Content.ToString());
            }
        }
    );
}
```

### Preview Selection

Show preview before applying:

```csharp
public class MyViewModel
{
    public ICommand PreviewColorCommand { get; }
    public ICommand ApplyColorCommand { get; }

    public MyViewModel()
    {
        PreviewColorCommand = new RelayCommand<object>(
            execute: (color) => PreviewColor(color?.ToString())
        );

        ApplyColorCommand = new RelayCommand<object>(
            execute: (color) => ApplyColor(color?.ToString())
        );
    }

    private void PreviewColor(string colorName)
    {
        // Temporarily apply color for preview
    }

    private void ApplyColor(string colorName)
    {
        // Permanently apply color
    }
}
```

## Gallery Groups

### Grouped Gallery

Organize items into categories:

```xml
<syncfusion:RibbonGallery Label="Text Styles">
    <!-- Group 1: Built-in Styles -->
    <syncfusion:RibbonGalleryGroup Label="Built-in">
        <syncfusion:RibbonGalleryItem Content="Title" />
        <syncfusion:RibbonGalleryItem Content="Heading 1" />
        <syncfusion:RibbonGalleryItem Content="Heading 2" />
    </syncfusion:RibbonGalleryGroup>
    
    <!-- Group 2: Custom Styles -->
    <syncfusion:RibbonGalleryGroup Label="Custom">
        <syncfusion:RibbonGalleryItem Content="Custom Style 1" />
        <syncfusion:RibbonGalleryItem Content="Custom Style 2" />
    </syncfusion:RibbonGalleryGroup>
    
    <!-- Group 3: Theme Styles -->
    <syncfusion:RibbonGalleryGroup Label="Themes">
        <syncfusion:RibbonGalleryItem Content="Office" />
        <syncfusion:RibbonGalleryItem Content="Professional" />
    </syncfusion:RibbonGalleryGroup>
</syncfusion:RibbonGallery>
```

### Collapsible Groups

```xml
<syncfusion:RibbonGalleryGroup Label="Colors">
    <syncfusion:RibbonGalleryItem Content="Red" />
    <syncfusion:RibbonGalleryItem Content="Blue" />
</syncfusion:RibbonGalleryGroup>

<syncfusion:RibbonGalleryGroup Label="More Colors">
    <syncfusion:RibbonGalleryItem Content="Cyan" />
    <syncfusion:RibbonGalleryItem Content="Magenta" />
</syncfusion:RibbonGalleryGroup>
```

## Filtering & Searching

### Filter Gallery Items

Filter items based on criteria:

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    private ObservableCollection<GalleryItem> _allItems;
    private ObservableCollection<GalleryItem> _filteredItems;

    public ObservableCollection<GalleryItem> FilteredItems
    {
        get => _filteredItems;
        set
        {
            if (_filteredItems != value)
            {
                _filteredItems = value;
                OnPropertyChanged(nameof(FilteredItems));
            }
        }
    }

    private string _filterText;
    public string FilterText
    {
        get => _filterText;
        set
        {
            if (_filterText != value)
            {
                _filterText = value;
                ApplyFilter();
                OnPropertyChanged(nameof(FilterText));
            }
        }
    }

    public MyViewModel()
    {
        _allItems = new ObservableCollection<GalleryItem>
        {
            new GalleryItem { Name = "Red", Color = "Red" },
            new GalleryItem { Name = "Blue", Color = "Blue" },
            new GalleryItem { Name = "Green", Color = "Green" }
        };
        FilteredItems = _allItems;
    }

    private void ApplyFilter()
    {
        if (string.IsNullOrEmpty(FilterText))
        {
            FilteredItems = _allItems;
        }
        else
        {
            var filtered = new ObservableCollection<GalleryItem>(
                _allItems.Where(i => i.Name.Contains(FilterText))
            );
            FilteredItems = filtered;
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, 
            new PropertyChangedEventArgs(propertyName));
    }
}

public class GalleryItem
{
    public string Name { get; set; }
    public string Color { get; set; }
}
```

### Search-Enabled Gallery

Add search/filter box with gallery:

```xml
<syncfusion:RibbonBar Header="Search Styles">
    <!-- Search box -->
    <syncfusion:RibbonTextBox Text="Filter:" 
                              Text="{Binding FilterText, UpdateSourceTrigger=PropertyChanged}"
                              Width="150" />
    
    <!-- Filtered gallery -->
    <syncfusion:RibbonGallery Label="Styles"
                              ItemsSource="{Binding FilteredItems}" />
</syncfusion:RibbonBar>
```

## Data Binding

### Binding Items to View Model

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    public ObservableCollection<string> GalleryItems { get; set; }

    public MyViewModel()
    {
        GalleryItems = new ObservableCollection<string>
        {
            "Style 1",
            "Style 2",
            "Style 3",
            "Style 4",
            "Style 5"
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, 
            new PropertyChangedEventArgs(propertyName));
    }
}
```

```xml
<!-- XAML -->
<syncfusion:RibbonGallery Label="Styles"
                          ItemsSource="{Binding GalleryItems}" />

### Binding Complex Objects

For galleries with images or rich data:

```csharp
public class ThemeItem
{
    public string Name { get; set; }
    public string ImagePath { get; set; }
    public Color PrimaryColor { get; set; }
    public Color AccentColor { get; set; }
}

public class MyViewModel
{
    public ObservableCollection<ThemeItem> Themes { get; set; }

    public MyViewModel()
    {
        Themes = new ObservableCollection<ThemeItem>
        {
            new ThemeItem 
            { 
                Name = "Light", 
                ImagePath = "pack://application:,,,/Resources/light-theme.png",
                PrimaryColor = Colors.White,
                AccentColor = Colors.Black
            },
            new ThemeItem 
            { 
                Name = "Dark", 
                ImagePath = "pack://application:,,,/Resources/dark-theme.png",
                PrimaryColor = Colors.Black,
                AccentColor = Colors.White
            }
        };
    }
}
```

```xml
<syncfusion:RibbonGallery Label="Themes" ItemsSource="{Binding Themes}">
    <syncfusion:RibbonGallery.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Vertical">
                <Image Source="{Binding ImagePath}" Width="50" Height="50" />
                <TextBlock Text="{Binding Name}" 
                           FontSize="10"
                           TextAlignment="Center" />
            </StackPanel>
        </DataTemplate>
    </syncfusion:RibbonGallery.ItemTemplate>
</syncfusion:RibbonGallery>
```

## Common Patterns

### Pattern 1: Chart Type Selection Gallery
```xml
<syncfusion:RibbonGallery Label="Chart Types">
    <syncfusion:RibbonGalleryItem Content="Column Chart" />
    <syncfusion:RibbonGalleryItem Content="Line Chart" />
    <syncfusion:RibbonGalleryItem Content="Pie Chart" />
    <syncfusion:RibbonGalleryItem Content="Area Chart" />
    <syncfusion:RibbonGalleryItem Content="Bar Chart" />
    <syncfusion:RibbonGalleryItem Content="Combo Chart" />
</syncfusion:RibbonGallery>
```

### Pattern 2: Theme Selection with Preview
```xml
<syncfusion:RibbonGallery Label="Themes"
                          ItemsSource="{Binding AvailableThemes}"
                          ItemSelectionChangedCommand="{Binding PreviewThemeCommand}" />
```
With command handling that previews theme before committing.

### Pattern 3: Grouped Style Gallery
```xml
<syncfusion:RibbonGallery Label="Styles">
    <syncfusion:RibbonGalleryGroup Label="Paragraph Styles">
        <syncfusion:RibbonGalleryItem Content="Normal" />
        <syncfusion:RibbonGalleryItem Content="Heading 1" />
    </syncfusion:RibbonGalleryGroup>
    
    <syncfusion:RibbonGalleryGroup Label="Text Styles">
        <syncfusion:RibbonGalleryItem Content="Strong" />
        <syncfusion:RibbonGalleryItem Content="Emphasis" />
    </syncfusion:RibbonGalleryGroup>
</syncfusion:RibbonGallery>
```

## Troubleshooting

### Issue: Gallery items not showing
**Solution:** Verify items are `RibbonGalleryItem` elements.

### Issue: Selection event not firing
**Solution:** Use `ItemSelectionChangedCommand` (MVVM) or ensure event handler is properly attached.

### Issue: Data binding not updating gallery
**Solution:** Ensure source collection implements `INotifyCollectionChanged` (use `ObservableCollection`).

### Issue: Custom templates not rendering
**Solution:** Check `ItemTemplate` DataContext. Ensure template elements have valid bindings.

### Issue: Gallery too large/small
**Solution:** The gallery automatically adjusts to the available space in the ribbon bar.
### Issue: Groups not displaying
**Solution:** Use `RibbonGalleryGroup` elements. Ensure they contain `RibbonGalleryItem` children.

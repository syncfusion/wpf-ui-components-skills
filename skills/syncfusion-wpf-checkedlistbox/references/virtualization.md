# Virtualization and Performance

## Table of Contents
- [Overview](#overview)
- [How Virtualization Works](#how-virtualization-works)
- [Default Virtualization Behavior](#default-virtualization-behavior)
- [Disabling Virtualization](#disabling-virtualization)
- [When to Use Virtualization](#when-to-use-virtualization)
- [Performance Best Practices](#performance-best-practices)

## Overview

UI Virtualization is a powerful performance optimization technique that loads only visible items into memory, dramatically improving performance when working with large datasets. The CheckListBox has virtualization **enabled by default**, allowing you to display thousands or even millions of items without affecting loading or scrolling performance.

**Key benefits:**
- **Fast loading** - Control loads instantly regardless of item count
- **Smooth scrolling** - No lag when scrolling through thousands of items
- **Low memory usage** - Only visible items consume memory
- **Automatic** - Works out-of-the-box, no configuration needed

**How it helps:**
- Load 10,000 items in milliseconds instead of seconds
- Reduce memory usage from hundreds of MB to just a few MB
- Maintain responsive UI even with massive datasets
- Scale to enterprise-level data without performance degradation

## How Virtualization Works

### The Problem Without Virtualization

When you add 10,000 items to a non-virtualized list:
1. **All 10,000 UI elements are created** in memory
2. Each element consumes memory (~1-5 KB each)
3. Total memory: ~10-50 MB just for UI elements
4. Loading time: Several seconds
5. Scrolling performance: Laggy and slow

### The Solution With Virtualization

With virtualization enabled (default):
1. **Only ~20-30 visible UI elements are created**
2. As you scroll, items are recycled and reused
3. Total memory: ~20-150 KB for UI elements
4. Loading time: < 100 milliseconds
5. Scrolling performance: Smooth and instant

**Visual representation:**

```
[Items 1-10 visible on screen] ← These are rendered
[Items 11-9,990]               ← These DON'T exist in UI yet
[Items 9,991-10,000 at bottom] ← These DON'T exist in UI yet

When you scroll down:
- Items 1-5 are recycled
- Items 15-20 are created using recycled containers
```

## Default Virtualization Behavior

Virtualization is **enabled by default** using WPF's `VirtualizingStackPanel`.

### Basic Usage (Virtualization Enabled)

```csharp
// Model
public class Item
{
    public string Name { get; set; }
    public string GroupName { get; set; }
}

// ViewModel
public class ViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    private ObservableCollection<Item> _items;
    
    public ObservableCollection<Item> Items
    {
        get => _items;
        set
        {
            _items = value;
            OnPropertyChanged(nameof(Items));
        }
    }
    
    public ViewModel()
    {
        Items = new ObservableCollection<Item>();
        
        // Add 10,000 items - loads instantly!
        for (int i = 0; i < 10000; i++)
        {
            Items.Add(new Item
            {
                Name = $"Item {i}",
                GroupName = $"Group {i % 100}"
            });
        }
    }
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:VirtualizationDemo">
    
    <Grid>
        <syncfusion:CheckListBox ItemsSource="{Binding Items}"
                                 DisplayMemberPath="Name"
                                 Margin="20">
            <syncfusion:CheckListBox.DataContext>
                <local:ViewModel />
            </syncfusion:CheckListBox.DataContext>
        </syncfusion:CheckListBox>
    </Grid>
</Window>
```

**Result:** 10,000 items load instantly, scroll smoothly, use minimal memory.

### Virtualization with Grouping

Virtualization works seamlessly with grouping:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    private ObservableCollection<Item> _items;
    private ObservableCollection<GroupDescription> _groupDescriptions;
    
    public ObservableCollection<Item> Items { get; set; }
    
    public ObservableCollection<GroupDescription> GroupDescriptions
    {
        get => _groupDescriptions;
        set
        {
            _groupDescriptions = value;
            OnPropertyChanged(nameof(GroupDescriptions));
        }
    }
    
    public ViewModel()
    {
        GroupDescriptions = new ObservableCollection<GroupDescription>();
        Items = new ObservableCollection<Item>();
        
        // Add 1000 items across 10 groups
        for (int i = 0; i < 1000; i++)
        {
            for (int j = 0; j < 10; j++)
            {
                Items.Add(new Item
                {
                    Name = $"Module {i}",
                    GroupName = $"Group{j}"
                });
            }
        }
    }
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML:**

```xml
<Window.Resources>
    <local:ViewModel x:Key="viewModel">
        <local:ViewModel.GroupDescriptions>
            <PropertyGroupDescription PropertyName="GroupName" />
        </local:ViewModel.GroupDescriptions>
    </local:ViewModel>
</Window.Resources>

<Grid>
    <syncfusion:CheckListBox ItemsSource="{Binding Items}"
                             GroupDescriptions="{Binding GroupDescriptions}"
                             DataContext="{StaticResource viewModel}"
                             DisplayMemberPath="Name"
                             Margin="20" />
</Grid>
```

**Performance:** Groups are also virtualized - only visible groups are rendered.

## Disabling Virtualization

In certain scenarios, you need to disable virtualization. This loads ALL items into memory.

### When to Disable Virtualization

**Disable virtualization when:**
1. **Using IsChecked property binding** - Virtualized items can't sync checked state
2. **Need to programmatically check off-screen items** - Items not loaded can't be checked
3. **Custom item templates with stateful UI** - State lost when items recycle
4. **Small datasets** (< 100 items) - Virtualization overhead not worth it

**Keep virtualization enabled when:**
- Large datasets (> 500 items)
- Only checking visible items
- Using SelectedItems collection (works with virtualization)
- Performance is critical

### How to Disable Virtualization

Replace `VirtualizingStackPanel` with `StackPanel`:

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Items}"
                         DisplayMemberPath="Name">
    <syncfusion:CheckListBox.ItemsPanel>
        <ItemsPanelTemplate>
            <StackPanel />
        </ItemsPanelTemplate>
    </syncfusion:CheckListBox.ItemsPanel>
</syncfusion:CheckListBox>
```

**In code:**

```csharp
CheckListBox checkListBox = new CheckListBox();

// Create StackPanel instead of VirtualizingStackPanel
var factory = new FrameworkElementFactory(typeof(StackPanel));
checkListBox.ItemsPanel = new ItemsPanelTemplate(factory);
```

**⚠️ Performance Impact:**
- 100 items: Negligible impact
- 1,000 items: ~1-2 second load time, ~10 MB memory
- 10,000 items: ~10-30 second load time, ~100 MB memory
- 100,000 items: May freeze UI, ~1 GB memory

### Example: Disabling for IsChecked Binding

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Items}"
                         DisplayMemberPath="Name">
    
    <!-- Bind IsChecked property to model -->
    <syncfusion:CheckListBox.Resources>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="IsChecked" Value="{Binding IsChecked}" />
        </Style>
    </syncfusion:CheckListBox.Resources>
    
    <!-- MUST disable virtualization for IsChecked binding -->
    <syncfusion:CheckListBox.ItemsPanel>
        <ItemsPanelTemplate>
            <StackPanel />
        </ItemsPanelTemplate>
    </syncfusion:CheckListBox.ItemsPanel>
</syncfusion:CheckListBox>
```

**Why?** When virtualization is enabled:
- Off-screen items aren't rendered
- IsChecked binding doesn't execute for non-rendered items
- SelectedItems collection becomes out of sync with model
- Checking items programmatically fails for virtualized items

## When to Use Virtualization

### Decision Matrix

| Scenario | Virtualization | Reason |
|----------|---------------|---------|
| 10-100 items | Either | Performance difference minimal |
| 100-500 items | Recommended | Noticeable performance improvement |
| 500+ items | **Required** | Critical for performance |
| Using SelectedItems collection | ✅ Enabled | Works perfectly |
| Using IsChecked binding | ❌ Disabled | Doesn't work with virtualization |
| Grouping with 1000+ items | ✅ Enabled | Groups also virtualized |
| Checking off-screen items | ❌ Disabled | Items must be loaded |
| Memory-constrained devices | ✅ Enabled | Reduces memory usage 90%+ |

### Real-World Examples

**Example 1: Employee Directory (5,000 employees)**

```xml
<!-- Keep virtualization enabled -->
<syncfusion:CheckListBox ItemsSource="{Binding Employees}"
                         DisplayMemberPath="Name"
                         SelectedItems="{Binding SelectedEmployees}">
    <!-- Default VirtualizingStackPanel is used -->
</syncfusion:CheckListBox>
```

**Result:** Loads instantly, smooth scrolling, low memory.

**Example 2: Settings Panel (50 options)**

```xml
<!-- Disable virtualization for IsChecked binding -->
<syncfusion:CheckListBox ItemsSource="{Binding Settings}">
    <syncfusion:CheckListBox.Resources>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="IsChecked" Value="{Binding IsEnabled, Mode=TwoWay}" />
        </Style>
    </syncfusion:CheckListBox.Resources>
    
    <syncfusion:CheckListBox.ItemsPanel>
        <ItemsPanelTemplate>
            <StackPanel />
        </ItemsPanelTemplate>
    </syncfusion:CheckListBox.ItemsPanel>
</syncfusion:CheckListBox>
```

**Result:** All settings loaded, IsChecked binding works correctly.

## Performance Best Practices

### 1. Use ObservableCollection

```csharp
// ✅ GOOD - Change notifications work
public ObservableCollection<Item> Items { get; set; }

// ❌ BAD - UI won't update when collection changes
public List<Item> Items { get; set; }
```

### 2. Implement INotifyPropertyChanged

```csharp
public class Item : INotifyPropertyChanged
{
    private bool _isChecked;
    
    public bool IsChecked
    {
        get => _isChecked;
        set
        {
            _isChecked = value;
            OnPropertyChanged(nameof(IsChecked));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### 3. Avoid Complex ItemTemplates

**Slow template:**

```xml
<DataTemplate>
    <Border Background="White" BorderBrush="Gray" BorderThickness="1">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition />
                <RowDefinition />
            </Grid.RowDefinitions>
            <Image Source="{Binding Image}" Width="50" Height="50" />
            <StackPanel Grid.Row="1">
                <TextBlock Text="{Binding Name}" FontWeight="Bold" />
                <TextBlock Text="{Binding Description}" />
            </StackPanel>
        </Grid>
    </Border>
</DataTemplate>
```

**Faster template:**

```xml
<DataTemplate>
    <TextBlock Text="{Binding Name}" />
</DataTemplate>
```

### 4. Use Virtualization-Compatible Checking

```csharp
// ✅ GOOD - Works with virtualization
checkListBox.SelectedItems.Add(item);

// ❌ BAD - Requires disabling virtualization
item.IsChecked = true;  // (via IsChecked property binding)
```

### 5. Defer Loading if Possible

```csharp
// Load data asynchronously
public async Task LoadDataAsync()
{
    await Task.Run(() =>
    {
        var items = Database.GetItems();  // Heavy operation
        
        Application.Current.Dispatcher.Invoke(() =>
        {
            Items = new ObservableCollection<Item>(items);
        });
    });
}
```

### 6. Monitor Memory Usage

Use Visual Studio Diagnostic Tools to verify virtualization is working:

1. Open Diagnostic Tools (Debug → Performance Profiler)
2. Select "Memory Usage"
3. Load 10,000 items
4. Take memory snapshot
5. With virtualization: ~5-20 MB
6. Without virtualization: ~100-500 MB

## Troubleshooting

**Issue: Checked items reset when scrolling**
- Virtualization is enabled but using IsChecked binding
- Solution: Disable virtualization or use SelectedItems collection

**Issue: Can't check items programmatically**
- Trying to check virtualized (off-screen) items
- Solution: Disable virtualization or ensure items are visible first

**Issue: Slow performance with large dataset**
- Virtualization might be disabled
- Solution: Remove custom ItemsPanel or verify VirtualizingStackPanel is used

**Issue: SelectedItems out of sync**
- Using IsChecked binding with virtualization enabled
- Solution: Disable virtualization

**Issue: High memory usage**
- Virtualization disabled unintentionally
- Solution: Remove StackPanel from ItemsPanel

## Next Steps

- **Grouping with Large Datasets:** [grouping-and-sorting.md](grouping-and-sorting.md)
- **Data Binding Strategies:** [data-binding.md](data-binding.md)
- **Selection Management:** [checking-and-selection.md](checking-and-selection.md)

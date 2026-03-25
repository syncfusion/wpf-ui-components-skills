# Item Checking and Selection

## Table of Contents
- [Overview](#overview)
- [Checking Items Programmatically](#checking-items-programmatically)
- [User Interaction Methods](#user-interaction-methods)
- [SelectAll Functionality](#selectall-functionality)
- [Event Notifications](#event-notifications)
- [Getting Checked Items](#getting-checked-items)
- [Advanced Scenarios](#advanced-scenarios)

## Overview

The CheckListBox provides multiple ways to check and uncheck items, giving you complete control over selection behavior whether through user interaction or programmatic manipulation.

**Available Methods:**
1. **Collection-based** - Add/remove items from SelectedItems
2. **Property-based** - Bind IsChecked property to model
3. **Mouse interaction** - Click checkbox or content
4. **Keyboard interaction** - Space bar to toggle

**Key Properties:**
- `SelectedItems` - Collection of all checked items
- `SelectedItem` - Currently focused item (checked or not)
- `IsCheckOnFirstClick` - Single vs double-click behavior
- `IsSelectAllEnabled` - Show/hide SelectAll option

## Checking Items Programmatically

### Method 1: Using SelectedItems Collection (Recommended)

The most straightforward and reliable way to check items programmatically is through the `SelectedItems` collection.

**Adding items to check them:**

```csharp
// Check specific items by adding to SelectedItems
checkListBox.SelectedItems.Add(item1);
checkListBox.SelectedItems.Add(item2);
checkListBox.SelectedItems.Add(item3);
```

**Removing items to uncheck them:**

```csharp
// Uncheck items by removing from SelectedItems
checkListBox.SelectedItems.Remove(item1);
```

**Complete example with data binding:**

```csharp
// Model
public class Vegetable
{
    public string Name { get; set; }
    public string Category { get; set; }
    public int Price { get; set; }
}

// ViewModel
public class ViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    private ObservableCollection<Vegetable> _vegetables;
    private ObservableCollection<object> _checkedVegetables;
    
    public ObservableCollection<Vegetable> Vegetables
    {
        get => _vegetables;
        set
        {
            _vegetables = value;
            RaisePropertyChanged(nameof(Vegetables));
        }
    }
    
    public ObservableCollection<object> CheckedVegetables
    {
        get => _checkedVegetables;
        set
        {
            _checkedVegetables = value;
            RaisePropertyChanged(nameof(CheckedVegetables));
        }
    }
    
    public ViewModel()
    {
        // Populate vegetables
        Vegetables = new ObservableCollection<Vegetable>
        {
            new Vegetable { Name = "Yarrow", Category = "Leafy and Salad", Price = 10 },
            new Vegetable { Name = "Cabbage", Category = "Leafy and Salad", Price = 20 },
            new Vegetable { Name = "Horse gram", Category = "Beans", Price = 30 },
            new Vegetable { Name = "Green bean", Category = "Beans", Price = 20 },
            new Vegetable { Name = "Onion", Category = "Bulb and Stem", Price = 10 },
            new Vegetable { Name = "Nopal", Category = "Bulb and Stem", Price = 30 }
        };
        
        // Pre-check specific items (indices 0, 2, 4)
        CheckedVegetables = new ObservableCollection<object>
        {
            Vegetables[0],  // Yarrow
            Vegetables[2],  // Horse gram
            Vegetables[4]   // Onion
        };
    }
    
    protected void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML binding:**

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Vegetables}"
                         SelectedItems="{Binding CheckedVegetables}"
                         DisplayMemberPath="Name"
                         Margin="30">
    <syncfusion:CheckListBox.DataContext>
        <local:ViewModel />
    </syncfusion:CheckListBox.DataContext>
</syncfusion:CheckListBox>
```

**Why this method is preferred:**
- Works reliably with data binding
- Automatically updates UI
- Supports virtualization
- Integrates cleanly with MVVM

### Method 2: Using IsChecked Property Binding

Bind the `CheckListBoxItem.IsChecked` property to a boolean property in your model.

**⚠️ Important Limitation:** This method does NOT work with virtualization. The `SelectedItems` collection won't stay synchronized because virtualized items aren't loaded. Disable virtualization if you use this approach.

```csharp
// Model with IsChecked property
public class GroupItem
{
    public string Name { get; set; }
    public string GroupName { get; set; }
    public bool IsChecked { get; set; }
}

// ViewModel
public class ViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    private ObservableCollection<GroupItem> _items;
    
    public ObservableCollection<GroupItem> Items
    {
        get => _items;
        set
        {
            _items = value;
            RaisePropertyChanged(nameof(Items));
        }
    }
    
    public ViewModel()
    {
        Items = new ObservableCollection<GroupItem>();
        
        for (int i = 0; i < 100; i++)
        {
            Items.Add(new GroupItem
            {
                Name = "Item " + i,
                GroupName = "Group " + (i % 10),
                IsChecked = i % 2 == 0  // Check even-indexed items
            });
        }
    }
    
    protected void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**XAML with IsChecked binding:**

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Items}"
                         DisplayMemberPath="Name"
                         Margin="20">
    <syncfusion:CheckListBox.DataContext>
        <local:ViewModel />
    </syncfusion:CheckListBox.DataContext>
    
    <!-- Bind IsChecked property -->
    <syncfusion:CheckListBox.Resources>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="IsChecked" Value="{Binding IsChecked}" />
        </Style>
    </syncfusion:CheckListBox.Resources>
    
    <!-- CRITICAL: Disable virtualization for IsChecked binding to work -->
    <syncfusion:CheckListBox.ItemsPanel>
        <ItemsPanelTemplate>
            <StackPanel />
        </ItemsPanelTemplate>
    </syncfusion:CheckListBox.ItemsPanel>
</syncfusion:CheckListBox>
```

**When to use:**
- Small datasets (< 100 items)
- Virtualization is not needed
- You need direct binding to model property

**When NOT to use:**
- Large datasets requiring virtualization
- Performance is critical
- Use Method 1 (SelectedItems) instead

### Checking Items on Initialization

To pre-check items when the control loads, add them to `SelectedItems` immediately after populating the control.

**In code-behind:**

```csharp
// After populating items
checkListBox.SelectedItems.Add(checkListBox.Items[0]);
checkListBox.SelectedItems.Add(checkListBox.Items[4]);
checkListBox.SelectedItems.Add(checkListBox.Items[7]);
```

**With static items:**

```xml
<syncfusion:CheckListBox>
    <syncfusion:CheckListBoxItem Content="Austria" />
    <syncfusion:CheckListBoxItem Content="Australia" IsSelected="True" />
    <syncfusion:CheckListBoxItem Content="Canada" IsSelected="True" />
    <syncfusion:CheckListBoxItem Content="Finland" />
</syncfusion:CheckListBox>
```

**In ViewModel (MVVM):**

```csharp
public ViewModel()
{
    LoadItems();
    
    // Pre-select items after loading
    CheckedItems.Add(Items[2]);
    CheckedItems.Add(Items[5]);
    CheckedItems.Add(Items[8]);
}
```

## User Interaction Methods

### Mouse-Based Checking

Users can check/uncheck items by clicking on:
- The checkbox itself
- The item content (text/template)
- Group headers (checks/unchecks all children)
- SelectAll item (checks/unchecks all items)

#### Single-Click vs Double-Click

Control the click behavior with the `IsCheckOnFirstClick` property.

**Single-click (default):**

```xml
<syncfusion:CheckListBox IsCheckOnFirstClick="True"
                         ItemsSource="{Binding Days}" />
```

```csharp
checkListBox.IsCheckOnFirstClick = true; // Default
```

**Double-click required:**

```xml
<syncfusion:CheckListBox IsCheckOnFirstClick="False"
                         ItemsSource="{Binding Days}" />
```

```csharp
checkListBox.IsCheckOnFirstClick = false;
```

**When to use double-click:**
- Prevent accidental checking
- Application with lots of click interactions
- Need confirmation before state change

**Complete example:**

```csharp
// ViewModel
public class ViewModel
{
    public ObservableCollection<string> Days { get; set; }
    
    public ViewModel()
    {
        Days = new ObservableCollection<string>
        {
            "Sunday", "Monday", "Tuesday", "Wednesday",
            "Thursday", "Friday", "Saturday"
        };
    }
}
```

```xml
<syncfusion:CheckListBox IsCheckOnFirstClick="False"
                         ItemsSource="{Binding Days}">
    <syncfusion:CheckListBox.DataContext>
        <local:ViewModel />
    </syncfusion:CheckListBox.DataContext>
</syncfusion:CheckListBox>
```

### Keyboard-Based Checking

The **Space bar** toggles the checked state of the currently focused (selected) item.

**Behavior:**
- Press Space once: Check/uncheck (if `IsCheckOnFirstClick="True"`)
- Press Space twice: Check/uncheck (if `IsCheckOnFirstClick="False"`)

**Navigation keys:**
- Arrow Up/Down: Move focus between items
- Space: Toggle focused item's checked state
- Tab: Move focus out of control

**Keyboard + Group Headers:**
- Focus on group header + Space = Check/uncheck all children
- Focus on SelectAll + Space = Check/uncheck all items

## SelectAll Functionality

The SelectAll feature provides a single checkbox to check or uncheck all items at once.

### Enabling SelectAll

```xml
<syncfusion:CheckListBox IsSelectAllEnabled="True"
                         ItemsSource="{Binding Days}" />
```

```csharp
checkListBox.IsSelectAllEnabled = true;
```

### SelectAll States

The SelectAll checkbox has three states:

1. **Unchecked** ☐ - No items are checked
2. **Intermediate** ☑ - Some (but not all) items are checked
3. **Checked** ☑ - All items are checked

**State transitions:**
- Click when Unchecked → All items checked
- Click when Checked → All items unchecked
- Click when Intermediate → All items checked

### Complete SelectAll Example

```csharp
public class ViewModel
{
    public ObservableCollection<string> Days { get; set; }
    
    public ViewModel()
    {
        Days = new ObservableCollection<string>
        {
            "Sunday", "Monday", "Tuesday", "Wednesday",
            "Thursday", "Friday", "Saturday"
        };
    }
}
```

```xml
<Window x:Class="SelectAllDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:SelectAllDemo">
    
    <Grid Margin="20">
        <syncfusion:CheckListBox IsSelectAllEnabled="True"
                                 ItemsSource="{Binding Days}"
                                 Width="200"
                                 Height="300">
            <syncfusion:CheckListBox.DataContext>
                <local:ViewModel />
            </syncfusion:CheckListBox.DataContext>
        </syncfusion:CheckListBox>
    </Grid>
</Window>
```

### Disabling SelectAll

```xml
<syncfusion:CheckListBox IsSelectAllEnabled="False"
                         ItemsSource="{Binding Days}" />
```

**When to disable:**
- UI space is limited
- Selection logic is complex
- Custom "select all" button exists elsewhere

## Event Notifications

### ItemChecked Event

Fires whenever an item's checked state changes (checked → unchecked or unchecked → checked).

**Event signature:**

```csharp
public event EventHandler<ItemCheckedEventArgs> ItemChecked;
```

**ItemCheckedEventArgs properties:**
- `Item` - The item that was checked/unchecked (object)
- `IsChecked` - New checked state (bool)

**XAML:**

```xml
<syncfusion:CheckListBox ItemChecked="CheckListBox_ItemChecked" />
```

**Event handler:**

```csharp
private void CheckListBox_ItemChecked(object sender, ItemCheckedEventArgs e)
{
    // Cast to appropriate type
    var item = e.Item as CheckListBoxItem;
    
    // Or for data-bound items
    var vegetable = e.Item as Vegetable;
    
    // Get the new state
    bool isChecked = e.IsChecked;
    
    // Perform actions
    if (isChecked)
    {
        Console.WriteLine($"{vegetable.Name} was checked");
    }
    else
    {
        Console.WriteLine($"{vegetable.Name} was unchecked");
    }
}
```

**Subscribe in code:**

```csharp
checkListBox.ItemChecked += (sender, e) =>
{
    var item = e.Item;
    MessageBox.Show($"Item {item} is now {(e.IsChecked ? "checked" : "unchecked")}");
};
```

**Use cases:**
- Update summary counts
- Validate selection rules
- Trigger dependent actions
- Log user selections
- Update related UI elements

### SelectionChanged Event

Fires when the focused item changes (not necessarily when checked state changes).

**Event signature:**

```csharp
public event SelectionChangedEventHandler SelectionChanged;
```

**SelectionChangedEventArgs properties:**
- `AddedItems` - Items added to selection
- `RemovedItems` - Items removed from selection

**XAML:**

```xml
<syncfusion:CheckListBox SelectionChanged="CheckListBox_SelectionChanged" />
```

**Event handler:**

```csharp
private void CheckListBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Items added to selection
    foreach (var item in e.AddedItems)
    {
        Console.WriteLine($"Added: {item}");
    }
    
    // Items removed from selection
    foreach (var item in e.RemovedItems)
    {
        Console.WriteLine($"Removed: {item}");
    }
}
```

**⚠️ Important Distinction:**
- `ItemChecked` = Checkbox state changed (checked/unchecked)
- `SelectionChanged` = Focused item changed (focus moved)

## Getting Checked Items

### SelectedItems Property

Returns a collection of all currently checked items.

**Basic usage:**

```csharp
// Get all checked items
var checkedItems = checkListBox.SelectedItems;

// Count checked items
int count = checkedItems.Count;

// Iterate through checked items
foreach (var item in checkedItems)
{
    var checkListItem = item as CheckListBoxItem;
    Console.WriteLine(checkListItem.Content);
}
```

**With data binding:**

```csharp
// Assuming ItemsSource is ObservableCollection<Vegetable>
foreach (var item in checkListBox.SelectedItems)
{
    var vegetable = item as Vegetable;
    Console.WriteLine($"{vegetable.Name} - {vegetable.Category}");
}
```

**Display checked items in UI:**

```csharp
private void ShowCheckedItems_Click(object sender, RoutedEventArgs e)
{
    var sb = new StringBuilder();
    sb.AppendLine($"Total Checked: {checkListBox.SelectedItems.Count}");
    sb.AppendLine();
    
    foreach (var item in checkListBox.SelectedItems)
    {
        var vegetable = item as Vegetable;
        sb.AppendLine($"✓ {vegetable.Name} ({vegetable.Category})");
    }
    
    MessageBox.Show(sb.ToString(), "Checked Items");
}
```

### SelectedItem Property

Returns the currently focused item (whether checked or not).

```csharp
// Get focused item
var currentItem = checkListBox.SelectedItem;

if (currentItem != null)
{
    var vegetable = currentItem as Vegetable;
    MessageBox.Show($"Currently focused: {vegetable.Name}");
}
```

**Key difference:**
- `SelectedItems` (plural) = All checked items
- `SelectedItem` (singular) = Currently focused item

## Advanced Scenarios

### Programmatic Check/Uncheck All

**Check all items:**

```csharp
public void CheckAll()
{
    foreach (var item in checkListBox.Items)
    {
        if (!checkListBox.SelectedItems.Contains(item))
        {
            checkListBox.SelectedItems.Add(item);
        }
    }
}
```

**Uncheck all items:**

```csharp
public void UncheckAll()
{
    checkListBox.SelectedItems.Clear();
}
```

**Toggle all items:**

```csharp
public void ToggleAll()
{
    if (checkListBox.SelectedItems.Count == checkListBox.Items.Count)
    {
        // All checked, so uncheck all
        checkListBox.SelectedItems.Clear();
    }
    else
    {
        // Some unchecked, so check all
        CheckAll();
    }
}
```

### Conditional Selection

**Check items based on criteria:**

```csharp
public void CheckItemsAbovePrice(int minPrice)
{
    foreach (Vegetable vegetable in checkListBox.Items)
    {
        if (vegetable.Price > minPrice)
        {
            if (!checkListBox.SelectedItems.Contains(vegetable))
            {
                checkListBox.SelectedItems.Add(vegetable);
            }
        }
    }
}
```

**Check items by category:**

```csharp
public void CheckByCategory(string category)
{
    foreach (Vegetable vegetable in checkListBox.Items)
    {
        if (vegetable.Category == category)
        {
            checkListBox.SelectedItems.Add(vegetable);
        }
    }
}
```

### Validation Rules

**Limit maximum selections:**

```csharp
private void CheckListBox_ItemChecked(object sender, ItemCheckedEventArgs e)
{
    const int maxSelections = 3;
    
    if (e.IsChecked && checkListBox.SelectedItems.Count > maxSelections)
    {
        // Remove the item that was just checked
        checkListBox.SelectedItems.Remove(e.Item);
        
        MessageBox.Show($"Maximum {maxSelections} items can be selected.",
                        "Selection Limit", MessageBoxButton.OK, MessageBoxImage.Warning);
    }
}
```

**Require minimum selections:**

```csharp
private void SaveButton_Click(object sender, RoutedEventArgs e)
{
    const int minSelections = 2;
    
    if (checkListBox.SelectedItems.Count < minSelections)
    {
        MessageBox.Show($"Please select at least {minSelections} items.",
                        "Validation", MessageBoxButton.OK, MessageBoxImage.Warning);
        return;
    }
    
    // Proceed with save
    SaveSelections();
}
```

### Export Checked Items

```csharp
public List<T> GetCheckedItems<T>()
{
    return checkListBox.SelectedItems.Cast<T>().ToList();
}

// Usage
var checkedVegetables = GetCheckedItems<Vegetable>();
```

## Troubleshooting

**Issue: SelectedItems not updating**
- Ensure items are added/removed from collection correctly
- Check if virtualization is enabled with IsChecked binding
- Verify binding mode is TwoWay if using ItemContainerStyle

**Issue: ItemChecked event not firing**
- Verify event handler is attached
- Check if IsCheckOnFirstClick conflicts with expected behavior
- Ensure items are being checked (not just focused)

**Issue: SelectAll not appearing**
- Set `IsSelectAllEnabled="True"`
- Check if control has enough space
- Verify template hasn't been customized to hide it

**Issue: Can't check items programmatically**
- Items must exist in Items collection before adding to SelectedItems
- Use correct item reference (object from ItemsSource, not new instance)
- Check for binding errors in Output window

## Next Steps

- **Organize Items:** [grouping-and-sorting.md](grouping-and-sorting.md)
- **Customize Look:** [appearance-customization.md](appearance-customization.md)
- **Optimize Performance:** [virtualization.md](virtualization.md)

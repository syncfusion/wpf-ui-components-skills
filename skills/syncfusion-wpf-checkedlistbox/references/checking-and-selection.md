# Item Checking and Selection (summary)

This file covers the essential patterns for checking and selecting items in the CheckListBox. Extended examples and advanced scenarios have been moved to a companion file to reduce length.

Core recommendations:
- For reliability and virtualization compatibility, use the `SelectedItems` collection to programmatically check/uncheck items.
- Use `ItemContainerStyle` binding `IsSelected` ↔ model boolean for small datasets or when virtualization is disabled.
- Control click behavior with `IsCheckOnFirstClick` and enable `IsSelectAllEnabled` to show a global select-all checkbox.

Common APIs:
- `SelectedItems` — all currently checked items
- `SelectedItem` — focused item
- `ItemChecked` event — fires on check state changes

For advanced patterns, pre-checking, validation, and extended examples see: [checking-and-selection-advanced.md](checking-and-selection-advanced.md)

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

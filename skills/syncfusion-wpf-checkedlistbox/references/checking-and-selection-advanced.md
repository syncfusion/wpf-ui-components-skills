````markdown
# Checking and Selection — Advanced Examples

This file contains the extended examples and advanced scenarios moved from the main selection reference.

## Programmatic Selection (SelectedItems)

```csharp
// Check specific items by adding to SelectedItems
checkListBox.SelectedItems.Add(item1);
checkListBox.SelectedItems.Add(item2);

// Uncheck items by removing from SelectedItems
checkListBox.SelectedItems.Remove(item1);
```

## SelectedItems binding with MVVM

```csharp
public ObservableCollection<object> CheckedVegetables { get; set; }

// In ViewModel constructor
CheckedVegetables = new ObservableCollection<object>();
CheckedVegetables.Add(Vegetables[0]);
```

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Vegetables}" SelectedItems="{Binding CheckedVegetables}" DisplayMemberPath="Name" />
```

## IsChecked binding (disable virtualization)

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Items}">
  <syncfusion:CheckListBox.Resources>
    <Style TargetType="syncfusion:CheckListBoxItem">
      <Setter Property="IsChecked" Value="{Binding IsChecked}" />
    </Style>
  </syncfusion:CheckListBox.Resources>
  <syncfusion:CheckListBox.ItemsPanel>
    <ItemsPanelTemplate>
      <StackPanel />
    </ItemsPanelTemplate>
  </syncfusion:CheckListBox.ItemsPanel>
</syncfusion:CheckListBox>
```

## Validation Example: Limit maximum selections

```csharp
private void CheckListBox_ItemChecked(object sender, ItemCheckedEventArgs e)
{
    const int maxSelections = 3;
    if (e.IsChecked && checkListBox.SelectedItems.Count > maxSelections)
    {
        checkListBox.SelectedItems.Remove(e.Item);
        MessageBox.Show($"Maximum {maxSelections} items can be selected.");
    }
}
```

## SelectAll behavior notes

- The SelectAll checkbox is tri-state (Unchecked / Intermediate / Checked). Clicking toggles between checked and unchecked; intermediate is shown when some children are selected.

## Troubleshooting

- If SelectedItems doesn't update with data-bound collections, ensure you're binding to the actual item instances (not new instances) and using `ObservableCollection`.
- For virtualization-related synching problems, prefer `SelectedItems` or disable virtualization when necessary.

````

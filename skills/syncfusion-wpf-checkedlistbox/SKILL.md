---
name: syncfusion-wpf-checkedlistbox
description: Implements the Syncfusion WPF CheckedListBox (CheckListBox) control for multi-select lists with checkboxes. Use this when adding checklist controls, checkbox-enabled item lists, or SelectAll functionality in WPF applications. Covers data binding, appearance customization, grouping/sorting, item selection handling, and virtualization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF CheckedListBox

Guide for implementing the Syncfusion® WPF CheckedListBox (CheckListBox) control - a powerful list control that displays items with checkboxes for multi-item selection. This skill helps you integrate, configure, and customize the CheckListBox control in WPF applications with proper data binding, styling, and performance optimization.

## When to Use This Skill

Use this skill when you need to:
- **Implement multi-select lists** with checkbox controls in WPF
- **Display checkable items** with custom content and templates
- **Bind data collections** to CheckListBox with MVVM pattern
- **Handle item checking/unchecking** events and selection states
- **Group and sort items** within the list
- **Customize appearance** including themes, colors, and checkbox placement
- **Optimize performance** with virtualization for large datasets
- **Implement SelectAll/UnselectAll** functionality
- **Configure layout options** like orientation and wrapping
- **Troubleshoot** CheckListBox-related issues

## Component Overview

The CheckListBox control implements a classic list box with checkbox items, enabling multiple selection through checkboxes. Each item can be checked or unchecked independently, and the control supports rich customization, data binding, grouping, sorting, and virtualization for high-performance scenarios.

**Key Features:**
- Single-click or double-click item selection
- Checkbox alignment (left or right)
- Full data binding and MVVM support
- Grouping with CollectionView
- Sorting capabilities
- SelectAll/UnselectAll functionality
- Built-in themes and visual styles
- Virtualization for large item collections
- Custom templates and styling
- Event notifications for checked state changes

**Assemblies Required:**
- `Syncfusion.Shared.WPF`
- `Syncfusion.Tools.WPF`

**Namespace:** `Syncfusion.Windows.Tools.Controls`

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When to read: Starting a new CheckListBox implementation, setting up assemblies, or creating basic lists.

Topics covered:
- Assembly deployment and NuGet packages
- Creating WPF project with CheckListBox
- Adding control via designer and XAML
- Adding control manually in C#
- Populating items using CheckListBoxItem
- Populating items via data binding with ItemsSource
- DisplayMemberPath configuration
- Basic examples and minimal setup

### Data Binding

📄 **Read:** [references/data-binding.md](references/data-binding.md)

When to read: Implementing MVVM pattern, binding collections, or managing checked state through data binding.

Topics covered:
- ItemsSource property configuration
- DisplayMemberPath for display values
- IsSelected property binding to model
- Model and ViewModel implementation
- ObservableCollection setup
- TwoWay binding for checked state
- INotifyPropertyChanged pattern
- ItemContainerStyle for binding IsSelected

### Item Checking and Selection

📄 **Read:** [references/checking-and-selection.md](references/checking-and-selection.md)

When to read: Managing item selection behavior, handling checked events, or implementing SelectAll functionality.

Topics covered:
- Checking/unchecking items programmatically
- IsCheckOnFirstClick property (single vs double-click)
- SelectedItems collection management
- SelectedItem property for current selection
- Checking items on initialization
- SelectAll and UnselectAll implementation
- ItemChecked event handling
- ItemCheckedEventArgs usage
- Keyboard navigation (Space key)
- Getting list of checked items

### Grouping and Sorting

📄 **Read:** [references/grouping-and-sorting.md](references/grouping-and-sorting.md)

When to read: Organizing items into groups, sorting items alphabetically/numerically, or implementing hierarchical views.

Topics covered:
- Grouping with CollectionView.GroupDescriptions
- PropertyGroupDescription configuration
- Group header selection state
- Expanding/collapsing groups
- Sorting with CollectionView.SortDescriptions
- SortDescription properties (PropertyName, Direction)
- Ascending and descending order
- Multiple sort criteria
- Combining grouping and sorting

### Appearance and Customization

📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

When to read: Styling the control, customizing colors/fonts, changing checkbox placement, or applying themes.

Topics covered:
- Foreground and Background properties
- MouseOverBackground for hover effects
- SelectedItemBackground styling
- CheckBox alignment (left/right side)
- CheckBoxPlacement property
- Built-in visual styles and themes
- Custom templates and styles
- Border and padding customization
- Font styling and text appearance
- ItemContainerStyle customization

### Layout Features

📄 **Read:** [references/layout-features.md](references/layout-features.md)

When to read: Configuring control layout, adjusting dimensions, or managing scrolling behavior.

Topics covered:
- Layout-related properties overview
- Orientation settings (vertical/horizontal)
- Item wrapping behavior
- Scrolling configuration
- Height and Width management
- ScrollViewer settings
- Item spacing and padding
- Container sizing

### Virtualization and Performance

📄 **Read:** [references/virtualization.md](references/virtualization.md)

When to read: Optimizing performance for large datasets, reducing memory usage, or experiencing performance issues.

Topics covered:
- VirtualizingStackPanel usage
- Enabling UI virtualization
- Performance optimization techniques
- ScrollUnit configuration
- Memory management benefits
- Best practices for large collections
- Virtualization limitations
- When to use virtualization

## Quick Start Example

### Basic CheckListBox with Static Items

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:CheckListBox x:Name="checkListBox" 
                                 Width="200" 
                                 Height="300">
            <syncfusion:CheckListBoxItem Content="Mexico" />
            <syncfusion:CheckListBoxItem Content="Canada" />
            <syncfusion:CheckListBoxItem Content="Bermuda" />
            <syncfusion:CheckListBoxItem Content="Belize" />
            <syncfusion:CheckListBoxItem Content="Panama" />
        </syncfusion:CheckListBox>
    </Grid>
</Window>
```

### CheckListBox with Data Binding

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Countries}"
                         DisplayMemberPath="Name"
                         IsCheckOnFirstClick="True">
    <syncfusion:CheckListBox.ItemContainerStyle>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="IsSelected" Value="{Binding IsChecked, Mode=TwoWay}" />
        </Style>
    </syncfusion:CheckListBox.ItemContainerStyle>
</syncfusion:CheckListBox>
```

```csharp
// ViewModel
public class ViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Country> Countries { get; set; }
    
    public ViewModel()
    {
        Countries = new ObservableCollection<Country>
        {
            new Country { Name = "Mexico", IsChecked = true },
            new Country { Name = "Canada", IsChecked = false },
            new Country { Name = "Bermuda", IsChecked = false }
        };
    }
}

// Model
public class Country : INotifyPropertyChanged
{
    private bool isChecked;
    public string Name { get; set; }
    public bool IsChecked 
    { 
        get => isChecked;
        set { isChecked = value; OnPropertyChanged(); }
    }
}
```

## Common Patterns

### Pattern 1: Handle Item Checked Events

```csharp
// XAML
<syncfusion:CheckListBox ItemChecked="CheckListBox_ItemChecked" />

// Code-behind
private void CheckListBox_ItemChecked(object sender, ItemCheckedEventArgs e)
{
    var item = e.Item as CheckListBoxItem;
    MessageBox.Show($"{item.Content} is {(e.IsChecked ? "checked" : "unchecked")}");
}
```

### Pattern 2: Get All Checked Items

```csharp
// Get checked items from SelectedItems property
var checkedItems = checkListBox.SelectedItems;

foreach (var item in checkedItems)
{
    var checkListBoxItem = item as CheckListBoxItem;
    Console.WriteLine(checkListBoxItem.Content);
}
```

### Pattern 3: Programmatically Check Items

```csharp
// Add items to SelectedItems collection
checkListBox.SelectedItems.Add(countryModel1);
checkListBox.SelectedItems.Add(countryModel2);
```

### Pattern 4: SelectAll Implementation

```csharp
// Check all items
foreach (var item in checkListBox.Items)
{
    if (!checkListBox.SelectedItems.Contains(item))
    {
        checkListBox.SelectedItems.Add(item);
    }
}
```

## Key Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `ItemsSource` | IEnumerable | Data source for items | null |
| `DisplayMemberPath` | string | Property path for display text | null |
| `SelectedItems` | IList | Collection of checked items | Empty |
| `SelectedItem` | object | Currently selected (focused) item | null |
| `IsCheckOnFirstClick` | bool | Check item on first click vs double-click | true |
| `CheckBoxPlacement` | CheckBoxPlacement | Checkbox position (Left/Right) | Left |
| `Foreground` | Brush | Text color | Black |
| `Background` | Brush | Background color | Transparent |
| `MouseOverBackground` | Brush | Hover background color | System default |
| `SelectedItemBackground` | Brush | Selected item background | System default |

## Key Events

| Event | EventArgs | Description |
|-------|-----------|-------------|
| `ItemChecked` | ItemCheckedEventArgs | Raised when item checked state changes |
| `SelectionChanged` | SelectionChangedEventArgs | Raised when selection changes |

## Common Use Cases

### Use Case 1: Multi-Select Filter List
Display filterable items with checkboxes for multi-selection in search filters, report options, or settings panels.

→ **Read:** [getting-started.md](references/getting-started.md) + [data-binding.md](references/data-binding.md)

### Use Case 2: Settings Configuration Panel
Create settings UI with multiple toggleable options, feature flags, or preference selections.

→ **Read:** [checking-and-selection.md](references/checking-and-selection.md) + [appearance-customization.md](references/appearance-customization.md)

### Use Case 3: Large Dataset with Grouping
Display thousands of checkable items organized by category with virtualization for performance.

→ **Read:** [grouping-and-sorting.md](references/grouping-and-sorting.md) + [virtualization.md](references/virtualization.md)

### Use Case 4: Permissions/Roles Manager
Implement role or permission selection UI with grouped capabilities and SelectAll per group.

→ **Read:** [grouping-and-sorting.md](references/grouping-and-sorting.md) + [checking-and-selection.md](references/checking-and-selection.md)

### Use Case 5: Styled Checklist for Branding
Apply custom themes, colors, and checkbox positioning to match application branding.

→ **Read:** [appearance-customization.md](references/appearance-customization.md)

## Troubleshooting Quick Links

| Issue | Navigate To |
|-------|-------------|
| Items not binding correctly | [data-binding.md](references/data-binding.md) |
| Checked state not updating | [checking-and-selection.md](references/checking-and-selection.md) |
| Performance issues with many items | [virtualization.md](references/virtualization.md) |
| Styling not applying | [appearance-customization.md](references/appearance-customization.md) |
| Groups not displaying | [grouping-and-sorting.md](references/grouping-and-sorting.md) |

---

**Next Steps:** Choose a reference guide above based on your implementation needs, or start with [getting-started.md](references/getting-started.md) for initial setup.

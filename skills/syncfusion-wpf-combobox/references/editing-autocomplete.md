# Editing and AutoComplete Support

## Table of Contents
- [Overview](#overview)
- [Editable Mode](#editable-mode)
- [AutoComplete Support](#autocomplete-support)
- [AutoComplete Modes](#autocomplete-modes)
- [Combining Editable with MultiSelect](#combining-editable-with-multiselect)
- [Text Search Configuration](#text-search-configuration)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

ComboBoxAdv supports editable mode where users can type custom text or filter dropdown items. AutoComplete functionality helps users find items quickly by suggesting matches as they type.

**Key Features:**
- Editable text input
- AutoComplete with suggest mode
- Dynamic item filtering
- Integration with multiselection and tokens
- Text search capabilities

## Editable Mode

Enable text editing in the combo box with the `IsEditable` property.

### Basic Editable ComboBox

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        Height="30" Width="200"/>
```

```csharp
ComboBoxAdv comboBox = new ComboBoxAdv();
comboBox.IsEditable = true;
```

**Behavior:**
- User can type in the text box
- Typed text can be selected from dropdown (if it matches)
- Allows custom input (text not in dropdown list)

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `IsEditable` | bool | Enables text editing in combo box |
| `Text` | string | Gets or sets the text content |
| `IsTextSearchEnabled` | bool | Enables text-based item search |

## AutoComplete Support

AutoComplete helps users find items quickly by filtering dropdown suggestions based on typed text.

### Why Use AutoComplete?

- **Large item lists**: Quick filtering of hundreds/thousands of items
- **User efficiency**: Reduce clicks and scrolling
- **Error reduction**: Suggest valid options as user types
- **Better UX**: Instant feedback on available options

### Requirements

- `ItemsSource` must be populated (AutoComplete only works with data-bound items)
- `IsEditable` should be `True` for best experience
- Works with both single and multiselection modes

## AutoComplete Modes

Set the `AutoCompleteMode` property to configure suggestion behavior.

### AutoCompleModes.None (Default)

No autocomplete suggestions are shown.

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="None"/>
```

**Behavior:** Standard editable combo box with no filtering.

### AutoCompleModes.Suggest

Display filtered suggestions in dropdown as user types.

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        Height="30" Width="250"/>
```

```csharp
comboBox.AutoCompleteMode = AutoCompleModes.Suggest;
```

**Behavior:**
- As user types, dropdown shows only matching items
- Matches items that **start with** or **contain** the typed text
- Case-insensitive matching
- Dropdown opens automatically when matches are found

### Complete Example with Data

```csharp
public class Country
{
    public string Name { get; set; }
    public string Code { get; set; }
}

public class ViewModel
{
    public ObservableCollection<Country> Countries { get; set; }

    public ViewModel()
    {
        Countries = new ObservableCollection<Country>
        {
            new Country { Name = "Argentina", Code = "AR" },
            new Country { Name = "Australia", Code = "AU" },
            new Country { Name = "Austria", Code = "AT" },
            new Country { Name = "Belgium", Code = "BE" },
            new Country { Name = "Brazil", Code = "BR" },
            new Country { Name = "Canada", Code = "CA" },
            new Country { Name = "China", Code = "CN" },
            new Country { Name = "Denmark", Code = "DK" },
            new Country { Name = "Egypt", Code = "EG" },
            new Country { Name = "France", Code = "FR" },
            new Country { Name = "Germany", Code = "DE" },
            new Country { Name = "India", Code = "IN" },
            new Country { Name = "Japan", Code = "JP" },
            new Country { Name = "Mexico", Code = "MX" },
            new Country { Name = "Netherlands", Code = "NL" },
            new Country { Name = "New Zealand", Code = "NZ" },
            new Country { Name = "Norway", Code = "NO" },
            new Country { Name = "Russia", Code = "RU" },
            new Country { Name = "South Africa", Code = "ZA" },
            new Country { Name = "Spain", Code = "ES" },
            new Country { Name = "Sweden", Code = "SE" },
            new Country { Name = "Switzerland", Code = "CH" },
            new Country { Name = "United Kingdom", Code = "GB" },
            new Country { Name = "United States", Code = "US" }
        };
    }
}
```

```xml
<Window.DataContext>
    <local:ViewModel />
</Window.DataContext>

<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        DefaultText="Type to search countries..."
                        Height="30" Width="250"/>
```

**User Experience:**
1. User types "au"
2. Dropdown shows: Argentina, Australia, Austria
3. User types "aus"
4. Dropdown shows: Australia, Austria
5. User selects from filtered list

## Combining Editable with MultiSelect

Editable mode works seamlessly with multiselection.

### Without Tokens

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        AllowMultiSelect="True"
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"/>
```

**Behavior:**
- Type to filter items
- Select multiple items with checkboxes
- Selected items displayed with delimiter (default: comma)

### With Tokens (Recommended)

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        AllowMultiSelect="True"
                        EnableToken="True"
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"
                        Height="Auto" Width="300"/>
```

**Behavior:**
- Type to filter and select items
- Selected items appear as tokens (chips)
- Type additional text to add more items
- Remove items by clicking token close icon
- Autocomplete filters as you type

**User Flow:**
1. User types "ca"
2. Dropdown filters to: Canada, Cambodia, etc.
3. User presses Enter → "Canada" added as token
4. User types "fr"
5. Dropdown filters to: France, etc.
6. User presses Enter → "France" added as token

## Text Search Configuration

### IsTextSearchEnabled

When `AutoCompleteMode` is `Suggest` and `IsEditable` is `True`, the `IsTextSearchEnabled` property is less relevant as AutoComplete takes precedence.

However, for non-editable combo boxes with text search:

```xml
<syncfusion:ComboBoxAdv IsTextSearchEnabled="True"
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"/>
```

**Behavior:** When dropdown is open, typing a letter jumps to first matching item.

### AutoComplete vs IsTextSearchEnabled Priority

- **AutoComplete Mode = Suggest** → Filters dropdown (recommended for editable)
- **IsTextSearchEnabled = True** → Jumps to item (useful for non-editable)
- If both are set, `AutoCompleteMode` takes precedence in editable mode

## Best Practices

### 1. Always Use AutoComplete with Large Lists

For 20+ items, enable autocomplete:

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        ItemsSource="{Binding LargeItemList}"/>
```

### 2. Provide Helpful DefaultText

Guide users on what to type:

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        DefaultText="Start typing to search..."/>
```

### 3. Use Tokens for MultiSelect + Editable

Clearer visual feedback:

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        AllowMultiSelect="True"
                        EnableToken="True"
                        Height="Auto"/>
```

### 4. Ensure DisplayMemberPath Is Set

AutoComplete needs to know which property to filter:

```xml
<!-- ✅ Correct -->
<syncfusion:ComboBoxAdv AutoCompleteMode="Suggest"
                        DisplayMemberPath="Name"/>

<!-- ❌ Won't work properly -->
<syncfusion:ComboBoxAdv AutoCompleteMode="Suggest"/>
```

### 5. Consider Minimum Width for Editable Boxes

Ensure enough space for typing:

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        MinWidth="200"/>
```

## Common Patterns

### Pattern 1: Searchable Single-Select Dropdown

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        ItemsSource="{Binding Products}"
                        DisplayMemberPath="ProductName"
                        DefaultText="Search products..."
                        Height="30" Width="250"/>
```

## Troubleshooting

### Issue: AutoComplete Not Working

**Cause:** Not using data binding or `AutoCompleteMode` not set

**Solution:**
1. Ensure `ItemsSource` is bound to a collection
2. Set `AutoCompleteMode="Suggest"`
3. Set `IsEditable="True"`
4. Verify `DisplayMemberPath` is correct

```xml
<!-- All required properties -->
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"/>
```

### Issue: AutoComplete Works but No Items Match

**Cause:** Case sensitivity or property name mismatch

**Solution:**
- AutoComplete is case-insensitive by default (this should work)
- Verify `DisplayMemberPath` matches your model property exactly
- Check that your data actually contains matching items

```csharp
// ✅ Correct
DisplayMemberPath="Name"  // Property: public string Name { get; set; }

// ❌ Incorrect
DisplayMemberPath="name"  // Property is Name, not name (case matters in property names)
```

### Issue: Typed Text Not Adding to Tokens

**Cause:** Text doesn't match any dropdown item

**Solution:** Tokens only add items that exist in `ItemsSource`. If you need custom items, consider a different approach or add the item to the collection first.

**Validation logic:**
```csharp
// Only items matching ItemsSource are added as tokens
// Custom text that doesn't match is discarded on Enter/Tab
```

### Issue: Dropdown Not Opening When Typing

**Cause:** `AutoCompleteMode` is `None`

**Solution:**
```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"/>  <!-- Must be Suggest -->
```

### Issue: IsTextSearchEnabled and AutoComplete Conflict

**Cause:** Both properties set, but `AutoCompleteMode` takes precedence

**Solution:** Choose one:
- **For editable filtering:** Use `AutoCompleteMode="Suggest"`
- **For non-editable jump-to:** Use `IsTextSearchEnabled="True"` (don't set AutoComplete)

```xml
<!-- Editable with filtering -->
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"/>

<!-- Non-editable with jump-to-item -->
<syncfusion:ComboBoxAdv IsTextSearchEnabled="True"/>
```

## Performance Considerations

### Large Data Sets

AutoComplete performs well with thousands of items due to efficient filtering:

```xml
<!-- Works efficiently with 1000+ items -->
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        ItemsSource="{Binding LargeCollection}"
                        DisplayMemberPath="Name"
                        VirtualizingPanel.IsVirtualizing="True"/>
```

### Virtualization

For very large lists, enable virtualization:

```xml
<syncfusion:ComboBoxAdv VirtualizingPanel.IsVirtualizing="True"
                        VirtualizingPanel.VirtualizationMode="Recycling"/>
```

## See Also

- [Selection and MultiSelection](selection-multiselection.md) - Selection features including tokens
- [Watermark and Delimiter](watermark-delimiter.md) - Text customization
- [Getting Started](getting-started.md) - Basic setup and data binding
- [Syncfusion AutoComplete Documentation](https://help.syncfusion.com/wpf/combobox/editing)

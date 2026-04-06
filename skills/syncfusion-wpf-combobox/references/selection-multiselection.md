# Selection and MultiSelection Support

## Table of Contents
- [Overview](#overview)
- [Single Selection](#single-selection)
- [MultiSelection Support](#multiselection-support)
- [Programmatic Selection](#programmatic-selection)
- [Select All Functionality](#select-all-functionality)
- [OK and Cancel Buttons](#ok-and-cancel-buttons)
- [Token Support](#token-support)
- [Keyboard Access](#keyboard-access)
- [Selection Events](#selection-events)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

ComboBoxAdv supports both single and multiple item selection with various visualization options including checkboxes, tokens, and custom selection handling.

**Selection Features:**
- Single selection (default)
- Multiple selection with checkboxes
- Token-based multiselection display
- Programmatic selection control
- Select All functionality
- OK/Cancel confirmation buttons
- Custom selection behavior via method overrides

## Single Selection

By default, ComboBoxAdv operates in single-selection mode.

### Using SelectedItem

```xml
<syncfusion:ComboBoxAdv x:Name="comboBoxAdv"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        SelectedItem="{Binding SelectedCountry, Mode=TwoWay}"/>
```

```csharp
// Get selected item
var selected = comboBoxAdv.SelectedItem;

// Set selected item programmatically
comboBoxAdv.SelectedItem = Countries[0];
```

### Using SelectedIndex

```xml
<syncfusion:ComboBoxAdv SelectedIndex="2"/>
```

```csharp
// Get index of selected item
int index = comboBoxAdv.SelectedIndex; // 0-based

// Set selected by index
comboBoxAdv.SelectedIndex = 1;
```

### Using SelectedValue

For extracting a specific property value from the selected object:

```xml
<syncfusion:ComboBoxAdv ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        SelectedValuePath="Code"
                        SelectedValue="{Binding SelectedCountryCode}"/>
```

```csharp
// Gets the Code property value of selected item
string countryCode = comboBoxAdv.SelectedValue as string;
```

## MultiSelection Support

Enable multiple item selection with the `AllowMultiSelect` property.

### Basic MultiSelection

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"/>
```

```csharp
ComboBoxAdv comboBoxAdv = new ComboBoxAdv();
comboBoxAdv.AllowMultiSelect = true;
```

**Visual Result:** Checkboxes appear next to each item in the dropdown.

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `AllowMultiSelect` | bool | Enables multiple item selection |
| `SelectedItems` | ObservableCollection<object> | Collection of selected items |

## Programmatic Selection

Select items programmatically using the `SelectedItems` property.

### Creating a ViewModel with Selected Items

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> selectedItems;
    
    public ObservableCollection<object> SelectedItems
    {
        get { return selectedItems; }
        set
        {
            selectedItems = value;
            RaisePropertyChanged("SelectedItems");
        }
    }
    
    public ObservableCollection<Country> Countries { get; set; }

    public ViewModel()
    {
        Countries = new ObservableCollection<Country>
        {
            new Country { Name = "Denmark" },
            new Country { Name = "New Zealand" },
            new Country { Name = "Canada" },
            new Country { Name = "Russia" },
            new Country { Name = "Japan" }
        };

        // Pre-select first two items
        var items = new ObservableCollection<object>();
        for (int i = 0; i < 2; i++)
        {
            items.Add(Countries[i]);
        }
        SelectedItems = new ObservableCollection<object>(items);
    }

    public event PropertyChangedEventHandler PropertyChanged;
    
    public void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

public class Country
{
    public string Name { get; set; }
}
```

### Binding SelectedItems

```xml
<Window.DataContext>
    <local:ViewModel />
</Window.DataContext>

<syncfusion:ComboBoxAdv DisplayMemberPath="Name"
                        SelectedItems="{Binding SelectedItems, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"
                        AllowMultiSelect="True"
                        ItemsSource="{Binding Countries}"
                        Height="30" Width="200"/>
```

**Result:** Denmark and New Zealand are pre-selected.

## Select All Functionality

Add a "Select All" checkbox to the dropdown.

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        AllowSelectAll="True"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"/>
```

```csharp
comboBoxAdv.AllowSelectAll = true;
```

**Behavior:**
- "Select All" checkbox appears at the top of dropdown
- Clicking it selects/deselects all items
- Useful for bulk selection scenarios

## OK and Cancel Buttons

Add confirmation buttons to the dropdown for multiselection.

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        EnableOKCancel="True"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"/>
```

```csharp
comboBoxAdv.EnableOKCancel = true;
```

**Behavior:**
- OK button: Confirms selection and closes dropdown
- Cancel button: Reverts selection and closes dropdown
- Useful for preventing accidental selections

### Combined Example: Select All + OK/Cancel

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        AllowSelectAll="True"
                        EnableOKCancel="True"
                        DefaultText="Select continents..."
                        ItemsSource="{Binding Continents}"
                        DisplayMemberPath="Name"
                        Height="30" Width="250"/>
```

## Token Support

Display selected items as visual tokens (chips) with close icons.

### Enabling Tokens

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        IsEditable="True"
                        EnableToken="True"
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"
                        Height="Auto" Width="300"/>
```

```csharp
comboBoxAdv.AllowMultiSelect = true;
comboBoxAdv.IsEditable = true;
comboBoxAdv.EnableToken = true;
```

**Requirements:**
- `AllowMultiSelect` must be `true`
- `IsEditable` must be `true`

**Behavior:**
- Selected items appear as rounded tokens with close icons
- Click close icon to remove item
- Control height adjusts automatically based on token count
- Tokens wrap to multiple lines as needed

### Token Editing

When `EnableToken` is true and `IsEditable` is true:
- Type text in the control
- If text matches a dropdown item, it's added as a token on Enter/Tab
- Non-matching text is discarded

### Token with Custom Delimiter (Not Visual)

When tokens are enabled, `SelectedValueDelimiter` doesn't affect visual display (tokens are shown), but it's used for text representation:

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        IsEditable="True"
                        EnableToken="True"
                        SelectedValueDelimiter=" | "/>
```

## Keyboard Access

### Selection Navigation

- **Down Arrow / Up Arrow**: Navigate through dropdown items
- **Space**: Select/deselect current item (multiselect mode)
- **Enter**: Select item and close dropdown (single select) or add item (editable with tokens)
- **Tab**: Same as Enter
- **Esc**: Close dropdown without changes

### Token-Specific Keyboard

- **Backspace**: Remove the last token
- **Enter/Tab**: Validate typed text and add as token if matching

## Selection Events

### SelectionChanged Event

Fired when the selected item(s) change.

```xml
<syncfusion:ComboBoxAdv SelectionChanged="ComboBoxAdv_SelectionChanged"/>
```

```csharp
private void ComboBoxAdv_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var comboBox = sender as ComboBoxAdv;
    
    if (comboBox.AllowMultiSelect)
    {
        // Access multiple selected items
        var selectedItems = comboBox.SelectedItems;
        MessageBox.Show($"Selected {selectedItems.Count} items");
    }
    else
    {
        // Access single selected item
        var selectedItem = comboBox.SelectedItem;
        MessageBox.Show($"Selected: {selectedItem}");
    }
}
```

### Binding Command (MVVM Pattern)

```csharp
// In ViewModel
public ICommand SelectionChangedCommand { get; set; }

public ViewModel()
{
    SelectionChangedCommand = new RelayCommand<object>(OnSelectionChanged);
}

private void OnSelectionChanged(object parameter)
{
    // Handle selection change
}
```

```xml
<syncfusion:ComboBoxAdv>
    <i:Interaction.Triggers>
        <i:EventTrigger EventName="SelectionChanged">
            <i:InvokeCommandAction Command="{Binding SelectionChangedCommand}"/>
        </i:EventTrigger>
    </i:Interaction.Triggers>
</syncfusion:ComboBoxAdv>
```

## Best Practices

### 1. Use ObservableCollection for SelectedItems

```csharp
// ✅ Correct
public ObservableCollection<object> SelectedItems { get; set; }

// ❌ Incorrect
public List<object> SelectedItems { get; set; }
```

### 2. Implement INotifyPropertyChanged

For two-way binding to work properly:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> selectedItems;
    
    public ObservableCollection<object> SelectedItems
    {
        get { return selectedItems; }
        set
        {
            selectedItems = value;
            OnPropertyChanged(nameof(SelectedItems));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### 3. Use EnableOKCancel for Critical Selections

Prevent accidental selections:

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        EnableOKCancel="True"
                        DefaultText="Select options (confirm with OK)"/>
```

### 4. Consider Token UI for Space-Constrained Layouts

Tokens provide clear visual feedback and easy removal:

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        IsEditable="True"
                        EnableToken="True"
                        Height="Auto"/>  <!-- Allow height to adjust -->
```

### 5. Provide Clear DefaultText for MultiSelect

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        DefaultText="Select one or more countries..."/>
```

## Troubleshooting

### Issue: SelectedItems Not Updating ViewModel

**Cause:** Missing `Mode=TwoWay` or `UpdateSourceTrigger=PropertyChanged`

**Solution:**
```xml
<syncfusion:ComboBoxAdv SelectedItems="{Binding SelectedItems, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
```

### Issue: Tokens Not Appearing

**Cause:** Missing required properties

**Solution:** Ensure all three are set:
```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        IsEditable="True"
                        EnableToken="True"/>
```

### Issue: SelectionChanged Event Firing Multiple Times

**Cause:** Event fires for each item in multiselect

**Solution:** Debounce or use OK/Cancel buttons:
```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        EnableOKCancel="True"/>
```

### Issue: Pre-Selected Items Not Showing

**Cause:** Items added before ItemsSource is bound

**Solution:** Ensure ItemsSource is bound first, then set SelectedItems:
```csharp
comboBoxAdv.ItemsSource = Countries;
comboBoxAdv.SelectedItems = new ObservableCollection<object> { Countries[0], Countries[1] };
```

Or use proper binding with initialization in ViewModel constructor.

## See Also

- [Getting Started](getting-started.md) - Basic setup and data binding
- [Editing and AutoComplete](editing-autocomplete.md) - Editable mode features
- [Watermark and Delimiter](watermark-delimiter.md) - Text customization
- [Syncfusion MultiSelection Documentation](https://help.syncfusion.com/wpf/combobox/multiselection)

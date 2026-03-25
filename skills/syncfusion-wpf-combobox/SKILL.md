---
name: syncfusion-wpf-combobox
description: Implement Syncfusion WPF ComboBoxAdv with multiselection, editable support, autocomplete, data binding, and token features. Use this when working with dropdown selection, multiselect dropdowns, editable dropdowns, or autocomplete in WPF. Covers tokens, watermarks, delimiters, custom item templates, and dropdown behavior configuration.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF ComboBox (ComboBoxAdv)

Comprehensive guide for implementing the Syncfusion® WPF ComboBoxAdv control, a powerful dropdown component that allows users to type a value or choose options from a list with support for multiselection, editing, autocomplete, data binding, and advanced features.

## When to Use This Skill

Use this skill when you need to:
- **Implement dropdown selection** in WPF applications
- **Enable multiselection** with checkboxes or tokens
- **Add editable dropdowns** with autocomplete functionality
- **Bind data** to dropdown controls using MVVM patterns
- **Customize item display** with templates
- **Configure watermarks** (default text) for user prompts
- **Set custom delimiters** for multiselect displays
- **Integrate with Expression Blend** for design-time editing
- **Apply themes** using SfSkinManager or ThemeStudio

## Component Overview

The `ComboBoxAdv` control (`Syncfusion.Windows.Tools.Controls.ComboBoxAdv`) extends standard WPF dropdown functionality with:

**Key Features:**
- **Multiselection** with checkboxes, tokens, OK/Cancel buttons
- **Editable mode** with autocomplete (suggest mode)
- **Data binding** via ItemsSource with DisplayMemberPath
- **Token display** for selected items with close icons
- **Watermark support** (DefaultText property)
- **Custom delimiters** for multiselect display
- **Item templates** for complex UI
- **Theme support** with built-in and custom themes
- **Expression Blend integration** for design-time editing

**Namespace:** `Syncfusion.Windows.Tools.Controls`  
**Assembly:** `Syncfusion.Shared.WPF`  
**Schema:** `http://schemas.syncfusion.com/wpf`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly deployment
- Adding control via designer, XAML, and C#
- Creating data models and ViewModels
- Basic data binding patterns
- DisplayMemberPath configuration
- ItemTemplate customization
- Theme setup and application

### Selection and MultiSelection
📄 **Read:** [references/selection-multiselection.md](references/selection-multiselection.md)
- Single selection (SelectedItem, SelectedIndex, SelectedValue)
- Enabling multiselection (AllowMultiSelect)
- Programmatic selection with SelectedItems
- SelectAll functionality
- OK/Cancel buttons for multiselect
- Overriding OnItemChecked/OnItemUnchecked
- Token support (EnableToken) for visual item representation
- Token editing and keyboard access
- SelectionChanged event handling

### Editing and AutoComplete
📄 **Read:** [references/editing-autocomplete.md](references/editing-autocomplete.md)
- IsEditable property for text input
- AutoComplete modes (Suggest, None)
- Filtering dropdown items dynamically
- Editable mode with token support
- Text search and item matching

### Watermark and Delimiter
📄 **Read:** [references/watermark-delimiter.md](references/watermark-delimiter.md)
- DefaultText property for watermarks
- SelectedValueDelimiter customization
- Usage patterns and best practices

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- ItemsSource binding fundamentals
- Creating MVVM-compatible models and ViewModels
- DisplayMemberPath vs ItemTemplate
- ObservableCollection patterns
- Two-way binding with SelectedItems
- INotifyPropertyChanged implementation

### Customization and Blendability
📄 **Read:** [references/customization-blendability.md](references/customization-blendability.md)
- Expression Blend workflow
- Style editing and template customization
- Theme application with SfSkinManager
- Creating custom themes with ThemeStudio
- Control appearance customization

## Quick Start

### Basic ComboBoxAdv with Static Items

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:ComboBoxAdv Height="30" Width="200"
                                HorizontalAlignment="Center"
                                VerticalAlignment="Center">
            <syncfusion:ComboBoxItemAdv Content="Denmark" />
            <syncfusion:ComboBoxItemAdv Content="New Zealand" />
            <syncfusion:ComboBoxItemAdv Content="Canada" />
            <syncfusion:ComboBoxItemAdv Content="Russia" />
            <syncfusion:ComboBoxItemAdv Content="Japan" />
        </syncfusion:ComboBoxAdv>
    </Grid>
</Window>
```

### Data-Bound ComboBoxAdv with MVVM

```csharp
// Model
public class Country
{
    public string Name { get; set; }
    public string Code { get; set; }
}

// ViewModel
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Country> countries;
    public ObservableCollection<Country> Countries
    {
        get { return countries; }
        set { countries = value; OnPropertyChanged(nameof(Countries)); }
    }

    public ViewModel()
    {
        Countries = new ObservableCollection<Country>
        {
            new Country { Name = "Denmark", Code = "DK" },
            new Country { Name = "New Zealand", Code = "NZ" },
            new Country { Name = "Canada", Code = "CA" }
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

```xml
<Window.DataContext>
    <local:ViewModel />
</Window.DataContext>
<syncfusion:ComboBoxAdv ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        Height="30" Width="200"/>
```

## Common Patterns

### Pattern 1: Multiselection with Tokens

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        IsEditable="True"
                        EnableToken="True"
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"
                        Height="Auto" Width="300"/>
```

### Pattern 2: Editable with AutoComplete

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"
                        Height="30" Width="200"/>
```

### Pattern 3: Watermark with Delimiter

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        DefaultText="Select items..."
                        SelectedValueDelimiter=" | "
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"
                        Height="30" Width="300"/>
```

### Pattern 4: Multiselect with OK/Cancel

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        AllowSelectAll="True"
                        EnableOKCancel="True"
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"
                        Height="30" Width="250"/>
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `AllowMultiSelect` | bool | Enables multiple item selection |
| `IsEditable` | bool | Allows text input and editing |
| `EnableToken` | bool | Displays selected items as tokens (multiselect) |
| `AutoCompleteMode` | AutoCompleModes | Suggest or None for autocomplete |
| `DefaultText` | string | Watermark text when no item selected |
| `SelectedValueDelimiter` | string | Separator between selected items |
| `ItemsSource` | IEnumerable | Data collection for binding |
| `DisplayMemberPath` | string | Property path for item display |
| `SelectedItem` | object | Currently selected item |
| `SelectedItems` | ObservableCollection<object> | Collection of selected items (multiselect) |
| `SelectedIndex` | int | Index of selected item |
| `EnableOKCancel` | bool | Shows OK/Cancel buttons in dropdown |
| `AllowSelectAll` | bool | Adds "Select All" option |

## Common Use Cases

1. **Country/Region Selector** - Single selection with data binding
2. **Tag Selector** - Multiselection with tokens for visual feedback
3. **Searchable Dropdown** - Editable with autocomplete for large lists
4. **Category Filter** - Multiselection with delimiters
5. **Form Input with Prompt** - Watermark text for user guidance
6. **Bulk Selection** - Select All with OK/Cancel for confirmation

## See Also

- [Syncfusion WPF ComboBoxAdv Documentation](https://help.syncfusion.com/wpf/combobox/overview)
- [WPF ComboBoxAdv API Reference](https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Tools.Controls.ComboBoxAdv.html)

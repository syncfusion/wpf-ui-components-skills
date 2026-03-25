---
name: syncfusion-wpf-autocomplete
description: Implements the Syncfusion WPF SfTextBoxExt autocomplete features for intelligent suggestions, filtering, and tokenized multi-selection. Use when adding predictive text input, filtered suggestion lists, or multi-select search boxes.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF Autocomplete (SfTextBoxExt)

A comprehensive guide for implementing the Syncfusion WPF Autocomplete control (SfTextBoxExt) that provides intelligent text suggestions, advanced filtering, multi-selection capabilities, and extensive customization options for enhanced user input experiences.

## When to Use This Skill

Use this skill when user need to:
- **Implement autocomplete textbox** with predictive text suggestions
- **Add search functionality** with filtered dropdown suggestions
- **Enable multi-selection** with token or delimiter modes
- **Create type-ahead input fields** with intelligent filtering
- **Build searchable dropdowns** with custom data sources
- **Implement tag/token selection UI** for multiple items
- **Add suggestion boxes** to text input controls
- **Configure advanced filtering** with 18+ filter modes
- **Customize text input appearance** with watermarks and templates
- **Handle selection events** and retrieve selected values
- **Support diacritic-insensitive search** for international text
- **Highlight matching text** in suggestion lists

This skill covers the complete SfTextBoxExt control with detailed implementation guides for all features including data binding, filtering strategies, selection handling, dropdown customization, and advanced capabilities.

## Component Overview

**SfTextBoxExt** is an enhanced TextBox control that extends standard WPF TextBox functionality with autocomplete capabilities. It provides intelligent suggestions based on user input, supports multiple filtering modes, and enables both single and multi-selection scenarios.

### Key Capabilities

**Core Features:**
- **18+ Filtering Modes**: StartsWith, EndsWith, Contains, Equals, Custom, and more
- **Multiple AutoComplete Modes**: Suggest, Append, SuggestAppend
- **Multi-Selection Support**: Token and Delimiter modes
- **Rich Data Binding**: Support for simple and complex data types
- **Custom Templates**: AutoComplete items, watermark, no-results templates
- **Event-Driven**: Selection, popup, and suggestion change events

**Advanced Features:**
- **Diacritic Sensitivity**: Search across languages with accent-insensitive filtering
- **Text Highlighting**: Visual indication of matching text
- **Custom Filtering**: Implement multi-path or complex filter logic
- **Popup Configuration**: Placement, delay, height, and behavior settings
- **Performance Optimization**: Minimum prefix characters, maximum suggestions

**UI Customization:**
- **Watermark Support**: Placeholder text and custom templates
- **Visual Styling**: Font, color, background customization
- **Token Appearance**: Custom token templates with images
- **Dropdown Styling**: Background, selection color, item templates
- **Button Controls**: Optional dropdown and clear buttons

## Documentation and Navigation Guide

This skill uses a progressive disclosure pattern. Start with SKILL.md for overview and navigation, then read specific reference files based on your implementation needs.

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Setting up SfTextBoxExt for the first time, initial implementation

**What's covered:**
- Assembly deployment and NuGet package installation
- Creating WPF application with SfTextBoxExt
- Adding control via designer, XAML, and C#
- Populating AutoComplete with data sources
- Data models and ViewModel setup
- AutoComplete modes (Suggest, Append, SuggestAppend, None)
- Basic selection configuration
- Theme support and styling

### AutoComplete and Filtering

📄 **Read:** [references/autocomplete-filtering.md](references/autocomplete-filtering.md)

**When to read:** Implementing filtering logic, customizing suggestion behavior

**What's covered:**
- AutoCompleteSource property and data binding
- SearchItemPath for custom data objects
- AutoCompleteItemTemplate for visual customization
- SuggestionMode options (18+ filter modes including StartsWith, EndsWith, Contains, Equals, Custom)
- MinimumPrefixCharacters configuration
- IgnoreCase for case-insensitive filtering
- ImageMemberPath for displaying images
- MaximumSuggestionsCount to limit results
- NoResultsFoundTemplate customization
- FilterSuggestions method for manual filtering
- SuggestionsChanged event handling

### Selection and Value Retrieval

📄 **Read:** [references/selection.md](references/selection.md)

**When to read:** Implementing single or multi-selection, retrieving selected values

**What's covered:**
- MultiSelectMode options (None, Token, Delimiter)
- SelectedItem property for single selection
- SelectedItems collection for multi-selection
- Token representation and layout
- TokensWrapMode (Wrap, None) configuration
- EnableAutoSize for dynamic height
- Delimiter mode with custom separators
- Delimiter property configuration
- SelectedValue and ValueMemberPath
- SuggestionIndex for selected item index
- ImageMemberPath for token images
- SelectedItemChanged event handling

### Dropdown Customization

📄 **Read:** [references/dropdown-customization.md](references/dropdown-customization.md)

**When to read:** Customizing suggestion popup appearance and behavior

**What's covered:**
- DropDownBackground for suggestion box styling
- SuggestionBoxPlacement options (Bottom, Top, None)
- MaximumDropDownHeight configuration
- ShowSuggestionsOnFocus behavior
- PopupDelay for delayed display
- SelectionBackgroundColor for selected items
- AutoHighlightMatchedItem API
- SuggestionPopupOpened event
- SuggestionPopupClosed event
- Complete dropdown styling examples

### TextBox Customization

📄 **Read:** [references/textbox-customization.md](references/textbox-customization.md)

**When to read:** Customizing TextBox appearance, adding watermark or buttons

**What's covered:**
- Watermark property for placeholder text
- WatermarkTemplate for custom placeholder UI
- Text styling (FontSize, FontWeight, FontFamily)
- ShowDropDownButton for dropdown icon
- ShowClearButton for clear functionality
- Visual customization patterns
- Template customization examples

### Advanced Features

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

**When to read:** Implementing diacritic search, text highlighting, custom filters

**What's covered:**
- Diacritic sensitivity with IgnoreDiacritic
- TextHighlightMode (FirstOccurrence, MultiOccurrence)
- HighlightedTextColor configuration
- HighlightedTextStyle customization
- Multipath search implementation
- Custom SuggestionMode setup
- Filter property for custom filtering logic
- Custom filter implementation examples
- Dispose method for resource cleanup
- Performance optimization tips

## Quick Start Example

### Basic AutoComplete with Suggestions

```xaml
<Window xmlns:editors="http://schemas.syncfusion.com/wpf">
    <Window.DataContext>
        <local:EmployeeViewModel/>
    </Window.DataContext>
    
    <editors:SfTextBoxExt 
        Width="300"
        Height="40"
        Watermark="Search employees..."
        SearchItemPath="Name"
        AutoCompleteMode="Suggest"
        SuggestionMode="Contains"
        AutoCompleteSource="{Binding Employees}" />
</Window>
```

```csharp
// ViewModel
public class EmployeeViewModel
{
    public List<Employee> Employees { get; set; }
    
    public EmployeeViewModel()
    {
        Employees = new List<Employee>
        {
            new Employee { Name = "John Doe", Email = "john@example.com" },
            new Employee { Name = "Jane Smith", Email = "jane@example.com" },
            new Employee { Name = "Mike Johnson", Email = "mike@example.com" }
        };
    }
}

// Model
public class Employee
{
    public string Name { get; set; }
    public string Email { get; set; }
}
```

### Multi-Selection with Tokens

```xaml
<editors:SfTextBoxExt 
    Width="400"
    Height="60"
    Watermark="Select multiple items..."
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    MultiSelectMode="Token"
    TokensWrapMode="Wrap"
    EnableAutoSize="True"
    AutoCompleteSource="{Binding Items}" />
```

```csharp
// Code-behind
var textBoxExt = new SfTextBoxExt
{
    Width = 400,
    Watermark = "Select multiple items...",
    SearchItemPath = "Name",
    AutoCompleteMode = AutoCompleteMode.Suggest,
    MultiSelectMode = MultiSelectMode.Token,
    TokensWrapMode = TokensWrapMode.Wrap,
    EnableAutoSize = true,
    AutoCompleteSource = viewModel.Items
};
```

## Common Patterns

### Pattern 1: Basic AutoComplete with Case-Insensitive Search

```csharp
var autoComplete = new SfTextBoxExt
{
    AutoCompleteMode = AutoCompleteMode.Suggest,
    SuggestionMode = SuggestionMode.Contains,
    IgnoreCase = true,
    MinimumPrefixCharacters = 2,
    AutoCompleteSource = GetDataSource()
};
```

### Pattern 2: Multi-Selection with Event Handling

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    MultiSelectMode="Token"
    SelectedItemChanged="OnSelectedItemChanged"
    AutoCompleteSource="{Binding Items}" />
```

```csharp
private void OnSelectedItemChanged(object sender, SelectedItemChangedEventArgs e)
{
    var selectedItems = (sender as SfTextBoxExt)?.SelectedItems;
    // Handle selected items
    foreach (var item in selectedItems)
    {
        Console.WriteLine(item.ToString());
    }
}
```

### Pattern 3: Custom Filtering

```csharp
var autoComplete = new SfTextBoxExt
{
    SuggestionMode = SuggestionMode.Custom,
    AutoCompleteSource = employees
};

// Custom filter for multi-field search
autoComplete.Filter = (search, item) =>
{
    if (item is Employee emp)
    {
        return emp.Name.Contains(search, StringComparison.OrdinalIgnoreCase) ||
               emp.Email.Contains(search, StringComparison.OrdinalIgnoreCase) ||
               emp.Department.Contains(search, StringComparison.OrdinalIgnoreCase);
    }
    return false;
};
```

### Pattern 4: Popup Configuration

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    ShowSuggestionsOnFocus="True"
    PopupDelay="00:00:00.3"
    MaxDropDownHeight="200"
    SuggestionBoxPlacement="Bottom"
    AutoHighlightMatchedItem="True"
    AutoCompleteSource="{Binding Data}" />
```

## Key Properties Reference

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| `AutoCompleteSource` | `IEnumerable` | Data source for suggestions |
| `SearchItemPath` | `string` | Property path for filtering custom objects |
| `AutoCompleteMode` | `AutoCompleteMode` | Suggestion display mode (Suggest, Append, SuggestAppend, None) |
| `SuggestionMode` | `SuggestionMode` | Filter strategy (StartsWith, Contains, Equals, etc.) |
| `MultiSelectMode` | `MultiSelectMode` | Selection mode (None, Token, Delimiter) |
| `SelectedItem` | `object` | Currently selected item (single selection) |
| `SelectedItems` | `ObservableCollection<object>` | Selected items (multi-selection) |
| `SelectedValue` | `object` | Value of selected item |

### Configuration Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `MinimumPrefixCharacters` | `int` | `1` | Minimum characters before filtering |
| `IgnoreCase` | `bool` | `false` | Case-insensitive filtering |
| `MaximumSuggestionsCount` | `int` | `0` (unlimited) | Maximum suggestions displayed |
| `ShowSuggestionsOnFocus` | `bool` | `false` | Show all suggestions on focus |
| `PopupDelay` | `TimeSpan` | `00:00:00` | Delay before popup opens |
| `MaxDropDownHeight` | `double` | Auto | Maximum popup height |

### Customization Properties

| Property | Type | Description |
|----------|------|-------------|
| `Watermark` | `string` | Placeholder text |
| `WatermarkTemplate` | `DataTemplate` | Custom placeholder template |
| `AutoCompleteItemTemplate` | `DataTemplate` | Custom suggestion item template |
| `NoResultsFoundTemplate` | `DataTemplate` | Template when no matches found |
| `ShowDropDownButton` | `bool` | Display dropdown button |
| `ShowClearButton` | `bool` | Display clear button |

### Events

| Event | Description |
|-------|-------------|
| `SelectedItemChanged` | Raised when selected item changes |
| `SuggestionsChanged` | Raised when filtered suggestions update |
| `SuggestionPopupOpened` | Raised when popup opens |
| `SuggestionPopupClosed` | Raised when popup closes |

## Common Use Cases

### 1. Employee Search and Selection
Implement autocomplete for searching employees by name, email, or department with multi-field filtering.

### 2. Tag/Label Selection
Enable multi-selection with token mode for selecting tags, categories, or labels with visual token representation.

### 3. Product Search
Create searchable product dropdown with images, custom templates, and rich item display.

### 4. Email Recipients (To/CC/BCC)
Build email recipient selection with token mode, delimiter support, and contact suggestions.

### 5. Location Search
Implement location autocomplete with address suggestions, diacritic-insensitive search for international addresses.

### 6. Code Editor Auto-Completion
Create code suggestion dropdown with syntax highlighting, custom filtering, and keyboard navigation.

### 7. Form Auto-Fill
Add intelligent form input suggestions based on historical data or predefined options.

### 8. Multi-Language Search
Implement search with diacritic sensitivity for international content with accent-insensitive matching.

## Related Resources

- **Syncfusion Documentation:** https://help.syncfusion.com/wpf/autocomplete/overview
- **API Reference:** https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Controls.Input.SfTextBoxExt.html
- **Sample Browser:** Available via Syncfusion Control Panel
- **GitHub Samples:** https://github.com/SyncfusionExamples/wpf-textboxext-examples

## Support and Licensing

SfTextBoxExt is part of Syncfusion Essential Studio for WPF, a commercial product requiring a valid license. Visit https://www.syncfusion.com/sales/products for licensing information.

---

**Next Steps:** Navigate to the specific reference file based on your implementation needs. Start with [getting-started.md](references/getting-started.md) for initial setup, or jump directly to feature-specific guides for advanced scenarios.

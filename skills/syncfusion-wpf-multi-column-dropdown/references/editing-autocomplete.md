# Editing and Autocomplete in SfMultiColumnDropDownControl

## Table of Contents
- [Overview](#overview)
- [Editor Properties](#editor-properties)
- [Autocomplete](#autocomplete)
- [Incremental Filtering](#incremental-filtering)
- [Search Conditions](#search-conditions)
- [Case-Sensitive Filtering](#case-sensitive-filtering)
- [Diacritic-Sensitive Filtering](#diacritic-sensitive-filtering)
- [Custom Multi-Column Filtering](#custom-multi-column-filtering)
- [Editor Behavior Settings](#editor-behavior-settings)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The SfMultiColumnDropDownControl editor provides powerful text editing capabilities with autocomplete and filtering features. Users can type in the TextBox editor to search, filter, and select items from the dropdown grid.

## Editor Properties

Core properties that control editor behavior:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| **Text** | string | "" | Current text in the editor |
| **SearchText** | string | (Read-only) | Text used for filtering |
| **ReadOnly** | bool | false | Makes editor non-editable |
| **AllowNullInput** | bool | false | Allows null/empty values |
| **AllowSpinOnMouseWheel** | bool | true | Change selection with mouse wheel |
| **AllowImmediatePopup** | bool | false | Opens popup immediately when match found |
| **IsDropDownOpen** | bool | false | Controls popup open/close state |

### Basic Editor Usage

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    Text="Maria"
    ReadOnly="False"
    AllowNullInput="False"/>
```

## Autocomplete

The `AllowAutoComplete` property enables automatic text completion based on the DisplayMember property.

**Default:** `false`

### Enabling Autocomplete

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AllowAutoComplete="True"/>
```

### How Autocomplete Works

When enabled, as the user types, the control suggests completions:

1. User types: "Mar"
2. Control finds "Maria Anders" in DisplayMember
3. Suggested text "ia Anders" is appended and selected
4. User can accept by continuing to type or pressing Tab

### Example

**Data:**
- Maria Anders
- Mario Pontes
- Martin Sommer

**User types:** "Mar"
**Autocomplete suggests:** "Maria Anders" (first match)

### Code-Behind Control

```csharp
// Enable autocomplete
sfMultiColumn.AllowAutoComplete = true;

// Get current text
string currentText = sfMultiColumn.Text;

// Check what user is searching
string searchText = sfMultiColumn.SearchText;
```

## Incremental Filtering

The `AllowIncrementalFiltering` property filters the dropdown grid items based on typed text.

**Default:** `false`

### Enabling Filtering

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AllowIncrementalFiltering="True"
    AutoGenerateColumns="False">
    
    <syncfusion:SfMultiColumnDropDownControl.Columns>
        <syncfusion:GridTextColumn MappingName="CustomerName"/>
        <syncfusion:GridTextColumn MappingName="OrderID"/>
        <syncfusion:GridTextColumn MappingName="Country"/>
    </syncfusion:SfMultiColumnDropDownControl.Columns>
</syncfusion:SfMultiColumnDropDownControl>
```

### Filtering Behavior

When incremental filtering is enabled:

1. User types text in editor
2. Dropdown grid filters items matching DisplayMember
3. Only matching rows are shown in popup
4. Filter updates as user types each character

**Example:**

**Full Data:**
- Maria Anders (Germany)
- Ana Trujilo (Mexico)  
- Thomas Hardy (UK)
- Martin Sommer (Spain)

**User types "Ma":**
**Filtered results:**
- Maria Anders
- Martin Sommer

### Combined with Autocomplete

```xml
<!-- Best user experience: both enabled -->
<syncfusion:SfMultiColumnDropDownControl 
    AllowAutoComplete="True"
    AllowIncrementalFiltering="True"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"/>
```

This combination provides:
- Text completion suggestions
- Filtered dropdown list
- Faster item selection

## Search Conditions

The `SearchCondition` property controls how text matching is performed during filtering and autocomplete.

**Default:** `StartsWith`

### Available Search Conditions

| Condition | Description | Example |
|-----------|-------------|---------|
| **StartsWith** | Match beginning of text | "Mar" matches "Maria Anders" |
| **Contains** | Match anywhere in text | "and" matches "Maria **And**ers" |
| **Equals** | Exact match only | "Maria Anders" matches only exact text |

### Using StartsWith (Default)

```xml
<syncfusion:SfMultiColumnDropDownControl 
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AllowIncrementalFiltering="True"
    SearchCondition="StartsWith"/>
```

**User types:** "Tho"  
**Matches:** "Thomas Hardy"  
**Does not match:** "Bartholomew"

### Using Contains

```xml
<syncfusion:SfMultiColumnDropDownControl 
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AllowIncrementalFiltering="True"
    SearchCondition="Contains"/>
```

**User types:** "and"  
**Matches:** "Maria **And**ers", "Al**and**ra Smith"

### Using Equals

```xml
<syncfusion:SfMultiColumnDropDownControl 
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AllowIncrementalFiltering="True"
    SearchCondition="Equals"/>
```

**User types:** "Maria Anders"  
**Matches:** Only exact "Maria Anders"

## Case-Sensitive Filtering

The `AllowCaseSensitiveFiltering` property controls whether filtering respects character casing.

**Default:** `false` (case-insensitive)

### Case-Insensitive (Default)

```xml
<syncfusion:SfMultiColumnDropDownControl 
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AllowIncrementalFiltering="True"
    AllowCaseSensitiveFiltering="False"/>
```

**User types:** "maria" or "MARIA" or "Maria"  
**All match:** "Maria Anders"

### Case-Sensitive

```xml
<syncfusion:SfMultiColumnDropDownControl 
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AllowIncrementalFiltering="True"
    AllowCaseSensitiveFiltering="True"/>
```

**User types:** "maria"  
**Does not match:** "Maria Anders"  
**User types:** "Maria"  
**Matches:** "Maria Anders"

## Diacritic-Sensitive Filtering

The `AllowDiacriticSensitiveFiltering` property controls whether diacritical marks (accents) are considered during filtering.

**Default:** `true` (diacritic-sensitive)

### Diacritic-Insensitive

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding PopulationDetails}"
    DisplayMember="Continent"
    AllowIncrementalFiltering="True"
    AllowImmediatePopup="True"
    AllowDiacriticSensitiveFiltering="False"/>
```

**Example Data:**
- Café
- Naïve
- Résumé

**User types:** "Cafe" (without accent)  
**Matches:** "Café" (with diacritic-insensitive filtering)

### Diacritic-Sensitive (Default)

```xml
<syncfusion:SfMultiColumnDropDownControl 
    AllowDiacriticSensitiveFiltering="True"
    ItemsSource="{Binding PopulationDetails}"
    DisplayMember="Continent"/>
```

**User types:** "Cafe"  
**Does not match:** "Café"  
**User must type:** "Café" (exact diacritics)

## Custom Multi-Column Filtering

By default, filtering only considers the DisplayMember column. Override the `FilterRecord` method to filter based on multiple columns.

### Custom Filter Implementation

```csharp
public class CustomMultiColumnControl : SfMultiColumnDropDownControl
{
    /// <summary>
    /// Returns true if item should be displayed in filtered list
    /// </summary>
    protected override bool FilterRecord(object item)
    {
        var orderInfo = item as OrderInfo;
        
        if (orderInfo == null)
            return false;
        
        // Get the search text entered by user
        string searchText = this.SearchText?.ToLower() ?? "";
        
        if (string.IsNullOrEmpty(searchText))
            return true;
        
        // Filter by multiple properties
        bool matchesCustomerName = orderInfo.CustomerName?.ToLower().Contains(searchText) ?? false;
        bool matchesCity = orderInfo.City?.ToLower().Contains(searchText) ?? false;
        bool matchesCountry = orderInfo.Country?.ToLower().Contains(searchText) ?? false;
        
        // Return true if any column matches
        return matchesCustomerName || matchesCity || matchesCountry;
    }
}
```

### Using Custom Control in XAML

```xml
<Window xmlns:local="clr-namespace:YourNamespace">
    <local:CustomMultiColumnControl 
        x:Name="sfMultiColumn"
        ItemsSource="{Binding Orders}"
        DisplayMember="CustomerName"
        AllowIncrementalFiltering="True"
        AutoGenerateColumns="False">
        
        <local:CustomMultiColumnControl.Columns>
            <syncfusion:GridTextColumn MappingName="CustomerName"/>
            <syncfusion:GridTextColumn MappingName="City"/>
            <syncfusion:GridTextColumn MappingName="Country"/>
        </local:CustomMultiColumnControl.Columns>
    </local:CustomMultiColumnControl>
</Window>
```

### Advanced Filtering Logic

```csharp
protected override bool FilterRecord(object item)
{
    var order = item as OrderInfo;
    string search = this.SearchText?.ToLower() ?? "";
    
    if (string.IsNullOrEmpty(search))
        return true;
    
    // Check if search contains space (multiple terms)
    if (search.Contains(" "))
    {
        string[] terms = search.Split(' ');
        
        // All terms must match somewhere
        foreach (string term in terms)
        {
            bool termFound = 
                (order.CustomerName?.ToLower().Contains(term) ?? false) ||
                (order.City?.ToLower().Contains(term) ?? false) ||
                (order.Country?.ToLower().Contains(term) ?? false);
            
            if (!termFound)
                return false;
        }
        
        return true;
    }
    
    // Single term search
    return 
        (order.CustomerName?.ToLower().StartsWith(search) ?? false) ||
        (order.City?.ToLower().Contains(search) ?? false) ||
        (order.Country?.ToLower().Contains(search) ?? false) ||
        (order.OrderID.ToString().StartsWith(search));
}
```

## Editor Behavior Settings

### AllowImmediatePopup

Controls whether popup opens immediately when typing finds a match.

```xml
<syncfusion:SfMultiColumnDropDownControl 
    AllowIncrementalFiltering="True"
    AllowImmediatePopup="True"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"/>
```

**When True:**
- User starts typing
- Popup opens automatically when matches are found
- Popup closes if no matches

**When False (default):**
- User must click dropdown button or press F4 to open popup
- Typing filters data but popup stays closed unless already open

### AllowSpinOnMouseWheel

Enables changing selection using the mouse wheel when editor has focus.

```xml
<syncfusion:SfMultiColumnDropDownControl 
    AllowSpinOnMouseWheel="True"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"/>
```

### AllowNullInput

Allows the editor to accept null or empty values.

```xml
<syncfusion:SfMultiColumnDropDownControl 
    AllowNullInput="True"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"/>
```

**When True:**
- User can clear the editor
- SelectedItem can be null
- SelectedIndex becomes -1

**When False (default):**
- Editor must have a value
- Clearing text restores previous selection

### ReadOnly Mode

Makes the editor non-editable. Users can only select from the dropdown grid.

```xml
<syncfusion:SfMultiColumnDropDownControl 
    ReadOnly="True"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"/>
```

**Behavior:**
- User cannot type in editor
- Selection only through dropdown grid
- Arrow keys change selection
- Autocomplete and filtering disabled

## Best Practices

### 1. Combine Autocomplete and Filtering

```xml
<!-- Optimal user experience -->
<syncfusion:SfMultiColumnDropDownControl 
    AllowAutoComplete="True"
    AllowIncrementalFiltering="True"
    SearchCondition="Contains"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"/>
```

### 2. Use Contains for Flexible Search

```xml
<!-- Users can search by any part of the text -->
<syncfusion:SfMultiColumnDropDownControl 
    AllowIncrementalFiltering="True"
    SearchCondition="Contains"
    DisplayMember="CustomerName"/>
```

### 3. Keep Case-Insensitive by Default

```xml
<!-- Easier for users -->
<syncfusion:SfMultiColumnDropDownControl 
    AllowCaseSensitiveFiltering="False"
    AllowDiacriticSensitiveFiltering="False"
    DisplayMember="CustomerName"/>
```

### 4. Implement Multi-Column Search for Better UX

```csharp
// Allow users to search across multiple relevant columns
protected override bool FilterRecord(object item)
{
    var data = item as YourDataType;
    string search = this.SearchText?.ToLower() ?? "";
    
    return 
        data.PrimaryField?.ToLower().Contains(search) ?? false ||
        data.SecondaryField?.ToLower().Contains(search) ?? false;
}
```

## Troubleshooting

### Autocomplete Not Working

**Problem:** Typing doesn't show suggestions

**Solutions:**
```xml
<!-- Verify AllowAutoComplete is True -->
<syncfusion:SfMultiColumnDropDownControl 
    AllowAutoComplete="True"
    DisplayMember="CustomerName"/>  <!-- DisplayMember must be set -->
```

### Filtering Not Working

**Problem:** Typing doesn't filter dropdown list

**Solutions:**
```xml
<!-- Enable incremental filtering -->
<syncfusion:SfMultiColumnDropDownControl 
    AllowIncrementalFiltering="True"
    DisplayMember="CustomerName"
    ItemsSource="{Binding Orders}"/>  <!-- ItemsSource must have data -->
```

### No Matches Found

**Problem:** Filter seems too restrictive

**Solutions:**
```xml
<!-- Use Contains instead of StartsWith -->
<syncfusion:SfMultiColumnDropDownControl 
    SearchCondition="Contains"
    AllowCaseSensitiveFiltering="False"
    AllowIncrementalFiltering="True"/>
```

### Custom FilterRecord Not Called

**Problem:** Override method never executes

**Solutions:**
```csharp
// Ensure AllowIncrementalFiltering is True
<local:CustomMultiColumnControl AllowIncrementalFiltering="True"/>

// Verify class inheritance
public class CustomMultiColumnControl : SfMultiColumnDropDownControl  // ✓
```

### Popup Doesn't Open When Typing

**Problem:** Want popup to open automatically

**Solution:**
```xml
<syncfusion:SfMultiColumnDropDownControl 
    AllowIncrementalFiltering="True"
    AllowImmediatePopup="True"/>  <!-- Opens popup when typing -->
```

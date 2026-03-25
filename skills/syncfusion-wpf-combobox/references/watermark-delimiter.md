# Watermark and Delimiter Support

## Overview

ComboBoxAdv provides watermark (placeholder text) and delimiter customization features to enhance user experience and visual clarity, especially for multiselection scenarios.

**Key Features:**
- **Watermark (DefaultText)**: Placeholder text when no item is selected
- **Delimiter Customization**: Custom separator for multiselect display

## Watermark Support (DefaultText)

The `DefaultText` property displays placeholder text in the combo box when no items are selected, providing helpful hints to users about expected input.

### Basic Watermark

```xml
<syncfusion:ComboBoxAdv DefaultText="Choose Items..."
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        Height="30" Width="200"/>
```

```csharp
ComboBoxAdv comboBox = new ComboBoxAdv();
comboBox.DefaultText = "Choose Items...";
```

**Behavior:**
- Text appears in gray when no items are selected
- Disappears when an item is selected
- Returns when all items are deselected

### Property Details

| Property | Type | Description |
|----------|------|-------------|
| `DefaultText` | string | Watermark text displayed when no selection |

### Common Watermark Patterns

#### Pattern 1: Instructional Prompt

```xml
<syncfusion:ComboBoxAdv DefaultText="Select a country from the list"/>
```

#### Pattern 2: Required Field Indicator

```xml
<syncfusion:ComboBoxAdv DefaultText="* Select a category (required)"/>
```

#### Pattern 3: Search Hint

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        DefaultText="Type to search..."/>
```

#### Pattern 4: Contextual Hint

```xml
<syncfusion:ComboBoxAdv DefaultText="Filter by status: Active, Inactive, Pending"/>
```

#### Pattern 5: MultiSelect Hint

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        DefaultText="Select one or more options"/>
```

### Best Practices for Watermarks

**1. Be Clear and Concise**
```xml
<!-- ✅ Good -->
<syncfusion:ComboBoxAdv DefaultText="Select country"/>

<!-- ❌ Too verbose -->
<syncfusion:ComboBoxAdv DefaultText="Please choose a country from this dropdown list"/>
```

**2. Use Action Verbs**
```xml
<!-- ✅ Good -->
<syncfusion:ComboBoxAdv DefaultText="Choose category"/>
<syncfusion:ComboBoxAdv DefaultText="Select date range"/>

<!-- ❌ Passive -->
<syncfusion:ComboBoxAdv DefaultText="Category"/>
```

**3. Indicate Expected Action for Editable Boxes**
```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        DefaultText="Type or select..."/>
```

**4. Use Ellipsis for Actions**
```xml
<syncfusion:ComboBoxAdv DefaultText="Select language..."/>
```

### Styling DefaultText

Customize the appearance using control templates and styles:

```xml
<syncfusion:ComboBoxAdv DefaultText="Custom styled text">
    <syncfusion:ComboBoxAdv.Resources>
        <Style TargetType="TextBlock" x:Key="WatermarkStyle">
            <Setter Property="Foreground" Value="#999999"/>
            <Setter Property="FontStyle" Value="Italic"/>
        </Style>
    </syncfusion:ComboBoxAdv.Resources>
</syncfusion:ComboBoxAdv>
```

## Delimiter String Customization

The `SelectedValueDelimiter` property customizes the separator string displayed between selected items in multiselection mode (when not using tokens).

### Basic Delimiter

**Default Delimiter (Comma):**
```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"/>
```
**Display:** "Denmark, Canada, Russia"

**Custom Delimiter:**
```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter=" | "/>
```
**Display:** "Denmark | Canada | Russia"

```csharp
comboBox.SelectedValueDelimiter = " | ";
```

### Property Details

| Property | Type | Description |
|----------|------|-------------|
| `SelectedValueDelimiter` | string | Separator between selected items in multiselect |

### Delimiter Patterns

#### Pattern 1: Pipe Separator

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter=" | "
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"/>
```
**Result:** Denmark | Canada | Russia

#### Pattern 2: Semicolon Separator

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter="; "
                        ItemsSource="{Binding Items}"/>
```
**Result:** Item1; Item2; Item3

#### Pattern 3: Bullet Separator

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter=" • "
                        ItemsSource="{Binding Tags}"/>
```
**Result:** Tag1 • Tag2 • Tag3

#### Pattern 4: Comma with Space (More Readable)

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter=", "
                        ItemsSource="{Binding Options}"/>
```
**Result:** Option1, Option2, Option3

#### Pattern 5: Line Break (Vertical List)

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter="&#10;"
                        Height="Auto"
                        ItemsSource="{Binding Items}"/>
```
**Result:**
```
Item1
Item2
Item3
```

#### Pattern 6: Custom Symbol

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter=" → "
                        ItemsSource="{Binding Steps}"/>
```
**Result:** Step1 → Step2 → Step3

### Delimiter with Watermark

Combine for a complete user experience:

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        DefaultText="Select multiple items..."
                        SelectedValueDelimiter=" | "
                        ItemsSource="{Binding Items}"
                        DisplayMemberPath="Name"
                        Height="30" Width="300"/>
```

**User Experience:**
1. Initially shows: "Select multiple items..."
2. After selecting Denmark, Canada: "Denmark | Canada"
3. If all deselected: Returns to "Select multiple items..."

### Delimiter vs Tokens

**Important:** When `EnableToken` is `True`, the delimiter is NOT used for visual display (tokens are shown instead), but it may be used for text representation in some scenarios.

```xml
<!-- Delimiter affects display -->
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter=" | "/>

<!-- Tokens override delimiter display -->
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        EnableToken="True"
                        IsEditable="True"
                        SelectedValueDelimiter=" | "/>  <!-- Not visible -->
```

**Recommendation:** Use tokens for better UX in multiselect scenarios:
```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        IsEditable="True"
                        EnableToken="True"
                        DefaultText="Select tags..."/>
```

## Complete Examples

### Example 1: Single Select with Watermark

```xml
<Window.DataContext>
    <local:ViewModel />
</Window.DataContext>

<syncfusion:ComboBoxAdv DefaultText="Select your country"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        SelectedItem="{Binding SelectedCountry}"
                        Height="30" Width="250"/>
```

### Example 2: MultiSelect with Custom Delimiter

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        DefaultText="Choose categories..."
                        SelectedValueDelimiter=" • "
                        ItemsSource="{Binding Categories}"
                        DisplayMemberPath="CategoryName"
                        SelectedItems="{Binding SelectedCategories}"
                        Height="30" Width="300"/>
```

### Example 3: Editable Search with Watermark

```xml
<syncfusion:ComboBoxAdv IsEditable="True"
                        AutoCompleteMode="Suggest"
                        DefaultText="🔍 Type to search products..."
                        ItemsSource="{Binding Products}"
                        DisplayMemberPath="ProductName"
                        Height="30" Width="300"/>
```

### Example 4: MultiSelect with OK/Cancel and Delimiter

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        AllowSelectAll="True"
                        EnableOKCancel="True"
                        DefaultText="Select options (use OK to confirm)"
                        SelectedValueDelimiter="; "
                        ItemsSource="{Binding Options}"
                        DisplayMemberPath="OptionName"
                        Height="30" Width="350"/>
```

## Best Practices

### 1. Match Delimiter to Context

**For formal lists:** Use semicolon or pipe
```xml
<syncfusion:ComboBoxAdv SelectedValueDelimiter="; "/>
```

**For casual lists:** Use bullet or comma
```xml
<syncfusion:ComboBoxAdv SelectedValueDelimiter=" • "/>
```

### 2. Always Include Space Around Delimiters

```xml
<!-- ✅ Good - readable -->
<syncfusion:ComboBoxAdv SelectedValueDelimiter=" | "/>

<!-- ❌ Bad - cramped -->
<syncfusion:ComboBoxAdv SelectedValueDelimiter="|"/>
```

### 3. Consider Width for Long Delimiter Displays

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter=" | "
                        MinWidth="300"/>  <!-- Ensure enough space -->
```

### 4. Use Tokens Instead for Better UX

For multiselect, tokens provide clearer visual feedback:

```xml
<!-- Better approach -->
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        IsEditable="True"
                        EnableToken="True"
                        DefaultText="Add items..."/>
```

### 5. Keep DefaultText Short and Action-Oriented

```xml
<!-- ✅ Good -->
<syncfusion:ComboBoxAdv DefaultText="Select status"/>

<!-- ❌ Too long -->
<syncfusion:ComboBoxAdv DefaultText="Please use this dropdown to select the current status of the item"/>
```

## Troubleshooting

### Issue: DefaultText Not Showing

**Cause:** Item already selected or SelectedItem bound to non-null value

**Solution:**
- Ensure no item is initially selected
- Check ViewModel initialization doesn't set a default selection

```csharp
// ❌ Causes issue
public ViewModel()
{
    SelectedItem = Items[0];  // DefaultText won't show
}

// ✅ Correct
public ViewModel()
{
    SelectedItem = null;  // DefaultText will show
}
```

### Issue: Delimiter Not Visible

**Cause:** Tokens enabled or not in multiselect mode

**Solution:**
```xml
<!-- Ensure AllowMultiSelect=True and EnableToken=False (or not set) -->
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter=" | "/>
```

### Issue: Delimiter Shows Even with Tokens

**Cause:** Both delimiter and tokens configured

**Solution:** Choose one approach:
```xml
<!-- Option 1: Delimiter (simpler) -->
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        SelectedValueDelimiter=" | "/>

<!-- Option 2: Tokens (better UX) -->
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        IsEditable="True"
                        EnableToken="True"/>
```

### Issue: DefaultText Overlaps Selected Text

**Cause:** Custom styling or template issue

**Solution:** Use default template or ensure proper visibility bindings in custom templates.

## See Also

- [Selection and MultiSelection](selection-multiselection.md) - Selection features and tokens
- [Editing and AutoComplete](editing-autocomplete.md) - Editable mode with search
- [Getting Started](getting-started.md) - Basic setup
- [Syncfusion DefaultText Documentation](https://help.syncfusion.com/wpf/combobox/watermark-support)

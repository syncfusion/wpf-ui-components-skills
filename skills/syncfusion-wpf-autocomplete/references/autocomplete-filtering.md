# AutoComplete and Filtering

Complete guide for implementing autocomplete functionality with advanced filtering options in WPF Autocomplete (SfTextBoxExt).

## Table of Contents
- [AutoComplete Source](#autocomplete-source)
- [Custom Data with SearchItemPath](#custom-data-with-searchitempath)
- [AutoCompleteItemTemplate](#autocompleteitemtemplate)
- [Filtering Options (SuggestionMode)](#filtering-options-suggestionmode)
- [Prefix Characters Constraint](#prefix-characters-constraint)
- [Case Sensitivity](#case-sensitivity)
- [Image Support](#image-support)
- [No Results Found Template](#no-results-found-template)
- [Maximum Suggestions Count](#maximum-suggestions-count)
- [FilterSuggestions Method](#filtersuggestions-method)
- [SuggestionsChanged Event](#suggestionschanged-event)

## AutoComplete Source

The `AutoCompleteSource` property is the core data source for suggestions. It accepts any `IEnumerable` implementation including simple strings or complex custom objects.

### Binding Simple String Collection

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Countries}" />
```

```csharp
public class ViewModel
{
    public List<string> Countries { get; set; }
    
    public ViewModel()
    {
        Countries = new List<string>
        {
            "United States", "United Kingdom", "Canada", 
            "Australia", "Germany", "France"
        };
    }
}
```

### Binding Custom Objects

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
public class Employee
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string Department { get; set; }
}

public class EmployeeViewModel
{
    public List<Employee> Employees { get; set; }
    
    public EmployeeViewModel()
    {
        Employees = new List<Employee>
        {
            new Employee { Name = "Eric", Email = "Eric@syncfusion.com", Department = "Engineering" },
            new Employee { Name = "James", Email = "James@syncfusion.com", Department = "Sales" },
            // ... more employees
        };
    }
}
```

## Custom Data with SearchItemPath

When binding custom objects, use `SearchItemPath` to specify which property should be used for filtering and display.

### Basic Usage

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
textBoxExt.SearchItemPath = "Name";
```

**What it does:**
- Filters based on the "Name" property
- Displays the "Name" property in the suggestion list
- Returns the entire Employee object when selected

### Multiple Property Paths

For nested properties, use dot notation:

```csharp
// For model: Customer → Address → City
textBoxExt.SearchItemPath = "Address.City";
```

## AutoCompleteItemTemplate

Customize the visual appearance of suggestion items using `AutoCompleteItemTemplate`.

### Adding Images to Suggestions

```xaml
<editors:SfTextBoxExt 
    Width="400"
    Height="40"
    SearchItemPath="Name"
    AutoCompleteMode="SuggestAppend"
    AutoCompleteSource="{Binding Employees}">
    <editors:SfTextBoxExt.AutoCompleteItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Source="User.png" Margin="2" Stretch="Uniform" Width="16" Height="16"/>
                <TextBlock Text="{Binding Name}" Margin="5,2" VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </editors:SfTextBoxExt.AutoCompleteItemTemplate>
</editors:SfTextBoxExt>
```

### Multi-Line Item Template

```xaml
<editors:SfTextBoxExt.AutoCompleteItemTemplate>
    <DataTemplate>
        <StackPanel Margin="5">
            <TextBlock Text="{Binding Name}" 
                       FontWeight="Bold" 
                       FontSize="14"/>
            <TextBlock Text="{Binding Email}" 
                       Foreground="Gray" 
                       FontSize="11"/>
            <TextBlock Text="{Binding Department}" 
                       Foreground="DarkGray" 
                       FontSize="10"/>
        </StackPanel>
    </DataTemplate>
</editors:SfTextBoxExt.AutoCompleteItemTemplate>
```

### Template with Conditional Formatting

```xaml
<editors:SfTextBoxExt.AutoCompleteItemTemplate>
    <DataTemplate>
        <Border BorderBrush="LightGray" BorderThickness="0,0,0,1" Padding="5">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                
                <Ellipse Grid.Column="0" Width="8" Height="8" Margin="0,0,8,0">
                    <Ellipse.Fill>
                        <SolidColorBrush Color="{Binding StatusColor}"/>
                    </Ellipse.Fill>
                </Ellipse>
                
                <TextBlock Grid.Column="1" Text="{Binding Name}" VerticalAlignment="Center"/>
                
                <TextBlock Grid.Column="2" 
                           Text="{Binding Badge}" 
                           FontSize="10" 
                           Foreground="White" 
                           Background="DodgerBlue"
                           Padding="4,2"
                           BorderRadius="3"/>
            </Grid>
        </Border>
    </DataTemplate>
</editors:SfTextBoxExt.AutoCompleteItemTemplate>
```

## Filtering Options (SuggestionMode)

The `SuggestionMode` property provides 18 different filtering strategies for matching user input with suggestions.

### SuggestionMode Options

| Mode | Case Sensitive | Description | Example (Input: "al") |
|------|----------------|-------------|----------------------|
| **None** | N/A | Returns entire collection without filtering | All items returned |
| **StartsWith** | No | Matches items beginning with typed text | "**Al**an", "**Al**ice" |
| **StartsWithCaseSensitive** | Yes | Case-sensitive begins-with match | "**Al**an" (not "alice") |
| **StartsWithOrdinal** | No | Ordinal ignore case begins-with | "**Al**an", "**AL**ICE" |
| **StartsWithOrdinalCaseSensitive** | Yes | Ordinal case-sensitive begins-with | "**Al**an" (not "ALICE") |
| **Contains** | No | Matches items containing typed text | "**Al**an", "M**al**com", "V**al**erie" |
| **ContainsCaseSensitive** | Yes | Case-sensitive contains match | "**Al**an", "M**al**com" (not "ALICE") |
| **ContainsOrdinal** | No | Ordinal ignore case contains | "**Al**an", "M**AL**COM" |
| **ContainsOrdinalCaseSensitive** | Yes | Ordinal case-sensitive contains | "**Al**an" only |
| **EndsWith** | No | Matches items ending with typed text | "Cryst**al**", "Roy**al**" |
| **EndsWithCaseSensitive** | Yes | Case-sensitive ends-with match | "cryst**al**" (not "ROYAL") |
| **EndsWithOrdinal** | No | Ordinal ignore case ends-with | "Cryst**AL**", "Roy**al**" |
| **EndsWithOrdinalCaseSensitive** | Yes | Ordinal case-sensitive ends-with | "cryst**al**" only |
| **Equals** | No | Exact match ignoring case | "Al" = "**al**", "**AL**" |
| **EqualsCaseSensitive** | Yes | Exact case-sensitive match | "Al" = "**Al**" only |
| **EqualsOrdinal** | No | Ordinal ignore case exact match | "Al" = "**al**", "**AL**" |
| **EqualsOrdinalCaseSensitive** | Yes | Ordinal case-sensitive exact | "Al" = "**Al**" only |
| **Custom** | User-defined | Uses custom Filter delegate | See Custom Filter section |

### Using StartsWith (Default)

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    SuggestionMode="StartsWith"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}"/>
```

```csharp
textBoxExt.SuggestionMode = SuggestionMode.StartsWith;
```

### Using Contains

Most flexible for user input:

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    SuggestionMode="Contains"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}"/>
```

```csharp
textBoxExt.SuggestionMode = SuggestionMode.Contains;
```

### Custom Filtering

Implement complex filtering logic with `Custom` mode:

```xaml
<editors:SfTextBoxExt x:Name="autoComplete"
    Width="300"
    Height="40"
    AutoCompleteMode="Suggest"
    SuggestionMode="Custom"
    AutoCompleteSource="{Binding Countries}"/>
```

```csharp
public MainWindow()
{
    InitializeComponent();
    autoComplete.Filter = ContainingSpaceFilter;
}

// Custom filter for space-separated search
public bool ContainingSpaceFilter(string search, object item)
{
    if (item == null) return false;
    
    string text = item.ToString().ToLower();
    var searchTerms = search.ToLower().Split(new[] { ' ' }, StringSplitOptions.RemoveEmptyEntries);
    
    // All terms must be present
    return searchTerms.All(term => text.Contains(term));
}
```

### Multi-Field Custom Filter

```csharp
// Filter across multiple properties
public bool MultiFieldFilter(string search, object item)
{
    if (item is Employee emp)
    {
        string searchLower = search.ToLower();
        return emp.Name.ToLower().Contains(searchLower) ||
               emp.Email.ToLower().Contains(searchLower) ||
               emp.Department.ToLower().Contains(searchLower);
    }
    return false;
}

autoComplete.Filter = MultiFieldFilter;
```

### Fuzzy Search Filter

```csharp
// Tolerates typos (simplified Levenshtein)
public bool FuzzyFilter(string search, object item)
{
    if (item == null || string.IsNullOrEmpty(search)) return false;
    
    string text = item.ToString().ToLower();
    string searchLower = search.ToLower();
    
    // Allow up to 2 character differences
    int maxDistance = 2;
    int distance = ComputeLevenshteinDistance(text, searchLower);
    
    return distance <= maxDistance || text.Contains(searchLower);
}
```

## Prefix Characters Constraint

Control when suggestions appear by setting minimum characters required before filtering.

### MinimumPrefixCharacters Property

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    MinimumPrefixCharacters="2"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
// Require at least 3 characters before filtering
textBoxExt.MinimumPrefixCharacters = 3;
```

**Benefits:**
- **Performance**: Reduces filtering on large datasets
- **User Experience**: Avoids overwhelming users with too many initial suggestions
- **Network Efficiency**: Useful with remote data sources

**Default Value:** `1` (filter on every character)

### Use Cases

| Min Characters | Use Case |
|----------------|----------|
| 1 | Small datasets (<100 items), instant feedback |
| 2-3 | Medium datasets (100-1000 items), balanced UX |
| 3-4 | Large datasets (1000+ items), search-focused |
| 4+ | Very large datasets, API searches |

## Case Sensitivity

Control case-sensitive filtering with the `IgnoreCase` property.

### IgnoreCase Property

```xaml
<editors:SfTextBoxExt 
    IgnoreCase="True"
    AutoCompleteMode="Suggest"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}"/>
```

```csharp
// Enable case-insensitive filtering
textBoxExt.IgnoreCase = true;
```

**Default Value:** `false`

**When True:**
- "eric" matches "Eric", "ERIC", "ErIc"
- More user-friendly for general search

**When False:**
- "eric" only matches "eric"
- Useful for case-sensitive data (codes, IDs)

**Note:** `IgnoreCase` works with all `SuggestionMode` options except the `CaseSensitive` variants.

## Image Support

Display images in both suggestion items and selected tokens.

### ImageMemberPath for Tokens

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    ImageMemberPath="ImagePath"
    AutoCompleteMode="Suggest"
    MultiSelectMode="Token"
    AutoCompleteSource="{Binding Employees}"/>
```

```csharp
public class Employee
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string ImagePath { get; set; } // Path to image file
}
```

**Note:** `ImageMemberPath` only works with `MultiSelectMode="Token"`.

### Images in Dropdown (AutoCompleteItemTemplate)

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}">
    <editors:SfTextBoxExt.AutoCompleteItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal" Height="40">
                <Image Source="{Binding ImagePath}" 
                       Margin="2" 
                       Height="35" 
                       Width="35" 
                       Stretch="Uniform"/>
                <StackPanel Margin="5,2" Orientation="Vertical">
                    <TextBlock Text="{Binding Name}" 
                               FontSize="12" 
                               Foreground="Black"/>
                    <TextBlock Text="{Binding Email}" 
                               FontSize="10" 
                               Foreground="Gray"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </editors:SfTextBoxExt.AutoCompleteItemTemplate>
</editors:SfTextBoxExt>
```

## No Results Found Template

Customize the message displayed when no matches are found.

### Basic Custom Message

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}">
    <editors:SfTextBoxExt.NoResultsFoundTemplate>
        <DataTemplate>
            <Label Content="No Results Found" 
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center"
                   Foreground="Gray"/>
        </DataTemplate>
    </editors:SfTextBoxExt.NoResultsFoundTemplate>
</editors:SfTextBoxExt>
```

### Advanced Template with Icon

```xaml
<editors:SfTextBoxExt.NoResultsFoundTemplate>
    <DataTemplate>
        <StackPanel HorizontalAlignment="Center" 
                    VerticalAlignment="Center" 
                    Margin="20">
            <Path Data="M12,2C6.5,2 2,6.5 2,12C2,17.5 6.5,22 12,22..." 
                  Fill="LightGray" 
                  Width="32" 
                  Height="32"
                  Stretch="Uniform"
                  HorizontalAlignment="Center"/>
            <TextBlock Text="No matches found" 
                       FontSize="14" 
                       Foreground="Gray" 
                       Margin="0,10,0,0"
                       HorizontalAlignment="Center"/>
            <TextBlock Text="Try a different search term" 
                       FontSize="11" 
                       Foreground="DarkGray"
                       HorizontalAlignment="Center"/>
        </StackPanel>
    </DataTemplate>
</editors:SfTextBoxExt.NoResultsFoundTemplate>
```

## Maximum Suggestions Count

Limit the number of suggestions displayed in the dropdown.

### MaximumSuggestionsCount Property

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    MaximumSuggestionsCount="5"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
// Limit to 10 suggestions
textBoxExt.MaximumSuggestionsCount = 10;
```

**Default Value:** `0` (unlimited)

**Benefits:**
- **Performance**: Faster rendering with fewer items
- **UX**: Prevents overwhelming dropdown
- **Scrolling**: Reduces need for scrolling

**Recommended Values:**
- **5-7**: Mobile or compact UIs
- **10-15**: Desktop applications
- **20+**: Search-heavy applications

## FilterSuggestions Method

Manually trigger suggestion filtering programmatically.

### Basic Usage

```csharp
// Force refresh of suggestions
textBoxExt.FilterSuggestions();
```

### Use Cases

**1. Dynamic Data Source Changes:**

```csharp
public void UpdateDataSource()
{
    // Update the data source
    textBoxExt.AutoCompleteSource = GetUpdatedEmployees();
    
    // Manually refresh suggestions
    textBoxExt.FilterSuggestions();
}
```

**2. External Filter Changes:**

```csharp
private void OnFilterCriteriaChanged(object sender, EventArgs e)
{
    // Update filter settings
    textBoxExt.SuggestionMode = GetNewSuggestionMode();
    
    // Reapply filtering
    textBoxExt.FilterSuggestions();
}
```

**3. Programmatic Text Updates:**

```csharp
public void SetSearchText(string text)
{
    textBoxExt.Text = text;
    textBoxExt.FilterSuggestions(); // Show suggestions for the new text
}
```

## SuggestionsChanged Event

Respond to changes in the filtered suggestion list.

### Event Handler

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    SearchItemPath="Name"
    SuggestionsChanged="OnSuggestionsChanged"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
private void OnSuggestionsChanged(object sender, EventArgs e)
{
    var textBox = sender as SfTextBoxExt;
    
    // Access current text
    string searchText = textBox.Text;
    
    // Log suggestion count (requires reflection or tracking)
    Console.WriteLine($"Suggestions updated for: {searchText}");
}
```

### Use Cases

**1. Analytics Tracking:**

```csharp
private void OnSuggestionsChanged(object sender, EventArgs e)
{
    var textBox = sender as SfTextBoxExt;
    AnalyticsService.TrackSearch(textBox.Text);
}
```

**2. Status Updates:**

```csharp
private void OnSuggestionsChanged(object sender, EventArgs e)
{
    var textBox = sender as SfTextBoxExt;
    StatusLabel.Text = $"Searching for: {textBox.Text}...";
}
```

**3. Conditional Filtering:**

```csharp
private void OnSuggestionsChanged(object sender, EventArgs e)
{
    var textBox = sender as SfTextBoxExt;
    
    // Show message if text is too short
    if (textBox.Text.Length < 3)
    {
        MessageLabel.Text = "Type at least 3 characters";
    }
    else
    {
        MessageLabel.Text = "";
    }
}
```

## Best Practices

### Performance Optimization

```csharp
// Large datasets: increase min prefix, limit suggestions
textBoxExt.MinimumPrefixCharacters = 3;
textBoxExt.MaximumSuggestionsCount = 15;
textBoxExt.SuggestionMode = SuggestionMode.StartsWith; // Faster than Contains
```

### User Experience

```csharp
// Flexible search for better UX
textBoxExt.SuggestionMode = SuggestionMode.Contains;
textBoxExt.IgnoreCase = true;
textBoxExt.MinimumPrefixCharacters = 2; // Not too restrictive
```

### Remote Data

```csharp
// Reduce API calls with prefix constraint
textBoxExt.MinimumPrefixCharacters = 3;
textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(300); // Debounce
```

## Complete Example

```xaml
<editors:SfTextBoxExt 
    Width="400"
    Height="40"
    Watermark="Search employees by name, email, or department..."
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SuggestionMode="Contains"
    IgnoreCase="True"
    MinimumPrefixCharacters="2"
    MaximumSuggestionsCount="10"
    SuggestionsChanged="OnSuggestionsChanged"
    AutoCompleteSource="{Binding Employees}">
    
    <editors:SfTextBoxExt.AutoCompleteItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal" Margin="5">
                <Ellipse Width="32" Height="32" Margin="0,0,8,0">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="{Binding ImagePath}"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel>
                    <TextBlock Text="{Binding Name}" FontWeight="SemiBold"/>
                    <TextBlock Text="{Binding Email}" FontSize="10" Foreground="Gray"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </editors:SfTextBoxExt.AutoCompleteItemTemplate>
    
    <editors:SfTextBoxExt.NoResultsFoundTemplate>
        <DataTemplate>
            <TextBlock Text="No employees found" 
                       Foreground="Gray" 
                       HorizontalAlignment="Center" 
                       Margin="20"/>
        </DataTemplate>
    </editors:SfTextBoxExt.NoResultsFoundTemplate>
</editors:SfTextBoxExt>
```

## Next Steps

- **Selection Handling**: Learn about multi-selection → [selection.md](selection.md)
- **Dropdown Customization**: Style the suggestion popup → [dropdown-customization.md](dropdown-customization.md)
- **Advanced Features**: Diacritic search and highlighting → [advanced-features.md](advanced-features.md)

## Resources

- **GitHub Sample**: https://github.com/SyncfusionExamples/wpf-textboxext-examples/tree/master/Samples/AutoComplete-and-filtering
- **API Documentation**: https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Controls.Input.SfTextBoxExt.html

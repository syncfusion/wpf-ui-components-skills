# Advanced Features

Advanced capabilities including diacritic search, text highlighting, custom filtering, and resource management.

## Table of Contents
- [Diacritic Sensitivity](#diacritic-sensitivity)
- [Text Highlighting](#text-highlighting)
- [Multi-Path Search](#multi-path-search)
- [Resource Management](#resource-management)

## Diacritic Sensitivity

Control whether diacritic characters (accents, umlauts) are considered during search.

### What is Diacritic Sensitivity?

Diacritics are marks added to letters (é, ü, ñ, ç). When enabled, the control allows searching international text using English characters from a standard en-US keyboard.

### IgnoreDiacritic Property

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    SuggestionMode="Contains"
    IgnoreDiacritic="True"
    SearchItemPath="Item"
    AutoCompleteSource="{Binding DiacriticCollection}"
    Width="300"
    Height="40"/>
```

```csharp
textBoxExt.IgnoreDiacritic = true;
```

**Values:**
- `true`: Ignores diacritics (default) - "cafe" matches "café"
- `false`: Respects diacritics - "cafe" doesn't match "café"

### Example: International Employee Names

**Data Model:**

```csharp
public class Employee
{
    public string Name { get; set; }
}

// ViewModel
public ObservableCollection<Employee> Employees { get; set; }

Employees = new ObservableCollection<Employee>()
{
    new Employee { Name = "François" },
    new Employee { Name = "José" },
    new Employee { Name = "Müller" },
    new Employee { Name = "Søren" },
    new Employee { Name = "Adrián" }
};
```

**With IgnoreDiacritic = true:**

```csharp
// User types "francois" → Matches "François"
// User types "jose" → Matches "José"
// User types "muller" → Matches "Müller"
```

**With IgnoreDiacritic = false:**

```csharp
// User types "francois" → No match (exact characters required)
// User types "françois" → Matches "François"
```

### Combined with Text Highlighting

```xaml
<editors:SfTextBoxExt 
    AutoCompleteMode="Suggest"
    SuggestionMode="Contains"
    IgnoreDiacritic="True"
    HighlightedTextColor="Red"
    TextHighlightMode="MultipleOccurrence"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40"/>
```

### Use Cases

- **International applications**: Support users with different keyboard layouts
- **Customer databases**: Search names with accents using standard keyboards
- **Multi-language support**: French, Spanish, German, Nordic languages
- **Accessibility**: Easier for users without international keyboards

**GitHub Sample**: https://github.com/SyncfusionExamples/wpf-textboxext-examples/tree/master/Samples/Diacritic-sensitivity

## Text Highlighting

Highlight matched or unmatched characters in suggestions for better visual clarity.

### TextHighlightMode Property

```csharp
public enum OccurrenceMode
{
    None,              // No highlighting
    FirstOccurrence,   // Highlight first match
    MultipleOccurrence, // Highlight all matches
    Unmatched          // Highlight non-matching text
}
```

### First Occurrence

Highlights the first position of matching characters.

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    TextHighlightMode="FirstOccurrence"
    HighlightedTextColor="Red"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40"/>
```

```csharp
textBoxExt.TextHighlightMode = OccurrenceMode.FirstOccurrence;
textBoxExt.HighlightedTextColor = Colors.Red;
```

**Example:**
- User types "er"
- Results: **Er**ic, Pet**er**, Ch**er**yl (only first occurrence highlighted)

### Multiple Occurrence

Highlights all matching characters (best with `SuggestionMode="Contains"`).

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SuggestionMode="Contains"
    TextHighlightMode="MultipleOccurrence"
    HighlightedTextColor="Red"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40"/>
```

```csharp
textBoxExt.TextHighlightMode = OccurrenceMode.MultipleOccurrence;
textBoxExt.SuggestionMode = SuggestionMode.Contains;
```

**Example:**
- User types "ar"
- Results: M**ar**y C**ar**pent**ar**, Ch**ar**les H**ar**ris (all occurrences highlighted)

### Unmatched Highlighting

Highlights unmatched characters to show what differs.

```xaml
<Window.Resources>
    <Style x:Key="highlightedTextStyle" TargetType="Run">
        <Setter Property="FontWeight" Value="Bold"/>
        <Setter Property="FontStyle" Value="Italic"/>
    </Style>
</Window.Resources>

<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SuggestionMode="StartsWith"
    TextHighlightMode="Unmatched"
    HighlightedTextColor="Red"
    HighlightedTextStyle="{StaticResource highlightedTextStyle}"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40"/>
```

```csharp
textBoxExt.TextHighlightMode = OccurrenceMode.Unmatched;
```

**Example:**
- User types "John"
- Results: John**son**, John**ny**, John**athan** (unmatched parts highlighted)

### HighlightedTextColor

Set the color for highlighted text.

```xaml
<editors:SfTextBoxExt 
    HighlightedTextColor="DodgerBlue"
    TextHighlightMode="MultipleOccurrence"/>
```

```csharp
textBoxExt.HighlightedTextColor = Colors.DodgerBlue;
```

### HighlightedTextStyle

Apply a style to highlighted text (target type: `Run`).

```xaml
<Window.Resources>
    <Style x:Key="HighlightStyle" TargetType="Run">
        <Setter Property="FontWeight" Value="Bold"/>
        <Setter Property="FontSize" Value="16"/>
        <Setter Property="Background" Value="Yellow"/>
    </Style>
</Window.Resources>

<editors:SfTextBoxExt 
    HighlightedTextStyle="{StaticResource HighlightStyle}"
    TextHighlightMode="FirstOccurrence"/>
```

**Important:** Style target type MUST be `Run`, not `TextBlock`.

### Highlighting Strategies by Use Case

**Product search:**
```csharp
TextHighlightMode = OccurrenceMode.MultipleOccurrence;
SuggestionMode = SuggestionMode.Contains;
// Highlights all matching parts in product names
```

**Name autocomplete:**
```csharp
TextHighlightMode = OccurrenceMode.FirstOccurrence;
SuggestionMode = SuggestionMode.StartsWith;
// Highlights beginning of names
```

**Spelling assistance:**
```csharp
TextHighlightMode = OccurrenceMode.Unmatched;
// Shows what the user needs to complete
```

**GitHub Sample**: https://github.com/SyncfusionExamples/wpf-textboxext-examples/tree/master/Samples/TextHighlightMode

## Multi-Path Search

Search across multiple properties using custom filtering.

### When to Use Multi-Path Search

- **Employee directory**: Search by name OR email OR employee ID
- **Product catalog**: Search by name OR SKU OR category
- **Contact list**: Search by first name OR last name OR phone
- **Customer database**: Search across multiple fields simultaneously

### Setup: SuggestionMode.Custom

```csharp
textBoxExt.SuggestionMode = SuggestionMode.Custom;
textBoxExt.Filter = CustomFilterMethod;
```

### Data Model

```csharp
public class Employee
{
    public int ID { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}
```

### ViewModel with Custom Filter

```csharp
public class ViewModel
{
    public ObservableCollection<Employee> Employees { get; set; }
    public ICommand ControlLoaded { get; set; }

    public ViewModel()
    {
        ControlLoaded = new DelegateCommand(Loaded);
        
        Employees = new ObservableCollection<Employee>
        {
            new Employee { ID = 1, Name = "Eric", Email = "eric@company.com" },
            new Employee { ID = 2, Name = "James", Email = "james@company.com" },
            new Employee { ID = 3, Name = "Jacob", Email = "jacob@company.com" },
            new Employee { ID = 4, Name = "Lucas", Email = "lucas@company.com" }
        };
    }

    private void Loaded(object obj)
    {
        var autocomplete = obj as SfTextBoxExt;
        if (autocomplete != null)
        {
            autocomplete.Filter = Filtering;
        }
    }

    // Custom filter: Search by ID OR Name OR Email
    public bool Filtering(string search, object item)
    {
        var model = item as Employee;
        if (model != null)
        {
            string searchLower = search.ToLower();
            
            // Match any property
            if (model.Name.ToLower().Contains(searchLower) ||
                model.ID.ToString().Contains(searchLower) ||
                model.Email.ToLower().Contains(searchLower))
            {
                return true;
            }
        }
        return false;
    }
}
```

### XAML with Multi-Path Template

```xaml
<Window.DataContext>
    <local:ViewModel />
</Window.DataContext>

<editors:SfTextBoxExt
    x:Name="autocomplete"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}"
    SearchItemPath="Name"
    SuggestionMode="Custom"
    Width="300"
    Height="40">
    
    <!-- Custom template showing multiple fields -->
    <editors:SfTextBoxExt.AutoCompleteItemTemplate>
        <DataTemplate>
            <Grid HorizontalAlignment="Stretch">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="40"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                
                <TextBlock Grid.Column="0"
                           Text="{Binding ID}"
                           Padding="2,8,0,8"
                           VerticalAlignment="Center"
                           FontFamily="Segoe UI"
                           FontSize="12"
                           Foreground="Gray"/>
                
                <StackPanel Grid.Column="1" Padding="10,8,0,8">
                    <TextBlock Text="{Binding Name}"
                               FontFamily="Segoe UI"
                               FontSize="12"
                               FontWeight="SemiBold"/>
                    <TextBlock Text="{Binding Email}"
                               FontFamily="Segoe UI"
                               FontSize="10"
                               Foreground="Gray"
                               Margin="0,2,0,0"/>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </editors:SfTextBoxExt.AutoCompleteItemTemplate>
    
    <!-- Attach filter when control loads -->
    <interaction:Interaction.Triggers>
        <interaction:EventTrigger EventName="Loaded">
            <interaction:InvokeCommandAction 
                Command="{Binding ControlLoaded}" 
                CommandParameter="{Binding ElementName=autocomplete}"/>
        </interaction:EventTrigger>
    </interaction:Interaction.Triggers>
</editors:SfTextBoxExt>
```

### Code-Behind Setup

```csharp
ViewModel viewmodel = new ViewModel();
this.DataContext = viewmodel;

SfTextBoxExt autocomplete = new SfTextBoxExt
{
    Width = 300,
    Height = 40,
    AutoCompleteMode = AutoCompleteMode.Suggest,
    AutoCompleteSource = viewmodel.Employees,
    Filter = viewmodel.Filtering,
    SearchItemPath = "Name",
    SuggestionMode = SuggestionMode.Custom
};
```

### Search Examples

**Search by ID:**
- User types "1" → Matches ID 1
- User types "10" → Matches IDs 10, 100, 101

**Search by Name:**
- User types "eric" → Matches "Eric"
- User types "ja" → Matches "James", "Jacob"

**Search by Email:**
- User types "@company" → Matches all with "@company.com"
- User types "lucas@" → Matches "lucas@company.com"

### Advanced Custom Filter Patterns

**Case-insensitive with StartsWith:**

```csharp
public bool FilteringStartsWith(string search, object item)
{
    var employee = item as Employee;
    if (employee != null)
    {
        string searchLower = search.ToLower();
        return employee.Name.ToLower().StartsWith(searchLower) ||
               employee.Email.ToLower().StartsWith(searchLower);
    }
    return false;
}
```

**Weighted search (prioritize name over other fields):**

```csharp
// Return collection with priority sorting
private ObservableCollection<Employee> FilterWithPriority(string search)
{
    var nameMatches = Employees.Where(e => 
        e.Name.ToLower().Contains(search.ToLower()));
    var emailMatches = Employees.Where(e => 
        e.Email.ToLower().Contains(search.ToLower()) && 
        !nameMatches.Contains(e));
    
    return new ObservableCollection<Employee>(
        nameMatches.Concat(emailMatches)
    );
}
```

**GitHub Sample**: https://github.com/SyncfusionExamples/wpf-textboxext-examples/tree/master/Samples/Multipath_Search

## Resource Management

### Dispose Method

Properly release resources when the control is no longer needed.

```csharp
public void Dispose()
{
    if (textBoxExt != null)
    {
        textBoxExt.Dispose();
        textBoxExt = null;
    }
}
```

### When to Call Dispose

**Window closing:**

```csharp
protected override void OnClosing(CancelEventArgs e)
{
    textBoxExt?.Dispose();
    base.OnClosing(e);
}
```

**UserControl unloading:**

```csharp
private void OnUnloaded(object sender, RoutedEventArgs e)
{
    textBoxExt?.Dispose();
}
```

**Dynamic control removal:**

```csharp
private void RemoveAutoComplete()
{
    if (textBoxExt != null)
    {
        // Unsubscribe events
        textBoxExt.SelectionChanged -= OnSelectionChanged;
        textBoxExt.SuggestionsChanged -= OnSuggestionsChanged;
        
        // Dispose
        textBoxExt.Dispose();
        
        // Remove from UI
        parentPanel.Children.Remove(textBoxExt);
        textBoxExt = null;
    }
}
```

### Best Practices

**Unsubscribe events before disposing:**

```csharp
private void CleanUp()
{
    // 1. Unsubscribe all events
    textBoxExt.SelectionChanged -= OnSelectionChanged;
    textBoxExt.SuggestionsChanged -= OnSuggestionsChanged;
    textBoxExt.SuggestionPopupOpened -= OnPopupOpened;
    textBoxExt.SuggestionPopupClosed -= OnPopupClosed;
    
    // 2. Clear data sources
    textBoxExt.AutoCompleteSource = null;
    
    // 3. Dispose
    textBoxExt.Dispose();
}
```

**IDisposable pattern:**

```csharp
public class MyViewModel : IDisposable
{
    private SfTextBoxExt textBoxExt;
    
    public void Initialize(SfTextBoxExt control)
    {
        textBoxExt = control;
        // Setup...
    }
    
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    protected virtual void Dispose(bool disposing)
    {
        if (disposing)
        {
            textBoxExt?.Dispose();
            textBoxExt = null;
        }
    }
}
```

## Performance Optimization

### Large Data Sets

**Use virtualization:**

```csharp
// Let the control handle virtualization automatically
textBoxExt.AutoCompleteSource = largeCollection; // 10,000+ items
textBoxExt.MaximumSuggestionsCount = 20; // Limit visible items
```

**Async data loading:**

```csharp
private async void OnSuggestionsChanged(object sender, EventArgs e)
{
    var searchText = textBoxExt.Text;
    if (searchText.Length >= 3)
    {
        var results = await LoadDataAsync(searchText);
        textBoxExt.AutoCompleteSource = results;
    }
}

private async Task<ObservableCollection<Employee>> LoadDataAsync(string search)
{
    return await Task.Run(() => 
        Database.SearchEmployees(search)
    );
}
```

### Debouncing

**Avoid excessive filtering:**

```csharp
private DispatcherTimer debounceTimer;

private void InitializeDebounce()
{
    debounceTimer = new DispatcherTimer
    {
        Interval = TimeSpan.FromMilliseconds(300)
    };
    debounceTimer.Tick += PerformSearch;
}

private void OnTextChanged(object sender, EventArgs e)
{
    debounceTimer.Stop();
    debounceTimer.Start();
}

private void PerformSearch(object sender, EventArgs e)
{
    debounceTimer.Stop();
    // Execute search
}
```

### Memory Management

**Clear references:**

```csharp
private void ClearData()
{
    // Clear large collections
    if (textBoxExt.AutoCompleteSource is ObservableCollection<Employee> collection)
    {
        collection.Clear();
    }
    textBoxExt.AutoCompleteSource = null;
    
    // Force garbage collection if needed
    GC.Collect();
    GC.WaitForPendingFinalizers();
    GC.Collect();
}
```

## Next Steps

- **Basic Setup**: Installation and configuration → [getting-started.md](getting-started.md)
- **Filtering**: AutoComplete modes and suggestions → [autocomplete-filtering.md](autocomplete-filtering.md)
- **Selection**: Token and delimiter modes → [selection.md](selection.md)
- **Main Guide**: Overview and navigation → [SKILL.md](../SKILL.md)

## Resources

- **API Documentation**: https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Controls.Input.SfTextBoxExt.html
- **GitHub Samples**: https://github.com/SyncfusionExamples/wpf-textboxext-examples

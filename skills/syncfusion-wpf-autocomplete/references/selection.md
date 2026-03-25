# Selection and Value Retrieval

Complete guide for implementing single and multi-selection capabilities in WPF Autocomplete (SfTextBoxExt).

## Table of Contents
- [Single Selection](#single-selection)
- [Multi-Selection Overview](#multi-selection-overview)
- [Token Mode](#token-mode)
- [Delimiter Mode](#delimiter-mode)
- [Setting and Retrieving SelectedItem](#setting-and-retrieving-selecteditem)
- [Setting and Retrieving SelectedItems](#setting-and-retrieving-selecteditems)
- [Retrieving SelectedValue](#retrieving-selectedvalue)
- [Retrieving SuggestionIndex](#retrieving-suggestionindex)
- [SelectedItemChanged Event](#selecteditemchanged-event)

## Single Selection

Single selection is the default mode where users can select one item at a time from the suggestion list.

### Configuration

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    MultiSelectMode="None"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
// Enable single selection (default)
textBoxExt.MultiSelectMode = MultiSelectMode.None;
```

### Retrieving Selected Item

```csharp
// Get the selected item
var selectedEmployee = textBoxExt.SelectedItem as Employee;

if (selectedEmployee != null)
{
    MessageBox.Show($"Selected: {selectedEmployee.Name}");
}
```

### When to Use Single Selection

- **Standard dropdowns**: User selects one option
- **Form fields**: Single value input
- **Simple searches**: One result needed
- **Category selection**: Single category choice

## Multi-Selection Overview

Multi-selection enables users to select multiple items from the suggestion list. SfTextBoxExt provides two display modes:

1. **Token Mode**: Visual tokens/chips for each selected item
2. **Delimiter Mode**: Text-based separation with delimiters

### MultiSelectMode Property

```csharp
public enum MultiSelectMode
{
    None,      // Single selection (default)
    Token,     // Multi-selection with visual tokens
    Delimiter  // Multi-selection with text delimiters
}
```

## Token Mode

Token mode displays each selected item as a visual token (chip) with a close button for easy removal.

### Basic Token Mode

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="60"
    MultiSelectMode="Token"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
textBoxExt.MultiSelectMode = MultiSelectMode.Token;
```

### TokensWrapMode Property

Control how tokens are arranged within the control:

**Wrap Mode**: Tokens wrap to next line when space runs out

```xaml
<editors:SfTextBoxExt 
    MultiSelectMode="Token"
    TokensWrapMode="Wrap"
    Width="300"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}" />
```

**None Mode**: Tokens arranged horizontally in single line

```xaml
<editors:SfTextBoxExt 
    MultiSelectMode="Token"
    TokensWrapMode="None"
    Width="300"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
// Wrap tokens to multiple lines
textBoxExt.TokensWrapMode = TokensWrapMode.Wrap;

// Keep tokens in single line (horizontal scroll)
textBoxExt.TokensWrapMode = TokensWrapMode.None;
```

### EnableAutoSize for Tokens

Automatically adjust control height based on number of token lines:

```xaml
<editors:SfTextBoxExt 
    MultiSelectMode="Token"
    TokensWrapMode="Wrap"
    EnableAutoSize="True"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}"
    Width="300" />
```

```csharp
textBoxExt.MultiSelectMode = MultiSelectMode.Token;
textBoxExt.TokensWrapMode = TokensWrapMode.Wrap;
textBoxExt.EnableAutoSize = true;
```

**Requirements:**
- `MultiSelectMode` must be `Token`
- `TokensWrapMode` must be `Wrap`
- Control height will dynamically adjust as tokens wrap

### ShowClearButton in Token Mode

Display a clear button to remove all selected tokens:

```xaml
<editors:SfTextBoxExt 
    MultiSelectMode="Token"
    ShowClearButton="True"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40" />
```

```csharp
textBoxExt.ShowClearButton = true;
```

**Note:** Clear button is only available in Token mode (`MultiSelectMode="Token"`).

### Customizing Token Appearance

Override the default token style by targeting the `TokenItem` class:

```xaml
<Window.Resources>
    <Style TargetType="editors:TokenItem">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="editors:TokenItem">
                    <Border x:Name="TokenBorder" 
                            Margin="4,4,2,4" 
                            Background="DodgerBlue" 
                            CornerRadius="12" 
                            Height="{TemplateBinding Height}">
                        <Grid Margin="2" x:Name="TokenGrid">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="*"/>
                                <ColumnDefinition Width="Auto"/>
                            </Grid.ColumnDefinitions>
                            
                            <!-- Token Text -->
                            <TextBlock Grid.Column="0"
                                      Text="{TemplateBinding Text}" 
                                      Foreground="White" 
                                      Margin="8,0,4,0" 
                                      VerticalAlignment="Center" 
                                      FontWeight="SemiBold"/>
                            
                            <!-- Close Button -->
                            <Button Grid.Column="1" 
                                   Margin="2" 
                                   x:Name="TokenCloseButton" 
                                   IsTabStop="False" 
                                   Background="White" 
                                   Width="20" 
                                   Height="20"
                                   CommandParameter="{Binding RelativeSource={RelativeSource Mode=TemplatedParent}}">
                                <Button.Content>
                                    <TextBlock Text="&#xE711;" 
                                              VerticalAlignment="Center" 
                                              FontSize="10" 
                                              Foreground="DodgerBlue" 
                                              FontFamily="Segoe MDL2 Assets"/>
                                </Button.Content>
                                <Button.Resources>
                                    <Style TargetType="{x:Type Border}">
                                        <Setter Property="CornerRadius" Value="10"/>
                                    </Style>
                                </Button.Resources>
                            </Button>
                        </Grid>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<editors:SfTextBoxExt 
    MultiSelectMode="Token"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40" />
```

### Token with Images

Display images in tokens using `ImageMemberPath`:

```xaml
<editors:SfTextBoxExt 
    MultiSelectMode="Token"
    SearchItemPath="Name"
    ImageMemberPath="ImagePath"
    TokensWrapMode="None"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40" />
```

```csharp
public class Employee
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string ImagePath { get; set; } // Path to image file
}

textBoxExt.ImageMemberPath = "ImagePath";
```

**Note:** Images in tokens only work with `MultiSelectMode="Token"`.

## Delimiter Mode

Delimiter mode separates selected items with a character (default: comma).

### Basic Delimiter Mode

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    MultiSelectMode="Delimiter"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
textBoxExt.MultiSelectMode = MultiSelectMode.Delimiter;
```

**Output:** "Eric, James, Jacob, Lucas"

### Custom Delimiter

Change the separator character:

```xaml
<editors:SfTextBoxExt 
    MultiSelectMode="Delimiter"
    Delimiter=";"
    SearchItemPath="Name"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40" />
```

```csharp
// Use semicolon as delimiter
textBoxExt.Delimiter = ";";

// Use pipe as delimiter
textBoxExt.Delimiter = "|";

// Use custom string
textBoxExt.Delimiter = " • ";
```

**Output with semicolon:** "Eric; James; Jacob; Lucas"

### When to Use Delimiter Mode

- **Email recipients**: Separated by commas or semicolons
- **Tag lists**: Simple text representation
- **Export formats**: CSV-style output
- **Compact display**: Save screen space

## Setting and Retrieving SelectedItem

For single selection (`MultiSelectMode="None"`), use the `SelectedItem` property.

### Setting SelectedItem in ViewModel

```csharp
public class EmployeeViewModel : INotifyPropertyChanged
{
    public List<Employee> Employees { get; set; }
    
    private object selectedItem;
    public object SelectedItem
    {
        get { return selectedItem; }
        set
        {
            selectedItem = value;
            OnPropertyChanged(nameof(SelectedItem));
        }
    }

    public EmployeeViewModel()
    {
        Employees = new List<Employee>
        {
            new Employee { Name = "Eric", Email = "Eric@syncfusion.com" },
            new Employee { Name = "James", Email = "James@syncfusion.com" },
            new Employee { Name = "Jacob", Email = "Jacob@syncfusion.com" }
        };
        
        // Set initial selection
        SelectedItem = Employees[0];
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

### Binding in XAML

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    MultiSelectMode="None"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SelectedItem="{Binding SelectedItem, Mode=TwoWay}"
    AutoCompleteSource="{Binding Employees}" />
```

### Retrieving SelectedItem in Code

```csharp
private void GetSelectedItem()
{
    if (textBoxExt.SelectedItem is Employee employee)
    {
        string name = employee.Name;
        string email = employee.Email;
        MessageBox.Show($"Selected: {name} ({email})");
    }
}
```

## Setting and Retrieving SelectedItems

For multi-selection modes (`Token` or `Delimiter`), use the `SelectedItems` collection.

### Setting SelectedItems in ViewModel

```csharp
public class EmployeeViewModel : INotifyPropertyChanged
{
    public List<Employee> Employees { get; set; }
    
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

    public EmployeeViewModel()
    {
        Employees = new List<Employee>
        {
            new Employee { Name = "Eric", Email = "Eric@syncfusion.com" },
            new Employee { Name = "James", Email = "James@syncfusion.com" },
            new Employee { Name = "Jacob", Email = "Jacob@syncfusion.com" },
            new Employee { Name = "Lucas", Email = "Lucas@syncfusion.com" }
        };
        
        // Initialize with pre-selected items
        SelectedItems = new ObservableCollection<object>
        {
            Employees[0],  // Eric
            Employees[2],  // Jacob
            Employees[1]   // James
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

### Binding in XAML

```xaml
<editors:SfTextBoxExt 
    Width="400"
    Height="60"
    MultiSelectMode="Token"
    TokensWrapMode="Wrap"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SelectedItems="{Binding SelectedItems, Mode=TwoWay}"
    AutoCompleteSource="{Binding Employees}" />
```

### Retrieving SelectedItems in Code

```csharp
private void GetSelectedItems()
{
    if (textBoxExt.SelectedItems != null)
    {
        var selectedNames = new List<string>();
        
        foreach (var item in textBoxExt.SelectedItems)
        {
            if (item is Employee employee)
            {
                selectedNames.Add(employee.Name);
            }
        }
        
        string result = string.Join(", ", selectedNames);
        MessageBox.Show($"Selected Items: {result}");
    }
}
```

### Programmatically Adding/Removing Items

```csharp
// Add item to selection
if (textBoxExt.SelectedItems is ObservableCollection<object> items)
{
    items.Add(newEmployee);
}

// Remove item from selection
items.Remove(employeeToRemove);

// Clear all selections
items.Clear();
```

## Retrieving SelectedValue

Get the value of the selected item using a specific property path.

### Using ValueMemberPath

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    MultiSelectMode="None"
    SearchItemPath="Name"
    ValueMemberPath="Email"
    AutoCompleteMode="Suggest"
    SelectedItemChanged="OnSelectedItemChanged"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
private void OnSelectedItemChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    var textBox = d as SfTextBoxExt;
    
    if (textBox.SelectedValue != null)
    {
        string selectedEmail = textBox.SelectedValue.ToString();
        MessageBox.Show($"Selected Email: {selectedEmail}", "SelectedValue");
    }
}
```

**How it works:**
- `SearchItemPath="Name"` → Displays employee name
- `ValueMemberPath="Email"` → Returns employee email
- User sees "Eric" but `SelectedValue` returns "Eric@syncfusion.com"

### Use Cases for SelectedValue

```csharp
// Get employee ID for database query
textBoxExt.ValueMemberPath = "EmployeeId";
int employeeId = (int)textBoxExt.SelectedValue;

// Get product SKU
textBoxExt.ValueMemberPath = "SKU";
string sku = textBoxExt.SelectedValue?.ToString();

// Get category code
textBoxExt.ValueMemberPath = "CategoryCode";
string code = textBoxExt.SelectedValue?.ToString();
```

## Retrieving SuggestionIndex

Get the zero-based index of the selected item in the suggestion list.

### Using SuggestionIndex

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SelectedItemChanged="OnSelectedItemChanged"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
private void OnSelectedItemChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    var textBox = d as SfTextBoxExt;
    
    int index = textBox.SuggestionIndex;
    MessageBox.Show($"Selected item index: {index}", "SuggestionIndex");
}
```

**Note:** Returns the index within the filtered suggestion list, not the original data source.

### Use Cases

- **Keyboard navigation**: Track user selection position
- **Analytics**: Monitor which suggestions users select
- **Ranking**: Analyze position of selected items
- **Testing**: Verify selection behavior

## SelectedItemChanged Event

Raised when the `SelectedItem` property changes, providing both old and new values.

### Event Handler

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SelectedItemChanged="OnSelectedItemChanged"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
private void OnSelectedItemChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    var textBox = d as SfTextBoxExt;
    
    // Old value
    var oldEmployee = e.OldValue as Employee;
    
    // New value
    var newEmployee = e.NewValue as Employee;
    
    if (newEmployee != null)
    {
        Console.WriteLine($"Selection changed from {oldEmployee?.Name ?? "none"} to {newEmployee.Name}");
        
        // Perform actions based on new selection
        LoadEmployeeDetails(newEmployee);
    }
}
```

### Subscribing in Code

```csharp
textBoxExt.SelectedItemChanged += OnSelectedItemChanged;
```

### Use Cases

**1. Load Related Data:**

```csharp
private void OnSelectedItemChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (e.NewValue is Employee employee)
    {
        // Load employee details
        DetailsPanel.DataContext = employee;
        
        // Load related records
        LoadEmployeeProjects(employee.Id);
    }
}
```

**2. Validation:**

```csharp
private void OnSelectedItemChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (e.NewValue is Employee employee)
    {
        if (!IsEmployeeAvailable(employee))
        {
            MessageBox.Show($"{employee.Name} is currently unavailable.");
            // Revert to old selection
            (d as SfTextBoxExt).SelectedItem = e.OldValue;
        }
    }
}
```

**3. Update UI:**

```csharp
private void OnSelectedItemChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    if (e.NewValue is Employee employee)
    {
        // Update status bar
        StatusLabel.Text = $"Selected: {employee.Name} - {employee.Department}";
        
        // Enable/disable buttons
        EditButton.IsEnabled = true;
        DeleteButton.IsEnabled = true;
    }
}
```

## Complete Examples

### Token Mode with Auto-Size

```xaml
<editors:SfTextBoxExt 
    Width="400"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    MultiSelectMode="Token"
    TokensWrapMode="Wrap"
    EnableAutoSize="True"
    ShowClearButton="True"
    AutoCompleteSource="{Binding Employees}"
    SelectedItems="{Binding SelectedEmployees, Mode=TwoWay}" />
```

### Delimiter Mode with Custom Separator

```xaml
<editors:SfTextBoxExt 
    Width="400"
    Height="40"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    MultiSelectMode="Delimiter"
    Delimiter="; "
    AutoCompleteSource="{Binding Employees}"
    SelectedItems="{Binding SelectedEmployees, Mode=TwoWay}" />
```

### Single Selection with Events

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    SearchItemPath="Name"
    ValueMemberPath="EmployeeId"
    AutoCompleteMode="Suggest"
    MultiSelectMode="None"
    SelectedItemChanged="OnSelectionChanged"
    AutoCompleteSource="{Binding Employees}"
    SelectedItem="{Binding SelectedEmployee, Mode=TwoWay}" />
```

```csharp
private void OnSelectionChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    var textBox = d as SfTextBoxExt;
    
    if (textBox.SelectedItem is Employee emp)
    {
        ResultText.Text = $"Name: {emp.Name}\n" +
                         $"Email: {emp.Email}\n" +
                         $"ID: {textBox.SelectedValue}\n" +
                         $"Index: {textBox.SuggestionIndex}";
    }
}
```

## Comparison Table

| Feature | Single Selection | Token Mode | Delimiter Mode |
|---------|-----------------|------------|----------------|
| **MultiSelectMode** | None | Token | Delimiter |
| **Visual Display** | Text only | Visual tokens/chips | Text with separators |
| **Property to Use** | SelectedItem | SelectedItems | SelectedItems |
| **Remove Items** | Clear text | Click token close button | Edit text |
| **EnableAutoSize** | N/A | Supported | N/A |
| **ShowClearButton** | No | Yes | No |
| **ImageMemberPath** | No | Yes | No |
| **Best For** | Single choice | Visual selection | Compact multi-select |

## Best Practices

### Performance

```csharp
// For large selections, use ObservableCollection
public ObservableCollection<object> SelectedItems { get; set; }

// Not List<object> which requires complete replacement
```

### User Experience

```csharp
// Token mode: Enable auto-size for better UX
textBoxExt.EnableAutoSize = true;
textBoxExt.TokensWrapMode = TokensWrapMode.Wrap;

// Provide clear button for easy reset
textBoxExt.ShowClearButton = true;
```

### Data Binding

```csharp
// Always use TwoWay binding for selection properties
SelectedItem="{Binding SelectedEmployee, Mode=TwoWay}"
SelectedItems="{Binding SelectedEmployees, Mode=TwoWay}"
```

## Next Steps

- **Dropdown Customization**: Style the suggestion popup → [dropdown-customization.md](dropdown-customization.md)
- **TextBox Customization**: Add watermarks and buttons → [textbox-customization.md](textbox-customization.md)
- **Advanced Features**: Highlighting and diacritic search → [advanced-features.md](advanced-features.md)

## Resources

- **GitHub Sample**: https://github.com/SyncfusionExamples/wpf-textboxext-examples/tree/master/Samples/Single-and-multiple-selection
- **API Documentation**: https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Controls.Input.SfTextBoxExt.html

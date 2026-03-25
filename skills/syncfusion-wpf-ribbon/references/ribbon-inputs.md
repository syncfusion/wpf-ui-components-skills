# Ribbon Input Controls

## Table of Contents
- [RibbonCheckBox](#ribboncheckbox)
- [RibbonRadioButton](#ribbonradiobutton)
- [RibbonTextBox](#ribbontextbox)
- [RibbonComboBox](#ribboncombobox)
- [RibbonListBox](#ribbonlistbox)
- [RibbonSeparator](#ribbonseparator)
- [Data Binding](#data-binding)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## RibbonCheckBox

### Basic CheckBox

```xml
<syncfusion:RibbonCheckBox Content="Show Ruler" 
                           Click="OnShowRulerClick" />
```

```csharp
private void OnShowRulerClick(object sender, EventArgs e)
{
    var checkbox = sender as RibbonCheckBox;
    if (checkbox.IsChecked == true)
    {
        ShowRuler();
    }
    else
    {
        HideRuler();
    }
}
```

### CheckBox with Default State

```xml
<syncfusion:RibbonCheckBox Content="Show Grid" IsChecked="True" />
```

### Multiple CheckBoxes in Group

```xml
<syncfusion:RibbonBar Header="View Options">
    <syncfusion:RibbonCheckBox Content="Ruler" IsChecked="True" />
    <syncfusion:RibbonCheckBox Content="Grid" IsChecked="False" />
    <syncfusion:RibbonCheckBox Content="Guidelines" IsChecked="True" />
</syncfusion:RibbonBar>
```

### CheckBox with Command Binding

```xml
<syncfusion:RibbonCheckBox Content="Show Comments" 
                           IsChecked="{Binding ShowComments}" 
                           Command="{Binding ToggleCommentsCommand}" />
```

## RibbonRadioButton

### Basic Radio Button

Use radio buttons for mutually exclusive options:

```xml
<syncfusion:RibbonBar Header="View Mode">
    <syncfusion:RibbonRadioButton Content="Normal View" GroupName="ViewMode" />
    <syncfusion:RibbonRadioButton Content="Outline View" GroupName="ViewMode" />
    <syncfusion:RibbonRadioButton Content="Draft View" GroupName="ViewMode" />
</syncfusion:RibbonBar>
```

### Pre-Selected Radio Button

```xml
<syncfusion:RibbonBar Header="Zoom Level">
    <syncfusion:RibbonRadioButton Content="75%" GroupName="Zoom" />
    <syncfusion:RibbonRadioButton Content="100%" GroupName="Zoom" IsChecked="True" />
    <syncfusion:RibbonRadioButton Content="125%" GroupName="Zoom" />
</syncfusion:RibbonBar>
```

### Radio Button with Click Handler

```xml
<syncfusion:RibbonRadioButton Content="Dark Theme" 
                              GroupName="Theme"
                              Click="OnThemeChanged" />
```

```csharp
private void OnThemeChanged(object sender, EventArgs e)
{
    var radio = sender as RibbonRadioButton;
    if (radio.IsChecked == true)
    {
        ApplyDarkTheme();
    }
}
```

### Radio Button Groups

Multiple independent groups:

```xml
<syncfusion:RibbonTab Caption="Format">
    <!-- Alignment Group -->
    <syncfusion:RibbonBar Header="Alignment">
        <syncfusion:RibbonRadioButton Content="Left" GroupName="Alignment" />
        <syncfusion:RibbonRadioButton Content="Center" GroupName="Alignment" />
        <syncfusion:RibbonRadioButton Content="Right" GroupName="Alignment" />
    </syncfusion:RibbonBar>
    
    <!-- Indentation Group -->
    <syncfusion:RibbonBar Header="Indentation">
        <syncfusion:RibbonRadioButton Content="None" GroupName="Indent" IsChecked="True" />
        <syncfusion:RibbonRadioButton Content="Small" GroupName="Indent" />
        <syncfusion:RibbonRadioButton Content="Large" GroupName="Indent" />
    </syncfusion:RibbonBar>
</syncfusion:RibbonTab>
```

## RibbonTextBox

### Basic Text Input

```xml
<syncfusion:RibbonTextBox Text="Search:" 
                          Width="200" />
```

### TextBox with Default Value

```xml
<syncfusion:RibbonTextBox Text="Untitled"
                          Width="250" />
```

### TextBox with Data Binding

```xml
<syncfusion:RibbonTextBox Text="Find:" 
                          Width="150" />
```

### TextBox with Key Press Event

```xml
<syncfusion:RibbonTextBox x:Name="SearchBox"
                          Text="Search:"
                          Width="200" />
```
```csharp
// In code-behind
public MainWindow()
{
    InitializeComponent();
    SearchBox.KeyDown += SearchBox_KeyDown;
}

private void SearchBox_KeyDown(object sender, KeyEventArgs e)
{
    if (e.Key == Key.Return)
    {
        var textBox = sender as RibbonTextBox;
        PerformSearch(textBox.Text);
    }
}
```

### Multi-Line TextBox Alternative

For multi-line input, use `RibbonItemHost` with regular `TextBox`:

```xml
<syncfusion:RibbonItemHost>
    <TextBox Width="250" Height="60" AcceptsReturn="True" TextWrapping="Wrap" />
</syncfusion:RibbonItemHost>
```

## RibbonComboBox

### Basic ComboBox

Predefined list of options:

```xml
<syncfusion:RibbonComboBox Label="Font:" Width="150">
    <syncfusion:RibbonComboBoxItem Content="Arial" />
    <syncfusion:RibbonComboBoxItem Content="Times New Roman" />
    <syncfusion:RibbonComboBoxItem Content="Courier New" />
    <syncfusion:RibbonComboBoxItem Content="Calibri" />
</syncfusion:RibbonComboBox>
```

### ComboBox with Default Selection

```xml
<syncfusion:RibbonComboBox Label="Font Size:" SelectedIndex="2">
    <syncfusion:RibbonComboBoxItem Content="10" />
    <syncfusion:RibbonComboBoxItem Content="12" />
    <syncfusion:RibbonComboBoxItem Content="14" />
    <syncfusion:RibbonComboBoxItem Content="16" />
    <syncfusion:RibbonComboBoxItem Content="18" />
</syncfusion:RibbonComboBox>
```

### ComboBox with Data Binding

```xml
<syncfusion:RibbonComboBox Label="Template:" 
                           ItemsSource="{Binding AvailableTemplates}"
                           SelectedItem="{Binding SelectedTemplate}" />
```

```csharp
public class MyViewModel
{
    public ObservableCollection<string> AvailableTemplates { get; set; }
    
    private string _selectedTemplate;
    public string SelectedTemplate
    {
        get => _selectedTemplate;
        set
        {
            if (_selectedTemplate != value)
            {
                _selectedTemplate = value;
                ApplyTemplate(value);
            }
        }
    }

    public MyViewModel()
    {
        AvailableTemplates = new ObservableCollection<string>
        {
            "Blank",
            "Professional",
            "Modern",
            "Classic"
        };
        SelectedTemplate = "Blank";
    }
}
```

### ComboBox with Selection Changed Event

```xml
<syncfusion:RibbonComboBox Label="Style:"
                           SelectionChanged="OnStyleChanged">
    <syncfusion:RibbonComboBoxItem Content="Normal" />
    <syncfusion:RibbonComboBoxItem Content="Heading" />
    <syncfusion:RibbonComboBoxItem Content="Title" />
</syncfusion:RibbonComboBox>
```

```csharp
private void OnStyleChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var selectedStyle = e.AddedItems[0].ToString();
        ApplyStyle(selectedStyle);
    }
}
```

## RibbonListBox

### Basic ListBox

```xml
<syncfusion:RibbonListBox>
    <syncfusion:RibbonListBoxItem Content="Red" />
    <syncfusion:RibbonListBoxItem Content="Green" />
    <syncfusion:RibbonListBoxItem Content="Blue" />
    <syncfusion:RibbonListBoxItem Content="Yellow" />
</syncfusion:RibbonListBox>
```

### ListBox with Multiple Selection

```xml
<syncfusion:RibbonListBox SelectionMode="Multiple">
    <syncfusion:RibbonListBoxItem Content="Background" />
    <syncfusion:RibbonListBoxItem Content="Content" IsSelected="True" />
    <syncfusion:RibbonListBoxItem Content="Foreground" />
</syncfusion:RibbonListBox>
```

### ListBox with Data Binding

```xml
<syncfusion:RibbonListBox ItemsSource="{Binding Options}"
                          SelectedItem="{Binding SelectedOption}"
                          

```csharp
public class MyViewModel
{
    public ObservableCollection<string> Options { get; set; }
    
    public MyViewModel()
    {
        Options = new ObservableCollection<string>
        {
            "Option 1",
            "Option 2",
            "Option 3"
        };
    }
}
```

## RibbonSeparator

### Basic Separator

Visual divider between control groups:

```xml
<syncfusion:RibbonBar Header="Formatting">
    <syncfusion:RibbonCheckBox Content="Bold" />
    <syncfusion:RibbonCheckBox Content="Italic" />
    <syncfusion:RibbonCheckBox Content="Underline" />
    
    <!-- Separator -->
    <syncfusion:RibbonSeparator />
    
    <syncfusion:RibbonCheckBox Content="Strikethrough" />
    <syncfusion:RibbonCheckBox Content="Subscript" />
</syncfusion:RibbonBar>
```

### Separator in Menu

```xml
<syncfusion:MenuButton Label="Format">
    <syncfusion:RibbonMenuItem Header="Bold" />
    <syncfusion:RibbonMenuItem Header="Italic" />
    
    <syncfusion:MenuItemSeparator />
    
    <syncfusion:RibbonMenuItem Header="Styles..." />
</syncfusion:MenuButton>

### MVVM Pattern with ObservableCollection

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    private ObservableCollection<string> _items;
    public ObservableCollection<string> Items
    {
        get => _items;
        set
        {
            if (_items != value)
            {
                _items = value;
                OnPropertyChanged(nameof(Items));
            }
        }
    }

    private string _selectedItem;
    public string SelectedItem
    {
        get => _selectedItem;
        set
        {
            if (_selectedItem != value)
            {
                _selectedItem = value;
                OnPropertyChanged(nameof(SelectedItem));
            }
        }
    }

    public MyViewModel()
    {
        Items = new ObservableCollection<string>
        {
            "Item 1",
            "Item 2",
            "Item 3"
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, 
            new PropertyChangedEventArgs(propertyName));
    }
}
```

```xml
<!-- In XAML -->
<syncfusion:RibbonComboBox Label="Select:" 
                           ItemsSource="{Binding Items}"
                           SelectedItem="{Binding SelectedItem}" />
```

### Two-Way Binding

Ensure changes flow both directions:

```xml
<syncfusion:RibbonTextBox Text="{Binding DocumentTitle, UpdateSourceTrigger=PropertyChanged, Mode=TwoWay}" />
```

## Common Patterns

### Pattern 1: Font Selection
```xml
<syncfusion:RibbonBar Header="Font">
    <syncfusion:RibbonComboBox Label="Font:" ItemsSource="{Binding AvailableFonts}">
        <syncfusion:RibbonComboBoxItem Content="Arial" />
        <syncfusion:RibbonComboBoxItem Content="Times New Roman" />
    </syncfusion:RibbonComboBox>
    
    <syncfusion:RibbonComboBox Label="Size:" Width="60">
        <syncfusion:RibbonComboBoxItem Content="10" />
        <syncfusion:RibbonComboBoxItem Content="12" />
    </syncfusion:RibbonComboBox>
</syncfusion:RibbonBar>
```

### Pattern 2: Options Group with CheckBoxes
```xml
<syncfusion:RibbonBar Header="Options">
    <syncfusion:RibbonCheckBox Content="Show Borders" />
    <syncfusion:RibbonCheckBox Content="Show Gridlines" />
    <syncfusion:RibbonCheckBox Content="Show Headings" />
</syncfusion:RibbonBar>
```

### Pattern 3: Mutually Exclusive Selection
```xml
<syncfusion:RibbonBar Header="Display">
    <syncfusion:RibbonRadioButton Content="Icon Only" GroupName="Display" />
    <syncfusion:RibbonRadioButton Content="Text Only" GroupName="Display" />
    <syncfusion:RibbonRadioButton Content="Icon And Text" GroupName="Display" IsChecked="True" />
</syncfusion:RibbonBar>
```

## Troubleshooting

### Issue: ComboBox not showing items
**Solution:** Ensure items are `RibbonComboBoxItem` elements, not regular `ComboBoxItem`.

### Issue: Data binding not updating
**Solution:** Verify view model implements `INotifyPropertyChanged` and raises `PropertyChanged` events.

### Issue: Radio buttons not mutually exclusive
**Solution:** Ensure all radio buttons in a group have the same `GroupName` property.

### Issue: TextBox text not updating view model
**Solution:** Use `UpdateSourceTrigger=PropertyChanged` for real-time updates.

### Issue: ListBox selection not working
**Solution:** Check that `SelectionChanged` event handler is properly wired and that binding is bi-directional with `Mode=TwoWay`.

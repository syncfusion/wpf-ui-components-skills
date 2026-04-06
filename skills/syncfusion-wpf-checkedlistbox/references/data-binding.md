# Data Binding in CheckListBox

## Overview

Data binding enables the CheckListBox to display items from a data source (like a database, service, or in-memory collection) rather than manually creating CheckListBoxItem objects. This approach is essential for MVVM pattern implementation and dynamic data scenarios.

**When to use data binding:**
- Working with collections of business objects
- Implementing MVVM architecture
- Data comes from external sources (database, API, files)
- Need to track checked state in your data model
- Items change dynamically at runtime

**Key concepts:**
- `ItemsSource` - Binds the control to a data collection
- `DisplayMemberPath` - Specifies which property to display
- `IsSelected` binding - Syncs checked state with model property
- `ItemContainerStyle` - Styles and binds item-level properties

## Basic Data Binding Setup

### Step 1: Create the Model

The model represents a single item in your list.

```csharp
public class Country
{
    # Data Binding in CheckListBox (summary)

    This file describes the recommended data-binding patterns for the CheckListBox. Extended code samples and complete model/viewmodel examples have been moved to a companion file to keep the primary reference compact.

    Essentials:
    - Use `ItemsSource` + `DisplayMemberPath` for simple lists.
    - Use `ObservableCollection<T>` and `INotifyPropertyChanged` for dynamic data and model change propagation.
    - Preferred pattern for checkbox state: bind `IsSelected` on the `ItemContainerStyle` to a boolean model property with `Mode=TwoWay` (or use `SelectedItems` for large/virtualized lists).

    For comprehensive examples (models, converters, full XAML), see: [data-binding-examples.md](data-binding-examples.md)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    
    private void UpdateRelatedProperties()
    {
        // Example: Update dependent properties
        // e.g., CanSave, TotalSelected, etc.
    }
}
```

**Benefits:**
- UI automatically updates when model changes
- Can add business logic to property setters
- Enables commanding and complex binding scenarios

## Setting DataContext in Code-Behind

Alternative to setting DataContext in XAML:

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Set DataContext to ViewModel
        this.DataContext = new ViewModel();
    }
}
```

Or set directly on the control:

```csharp
checkListBox.DataContext = new ViewModel();
checkListBox.ItemsSource = (checkListBox.DataContext as ViewModel).Countries;
checkListBox.DisplayMemberPath = "Name";
```

## Working with Bound Data

### Adding Items at Runtime

```csharp
// In ViewModel
public void AddCountry(string name, string description)
{
    Countries.Add(new Country 
    { 
        Name = name, 
        Description = description, 
        IsChecked = false 
    });
    // UI automatically updates thanks to ObservableCollection
}
```

### Removing Items

```csharp
public void RemoveCountry(Country country)
{
    Countries.Remove(country);
    // UI automatically updates
}
```

### Getting Checked Items from ViewModel

```csharp
public IEnumerable<Country> GetCheckedCountries()
{
    return Countries.Where(c => c.IsChecked);
}

public int CheckedCount => Countries.Count(c => c.IsChecked);
```

### Checking Items Programmatically

```csharp
public void CheckAll()
{
    foreach (var country in Countries)
    {
        country.IsChecked = true;
        // UI updates automatically if model implements INotifyPropertyChanged
    }
}

public void UncheckAll()
{
    foreach (var country in Countries)
    {
        country.IsChecked = false;
    }
}

public void CheckByName(string name)
{
    var country = Countries.FirstOrDefault(c => c.Name == name);
    if (country != null)
    {
        country.IsChecked = true;
    }
}
```

## Complex Binding Scenarios

### Binding Multiple Properties with DataTemplate

When you need to display more than one property:

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Countries}">
    <syncfusion:CheckListBox.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Vertical">
                <TextBlock Text="{Binding Name}" FontWeight="Bold" />
                <TextBlock Text="{Binding Description}" 
                           FontSize="10" 
                           Foreground="Gray" />
            </StackPanel>
        </DataTemplate>
    </syncfusion:CheckListBox.ItemTemplate>
    
    <syncfusion:CheckListBox.ItemContainerStyle>
        <Style TargetType="syncfusion:CheckListBoxItem">
            <Setter Property="IsSelected" Value="{Binding IsChecked, Mode=TwoWay}" />
        </Style>
    </syncfusion:CheckListBox.ItemContainerStyle>
</syncfusion:CheckListBox>
```

### Conditional Styling with DataTriggers

Style items based on data values:

```xml
<syncfusion:CheckListBox.ItemContainerStyle>
    <Style TargetType="syncfusion:CheckListBoxItem">
        <Setter Property="IsSelected" Value="{Binding IsChecked, Mode=TwoWay}" />
        <Style.Triggers>
            <DataTrigger Binding="{Binding IsImportant}" Value="True">
                <Setter Property="Foreground" Value="Red" />
                <Setter Property="FontWeight" Value="Bold" />
            </DataTrigger>
        </Style.Triggers>
    </Style>
</syncfusion:CheckListBox.ItemContainerStyle>
```

### Value Converters

Convert data values for display:

```csharp
public class BoolToYesNoConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? "Yes" : "No";
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return value.ToString() == "Yes";
    }
}
```

```xml
<Window.Resources>
    <local:BoolToYesNoConverter x:Key="BoolToYesNo" />
</Window.Resources>

<TextBlock Text="{Binding IsChecked, Converter={StaticResource BoolToYesNo}}" />
```

## Complete Working Example

**Country.cs:**
```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

namespace CheckListBoxDataBinding
{
    public class Country : INotifyPropertyChanged
    {
        private string _name;
        private bool _isChecked;

        public string Name
        {
            get => _name;
            set { _name = value; OnPropertyChanged(); }
        }

        public string Description { get; set; }

        public bool IsChecked
        {
            get => _isChecked;
            set { _isChecked = value; OnPropertyChanged(); }
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected void OnPropertyChanged([CallerMemberName] string name = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
        }
    }
}
```

**ViewModel.cs:**
```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Linq;

namespace CheckListBoxDataBinding
{
    public class ViewModel : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;
        
        private ObservableCollection<Country> _countries;

        public ObservableCollection<Country> Countries
        {
            get => _countries;
            set
            {
                _countries = value;
                OnPropertyChanged(nameof(Countries));
            }
        }

        public ViewModel()
        {
            LoadCountries();
        }

        private void LoadCountries()
        {
            Countries = new ObservableCollection<Country>
            {
                new Country { Name = "Mexico", Description = "North America", IsChecked = true },
                new Country { Name = "Canada", Description = "North America", IsChecked = true },
                new Country { Name = "Bermuda", Description = "Island", IsChecked = false },
                new Country { Name = "Belize", Description = "Central America", IsChecked = false },
                new Country { Name = "Panama", Description = "Central America", IsChecked = true }
            };
        }

        public int CheckedCount => Countries.Count(c => c.IsChecked);

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

**MainWindow.xaml:**
```xml
<Window x:Class="CheckListBoxDataBinding.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:CheckListBoxDataBinding"
        Title="Data Binding Demo" Height="450" Width="400">
    
    <Window.DataContext>
        <local:ViewModel />
    </Window.DataContext>
    
    <Grid Margin="20">
        <syncfusion:CheckListBox ItemsSource="{Binding Countries}"
                                 DisplayMemberPath="Name"
                                 IsCheckOnFirstClick="True">
            <syncfusion:CheckListBox.ItemContainerStyle>
                <Style TargetType="syncfusion:CheckListBoxItem">
                    <Setter Property="IsSelected" Value="{Binding IsChecked, Mode=TwoWay}" />
                </Style>
            </syncfusion:CheckListBox.ItemContainerStyle>
        </syncfusion:CheckListBox>
    </Grid>
</Window>
```

## Troubleshooting

**Issue: Items display but checked state doesn't sync**
- Verify `ItemContainerStyle` is set correctly
- Check `IsSelected` binding uses `Mode=TwoWay`
- Ensure model has `IsChecked` property

**Issue: Changes to model don't update UI**
- Implement `INotifyPropertyChanged` in model
- Use `ObservableCollection` for collections
- Call `OnPropertyChanged` in property setters

**Issue: DisplayMemberPath shows wrong value**
- Verify property name spelling matches exactly (case-sensitive)
- Check property is public with getter
- Try binding to different property to isolate issue

**Issue: ItemsSource binding doesn't work**
- Verify DataContext is set correctly
- Check ViewModel property name in binding
- Use Snoop or debugging to inspect DataContext

## Next Steps

- **Selection Management:** [checking-and-selection.md](checking-and-selection.md)
- **Organize Data:** [grouping-and-sorting.md](grouping-and-sorting.md)
- **Custom Appearance:** [appearance-customization.md](appearance-customization.md)

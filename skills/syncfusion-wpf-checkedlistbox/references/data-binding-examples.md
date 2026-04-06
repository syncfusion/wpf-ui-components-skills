````markdown
# Data Binding — Examples and Patterns

This file contains the more complete model/viewmodel examples, converters, and XAML snippets moved out of the main binding reference.

## Model with INotifyPropertyChanged

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class Country : INotifyPropertyChanged
{
    private string _name;
    private bool _isChecked;

    public string Name { get => _name; set { _name = value; OnPropertyChanged(); } }
    public string Description { get; set; }
    public bool IsChecked { get => _isChecked; set { _isChecked = value; OnPropertyChanged(); } }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string name = null) => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

## ViewModel with ObservableCollection

```csharp
using System.Collections.ObjectModel;

public class ViewModel
{
    public ObservableCollection<Country> Countries { get; set; }

    public ViewModel()
    {
        Countries = new ObservableCollection<Country>
        {
            new Country { Name = "Mexico", IsChecked = true },
            new Country { Name = "Canada", IsChecked = false }
        };
    }
}
```

## XAML: ItemTemplate + IsSelected binding

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Countries}" DisplayMemberPath="Name">
  <syncfusion:CheckListBox.ItemContainerStyle>
    <Style TargetType="syncfusion:CheckListBoxItem">
      <Setter Property="IsSelected" Value="{Binding IsChecked, Mode=TwoWay}" />
    </Style>
  </syncfusion:CheckListBox.ItemContainerStyle>
</syncfusion:CheckListBox>
```

## Value Converter Example

```csharp
public class BoolToYesNoConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture) => (bool)value ? "Yes" : "No";
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture) => value.ToString() == "Yes";
}
```

````

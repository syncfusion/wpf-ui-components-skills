````markdown
# Grouping and Sorting — Extended Examples

This companion file contains the detailed grouping, nested grouping, sorting, and custom grouping converter examples moved from the main summary file.

## Basic grouping in code

```csharp
var view = CollectionViewSource.GetDefaultView(Vegetables);
view.GroupDescriptions.Add(new PropertyGroupDescription("Category"));
```

## Grouping on Loaded via Behaviors

Requires `Microsoft.Xaml.Behaviors.Wpf` for `i:Interaction.Triggers`.

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Vegetables}" DisplayMemberPath="Name">
  <i:Interaction.Triggers>
    <i:EventTrigger EventName="Loaded">
      <i:InvokeCommandAction Command="{Binding LoadedCommand}" />
    </i:EventTrigger>
  </i:Interaction.Triggers>
</syncfusion:CheckListBox>
```

## Nested grouping example

```csharp
var view = CollectionViewSource.GetDefaultView(Vegetables);
view.GroupDescriptions.Add(new PropertyGroupDescription("Category"));
view.GroupDescriptions.Add(new PropertyGroupDescription("Price"));
```

## Custom grouping via IValueConverter

```csharp
public class DateConverter : IValueConverter
{
  public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
  {
    // convert date to group label
  }
  public object ConvertBack(...) => throw new NotImplementedException();
}
```

## Sorting examples

```csharp
var view = CollectionViewSource.GetDefaultView(Vegetables);
view.SortDescriptions.Add(new SortDescription("Name", ListSortDirection.Ascending));
```

````

# Header and Selection

## Table of Contents
- [Header Property](#header-property)
- [HeaderTemplate](#headertemplate)
- [SelectedItem](#selecteditem)
- [TwoWay MVVM Binding](#twoway-mvvm-binding)
- [Programmatic Selection](#programmatic-selection)
- [Gotchas](#gotchas)

---

## Header Property

`Header` sets the root label displayed at the top of the navigator — always visible regardless of how deep the user has drilled.

```xaml
<navigation:SfTreeNavigator Header="Enterprise Suite" .../>
```

```csharp
treeNavigator.Header = "Enterprise Suite";
```

**Accepts:** Any object — string, `TextBlock`, or `StackPanel`. For complex content, use `HeaderTemplate`.

---

## HeaderTemplate

Use `HeaderTemplate` to customize the root header appearance with a `DataTemplate`:

```xaml
<navigation:SfTreeNavigator Width="300" Height="400">
    <navigation:SfTreeNavigator.HeaderTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Source="/Assets/logo.png" Width="20" Height="20" Margin="0,0,8,0"/>
                <TextBlock Text="Enterprise Suite"
                           FontSize="15"
                           FontWeight="SemiBold"
                           Foreground="#1565C0"
                           VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </navigation:SfTreeNavigator.HeaderTemplate>
    ...
</navigation:SfTreeNavigator>
```

**Binding inside HeaderTemplate:**

When `DataContext` is set on the window, use `{Binding}` in the template to display bound data:

```xaml
<navigation:SfTreeNavigator Header="{Binding Title}">
    <navigation:SfTreeNavigator.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}"
                       FontSize="14"
                       FontWeight="Bold"/>
        </DataTemplate>
    </navigation:SfTreeNavigator.HeaderTemplate>
</navigation:SfTreeNavigator>
```

---

## SelectedItem

`SelectedItem` returns the currently selected `SfTreeNavigatorItem` (or data object in data-bound mode).

### Reading SelectedItem in Code

```csharp
var selected = treeNavigator.SelectedItem as SfTreeNavigatorItem;
if (selected != null)
    MessageBox.Show("Selected: " + selected.Header);
```

### Setting SelectedItem Declaratively

```xaml
<navigation:SfTreeNavigator SelectedItem="{Binding ElementName=targetItem}">
    <navigation:SfTreeNavigatorItem x:Name="targetItem" Header="Pre-selected"/>
</navigation:SfTreeNavigator>
```

---

## TwoWay MVVM Binding

Bind `SelectedItem` two-way to a ViewModel property to react to user navigation:

### ViewModel

```csharp
using System.ComponentModel;

public class TreeViewModel : INotifyPropertyChanged
{
    private object _selectedItem;
    public object SelectedItem
    {
        get => _selectedItem;
        set
        {
            _selectedItem = value;
            OnPropertyChanged(nameof(SelectedItem));
            // React to selection change
            OnSelectionChanged(value);
        }
    }

    private void OnSelectionChanged(object selected)
    {
        if (selected is TreeModel model)
            Console.WriteLine($"Navigated to: {model.Header}");
    }

    public ObservableCollection<TreeModel> Models { get; set; }

    public TreeViewModel()
    {
        Models = new ObservableCollection<TreeModel>();
        PopulateData();
    }

    // ... PopulateData() ...

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

### XAML Binding

```xaml
<navigation:SfTreeNavigator Header="Products"
                             ItemsSource="{Binding Models}"
                             SelectedItem="{Binding SelectedItem, Mode=TwoWay}">
    <navigation:SfTreeNavigator.ItemTemplate>
        <HierarchicalDataTemplate ItemsSource="{Binding Models}">
            <TextBlock Text="{Binding Header}"/>
        </HierarchicalDataTemplate>
    </navigation:SfTreeNavigator.ItemTemplate>
</navigation:SfTreeNavigator>
```

---

## Programmatic Selection

### Select an Item at Root Level

```csharp
// Declarative items
treeNavigator.SelectedItem = treeNavigator.Items[0];
```

### Select an Item in Data-Bound Mode

```csharp
// Set SelectedItem to the data object (not the container)
var vm = this.DataContext as TreeViewModel;
if (vm != null)
    vm.SelectedItem = vm.Models[1];  // Selects "Metro Studio" (index 1)
```

### Select a Nested Item

```csharp
// In declarative mode — select a grandchild
var parent = treeNavigator.Items[0] as SfTreeNavigatorItem;
if (parent != null)
    treeNavigator.SelectedItem = parent.Items[0];
```

### Pre-select on Load

Set `SelectedItem` in ViewModel constructor or `Loaded` event:

```csharp
// In ViewModel constructor
SelectedItem = Models.FirstOrDefault(m => m.Header == "Metro Studio");
```

```csharp
// In MainWindow code-behind
private void Window_Loaded(object sender, RoutedEventArgs e)
{
    treeNavigator.SelectedItem = treeNavigator.Items[0];
}
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| SelectedItem doesn't update ViewModel | Binding is one-way | Set `Mode=TwoWay` on the `SelectedItem` binding |
| Pre-selection not working | Setting `SelectedItem` before items are loaded | Set in `Loaded` event or after `ItemsSource` is populated |
| SelectedItem returns null | No item selected / user hasn't navigated yet | Check for null before casting |
| HeaderTemplate not appearing | `Header` not set as the binding source | Use `{Binding}` (not `{Binding Header}`) inside `HeaderTemplate` when `Header` is a string |
| SelectedItem is data object, not container | Data-bound mode returns model, not `SfTreeNavigatorItem` | Cast to your model type (`TreeModel`), not `SfTreeNavigatorItem` |

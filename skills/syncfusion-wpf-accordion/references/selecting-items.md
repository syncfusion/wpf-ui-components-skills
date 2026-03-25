# Selecting Items

## Table of Contents
- [SelectedIndex](#selectedindex)
- [SelectedItem](#selecteditem)
- [SelectedItems and SelectedIndices](#selecteditems-and-selectedindices)
- [IsSelected on SfAccordionItem](#isselected-on-sfaccordionitem)
- [IsLocked](#islocked)
- [SelectAll and UnselectAll](#selectall-and-unselectall)
- [Events](#events)
- [Gotchas](#gotchas)

---

## SelectedIndex

Select an item by its zero-based index. In multi-selection modes (`OneOrMore`, `ZeroOrMore`), `SelectedIndex` reflects the **most recently** selected item.

```xaml
<syncfusion:SfAccordion SelectedIndex="2" Width="400" Height="200">
    <syncfusion:SfAccordionItem Header="WPF"           Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="UWP"           Content="Essential Studio for UWP"/>
    <syncfusion:SfAccordionItem Header="WinUI"         Content="Essential Studio for WinUI"/>
    <syncfusion:SfAccordionItem Header="Windows Forms" Content="Essential Studio for Windows Forms"/>
</syncfusion:SfAccordion>
```

```csharp
accordion.SelectedIndex = 2; // expands 3rd item
```

---

## SelectedItem

Select an item by its object instance. Supports TwoWay binding with MVVM.

### XAML + ViewModel

```csharp
public class AccordionViewModel : INotifyPropertyChanged
{
    private object _selectedItem;
    public object SelectedItem
    {
        get => _selectedItem;
        set { _selectedItem = value; OnPropertyChanged(nameof(SelectedItem)); }
    }

    public ObservableCollection<AccordionData> Items { get; set; }

    public AccordionViewModel()
    {
        Items = new ObservableCollection<AccordionData>
        {
            new AccordionData { Name = "WPF",          Description = "Rich desktop framework" },
            new AccordionData { Name = "UWP",          Description = "Cross-platform Windows apps" },
            new AccordionData { Name = "WinUI",        Description = "Modern Windows UI" },
            new AccordionData { Name = "Windows Forms",Description = "Classic Windows desktop" },
        };
        SelectedItem = Items[2]; // pre-select WinUI
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}

public class AccordionData
{
    public string Name        { get; set; }
    public string Description { get; set; }
}
```

```xaml
<Window.DataContext>
    <local:AccordionViewModel/>
</Window.DataContext>

<syncfusion:SfAccordion ItemsSource="{Binding Items}"
                        SelectedItem="{Binding SelectedItem, Mode=TwoWay}"
                        Width="400" Height="250">
    <syncfusion:SfAccordion.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}"/>
        </DataTemplate>
    </syncfusion:SfAccordion.HeaderTemplate>
    <syncfusion:SfAccordion.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Description}" Margin="8"/>
        </DataTemplate>
    </syncfusion:SfAccordion.ContentTemplate>
</syncfusion:SfAccordion>
```

---

## SelectedItems and SelectedIndices

Both are **read-only** — they cannot be set via binding or code. Use them to read the current selection state.

```csharp
// Read all currently expanded items
foreach (var item in accordion.SelectedItems)
{
    var accordionItem = item as SfAccordionItem;
    Console.WriteLine(accordionItem?.Header);
}

// Read all currently expanded indices
foreach (int index in accordion.SelectedIndices)
{
    Console.WriteLine(index);
}
```

Useful inside a `SelectedItemsChanged` event handler to report multi-selection state.

---

## IsSelected on SfAccordionItem

`IsSelected=true` means the item is **expanded**. `IsSelected=false` means **collapsed**.

```xaml
<syncfusion:SfAccordion SelectionMode="ZeroOrMore">
    <syncfusion:SfAccordionItem Header="WPF"        IsSelected="True"  Content="..."/>
    <syncfusion:SfAccordionItem Header="Silverlight" IsSelected="False" Content="..."/>
    <syncfusion:SfAccordionItem Header="WinRT"       IsSelected="True"  Content="..."/>
</syncfusion:SfAccordion>
```

Bind to a CheckBox for two-way toggle:

```xaml
<CheckBox Content="WPF"
          IsChecked="{Binding ElementName=wpfItem, Path=IsSelected, Mode=TwoWay}"/>

<syncfusion:SfAccordion SelectionMode="ZeroOrMore">
    <syncfusion:SfAccordionItem x:Name="wpfItem" Header="WPF" Content="..."/>
</syncfusion:SfAccordion>
```

---

## IsLocked

`IsLocked` is **read-only**. An item is locked when the current `SelectionMode` prevents it from being collapsed (e.g., in `One` mode, the single expanded item is locked until another is selected).

```csharp
bool locked = (accordion.SelectedItem as SfAccordionItem)?.IsLocked ?? false;
```

---

## SelectAll and UnselectAll

```csharp
// Expand all items
accordion.SelectAll();

// Collapse all items
accordion.UnselectAll();
```

> **Behavior notes:**
> - `SelectAll()` in `One` / `ZeroOrOne` mode selects only the **last** item.
> - `UnselectAll()` in `One` mode has **no effect** (one item must always be selected).
> - `UnselectAll()` in `OneOrMore` mode keeps the **highest-index** item selected.

---

## Events

### SelectedItemsChanged

Fires when any item is expanded or collapsed. Event args follow `NotifyCollectionChangedEventArgs`:

| Argument | Item expanded | Item collapsed |
|---|---|---|
| `OldStartingIndex` | `-1` | Index of collapsed item |
| `OldItems` | `null` | Collapsed item instance |
| `NewStartingIndex` | `SelectedIndex` | `-1` |
| `NewItems` | `SelectedItem` | `null` |

```xaml
<syncfusion:SfAccordion x:Name="accordion"
                        SelectionMode="ZeroOrMore"
                        SelectedItemsChanged="Accordion_SelectedItemsChanged">
```

```csharp
private void Accordion_SelectedItemsChanged(object sender,
    NotifyCollectionChangedEventArgs e)
{
    if (e.NewItems != null)
        foreach (SfAccordionItem item in e.NewItems)
            Debug.WriteLine($"Expanded: {item.Header}");

    if (e.OldItems != null)
        foreach (SfAccordionItem item in e.OldItems)
            Debug.WriteLine($"Collapsed: {item.Header}");
}
```

### SelectionChanged

Same trigger as `SelectedItemsChanged`, but uses `AddedItems` / `RemovedItems` instead:

```xaml
<syncfusion:SfAccordion SelectionChanged="Accordion_SelectionChanged"/>
```

```csharp
private void Accordion_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    foreach (var item in e.AddedItems)   // newly expanded
        Debug.WriteLine($"Added: {(item as SfAccordionItem)?.Header}");
    foreach (var item in e.RemovedItems) // newly collapsed
        Debug.WriteLine($"Removed: {(item as SfAccordionItem)?.Header}");
}
```

### Per-Item Events

```xaml
<syncfusion:SfAccordionItem Header="WPF"
                             Selected="Item_Selected"
                             Unselected="Item_Unselected"/>
```

```csharp
private void Item_Selected(object sender, RoutedEventArgs e)   { /* item expanded */ }
private void Item_Unselected(object sender, RoutedEventArgs e) { /* item collapsed */ }
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `SelectedItem` binding doesn't update VM | Binding is not `TwoWay` | Add `Mode=TwoWay` to `SelectedItem` binding |
| `UnselectAll()` does nothing | `SelectionMode="One"` prevents empty selection | Change to `ZeroOrOne` or `ZeroOrMore` |
| `SelectedItems` is always empty | Checked too early before items are loaded | Read it inside `SelectedItemsChanged` event |
| `SelectedIndex` shows wrong value in multi-select | It reflects the most recently selected, not all | Use `SelectedIndices` for full list |

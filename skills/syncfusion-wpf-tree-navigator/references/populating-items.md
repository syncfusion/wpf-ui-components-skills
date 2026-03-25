# Populating SfTreeNavigator Items

## Table of Contents
- [Declarative Items](#declarative-items)
- [Data Binding with ItemsSource](#data-binding-with-itemssource)
- [Model and ViewModel Setup](#model-and-viewmodel-setup)
- [HierarchicalDataTemplate](#hierarchicaldatatemplate)
- [Enabling and Disabling Items](#enabling-and-disabling-items)

---

## Declarative Items

Use nested `SfTreeNavigatorItem` elements directly in XAML for static hierarchies:

```xaml
<navigation:SfTreeNavigator Header="Main Menu">
    <navigation:SfTreeNavigatorItem Header="Products">
        <navigation:SfTreeNavigatorItem Header="Desktop"/>
        <navigation:SfTreeNavigatorItem Header="Mobile"/>
    </navigation:SfTreeNavigatorItem>
    <navigation:SfTreeNavigatorItem Header="Support">
        <navigation:SfTreeNavigatorItem Header="Documentation"/>
        <navigation:SfTreeNavigatorItem Header="Downloads"/>
    </navigation:SfTreeNavigatorItem>
</navigation:SfTreeNavigator>
```

**Use when:** The hierarchy is fixed at design time and doesn't change at runtime.

---

## Data Binding with ItemsSource

Bind a collection to `ItemsSource` for dynamic, data-driven trees:

```xaml
<navigation:SfTreeNavigator Header="Products"
                             Width="300" Height="400"
                             ItemsSource="{Binding Models}">
    <navigation:SfTreeNavigator.ItemTemplate>
        <HierarchicalDataTemplate ItemsSource="{Binding Models}">
            <TextBlock Text="{Binding Header}"/>
        </HierarchicalDataTemplate>
    </navigation:SfTreeNavigator.ItemTemplate>
</navigation:SfTreeNavigator>
```

**Key points:**
- `ItemsSource` on `SfTreeNavigator` binds the root collection
- `HierarchicalDataTemplate` uses `ItemsSource="{Binding Models}"` — the **same property name** that holds child items on each model object
- The `TextBlock` inside the template displays the item's text

---

## Model and ViewModel Setup

### Model Class

```csharp
using System.Collections.ObjectModel;

public class TreeModel
{
    public string Header { get; set; }

    // Child items collection — same property name used in HierarchicalDataTemplate
    public ObservableCollection<TreeModel> Models { get; set; }

    public TreeModel()
    {
        Models = new ObservableCollection<TreeModel>();
    }
}
```

### ViewModel Class

```csharp
using System.Collections.ObjectModel;

public class TreeViewModel
{
    public ObservableCollection<TreeModel> Models { get; set; }

    public TreeViewModel()
    {
        Models = new ObservableCollection<TreeModel>();
        PopulateData();
    }

    private void PopulateData()
    {
        // Root items
        var winrt = new TreeModel { Header = "WinRT (XAML)" };
        winrt.Models.Add(new TreeModel { Header = "Grid" });
        winrt.Models.Add(new TreeModel { Header = "Chart" });

        var toolsItem = new TreeModel { Header = "Tools" };
        toolsItem.Models.Add(new TreeModel { Header = "TabControl" });
        toolsItem.Models.Add(new TreeModel { Header = "Docking Manager" });
        winrt.Models.Add(toolsItem);

        var metro = new TreeModel { Header = "Metro Studio" };

        Models.Add(winrt);
        Models.Add(metro);
    }
}
```

### Connecting ViewModel in XAML

```xaml
<Window.DataContext>
    <local:TreeViewModel/>
</Window.DataContext>
```

Or in code-behind:

```csharp
this.DataContext = new TreeViewModel();
```

---

## HierarchicalDataTemplate

### Full Example

```xaml
<navigation:SfTreeNavigator Header="Syncfusion Products"
                             Width="300" Height="400"
                             ItemsSource="{Binding Models}">
    <navigation:SfTreeNavigator.ItemTemplate>
        <HierarchicalDataTemplate ItemsSource="{Binding Models}">
            <TextBlock Text="{Binding Header}" FontSize="14" Margin="4,2"/>
        </HierarchicalDataTemplate>
    </navigation:SfTreeNavigator.ItemTemplate>
</navigation:SfTreeNavigator>
```

**`ItemsSource="{Binding Models}"` in the template:**
- Tells the control how to find children for each item
- Must match the **property name** on your model that holds the child collection
- If your child property is named `Children`, use `ItemsSource="{Binding Children}"`

### Custom Display Content

```xaml
<HierarchicalDataTemplate ItemsSource="{Binding Models}">
    <StackPanel Orientation="Horizontal">
        <Image Source="{Binding IconPath}" Width="16" Height="16" Margin="0,0,6,0"/>
        <TextBlock Text="{Binding Header}" VerticalAlignment="Center"/>
    </StackPanel>
</HierarchicalDataTemplate>
```

---

## Enabling and Disabling Items

Bind `IsEnabled` on `SfTreeNavigatorItem` to control interactability:

### Declarative

```xaml
<navigation:SfTreeNavigatorItem Header="Disabled Category" IsEnabled="False"/>
```

### Data-Bound

Add an `IsEnabled` property to your model:

```csharp
public bool IsEnabled { get; set; } = true;
```

```xaml
<HierarchicalDataTemplate ItemsSource="{Binding Models}">
    <TextBlock Text="{Binding Header}"/>
</HierarchicalDataTemplate>
```

Use an `ItemContainerStyle` to bind `IsEnabled`:

```xaml
<navigation:SfTreeNavigator.ItemContainerStyle>
    <Style TargetType="navigation:SfTreeNavigatorItem">
        <Setter Property="IsEnabled" Value="{Binding IsEnabled}"/>
    </Style>
</navigation:SfTreeNavigator.ItemContainerStyle>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Items don't expand | `HierarchicalDataTemplate.ItemsSource` property name mismatch | Match property name exactly: `{Binding Models}` vs `{Binding Children}` |
| Children not showing | Child collection is null | Initialize child collection in model constructor |
| Binding errors in Output | DataContext not set | Set `DataContext` before or in Window constructor |
| Root items missing | `ItemsSource` bound to wrong property | Bind to the root `ObservableCollection<TreeModel>` in ViewModel |

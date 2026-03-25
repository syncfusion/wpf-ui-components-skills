# Command Binding

The HierarchyNavigator supports MVVM command binding via its `Command` property. The command fires whenever the user selects a navigation item, passing the selected `HierarchyNavigatorItem` as the command parameter.

## Table of Contents
- [Overview](#overview)
- [DelegateCommand Implementation](#delegatecommand-implementation)
- [ViewModel Setup](#viewmodel-setup)
- [XAML Binding](#xaml-binding)
- [Accessing Item Content](#accessing-item-content)
- [Typed Generic Commands](#typed-generic-commands)
- [Common Patterns](#common-patterns)

---

## Overview

**When to use command binding:**
- You need to respond to navigation in a ViewModel (MVVM pattern)
- You want to trigger data loading when the user navigates to a new level
- You need `CanExecute` logic to restrict certain navigation actions

**Command property behavior:**
- `Command` fires when a breadcrumb item is selected (clicked, or confirmed in edit mode)
- `CommandParameter` is automatically set to the selected `HierarchyNavigatorItem` instance
- The command fires on **every selection change**, including programmatic selection

---

## DelegateCommand Implementation

WPF does not include a built-in `DelegateCommand`. Implement one or use a framework (Prism, MVVM Toolkit).

**Minimal DelegateCommand:**
```csharp
using System;
using System.Windows.Input;

public class DelegateCommand : ICommand
{
    private readonly Action<object> _execute;
    private readonly Func<object, bool> _canExecute;

    public DelegateCommand(Action<object> execute, Func<object, bool> canExecute = null)
    {
        _execute = execute ?? throw new ArgumentNullException(nameof(execute));
        _canExecute = canExecute;
    }

    public event EventHandler CanExecuteChanged
    {
        add => CommandManager.RequerySuggested += value;
        remove => CommandManager.RequerySuggested -= value;
    }

    public bool CanExecute(object parameter) => _canExecute?.Invoke(parameter) ?? true;
    
    public void Execute(object parameter) => _execute(parameter);
}
```

**Generic DelegateCommand<T> (type-safe):**
```csharp
public class DelegateCommand<T> : ICommand
{
    private readonly Action<T> _execute;
    private readonly Func<T, bool> _canExecute;

    public DelegateCommand(Action<T> execute, Func<T, bool> canExecute = null)
    {
        _execute = execute ?? throw new ArgumentNullException(nameof(execute));
        _canExecute = canExecute;
    }

    public event EventHandler CanExecuteChanged
    {
        add => CommandManager.RequerySuggested += value;
        remove => CommandManager.RequerySuggested -= value;
    }

    public bool CanExecute(object parameter) =>
        _canExecute?.Invoke((T)parameter) ?? true;

    public void Execute(object parameter) => _execute((T)parameter);
}
```

---

## ViewModel Setup

```csharp
using Syncfusion.Windows.Tools.Controls;
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Windows.Input;

public class NavigationViewModel : INotifyPropertyChanged
{
    public ObservableCollection<FileItem> Items { get; } = new();

    // Command bound to HierarchyNavigator.Command
    public ICommand SelectedItemCommand { get; }

    private string _currentPath;
    public string CurrentPath
    {
        get => _currentPath;
        set { _currentPath = value; OnPropertyChanged(nameof(CurrentPath)); }
    }

    public NavigationViewModel()
    {
        SelectedItemCommand = new DelegateCommand(OnItemSelected);
        LoadRootItems();
    }

    private void OnItemSelected(object parameter)
    {
        // parameter is the selected HierarchyNavigatorItem
        var navItem = parameter as HierarchyNavigatorItem;
        if (navItem == null) return;

        // Access the display content
        string selectedPath = navItem.Content?.ToString();
        CurrentPath = selectedPath;

        // Trigger child loading, navigation logic, etc.
        LoadChildren(selectedPath);
    }

    private void LoadRootItems()
    {
        Items.Add(new FileItem { Name = "Root", Children = new() });
        // ... populate hierarchy
    }

    private void LoadChildren(string path)
    {
        // Load sub-items for the selected path
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

---

## XAML Binding

```xml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">

    <Window.Resources>
        <HierarchicalDataTemplate x:Key="NavTemplate"
                                  ItemsSource="{Binding Children}">
            <TextBlock Text="{Binding Name}" />
        </HierarchicalDataTemplate>
    </Window.Resources>

    <Grid>
        <syncfusion:HierarchyNavigator
            x:Name="hierarchyNavigator"
            ItemsSource="{Binding Items}"
            ItemTemplate="{StaticResource NavTemplate}"
            Command="{Binding SelectedItemCommand}" />

        <!-- Display current path from ViewModel -->
        <TextBlock Text="{Binding CurrentPath}"
                   Margin="0,60,0,0" />
    </Grid>
</Window>
```

**Code-behind (set DataContext):**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        DataContext = new NavigationViewModel();
    }
}
```

---

## Accessing Item Content

The command parameter is a `HierarchyNavigatorItem`. Key members:

| Member | Type | Description |
|--------|------|-------------|
| `Content` | `object` | The display value; cast to your data type |
| `Items` | `ItemCollection` | Child items (if any) |
| `IsSelected` | `bool` | Whether this item is currently selected |
| `Level` (inferred) | — | Depth can be inferred by traversing parent chain |

**Extracting the bound data object:**
```csharp
private void OnItemSelected(object parameter)
{
    var navItem = parameter as HierarchyNavigatorItem;
    if (navItem == null) return;

    // If using declarative items: Content is a string or set value
    string name = navItem.Content?.ToString();

    // If using data-bound items: Content holds the data object
    var fileItem = navItem.DataContext as FileItem;
    if (fileItem != null)
    {
        LoadChildren(fileItem.FullPath);
    }
}
```

---

## Typed Generic Commands

When using `DelegateCommand<T>`, cast at the binding level is automatic:

```csharp
// In ViewModel — type-safe command
public ICommand SelectedItemCommand { get; } =
    new DelegateCommand<HierarchyNavigatorItem>(
        execute: item => NavigateTo(item?.Content?.ToString()),
        canExecute: item => item != null
    );

private void NavigateTo(string path)
{
    if (string.IsNullOrEmpty(path)) return;
    CurrentPath = path;
    LoadChildren(path);
}
```

---

## Common Patterns

### Pattern 1: Load children on selection
```csharp
SelectedItemCommand = new DelegateCommand(parameter =>
{
    var item = parameter as HierarchyNavigatorItem;
    var dataItem = item?.DataContext as FolderViewModel;
    if (dataItem != null && !dataItem.ChildrenLoaded)
    {
        dataItem.LoadChildren();
    }
});
```

### Pattern 2: Show progress bar while loading
```csharp
SelectedItemCommand = new DelegateCommand(async parameter =>
{
    var item = parameter as HierarchyNavigatorItem;
    hierarchyNavigator.ShowProgressBar(new TimeSpan(0, 0, 0, 0, 3000));
    
    await LoadChildrenAsync(item?.Content?.ToString());
    
    hierarchyNavigator.CancelProgressBar();
});
```
> **Note:** If accessing `hierarchyNavigator` from ViewModel, inject it or use a service/messenger pattern.

### Pattern 3: CanExecute to restrict navigation
```csharp
SelectedItemCommand = new DelegateCommand(
    execute: parameter => NavigateTo(parameter as HierarchyNavigatorItem),
    canExecute: parameter =>
    {
        var item = parameter as HierarchyNavigatorItem;
        // Only allow navigation to items that have children
        return item != null && item.Items.Count > 0;
    }
);
```

---

## Related References

- [Getting Started](getting-started.md) — Control setup
- [Populating Data](populating-data.md) — DataContext and binding setup
- [Navigation Features](navigation-features.md) — Refresh button and progress bar
- [Edit Mode and AutoComplete](edit-mode-and-autocomplete.md) — Edit mode command flow

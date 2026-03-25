# MVVM & Commands in WPF ButtonAdv

ButtonAdv fully supports the MVVM pattern via `Command` and `CommandParameter` properties.
This lets you keep click logic in a ViewModel rather than code-behind, which improves
testability and separation of concerns.

## Table of Contents
- [Command Binding Basics](#command-binding-basics)
- [CommandParameter](#commandparameter)
- [DelegateCommand Implementation](#delegatecommand-implementation)
- [Full ViewModel Example](#full-viewmodel-example)
- [CanExecute and Button Enabling](#canexecute-and-button-enabling)
- [Common Patterns](#common-patterns)

---

## Command Binding Basics

Bind the `Command` property to any `ICommand` implementation on your ViewModel.

**XAML:**
```xaml
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>

<syncfusion:ButtonAdv Label="Log in"
                      SizeMode="Large"
                      LargeIcon="Images/user-large.png"
                      Command="{Binding LoginCommand}"
                      Width="78" Height="68"/>
```

**ViewModel:**
```csharp
public class MainViewModel
{
    public ICommand LoginCommand { get; }

    public MainViewModel()
    {
        LoginCommand = new DelegateCommand<object>(ExecuteLogin);
    }

    private void ExecuteLogin(object parameter)
    {
        MessageBox.Show("Logging in...");
    }
}
```

> **Why use Command instead of Click?** Command binding lets you keep all logic in the ViewModel,
> enables `CanExecute` to auto-enable/disable the button, and makes the behavior unit-testable
> without a UI.

---

## CommandParameter

Use `CommandParameter` to pass additional data to the command handler. The parameter is received
as the argument in `Execute(object parameter)`.

```xaml
<syncfusion:ButtonAdv Label="Save Draft"
                      Command="{Binding SaveCommand}"
                      CommandParameter="draft"/>

<syncfusion:ButtonAdv Label="Save Final"
                      Command="{Binding SaveCommand}"
                      CommandParameter="final"/>
```

```csharp
private void ExecuteSave(object parameter)
{
    string saveType = parameter?.ToString();  // "draft" or "final"
    if (saveType == "draft")
        SaveAsDraft();
    else if (saveType == "final")
        SaveAsFinal();
}
```

You can also bind `CommandParameter` to a property:

```xaml
<syncfusion:ButtonAdv Label="Submit"
                      Command="{Binding SubmitCommand}"
                      CommandParameter="{Binding SelectedItem}"/>
```

---

## DelegateCommand Implementation

WPF does not include a built-in `DelegateCommand`. Implement one or use a framework like
Prism or CommunityToolkit.Mvvm. Here is a complete standalone implementation:

```csharp
using System;
using System.Windows.Input;

public class DelegateCommand<T> : ICommand
{
    private readonly Action<T> _execute;
    private readonly Predicate<T> _canExecute;
    private bool _canExecuteCache = true;

    public DelegateCommand(Action<T> execute) : this(execute, null) { }

    public DelegateCommand(Action<T> execute, Predicate<T> canExecute)
    {
        _execute    = execute    ?? throw new ArgumentNullException(nameof(execute));
        _canExecute = canExecute;
    }

    public bool CanExecute(object parameter)
    {
        if (_canExecute == null) return true;

        bool result = _canExecute((T)parameter);
        if (_canExecuteCache != result)
        {
            _canExecuteCache = result;
            RaiseCanExecuteChanged();
        }
        return _canExecuteCache;
    }

    public void Execute(object parameter) => _execute((T)parameter);

    public void RaiseCanExecuteChanged() =>
        CanExecuteChanged?.Invoke(this, EventArgs.Empty);

    public event EventHandler CanExecuteChanged;
}
```

For commands without a parameter, use `DelegateCommand<object>` and ignore the parameter:

```csharp
LoginCommand = new DelegateCommand<object>(_ => ExecuteLogin());
```

---

## Full ViewModel Example

This example shows a button that executes an action, but only when the user has enabled it
via a checkbox (simulating a permission or validation gate).

**ViewModel:**
```csharp
public class ButtonViewModel : INotifyPropertyChanged
{
    private bool _canPerformAction = true;

    public ButtonViewModel()
    {
        ClickCommand = new DelegateCommand<object>(
            execute:    OnButtonClick,
            canExecute: CanButtonClick);
    }

    public DelegateCommand<object> ClickCommand { get; }

    public bool CanPerformAction
    {
        get => _canPerformAction;
        set
        {
            _canPerformAction = value;
            ClickCommand.RaiseCanExecuteChanged();  // Re-evaluate CanExecute
            OnPropertyChanged(nameof(CanPerformAction));
        }
    }

    private bool CanButtonClick(object parameter) => CanPerformAction;

    private void OnButtonClick(object parameter)
    {
        MessageBox.Show(parameter?.ToString() ?? "Action executed");
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**XAML:**
```xaml
<Window.DataContext>
    <local:ButtonViewModel/>
</Window.DataContext>

<Grid VerticalAlignment="Center">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="220"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>

    <CheckBox Grid.Column="0"
              IsChecked="{Binding CanPerformAction}"
              Content="Enable button action"/>

    <syncfusion:ButtonAdv Grid.Column="1"
                          Label="Log in"
                          SizeMode="Large"
                          LargeIcon="Images/user-large.png"
                          Command="{Binding ClickCommand}"
                          CommandParameter="LoginAction"
                          Width="78" Height="68"/>
</Grid>
```

---

## CanExecute and Button Enabling

When `CanExecute` returns `false`, ButtonAdv automatically disables itself (grayed out).
Call `RaiseCanExecuteChanged()` whenever the condition changes to trigger re-evaluation.

```csharp
// In your ViewModel, whenever the condition changes:
public string Username
{
    get => _username;
    set
    {
        _username = value;
        LoginCommand.RaiseCanExecuteChanged();  // Re-check if button should be enabled
        OnPropertyChanged(nameof(Username));
    }
}

// CanExecute predicate
private bool CanLogin(object parameter) =>
    !string.IsNullOrWhiteSpace(Username) && !string.IsNullOrWhiteSpace(Password);
```

> **Important:** Without `RaiseCanExecuteChanged()`, the button will not update its enabled state
> even when the underlying data changes. Always call it after any property that affects `CanExecute`.

---

## Common Patterns

### Relay Command via CommunityToolkit.Mvvm

If you prefer a library approach, the `RelayCommand` from `CommunityToolkit.Mvvm` is a clean alternative:

```csharp
using CommunityToolkit.Mvvm.Input;

public partial class MainViewModel : ObservableObject
{
    [RelayCommand(CanExecute = nameof(CanSave))]
    private void Save() { /* save logic */ }

    private bool CanSave() => !string.IsNullOrEmpty(FileName);

    [ObservableProperty]
    [NotifyCanExecuteChangedFor(nameof(SaveCommand))]
    private string _fileName;
}
```

```xaml
<syncfusion:ButtonAdv Label="Save"
                      Command="{Binding SaveCommand}"/>
```

### Using Built-in WPF RoutedCommands

```xaml
<syncfusion:ButtonAdv Label="Copy"
                      Command="ApplicationCommands.Copy"/>
<syncfusion:ButtonAdv Label="Paste"
                      Command="ApplicationCommands.Paste"/>
```

These work without a ViewModel — WPF's command routing handles the execution automatically.

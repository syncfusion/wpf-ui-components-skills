# Command Binding for DropDownButtonAdv

Commands allow `DropDownMenuItem` to execute ViewModel actions when clicked, with optional enable/disable support via `CanExecute`.

## When to Use Command Binding

- MVVM architecture — no code-behind click handlers
- Need to enable/disable items based on application state
- Reusing the same command across multiple items with different `CommandParameter`
- Declarative XAML items (not data-bound) that still need ViewModel actions

---

## Setting Command on a MenuItem

Each `DropDownMenuItem` exposes `Command` and `CommandParameter`:

```xaml
<syncfusion:DropDownButtonAdv Label="Country" SizeMode="Large" LargeIcon="Images/flag.png">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left"
                                      Header="India"
                                      Command="{Binding ClickCommand}"
                                      CommandParameter="India">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="Images/india.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left"
                                      Header="France"
                                      Command="{Binding ClickCommand}"
                                      CommandParameter="France">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="Images/france.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
    </syncfusion:DropDownMenuGroup>
</syncfusion:DropDownButtonAdv>
```

Set `DataContext` on the window:

```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new DropDownViewModel();
}
```

---

## DelegateCommand Implementation

Syncfusion's `NotificationObject` doesn't include `DelegateCommand<T>`. Implement it or use a MVVM framework's `RelayCommand`:

```csharp
public class DelegateCommand<T> : ICommand
{
    private readonly Action<T> _execute;
    private readonly Predicate<T> _canExecute;
    private bool _canExecuteCache = true;

    public DelegateCommand(Action<T> execute, Predicate<T> canExecute = null)
    {
        _execute = execute;
        _canExecute = canExecute;
    }

    public bool CanExecute(object parameter)
    {
        if (_canExecute != null)
        {
            bool result = _canExecute((T)parameter);
            if (_canExecuteCache != result)
            {
                _canExecuteCache = result;
                RaiseCanExecuteChanged();
            }
        }
        return _canExecuteCache;
    }

    public void Execute(object parameter) => _execute?.Invoke((T)parameter);

    public void RaiseCanExecuteChanged() => CanExecuteChanged?.Invoke(this, EventArgs.Empty);

    public event EventHandler CanExecuteChanged;
}
```

---

## ViewModel with CanExecute Toggle

```csharp
public class DropDownViewModel : NotificationObject
{
    private bool _canPerformAction = true;

    public bool CanPerformAction
    {
        get => _canPerformAction;
        set
        {
            _canPerformAction = value;
            ClickCommand.RaiseCanExecuteChanged();
            RaisePropertyChanged(nameof(CanPerformAction));
        }
    }

    public DelegateCommand<object> ClickCommand { get; set; }

    public DropDownViewModel()
    {
        ClickCommand = new DelegateCommand<object>(OnItemClicked, CanClick);
    }

    private bool CanClick(object parameter) => CanPerformAction;

    private void OnItemClicked(object parameter)
    {
        MessageBox.Show($"{parameter} was clicked");
    }
}
```

**XAML with enable/disable toggle:**

```xaml
<Window.DataContext>
    <local:DropDownViewModel/>
</Window.DataContext>
<Grid>
    <CheckBox IsChecked="{Binding CanPerformAction}" Content="Enable menu items"/>
    <syncfusion:DropDownButtonAdv Label="Country">
        <syncfusion:DropDownMenuGroup>
            <syncfusion:DropDownMenuItem HorizontalAlignment="Left"
                                          Header="India"
                                          Command="{Binding ClickCommand}"
                                          CommandParameter="India"/>
            <syncfusion:DropDownMenuItem HorizontalAlignment="Left"
                                          Header="France"
                                          Command="{Binding ClickCommand}"
                                          CommandParameter="France"/>
        </syncfusion:DropDownMenuGroup>
    </syncfusion:DropDownButtonAdv>
</Grid>
```

When `CanPerformAction` is `false`, all items bound to `ClickCommand` are automatically grayed out (disabled).

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Command not found at runtime | DataContext not set on window | Set `DataContext` in code-behind or XAML |
| Items stay disabled permanently | `RaiseCanExecuteChanged()` not called | Always call it after changing the can-execute flag |
| `CommandParameter` is `null` | Binding path error or no DataContext | Double-check binding paths; use `x:Reference` in `DataTemplate` |
| Same command, wrong parameter | `CommandParameter` set to binding that resolves to `null` | Use `CommandParameter="{Binding .}"` for full object, or a string literal |
| `DelegateCommand<T>` cast fails | Wrong type `T` vs `CommandParameter` type | Match `T` to the actual type passed in `CommandParameter` |

# Command Binding in MenuAdv

## Table of Contents
- [Command Property](#command-property)
- [CommandParameter](#commandparameter)
- [CommandTarget](#commandtarget)
- [Built-in ApplicationCommands](#built-in-applicationcommands)
- [DelegateCommand for MVVM](#delegatecommand-for-mvvm)
- [Gotchas](#gotchas)

---

## Command Property

Bind a `MenuItemAdv` to an `ICommand` implementation using the `Command` property. The command executes when the user clicks the menu item or presses Enter while it is focused.

```xaml
<syncfusion:MenuAdv x:Name="Menu">
    <syncfusion:MenuItemAdv Header="File">
        <syncfusion:MenuItemAdv Header="Save"
                                 Command="{Binding SaveCommand}"/>
        <syncfusion:MenuItemAdv Header="Open"
                                 Command="{Binding OpenCommand}"/>
    </syncfusion:MenuItemAdv>
</syncfusion:MenuAdv>
```

The `CanExecute` method of the bound `ICommand` also controls whether the item is enabled or disabled automatically.

---

## CommandParameter

Pass a value to the command handler via `CommandParameter`:

```xaml
<syncfusion:MenuItemAdv Header="Themes">
    <syncfusion:MenuItemAdv Header="Default"
                             IsCheckable="True"
                             CheckIconType="RadioButton"
                             GroupName="Themes"
                             IsChecked="True"
                             CommandParameter="Default"
                             Command="{Binding ElementName=root, Path=ApplyThemeCommand}"/>
    <syncfusion:MenuItemAdv Header="Blend"
                             IsCheckable="True"
                             CheckIconType="RadioButton"
                             GroupName="Themes"
                             IsChecked="False"
                             CommandParameter="Blend"
                             Command="{Binding ElementName=root, Path=ApplyThemeCommand}"/>
    <syncfusion:MenuItemAdv Header="Vs2010"
                             IsCheckable="True"
                             CheckIconType="RadioButton"
                             GroupName="Themes"
                             IsChecked="False"
                             CommandParameter="Vs2010"
                             Command="{Binding ElementName=root, Path=ApplyThemeCommand}"/>
</syncfusion:MenuItemAdv>
```

```csharp
private void ApplyTheme(object parameter)
{
    string themeName = parameter as string;
    // Apply theme based on themeName
}
```

---

## CommandTarget

Route the command to a specific element using `CommandTarget`. Useful for built-in commands like Copy/Cut/Paste that operate on a target control:

```xaml
<TextBox x:Name="textbox" Width="300"/>

<syncfusion:MenuAdv>
    <syncfusion:MenuItemAdv Header="Edit">
        <syncfusion:MenuItemAdv Command="Copy"
                                 CommandTarget="{Binding ElementName=textbox}">
            <syncfusion:MenuItemAdv.Icon>
                <Image Source="Assets/CopyIcon.png" Width="15" Height="15"/>
            </syncfusion:MenuItemAdv.Icon>
        </syncfusion:MenuItemAdv>
        <syncfusion:MenuItemAdv Command="Cut"
                                 CommandTarget="{Binding ElementName=textbox}">
            <syncfusion:MenuItemAdv.Icon>
                <Image Source="Assets/CutIcon.png" Width="15" Height="15"/>
            </syncfusion:MenuItemAdv.Icon>
        </syncfusion:MenuItemAdv>
    </syncfusion:MenuItemAdv>
</syncfusion:MenuAdv>
```

`CommandTarget` sets the element that receives the routed command — the `Executed` and `CanExecute` events start routing from that element.

---

## Built-in ApplicationCommands

WPF provides built-in commands via `ApplicationCommands`. When bound to a `MenuItemAdv`, the `Header` and `InputGestureText` are auto-populated from the command:

```xaml
<syncfusion:MenuItemAdv Command="ApplicationCommands.Copy"
                         CommandTarget="{Binding ElementName=textbox}"/>
<syncfusion:MenuItemAdv Command="ApplicationCommands.Cut"
                         CommandTarget="{Binding ElementName=textbox}"/>
<syncfusion:MenuItemAdv Command="ApplicationCommands.Paste"
                         CommandTarget="{Binding ElementName=textbox}"/>
<syncfusion:MenuItemAdv Command="ApplicationCommands.Undo"
                         CommandTarget="{Binding ElementName=textbox}"/>
<syncfusion:MenuItemAdv Command="ApplicationCommands.Redo"
                         CommandTarget="{Binding ElementName=textbox}"/>
```

**Common built-in commands:** `Copy`, `Cut`, `Paste`, `Undo`, `Redo`, `New`, `Open`, `Save`, `SaveAs`, `Close`, `Print`, `Find`

---

## DelegateCommand for MVVM

Implement `ICommand` as a `DelegateCommand` for MVVM-style binding:

### DelegateCommand Implementation

```csharp
using System;
using System.Windows.Input;

public class DelegateCommand : ICommand
{
    private readonly Action<object>     _execute;
    private readonly Predicate<object>  _canExecute;

    public event EventHandler CanExecuteChanged;

    public DelegateCommand(Action<object> execute, Predicate<object> canExecute = null)
    {
        _execute    = execute    ?? throw new ArgumentNullException(nameof(execute));
        _canExecute = canExecute;
    }

    public bool CanExecute(object parameter)
        => _canExecute == null || _canExecute(parameter);

    public void Execute(object parameter)
    {
        _execute(parameter);
        RaiseCanExecuteChanged();
    }

    public void RaiseCanExecuteChanged()
        => CanExecuteChanged?.Invoke(this, EventArgs.Empty);
}
```

### ViewModel Usage

```csharp
public class MenuViewModel
{
    public ICommand SaveCommand   { get; }
    public ICommand DeleteCommand { get; }

    public MenuViewModel()
    {
        SaveCommand   = new DelegateCommand(OnSave, CanSave);
        DeleteCommand = new DelegateCommand(OnDelete);
    }

    private bool CanSave(object parameter) => true;  // or condition

    private void OnSave(object parameter)
    {
        // Save logic
    }

    private void OnDelete(object parameter)
    {
        // Delete logic
    }
}
```

### XAML Binding

```xaml
<Window.DataContext>
    <local:MenuViewModel/>
</Window.DataContext>

<syncfusion:MenuAdv>
    <syncfusion:MenuItemAdv Header="File">
        <syncfusion:MenuItemAdv Header="Save"   Command="{Binding SaveCommand}"/>
        <syncfusion:MenuItemAdv Header="Delete" Command="{Binding DeleteCommand}"/>
    </syncfusion:MenuItemAdv>
</syncfusion:MenuAdv>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Menu item always disabled | `CanExecute` returns `false` | Check `CanExecute` logic; ensure it returns `true` when appropriate |
| Command fires but nothing happens | `Execute` body is empty or throws silently | Add try/catch and debug inside `Execute` |
| `CommandTarget` not routing | Target element not in visual tree yet | Bind `CommandTarget` after the target is loaded |
| `ApplicationCommands.Copy` does nothing | No `CommandTarget` set | Set `CommandTarget="{Binding ElementName=targetControl}"` |
| ViewModel command not found | DataContext not set | Ensure `Window.DataContext` is set to the ViewModel instance |

# Command Binding in Split Button

This guide covers command binding patterns for implementing the command pattern with ICommand interface in the Split Button control.

## Table of Contents
- [Overview](#overview)
- [Command Properties](#command-properties)
- [ICommand Interface](#icommand-interface)
- [DelegateCommand Implementation](#delegatecommand-implementation)
- [Button Command Binding](#button-command-binding)
- [Dropdown Menu Item Command Binding](#dropdown-menu-item-command-binding)
- [CanExecute Logic](#canexecute-logic)
- [Complete Command Binding Example](#complete-command-binding-example)
- [Best Practices](#best-practices)

## Overview

The command and command parameter properties allow you to execute any action when clicking either the split button or the dropdown menu items. This enables clean MVVM implementation by separating UI from business logic.

**Benefits of Command Binding:**
- Decouples UI from business logic
- Supports enable/disable logic via CanExecute
- Reusable command implementations
- Testable code without UI dependencies
- MVVM pattern compliance

## Command Properties

### Command

The `Command` property accepts all commands derived from the `ICommand` interface.

**Property:** `Command`  
**Type:** `ICommand`  
**Description:** Executes when the button or menu item is clicked

**Supported Locations:**
- `SplitButtonAdv.Command` - Primary button action
- `DropDownMenuItem.Command` - Individual menu item actions

### CommandParameter

The `CommandParameter` property allows you to provide additional data required in the command handler to perform any operation.

**Property:** `CommandParameter`  
**Type:** `object`  
**Description:** Data passed to the command's Execute method

**Common Uses:**
- Pass the clicked item's data
- Pass identifiers or keys
- Pass complex objects
- Pass string values for actions

## ICommand Interface

The `ICommand` interface defines the contract for commands:

```csharp
public interface ICommand
{
    // Determines whether the command can execute
    bool CanExecute(object parameter);
    
    // Executes the command action
    void Execute(object parameter);
    
    // Notifies when CanExecute status changes
    event EventHandler CanExecuteChanged;
}
```

**Members:**
- `CanExecute` - Returns true if the command can execute; otherwise false
- `Execute` - Contains the command logic to execute
- `CanExecuteChanged` - Event fired when CanExecute status changes

## DelegateCommand Implementation

A common pattern is to create a reusable `DelegateCommand` class that implements `ICommand`:

```csharp
using System;
using System.Windows.Input;

public class DelegateCommand<T> : ICommand
{
    private Predicate<T> _canExecute;
    private Action<T> _method;
    private bool _canExecuteCache = true;

    /// <summary>
    /// Initializes a new instance of the DelegateCommand class.
    /// </summary>
    /// <param name="method">The method to execute.</param>
    public DelegateCommand(Action<T> method)
        : this(method, null)
    {
    }

    /// <summary>
    /// Initializes a new instance of the DelegateCommand class.
    /// </summary>
    /// <param name="method">The method to execute.</param>
    /// <param name="canExecute">The method that determines if command can execute.</param>
    public DelegateCommand(Action<T> method, Predicate<T> canExecute)
    {
        _method = method;
        _canExecute = canExecute;
    }

    /// <summary>
    /// Determines whether the command can execute in its current state.
    /// </summary>
    /// <param name="parameter">Data used by the command.</param>
    /// <returns>true if command can be executed; otherwise, false.</returns>
    public bool CanExecute(object parameter)
    {
        if (_canExecute != null)
        {
            bool tempCanExecute = _canExecute((T)parameter);

            if (_canExecuteCache != tempCanExecute)
            {
                _canExecuteCache = tempCanExecute;
                RaiseCanExecuteChanged();
            }
        }

        return _canExecuteCache;
    }

    /// <summary>
    /// Raises CanExecuteChanged event to notify changes in command status.
    /// </summary>
    public void RaiseCanExecuteChanged()
    {
        CanExecuteChanged?.Invoke(this, new EventArgs());
    }

    /// <summary>
    /// Defines the method to be called when the command is invoked.
    /// </summary>
    /// <param name="parameter">Data used by the command.</param>
    public void Execute(object parameter)
    {
        _method?.Invoke((T)parameter);
    }

    public event EventHandler CanExecuteChanged;
}
```

**Key Features:**
- Generic type parameter `<T>` for type-safe parameters
- Caches CanExecute result for performance
- Automatically raises CanExecuteChanged when status changes
- Null-safe implementation

## Button Command Binding

Bind a command to the primary split button action using the `Command` and `CommandParameter` properties.

**XAML Example:**

```xaml
<syncfusion:SplitButtonAdv Label="Save" 
                            SizeMode="Normal"
                            Command="{Binding SaveCommand}"
                            CommandParameter="DefaultFormat">
    <!-- Dropdown items -->
</syncfusion:SplitButtonAdv>
```

**View Model:**

```csharp
using Syncfusion.Windows.Shared;
using System.Windows;

public class DocumentViewModel : NotificationObject
{
    public DelegateCommand<object> SaveCommand { get; set; }

    public DocumentViewModel()
    {
        SaveCommand = new DelegateCommand<object>(ExecuteSave, CanExecuteSave);
    }

    private bool CanExecuteSave(object parameter)
    {
        // Return true if save operation is allowed
        return true;
    }

    private void ExecuteSave(object parameter)
    {
        string format = parameter?.ToString() ?? "Default";
        MessageBox.Show($"Saving in {format} format");
        
        // Implement save logic here
    }
}
```

**Complete Example with Enable/Disable:**

```xaml
<Window x:Class="CommandBinding.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:CommandBinding"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Window.DataContext>
        <local:DocumentViewModel/>
    </Window.DataContext>
    
    <Grid>
        <StackPanel>
            <CheckBox IsChecked="{Binding CanPerformAction}" 
                      Content="Enable save action"/>
            
            <syncfusion:SplitButtonAdv Label="Save" 
                                        SizeMode="Large"
                                        Command="{Binding SaveCommand}" 
                                        CommandParameter="Action completed">
                <syncfusion:DropDownMenuGroup>
                    <!-- Menu items -->
                </syncfusion:DropDownMenuGroup>
            </syncfusion:SplitButtonAdv>
        </StackPanel>
    </Grid>
</Window>
```

## Dropdown Menu Item Command Binding

Bind commands to individual dropdown menu items for item-specific actions.

**XAML Example:**

```xaml
<syncfusion:SplitButtonAdv Label="Country" 
                            SizeMode="Large"
                            Command="{Binding ClickCommand}" 
                            CommandParameter="Primary action">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="India" 
                                     Command="{Binding DropDownCommand}" 
                                     CommandParameter="India"/>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="France" 
                                     Command="{Binding DropDownCommand}" 
                                     CommandParameter="France"/>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="Germany" 
                                     Command="{Binding DropDownCommand}" 
                                     CommandParameter="Germany"/>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="Canada" 
                                     Command="{Binding DropDownCommand}" 
                                     CommandParameter="Canada"/>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="China" 
                                     Command="{Binding DropDownCommand}" 
                                     CommandParameter="China"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

**View Model:**

```csharp
using Syncfusion.Windows.Shared;
using System.Windows;

public class CountryViewModel : NotificationObject
{
    public DelegateCommand<object> ClickCommand { get; set; }
    public DelegateCommand<object> DropDownCommand { get; set; }

    public CountryViewModel()
    {
        ClickCommand = new DelegateCommand<object>(
            ExecuteClick, 
            CanExecuteClick);
            
        DropDownCommand = new DelegateCommand<object>(
            ExecuteDropDown, 
            CanExecuteDropDown);
    }

    private bool CanExecuteClick(object parameter)
    {
        return true;
    }

    private void ExecuteClick(object parameter)
    {
        MessageBox.Show($"Primary action: {parameter}");
    }

    private bool CanExecuteDropDown(object parameter)
    {
        return true;
    }

    private void ExecuteDropDown(object parameter)
    {
        MessageBox.Show($"Selected country: {parameter}");
    }
}
```

## CanExecute Logic

The `CanExecute` method controls whether a command can execute, enabling or disabling UI elements automatically.

**Dynamic Enable/Disable Example:**

```csharp
using Syncfusion.Windows.Shared;
using System.Windows;

public class ActionViewModel : NotificationObject
{
    private bool _canPerformAction = true;

    public bool CanPerformAction
    {
        get { return _canPerformAction; }
        set
        {
            _canPerformAction = value;
            ClickCommand.RaiseCanExecuteChanged();
            RaisePropertyChanged(nameof(CanPerformAction));
        }
    }

    public DelegateCommand<object> ClickCommand { get; set; }

    public ActionViewModel()
    {
        ClickCommand = new DelegateCommand<object>(
            ExecuteAction, 
            CanExecuteAction);
    }

    private bool CanExecuteAction(object parameter)
    {
        return CanPerformAction;
    }

    private void ExecuteAction(object parameter)
    {
        MessageBox.Show($"Action executed: {parameter}");
    }
}
```

**XAML with CheckBox Control:**

```xaml
<StackPanel>
    <CheckBox IsChecked="{Binding CanPerformAction}" 
              Content="Enable action"/>
    
    <syncfusion:SplitButtonAdv Label="Action" 
                                Command="{Binding ClickCommand}"
                                CommandParameter="Test Action"/>
</StackPanel>
```

**Behavior:**
- When `CanPerformAction` is `true`, button is enabled
- When `CanPerformAction` is `false`, button is disabled
- Toggling checkbox updates command state automatically
- `RaiseCanExecuteChanged()` triggers re-evaluation of CanExecute

## Complete Command Binding Example

**Complete XAML:**

```xaml
<Window x:Class="Split_Button_Command_Binding.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:Split_Button_Command_Binding"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Command Binding Example" Height="450" Width="800">
    <Window.DataContext>
        <local:SplitViewModel/>
    </Window.DataContext>
    
    <Grid VerticalAlignment="Center" HorizontalAlignment="Left" Margin="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="30"/>
            <RowDefinition Height="30"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <CheckBox IsChecked="{Binding CanPerformAction}" 
                  Grid.Row="0" 
                  Content="Can perform action in split button"/>
        
        <CheckBox IsChecked="{Binding CanPerformActionItem}" 
                  Grid.Row="1" 
                  Content="Can perform action in dropdown items"/>
        
        <syncfusion:SplitButtonAdv Label="Country" 
                                    SizeMode="Large"  
                                    Command="{Binding ClickCommand}" 
                                    CommandParameter="Action completed" 
                                    Grid.Row="2" 
                                    Height="72" 
                                    Width="122">
            <syncfusion:DropDownMenuGroup>
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="India" 
                                             Command="{Binding DropDownCommand}" 
                                             CommandParameter="India"/>
                
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="France" 
                                             Command="{Binding DropDownCommand}" 
                                             CommandParameter="France"/>
                
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="Germany" 
                                             Command="{Binding DropDownCommand}" 
                                             CommandParameter="Germany"/>
                
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="Canada" 
                                             Command="{Binding DropDownCommand}" 
                                             CommandParameter="Canada"/>
                
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="China" 
                                             Command="{Binding DropDownCommand}" 
                                             CommandParameter="China"/>
            </syncfusion:DropDownMenuGroup>
        </syncfusion:SplitButtonAdv>
    </Grid>
</Window>
```

**Complete View Model:**

```csharp
using Syncfusion.Windows.Shared;
using System.Windows;

namespace Split_Button_Command_Binding
{
    public class SplitViewModel : NotificationObject
    {
        private bool _canPerformAction = true;
        private bool _canPerformActionItem = true;

        public bool CanPerformAction
        {
            get { return _canPerformAction; }
            set
            {
                _canPerformAction = value;
                ClickCommand.RaiseCanExecuteChanged();
                RaisePropertyChanged(nameof(CanPerformAction));
            }
        }

        public bool CanPerformActionItem
        {
            get { return _canPerformActionItem; }
            set
            {
                _canPerformActionItem = value;
                DropDownCommand.RaiseCanExecuteChanged();
                RaisePropertyChanged(nameof(CanPerformActionItem));
            }
        }

        public DelegateCommand<object> ClickCommand { get; set; }
        public DelegateCommand<object> DropDownCommand { get; set; }

        public SplitViewModel()
        {
            ClickCommand = new DelegateCommand<object>(
                ExecuteClickAction, 
                CanExecuteClickAction);
                
            DropDownCommand = new DelegateCommand<object>(
                ExecuteDropDownAction, 
                CanExecuteDropDownAction);
        }

        private bool CanExecuteClickAction(object parameter)
        {
            return CanPerformAction;
        }

        private void ExecuteClickAction(object parameter)
        {
            MessageBox.Show(
                $"{parameter}",
                "Button Clicked",
                MessageBoxButton.OK,
                MessageBoxImage.Information);
        }

        private bool CanExecuteDropDownAction(object parameter)
        {
            return CanPerformActionItem;
        }

        private void ExecuteDropDownAction(object parameter)
        {
            MessageBox.Show(
                $"{parameter} has been selected",
                "Menu Item Clicked",
                MessageBoxButton.OK,
                MessageBoxImage.Information);
        }
    }
}
```

## Best Practices

### 1. Separate Commands for Different Actions

```csharp
// Good: Separate commands for different purposes
public DelegateCommand<object> SaveCommand { get; set; }
public DelegateCommand<object> DeleteCommand { get; set; }
public DelegateCommand<object> ExportCommand { get; set; }

// Avoid: Single command handling multiple actions
public DelegateCommand<object> GenericCommand { get; set; }
```

### 2. Always Implement CanExecute

```csharp
// Good: Provides enable/disable logic
new DelegateCommand<object>(Execute, CanExecute);

// Avoid: Missing CanExecute logic
new DelegateCommand<object>(Execute);
```

### 3. Call RaiseCanExecuteChanged When State Changes

```csharp
public bool IsEnabled
{
    get { return _isEnabled; }
    set
    {
        _isEnabled = value;
        SaveCommand.RaiseCanExecuteChanged(); // Important!
        RaisePropertyChanged(nameof(IsEnabled));
    }
}
```

### 4. Use Meaningful CommandParameters

```csharp
// Good: Descriptive parameter
CommandParameter="SaveAsPDF"

// Good: Pass entire object
CommandParameter="{Binding}"

// Avoid: Ambiguous parameters
CommandParameter="1"
```

### 5. Handle Null Parameters

```csharp
private void ExecuteAction(object parameter)
{
    if (parameter == null)
    {
        // Handle null case
        return;
    }
    
    string action = parameter.ToString();
    // Process action
}
```

### 6. Keep Execute Methods Simple

```csharp
// Good: Delegates to service/business logic
private void ExecuteSave(object parameter)
{
    _documentService.Save(parameter);
}

// Avoid: Complex logic in Execute method
private void ExecuteSave(object parameter)
{
    // 100 lines of business logic...
}
```

## Troubleshooting

**Problem:** Command not executing  
**Solution:** Verify `CanExecute` returns true and binding path is correct

**Problem:** Button not disabling when CanExecute returns false  
**Solution:** Ensure `RaiseCanExecuteChanged()` is called when state changes

**Problem:** CommandParameter not passed to Execute  
**Solution:** Check `CommandParameter` binding and verify parameter handling in Execute method

**Problem:** Memory leaks with commands  
**Solution:** Unsubscribe from events or use weak event patterns

## GitHub Sample

View the complete sample on GitHub:
- [Command Binding Sample](https://github.com/SyncfusionExamples/wpf-split-button-examples/blob/master/Samples/Command-Binding)

This sample showcases how to provide command binding for the `SplitButtonAdv` control with dynamic enable/disable functionality.

## Related Topics

- [Data Binding](data-binding.md) - Binding collections and data to split buttons
- [Dropdown Configuration](dropdown-configuration.md) - Event handling and configuration

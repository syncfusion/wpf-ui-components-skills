# Commands in WPF Smart Text Editor

## Overview

The WPF Smart Text Editor (SfSmartTextEditor) provides the `TextChangedCommand` property, which allows you to execute custom logic whenever the text content changes. This command-based approach follows the MVVM (Model-View-ViewModel) pattern and enables reactive programming, data validation, auto-save functionality, and analytics tracking.

**Key Features:**
- Command binding for text change events
- MVVM-friendly architecture
- Clean separation of concerns
- Testable business logic
- Reactive programming support

## TextChangedCommand

The `TextChangedCommand` is triggered whenever the text in the Smart Text Editor changes. This includes changes from:
- User typing
- Accepting AI suggestions
- Programmatic text updates
- Paste operations
- Text deletion

### Basic Implementation

#### XAML Binding

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:smarttexteditor="clr-namespace:Syncfusion.UI.Xaml.SmartComponents;assembly=Syncfusion.SfSmartComponents.WPF">

    <Window.DataContext>
        <local:SmartTextEditorViewModel/>
    </Window.DataContext>

    <smarttexteditor:SfSmartTextEditor 
        x:Name="smartTextEditor"
        TextChangedCommand="{Binding TextChangedCommand}" />
</Window>
```

#### ViewModel Implementation

```csharp
using System.Windows.Input;

public class SmartTextEditorViewModel
{
    public ICommand TextChangedCommand { get; set; }

    public SmartTextEditorViewModel()
    {
        TextChangedCommand = new RelayCommand(OnTextChanged);
    }

    private void OnTextChanged()
    {
        // Your custom logic here
        System.Diagnostics.Debug.WriteLine("Text changed!");
    }
}
```

### RelayCommand Implementation

If you don't have a command infrastructure, here's a simple RelayCommand implementation:

```csharp
using System;
using System.Windows.Input;

public class RelayCommand : ICommand
{
    private readonly Action _execute;
    private readonly Func<bool> _canExecute;

    public RelayCommand(Action execute, Func<bool> canExecute = null)
    {
        _execute = execute ?? throw new ArgumentNullException(nameof(execute));
        _canExecute = canExecute;
    }

    public event EventHandler CanExecuteChanged
    {
        add { CommandManager.RequerySuggested += value; }
        remove { CommandManager.RequerySuggested -= value; }
    }

    public bool CanExecute(object parameter)
    {
        return _canExecute == null || _canExecute();
    }

    public void Execute(object parameter)
    {
        _execute();
    }
}
```

## Best Practices

### 1. Use Commands for MVVM
- Prefer `TextChangedCommand` over event handlers in code-behind
- Keeps view logic separate from business logic
- Makes unit testing easier

### 2. Debounce Expensive Operations
- Use timers to delay expensive operations (API calls, file I/O)
- Prevents excessive processing on every keystroke
- Improves application performance

### 3. Avoid Circular Updates
- Set flags when updating text programmatically
- Prevents infinite loops in two-way binding scenarios
- Essential for collaborative editing or remote sync

### 4. Handle Null/Empty Text
- Always check for null or empty text in command handlers
- Provide appropriate default behavior
- Avoid null reference exceptions

### 5. Consider Performance
- Avoid heavy computations in the command handler
- Use async/await for I/O operations
- Offload work to background threads when appropriate

## Troubleshooting

### Command Not Firing
**Problem:** TextChangedCommand is not being invoked.

**Solutions:**
- Verify command is properly bound in XAML
- Ensure ViewModel is set as DataContext
- Check that command property is public and implements ICommand
- Confirm command is instantiated in ViewModel constructor

### Performance Issues
**Problem:** Application becomes slow when typing.

**Solutions:**
- Implement debouncing for expensive operations
- Move heavy processing to background threads
- Use async/await for I/O operations
- Optimize validation logic

### Memory Leaks
**Problem:** Memory usage increases over time.

**Solutions:**
- Properly dispose of timers and event subscriptions
- Limit size of undo/redo stacks
- Unsubscribe from events when ViewModel is disposed
- Use weak event patterns for long-lived subscriptions

---

**Related Topics:**
- Configure AI services: [ai-service-configuration.md](ai-service-configuration.md)
- Customize appearance: [customization.md](customization.md)
- Initial setup: [getting-started.md](getting-started.md)

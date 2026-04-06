# Troubleshooting

This file consolidates common troubleshooting steps for the Syncfusion WPF Ribbon reference materials.

## Buttons & Menus

- Issue: Button not responding to clicks
  - Solution: Check that `Click` event handler or `Command` binding is properly configured. Verify event handler method exists in code-behind and `DataContext` is set when using commands.

- Issue: Menu items not showing
  - Solution: Ensure you're using `RibbonMenuItem` inside `SplitButton` or `MenuButton`, not regular `MenuItem`.

- Issue: Icon not displaying
  - Solution: Verify image path is correct. Use `pack://application:,,,/YourAssembly;component/Resources/image.png` format for embedded resources and ensure build action is `Resource`.

- Issue: Disabled button not grayed out
  - Solution: If using `IsEnabled` binding, ensure view model property implements `INotifyPropertyChanged` and raises `PropertyChanged` events.

- Issue: Split button dropdown not opening
  - Solution: Ensure you have menu items (like `RibbonMenuItem`) inside the `SplitButton` and items are visible.

- Issue: Command not executing
  - Solution: Check that view model is set as `DataContext`. Verify `ICommand` implementation and `RelayCommand` class are available, and `CanExecute`/`RaiseCanExecuteChanged` behavior is correct.

## Input Controls

- Issue: ComboBox not showing items
  - Solution: Ensure items are `RibbonComboBoxItem` elements, not regular `ComboBoxItem`, and verify `ItemsSource` binding returns items.

- Issue: Data binding not updating
  - Solution: Verify view model implements `INotifyPropertyChanged` and raises `PropertyChanged` events for changed properties.

- Issue: Radio buttons not mutually exclusive
  - Solution: Ensure all radio buttons in a group have the same `GroupName` property.

- Issue: TextBox text not updating view model
  - Solution: Use `UpdateSourceTrigger=PropertyChanged` for real-time updates or explicitly update source in code-behind.

- Issue: ListBox selection not working
  - Solution: Check that `SelectionChanged` event handler is properly wired and that binding uses `Mode=TwoWay` if needed.

## General

- Verify project references include the Syncfusion WPF assemblies and that NuGet packages are restored.
- Confirm XAML namespace declarations: `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`.
- If encountering layout or visual issues, ensure styles/themes are loaded and resource dictionaries are included.

If an issue is not covered here, open an issue in the repository or consult Syncfusion documentation for deeper troubleshooting steps.

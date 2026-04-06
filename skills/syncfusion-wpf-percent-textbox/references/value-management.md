# Value Management and Binding (summary)

This page summarizes the common value- and binding-related behaviors for `PercentTextBox`.

- Use the `PercentValue` property for numeric value access and changes; avoid using `Text`.
- For data binding, prefer `Mode=TwoWay` with `UpdateSourceTrigger=PropertyChanged` when real-time synchronization is required.
- Implement `INotifyPropertyChanged` in view models and raise `PropertyChanged` with `new PropertyChangedEventArgs(propertyName)`.
- Handle `PercentValueChanged` or use bound properties to react to value changes (calculations, validation, UI updates).
- Use `UseNullOption` + `NullValue` to represent empty/null states; `NullValue` takes precedence over `WatermarkText`.
- `PasteMode` controls paste semantics (`Default` replaces value, `Advanced` applies cursor-aware rules).
- Spin buttons and programmatic updates respect `MinValue`/`MaxValue` limits.

For full examples and code snippets, consult the dedicated examples in the other `references/` files or contact the authoring guide.

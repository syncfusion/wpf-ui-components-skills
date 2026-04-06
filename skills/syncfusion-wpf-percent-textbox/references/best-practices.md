# Best Practices for PercentTextBox

This document consolidates common guidance used across the PercentTextBox references.

## Bindings & ViewModels
- Use `UpdateSourceTrigger=PropertyChanged` for real-time updates when the UI must reflect changes immediately.
- Implement `INotifyPropertyChanged` correctly; use `PropertyChangedEventArgs(propertyName)` when invoking `PropertyChanged`.

## Validation & UX
- Match validation mode to UX: `OnKeyPress` for strict data entry, `OnLostFocus` for flexible entry.
- When using `OnKeyPress`, set `MaxValueOnExceedMaxDigit` / `MinValueOnExceedMinDigit` explicitly to define exceed behavior.
- Provide clear Min/Max labels or tooltips so users know valid ranges.

## Formatting & Culture
- Prefer a single formatting approach per control: Culture OR NumberFormat OR dedicated properties (dedicated properties take precedence).
- Enable `GroupSeperatorEnabled` explicitly if group separators are required.

## Appearance
- Use color-coding for positive/negative/zero values, and ensure sufficient contrast for accessibility.
- Keep corner radius proportional to control height for balanced visuals.
- Provide informative tooltips for complex rules (e.g., allowed decimal places).

## Interaction
- Choose `ScrollInterval` appropriate to the use-case (precision vs speed).
- Disable mouse-wheel scrolling inside scrollable forms to avoid accidental value changes (`IsScrollingOnCircle="False"`).
- Use spin buttons (`ShowSpinButton`) for touch-friendly or discrete-value interfaces.

## Nulls & Watermarks
- Use `UseNullOption` + `NullValue` and `WatermarkText` together appropriately; `NullValue` takes precedence over watermark.

## Reuse & Maintainability
- Keep patterns small and focused; reference this consolidated page from other reference files to avoid duplication.

For examples and scenarios, see the specific reference pages under `references/`.

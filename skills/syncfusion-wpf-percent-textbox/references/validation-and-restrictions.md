# Validation and Restrictions

## Table of Contents
- [Restrict Values with Min/Max](#restrict-values-with-minmax)
- [Validation Timing](#validation-timing)
- [Exceed Limit Behavior](#exceed-limit-behavior)
- [Decimal Digit Restrictions](#decimal-digit-restrictions)
- [Read-Only Mode](#read-only-mode)

## Restrict Values with Min/Max

Constrain user input within specific boundaries using `MinValue` and `MaxValue` properties.

### Basic Min/Max Setup

```xml
<syncfusion:PercentTextBox x:Name="percentTextBox"
                        # Validation and Restrictions (summary)

                        This page summarizes key validation and restriction behaviours:

                        - Use `MinValue` and `MaxValue` to constrain input; these limits are enforced according to `MinValidation`/`MaxValidation` (e.g., `OnKeyPress` or `OnLostFocus`).
                        - `OnKeyPress` prevents invalid characters in real time; `OnLostFocus` allows temporary values corrected on blur.
                        - Configure `MaxValueOnExceedMaxDigit` / `MinValueOnExceedMinDigit` to control automatic resets when input would exceed bounds.
                        - Control decimal precision with `PercentDecimalDigits`, `MinPercentDecimalDigits`, and `MaxPercentDecimalDigits`. `PercentDecimalDigits` has highest precedence.
                        - Use `IsReadOnly` / `IsReadOnlyCaretVisible` to present non-editable values while allowing programmatic updates and optional selection.
                        - Prefer clear UI labels/tooltips to communicate valid ranges to users.

                        See `references/validation-and-restrictions.md` (this file) for this concise checklist; use the consolidated `references/best-practices.md` for recurring recommendations.

For consolidated best practices and guidance, see [references/best-practices.md](references/best-practices.md).

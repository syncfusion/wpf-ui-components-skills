# Interactive Features

# Interactive Features (summary)

This page summarizes the interactive controls and expected behaviors:

- `ScrollInterval` controls step size for arrows, wheel, spin buttons, and drag operations.
- Keyboard: Up/Down arrow keys change value by `ScrollInterval`; holding keys repeats changes.
- Mouse wheel: enabled by `IsScrollingOnCircle` (disable in scrollable forms to avoid accidental changes).
- Click-and-drag: `EnableExtendedScrolling` allows dragging (unfocused) to change values horizontally/vertically.
- Spin buttons: `ShowSpinButton` displays up/down buttons that respect `MinValue`/`MaxValue` and `ScrollInterval`.
- Range adorner: `EnableRangeAdorner` fills the control background proportionally; requires `MinValue` and `MaxValue`.
- Text selection on focus controlled by `TextSelectionOnFocus` (defaults to true for quick replacement).

Design tips:
- Pick `ScrollInterval` appropriate for precision vs speed (e.g., 0.1 for rates, 5 for coarse adjustments).
- Disable wheel scrolling on forms (`IsScrollingOnCircle="False"`) to prevent accidental edits.

See `references/interactive-features.md` (this file) for the concise checklist and `references/best-practices.md` for recurring recommendations.
- 🖱️ Mouse wheel scrolling (±5)
- 🖱️ Click and drag (±5)
- 🔼🔽 Spin buttons (±5)
- 📊 Visual progress indicator

## Best Practices

For consolidated best practices and guidance, see [references/best-practices.md](references/best-practices.md).

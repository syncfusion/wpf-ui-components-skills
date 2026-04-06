# Advanced Features — Performance & Troubleshooting

Performance optimization, common pitfalls, and troubleshooting guidance for Carousel configurations.

## Performance Optimization

Key techniques:

- **Enable Virtualization** for large collections: `EnableVirtualization="True"`.
- **Optimize item templates**: keep templates flat and minimal nesting.
- **Use appropriate `ItemsPerPage`** in CustomPath to limit visible items.
- **Disable unnecessary animations** (`EnableRotationAnimation="False"`) when performance is critical.
- **Async image loading** via an `IValueConverter` for heavy image sets.

## Common Pitfalls

- Not setting explicit size (Height/Width) may make Carousel invisible.
- Confusing `Standard` vs `CustomPath` properties (e.g., `RadiusX` only for Standard).
- `ItemsPerPage` has effect only in CustomPath mode.
- Ensure effects (`ScaleFraction`, `OpacityFraction`, skew properties) are enabled with their `*Enabled` flags.

## Troubleshooting

Short checklist:

- If items do not rotate, verify `EnableRotationAnimation` and `RotationSpeed`.
- If items overlap, increase `RadiusX`/`RadiusY` or extend the path length in CustomPath.
- If performance is poor, enable virtualization and simplify templates.

For detailed examples and path `Data` snippets (inlined path shapes), see the original `advanced-features.md` or test the provided `Path.Data` samples in a running WPF app before production use.

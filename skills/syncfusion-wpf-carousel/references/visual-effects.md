````markdown
# Visual Effects Reference

This file centralizes the shared visual-effect properties used by the Carousel control.

## Scale & Scaling
- `ScaleFraction` (double): Scale factor (0.0–1.0) applied to non-selected items. Use with `ScalingEnabled`.
- `ScalingEnabled` (bool): Enable or disable automatic scaling of non-selected items.

## Opacity
- `OpacityFraction` (double): Opacity for non-selected items (0.0–1.0). Use with `OpacityEnabled`.
- `OpacityEnabled` (bool): Enable or disable opacity adjustments.

## Skew
- `SkewAngleXFraction`, `SkewAngleYFraction` (double): Skew angles for X/Y axis.
- `SkewAngleXEnabled`, `SkewAngleYEnabled` (bool): Enable/disable skewing along each axis.

## Fraction Collections (CustomPath)
- `ScaleFractions`, `OpacityFractions`, `SkewAngleXFractions`, `SkewAngleYFractions` — use `PathFractionCollection` with `FractionValue` items to define per-position values along the path. Fraction is 0.0–1.0 along the path; Value is the parameter (scale/opacity/angle).

## Guidance
- Prefer moderate values for accessibility and performance (`ScaleFraction` 0.6–0.9, `OpacityFraction` 0.4–0.9).
- Combine scaling with opacity sparingly; extreme values reduce usability.
- For per-position control, use fraction collections to shape the appearance along the path.

For examples and usage patterns, see `getting-started.md` and `advanced-features.md`.
````

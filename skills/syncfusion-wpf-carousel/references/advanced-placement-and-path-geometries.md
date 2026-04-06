# Advanced Features — Placement, Rotation & Path Geometries

This file contains advanced placement, rotation, and path geometry examples for the Carousel control.

## Overview

Covers precise control over selected item positioning (`TopItemPosition`), rotation angle configuration for Standard mode, and advanced path geometry patterns (infinity, figure-8, spiral, zigzag, heart, circular arc, and combination paths).

## TopItemPosition - Selected Item Placement

The `TopItemPosition` property controls where the selected item appears along the custom path. Value range: `0.0` (start) to `1.0` (end). Typical center is `0.5`.

Example (selected at start):

```xml
<syncfusion:Carousel TopItemPosition="0" VisualMode="CustomPath">
    <syncfusion:Carousel.Path>
        <Path Data="M0,300 L600,300"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```

Coordinate `TopItemPosition` with fraction collections for scale/opacity to make the focused fraction visually dominant.

## Rotation Angle Configuration

Use `RotationAngle` in Standard mode to control the initial orientation (degrees). Example:

```xml
<syncfusion:Carousel RotationAngle="45" RotationSpeed="300" VisualMode="Standard"/>
```

## Path Geometry Patterns

Examples of custom-path geometries (useful in `VisualMode="CustomPath"`):

- Infinity / Figure-8 patterns using `Q` and `T` commands
- Spiral patterns built from chained quadratic segments
- Zigzag and multi-segment combinations using `L` and `Q`
- Heart and circular arc (using `A`) shapes

For concrete Path `Data` examples, see `performance-and-troubleshooting.md`.

# Appearance and Styling in WPF GridControl

## Table of Contents
- [GridStyleInfo Architecture](#gridstyleinfo-architecture)
- [Style Inheritance Hierarchy](#style-inheritance-hierarchy)
- [Base Styles](#base-styles)
- [Background and Foreground](#background-and-foreground)
- [Font and Text Orientation](#font-and-text-orientation)
- [Borders](#borders)
- [Data Formats](#data-formats)
- [PrepareRenderCell for View-Specific Overrides](#preparecrendercell-for-view-specific-overrides)

---

## GridStyleInfo Architecture

Every cell in the grid has a `GridStyleInfo` object that stores its visual and data state (background, font, cell value, cell type, format, etc.).

Two style caches exist:
- **Volatile Cell Styles** — populated via `QueryCellInfo`; weak-referenced and garbage-collected when memory is needed. Cleared with `gridControl.Model.VolatileCellStyles.Clear(cell)`.
- **Render Cell Styles** — created just before painting; discarded when a cell scrolls out of view. Access via `gridControl.GetRenderStyleInfo(row, col)`.

---

## Style Inheritance Hierarchy

When the grid needs a cell's final style, it merges from most-specific to most-general:

1. **Cell-level style** — `Model[row, col]`
2. **RowStyle** — `Model.RowStyles[row]`
3. **ColumnStyle** — `Model.ColStyles[col]`
4. **TableStyle** — `Model.TableStyle`
5. **StandardStyle** — fully-populated fallback in `BaseStylesMap["Standard"]`

Cell-level settings always win over row/column/table styles.

```csharp
// Table-level default
gridControl.Model.TableStyle.Background = Brushes.White;

// Row-level override
gridControl.Model.RowStyles[1].Background = Brushes.LightYellow;

// Cell-level override (highest priority)
gridControl.Model[1, 2].Background = Brushes.Coral;
```

---

## Base Styles

Named base styles let you define a style once and apply it to many cells. Changing the base style updates all cells that reference it.

```csharp
// Define named styles
var pink = new GridBaseStyle();
pink.Name = "PinkStyle";
pink.StyleInfo.Background = Brushes.LightPink;
pink.StyleInfo.Foreground = Brushes.Maroon;

var green = new GridBaseStyle();
green.Name = "GreenStyle";
green.StyleInfo.Background = Brushes.PaleGreen;
green.StyleInfo.Foreground = Brushes.Olive;

gridControl.Model.BaseStylesMap.Add(pink);
gridControl.Model.BaseStylesMap.Add(green);

// Apply to cells
for (int i = 1; i <= gridControl.Model.RowCount; i++)
    for (int j = 1; j <= gridControl.Model.ColumnCount; j++)
        gridControl.Model[i, j].BaseStyle = j == 2 ? "PinkStyle" : "GreenStyle";
```

---

## Background and Foreground

### Solid brush

```csharp
gridControl.Model[2, 1].Background = Brushes.Aquamarine;
gridControl.Model[2, 2].Background = Brushes.LightSteelBlue;
```

### Gradient brush

```csharp
gridControl.Model[3, 1].Background =
    new LinearGradientBrush(Colors.CornflowerBlue, Colors.White, 90.0);

gridControl.Model[3, 2].Background =
    new LinearGradientBrush(Colors.Gold, Colors.Yellow, 0.0);
```

### Foreground (text color)

```csharp
gridControl.Model[4, 1].Foreground = Brushes.DarkRed;
```

---

## Font and Text Orientation

```csharp
// Font size, weight, style
gridControl.Model[5, 1].Font.FontSize   = 14;
gridControl.Model[5, 1].Font.FontWeight = FontWeights.Bold;
gridControl.Model[5, 1].Font.FontStyle  = FontStyles.Italic;
gridControl.Model[5, 1].CellValue       = "Header Text";

// Alignment
gridControl.Model[5, 1].HorizontalAlignment = HorizontalAlignment.Center;
gridControl.Model[5, 1].VerticalAlignment   = VerticalAlignment.Center;

// Text orientation (rotation angle in degrees)
gridControl.Model[6, 1].Font.Orientation = 45;
gridControl.Model[6, 2].Font.Orientation = 90;
gridControl.Model[6, 3].Font.Orientation = 270;
// Increase row height to accommodate rotated text
gridControl.Model.RowHeights[6] = 60;
```

---

## Borders

Each cell supports independent Top, Bottom, Left, and Right border pens.

```csharp
// Basic border
gridControl.Model[7, 1].Borders.Bottom = new Pen(Brushes.Blue, 2);
gridControl.Model[7, 1].Borders.Top    = new Pen(Brushes.Red, 1);
gridControl.Model[7, 1].Borders.Left   = new Pen(Brushes.Green, 1);
gridControl.Model[7, 1].Borders.Right  = new Pen(Brushes.Purple, 3);

// Dashed border
var dashedPen = new Pen(Brushes.Gray, 1) { DashStyle = DashStyles.Dash };
gridControl.Model[8, 1].Borders.Bottom = dashedPen;
gridControl.Model[8, 1].Borders.Top    = dashedPen;

// Thick border with DashDot style
gridControl.Model[9, 1].Borders.Right = new Pen(Brushes.Chocolate, 4)
    { DashStyle = DashStyles.DashDotDot };
```

---

## Data Formats

Apply a format string to control how `CellValue` is displayed (the underlying value is unchanged).

### Numeric formats

| Format string | Example (value = Math.PI) |
|---|---|
| `"0.00"` | `3.14` |
| `"C"` | `$3.14` |
| `"###0.##%"` | `314.16%` |
| `"#0.#E+00"` | `3.1E-01` |

```csharp
gridControl.Model[10, 1].CellValue     = Math.PI;
gridControl.Model[10, 1].CellValueType = typeof(double);
gridControl.Model[10, 1].Format        = "0.00";

gridControl.Model[10, 2].CellValue     = Math.PI;
gridControl.Model[10, 2].CellValueType = typeof(double);
gridControl.Model[10, 2].Format        = "C";
```

### DateTime formats

| Format string | Example |
|---|---|
| `"d"` | `8/10/2026` |
| `"D"` | `Monday, August 10, 2026` |
| `"t"` | `7:00 AM` |
| `"s"` | `2026-08-10T07:00:15` |
| `"dddd, dd MMMM yyyy"` | `Monday, 10 August 2026` |

```csharp
gridControl.Model[11, 1].CellValue     = DateTime.Now;
gridControl.Model[11, 1].CellValueType = typeof(DateTime);
gridControl.Model[11, 1].Format        = "D";
```

### Custom FormatProvider

Implement `IFormatProvider` and `ICustomFormatter` to apply culture-specific or domain-specific formats:

```csharp
gridControl.Model[12, 1].CellValue      = Math.PI;
gridControl.Model[12, 1].Format         = "{0:ES}";
gridControl.Model[12, 1].FormatProvider = new CustomNumberFormat();

// CustomNumberFormat implements IFormatProvider + ICustomFormatter
// and converts the double using Spanish (es-ES) culture when format is "ES"
```

---

## PrepareRenderCell for View-Specific Overrides

`PrepareRenderCell` fires just before a cell is painted, from the **view** (not the model). Use it for dynamic, per-render styling — such as alternating row colors — without modifying model data.

```csharp
gridControl.PrepareRenderCell += Grid_PrepareRenderCell;

private void Grid_PrepareRenderCell(object sender, GridPrepareRenderCellEventArgs e)
{
    // Alternate row background
    if (e.Cell.RowIndex > 0 && e.Cell.RowIndex % 2 == 0)
        e.Style.Background = Brushes.AliceBlue;

    // Highlight negative numbers in red
    if (e.Style.CellValue is double d && d < 0)
        e.Style.Foreground = Brushes.Red;
}
```

> Use `PrepareRenderCell` when multiple views share the same `GridModel` but need different visual appearances, since this event fires per-view rather than per-model.

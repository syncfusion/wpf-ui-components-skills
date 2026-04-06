# Cell Types in WPF GridControl

## Table of Contents
- [Overview](#overview)
- [Setting a Cell Type](#setting-a-cell-type)
- [Basic Cell Types](#basic-cell-types)
- [Numeric and Date Cell Types](#numeric-and-date-cell-types)
- [ComboBox and DropDownList](#combobox-and-dropdownlist)
- [Special Cell Types](#special-cell-types)
- [Nested Grid Cell Type](#nested-grid-cell-type)
- [Custom Cell Types](#custom-cell-types)

---

## Overview

GridControl supports 20+ built-in cell types. Each cell type embeds a specific WPF control inside the grid cell. Set `Style.CellType` to the type's string identifier.

| Cell Type | String | Purpose |
|---|---|---|
| Header | `"Header"` | Row/column header display |
| Static | `"Static"` | Read-only, non-editable |
| CheckBox | `"CheckBox"` | Toggle boolean options |
| Button | `"Button"` | Clickable action cell |
| Image | `"ImageCell"` | Display images |
| ComboBox | `"ComboBox"` | Single-column dropdown |
| DropDownList | `"DropDownList"` | Multi-column dropdown |
| CurrencyEdit | `"CurrencyEdit"` | Monetary values |
| DateTimeEdit | `"DateTimeEdit"` | Date and time picker |
| DoubleEdit | `"DoubleEdit"` | Double-precision decimal |
| IntegerEdit | `"IntegerEdit"` | Integer-only input |
| MaskEdit | `"MaskEdit"` | Formatted text (phone, SSN) |
| PercentEdit | `"PercentEdit"` | Percentage values |
| RichTextBox | `"RichText"` | Per-character formatting |
| UpDownEdit | `"UpDownEdit"` | Spin-button numeric |
| DataTemplate | `"DataBoundTemplate"` | Custom XAML data template |
| NestedGrid | `"ScrollGrid"` | Grid inside a grid cell |

---

## Setting a Cell Type

Assign `CellType` directly or inside `QueryCellInfo`:

```csharp
// Direct assignment
gridControl.Model[2, 3].CellType = "CheckBox";

// Inside QueryCellInfo (virtual mode)
private void Grid_QueryCellInfo(object sender, GridQueryCellInfoEventArgs e)
{
    if (e.Cell.ColumnIndex == 3 && e.Cell.RowIndex > 0)
        e.Style.CellType = "CheckBox";
}
```

To apply a cell type to an entire column via column style:

```csharp
gridControl.Model.ColStyles[3].CellType = "CheckBox";
```

---

## Basic Cell Types

### Header

```csharp
gridControl.Model[0, 1].CellType = "Header";
gridControl.Model[0, 1].CellValue = "Employee Name";
```

### Static (read-only)

```csharp
gridControl.Model[1, 2].CellType = "Static";
gridControl.Model[1, 2].CellValue = "Read-only text";
```

### CheckBox

```csharp
gridControl.Model[2, 2].CellType = "CheckBox";
gridControl.Model[2, 2].CellValue = true; // pre-checked
```

### Button

```csharp
gridControl.Model[3, 2].CellType = "Button";
gridControl.Model[3, 2].CellValue = "Click Me";

gridControl.CellButtonClick += (s, e) =>
{
    MessageBox.Show($"Button clicked at row {e.RowIndex}, col {e.ColIndex}");
};
```

### Image

```csharp
var style = gridControl.Model[4, 2];
style.CellType = "ImageCell";
style.GridModel.RowHeights[4] = 80;
style.GridModel.ColumnWidths[2] = 120;

var image = new System.Windows.Controls.Image();
image.Source = new BitmapImage(new Uri(@"Images\photo.jpg", UriKind.Relative));
style.ImageList = new ObservableCollection<Image> { image };
style.ImageIndex = 0;
```

---

## Numeric and Date Cell Types

### DateTimeEdit

```csharp
gridControl.Model[5, 2].CellType = "DateTimeEdit";
gridControl.Model[5, 2].DateTimeEdit.DateTimePattern = DateTimePattern.ShortDate;
gridControl.Model[5, 2].CellValue = DateTime.Now;

// Available patterns: ShortDate, LongDate, LongTime, ShortTime,
//   FullDateTime, MonthDay, RFC1123, SortableDateTime, YearMonth
```

### IntegerEdit

```csharp
gridControl.Model[6, 2].CellType = "IntegerEdit";
gridControl.Model[6, 2].IsEditable = true;
gridControl.Model[6, 2].NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = ","
};
gridControl.Model[6, 2].CellValue = 1000;

// Set UseNullOption = true to allow empty/null value
gridControl.Model.ColStyles[2].IntegerEdit.UseNullOption = true;
```

### DoubleEdit

```csharp
gridControl.Model[7, 2].CellType = "DoubleEdit";
gridControl.Model[7, 2].NumberFormat = new NumberFormatInfo
{
    NumberGroupSeparator = ",",
    NumberDecimalSeparator = ".",
    NumberDecimalDigits = 2
};
gridControl.Model[7, 2].CellValue = 1234.56;
```

### CurrencyEdit

```csharp
gridControl.Model[8, 2].CellType = "CurrencyEdit";
gridControl.Model[8, 2].IsEditable = true;
gridControl.Model[8, 2].NumberFormat = new NumberFormatInfo
{
    CurrencyDecimalDigits = 2,
    CurrencyDecimalSeparator = ".",
    CurrencySymbol = "$",
    CurrencyNegativePattern = 0,
    CurrencyPositivePattern = 0
};
gridControl.Model[8, 2].CellValue = 1999.99;
```

### PercentEdit

```csharp
gridControl.Model[9, 2].CellType = "PercentEdit";
gridControl.Model[9, 2].NumberFormat = new NumberFormatInfo
{
    PercentSymbol = "%",
    PercentDecimalDigits = 2,
    PercentGroupSeparator = ","
};
gridControl.Model[9, 2].PercentEditMode = PercentEditMode.PercentMode;
gridControl.Model[9, 2].CellValue = 75;
```

### MaskEdit (formatted input)

```csharp
// Phone number format
gridControl.Model[10, 2].CellType = "MaskEdit";
gridControl.Model[10, 2].MaskEdit = GridMaskEditInfo.Default;
gridControl.Model[10, 2].MaskEdit.Mask = "(000) 000-0000";

// Date format
gridControl.Model[11, 2].CellType = "MaskEdit";
gridControl.Model[11, 2].MaskEdit = GridMaskEditInfo.Default;
gridControl.Model[11, 2].MaskEdit.Mask = "00/00/0000";
```

### UpDownEdit (spinner)

```csharp
var style = gridControl.Model[12, 2];
style.CellType = "UpDownEdit";
style.UpDownEdit.MinValue = 0;
style.UpDownEdit.MaxValue = 100;
style.UpDownEdit.FocusedBackground = Brushes.LightYellow;
style.CellValue = 50;
```

---

## ComboBox and DropDownList

### ComboBox — using ChoiceList

```csharp
var choices = new StringCollection { "Option A", "Option B", "Option C" };

var style = gridControl.Model[2, 3];
style.CellType = "ComboBox";
style.ChoiceList = choices;
// DropDownStyles: Editable, AutoComplete, Exclusive
style.DropDownStyle = GridDropDownStyle.Exclusive;
```

### ComboBox — using ItemsSource (data-bound)

```csharp
var style = gridControl.Model[3, 3];
style.CellType = "ComboBox";
style.ItemsSource = employees.Select(e => e.FirstName).ToList();
style.DisplayMember = "FirstName";
style.ValueMember = "EmployeeID";
style.DropDownStyle = GridDropDownStyle.AutoComplete;
```

### DropDownList (multi-column dropdown)

```csharp
var style = gridControl.Model[4, 3];
style.CellType = "DropDownList";
style.ItemsSource = employees.Select(e => new
{
    e.EmployeeID,
    e.FirstName,
    e.LastName
}).ToList();
style.DisplayMember = "FirstName";
style.ValueMember = "EmployeeID";
style.DropDownStyle = GridDropDownStyle.Exclusive;
```

---

## Special Cell Types

### RichText

Cell value must be a `FlowDocument`:

```csharp
var doc = new FlowDocument();
var para = new Paragraph();
para.Inlines.Add(new Run("Bold text") { FontWeight = FontWeights.Bold });
para.Inlines.Add(new Run(" and ") );
para.Inlines.Add(new Run("green text") { Foreground = Brushes.Green });
doc.Blocks.Add(para);

gridControl.Model[5, 2].CellType = "RichText";
gridControl.Model[5, 2].CellValue = doc;
```

### DataBoundTemplate

Define a `DataTemplate` in XAML resources, then reference it by key:

```xml
<Window.Resources>
    <DataTemplate x:Key="StarTemplate">
        <TextBlock Text="{Binding CellBoundValue}" Foreground="Gold" FontSize="14"/>
    </DataTemplate>
</Window.Resources>
```

```csharp
gridControl.Model[6, 2].CellType = "DataBoundTemplate";
gridControl.Model[6, 2].CellItemTemplateKey = "StarTemplate";
gridControl.Model[6, 2].CellValue = "★★★★";
```

---

## Nested Grid Cell Type

Host one `GridModel` inside a cell of the parent grid using covered ranges:

```csharp
// Register the nested grid cell model
var nestedModel = new GridCellNestedScrollGridModel();
gridControl.Model.CellModels.Add("ScrollGrid", nestedModel);
gridControl.Model[2, 2].CellType = "ScrollGrid";

// Build the inner grid
var innerGrid = new GridModel();
innerGrid.RowCount    = 20;
innerGrid.ColumnCount = 5;
for (int i = 0; i < 20; i++)
    for (int j = 0; j < 5; j++)
    {
        innerGrid.Data[i, j] = new GridStyleInfo
            { CellType = "TextBox", CellValue = $"{i}:{j}" }.Store;
    }

// Assign inner grid and span parent cell
gridControl.Model[2, 2].CellValue = innerGrid;
gridControl.CoveredCells.Add(new CoveredCellInfo(2, 2, 9, 6));
```

> For shared row/column layouts between parent and nested grid, use
> `GridCellNestedGridModel(GridNestedAxisLayout.Shared, GridNestedAxisLayout.Normal)`.

---

## Custom Cell Types

Every cell type needs a **CellModel** (creates the control) and a **CellRenderer** (handles UI):

```csharp
// 1. Derive CellModel
public class RatingCellModel : GridCellTextBoxCellModel { }

// 2. Derive CellRenderer (override rendering/input behavior)
public class RatingCellRenderer : GridCellTextBoxCellRenderer
{
    // Override ArrangeUIElement, OnInitializeContent, etc.
}

// 3. Register with the grid
gridControl.Model.CellModels.Add("Rating", new RatingCellModel());

// 4. Use it
gridControl.Model[3, 4].CellType = "Rating";
```

For dropdown-based custom cells, derive from `GridCellDropDownCellModel<TRenderer>`
and `GridCellDropDownCellRenderer<TControl>`.

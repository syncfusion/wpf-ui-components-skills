# Getting Started with WPF GridControl

## Assembly References

Add the following assemblies to your WPF project:

```
Syncfusion.Core.dll
Syncfusion.Grid.Wpf.dll
Syncfusion.GridCommon.Wpf.dll
Syncfusion.Shared.Wpf.dll
```

> The Syncfusion WPF suite includes three grid variants: **GridControl** (cell-based, Excel-like), **SfDataGrid** (data-bound), and **SfTreeGrid** (hierarchical). Use GridControl when you need cell-level flexibility, virtual mode, or Excel-like behavior without a fixed data structure.

---

## Adding GridControl via Designer (Visual Studio)

1. Create a new WPF application.
2. Open the Designer window.
3. Drag a **ScrollViewer** from the Toolbox into the designer canvas.
   - GridControl has no built-in scroll bar — it must be placed inside a `ScrollViewer`.
4. Drag **GridControl** from the Toolbox into the `ScrollViewer`.
5. Required assemblies are added to the project automatically.

---

## Adding GridControl Programmatically

### XAML — define the layout root

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="MyApp.MainWindow"
        Title="Grid Demo" Height="450" Width="800">
    <Grid Name="layoutRoot" />
</Window>
```

### Code-behind — create and add the control

```csharp
// Create ScrollViewer and GridControl
ScrollViewer scrollViewer = new ScrollViewer();
GridControl gridControl = new GridControl();

// GridControl goes inside ScrollViewer
scrollViewer.Content = gridControl;

// ScrollViewer goes into the layout root
this.layoutRoot.Children.Add(scrollViewer);
```

---

## Setting Row and Column Count

`RowCount` and `ColumnCount` are mandatory before data can be displayed.

```csharp
gridControl.Model.RowCount = 100;
gridControl.Model.ColumnCount = 20;
```

---

## Populating Data

### Option A — Direct Loop (small datasets)

Assign `CellValue` by iterating over every cell:

```csharp
for (int i = 0; i < gridControl.Model.RowCount; i++)
{
    for (int j = 0; j < gridControl.Model.ColumnCount; j++)
    {
        gridControl.Model[i, j].CellValue = $"{i}/{j}";
    }
}
```

### Option B — QueryCellInfo Event (recommended for large datasets)

The grid calls this event on-demand, loading only the cells it is about to paint. This is the key to handling millions of rows without storing all data internally.

```csharp
gridControl.Model.RowCount = 1_000_000;
gridControl.Model.ColumnCount = 50;

gridControl.QueryCellInfo += GridControl_QueryCellInfo;

private void GridControl_QueryCellInfo(object sender,
    Syncfusion.Windows.Controls.Grid.GridQueryCellInfoEventArgs e)
{
    // Provide data for each requested cell
    if (e.Cell.RowIndex > 0 && e.Cell.ColumnIndex > 0)
    {
        e.Style.CellValue = $"R{e.Cell.RowIndex} C{e.Cell.ColumnIndex}";
    }
}
```

> **Why QueryCellInfo?** The grid never stores data internally; it requests each cell's value as needed. This makes it suitable for real-time dashboards and very large data sets.

---

## Minimal Working Example (Complete)

```csharp
// MainWindow.xaml.cs
public partial class MainWindow : Window
{
    private GridControl _grid;

    public MainWindow()
    {
        InitializeComponent();
        SetupGrid();
    }

    private void SetupGrid()
    {
        var scrollViewer = new ScrollViewer();
        _grid = new GridControl();

        _grid.Model.RowCount    = 200;
        _grid.Model.ColumnCount = 10;
        _grid.QueryCellInfo    += OnQueryCellInfo;

        scrollViewer.Content = _grid;
        layoutRoot.Children.Add(scrollViewer);
    }

    private void OnQueryCellInfo(object sender,
        Syncfusion.Windows.Controls.Grid.GridQueryCellInfoEventArgs e)
    {
        if (e.Cell.RowIndex == 0)
            e.Style.CellValue = $"Col {e.Cell.ColumnIndex}"; // header row
        else
            e.Style.CellValue = $"{e.Cell.RowIndex},{e.Cell.ColumnIndex}";
    }
}
```

---

## Common Gotchas

| Issue | Solution |
|---|---|
| Grid shows but no scroll | Wrap GridControl in `ScrollViewer` |
| Cells empty after setting RowCount | Must also handle `QueryCellInfo` or assign via `Model[r,c]` |
| Changes not visible | Call `gridControl.InvalidateCell(row, col)` or `gridControl.Refresh()` |
| Performance slow with millions of rows | Switch to `QueryCellInfo` virtual mode instead of direct loop assignment |

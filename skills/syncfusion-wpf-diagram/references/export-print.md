# Exporting and Printing in WPF Diagram (SfDiagram)

## Exporting

SfDiagram exports its content as image (PNG, JPEG, TIFF, GIF, BMP, WDP) or XPS using the `Export()` method.

### Basic Export

```csharp
// Export with default settings (opens file save dialog)
diagram.Export();
```

### ExportSettings Properties

| Property | Description |
|----------|-------------|
| `ExportType` | Image format: `PNG`, `JPEG`, `TIFF`, `GIF`, `BMP`, `WDP` |
| `FileName` | Output file path/name (e.g., `"diagram.png"`) |
| `ExportStream` | Write to a stream instead of a file |
| `ExportMode` | `Content` (fits all elements) or `PageSettings` (uses page size) |
| `IsSaveToXps` | `true` to export as XPS instead of image |
| `Clip` | Export only a specific rectangle region |
| `ImageSize` | Override the output image size |
| `ImageShrunk` | `None`, `Expand`, `Shrink`, `BestFit` |
| `ExportBackground` | Background brush for the exported image |

### Export to PNG

```csharp
diagram.ExportSettings = new ExportSettings()
{
    ExportType = ExportType.PNG,
    FileName   = "diagram.png",
    ExportMode = ExportMode.Content,
};
diagram.Export();
```

### Export to Stream

```csharp
MemoryStream stream = new MemoryStream();
diagram.ExportSettings = new ExportSettings()
{
    ExportType   = ExportType.PNG,
    ExportStream = stream,
    ExportMode   = ExportMode.Content,
};
diagram.Export();
```

### Export Modes

| ExportMode | Description |
|------------|-------------|
| `Content` | Tight bounding box around all nodes/connectors |
| `PageSettings` | Entire page region (respects `PageSettings.PageWidth/Height`) |

### Export to XPS

```csharp
diagram.ExportSettings = new ExportSettings()
{
    IsSaveToXps = true,
    FileName    = "diagram.xps",
};
diagram.Export();
```

### Export to PDF

SfDiagram has no built-in PDF export. Export to XPS first, then convert using `Syncfusion.XPS.XPSToPdfConverter`:

```csharp
// 1. Export to XPS
diagram.ExportSettings = new ExportSettings() { IsSaveToXps = true, FileName = "temp.xps" };
diagram.Export();

// 2. Convert XPS to PDF using Syncfusion PDF library
XPSToPdfConverter converter = new XPSToPdfConverter();
PdfDocument pdfDoc = converter.Convert("temp.xps");
pdfDoc.Save("diagram.pdf");
```

### Export Specific Region

```csharp
diagram.ExportSettings = new ExportSettings()
{
    Clip = new Rect(200, 0, 400, 300),  // x, y, width, height
};
diagram.Export();
```

### Custom Image Size + Stretch

```csharp
diagram.ExportSettings = new ExportSettings()
{
    ExportType   = ExportType.PNG,
    ImageSize    = new Size(800, 600),
    ImageShrunk  = ImageShrunk.BestFit,
    ExportBackground = new SolidColorBrush(Colors.White),
};
diagram.Export();
```

### XAML Export Settings

```xaml
<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.ExportSettings>
        <syncfusion:ExportSettings ExportMode="PageSettings"
                                   ExportType="PNG"
                                   FileName="output.png"
                                   ExportBackground="White"/>
    </syncfusion:SfDiagram.ExportSettings>
</syncfusion:SfDiagram>
```

---

## Printing

SfDiagram prints via `PrintingService`.

### Direct Print (No Dialog)

```csharp
diagram.PrintingService.Print();
```

### Print with Preview Dialog

```csharp
diagram.PrintingService.ShowDialog = true;
diagram.PrintingService.Print();
```

### Print Settings

```csharp
// Page settings affect print layout
diagram.PageSettings = new PageSettings()
{
    PageWidth       = 800,
    PageHeight      = 800,
    PageOrientation = PageOrientation.Landscape,
};

// Print-specific settings
diagram.PrintingService.PrintSettings.PageMargin = new Thickness(10);
```

### Print Settings Properties

| Property | Description |
|----------|-------------|
| `PrintSettings.PageMargin` | Margin around the printed content |
| `PrintSettings.PageHeaderHeight` | Height reserved for print header |
| `PrintSettings.PageHeaderTemplate` | DataTemplate for the print header |
| `PrintSettings.PageFooterHeight` | Height reserved for print footer |
| `PrintSettings.PageFooterTemplate` | DataTemplate for the print footer |

### Print Scale Mode

```csharp
// 0 = Single Page, 1 = Multiple Pages
diagram.PrintingService.PrintManager.SelectedScaleIndex = 1;
```

### Print Header and Footer

```xaml
<DataTemplate x:Key="PrintHeaderTemplate">
    <TextBlock Text="My Diagram" HorizontalAlignment="Center" FontSize="14"/>
</DataTemplate>

<DataTemplate x:Key="PrintFooterTemplate">
    <TextBlock HorizontalAlignment="Center">
        <TextBlock.Text>
            <Binding Path="PageIndex"
                     RelativeSource="{RelativeSource Mode=FindAncestor,
                                      AncestorType={x:Type Printing:PrintPageControl}}"
                     StringFormat="Page {0}"/>
        </TextBlock.Text>
    </TextBlock>
</DataTemplate>
```

```csharp
diagram.PrintingService.PrintSettings.PageHeaderHeight   = 40;
diagram.PrintingService.PrintSettings.PageHeaderTemplate = Resources["PrintHeaderTemplate"] as DataTemplate;
diagram.PrintingService.PrintSettings.PageFooterHeight   = 30;
diagram.PrintingService.PrintSettings.PageFooterTemplate = Resources["PrintFooterTemplate"] as DataTemplate;
```

### Skip Empty Pages

Override `PrintingService.GetPrintInfo` to skip pages with no content:

```csharp
public class CustomPrintingService : PrintingService
{
    protected override void GetPrintInfo(PrintInfo args)
    {
        if (!(args.Elements as IEnumerable<object>).Any())
            args.Cancel = true;
        else
            base.GetPrintInfo(args);
    }
}

// Assign custom service
diagram.PrintingService = new CustomPrintingService();
```

### Printing Events

```csharp
(diagram.Info as IGraphInfo).Printing += (s, e) =>
{
    // e.PrintState: Started, Printing, Completed, Cancelled, etc.
    if (e.PrintState == PrintStatus.Started)
    {
        // Configure print dialog
    }
};
```

| PrintStatus | Description |
|-------------|-------------|
| `Started` | Printing began |
| `Printing` | In progress |
| `Completed` | Finished |
| `Cancelled` | User cancelled |
| `PagePrepared` | Individual page ready |

### Classic Print Preview (Legacy)

```csharp
diagram.PrintingService.ShowDialog = true;
diagram.PrintingService.ShowClassicPrintPreview();
```

### Print Orientation

```csharp
// Can also be changed at runtime in the print preview dialog
diagram.PageSettings.PageOrientation = PageOrientation.Landscape;
```

---

## Common Gotchas

- **Export shows empty image:** Use `ExportMode.Content` to capture actual element bounds; `PageSettings` mode requires `PageWidth`/`PageHeight` to be set.
- **Transparent background in PNG:** `ExportBackground` defaults to transparent for PNG — set `ExportBackground = Brushes.White` if you need white background.
- **Print button disabled:** Occurs when the current page size isn't supported by the selected printer. Choose a supported size from the dropdown or use custom paper size code.
- **Empty pages in print:** Override `GetPrintInfo` and set `args.Cancel = true` for pages with no elements.

## Related
- Page settings → `references/diagram-customization.md`
- Serialization (save/load) → `references/serialization-undo-redo.md`

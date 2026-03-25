# Printing and Exporting

Export charts as images or print them directly from your WPF application.

## Print Chart

Print the chart using the built-in print method:

```csharp
using Syncfusion.UI.Xaml.Charts;

// Print the chart
sfChart.Print();
```

### Print with Dialog

Show print dialog before printing:

```csharp
PrintDialog printDialog = new PrintDialog();
if (printDialog.ShowDialog() == true)
{
    sfChart.Print();
}
```

## Export to Image

Save chart as an image file.

### Export as PNG

```csharp
using Syncfusion.UI.Xaml.Charts;
using System.IO;
using System.Windows.Media.Imaging;

// Save as PNG
sfChart.Save("chart.png", new PngBitmapEncoder());
```

### Export as JPEG

```csharp
sfChart.Save("chart.jpg", new JpegBitmapEncoder());
```

### Export as BMP

```csharp
sfChart.Save("chart.bmp", new BmpBitmapEncoder());
```


## Export to Stream

Save to a memory stream:

```csharp
using (MemoryStream stream = new MemoryStream())
{
    sfChart.Save(stream, new PngBitmapEncoder());
    
    // Use the stream (e.g., send over network, save to database)
    byte[] imageBytes = stream.ToArray();
}
```

## Serialization

Save and load chart configuration.

### Save Chart State

```csharp
using Syncfusion.Windows.Shared;
using System.Xml;

// Serialize chart
sfChart.Serialize();

```

### Load Chart State

```csharp
// Deserialize chart
sfChart.Deserialize();

```

## Complete Export Example

```csharp
using System.Windows;
using System.Windows.Media.Imaging;
using Microsoft.Win32;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }
    
    // Export button click handler
    private void ExportChart_Click(object sender, RoutedEventArgs e)
    {
        SaveFileDialog saveDialog = new SaveFileDialog();
        saveDialog.Filter = "PNG Image|*.png|JPEG Image|*.jpg|BMP Image|*.bmp";
        saveDialog.Title = "Export Chart";
        saveDialog.FileName = "chart";
        
        if (saveDialog.ShowDialog() == true)
        {
            BitmapEncoder encoder;
            
            // Choose encoder based on file extension
            switch (System.IO.Path.GetExtension(saveDialog.FileName).ToLower())
            {
                case ".jpg":
                case ".jpeg":
                    encoder = new JpegBitmapEncoder();
                    break;
                case ".bmp":
                    encoder = new BmpBitmapEncoder();
                    break;
                default:
                    encoder = new PngBitmapEncoder();
                    break;
            }
            
            // Save the chart
            sfChart.Save(saveDialog.FileName, encoder);
            
            MessageBox.Show("Chart exported successfully!", "Export",
                          MessageBoxButton.OK, MessageBoxImage.Information);
        }
    }
    
    // Print button click handler
    private void PrintChart_Click(object sender, RoutedEventArgs e)
    {
        PrintDialog printDialog = new PrintDialog();
        
        if (printDialog.ShowDialog() == true)
        {
            sfChart.Print();
        }
    }
}
```

## XAML with Export/Print Buttons

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <!-- Toolbar -->
    <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="5">
        <Button Content="Export" 
                Click="ExportChart_Click" 
                Margin="5"/>
        <Button Content="Print" 
                Click="PrintChart_Click" 
                Margin="5"/>
    </StackPanel>
    
    <!-- Chart -->
    <syncfusion:SfChart x:Name="sfChart" Grid.Row="1">
        <syncfusion:SfChart.PrimaryAxis>
            <syncfusion:CategoryAxis/>
        </syncfusion:SfChart.PrimaryAxis>
        
        <syncfusion:SfChart.SecondaryAxis>
            <syncfusion:NumericalAxis/>
        </syncfusion:SfChart.SecondaryAxis>
        
        <syncfusion:ColumnSeries ItemsSource="{Binding Data}"
                                 XBindingPath="Category"
                                 YBindingPath="Value"/>
    </syncfusion:SfChart>
</Grid>
```

## Export Quality Settings

### High-Quality Export

```csharp
// Export at higher resolution
sfChart.Save("chart_hq.png", new PngBitmapEncoder());
```

### JPEG Quality

```csharp
JpegBitmapEncoder jpegEncoder = new JpegBitmapEncoder();
jpegEncoder.QualityLevel = 95;  // 0-100, higher is better quality
sfChart.Save("chart.jpg", jpegEncoder);
```


## Batch Export

Export multiple charts:

```csharp
public void ExportAllCharts(List<SfChart> charts, string directory)
{
    for (int i = 0; i < charts.Count; i++)
    {
        string filename = System.IO.Path.Combine(directory, $"chart_{i + 1}.png");
        charts[i].Save(filename, new PngBitmapEncoder());
    }
}
```

## Export with Transparent Background

```csharp
// Set transparent background before export
sfChart.Background = Brushes.Transparent;
sfChart.AreaBackground = Brushes.Transparent;

// Export
sfChart.Save("chart_transparent.png", new PngBitmapEncoder());

// Restore background after export
sfChart.Background = Brushes.White;
```

## Common Export Scenarios

### Scenario 1: Save for Report

```csharp
// PNG for reports
sfChart.Save("monthly_report.png", new PngBitmapEncoder());
```

### Scenario 2: Email Attachment

```csharp
using (MemoryStream stream = new MemoryStream())
{
    sfChart.Save(stream, new JpegBitmapEncoder());
    
    // Attach to email
    Attachment attachment = new Attachment(stream, "chart.jpg");
    mail.Attachments.Add(attachment);
}
```

### Scenario 3: Clipboard Copy

```csharp
using System.Windows;

// Copy chart to clipboard
RenderTargetBitmap rtb = new RenderTargetBitmap(
    (int)sfChart.ActualWidth,
    (int)sfChart.ActualHeight,
    96, 96, PixelFormats.Pbgra32);
    
rtb.Render(sfChart);
Clipboard.SetImage(rtb);
```

## Key Methods

| Method | Description |
|--------|-------------|
| `Print()` | Print chart directly |
| `Save(filename, encoder)` | Export to image file |
| `Save(stream, encoder)` | Export to stream |
| `Serialize()` | Save chart configuration |
| `Deserialize()` | Load chart configuration |

## Supported Image Formats

- **PNG:** Best for web, reports, preserves transparency
- **JPEG:** Smaller file size, good for photos/complex charts
- **BMP:** Uncompressed, largest file size

## Best Practices

1. **Use PNG** for most scenarios (quality + transparency)
2. **Use JPEG** for large charts to reduce file size
3. **Export at high resolution** (1920x1080 or higher) for print
4. **Show save dialog** to let users choose location and format
5. **Handle exceptions** when saving to disk
6. **Consider background color** for transparency needs
7. **Test print layout** before distributing

# Events & Interactions

## Table of Contents
- [SelectedBrushChanged Event](#selectedbrushchanged-event)
- [SelectedCommand Binding](#selectedcommand-binding)
- [No Color Button](#no-color-button)
- [More Colors Window](#more-colors-window)
- [Color Tab Visibility](#color-tab-visibility)
- [Tooltip Support](#tooltip-support)
- [Header Template](#header-template)

## SelectedBrushChanged Event

The `SelectedBrushChanged` event fires whenever the user selects a color, providing access to both old and new values.

### Basic Event Handling
```xaml
<syncfusion:ColorPickerPalette SelectedBrushChanged="ColorPickerPalette_SelectedBrushChanged"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
private void ColorPickerPalette_SelectedBrushChanged(object sender, SelectedBrushChangedEventArgs e)
{
    // Get new selected values
    var newColor = e.NewColor;
    var newBrush = e.NewBrush;

    // Get previous values
    var oldColor = e.OldColor;
    var oldBrush = e.OldBrush;

    // Apply color to UI elements
    myTextBlock.Foreground = newBrush;
    statusLabel.Text = $"Color changed: {oldColor} → {newColor}";
}
```

### Example: Real-time Preview
```xaml
<Grid>
    <StackPanel Margin="20">
        <TextBlock Text="Text Color:" FontSize="14" FontWeight="Bold" Margin="0,0,0,10"/>
        
        <syncfusion:ColorPickerPalette x:Name="colorPicker"
                                       Color="Black"
                                       SelectedBrushChanged="ColorPicker_SelectedBrushChanged"
                                       Width="60"
                                       Height="40"
                                       Margin="0,0,0,20" />
        
        <TextBlock x:Name="previewText"
                   Text="Preview Text"
                   FontSize="20"
                   Foreground="Black" />
    </StackPanel>
</Grid>
```

```csharp
private void ColorPicker_SelectedBrushChanged(object sender, SelectedBrushChangedEventArgs e)
{
    previewText.Foreground = e.NewBrush;
}
```

### Example: Logging Color Changes
```csharp
public class ColorSelectionLogger
{
    private List<ColorChangeRecord> history = new List<ColorChangeRecord>();

    public void OnColorChanged(object sender, SelectedBrushChangedEventArgs e)
    {
        var record = new ColorChangeRecord
        {
            Timestamp = DateTime.Now,
            OldColor = e.OldColor,
            NewColor = e.NewColor,
            Source = (sender as ColorPickerPalette).Name
        };

        history.Add(record);
        LogChange(record);
    }

    private void LogChange(ColorChangeRecord record)
    {
        Console.WriteLine($"[{record.Timestamp:HH:mm:ss}] Color changed from {record.OldColor} to {record.NewColor}");
    }
}

public class ColorChangeRecord
{
    public DateTime Timestamp { get; set; }
    public Color OldColor { get; set; }
    public Color NewColor { get; set; }
    public string Source { get; set; }
}
```

### Example: Format Change Detection
```csharp
private void ColorPickerPalette_SelectedBrushChanged(object sender, SelectedBrushChangedEventArgs e)
{
    if (e.NewColor == Colors.Transparent)
    {
        // Handle transparent color
        myElement.Opacity = 0;
    }
    else if (e.NewColor == Colors.Black || e.NewColor == Colors.White)
    {
        // Handle neutral colors
        ApplyNeutralFormatting();
    }
    else
    {
        // Handle regular color
        ApplyColorFormatting(e.NewBrush);
    }
}
```

## SelectedCommand Binding

Use `SelectedCommand` to execute logic when user clicks the header in Split mode.

### Basic Command Binding
```xaml
<syncfusion:ColorPickerPalette Mode="Split"
                               SelectedCommand="{Binding ApplyColorCommand}"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40" />
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    public ICommand ApplyColorCommand { get; }

    public ViewModel()
    {
        ApplyColorCommand = new DelegateCommand(OnApplyColor);
    }

    private void OnApplyColor()
    {
        // Execute when user clicks the button part in Split mode
        MessageBox.Show("Applying last selected color...");
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Example: Reapply Color Command
```csharp
public class TextFormattingViewModel : INotifyPropertyChanged
{
    private RichTextBox richTextBox;
    public ICommand ApplyColorCommand { get; }

    public TextFormattingViewModel(RichTextBox textBox)
    {
        richTextBox = textBox;
        ApplyColorCommand = new DelegateCommand(ApplySelectedColor);
    }

    private void ApplySelectedColor()
    {
        // Apply the last selected color to selected text
        if (richTextBox.Selection.IsEmpty)
        {
            MessageBox.Show("Please select text to format.");
            return;
        }

        // Get the color from the ColorPickerPalette
        // In Split mode, clicking button reapplies the last selected color
        TextRange range = new TextRange(richTextBox.Selection.Start, richTextBox.Selection.End);
        range.ApplyPropertyValue(TextElement.ForegroundProperty, 
            new SolidColorBrush(Colors.Red));  // Use actual picker color
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Example: Integration with Color Change Event
```csharp
public class ColorCommandViewModel : INotifyPropertyChanged
{
    private Color lastSelectedColor = Colors.Black;
    public ICommand ApplyColorCommand { get; }

    public ColorCommandViewModel()
    {
        ApplyColorCommand = new DelegateCommand(ApplyLastColor);
    }

    public void OnColorChanged(object sender, SelectedBrushChangedEventArgs e)
    {
        // Store the color for later use
        lastSelectedColor = e.NewColor;
    }

    private void ApplyLastColor()
    {
        // Apply the color that was last selected
        ApplyColorToSelection(lastSelectedColor);
    }

    private void ApplyColorToSelection(Color color)
    {
        // Implementation for applying color
    }

    public event PropertyChangedEventHandler PropertyChanged;
}
```

## No Color Button

### Enable "No Color" Button
```xaml
<syncfusion:ColorPickerPalette NoColorVisibility="Visible"
                               Name="colorPickerPalette"/>
```

```csharp
colorPickerPalette.NoColorVisibility = Visibility.Visible;
```

### Detect "No Color" Selection
```csharp
private void ColorPickerPalette_SelectedBrushChanged(object sender, SelectedBrushChangedEventArgs e)
{
    if (e.NewColor == Colors.Transparent)
    {
        // "No Color" was clicked
        RemoveBackgroundColor();
    }
    else
    {
        ApplyBackgroundColor(e.NewBrush);
    }
}

private void RemoveBackgroundColor()
{
    myElement.Background = null;
    statusLabel.Text = "Background removed";
}

private void ApplyBackgroundColor(Brush brush)
{
    myElement.Background = brush;
    statusLabel.Text = "Background applied";
}
```

### Example: Optional Fill Color
```xaml
<Grid Margin="20">
    <StackPanel>
        <TextBlock Text="Shape Fill (Optional):" FontSize="12" Margin="0,0,0,5"/>
        <syncfusion:ColorPickerPalette x:Name="fillColorPicker"
                                       Color="White"
                                       NoColorVisibility="Visible"
                                       SelectedBrushChanged="FillColorPicker_SelectedBrushChanged"
                                       Width="60"
                                       Height="40"
                                       Margin="0,0,0,20" />
        
        <Border x:Name="shapePreview"
                Width="100"
                Height="100"
                BorderBrush="Black"
                BorderThickness="1" />
    </StackPanel>
</Grid>
```

```csharp
private void FillColorPicker_SelectedBrushChanged(object sender, SelectedBrushChangedEventArgs e)
{
    if (e.NewColor == Colors.Transparent)
    {
        shapePreview.Background = new SolidColorBrush(Colors.White);
        shapePreview.Opacity = 0.3;  // Visual hint that fill is removed
    }
    else
    {
        shapePreview.Background = e.NewBrush;
        shapePreview.Opacity = 1.0;
    }
}
```

## More Colors Window

The "More Colors" option opens an extended color picker window with standard and custom color tabs.

### Enable More Colors Option
```xaml
<syncfusion:ColorPickerPalette MoreColorOptionVisibility="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
colorPickerPalette.MoreColorOptionVisibility = Visibility.Visible;
```

### Control Standard Tab in More Colors
```xaml
<syncfusion:ColorPickerPalette IsStandardTabVisible="Visible"
                               IsCustomTabVisible="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
// Show standard color picker (140 colors in hexagon)
colorPickerPalette.IsStandardTabVisible = Visibility.Visible;

// Hide custom color picker
colorPickerPalette.IsCustomTabVisible = Visibility.Collapsed;
```

### Auto-hide More Colors if Tabs Hidden
```csharp
public void ConfigureMoreColorsWindow(ColorPickerPalette picker)
{
    // If both tabs are hidden, More Colors is automatically hidden
    if (picker.IsStandardTabVisible == Visibility.Collapsed && 
        picker.IsCustomTabVisible == Visibility.Collapsed)
    {
        picker.MoreColorOptionVisibility = Visibility.Collapsed;
    }
}
```

### Example: Expert Mode
```csharp
public void EnableExpertMode(ColorPickerPalette picker)
{
    // Only show More Colors window for power users
    picker.ThemePanelVisibility = Visibility.Collapsed;
    picker.StandardPanelVisibility = Visibility.Collapsed;
    picker.MoreColorOptionVisibility = Visibility.Visible;
    picker.IsStandardTabVisible = Visibility.Visible;
    picker.IsCustomTabVisible = Visibility.Visible;
}
```

## Color Tab Visibility

Control which tabs are visible in the "More Colors" window:

### Standard Tab (140 Colors)
```xaml
<syncfusion:ColorPickerPalette IsStandardTabVisible="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
// Show the hexagon of 140 standard colors
colorPickerPalette.IsStandardTabVisible = Visibility.Visible;

// Hide it
colorPickerPalette.IsStandardTabVisible = Visibility.Collapsed;
```

### Custom Tab (Color Picker)
```xaml
<syncfusion:ColorPickerPalette IsCustomTabVisible="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

```csharp
// Show the custom color picker
colorPickerPalette.IsCustomTabVisible = Visibility.Visible;

// Hide it
colorPickerPalette.IsCustomTabVisible = Visibility.Collapsed;
```

### Both Tabs Visible
```xaml
<syncfusion:ColorPickerPalette IsStandardTabVisible="Visible"
                               IsCustomTabVisible="Visible"
                               MoreColorOptionVisibility="Visible"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
</syncfusion:ColorPickerPalette>
```

## Tooltip Support

ColorPickerPalette displays tooltips showing color names when hovering over colors.

### Default Tooltip Behavior
By default, when you hover over a color, a tooltip displays the color name:

```xaml
<syncfusion:ColorPickerPalette GenerateThemeVariants="True"
                               Name="colorPickerPalette" 
                               Width="60"
                               Height="40">
    <!-- Hovering shows color name like "Red", "Green", etc. -->
</syncfusion:ColorPickerPalette>
```

### Custom Color Tooltips
When using custom colors with ColorName, that name appears in tooltip:

```csharp
var customColors = new ObservableCollection<CustomColor>
{
    new CustomColor 
    { 
        Color = Color.FromRgb(51, 122, 183), 
        ColorName = "Brand Primary Blue"  // This appears in tooltip
    },
    new CustomColor 
    { 
        Color = Color.FromRgb(220, 53, 69), 
        ColorName = "Alert Red"  // This appears in tooltip
    }
};

colorPickerPalette.CustomColorsCollection = customColors;
colorPickerPalette.SetCustomColors = true;
```

### Recently Used Tooltips
Colors in Recently Used panel also show tooltips:

```xaml
<syncfusion:ColorPickerPalette RecentlyUsedPanelVisibility="Visible"
                               Name="colorPickerPalette"
                               Width="60"
                               Height="40" />
```

When hovering over a recently used color, tooltip shows the color name.

## Header Template

### Customize the Header Display
The header template allows custom rendering of the selected color indicator and label:

```xaml
<Window.Resources>
    <DataTemplate x:Key="CustomHeaderTemplate">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            
            <!-- Custom color box -->
            <Border Grid.Column="0"
                    Width="16"
                    Height="16"
                    BorderBrush="Black"
                    BorderThickness="1"
                    CornerRadius="2"
                    Margin="3,0,5,0">
                <Border.Background>
                    <SolidColorBrush Color="{Binding Color, 
                        RelativeSource={RelativeSource FindAncestor, 
                            AncestorType={x:Type syncfusion:ColorPickerPalette}}}"/>
                </Border.Background>
            </Border>
            
            <!-- Custom label -->
            <TextBlock Grid.Column="1"
                       Text="Pick Color"
                       VerticalAlignment="Center"
                       FontSize="11" />
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:ColorPickerPalette HeaderTemplate="{StaticResource CustomHeaderTemplate}"
                               Name="colorPickerPalette"
                               Width="100"
                               Height="40" />
```

### Header Template with Color Name
```xaml
<Window.Resources>
    <DataTemplate x:Key="ColorNameHeaderTemplate">
        <StackPanel Orientation="Horizontal">
            <Border Width="20"
                    Height="20"
                    CornerRadius="2"
                    Margin="3,0,8,0">
                <Border.Background>
                    <SolidColorBrush Color="{Binding Color, 
                        RelativeSource={RelativeSource FindAncestor, 
                            AncestorType={x:Type syncfusion:ColorPickerPalette}}}"/>
                </Border.Background>
            </Border>
            <TextBlock Text="{Binding ColorName, 
                        RelativeSource={RelativeSource FindAncestor, 
                            AncestorType={x:Type syncfusion:ColorPickerPalette}}}"
                       VerticalAlignment="Center"
                       FontSize="12" />
        </StackPanel>
    </DataTemplate>
</Window.Resources>

<syncfusion:ColorPickerPalette HeaderTemplate="{StaticResource ColorNameHeaderTemplate}"
                               Name="colorPickerPalette"
                               Width="150"
                               Height="40" />
```

### DataContext in Template
The DataContext of the header template is the ColorPickerPalette itself:
- Access `Color` for the currently selected color
- Access `ColorName` for the color's name
- Access `SelectedBrush` for the selected brush
- Access any other public property of ColorPickerPalette

## Complete Interaction Example

```xaml
<Grid Background="#F5F5F5">
    <StackPanel Margin="20">
        <TextBlock Text="Advanced Color Picker" FontSize="16" FontWeight="Bold" Margin="0,0,0,15"/>
        
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            
            <!-- Color Picker -->
            <StackPanel Grid.Column="0" Margin="0,0,20,0">
                <TextBlock Text="Select Color:" FontSize="12" Margin="0,0,0,5"/>
                <syncfusion:ColorPickerPalette x:Name="colorPicker"
                                               Mode="Split"
                                               Color="Blue"
                                               GenerateThemeVariants="True"
                                               GenerateStandardVariants="True"
                                               RecentlyUsedPanelVisibility="Visible"
                                               MoreColorOptionVisibility="Visible"
                                               NoColorVisibility="Visible"
                                               SelectedBrushChanged="ColorPicker_SelectedBrushChanged"
                                               SelectedCommand="{Binding ApplyColorCommand}"
                                               Width="60"
                                               Height="40" />
            </StackPanel>
            
            <!-- Preview Area -->
            <StackPanel Grid.Column="1">
                <TextBlock Text="Preview:" FontSize="12" Margin="0,0,0,5"/>
                <TextBlock x:Name="statusText"
                           Text="No color selected"
                           FontSize="11"
                           Foreground="Gray"
                           Margin="0,0,0,10" />
                <Border x:Name="colorPreview"
                        Width="200"
                        Height="100"
                        BorderBrush="Black"
                        BorderThickness="1"
                        Background="Blue" />
            </StackPanel>
        </Grid>
    </StackPanel>
</Grid>
```

```csharp
private void ColorPicker_SelectedBrushChanged(object sender, SelectedBrushChangedEventArgs e)
{
    colorPreview.Background = e.NewBrush;
    
    if (e.NewColor == Colors.Transparent)
    {
        statusText.Text = "Color: Transparent (No Fill)";
    }
    else
    {
        statusText.Text = $"Color: {e.NewColor.ToString()} - R:{e.NewColor.R} G:{e.NewColor.G} B:{e.NewColor.B}";
    }
}
```

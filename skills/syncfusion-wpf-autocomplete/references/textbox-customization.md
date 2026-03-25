# TextBox Customization

Complete guide for customizing the TextBox appearance and behavior in WPF Autocomplete (SfTextBoxExt).

## Table of Contents
- [Watermark](#watermark)
- [Watermark Template](#watermark-template)
- [Text Styling](#text-styling)
- [ShowDropDownButton](#showdropdownbutton)
- [ShowClearButton](#showclearbutton)

## Watermark

Display placeholder text when the textbox is empty using the `Watermark` property.

### Basic Watermark

```xaml
<editors:SfTextBoxExt 
    Width="300" 
    Height="40"
    Watermark="Enter names separated by comma (Ex: John, Kate)"/>
```

```csharp
textBoxExt.Watermark = "Enter names separated by comma (Ex: John, Kate)";
```

### Watermark with Visual Elements

The `Watermark` property accepts any object, including complex visual elements:

```xaml
<editors:SfTextBoxExt Width="300" Height="50">
    <editors:SfTextBoxExt.Watermark>
        <StackPanel Orientation="Horizontal">
            <Image Source="search_icon.png" Width="16" Height="16" Margin="2"/>
            <TextBlock Text="Search Windows" 
                       VerticalAlignment="Center" 
                       Opacity="0.5"
                       Margin="5,0,0,0"/>
        </StackPanel>
    </editors:SfTextBoxExt.Watermark>
</editors:SfTextBoxExt>
```

### Styled Watermark

```xaml
<editors:SfTextBoxExt Width="300" Height="40">
    <editors:SfTextBoxExt.Watermark>
        <TextBlock Text="Type to search..." 
                   FontStyle="Italic" 
                   Foreground="Gray"
                   FontSize="14"/>
    </editors:SfTextBoxExt.Watermark>
</editors:SfTextBoxExt>
```

## Watermark Template

Apply a data template to customize watermark appearance.

### Basic Template

```xaml
<Window.Resources>
    <DataTemplate x:Key="WatermarkTemplate">
        <TextBlock Text="{Binding}" 
                   FontStyle="Italic" 
                   Foreground="DarkGray"
                   Opacity="0.7"/>
    </DataTemplate>
</Window.Resources>

<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    Watermark="Type here"
    WatermarkTemplate="{StaticResource WatermarkTemplate}"/>
```

### Advanced Template with Icon

```xaml
<Window.Resources>
    <DataTemplate x:Key="SearchWatermarkTemplate">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
            </Grid.ColumnDefinitions>
            
            <Path Grid.Column="0" 
                  Data="M15.5 14h-.79l-.28-.27C15.41 12.59 16 11.11 16 9.5 16 5.91 13.09 3 9.5 3S3 5.91 3 9.5 5.91 16 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z" 
                  Fill="Gray" 
                  Width="16" 
                  Height="16"
                  Stretch="Uniform"
                  Margin="0,0,8,0"/>
            
            <TextBlock Grid.Column="1" 
                       Text="{Binding}" 
                       VerticalAlignment="Center"
                       Foreground="Gray"
                       FontStyle="Italic"/>
        </Grid>
    </DataTemplate>
</Window.Resources>

<editors:SfTextBoxExt 
    Watermark="Search employees..."
    WatermarkTemplate="{StaticResource SearchWatermarkTemplate}"/>
```

## Text Styling

Customize the appearance of entered text using standard WPF text properties.

### Font Properties

```xaml
<editors:SfTextBoxExt 
    Text="Sample Text"
    FontSize="16"
    FontWeight="SemiBold"
    FontFamily="Segoe UI"
    Foreground="DarkBlue"
    Width="300"
    Height="40"/>
```

```csharp
// Font customization
textBoxExt.FontSize = 16;
textBoxExt.FontWeight = FontWeights.SemiBold;
textBoxExt.FontFamily = new FontFamily("Segoe UI");
textBoxExt.Foreground = new SolidColorBrush(Colors.DarkBlue);
```

### Complete Styling Example

```xaml
<editors:SfTextBoxExt 
    Width="400"
    Height="50"
    Padding="10,5"
    FontSize="14"
    FontFamily="Arial"
    FontWeight="Normal"
    Foreground="Black"
    Background="WhiteSmoke"
    BorderBrush="Gray"
    BorderThickness="1"
    Watermark="Enter text..."/>
```

```csharp
// Complete styling in code
textBoxExt.Width = 400;
textBoxExt.Height = 50;
textBoxExt.Padding = new Thickness(10, 5, 10, 5);
textBoxExt.FontSize = 14;
textBoxExt.FontFamily = new FontFamily("Arial");
textBoxExt.FontWeight = FontWeights.Normal;
textBoxExt.Foreground = Brushes.Black;
textBoxExt.Background = Brushes.WhiteSmoke;
textBoxExt.BorderBrush = Brushes.Gray;
textBoxExt.BorderThickness = new Thickness(1);
```

## ShowDropDownButton

Display a dropdown button icon for easy access to suggestions.

### Basic Usage

```xaml
<editors:SfTextBoxExt 
    ShowDropDownButton="True"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40"/>
```

```csharp
textBoxExt.ShowDropDownButton = true;
```

**Default:** `false`

**When Enabled:**
- Dropdown arrow icon appears on the right
- Click icon to show all suggestions
- Works with `ShowSuggestionsOnFocus` for better UX

### Combined with Auto-Complete

```xaml
<editors:SfTextBoxExt 
    ShowDropDownButton="True"
    ShowSuggestionsOnFocus="True"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Data}"
    Width="300"
    Height="40"/>
```

### Use Cases

- **Combo box behavior**: Users can type or select from list
- **Browse mode**: Quick access to all options
- **Touch devices**: Easier to open dropdown with button
- **Discoverability**: Visual cue that dropdown exists

## ShowClearButton

Display a clear button to quickly remove all text.

### Basic Usage

```xaml
<editors:SfTextBoxExt 
    ShowClearButton="True"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}"
    Width="300"
    Height="40"/>
```

```csharp
textBoxExt.ShowClearButton = true;
```

**Default:** `false`

**When Enabled:**
- Clear (X) button appears when text is entered
- Click to remove all text
- Button automatically hides when textbox is empty

### Combined with Dropdown Button

```xaml
<editors:SfTextBoxExt 
    ShowDropDownButton="True"
    ShowClearButton="True"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Data}"
    Width="300"
    Height="40"/>
```

**Note:** In Token mode (`MultiSelectMode="Token"`), clear button removes all selected tokens.

### Use Cases

- **Quick reset**: Easy way to start over
- **Long text**: Faster than backspace
- **Touch devices**: Better than selecting and deleting
- **User convenience**: Improved UX for corrections

## Complete Customization Example

```xaml
<Window.Resources>
    <DataTemplate x:Key="CustomWatermark">
        <StackPanel Orientation="Horizontal">
            <Path Data="M15.5 14h-.79l-.28-.27..." Fill="Gray" Width="14" Height="14"/>
            <TextBlock Text="Search employees by name or email..." 
                       Margin="8,0,0,0" 
                       FontStyle="Italic" 
                       Foreground="Gray"/>
        </StackPanel>
    </DataTemplate>
</Window.Resources>

<editors:SfTextBoxExt 
    Width="450"
    Height="45"
    Padding="12,8"
    
    <!-- Watermark -->
    Watermark="Search..."
    WatermarkTemplate="{StaticResource CustomWatermark}"
    
    <!-- Text Styling -->
    FontSize="14"
    FontFamily="Segoe UI"
    FontWeight="Normal"
    Foreground="Black"
    
    <!-- Background & Border -->
    Background="White"
    BorderBrush="LightGray"
    BorderThickness="1"
    
    <!-- Buttons -->
    ShowDropDownButton="True"
    ShowClearButton="True"
    
    <!-- AutoComplete -->
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}"/>
```

## Styling Patterns

### Material Design Style

```xaml
<editors:SfTextBoxExt 
    Height="56"
    Padding="16,18,16,0"
    FontSize="16"
    Background="WhiteSmoke"
    BorderThickness="0,0,0,2"
    BorderBrush="DodgerBlue"
    Watermark="Label"
    ShowClearButton="True"/>
```

### Modern Flat Style

```xaml
<editors:SfTextBoxExt 
    Height="40"
    Padding="12,8"
    FontSize="14"
    Background="White"
    BorderThickness="1"
    BorderBrush="#E0E0E0"
    Watermark="Search..."
    ShowDropDownButton="True"
    ShowClearButton="True"/>
```

### Rounded Corner Style

```xaml
<Border CornerRadius="20" BorderBrush="Gray" BorderThickness="1">
    <editors:SfTextBoxExt 
        Height="40"
        Padding="16,8"
        FontSize="14"
        Background="Transparent"
        BorderThickness="0"
        Watermark="Search..."
        ShowClearButton="True"/>
</Border>
```

## Best Practices

### Accessibility

```csharp
// Set accessible names
AutomationProperties.SetName(textBoxExt, "Employee Search");
AutomationProperties.SetHelpText(textBoxExt, "Type to search employees");
```

### Consistent Styling

```csharp
// Create reusable style
public Style GetAutoCompleteStyle()
{
    var style = new Style(typeof(SfTextBoxExt));
    style.Setters.Add(new Setter(SfTextBoxExt.HeightProperty, 40.0));
    style.Setters.Add(new Setter(SfTextBoxExt.FontSizeProperty, 14.0));
    style.Setters.Add(new Setter(SfTextBoxExt.ShowClearButtonProperty, true));
    return style;
}
```

### Responsive Design

```csharp
// Adjust based on window size
private void AdjustTextBoxSize()
{
    if (this.ActualWidth < 600) // Mobile/tablet
    {
        textBoxExt.FontSize = 16; // Larger for touch
        textBoxExt.Height = 48;
        textBoxExt.ShowDropDownButton = true; // Easier tapping
    }
    else // Desktop
    {
        textBoxExt.FontSize = 14;
        textBoxExt.Height = 40;
    }
}
```

## Next Steps

- **Advanced Features**: Text highlighting and diacritic search → [advanced-features.md](advanced-features.md)
- **Dropdown Customization**: Suggestion popup styling → [dropdown-customization.md](dropdown-customization.md)
- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)

## Resources

- **GitHub Sample**: https://github.com/SyncfusionExamples/wpf-textboxext-examples/tree/master/Samples/Textbox-customization
- **API Documentation**: https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Controls.Input.SfTextBoxExt.html

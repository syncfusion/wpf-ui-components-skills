# Customization in WPF Syntax Editor

This guide covers appearance customization options for the Syncfusion WPF Syntax Editor (EditControl) including fonts, colors, backgrounds, line customization, and visual styling.

## Table of Contents
- [Font and Text Customization](#font-and-text-customization)
- [Color Customization](#color-customization)
- [Background Customization](#background-customization)
- [Line Background Customization](#line-background-customization)
- [Margin and Padding](#margin-and-padding)
- [Cursor Customization](#cursor-customization)
- [Visual Styles and Themes](#visual-styles-and-themes)

## Font and Text Customization

### Setting Font Properties

```xml
<syncfusion:EditControl Name="editControl"
                        FontFamily="Consolas"
                        FontSize="12"
                        FontWeight="Normal"
                        FontStyle="Normal"/>
```

```csharp
editControl.FontFamily = new FontFamily("Consolas");
editControl.FontSize = 12;
editControl.FontWeight = FontWeights.Normal;
editControl.FontStyle = FontStyles.Normal;
```

### Common Monospace Fonts for Code

- **Consolas** - Modern, clear (Windows default)
- **Courier New** - Classic monospace
- **Fira Code** - With ligatures
- **Source Code Pro** - Adobe's programmer font
- **JetBrains Mono** - Optimized for coding

## Color Customization

### Foreground and Background Colors

```xml
<syncfusion:EditControl Name="editControl"
                        Foreground="Black"
                        Background="White"
                        BorderBrush="Gray"
                        BorderThickness="1"/>
```

```csharp
editControl.Foreground = Brushes.Black;
editControl.Background = Brushes.White;
editControl.BorderBrush = Brushes.Gray;
editControl.BorderThickness = new Thickness(1);
```

### Selection Colors

```csharp
// Customize selection appearance
editControl.SelectionBrush = Brushes.LightBlue;
editControl.SelectionForeground = Brushes.Black;
```

### Syntax Highlighting Colors

Syntax colors are configured through the language definition:

```csharp
// Custom keyword color in language definition
var language = new CSharpLanguage(editControl);
language.Lexems[0].ForeColor = Colors.Blue; // Keywords
language.Lexems[1].ForeColor = Colors.Green; // Comments
language.Lexems[2].ForeColor = Colors.Brown; // Strings
```

## Background Customization

### Solid Background

```xml
<syncfusion:EditControl Background="White"/>
```

```csharp
editControl.Background = Brushes.White;
```

### Gradient Background

```xml
<syncfusion:EditControl>
    <syncfusion:EditControl.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="White" Offset="0"/>
            <GradientStop Color="#F5F5F5" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:EditControl.Background>
</syncfusion:EditControl>
```

### Image Background

```xml
<syncfusion:EditControl>
    <syncfusion:EditControl.Background>
        <ImageBrush ImageSource="background.png" 
                    Opacity="0.1"
                    Stretch="UniformToFill"/>
    </syncfusion:EditControl.Background>
</syncfusion:EditControl>
```

## Line Background Customization

### Set Line Background for Specific Lines

Use `SetLineBackground` to highlight specific lines:

```csharp
// Highlight current line
editControl.SetLineBackground(
    editControl.LineNumber,  // Line number
    true,                    // Full line
    Brushes.Yellow          // Background brush
);

// Highlight error line
editControl.SetLineBackground(
    errorLineNumber,
    true,
    Brushes.LightCoral
);

// Highlight breakpoint line
editControl.SetLineBackground(
    breakpointLine,
    false,  // Only text portion
    Brushes.LightPink
);
```

### Reset Line Background

```csharp
// Reset specific line
editControl.ResetLineBackground(editControl.LineNumber);

// Reset all custom line backgrounds
for (int i = 1; i <= editControl.LineCount; i++)
{
    editControl.ResetLineBackground(i);
}
```

### On-Demand Line Background Customization

Use `OnBeforeLineRender` event for dynamic line coloring:

```csharp
public MainWindow()
{
    InitializeComponent();
    editControl.DocumentSource = "Source.cs";
    editControl.OnBeforeLineRender += EditControl_OnBeforeLineRender;
}

private void EditControl_OnBeforeLineRender(
    object sender, 
    OnBeforeLineRenderArgs args)
{
    // Alternate line colors (zebra striping)
    if (args.LineItem.LineNumber % 2 == 0)
    {
        args.BackgroundColor = Brushes.LightGray;
        args.IsFullLine = false;
    }
    
    // Highlight lines with errors
    if (errorLines.Contains(args.LineItem.LineNumber))
    {
        args.BackgroundColor = Brushes.LightCoral;
        args.IsFullLine = true;
    }
    
    // Highlight lines with TODO comments
    string lineText = args.LineItem.Text;
    if (lineText.Contains("// TODO") || lineText.Contains("// FIXME"))
    {
        args.BackgroundColor = Brushes.LightYellow;
        args.IsFullLine = true;
    }
}
```

**Use Cases for OnBeforeLineRender:**
- Zebra striping (alternating line colors)
- Error/warning highlighting
- Current line highlighting
- Breakpoint indication
- Code coverage visualization
- Search result highlighting
- TODO/FIXME highlighting

**Performance Note:** On-demand customization is recommended for large files as it only processes visible lines.

## Margin and Padding

### Content Margins

```xml
<syncfusion:EditControl Padding="10"
                        Margin="5"/>
```

```csharp
editControl.Padding = new Thickness(10);
editControl.Margin = new Thickness(5);
```

### Line Number Margin

```csharp
// Configure line number appearance
editControl.ShowLineNumber = true;
editControl.LineNumberForeground = Brushes.Gray;
editControl.LineNumberMargin = new Thickness(5, 0, 10, 0);
```

## Cursor Customization

### Cursor Appearance

```csharp
// Change cursor color
editControl.CaretBrush = Brushes.Red;

// Change cursor style
editControl.CaretWidth = 2;

// Blinking cursor
editControl.IsCaretBlinkEnabled = true;
```

### Insert/Overwrite Mode

```csharp
// Toggle insert/overwrite mode
editControl.InsertMode = InsertMode.Insert;  // or InsertMode.Overwrite

// Handle mode changes
editControl.InsertModeChanged += (s, e) =>
{
    UpdateStatusBar(editControl.InsertMode);
};
```

## Visual Styles and Themes

### Applying Visual Styles

```csharp
// Apply Syncfusion visual style
SfSkinManager.SetVisualStyle(editControl, VisualStyles.MaterialLight);

// Available styles
// - Metro
// - Office2019Colorful
// - MaterialLight
// - MaterialDark
// - FluentLight
// - FluentDark
```

### Custom Theme Colors

```xml
<syncfusion:EditControl>
    <syncfusion:EditControl.Resources>
        <SolidColorBrush x:Key="SyncfusionEditControlBackground" 
                         Color="#1E1E1E"/>
        <SolidColorBrush x:Key="SyncfusionEditControlForeground" 
                         Color="#D4D4D4"/>
    </syncfusion:EditControl.Resources>
</syncfusion:EditControl>
```

### Dark Theme Example

```csharp
// Configure dark theme
editControl.Background = new SolidColorBrush(Color.FromRgb(30, 30, 30));
editControl.Foreground = new SolidColorBrush(Color.FromRgb(212, 212, 212));
editControl.SelectionBrush = new SolidColorBrush(Color.FromRgb(38, 79, 120));
editControl.LineNumberForeground = Brushes.DarkGray;
editControl.CaretBrush = Brushes.White;

// Dark theme syntax colors (if using custom language)
if (editControl.DocumentLanguage is CustomLanguage customLang)
{
    // Keywords - lighter blue
    customLang.Lexems[0].ForeColor = Color.FromRgb(86, 156, 214);
    
    // Comments - green
    customLang.Lexems[1].ForeColor = Color.FromRgb(106, 153, 85);
    
    // Strings - orange
    customLang.Lexems[2].ForeColor = Color.FromRgb(206, 145, 120);
}
```

### Light Theme Example

```csharp
// Configure light theme
editControl.Background = Brushes.White;
editControl.Foreground = Brushes.Black;
editControl.SelectionBrush = Brushes.LightBlue;
editControl.LineNumberForeground = Brushes.Gray;
editControl.CaretBrush = Brushes.Black;

// Light theme syntax colors
if (editControl.DocumentLanguage is CustomLanguage customLang)
{
    customLang.Lexems[0].ForeColor = Colors.Blue;      // Keywords
    customLang.Lexems[1].ForeColor = Colors.Green;     // Comments
    customLang.Lexems[2].ForeColor = Colors.Brown;     // Strings
}
```

## Complete Customization Example

```csharp
public class EditorTheme
{
    public static void ApplyVSCodeDarkTheme(EditControl editor)
    {
        // Background and text
        editor.Background = new SolidColorBrush(Color.FromRgb(30, 30, 30));
        editor.Foreground = new SolidColorBrush(Color.FromRgb(212, 212, 212));
        
        // Selection
        editor.SelectionBrush = new SolidColorBrush(Color.FromRgb(38, 79, 120));
        
        // Line numbers
        editor.ShowLineNumber = true;
        editor.LineNumberForeground = new SolidColorBrush(Color.FromRgb(133, 133, 133));
        
        // Cursor
        editor.CaretBrush = Brushes.White;
        editor.CaretWidth = 2;
        
        // Current line highlight
        editor.OnBeforeLineRender += (s, e) =>
        {
            if (e.LineItem.LineNumber == editor.LineNumber)
            {
                e.BackgroundColor = new SolidColorBrush(
                    Color.FromArgb(25, 255, 255, 255));
                e.IsFullLine = true;
            }
        };
        
        // Font
        editor.FontFamily = new FontFamily("Consolas");
        editor.FontSize = 14;
    }
    
    public static void ApplyVisualStudioLightTheme(EditControl editor)
    {
        // Background and text
        editor.Background = Brushes.White;
        editor.Foreground = Brushes.Black;
        
        // Selection
        editor.SelectionBrush = new SolidColorBrush(Color.FromRgb(173, 214, 255));
        
        // Line numbers
        editor.ShowLineNumber = true;
        editor.LineNumberForeground = new SolidColorBrush(Color.FromRgb(43, 145, 175));
        
        // Cursor
        editor.CaretBrush = Brushes.Black;
        
        // Current line highlight
        editor.OnBeforeLineRender += (s, e) =>
        {
            if (e.LineItem.LineNumber == editor.LineNumber)
            {
                e.BackgroundColor = new SolidColorBrush(
                    Color.FromRgb(232, 242, 254));
                e.IsFullLine = true;
            }
        };
        
        // Font
        editor.FontFamily = new FontFamily("Consolas");
        editor.FontSize = 10;
    }
}

// Usage
EditorTheme.ApplyVSCodeDarkTheme(editControl);
```

## Best Practices

1. **Use Monospace Fonts**
   - Always use monospace fonts for code editing
   - Consolas, Courier New, or Fira Code are recommended

2. **Sufficient Contrast**
   - Ensure text is readable against background
   - WCAG AA: 4.5:1 contrast ratio minimum
   - Test with different lighting conditions

3. **Current Line Highlighting**
   - Use subtle highlighting for current line
   - Helps users track cursor position

4. **Consistent Theming**
   - Apply consistent color scheme across all UI elements
   - Match syntax colors to overall theme

5. **Performance with Line Rendering**
   - Use `OnBeforeLineRender` for dynamic styling
   - Avoid heavy computation in render events
   - Cache colors and brushes

6. **User Preferences**
   - Allow users to customize colors and fonts
   - Save/load theme preferences
   - Provide dark and light theme options

## Related Topics

- [Syntax Highlighting](syntax-highlighting.md) - Configure language-specific colors
- [Line Numbers](line-numbers.md) - Line number display options
- [Getting Started](getting-started.md) - Initial setup and configuration

# Appearance & Theming

## Colors and Backgrounds

Customize control appearance with background colors, selection brushes, and foreground text colors.

**Color Properties:**
- `Background` — Control background (default: White)
- `Foreground` — Text color (default: Black)  
- `SelectionBrush` — Field highlight color (default: Royal Blue)

**XAML example:**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         Background="White"
                         SelectionBrush="#2196F3"
                         Foreground="Black" />
```

**C# code:**
```csharp
editor.Background = new SolidColorBrush(Color.FromArgb(255, 240, 240, 240));
editor.SelectionBrush = new SolidColorBrush(Color.FromArgb(255, 33, 150, 243));
editor.Foreground = Brushes.Black;
```

**Predefined color schemes:**

| Scheme | Background | Foreground | SelectionBrush | Use Case |
|--------|-----------|-----------|---|---|
| Light | White | Black | #2196F3 (Blue) | Default, web-like |
| Dark | #2D2D2D | White | #4CAF50 (Green) | Dark mode, reduced eye strain |
| Error | #FFE0E0 | Red | DarkRed | Invalid input, alerts |
| Success | #E0FFE0 | Green | DarkGreen | Valid input, confirmations |
| Warning | #FFF3E0 | Orange | OrangeRed | Caution states |
| Professional | #F5F5F5 | #333333 | #1976D2 | Corporate applications |
| High Contrast | White | Black | Black | Accessibility, WCAG compliance |

**Dynamic styling in C#:**
```csharp
private void UpdateStatusColors(TimeSpan value)
{
    if (value.TotalHours > 12)
        editor.Foreground = Brushes.Orange;  // Warning
    else if (value.TotalHours > 8)
        editor.Foreground = Brushes.Black;   // Normal
    else
        editor.Foreground = Brushes.Green;   // Low/OK
}
```

---

## Flow Direction & RTL

Support right-to-left (RTL) for Arabic, Hebrew, and other RTL languages.

**XAML RTL example:**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45" FlowDirection="RightToLeft" />
```

**C# code:**
```csharp
editor.FlowDirection = FlowDirection.RightToLeft;  // Enable RTL
editor.FlowDirection = FlowDirection.LeftToRight;  // LTR (default)
```

Use for: Arabic-speaking regions, Hebrew language, bidirectional text applications.

## Built-In Themes

Apply coordinated visual styles using `SfSkinManager`.

**Available themes:** Default, Light, Dark, Office2016White, Office2016Black, Office2016Colorful, VisualStudio2015, VisualStudioDark, MaterialLight, MaterialDark, FluentLight, FluentDark.

**XAML:**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45"
                         syncfusion:SfSkinManager.VisualStyle="MaterialDark" />
```

**C# code:**
```csharp
using Syncfusion.SfSkinManager;

// Apply theme to entire window
SfSkinManager.SetVisualStyle(this, VisualStyles.MaterialDark);

// Apply theme to specific control
SfSkinManager.SetVisualStyle(timeSpanEdit, VisualStyles.FluentLight);
```

## Theme Switching

**Dynamic switching:**
```csharp
private void DarkModeButton_Click(object sender, RoutedEventArgs e)
{
    SfSkinManager.SetVisualStyle(this, VisualStyles.MaterialDark);
}

private void LightModeButton_Click(object sender, RoutedEventArgs e)
{
    SfSkinManager.SetVisualStyle(this, VisualStyles.MaterialLight);
}
```

## Custom Themes (ThemeStudio)

Use [ThemeStudio](https://themebuilder.syncfusion.com) to create custom themes:
1. Select "Syncfusion WPF" platform
2. Customize colors, fonts, component styles
3. Export theme file (.xaml)

**Apply custom theme:**
```csharp
ThemeSettings.RegisterTheme("CustomTheme", "pack://application:,,,/Themes/CustomTheme.xaml");
SfSkinManager.SetVisualStyle(MainWindow, "CustomTheme");
```

## Styling Scenarios

**Display-only (read-only):**
```xml
<syncfusion:TimeSpanEdit Value="5.12:30:45" IsReadOnly="True" Background="#F5F5F5" Foreground="#333333"/>
```

**Data entry form:**
```xml
<syncfusion:TimeSpanEdit Value="0.08:00:00" Format="h 'hrs' m 'mins'" Background="White" SelectionBrush="#2196F3"/>
```

**Error state:**
```xml
<syncfusion:TimeSpanEdit Foreground="Red" Background="#FFE0E0" SelectionBrush="DarkRed"/>
```

**High contrast (accessibility):**
```xml
<syncfusion:TimeSpanEdit Background="White" Foreground="Black" SelectionBrush="Black"/>
```


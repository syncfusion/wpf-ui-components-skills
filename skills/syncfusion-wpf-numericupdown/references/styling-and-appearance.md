# Styling and Appearance in WPF NumericUpdown

## Table of Contents
- [Positive Value Styling](#positive-value-styling)
- [Negative Value Styling](#negative-value-styling)
- [Zero Value Styling](#zero-value-styling)
- [Focused State Styling](#focused-state-styling)
- [Theme Integration](#theme-integration)
- [Custom Theme Creation](#custom-theme-creation)

## Positive Value Styling

### Basic Positive Value Colors

Customize the background and foreground colors for positive numeric values using the `Background` and `Foreground` properties.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  Background="MediumBlue" 
                  Foreground="White" 
                  Value="10" 
                  Height="25" 
                  Width="100" />
```

**C#:**
```csharp
UpDown updown = new UpDown();
updown.Height = 25;
updown.Width = 100;
updown.Value = 10;
updown.Background = Brushes.MediumBlue;
updown.Foreground = Brushes.White;
grid.Children.Add(updown);
```

### Color Property Options

Use any WPF Brush or named color:

```csharp
// Named colors
updown.Background = Brushes.LightGray;
updown.Foreground = Brushes.Black;

// Color with transparency
updown.Background = new SolidColorBrush(Color.FromArgb(100, 0, 128, 255));

// Hex colors
updown.Background = new SolidColorBrush((Color)ColorConverter.ConvertFromString("#FF0099"));
```

### Common Positive Value Styling Patterns

**Success/Positive Indicator:**
```csharp
updown.Background = Brushes.LightGreen;
updown.Foreground = Brushes.DarkGreen;
```

**Professional Blue Theme:**
```csharp
updown.Background = Brushes.AliceBlue;
updown.Foreground = Brushes.MidnightBlue;
```

**High Contrast:**
```csharp
updown.Background = Brushes.Black;
updown.Foreground = Brushes.Yellow;
```

## Negative Value Styling

### EnableNegativeColors Property

Enable special styling for negative values separately from positive values.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  EnableNegativeColors="True" 
                  NegativeBackground="Yellow" 
                  NegativeForeground="BlueViolet" 
                  Value="-2" 
                  Height="25" 
                  Width="100" />
```

**C#:**
```csharp
updown.Value = -2;
updown.EnableNegativeColors = true;
updown.NegativeBackground = Brushes.Yellow;
updown.NegativeForeground = Brushes.BlueViolet;
```

### Negative Styling Properties

- **NegativeBackground**: Background color for negative values
- **NegativeForeground**: Text color for negative values
- **EnableNegativeColors**: Must be True to apply negative styling

### Common Negative Value Patterns

**Alert/Warning Style:**
```csharp
updown.EnableNegativeColors = true;
updown.NegativeBackground = Brushes.LightCoral;
updown.NegativeForeground = Brushes.DarkRed;
```

**Loss Indicator (Financial):**
```csharp
updown.EnableNegativeColors = true;
updown.NegativeBackground = Brushes.MistyRose;
updown.NegativeForeground = Brushes.Red;
```

**Subtle Negative:**
```csharp
updown.EnableNegativeColors = true;
updown.NegativeBackground = Brushes.WhiteSmoke;
updown.NegativeForeground = Brushes.Gray;
```

### Complete Positive/Negative Example

```xaml
<StackPanel Margin="20" Spacing="10">
    <TextBlock Text="Profit/Loss Display" FontSize="14" FontWeight="Bold"/>
    
    <!-- Positive (Profit) -->
    <StackPanel>
        <TextBlock Text="Profit:"/>
        <syncfusion:UpDown Name="profitUpDown"
                          Background="LightGreen"
                          Foreground="DarkGreen"
                          Value="5000"
                          Height="25"
                          Width="150"/>
    </StackPanel>
    
    <!-- Negative (Loss) -->
    <StackPanel>
        <TextBlock Text="Loss:"/>
        <syncfusion:UpDown Name="lossUpDown"
                          EnableNegativeColors="True"
                          NegativeBackground="LightCoral"
                          NegativeForeground="DarkRed"
                          Value="-2500"
                          Height="25"
                          Width="150"/>
    </StackPanel>
</StackPanel>
```

## Zero Value Styling

### ApplyZeroColor Property

Apply special styling specifically for the value zero.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  ApplyZeroColor="True" 
                  ZeroColor="DarkViolet" 
                  Value="0" 
                  Height="25" 
                  Width="100" />
```

**C#:**
```csharp
updown.ApplyZeroColor = true;
updown.ZeroColor = Brushes.DarkViolet;
```

### Zero Styling Properties

- **ApplyZeroColor**: Must be True to apply zero styling
- **ZeroColor**: The color (foreground) applied when value is exactly 0

### Zero Value Use Cases

**Neutral/Reset Indicator:**
```csharp
updown.ApplyZeroColor = true;
updown.ZeroColor = Brushes.Gray;
```

**Break-even Point:**
```csharp
updown.ApplyZeroColor = true;
updown.ZeroColor = Brushes.Orange;
```

**Special Highlight:**
```csharp
updown.ApplyZeroColor = true;
updown.ZeroColor = Brushes.Purple;
```

### Three-State Color Styling

Combine positive, negative, and zero colors for comprehensive value indication:

```csharp
// Positive values - Green
updown.Background = Brushes.LightGreen;
updown.Foreground = Brushes.DarkGreen;

// Negative values - Red
updown.EnableNegativeColors = true;
updown.NegativeBackground = Brushes.LightCoral;
updown.NegativeForeground = Brushes.DarkRed;

// Zero value - Gray
updown.ApplyZeroColor = true;
updown.ZeroColor = Brushes.Gray;
```

**Display Behavior:**
- Value = 100: Green background with dark green text
- Value = -50: Light coral background with dark red text
- Value = 0: Gray text (default background)

## Focused State Styling

### EnableFocusedColors Property

Apply special styling when the UpDown control receives keyboard focus.

**XAML:**
```xaml
<syncfusion:UpDown Name="upDown" 
                  EnableFocusedColors="True" 
                  FocusedBackground="BurlyWood" 
                  FocusedForeground="White" 
                  FocusedBorderBrush="Green" 
                  Value="10" 
                  Height="25" 
                  Width="100" />
```

**C#:**
```csharp
updown.Value = 10;
updown.EnableFocusedColors = true;
updown.FocusedBackground = Brushes.BurlyWood;
updown.FocusedForeground = Brushes.White;
updown.FocusedBorderBrush = Brushes.Green;
```

### Focused State Properties

- **EnableFocusedColors**: Default True. Set to False to disable focus styling
- **FocusedBackground**: Background color when focused
- **FocusedForeground**: Text color when focused
- **FocusedBorderBrush**: Border color when focused

### Important Behavior

When the control is focused, the **positive, negative, and zero value colors are temporarily replaced** by the focused colors. When focus is lost, the original value-based colors are restored.

### Common Focused State Patterns

**Highlight on Focus:**
```csharp
updown.EnableFocusedColors = true;
updown.FocusedBackground = Brushes.LightYellow;
updown.FocusedBorderBrush = Brushes.Orange;
```

**Prominent Focus Indicator:**
```csharp
updown.EnableFocusedColors = true;
updown.FocusedBackground = Brushes.Cyan;
updown.FocusedForeground = Brushes.Black;
updown.FocusedBorderBrush = Brushes.Blue;
```

**Subtle Focus Indicator:**
```csharp
updown.EnableFocusedColors = true;
updown.FocusedBorderBrush = Brushes.LightGray;
// FocusedBackground and FocusedForeground use defaults
```

### Complete Styling with Focus Example

```csharp
public void SetupCompleteStyleing()
{
    // Positive values
    updown.Background = Brushes.LightGreen;
    updown.Foreground = Brushes.DarkGreen;
    
    // Negative values
    updown.EnableNegativeColors = true;
    updown.NegativeBackground = Brushes.LightCoral;
    updown.NegativeForeground = Brushes.DarkRed;
    
    // Zero value
    updown.ApplyZeroColor = true;
    updown.ZeroColor = Brushes.Gray;
    
    // Focused state (overrides above colors)
    updown.EnableFocusedColors = true;
    updown.FocusedBackground = Brushes.LightBlue;
    updown.FocusedForeground = Brushes.Black;
    updown.FocusedBorderBrush = Brushes.Blue;
}

// Interaction:
// 1. User clicks on UpDown with value 50
// 2. Focus acquired -> Shows blue background (FocusedBackground)
// 3. User exits (Tab to next control)
// 4. Focus lost -> Shows green background (positive color) again
```

## Theme Integration

### SfSkinManager

Apply built-in themes to the entire UpDown control and application.

**XAML:**
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        syncfusion:SfSkinManager.VisualStyle="MaterialLight">
    <Grid>
        <syncfusion:UpDown Name="upDown" Height="25" Width="100"/>
    </Grid>
</Window>
```

**C#:**
```csharp
// Apply theme to a specific control
SfSkinManager.SetVisualStyle(updown, VisualStyles.MaterialLight);

// Apply theme to entire window
SfSkinManager.SetVisualStyle(this, VisualStyles.MaterialLight);
```

### Available Built-in Themes

- **MaterialLight** - Material Design light theme
- **MaterialDark** - Material Design dark theme
- **Office2019Colorful** - Office 2019 colorful theme
- **Office2019Black** - Office 2019 dark theme
- **Office2019HighContrast** - High contrast for accessibility
- **FluentLight** - Fluent light theme
- **FluentDark** - Fluent dark theme

### Theme Application Examples

**Material Light Theme:**
```xaml
<Window syncfusion:SfSkinManager.VisualStyle="MaterialLight">
```

**Office Dark Theme:**
```xaml
<Window syncfusion:SfSkinManager.VisualStyle="Office2019Black">
```

**Fluent Theme:**
```xaml
<Window syncfusion:SfSkinManager.VisualStyle="FluentDark">
```

### Apply Theme Only to UpDown

```csharp
// Theme the specific UpDown control
SfSkinManager.SetVisualStyle(upDown, VisualStyles.MaterialDark);

// Theme multiple controls
foreach (var updown in GetUpDownControls())
{
    SfSkinManager.SetVisualStyle(updown, VisualStyles.FluentLight);
}
```

## Custom Theme Creation

### Using Theme Studio

Create custom themes tailored to your application branding.

**Steps to Create Custom Theme:**

1. **Launch Theme Studio**
   - Open Visual Studio
   - Go to Extensions menu → Syncfusion → Launch Theme Studio

2. **Select Component**
   - Choose UpDown component
   - Select base theme (Material, Office, Fluent, etc.)

3. **Customize Colors**
   - Modify primary, secondary, accent colors
   - Adjust background and foreground
   - Customize focus and hover states
   - Set borders and separators

4. **Export Theme**
   - Export as XAML resource dictionary
   - Save to your project

5. **Apply Custom Theme**

**XAML:**
```xaml
<Window.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/Themes/CustomUpDownTheme.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Window.Resources>
```

### Theme Studio Properties for UpDown

Customize in Theme Studio:
- Control background color
- Text/foreground color
- Border color and thickness
- Focus indicator colors
- Spin button appearance
- Hover state colors
- Disabled state appearance

### Example Custom Theme Application

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Window.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <!-- Custom theme -->
                <ResourceDictionary Source="pack://application:,,,/Themes/MyCustomTheme.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Window.Resources>
    
    <Grid>
        <!-- Uses custom theme automatically -->
        <syncfusion:UpDown Name="upDown" Height="25" Width="100"/>
    </Grid>
</Window>
```

### Combining Custom Colors with Themes

```csharp
// Apply theme
SfSkinManager.SetVisualStyle(updown, VisualStyles.MaterialLight);

// Override with custom colors if needed
updown.Foreground = Brushes.CustomColor;
updown.Background = Brushes.CustomBackground;
```

## Complete Styling Example

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        syncfusion:SfSkinManager.VisualStyle="MaterialLight">
    
    <StackPanel Margin="20" Spacing="20">
        <TextBlock Text="UpDown Styling Examples" FontSize="16" FontWeight="Bold"/>
        
        <!-- Basic Positive Styling -->
        <StackPanel>
            <TextBlock Text="Positive Values (Green)"/>
            <syncfusion:UpDown Background="LightGreen"
                              Foreground="DarkGreen"
                              Value="100"
                              Height="30"
                              Width="150"/>
        </StackPanel>
        
        <!-- Negative with Special Colors -->
        <StackPanel>
            <TextBlock Text="With Negative Styling (Red for negative)"/>
            <syncfusion:UpDown EnableNegativeColors="True"
                              NegativeBackground="LightCoral"
                              NegativeForeground="DarkRed"
                              Value="-50"
                              Height="30"
                              Width="150"/>
        </StackPanel>
        
        <!-- All Three States -->
        <StackPanel>
            <TextBlock Text="Three-State Indicator"/>
            <syncfusion:UpDown Background="LightGreen"
                              Foreground="DarkGreen"
                              EnableNegativeColors="True"
                              NegativeBackground="LightCoral"
                              NegativeForeground="DarkRed"
                              ApplyZeroColor="True"
                              ZeroColor="Gray"
                              EnableFocusedColors="True"
                              FocusedBackground="LightBlue"
                              FocusedBorderBrush="Blue"
                              Value="0"
                              Height="30"
                              Width="150"/>
        </StackPanel>
        
    </StackPanel>
</Window>
```

---

**Next:** Read [advanced-configuration.md](advanced-configuration.md) for step intervals, animations, and complex scenarios.

# Customization in WPF Smart Text Editor

## Table of Contents
- [Overview](#overview)
- [Text Style Customization](#text-style-customization)
- [Placeholder Text and Color Customization](#placeholder-text-and-color-customization)
- [Suggestion Text Color](#suggestion-text-color)
- [Suggestion Popup Background](#suggestion-popup-background)
- [Maximum Input Length](#maximum-input-length)
- [Complete Customization Examples](#complete-customization-examples)

## Overview

This guide explains how to customize the appearance and behavior of the WPF Smart Text Editor (SfSmartTextEditor). You can control text styles, placeholder options, suggestion colors, and input validation to match your application's design and requirements.

**Customizable Elements:**
- Text style (font family, size, color, weight)
- Placeholder text and styling
- Inline suggestion appearance
- Popup suggestion background and styling
- Maximum character length

## Text Style Customization

Customize the text style and font using the `Style` property to match your application's design system. You can control font family, size, color, weight, and other text properties.

### Basic Text Style in XAML

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:smarttexteditor="clr-namespace:Syncfusion.UI.Xaml.SmartComponents;assembly=Syncfusion.SfSmartComponents.WPF">

    <smarttexteditor:SfSmartTextEditor>
        <smarttexteditor:SfSmartTextEditor.Style>
            <Style TargetType="{x:Type smarttexteditor:SfSmartTextEditor}">
                <Setter Property="FontSize" Value="16"/>
                <Setter Property="Foreground" Value="Blue"/>
            </Style>
        </smarttexteditor:SfSmartTextEditor.Style>
    </smarttexteditor:SfSmartTextEditor>
</Window>
```

### Advanced Text Style with Multiple Properties

```xaml
<smarttexteditor:SfSmartTextEditor>
    <smarttexteditor:SfSmartTextEditor.Style>
        <Style TargetType="{x:Type smarttexteditor:SfSmartTextEditor}">
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="FontFamily" Value="Segoe UI"/>
            <Setter Property="FontWeight" Value="Normal"/>
            <Setter Property="Foreground" Value="#333333"/>
            <Setter Property="Background" Value="#FFFFFF"/>
            <Setter Property="Padding" Value="10"/>
            <Setter Property="BorderBrush" Value="#CCCCCC"/>
            <Setter Property="BorderThickness" Value="1"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.Style>
</smarttexteditor:SfSmartTextEditor>
```

### Text Style in C#

```csharp
var smartTextEditor = new SfSmartTextEditor();

var style = new Style(typeof(SfSmartTextEditor));
style.Setters.Add(new Setter(SfSmartTextEditor.FontSizeProperty, 16.0));
style.Setters.Add(new Setter(SfSmartTextEditor.ForegroundProperty, new SolidColorBrush(Colors.Blue)));
style.Setters.Add(new Setter(SfSmartTextEditor.FontFamilyProperty, new FontFamily("Segoe UI")));

smartTextEditor.Style = style;
```

**Common Customizations:**
- Font size for readability
- Font family for brand consistency
- Foreground color for light/dark themes
- Background color for contrast
- Padding for comfortable typing area

## Placeholder Text and Color Customization

Add a helpful placeholder to guide users when the editor is empty. Use `PlaceholderStyle` to customize the placeholder's appearance.

### Basic Placeholder

```xaml
<smarttexteditor:SfSmartTextEditor 
    Placeholder="Write your message here..." />
```

### Placeholder with Custom Styling

```xaml
<smarttexteditor:SfSmartTextEditor 
    x:Name="smartTextEditor" 
    Placeholder="Write your message...">
    <smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
        <Style TargetType="{x:Type ContentControl}">
            <Setter Property="FontSize" Value="16"/>
            <Setter Property="Foreground" Value="#7E57C2"/>
            <Setter Property="FontStyle" Value="Italic"/>
            <Setter Property="Opacity" Value="0.7"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
</smarttexteditor:SfSmartTextEditor>
```

### Advanced Placeholder Styling

```xaml
<smarttexteditor:SfSmartTextEditor Placeholder="Type your response...">
    <smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
        <Style TargetType="{x:Type ContentControl}">
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="FontFamily" Value="Segoe UI"/>
            <Setter Property="Foreground" Value="#999999"/>
            <Setter Property="FontStyle" Value="Italic"/>
            <Setter Property="HorizontalAlignment" Value="Left"/>
            <Setter Property="VerticalAlignment" Value="Top"/>
            <Setter Property="Margin" Value="2,2,0,0"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
</smarttexteditor:SfSmartTextEditor>
```

### Placeholder in C#

```csharp
var smartTextEditor = new SfSmartTextEditor
{
    Placeholder = "Write your message..."
};

var placeholderStyle = new Style(typeof(ContentControl));
placeholderStyle.Setters.Add(new Setter(ContentControl.FontSizeProperty, 16.0));
placeholderStyle.Setters.Add(new Setter(ContentControl.ForegroundProperty, 
    new SolidColorBrush((Color)ColorConverter.ConvertFromString("#7E57C2"))));
placeholderStyle.Setters.Add(new Setter(ContentControl.FontStyleProperty, FontStyles.Italic));

smartTextEditor.PlaceholderStyle = placeholderStyle;
```

**Best Practices:**
- Use placeholder text to guide users on expected input
- Choose subtle colors (grays) to avoid distraction
- Consider italic or lighter font weight to distinguish from actual text
- Keep placeholder text concise and actionable

## Suggestion Text Color

Customize the color of inline suggestion text using the `SuggestionInlineStyle` property. This helps match your theme and improves readability by differentiating suggestions from user-entered text.

### Basic Suggestion Text Color

```xaml
<smarttexteditor:SfSmartTextEditor SuggestionDisplayMode="Inline">
    <smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
        <Style TargetType="{x:Type TextBlock}">
            <Setter Property="Foreground" Value="SkyBlue"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
</smarttexteditor:SfSmartTextEditor>
```

### Advanced Suggestion Text Styling

```xaml
<smarttexteditor:SfSmartTextEditor SuggestionDisplayMode="Inline">
    <smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
        <Style TargetType="{x:Type TextBlock}">
            <Setter Property="FontSize" Value="16"/>
            <Setter Property="Foreground" Value="#4A90E2"/>
            <Setter Property="FontStyle" Value="Italic"/>
            <Setter Property="Opacity" Value="0.6"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
</smarttexteditor:SfSmartTextEditor>
```

### Dark Theme Suggestion Styling

```xaml
<smarttexteditor:SfSmartTextEditor SuggestionDisplayMode="Inline">
    <smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
        <Style TargetType="{x:Type TextBlock}">
            <Setter Property="Foreground" Value="#87CEEB"/>
            <Setter Property="Opacity" Value="0.8"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
</smarttexteditor:SfSmartTextEditor>
```

**Design Considerations:**
- Use distinct colors to differentiate suggestions from regular text
- Ensure sufficient contrast for readability
- Consider opacity (0.5-0.8) to maintain visual hierarchy
- Match suggestion color to your application's accent color

## Suggestion Popup Background

Customize the background color and styling of the suggestion popup using the `SuggestionPopupStyle` property when using Popup mode. This allows you to align the popup with your application's design.

### Basic Popup Background

```xaml
<smarttexteditor:SfSmartTextEditor SuggestionDisplayMode="Popup">
    <smarttexteditor:SfSmartTextEditor.SuggestionPopupStyle>
        <Style TargetType="smarttexteditor:SuggestionPopup">
            <Setter Property="Background" Value="#0078D4" />
            <Setter Property="Foreground" Value="White" />
        </Style>
    </smarttexteditor:SfSmartTextEditor.SuggestionPopupStyle>
</smarttexteditor:SfSmartTextEditor>
```

### Advanced Popup Styling

```xaml
<smarttexteditor:SfSmartTextEditor SuggestionDisplayMode="Popup">
    <smarttexteditor:SfSmartTextEditor.SuggestionPopupStyle>
        <Style TargetType="smarttexteditor:SuggestionPopup">
            <Setter Property="Foreground" Value="White" />
            <Setter Property="Background" Value="#0078D4" />
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="FontWeight" Value="SemiBold"/>
            <Setter Property="Padding" Value="12,6"/>
            <Setter Property="BorderBrush" Value="#005A9E"/>
            <Setter Property="BorderThickness" Value="1"/>
            <Setter Property="CornerRadius" Value="4"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.SuggestionPopupStyle>
</smarttexteditor:SfSmartTextEditor>
```

### Material Design Popup

```xaml
<smarttexteditor:SfSmartTextEditor SuggestionDisplayMode="Popup">
    <smarttexteditor:SfSmartTextEditor.SuggestionPopupStyle>
        <Style TargetType="smarttexteditor:SuggestionPopup">
            <Setter Property="Background" Value="#6200EE" />
            <Setter Property="Foreground" Value="White" />
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="Padding" Value="16,8"/>
            <Setter Property="CornerRadius" Value="8"/>
            <Setter Property="Effect">
                <Setter.Value>
                    <DropShadowEffect Color="Black" Opacity="0.2" 
                                     ShadowDepth="2" BlurRadius="8"/>
                </Setter.Value>
            </Setter>
        </Style>
    </smarttexteditor:SfSmartTextEditor.SuggestionPopupStyle>
</smarttexteditor:SfSmartTextEditor>
```

**Popup Styling Tips:**
- Use high contrast between background and foreground
- Add padding for comfortable touch targets
- Consider rounded corners for modern appearance
- Use shadows for depth and visual separation

## Maximum Input Length

Set a limit on the number of characters users can enter using the `MaxLength` property. This is useful for validation, database constraints, or UI requirements.

### Setting MaxLength in XAML

```xaml
<smarttexteditor:SfSmartTextEditor
    MaxLength="500"
    Placeholder="Maximum 500 characters" />
```

### Setting MaxLength in C#

```csharp
var smartTextEditor = new SfSmartTextEditor
{
    MaxLength = 500,
    Placeholder = "Maximum 500 characters"
};
```

### MaxLength with Visual Feedback

```xaml
<StackPanel>
    <smarttexteditor:SfSmartTextEditor
        x:Name="smartTextEditor"
        MaxLength="1000"
        Placeholder="Enter your response (max 1000 characters)"
        TextChanged="OnTextChanged" />
    
    <TextBlock x:Name="characterCount" 
               Text="0 / 1000 characters"
               HorizontalAlignment="Right"
               Foreground="Gray"
               Margin="0,4,0,0"/>
</StackPanel>
```

**Code-behind:**
```csharp
private void OnTextChanged(object sender, TextChangedEventArgs e)
{
    int currentLength = smartTextEditor.Text?.Length ?? 0;
    int maxLength = smartTextEditor.MaxLength;
    characterCount.Text = $"{currentLength} / {maxLength} characters";
    
    // Change color when approaching limit
    if (currentLength >= maxLength * 0.9)
        characterCount.Foreground = new SolidColorBrush(Colors.Red);
    else if (currentLength >= maxLength * 0.75)
        characterCount.Foreground = new SolidColorBrush(Colors.Orange);
    else
        characterCount.Foreground = new SolidColorBrush(Colors.Gray);
}
```

**Use Cases:**
- Social media posts (280 characters for tweets)
- SMS messages (160 characters)
- Database field constraints (VARCHAR limits)
- Form validation
- User experience (prevent overwhelming input)

## Complete Customization Examples

### Example 1: Professional Email Editor

```xaml
<smarttexteditor:SfSmartTextEditor
    Placeholder="Compose your professional email..."
    UserRole="Professional email author"
    SuggestionDisplayMode="Inline"
    MaxLength="5000">
    
    <!-- Text Style -->
    <smarttexteditor:SfSmartTextEditor.Style>
        <Style TargetType="{x:Type smarttexteditor:SfSmartTextEditor}">
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="FontFamily" Value="Calibri"/>
            <Setter Property="Foreground" Value="#2C3E50"/>
            <Setter Property="Background" Value="White"/>
            <Setter Property="Padding" Value="12"/>
            <Setter Property="BorderBrush" Value="#BDC3C7"/>
            <Setter Property="BorderThickness" Value="1"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.Style>
    
    <!-- Placeholder Style -->
    <smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
        <Style TargetType="{x:Type ContentControl}">
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="Foreground" Value="#95A5A6"/>
            <Setter Property="FontStyle" Value="Italic"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
    
    <!-- Suggestion Style -->
    <smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
        <Style TargetType="{x:Type TextBlock}">
            <Setter Property="Foreground" Value="#3498DB"/>
            <Setter Property="Opacity" Value="0.7"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
</smarttexteditor:SfSmartTextEditor>
```

### Example 2: Modern Dark Theme Editor

```xaml
<smarttexteditor:SfSmartTextEditor
    Placeholder="Start typing..."
    UserRole="Content creator"
    SuggestionDisplayMode="Popup"
    MaxLength="2000">
    
    <!-- Text Style (Dark Theme) -->
    <smarttexteditor:SfSmartTextEditor.Style>
        <Style TargetType="{x:Type smarttexteditor:SfSmartTextEditor}">
            <Setter Property="FontSize" Value="15"/>
            <Setter Property="FontFamily" Value="Consolas"/>
            <Setter Property="Foreground" Value="#E0E0E0"/>
            <Setter Property="Background" Value="#1E1E1E"/>
            <Setter Property="Padding" Value="15"/>
            <Setter Property="BorderBrush" Value="#3E3E3E"/>
            <Setter Property="BorderThickness" Value="1"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.Style>
    
    <!-- Placeholder Style -->
    <smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
        <Style TargetType="{x:Type ContentControl}">
            <Setter Property="FontSize" Value="15"/>
            <Setter Property="Foreground" Value="#6E6E6E"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
    
    <!-- Popup Style -->
    <smarttexteditor:SfSmartTextEditor.SuggestionPopupStyle>
        <Style TargetType="smarttexteditor:SuggestionPopup">
            <Setter Property="Background" Value="#007ACC" />
            <Setter Property="Foreground" Value="White" />
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="Padding" Value="10,6"/>
            <Setter Property="BorderBrush" Value="#005A9E"/>
            <Setter Property="BorderThickness" Value="1"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.SuggestionPopupStyle>
</smarttexteditor:SfSmartTextEditor>
```

### Example 3: Customer Support Ticket Editor

```xaml
<smarttexteditor:SfSmartTextEditor
    Placeholder="Write your response to the customer..."
    UserRole="Customer support specialist"
    SuggestionDisplayMode="Inline"
    MaxLength="1500">
    
    <smarttexteditor:SfSmartTextEditor.Style>
        <Style TargetType="{x:Type smarttexteditor:SfSmartTextEditor}">
            <Setter Property="FontSize" Value="13"/>
            <Setter Property="FontFamily" Value="Segoe UI"/>
            <Setter Property="Foreground" Value="#212121"/>
            <Setter Property="Padding" Value="10"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.Style>
    
    <smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
        <Style TargetType="{x:Type ContentControl}">
            <Setter Property="Foreground" Value="#9E9E9E"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.PlaceholderStyle>
    
    <smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
        <Style TargetType="{x:Type TextBlock}">
            <Setter Property="Foreground" Value="#4CAF50"/>
            <Setter Property="Opacity" Value="0.65"/>
        </Style>
    </smarttexteditor:SfSmartTextEditor.SuggestionInlineStyle>
    
    <smarttexteditor:SfSmartTextEditor.UserPhrases>
        <x:String>Thank you for reaching out to our support team.</x:String>
        <x:String>We've received your inquiry and are investigating.</x:String>
        <x:String>Could you please provide additional details?</x:String>
    </smarttexteditor:SfSmartTextEditor.UserPhrases>
</smarttexteditor:SfSmartTextEditor>
```

---

**Related Topics:**
- Configure suggestion display modes: [suggestion-display-modes.md](suggestion-display-modes.md)
- Set up AI services: [ai-service-configuration.md](ai-service-configuration.md)
- Implement commands and events: [commands-and-events.md](commands-and-events.md)

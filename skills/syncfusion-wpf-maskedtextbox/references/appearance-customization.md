# Appearance and Customization

This guide covers visual customization options for SfMaskedEdit including colors, borders, watermarks, and theming.

<!-- Table of contents removed for cross-environment compatibility; see section headings below -->

## Background Customization

### Setting Background Color

```xml
<syncfusion:SfMaskedEdit Background="LightYellow" x:Name="maskedEdit"/>
```

```csharp
SfMaskedEdit maskedEdit = new SfMaskedEdit();
maskedEdit.Background = Brushes.LightYellow;
```

**Default:** White

### Setting Selection Highlight Color

The `SelectionBrush` property controls the color of selected text:

```xml
<syncfusion:SfMaskedEdit 
    Background="White"
    SelectionBrush="LightBlue"
    x:Name="maskedEdit"/>
```

```csharp
maskedEdit.SelectionBrush = Brushes.LightBlue;
```

**Default:** Royal Blue

### Example: Custom Background Scheme

```xml
<StackPanel>
    <!-- Light theme -->
    <syncfusion:SfMaskedEdit 
        Background="White"
        SelectionBrush="DodgerBlue"
        Mask="\d{4}"
        MaskType="RegEx"
        Margin="5"/>
    
    <!-- Dark theme -->
    <syncfusion:SfMaskedEdit 
        Background="#2D2D30"
        Foreground="White"
        SelectionBrush="Orange"
        Mask="\d{4}"
        MaskType="RegEx"
        Margin="5"/>
    
    <!-- Accent theme -->
    <syncfusion:SfMaskedEdit 
        Background="#FFF8DC"
        SelectionBrush="#FFD700"
        Mask="\d{4}"
        MaskType="RegEx"
        Margin="5"/>
</StackPanel>
```

## Foreground Customization

### Setting Text Color

```xml
<syncfusion:SfMaskedEdit Foreground="DarkBlue" x:Name="maskedEdit"/>
```

```csharp
maskedEdit.Foreground = Brushes.DarkBlue;
```

**Default:** Black

### Setting Caret Color

The `CaretBrush` property controls the cursor color:

```xml
<syncfusion:SfMaskedEdit 
    Foreground="Black"
    CaretBrush="Red"
    x:Name="maskedEdit"/>
```

```csharp
maskedEdit.CaretBrush = Brushes.Red;
```

**Default:** null (uses system default)

### Example: Complete Foreground Styling

```xml
<syncfusion:SfMaskedEdit 
    Foreground="Navy"
    CaretBrush="OrangeRed"
    Background="AliceBlue"
    SelectionBrush="LightSteelBlue"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"/>
```

## Border Customization

### Setting Border Color

```xml
<syncfusion:SfMaskedEdit 
    BorderBrush="Blue"
    BorderThickness="2"
    x:Name="maskedEdit"/>
```

```csharp
maskedEdit.BorderBrush = Brushes.Blue;
maskedEdit.BorderThickness = new Thickness(2);
```

**Default:** Lavender (BorderBrush), 1 (BorderThickness)

### Custom Border Styles

```xml
<!-- Thick border -->
<syncfusion:SfMaskedEdit 
    BorderBrush="DarkGray"
    BorderThickness="3"
    Mask="\d+"
    MaskType="RegEx"/>

<!-- No border -->
<syncfusion:SfMaskedEdit 
    BorderThickness="0"
    Background="WhiteSmoke"
    Mask="\d+"
    MaskType="RegEx"/>

<!-- Asymmetric border (left accent) -->
<syncfusion:SfMaskedEdit 
    BorderBrush="DodgerBlue"
    BorderThickness="4,1,1,1"
    Mask="\d+"
    MaskType="RegEx"/>
```

### Focus Visual Style

Customize appearance when control has focus:

```xml
<syncfusion:SfMaskedEdit x:Name="maskedEdit">
    <syncfusion:SfMaskedEdit.Style>
        <Style TargetType="syncfusion:SfMaskedEdit">
            <Style.Triggers>
                <Trigger Property="IsFocused" Value="True">
                    <Setter Property="BorderBrush" Value="DodgerBlue"/>
                    <Setter Property="BorderThickness" Value="2"/>
                </Trigger>
            </Style.Triggers>
        </Style>
    </syncfusion:SfMaskedEdit.Style>
</syncfusion:SfMaskedEdit>
```

## Error Indication

### Error Border Color

When validation fails (`HasError` is true), an error border is displayed:

```xml
<syncfusion:SfMaskedEdit 
    ErrorBorderBrush="Red"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"
    x:Name="phoneInput"/>
```

```csharp
phoneInput.ErrorBorderBrush = Brushes.Red;
```

**Default:** Red

### Custom Error Styles

```xml
<!-- Orange error border -->
<syncfusion:SfMaskedEdit 
    ErrorBorderBrush="Orange"
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
    MaskType="RegEx"/>

<!-- Thick red error border -->
<syncfusion:SfMaskedEdit 
    ErrorBorderBrush="DarkRed"
    BorderThickness="3"
    Mask="\d{5}-\d{4}"
    MaskType="RegEx"/>
```

### Example: Error with Visual Feedback

```xml
<StackPanel>
    <syncfusion:SfMaskedEdit 
        x:Name="emailInput"
        Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
        MaskType="RegEx"
        ErrorBorderBrush="Red"
        ValidationMode="LostFocus"
        LostFocus="EmailInput_LostFocus"
        Width="250"
        HorizontalAlignment="Left"/>
    
    <TextBlock x:Name="errorMessage" 
               Foreground="Red" 
               Visibility="Collapsed"
               Text="Invalid email format"
               Margin="5,2,0,0"/>
</StackPanel>
```

```csharp
private void EmailInput_LostFocus(object sender, RoutedEventArgs e)
{
    if (emailInput.HasError)
    {
        errorMessage.Visibility = Visibility.Visible;
        emailInput.ErrorBorderBrush = Brushes.DarkRed;
    }
    else
    {
        errorMessage.Visibility = Visibility.Collapsed;
    }
}
```

## Flow Direction (RTL Support)

### Right-to-Left Layout

For languages like Arabic, Hebrew, Persian:

```xml
<syncfusion:SfMaskedEdit 
    FlowDirection="RightToLeft"
    Mask="\d+"
    MaskType="RegEx"
    x:Name="maskedEdit"/>
```

```csharp
maskedEdit.FlowDirection = FlowDirection.RightToLeft;
```

**Default:** LeftToRight

### Example: Bilingual Form

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    
    <!-- Left-to-right (English) -->
    <StackPanel Grid.Column="0" Margin="10">
        <TextBlock Text="Phone Number:" FontWeight="Bold"/>
        <syncfusion:SfMaskedEdit 
            FlowDirection="LeftToRight"
            Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
            MaskType="RegEx"/>
    </StackPanel>
    
    <!-- Right-to-left (Arabic) -->
    <StackPanel Grid.Column="1" Margin="10">
        <TextBlock Text="رقم الهاتف:" FontWeight="Bold" FlowDirection="RightToLeft"/>
        <syncfusion:SfMaskedEdit 
            FlowDirection="RightToLeft"
            Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
            MaskType="RegEx"/>
    </StackPanel>
</Grid>
```

## Watermark Configuration

### Basic Watermark Text

Display instructional text when control is empty:

```xml
<syncfusion:SfMaskedEdit 
    Watermark="Enter phone number"
    Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
    MaskType="RegEx"
    x:Name="phoneInput"/>
```

```csharp
phoneInput.Watermark = "Enter phone number";
```

**Default:** null (no watermark)

### When Watermark is Visible

Watermark is shown when:
- Control is not focused
- No valid characters have been entered
- Value is null or empty

Watermark is hidden when:
- Control has focus (unless ShowPromptOnFocus is false and no input yet)
- User has entered any valid character

### Example: Multiple Watermarks

```xml
<StackPanel Spacing="10">
    <syncfusion:SfMaskedEdit 
        Watermark="(___) ___-____"
        Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
        MaskType="RegEx"/>
    
    <syncfusion:SfMaskedEdit 
        Watermark="example@domain.com"
        Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
        MaskType="RegEx"/>
    
    <syncfusion:SfMaskedEdit 
        Watermark="MM/DD/YYYY"
        Mask="(0[1-9]|1[0-2])/(0[1-9]|[12][0-9]|3[01])/\d{4}"
        MaskType="RegEx"/>
    
    <syncfusion:SfMaskedEdit 
        Watermark="$ 0.00"
        Mask="$ \d+\.\d{2}"
        MaskType="RegEx"/>
</StackPanel>
```

## Custom Watermark Template

### Basic Custom Template

```xml
<syncfusion:SfMaskedEdit 
    Watermark="Enter email"
    Mask="[A-Za-z0-9._%-]+@[A-Za-z0-9]+\.[A-Za-z]{2,3}"
    MaskType="RegEx">
    <syncfusion:SfMaskedEdit.WatermarkTemplate>
        <DataTemplate>
            <TextBlock 
                Text="{Binding}" 
                Foreground="Gray"
                FontStyle="Italic"/>
        </DataTemplate>
    </syncfusion:SfMaskedEdit.WatermarkTemplate>
</syncfusion:SfMaskedEdit>
```

### Styled Watermark Template

```xml
<syncfusion:SfMaskedEdit 
    Watermark="Type here"
    Mask="\d+"
    MaskType="RegEx">
    <syncfusion:SfMaskedEdit.WatermarkTemplate>
        <DataTemplate>
            <Border Background="Yellow" Padding="2">
                <TextBlock 
                    Text="{Binding}" 
                    Foreground="Blue"
                    FontWeight="Bold"
                    TextAlignment="Center"/>
            </Border>
        </DataTemplate>
    </syncfusion:SfMaskedEdit.WatermarkTemplate>
</syncfusion:SfMaskedEdit>
```

### Watermark with Icon (compact)

Use a simple watermark template with an icon and text. Keep templates concise to reduce maintenance overhead.

```xml
<syncfusion:SfMaskedEdit Watermark="Enter phone number"
                         Mask="\([0-9]\d{2}\) [0-9]\d{2}-[0-9]\d{3}"
                         MaskType="RegEx" Width="250">
    <syncfusion:SfMaskedEdit.WatermarkTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="📞" Margin="0,0,6,0"/>
                <TextBlock Text="{Binding}" Foreground="Gray"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfMaskedEdit.WatermarkTemplate>
</syncfusion:SfMaskedEdit>
```

## Theme Integration
<!-- Theme and full styling sections moved to appearance-customization-themes.md -->

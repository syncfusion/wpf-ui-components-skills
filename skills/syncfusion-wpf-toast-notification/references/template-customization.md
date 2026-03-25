# Template Customization

## Table of Contents
- [Overview](#overview)
- [TitleTemplate Customization](#titletemplate-customization)
- [ContentTemplate Customization](#contenttemplate-customization)
- [CloseButtonTemplate Customization](#closebuttontemplate-customization)
- [Complete Custom Toast Example](#complete-custom-toast-example)
- [Advanced Customization Scenarios](#advanced-customization-scenarios)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Overview

SfToastNotification provides full template customization capabilities for Window and Screen modes. You can customize:
- **TitleTemplate:** The title/header section
- **ContentTemplate:** The message/content area
- **CloseButtonTemplate:** The close button appearance
- **ActionTemplate:** Action buttons (covered in action-buttons.md)

**Requirements:**
- Mode must be Window or Screen (not Default)
- Templates defined in XAML as DataTemplate resources
- Templates bound to respective properties in ToastOptions

---

## TitleTemplate Customization

### Basic Title Template

Define a custom template for the title section:

```xml
<!-- MainWindow.xaml -->
<Window.Resources>
    <!-- Custom Title Style -->
    <Style x:Key="ToastTitleStyle" TargetType="TextBlock">
        <Setter Property="FontSize" Value="18"/>
        <Setter Property="FontWeight" Value="Bold"/>
        <Setter Property="Foreground" Value="#2C3E50"/>
        <Setter Property="Margin" Value="0,0,0,8"/>
    </Style>
    
    <!-- Custom Title Template -->
    <DataTemplate x:Key="CustomTitleTemplate">
        <TextBlock Style="{StaticResource ToastTitleStyle}"
                   Text="{Binding Title}"
                   TextWrapping="Wrap"
                   HorizontalAlignment="Left"/>
    </DataTemplate>
</Window.Resources>
```

### Using Custom Title Template

```csharp
// Apply custom title template
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Custom Title Style",
    Message = "This toast has a custom title template.",
    Mode = ToastMode.Screen,
    TitleTemplate = (DataTemplate)Application.Current.Resources["CustomTitleTemplate"]
});
```

### Title Template with Icon

```xml
<!-- Title template with icon -->
<DataTemplate x:Key="TitleWithIconTemplate">
    <StackPanel Orientation="Horizontal">
        <!-- Icon -->
        <Path Width="20" Height="20"
              Margin="0,0,8,0"
              Fill="#3498DB"
              Data="M12,2A10,10 0 0,1 22,12A10,10 0 0,1 12,22A10,10 0 0,1 2,12A10,10 0 0,1 12,2M12,4A8,8 0 0,0 4,12A8,8 0 0,0 12,20A8,8 0 0,0 20,12A8,8 0 0,0 12,4M11,16.5L6.5,12L7.91,10.59L11,13.67L16.59,8.09L18,9.5L11,16.5Z"/>
        <!-- Title Text -->
        <TextBlock Text="{Binding Title}"
                   FontSize="16"
                   FontWeight="SemiBold"
                   VerticalAlignment="Center"/>
    </StackPanel>
</DataTemplate>
```

```csharp
// Use title with icon
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Success",
    Message = "Operation completed successfully.",
    Mode = ToastMode.Screen,
    Severity = ToastSeverity.Success,
    TitleTemplate = (DataTemplate)Application.Current.Resources["TitleWithIconTemplate"]
});
```

### Title Template with Severity-Based Styling

```xml
<!-- Dynamic title template with severity-based colors -->
<DataTemplate x:Key="SeverityTitleTemplate">
    <Border Padding="8,4"
            CornerRadius="4"
            Background="{Binding Severity, Converter={StaticResource SeverityToBrushConverter}}">
        <TextBlock Text="{Binding Title}"
                   FontWeight="Bold"
                   Foreground="White"/>
    </Border>
</DataTemplate>
```

---

## ContentTemplate Customization

### Basic Content Template

```xml
<!-- Custom content style -->
<Style x:Key="ToastContentStyle" TargetType="TextBlock">
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="Foreground" Value="#34495E"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineHeight" Value="20"/>
</Style>

<!-- Custom Content Template -->
<DataTemplate x:Key="CustomContentTemplate">
    <TextBlock Style="{StaticResource ToastContentStyle}"
               Text="{Binding Message}"
               MaxWidth="350"/>
</DataTemplate>
```

```csharp
// Apply custom content template
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Notification",
    Message = "This toast has custom content styling with proper text wrapping and spacing.",
    Mode = ToastMode.Screen,
    ContentTemplate = (DataTemplate)Application.Current.Resources["CustomContentTemplate"]
});
```

### Multi-Line Content Template

```xml
<!-- Multi-line content template -->
<DataTemplate x:Key="MultiLineContentTemplate">
    <StackPanel>
        <TextBlock Text="{Binding Message}"
                   FontSize="14"
                   FontWeight="Normal"
                   Margin="0,0,0,8"
                   TextWrapping="Wrap"/>
        <TextBlock Text="Additional information can be displayed here."
                   FontSize="12"
                   FontStyle="Italic"
                   Foreground="#7F8C8D"/>
    </StackPanel>
</DataTemplate>
```

### Rich Content Template

```xml
<!-- Rich content with bullet points -->
<DataTemplate x:Key="RichContentTemplate">
    <StackPanel>
        <TextBlock Text="{Binding Header}"
                   FontSize="14"
                   FontWeight="SemiBold"
                   Margin="0,0,0,8"/>
        
        <StackPanel>
            <TextBlock Margin="0,4">
                <Run Text="• "/>
                <Run Text="Item 1: Configuration updated"/>
            </TextBlock>
            <TextBlock Margin="0,4">
                <Run Text="• "/>
                <Run Text="Item 2: Cache cleared"/>
            </TextBlock>
            <TextBlock Margin="0,4">
                <Run Text="• "/>
                <Run Text="Item 3: Services restarted"/>
            </TextBlock>
        </StackPanel>
        
        <TextBlock Text="{Binding Message}"
                   Margin="0,8,0,0"
                   FontSize="12"
                   Foreground="#95A5A6"/>
    </StackPanel>
</DataTemplate>
```

```csharp
// Use rich content template
SfToastNotification.Show(this, new ToastOptions
{
    Header = "System Maintenance Complete",
    Message = "All services are now operational.",
    Mode = ToastMode.Screen,
    ContentTemplate = (DataTemplate)Application.Current.Resources["RichContentTemplate"]
});
```

### Content Template with Progress

```xml
<!-- Content with progress indicator -->
<DataTemplate x:Key="ProgressContentTemplate">
    <StackPanel>
        <TextBlock Text="{Binding Message}"
                   FontSize="14"
                   Margin="0,0,0,8"/>
        
        <ProgressBar Height="6"
                     Minimum="0"
                     Maximum="100"
                     Value="65"
                     Foreground="#3498DB"/>
        
        <TextBlock Text="65% complete"
                   FontSize="11"
                   Foreground="#7F8C8D"
                   Margin="0,4,0,0"
                   HorizontalAlignment="Right"/>
    </StackPanel>
</DataTemplate>
```

---

## CloseButtonTemplate Customization

### Basic Close Button Template

```xml
<!-- Custom close button template -->
<DataTemplate x:Key="CustomCloseButtonTemplate">
    <Border Width="24" Height="24"
            Background="Transparent"
            CornerRadius="12"
            Cursor="Hand">
        <Path Width="12" Height="12"
              Stretch="Uniform"
              StrokeThickness="2"
              Stroke="#95A5A6"
              Data="M1,1 L11,11 M11,1 L1,11"
              HorizontalAlignment="Center"
              VerticalAlignment="Center"/>
        <Border.Style>
            <Style TargetType="Border">
                <Style.Triggers>
                    <Trigger Property="IsMouseOver" Value="True">
                        <Setter Property="Background" Value="#ECF0F1"/>
                    </Trigger>
                </Style.Triggers>
            </Style>
        </Border.Style>
    </Border>
</DataTemplate>
```

```csharp
// Apply custom close button
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Custom Close Button",
    Message = "This toast has a custom close button style.",
    Mode = ToastMode.Screen,
    CloseButtonTemplate = (DataTemplate)Application.Current.Resources["CustomCloseButtonTemplate"]
});
```

### Styled Close Button

```xml
<!-- Modern close button with hover effect -->
<DataTemplate x:Key="ModernCloseButtonTemplate">
    <Button Width="28" Height="28"
            Background="Transparent"
            BorderThickness="0"
            Cursor="Hand">
        <Button.Template>
            <ControlTemplate TargetType="Button">
                <Border Background="{TemplateBinding Background}"
                        CornerRadius="4">
                    <Path Width="14" Height="14"
                          Stretch="Uniform"
                          StrokeThickness="2.5"
                          Stroke="#34495E"
                          Data="M0,0 L14,14 M14,0 L0,14"
                          HorizontalAlignment="Center"
                          VerticalAlignment="Center"/>
                </Border>
                <ControlTemplate.Triggers>
                    <Trigger Property="IsMouseOver" Value="True">
                        <Setter Property="Background" Value="#E74C3C"/>
                    </Trigger>
                    <Trigger Property="IsMouseOver" Value="True">
                        <Setter TargetName="PART_Path" Property="Stroke" Value="White"/>
                    </Trigger>
                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Button.Template>
    </Button>
</DataTemplate>
```

### Icon-Based Close Button

```xml
<!-- Close button with FontAwesome-style icon -->
<DataTemplate x:Key="IconCloseButtonTemplate">
    <Button Width="32" Height="32"
            Background="#E74C3C"
            BorderThickness="0"
            CornerRadius="16">
        <TextBlock Text="×"
                   FontSize="24"
                   FontWeight="Bold"
                   Foreground="White"
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"/>
    </Button>
</DataTemplate>
```

---

## Complete Custom Toast Example

### Fully Customized Toast

```xml
<!-- MainWindow.xaml - Complete custom toast resources -->
<Window.Resources>
    <!-- Title Template -->
    <DataTemplate x:Key="BrandedTitleTemplate">
        <StackPanel Orientation="Horizontal">
            <Ellipse Width="32" Height="32"
                     Fill="#3498DB"
                     Margin="0,0,12,0"/>
            <StackPanel VerticalAlignment="Center">
                <TextBlock Text="{Binding Title}"
                           FontSize="16"
                           FontWeight="Bold"
                           Foreground="#2C3E50"/>
                <TextBlock Text="MyApp Notification"
                           FontSize="10"
                           Foreground="#7F8C8D"/>
            </StackPanel>
        </StackPanel>
    </DataTemplate>
    
    <!-- Content Template -->
    <DataTemplate x:Key="BrandedContentTemplate">
        <Border BorderBrush="#BDC3C7"
                BorderThickness="0,1,0,0"
                Padding="0,12,0,0"
                Margin="0,8,0,0">
            <TextBlock Text="{Binding Message}"
                       FontSize="14"
                       TextWrapping="Wrap"
                       Foreground="#34495E"
                       LineHeight="20"/>
        </Border>
    </DataTemplate>
    
    <!-- Close Button Template -->
    <DataTemplate x:Key="BrandedCloseButtonTemplate">
        <Border Width="28" Height="28"
                Background="#E74C3C"
                CornerRadius="14"
                Cursor="Hand">
            <TextBlock Text="×"
                       FontSize="20"
                       FontWeight="Bold"
                       Foreground="White"
                       HorizontalAlignment="Center"
                       VerticalAlignment="Center"/>
        </Border>
    </DataTemplate>
    
    <!-- Action Button Template -->
    <DataTemplate x:Key="BrandedActionTemplate">
        <Button Content="{Binding Label}"
                Background="#3498DB"
                Foreground="White"
                BorderThickness="0"
                Padding="16,8"
                Margin="4,0"
                FontWeight="SemiBold"
                Cursor="Hand">
            <Button.Template>
                <ControlTemplate TargetType="Button">
                    <Border Background="{TemplateBinding Background}"
                            CornerRadius="4"
                            Padding="{TemplateBinding Padding}">
                        <ContentPresenter HorizontalAlignment="Center"
                                        VerticalAlignment="Center"/>
                    </Border>
                </ControlTemplate>
            </Button.Template>
        </Button>
    </DataTemplate>
</Window.Resources>
```

```csharp
// Show fully customized toast
SfToastNotification.Show(this, new ToastOptions
{
    Title = "Welcome Back!",
    Message = "You have 3 new notifications and 5 unread messages.",
    Mode = ToastMode.Screen,
    Placement = ToastPlacement.TopRight,
    Duration = TimeSpan.FromSeconds(8),
    
    // Apply custom templates
    TitleTemplate = (DataTemplate)Application.Current.Resources["BrandedTitleTemplate"],
    ContentTemplate = (DataTemplate)Application.Current.Resources["BrandedContentTemplate"],
    CloseButtonTemplate = (DataTemplate)Application.Current.Resources["BrandedCloseButtonTemplate"],
    
    // Custom actions
    Actions = new List<ToastAction>
    {
        new ToastAction("View All")
        {
            ActionTemplate = (DataTemplate)Application.Current.Resources["BrandedActionTemplate"],
            Callback = () => OpenNotifications(),
            CloseOnClick = true
        },
        new ToastAction("Dismiss")
        {
            ActionTemplate = (DataTemplate)Application.Current.Resources["BrandedActionTemplate"],
            CloseOnClick = true
        }
    }
});
```

---

## Advanced Customization Scenarios

### Theme-Aware Templates

```xml
<!-- Light/Dark theme support -->
<DataTemplate x:Key="ThemeAwareTitleTemplate">
    <TextBlock Text="{Binding Title}"
               FontSize="16"
               FontWeight="Bold">
        <TextBlock.Style>
            <Style TargetType="TextBlock">
                <Setter Property="Foreground" Value="#2C3E50"/>
                <Style.Triggers>
                    <DataTrigger Binding="{Binding Source={x:Static SystemParameters.HighContrast}}" Value="True">
                        <Setter Property="Foreground" Value="White"/>
                    </DataTrigger>
                </Style.Triggers>
            </Style>
        </TextBlock.Style>
    </TextBlock>
</DataTemplate>
```

### Animated Content Template

```xml
<!-- Content template with animation -->
<DataTemplate x:Key="AnimatedContentTemplate">
    <StackPanel>
        <TextBlock Text="{Binding Message}"
                   FontSize="14"
                   x:Name="contentText">
            <TextBlock.Triggers>
                <EventTrigger RoutedEvent="Loaded">
                    <BeginStoryboard>
                        <Storyboard>
                            <DoubleAnimation Storyboard.TargetProperty="Opacity"
                                           From="0" To="1"
                                           Duration="0:0:0.5"/>
                        </Storyboard>
                    </BeginStoryboard>
                </EventTrigger>
            </TextBlock.Triggers>
        </TextBlock>
    </StackPanel>
</DataTemplate>
```

### Data-Bound Custom Content

```xml
<!-- Template with complex data binding -->
<DataTemplate x:Key="DataBoundContentTemplate">
    <StackPanel>
        <TextBlock Text="{Binding Header}"
                   FontSize="14"
                   FontWeight="Bold"
                   Margin="0,0,0,8"/>
        
        <ItemsControl ItemsSource="{Binding DataItems}">
            <ItemsControl.ItemTemplate>
                <DataTemplate>
                    <Border BorderBrush="#ECF0F1"
                            BorderThickness="0,0,0,1"
                            Padding="0,4">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="*"/>
                                <ColumnDefinition Width="Auto"/>
                            </Grid.ColumnDefinitions>
                            
                            <TextBlock Text="{Binding Name}"
                                     Grid.Column="0"/>
                            <TextBlock Text="{Binding Value}"
                                     Grid.Column="1"
                                     Foreground="#7F8C8D"/>
                        </Grid>
                    </Border>
                </DataTemplate>
            </ItemsControl.ItemTemplate>
        </ItemsControl>
    </StackPanel>
</DataTemplate>
```

---

## Edge Cases and Troubleshooting

### Issue: Template Not Applied

**Problem:** Custom template defined but default appearance shows.

**Possible Causes:**
1. Resource key mismatch
2. Resource not accessible
3. Mode is Default (templates not supported)

**Solution:**
```csharp
// Verify resource exists
if (Application.Current.Resources.Contains("CustomTitleTemplate"))
{
    var template = (DataTemplate)Application.Current.Resources["CustomTitleTemplate"];
    SfToastNotification.Show(this, new ToastOptions
    {
        Title = "Test",
        Mode = ToastMode.Screen,  // ✅ Not Default mode
        TitleTemplate = template
    });
}
```

### Issue: Binding Not Working

**Problem:** Data binding in template doesn't display data.

**Solution:** Ensure binding path matches ToastOptions properties:

```xml
<!-- CORRECT binding paths -->
<TextBlock Text="{Binding Title}"/>      <!-- ✅ Binds to ToastOptions.Title -->
<TextBlock Text="{Binding Message}"/>    <!-- ✅ Binds to ToastOptions.Message -->
<TextBlock Text="{Binding Header}"/>     <!-- ✅ Binds to ToastOptions.Header -->

<!-- WRONG binding paths -->
<TextBlock Text="{Binding Content}"/>    <!-- ❌ No Content property -->
<TextBlock Text="{Binding Text}"/>       <!-- ❌ No Text property -->
```

### Issue: Template Layout Issues

**Problem:** Template content overflows or doesn't fit properly.

**Solution:** Add proper sizing and wrapping:

```xml
<!-- Good template with proper sizing -->
<DataTemplate x:Key="ProperSizedTemplate">
    <StackPanel MaxWidth="400">  <!-- Limit width -->
        <TextBlock Text="{Binding Title}"
                   TextWrapping="Wrap"  <!-- Enable wrapping -->
                   MaxWidth="380"/>      <!-- Respect padding -->
        <TextBlock Text="{Binding Message}"
                   TextWrapping="Wrap"
                   TextTrimming="CharacterEllipsis"  <!-- Trim if too long -->
                   MaxHeight="100"/>     <!-- Limit height -->
    </StackPanel>
</DataTemplate>
```

### Issue: Template Affects Performance

**Problem:** Complex templates cause slowdown with many toasts.

**Solution:** Simplify templates or use virtualization:

```xml
<!-- Lightweight template -->
<DataTemplate x:Key="LightweightTemplate">
    <!-- Minimize elements -->
    <TextBlock Text="{Binding Title}"
               FontSize="14"
               FontWeight="SemiBold"/>
    <!-- Avoid complex layouts, heavy graphics, or animations -->
</DataTemplate>
```

## Best Practices

1. **Keep templates simple:** Avoid overly complex layouts that impact performance
2. **Test with various content lengths:** Ensure templates handle short and long text
3. **Use proper text wrapping:** Prevent overflow with TextWrapping="Wrap"
4. **Maintain accessibility:** Ensure sufficient contrast and font sizes
5. **Provide fallbacks:** Check if resources exist before applying templates
6. **Be consistent:** Use the same template style across your application
7. **Consider themes:** Support light/dark themes in custom templates
8. **Test responsiveness:** Ensure templates work at different window sizes

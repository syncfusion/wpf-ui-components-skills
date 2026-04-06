# Template Customization

## Table of Contents
- [Overview](#overview)
- [TitleTemplate Customization](#titletemplate-customization)
- [ContentTemplate Customization](#contenttemplate-customization)
- [CloseButtonTemplate Customization](#closebuttontemplate-customization)
- [Best Practices](#best-practices)

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

---

## ContentTemplate Customization

### Basic Content Template

Use custom styling for message content. Define a TextBlock style and bind it in ContentTemplate for text wrapping, font sizing, and color customization. Apply via ContentTemplate property in ToastOptions.



---

## CloseButtonTemplate Customization

### Close Button Template

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
                    <Path x:Name="PART_Path"
                          Width="14" Height="14"
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

---

## Best Practices

1. Define all templates in Window.Resources or Application.Resources
2. Use consistent naming for template keys (e.g., *Template suffix)
3. Bind to properties like `{Binding Title}` and `{Binding Message}`
4. Test templates with different message lengths
5. Apply templates via TitleTemplate, ContentTemplate, CloseButtonTemplate, ActionTemplate properties


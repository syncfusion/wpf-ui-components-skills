# UI Customization and Extensions

## Table of Contents

1. [Overview](#overview)
2. [Toolbar Visibility](#toolbar-visibility)
   - 2.1 [IsToolbarVisiblity Property](#istoolbarvisiblity-property)
   - 2.2 [Showing and Hiding the Toolbar](#showing-and-hiding-the-toolbar)
3. [Toolbar Customization](#toolbar-customization)
   - 3.1 [Adding Custom Items](#adding-custom-items)
   - 3.2 [Using IconTemplate](#using-icontemplate)
   - 3.3 [Customizing Colors](#customizing-colors)
   - 3.4 [Customizing Dimensions](#customizing-dimensions)
4. [Available Commands](#available-commands)
   - 4.1 [BrowseImage Command](#browseimage-command)
   - 4.2 [Save Command](#save-command)
   - 4.3 [Undo and Redo Commands](#undo-and-redo-commands)
   - 4.4 [Reset Command](#reset-command)
   - 4.5 [Zoom Commands](#zoom-commands)
   - 4.6 [Select and Pan Commands](#select-and-pan-commands)
5. [Creating Custom Toolbars](#creating-custom-toolbars)
   - 5.1 [CommandTarget Binding](#commandtarget-binding)
   - 5.2 [Building a Custom Toolbar](#building-a-custom-toolbar)
6. [Custom Views](#custom-views)
   - 6.1 [AddCustomView Method](#addcustomview-method)
   - 6.2 [CustomViewSettings Properties](#customviewsettings-properties)
   - 6.3 [Positioning and Sizing](#positioning-and-sizing)
   - 6.4 [Rotation and Aspect Ratio](#rotation-and-aspect-ratio)
7. [Use Cases](#use-cases)
   - 7.1 [Adding Watermarks](#adding-watermarks)
   - 7.2 [Adding Logos](#adding-logos)
   - 7.3 [Creating Overlays](#creating-overlays)
8. [Complete Working Examples](#complete-working-examples)
   - 8.1 [Example 1: Custom Toolbar with Commands](#example-1-custom-toolbar-with-commands)
   - 8.2 [Example 2: Adding Watermark with Custom View](#example-2-adding-watermark-with-custom-view)
   - 8.3 [Example 3: Fully Customized Image Editor](#example-3-fully-customized-image-editor)
9. [Troubleshooting](#troubleshooting)
   - 9.1 [Common Issues](#common-issues)
   - 9.2 [Best Practices](#best-practices)

---

## 1. Overview {#overview}

The Syncfusion WPF Image Editor `SfImageEditor` provides extensive UI customization capabilities that allow you to tailor the editing experience to your application''s specific needs. This documentation covers:

- **Toolbar Customization**: Control the visibility, appearance, and content of the built-in toolbar
- **Custom Commands**: Leverage predefined commands to build your own toolbar interfaces
- **Custom Views**: Add custom visual elements like watermarks, logos, and overlays to images
- **Extension Points**: Integrate custom functionality seamlessly into the editing workflow

These features enable you to:
- Create a consistent user experience across your application
- Add branding elements to edited images
- Build specialized image editing workflows
- Extend the editor with domain-specific functionality

## 2. Toolbar Visibility {#toolbar-visibility}

### 2.1 IsToolbarVisiblity Property {#istoolbarvisiblity-property}

The `IsToolbarVisiblity` property in the `ToolbarSettings` class controls whether the built-in toolbar is displayed. This property is essential when you want to create your own custom toolbar or completely hide editing controls.

**Property Details:**
- **Type**: `bool`
- **Default Value**: `true`
- **Location**: `SfImageEditor.ToolbarSettings.IsToolbarVisiblity`

### 2.2 Showing and Hiding the Toolbar {#showing-and-hiding-the-toolbar}

**XAML Approach:**

```xaml
<editor:SfImageEditor.ToolbarSettings>
    <editor:ToolbarSettings IsToolbarVisiblity="True" />
</editor:SfImageEditor.ToolbarSettings>
```

**Code-Behind Approach:**

```csharp
editor.ToolbarSettings.IsToolbarVisibility = true;
```

**Use Cases for Hiding the Toolbar:**
- Creating a custom toolbar with your own UI design
- Building a simplified viewer without editing controls
- Implementing a programmatic-only editing workflow
- Integrating with existing application toolbars

## 3. Toolbar Customization {#toolbar-customization}

### 3.1 Adding Custom Items {#adding-custom-items}

You can extend the built-in toolbar by adding your own custom items. This allows you to integrate application-specific functionality directly into the toolbar.

**Step 1: Define a DataTemplate for your custom item**

```xaml
<Grid.Resources>
    <DataTemplate x:Key="template">
        <TextBlock Text="New Item"></TextBlock>
    </DataTemplate>
</Grid.Resources>
```

**Step 2: Add the item to the ToolbarItems collection**

```csharp
editor.ToolbarSettings.ToolbarItems.Add(new ToolbarItem() 
{ 
    IconTemplate = grid.Resources["template"] as DataTemplate 
});
```

### 3.2 Using IconTemplate {#using-icontemplate}

The `IconTemplate` property allows you to specify custom visual content for toolbar items. You can use:

- Simple text elements
- Icons from icon fonts (e.g., Segoe MDL2 Assets)
- Custom graphics and images
- Complex layouts with multiple elements

**Example with an Icon Font:**

```xaml
<Grid.Resources>
    <DataTemplate x:Key="customIconTemplate">
        <TextBlock Text="&#xE8B7;" FontFamily="Segoe MDL2 Assets" 
                   FontSize="16" Foreground="Black"/>
    </DataTemplate>
</Grid.Resources>
```

```csharp
var customItem = new ToolbarItem()
{
    IconTemplate = grid.Resources["customIconTemplate"] as DataTemplate
};
editor.ToolbarSettings.ToolbarItems.Add(customItem);
```

### 3.3 Customizing Colors {#customizing-colors}

The toolbar appearance can be customized using the following properties:

- **Background**: Sets the background color of the toolbar
- **BorderColor**: Sets the border color of the toolbar

**XAML Example:**

```xaml
<editor:SfImageEditor.ToolbarSettings>
    <editor:ToolbarSettings IsToolbarVisiblity="True" 
                           Background="#DEDEDE" 
                           BorderColor="Black" />
</editor:SfImageEditor.ToolbarSettings>
```

**Code-Behind Example:**

```csharp
editor.ToolbarSettings.Background = (SolidColorBrush)new BrushConverter()
    .ConvertFromString("#DEDEDE");
editor.ToolbarSettings.BorderColor = new SolidColorBrush(Colors.Black);
editor.ToolbarSettings.IsToolbarVisiblity = true;
```

### 3.4 Customizing Dimensions {#customizing-dimensions}

The Image Editor toolbar consists of three sections, each with customizable height:

- **HeaderToolbarHeight**: Height of the main toolbar
- **SubItemToolbarHeight**: Height of the sub-item toolbar
- **FooterToolbarHeight**: Height of the footer toolbar

**Complete Customization Example:**

```xaml
<editor:SfImageEditor.ToolbarSettings>
    <editor:ToolbarSettings IsToolbarVisiblity="True" 
                           HeaderToolbarHeight="36"
                           FooterToolbarHeight="36"  
                           Background="#DEDEDE" 
                           BorderColor="Black" />
</editor:SfImageEditor.ToolbarSettings>
```

```csharp
editor.ToolbarSettings.Background = (SolidColorBrush)new BrushConverter()
    .ConvertFromString("#DEDEDE");
editor.ToolbarSettings.BorderColor = new SolidColorBrush(Colors.Black);
editor.ToolbarSettings.HeaderToolbarHeight = 36;
editor.ToolbarSettings.FooterToolbarHeight = 36;
editor.ToolbarSettings.IsToolbarVisiblity = true;
```

## 4. Available Commands {#available-commands}

The Image Editor provides a comprehensive set of commands that can be invoked from custom toolbars. When using commands, you must set the `CommandTarget` to reference the Image Editor control.

### 4.1 BrowseImage Command {#browseimage-command}

**Description**: Opens a file dialog to browse and load an image from the local file system.

**Usage:**

```xaml
<Button Content="Browse" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.BrowseImage}"/>
```

### 4.2 Save Command {#save-command}

**Description**: Saves the currently edited image.

**Usage:**

```xaml
<Button Content="Save" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.Save}"/>
```

### 4.3 Undo and Redo Commands {#undo-and-redo-commands}

**Undo Description**: Reverses the last performed action.

**Redo Description**: Restores the actions carried out by Undo.

**Usage:**

```xaml
<Button Content="Undo" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.Undo}"/>

<Button Content="Redo" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.Redo}"/>
```

### 4.4 Reset Command {#reset-command}

**Description**: Clears all editing operations performed on the image and returns it to its initial state.

**Usage:**

```xaml
<Button Content="Reset" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.Reset}"/>
```

### 4.5 Zoom Commands {#zoom-commands}

The Image Editor provides three zoom-related commands:

**ResetZoom**: Resets the zoom level to 100%
**IncreaseZoom**: Zooms in by increasing the zoom level
**DecreaseZoom**: Zooms out by decreasing the zoom level

**Usage:**

```xaml
<Button Content="Zoom In" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.IncreaseZoom}"/>

<Button Content="Zoom Out" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.DecreaseZoom}"/>

<Button Content="Reset Zoom" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.ResetZoom}"/>
```

### 4.6 Select and Pan Commands {#select-and-pan-commands}

**Select**: Enables selection mode for shapes, text, and custom views added to the image.

**Pan**: Enables pan mode for navigating a zoomed image.

**Usage:**

```xaml
<Button Content="Select" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.Select}"/>

<Button Content="Pan" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.Pan}"/>
```

### Command Reference Table

| Command | Description |
|---------|-------------|
| `BrowseImage` | Browses the local folder to pick and load the image to an image editor. |
| `Save` | Saves the edited image. |
| `Undo` | Reverses the last performed action. |
| `Redo` | Restores the actions carried out by Undo. |
| `Reset` | Clears all the editing done on the image and brings to an initial state. |
| `ResetZoom` | Resets the zooming applied to the image. |
| `IncreaseZoom` | Zoom in the image by increasing the zoom level from its current state. |
| `DecreaseZoom` | Zoom out the image by decreasing the zoom level from its current state. |
| `Select` | Used to select the items such as Shapes, Text, Custom view added on the image. |
| `Pan` | Used to pan the image when it is in zoomed state. |

## 5. Creating Custom Toolbars {#creating-custom-toolbars}

### 5.1 CommandTarget Binding {#commandtarget-binding}

When creating a custom toolbar, the `CommandTarget` property is essential. It specifies which Image Editor instance the command should operate on.

**Key Points:**
- Always set `CommandTarget` when using Image Editor commands
- Use `ElementName` binding to reference the Image Editor by name
- The CommandTarget ensures commands are routed to the correct control

**Syntax:**

```xaml
CommandTarget="{Binding ElementName=imageEditor}"
```

### 5.2 Building a Custom Toolbar {#building-a-custom-toolbar}

**Complete Example:**

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*"/>
        <ColumnDefinition Width="200"/>
    </Grid.ColumnDefinitions>
    
    <editor:SfImageEditor Grid.Column="0" 
                          x:Name="imageEditor" 
                          ImageSource="Assets\RoadView.jpeg">
        <editor:SfImageEditor.ToolbarSettings>
            <editor:ToolbarSettings IsToolbarVisiblity="False">
            </editor:ToolbarSettings>
        </editor:SfImageEditor.ToolbarSettings>
    </editor:SfImageEditor>

    <Button Grid.Column="1" 
            HorizontalAlignment="Center" 
            VerticalAlignment="Center" 
            Background="White"
            Width="Auto" 
            CommandTarget="{Binding ElementName=imageEditor}"
            Content="Save" 
            Command="{x:Static editor:ImageEditorCommands.Save}">
    </Button>
</Grid>
```

## 6. Custom Views {#custom-views}

Custom views allow you to add any custom shapes, graphics, or UI elements directly onto the image being edited. This is particularly useful for watermarks, logos, stamps, and overlays.

### 6.1 AddCustomView Method {#addcustomview-method}

The `AddCustomView` method adds a custom visual element to the image.

**Method Signature:**

```csharp
public void AddCustomView(UIElement view, CustomViewSettings settings)
```

**Parameters:**
- `view`: Any WPF UIElement (Image, TextBlock, Grid, etc.)
- `settings`: Configuration for positioning, sizing, and behavior

**Basic Example:**

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    
    <Button x:Name="but" 
            Click="Button_Click" 
            Content="CustomView" />
    
    <editor:SfImageEditor Grid.Row="1" 
                          x:Name="editor" 
                          ImageSource="Assets\RoadView.jpeg">
    </editor:SfImageEditor>
</Grid>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    BitmapImage bitmapImage = new BitmapImage();
    bitmapImage.BeginInit();
    bitmapImage.UriSource = new Uri(@"Assets/adventure.jpg", UriKind.Relative);
    bitmapImage.EndInit();
    
    Image image = new Image() 
    { 
        Height = 100, 
        Width = 100, 
        Source = bitmapImage
    };
    
    editor.AddCustomView(image, new CustomViewSettings());
}
```

### 6.2 CustomViewSettings Properties {#customviewsettings-properties}

The `CustomViewSettings` class provides fine-grained control over custom view behavior:

**Properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `CanMaintainAspectRatio` | `bool` | Maintains aspect ratio when resizing | `true` |
| `Bounds` | `Rect` | Position and size of the custom view (in percentage: 0-100) | - |
| `Angle` | `float` | Rotation angle in degrees | `0` |
| `IsResizable` | `bool` | Enables/disables resizing | `true` |
| `IsRotatable` | `bool` | Enables/disables rotation | `false` |

### 6.3 Positioning and Sizing {#positioning-and-sizing}

The `Bounds` property uses a percentage-based coordinate system:
- X, Y: Position (0-100% of image dimensions)
- Width, Height: Size (0-100% of image dimensions)

**Example:**

```csharp
CustomViewSettings settings = new CustomViewSettings()
{
    Bounds = new Rect(0, 0, 100, 100), // Top-left corner, full image size
    CanMaintainAspectRatio = false
};
```

### 6.4 Rotation and Aspect Ratio {#rotation-and-aspect-ratio}

**Complete CustomViewSettings Example:**

```csharp
CustomViewSettings customViewSettings = new CustomViewSettings()
{
    CanMaintainAspectRatio = false,
    Bounds = new Rect(0, 0, 100, 100),
    IsResizable = false,
    IsRotatable = true,
    Angle = 45
};
```

## 7. Use Cases {#use-cases}

### 7.1 Adding Watermarks {#adding-watermarks}

Watermarks protect intellectual property by adding visible branding to images.

**Text Watermark Example:**

```csharp
private void AddTextWatermark()
{
    TextBlock watermark = new TextBlock()
    {
        Text = "© 2026 Your Company",
        FontSize = 24,
        Foreground = new SolidColorBrush(Color.FromArgb(128, 255, 255, 255)),
        FontWeight = FontWeights.Bold
    };
    
    CustomViewSettings settings = new CustomViewSettings()
    {
        Bounds = new Rect(70, 85, 25, 10), // Bottom-right corner
        IsResizable = false,
        IsRotatable = false,
        CanMaintainAspectRatio = true
    };
    
    editor.AddCustomView(watermark, settings);
}
```

**Image Watermark Example:**

```csharp
private void AddImageWatermark()
{
    BitmapImage watermarkImage = new BitmapImage();
    watermarkImage.BeginInit();
    watermarkImage.UriSource = new Uri(@"Assets/watermark.png", UriKind.Relative);
    watermarkImage.EndInit();
    
    Image watermark = new Image()
    {
        Source = watermarkImage,
        Opacity = 0.5,
        Stretch = Stretch.Uniform
    };
    
    CustomViewSettings settings = new CustomViewSettings()
    {
        Bounds = new Rect(40, 40, 20, 20), // Center of the image
        IsResizable = true,
        IsRotatable = false,
        CanMaintainAspectRatio = true
    };
    
    editor.AddCustomView(watermark, settings);
}
```

### 7.2 Adding Logos {#adding-logos}

Logos provide branding and identification for images.

**Corner Logo Example:**

```csharp
private void AddCompanyLogo()
{
    BitmapImage logo = new BitmapImage();
    logo.BeginInit();
    logo.UriSource = new Uri(@"Assets/company-logo.png", UriKind.Relative);
    logo.EndInit();
    
    Image logoImage = new Image()
    {
        Source = logo,
        Width = 80,
        Height = 80
    };
    
    CustomViewSettings settings = new CustomViewSettings()
    {
        Bounds = new Rect(2, 2, 15, 15), // Top-left corner
        IsResizable = false,
        IsRotatable = false,
        CanMaintainAspectRatio = true
    };
    
    editor.AddCustomView(logoImage, settings);
}
```

### 7.3 Creating Overlays {#creating-overlays}

Overlays add informational or decorative elements to images.

**Timestamp Overlay Example:**

```csharp
private void AddTimestampOverlay()
{
    Border overlay = new Border()
    {
        Background = new SolidColorBrush(Color.FromArgb(180, 0, 0, 0)),
        CornerRadius = new CornerRadius(5),
        Padding = new Thickness(10)
    };
    
    TextBlock timestamp = new TextBlock()
    {
        Text = DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss"),
        Foreground = Brushes.White,
        FontSize = 14,
        FontFamily = new FontFamily("Consolas")
    };
    
    overlay.Child = timestamp;
    
    CustomViewSettings settings = new CustomViewSettings()
    {
        Bounds = new Rect(2, 90, 30, 8), // Bottom-left corner
        IsResizable = false,
        IsRotatable = false
    };
    
    editor.AddCustomView(overlay, settings);
}
```

## 8. Complete Working Examples {#complete-working-examples}

### 8.1 Example 1: Custom Toolbar with Commands {#example-1-custom-toolbar-with-commands}

**XAML:**

```xaml
<Window x:Class="ImageEditorDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editor="clr-namespace:Syncfusion.UI.Xaml.ImageEditor;assembly=Syncfusion.SfImageEditor.WPF"
        Title="Custom Toolbar Example" Height="600" Width="800">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <!-- Custom Toolbar -->
        <ToolBar Grid.Row="0" Background="#F0F0F0">
            <Button Content="Browse" 
                    CommandTarget="{Binding ElementName=imageEditor}"
                    Command="{x:Static editor:ImageEditorCommands.BrowseImage}"/>
            <Separator/>
            <Button Content="Undo" 
                    CommandTarget="{Binding ElementName=imageEditor}"
                    Command="{x:Static editor:ImageEditorCommands.Undo}"/>
            <Button Content="Redo" 
                    CommandTarget="{Binding ElementName=imageEditor}"
                    Command="{x:Static editor:ImageEditorCommands.Redo}"/>
            <Separator/>
            <Button Content="Zoom In" 
                    CommandTarget="{Binding ElementName=imageEditor}"
                    Command="{x:Static editor:ImageEditorCommands.IncreaseZoom}"/>
            <Button Content="Zoom Out" 
                    CommandTarget="{Binding ElementName=imageEditor}"
                    Command="{x:Static editor:ImageEditorCommands.DecreaseZoom}"/>
            <Button Content="Reset Zoom" 
                    CommandTarget="{Binding ElementName=imageEditor}"
                    Command="{x:Static editor:ImageEditorCommands.ResetZoom}"/>
            <Separator/>
            <Button Content="Reset" 
                    CommandTarget="{Binding ElementName=imageEditor}"
                    Command="{x:Static editor:ImageEditorCommands.Reset}"/>
            <Button Content="Save" 
                    CommandTarget="{Binding ElementName=imageEditor}"
                    Command="{x:Static editor:ImageEditorCommands.Save}"/>
        </ToolBar>
        
        <!-- Image Editor with Hidden Default Toolbar -->
        <editor:SfImageEditor Grid.Row="1" 
                              x:Name="imageEditor" 
                              ImageSource="Assets\RoadView.jpeg">
            <editor:SfImageEditor.ToolbarSettings>
                <editor:ToolbarSettings IsToolbarVisiblity="False"/>
            </editor:SfImageEditor.ToolbarSettings>
        </editor:SfImageEditor>
    </Grid>
</Window>
```

### 8.2 Example 2: Adding Watermark with Custom View {#example-2-adding-watermark-with-custom-view}

**XAML:**

```xaml
<Window x:Class="ImageEditorDemo.WatermarkWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editor="clr-namespace:Syncfusion.UI.Xaml.ImageEditor;assembly=Syncfusion.SfImageEditor.WPF"
        Title="Watermark Example" Height="600" Width="800">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="10">
            <Button Content="Add Text Watermark" 
                    Click="AddTextWatermark_Click" 
                    Margin="5"/>
            <Button Content="Add Image Watermark" 
                    Click="AddImageWatermark_Click" 
                    Margin="5"/>
            <Button Content="Save" 
                    CommandTarget="{Binding ElementName=imageEditor}"
                    Command="{x:Static editor:ImageEditorCommands.Save}" 
                    Margin="5"/>
        </StackPanel>
        
        <editor:SfImageEditor Grid.Row="1" 
                              x:Name="imageEditor" 
                              ImageSource="Assets\RoadView.jpeg"/>
    </Grid>
</Window>
```

**Code-Behind:**

```csharp
using System;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using Syncfusion.UI.Xaml.ImageEditor;

namespace ImageEditorDemo
{
    public partial class WatermarkWindow : Window
    {
        public WatermarkWindow()
        {
            InitializeComponent();
        }
        
        private void AddTextWatermark_Click(object sender, RoutedEventArgs e)
        {
            TextBlock watermark = new TextBlock()
            {
                Text = "© 2026 Your Company",
                FontSize = 32,
                Foreground = new SolidColorBrush(Color.FromArgb(128, 255, 255, 255)),
                FontWeight = FontWeights.Bold
            };
            
            CustomViewSettings settings = new CustomViewSettings()
            {
                Bounds = new Rect(60, 80, 35, 15),
                IsResizable = true,
                IsRotatable = true,
                CanMaintainAspectRatio = true,
                Angle = -15
            };
            
            imageEditor.AddCustomView(watermark, settings);
        }
        
        private void AddImageWatermark_Click(object sender, RoutedEventArgs e)
        {
            BitmapImage watermarkImage = new BitmapImage();
            watermarkImage.BeginInit();
            watermarkImage.UriSource = new Uri(@"Assets/watermark.png", 
                                               UriKind.Relative);
            watermarkImage.EndInit();
            
            Image watermark = new Image()
            {
                Source = watermarkImage,
                Opacity = 0.4,
                Stretch = Stretch.Uniform
            };
            
            CustomViewSettings settings = new CustomViewSettings()
            {
                Bounds = new Rect(35, 35, 30, 30),
                IsResizable = true,
                IsRotatable = true,
                CanMaintainAspectRatio = true
            };
            
            imageEditor.AddCustomView(watermark, settings);
        }
    }
}
```

### 8.3 Example 3: Fully Customized Image Editor {#example-3-fully-customized-image-editor}

**XAML:**

```xaml
<Window x:Class="ImageEditorDemo.FullCustomizationWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editor="clr-namespace:Syncfusion.UI.Xaml.ImageEditor;assembly=Syncfusion.SfImageEditor.WPF"
        Title="Fully Customized Image Editor" Height="700" Width="1000">
    
    <Grid.Resources>
        <!-- Custom Toolbar Item Template -->
        <DataTemplate x:Key="customItemTemplate">
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="&#xE8B7;" FontFamily="Segoe MDL2 Assets" 
                          FontSize="16" Margin="5,0"/>
                <TextBlock Text="Custom" FontSize="12"/>
            </StackPanel>
        </DataTemplate>
    </Grid.Resources>
    
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="250"/>
        </Grid.ColumnDefinitions>
        
        <!-- Image Editor -->
        <editor:SfImageEditor Grid.Column="0" 
                              x:Name="imageEditor" 
                              ImageSource="Assets\RoadView.jpeg"
                              Loaded="ImageEditor_Loaded">
            <editor:SfImageEditor.ToolbarSettings>
                <editor:ToolbarSettings IsToolbarVisibility="True"
                                       Background="#2C3E50"
                                       BorderColor="#34495E"
                                       HeaderToolbarHeight="45"
                                       FooterToolbarHeight="40"
                                       ToolbarItemSelected="ToolbarSettings_ToolbarItemSelected"/>
            </editor:SfImageEditor.ToolbarSettings>
        </editor:SfImageEditor>
        
        <!-- Custom Side Panel -->
        <Border Grid.Column="1" Background="#ECF0F1" BorderBrush="#BDC3C7" 
                BorderThickness="1,0,0,0">
            <StackPanel Margin="10">
                <TextBlock Text="Custom Actions" FontSize="18" 
                          FontWeight="Bold" Margin="0,0,0,10"/>
                
                <GroupBox Header="Watermarks" Margin="0,5">
                    <StackPanel>
                        <Button Content="Add Text Watermark" 
                               Click="AddTextWatermark_Click" 
                               Margin="5"/>
                        <Button Content="Add Logo" 
                               Click="AddLogo_Click" 
                               Margin="5"/>
                        <Button Content="Add Timestamp" 
                               Click="AddTimestamp_Click" 
                               Margin="5"/>
                    </StackPanel>
                </GroupBox>
                
                <GroupBox Header="Quick Actions" Margin="0,10">
                    <StackPanel>
                        <Button Content="Browse Image" 
                               CommandTarget="{Binding ElementName=imageEditor}"
                               Command="{x:Static editor:ImageEditorCommands.BrowseImage}" 
                               Margin="5"/>
                        <Button Content="Save Image" 
                               CommandTarget="{Binding ElementName=imageEditor}"
                               Command="{x:Static editor:ImageEditorCommands.Save}" 
                               Margin="5"/>
                        <Button Content="Reset All" 
                               CommandTarget="{Binding ElementName=imageEditor}"
                               Command="{x:Static editor:ImageEditorCommands.Reset}" 
                               Margin="5"/>
                    </StackPanel>
                </GroupBox>
            </StackPanel>
        </Border>
    </Grid>
</Window>
```

**Code-Behind:**

```csharp
using System;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using Syncfusion.UI.Xaml.ImageEditor;

namespace ImageEditorDemo
{
    public partial class FullCustomizationWindow : Window
    {
        public FullCustomizationWindow()
        {
            InitializeComponent();
        }
        
        private void ImageEditor_Loaded(object sender, RoutedEventArgs e)
        {
            // Add custom toolbar item
            var customItem = new ToolbarItem()
            {
                IconTemplate = this.Resources["customItemTemplate"] as DataTemplate
            };
            imageEditor.ToolbarSettings.ToolbarItems.Add(customItem);
        }
        
        private void ToolbarSettings_ToolbarItemSelected(object sender, 
                                                         ToolbarItemSelectedEventArgs e)
        {
            // Handle custom toolbar item selection
            MessageBox.Show($"Toolbar item selected: {e.ToolbarItem}");
        }
        
        private void AddTextWatermark_Click(object sender, RoutedEventArgs e)
        {
            TextBlock watermark = new TextBlock()
            {
                Text = "CONFIDENTIAL",
                FontSize = 48,
                Foreground = new SolidColorBrush(Color.FromArgb(100, 255, 0, 0)),
                FontWeight = FontWeights.Bold
            };
            
            CustomViewSettings settings = new CustomViewSettings()
            {
                Bounds = new Rect(30, 45, 40, 10),
                IsResizable = false,
                IsRotatable = true,
                Angle = -30
            };
            
            imageEditor.AddCustomView(watermark, settings);
        }
        
        private void AddLogo_Click(object sender, RoutedEventArgs e)
        {
            BitmapImage logo = new BitmapImage();
            logo.BeginInit();
            logo.UriSource = new Uri(@"Assets/company-logo.png", UriKind.Relative);
            logo.EndInit();
            
            Image logoImage = new Image()
            {
                Source = logo,
                Opacity = 0.8
            };
            
            CustomViewSettings settings = new CustomViewSettings()
            {
                Bounds = new Rect(2, 2, 12, 12),
                IsResizable = true,
                IsRotatable = false,
                CanMaintainAspectRatio = true
            };
            
            imageEditor.AddCustomView(logoImage, settings);
        }
        
        private void AddTimestamp_Click(object sender, RoutedEventArgs e)
        {
            Border overlay = new Border()
            {
                Background = new SolidColorBrush(Color.FromArgb(200, 0, 0, 0)),
                CornerRadius = new CornerRadius(5),
                Padding = new Thickness(10, 5, 10, 5)
            };
            
            TextBlock timestamp = new TextBlock()
            {
                Text = DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss"),
                Foreground = Brushes.White,
                FontSize = 14,
                FontFamily = new FontFamily("Consolas")
            };
            
            overlay.Child = timestamp;
            
            CustomViewSettings settings = new CustomViewSettings()
            {
                Bounds = new Rect(2, 92, 25, 6),
                IsResizable = false,
                IsRotatable = false
            };
            
            imageEditor.AddCustomView(overlay, settings);
        }
    }
}
```

## 9. Troubleshooting {#troubleshooting}

### 9.1 Common Issues {#common-issues}

#### Issue 1: Commands Not Working

**Symptoms**: Clicking custom toolbar buttons has no effect.

**Solution**: Ensure `CommandTarget` is set correctly:

```xaml
<Button Content="Save" 
        CommandTarget="{Binding ElementName=imageEditor}"
        Command="{x:Static editor:ImageEditorCommands.Save}"/>
```

#### Issue 2: Custom View Not Appearing

**Symptoms**: `AddCustomView` is called but nothing appears on the image.

**Possible Causes and Solutions**:

1. **Invalid Bounds**: Ensure bounds are within 0-100 range
   ```csharp
   // Correct
   Bounds = new Rect(10, 10, 20, 20)
   
   // Incorrect - outside valid range
   Bounds = new Rect(110, 10, 20, 20)
   ```

2. **View Size Issues**: Ensure the UIElement has a valid size
   ```csharp
   Image image = new Image() 
   { 
       Width = 100,  // Explicitly set dimensions
       Height = 100,
       Source = bitmapImage
   };
   ```

#### Issue 3: Toolbar Not Hiding

**Symptoms**: Setting `IsToolbarVisiblity` to `False` doesn't hide the toolbar.

**Solution**: Check for typos in property name (note the spelling):

```csharp
// Correct property name
editor.ToolbarSettings.IsToolbarVisiblity = false;
```

#### Issue 4: Custom Toolbar Items Not Showing

**Symptoms**: Added toolbar items don't appear in the toolbar.

**Solution**: Ensure the DataTemplate is properly defined and referenced:

```xaml
<Grid.Resources>
    <DataTemplate x:Key="myTemplate">
        <TextBlock Text="Item"/>
    </DataTemplate>
</Grid.Resources>
```

```csharp
editor.ToolbarSettings.ToolbarItems.Add(new ToolbarItem() 
{ 
    IconTemplate = grid.Resources["myTemplate"] as DataTemplate 
});
```

### 9.2 Best Practices {#best-practices}

#### Design Guidelines

1. **Consistent Styling**: Match toolbar colors with your application theme
   ```csharp
   editor.ToolbarSettings.Background = Application.Current.Resources["PrimaryBackgroundBrush"] as Brush;
   ```

2. **Appropriate Dimensions**: Don't make toolbars too small or too large
   ```csharp
   editor.ToolbarSettings.HeaderToolbarHeight = 40; // Good balance
   ```

3. **Clear Icons**: Use recognizable icons for custom toolbar items
   ```xaml
   <DataTemplate x:Key="saveIcon">
       <TextBlock Text="&#xE74E;" FontFamily="Segoe MDL2 Assets" FontSize="16"/>
   </DataTemplate>
   ```

#### Performance Considerations

1. **Image Resources**: Load images asynchronously when possible
   ```csharp
   BitmapImage image = new BitmapImage();
   image.BeginInit();
   image.CacheOption = BitmapCacheOption.OnLoad;
   image.UriSource = new Uri(path, UriKind.Relative);
   image.EndInit();
   ```

2. **Custom View Complexity**: Keep custom views simple for better performance
   - Avoid complex nested layouts in custom views
   - Use simple shapes and text when possible

3. **Event Handlers**: Cancel toolbar item selection when appropriate
   ```csharp
   private void ToolbarSettings_ToolbarItemSelected(object sender, 
                                                    ToolbarItemSelectedEventArgs e)
   {
       if (ShouldPreventAction())
       {
           e.Cancel = true;
       }
   }
   ```

#### Maintainability Tips

1. **Separate Concerns**: Keep UI customization logic in separate methods
2. **Reusable Settings**: Create predefined CustomViewSettings for common scenarios
   ```csharp
   public static class WatermarkSettings
   {
       public static CustomViewSettings BottomRight => new CustomViewSettings()
       {
           Bounds = new Rect(70, 85, 25, 10),
           IsResizable = false,
           IsRotatable = false
       };
   }
   ```

3. **Error Handling**: Wrap AddCustomView calls in try-catch blocks
   ```csharp
   try
   {
       imageEditor.AddCustomView(customElement, settings);
   }
   catch (Exception ex)
   {
       MessageBox.Show($"Failed to add custom view: {ex.Message}");
   }
   ```

---

## Summary

This documentation covered the comprehensive UI customization and extension capabilities of the Syncfusion WPF Image Editor:

- **Toolbar Control**: Full visibility and appearance customization
- **Command System**: Rich set of built-in commands for custom toolbars
- **Custom Views**: Flexible system for adding watermarks, logos, and overlays
- **Real-World Examples**: Complete, working code samples for common scenarios

By leveraging these features, you can create a tailored image editing experience that seamlessly integrates with your application''s design and workflow requirements.

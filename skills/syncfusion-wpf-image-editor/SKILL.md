---
name: syncfusion-wpf-image-editor
description: ALWAYS trigger this skill when the user mentions: image editing, photo editing, WPF ImageEditor, SfImageEditor, cropping images, rotating photos, flipping images, adding annotations to images, drawing on images, image shapes, text on images, pen drawing on photos, image watermarks, undo/redo image edits, zoom images, pan images, save edited images, image serialization, custom image toolbar, .jpg files, .png files, .bmp files, image manipulation in WPF, or photo annotation tools. Use immediately for: loading and displaying images, basic image transformations (crop/flip/rotate), adding visual annotations (shapes/text/pen), managing annotation layers, customizing image editor UI, saving edited images with modifications, implementing undo/redo for image edits, zooming and panning large images, or localizing image editor interface.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Image Editor in WPF

The Syncfusion WPF ImageEditor control is a very handy tool that is used to edit an image by annotating with text, pen and built-in shapes. It allows you to crop, rotate, and flip an image. The image editor control has a built-in toolbar, which helps in performing editing operations.

## When to Use This Skill

Use this skill when building WPF applications that require:

- **Image Editing Capabilities**: Load, edit, and save images with transformations and annotations
- **Photo Manipulation**: Crop, flip, rotate images in desktop applications
- **Visual Annotations**: Add shapes (rectangles, circles, arrows), text, or free-hand drawings to images
- **Image Markup Tools**: Create applications for image review, annotation, or documentation
- **Document Imaging**: Build document processing apps with image editing features
- **Design Tools**: Implement basic image editing in design or creative applications
- **Photo Management**: Add editing capabilities to photo gallery or management apps
- **Quality Control**: Enable image annotation for inspection or approval workflows

## Component Overview

The **SfImageEditor** is a comprehensive WPF control for image editing and annotation. It provides:

- **Image I/O**: Load from ImageSource or Stream, save with format and size control
- **Transformations**: Crop (rectangle/circle with ratios), Flip (horizontal/vertical), Rotate (90° increments)
- **Annotations**: Add shapes (rectangle, circle, arrow), rich text, and free-hand pen drawings
- **Customization**: Full control over pen settings, text formatting, and visual styles
- **Layer Management**: Z-ordering controls, undo/redo, serialization/deserialization
- **Navigation**: Zoom (50-400%), pan for large images
- **UI Flexibility**: Built-in toolbar with customization, custom views (watermarks, logos)
- **Localization**: Full support for internationalization

## Documentation and Navigation Guide

### Setup and Basic Operations
📄 **Read:** [references/setup-and-basic-operations.md](references/setup-and-basic-operations.md)

When user needs to:
- Install and configure SfImageEditor
- Initialize the control in XAML or code
- Load images from file paths or streams
- Save edited images with specific formats (PNG, JPG, BMP)
- Control image save location and size
- Reset image to original state
- Handle save events
- Apply themes to the editor

### Image Transformations
📄 **Read:** [references/image-transformations.md](references/image-transformations.md)

When user needs to:
- Crop images (rectangle or circle cropping)
- Apply aspect ratio constraints during cropping
- Flip images horizontally or vertically
- Rotate images in 90-degree increments
- Enable zoom functionality (50-400% range)
- Control zoom with toolbar slider or mouse wheel
- Enable panning for navigated zoomed images
- Toggle between pan and select modes

### Annotations - Shapes and Text
📄 **Read:** [references/annotations-shapes-and-text.md](references/annotations-shapes-and-text.md)

When user needs to:
- Add shapes (rectangles, circles, arrows) to images
- Draw free-hand with pen tool
- Customize shape appearance (fill, stroke, opacity)
- Add text annotations with rich formatting
- Control text properties (font, color, alignment, effects)
- Make shapes and text resizable
- Handle shape/text selection events
- Delete annotations programmatically

### Annotation Management
📄 **Read:** [references/annotation-management.md](references/annotation-management.md)

When user needs to:
- Implement undo/redo for annotations
- Control annotation layer ordering (bring to front, send to back)
- Serialize annotations to XML for persistence
- Deserialize saved annotation state
- Access and filter annotations collection
- Understand undo/redo limitations

### UI Customization and Extensions
📄 **Read:** [references/ui-customization-and-extensions.md](references/ui-customization-and-extensions.md)

When user needs to:
- Show or hide the built-in toolbar
- Add custom toolbar items with icons
- Customize toolbar appearance (colors, dimensions)
- Create completely custom toolbars
- Add custom views (watermarks, logos, overlays)
- Control custom view properties (resizable, rotatable)
- Bind to available commands (save, undo, redo, zoom, etc.)

### Localization
📄 **Read:** [references/localization.md](references/localization.md)

When user needs to:
- Localize UI strings for different cultures
- Create resource files for localization
- Configure culture settings
- Support multiple languages in the image editor

## Quick Start Example

### Basic Image Editor Setup

\\\xaml
<Window x:Class="ImageEditorApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editor="clr-namespace:Syncfusion.UI.Xaml.ImageEditor;assembly=Syncfusion.SfImageEditor.WPF"
        Title="Image Editor" Height="600" Width="800">
    <Grid>
        <editor:SfImageEditor x:Name="imageEditor" 
                              ImageSource="Assets/photo.jpg"/>
    </Grid>
</Window>
\\\

### Load Image from Stream

\\\csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using Microsoft.Win32;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        LoadImageFromFile();
    }

    private void LoadImageFromFile()
    {
        OpenFileDialog openFileDialog = new OpenFileDialog();
        openFileDialog.Filter = "Image Files(*.BMP;*.JPG;*.PNG)|*.BMP;*.JPG;*.PNG|All files (*.*)|*.*";
        
        if (openFileDialog.ShowDialog() == true)
        {
            var stream = openFileDialog.OpenFile();
            imageEditor.Image = stream;
        }
    }
}
\\\

## Common Patterns

### Pattern 1: Load, Edit, and Save Image

\\\csharp
// Load image
BitmapImage image = new BitmapImage();
image.BeginInit();
image.UriSource = new Uri(@"Assets/photo.jpg", UriKind.Relative);
image.EndInit();
imageEditor.ImageSource = image;

// User performs edits via toolbar or code...

// Save programmatically
imageEditor.Save("C:\\EditedImages\\result.png");
\\\

### Pattern 2: Add Text Annotation Programmatically

\\\csharp
using Syncfusion.UI.Xaml.ImageEditor;

// Configure text settings
TextSettings textSettings = new TextSettings()
{
    FontFamily = new FontFamily("Arial"),
    FontSize = 24,
    Color = new SolidColorBrush(Colors.Red),
    TextAlignment = TextAlignment.Center
};

// Add text at specific position
imageEditor.AddText("Copyright 2026", textSettings);
\\\

### Pattern 3: Add Shape with Custom Styling

\\\csharp
// Configure pen settings for shapes
PenSettings penSettings = new PenSettings()
{
    Fill = new SolidColorBrush(Colors.Yellow),
    Stroke = new SolidColorBrush(Colors.Red),
    StrokeWidth = 3,
    Opacity = 0.7f
};

// Add rectangle shape
imageEditor.AddShape(AnnotationShape.Rectangle, penSettings);
\\\

### Pattern 4: Implement Undo/Redo

\\\csharp
// In toolbar or menu command
private void UndoButton_Click(object sender, RoutedEventArgs e)
{
    imageEditor.Undo();
}

private void RedoButton_Click(object sender, RoutedEventArgs e)
{
    imageEditor.Redo();
}
\\\

### Pattern 5: Serialize and Deserialize Annotations

\\\csharp
using System.IO;

// Serialize annotations to file
private void SaveAnnotations()
{
    using (FileStream stream = new FileStream("annotations.xml", FileMode.Create))
    {
        imageEditor.Serialize(stream);
    }
}

// Deserialize annotations from file
private void LoadAnnotations()
{
    using (FileStream stream = new FileStream("annotations.xml", FileMode.Open))
    {
        imageEditor.Deserialize(stream);
    }
}
\\\

### Pattern 6: Enable Zoom and Pan

\\\csharp
// Enable zoom with mouse wheel
imageEditor.EnableZooming = true;

// Enable panning for zoomed images
imageEditor.EnablePanning = true;

\\\

## Key Properties and Methods

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| ImageSource | ImageSource | Gets or sets the image to be loaded in the editor |
| Image | Stream | Loads image from a stream source |
| EnableZooming | bool | Enables or disables zoom functionality |
| EnablePanning | bool | Enables or disables pan mode for navigation |
| IsToolbarVisiblity | bool | Shows or hides the built-in toolbar |
| Annotations | ReadOnlyCollection<ImageEditorAnnotationsInfo> | Collection of all annotations in the editor |

### Essential Methods

| Method | Parameters | Description |
|--------|------------|-------------|
| Save() | string format = null, Size size = new Size(), string filePath = null | Saves edited image to specified path |
| Reset() | - | Resets image to original state |
| Undo() | - | Undoes the last annotation operation |
| Redo() | - | Redoes the previously undone operation |
| AddShape() | ShapeType shapeType, PenSettings penSettings | Adds a shape at specified bounds |
| AddText() | string text, TextSettings textSettings | Adds text at specified position |
| ToggleCropping() | - | Toggles crop mode on/off |
| ToggleCropping() | float xRatio, float yRatio, bool isEllipse = false | crop preview with the specified height to width ratio. |
| ToggleCropping() | Rect rect | crop preview to the specified bounds. |
| ToggleCropping() | Rect rect, bool isEllipse | Enables the cropping preview on the image with the specified rectangle bounds. |
| Crop() | Rect rect = null, bool isEllipse = false | Crops image to specified rectangle |
| AddCustomView() | FrameworkElement element, CustomViewSettings customViewSettings | Adds custom overlay view |
| Serialize() | Stream stream | Saves annotations to XML stream |
| Deserialize() | Stream stream | Loads annotations from XML stream |

### Key Events

| Event | Description |
|-------|-------------|
| ImageSaving | Raised before image is saved, allows cancellation |
| ImageSaved | Raised after image is saved successfully |
| BeginReset | Raised before reset operation begins |
| EndReset | Raised after reset operation completes |
| ItemSelected | Raised when annotation is selected |
| ItemUnselected | Raised when annotation is deselected |
| ImageEdited | Raised whenever the image is edited |
| ImageLoaded | Raised when the image is loaded |

## Common Use Cases

1. **Photo Editing Application**: Build desktop photo editors with crop, rotate, filters
2. **Document Annotation**: Mark up screenshots, diagrams, or scanned documents
3. **Quality Control**: Add inspection marks or notes to product images
4. **Design Review**: Allow stakeholders to annotate design mockups
5. **Medical Imaging**: Annotate X-rays or scans (with appropriate image formats)
6. **Real Estate**: Add notes or highlights to property photos
7. **E-Learning**: Create annotated images for educational content
8. **Bug Reporting**: Allow users to mark up screenshots when reporting issues

---

**Next Steps:**
- For detailed setup instructions, read [setup-and-basic-operations.md](references/setup-and-basic-operations.md)
- For transformation features, read [image-transformations.md](references/image-transformations.md)
- For annotation capabilities, read [annotations-shapes-and-text.md](references/annotations-shapes-and-text.md)

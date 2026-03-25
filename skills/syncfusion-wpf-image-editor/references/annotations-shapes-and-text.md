# Annotations - Shapes and Text

## Table of Contents
- [Overview](#overview)
- [Adding Shapes](#adding-shapes)
  - [Using Toolbar](#using-toolbar)
  - [Programmatic Shape Addition](#programmatic-shape-addition)
  - [Available Shape Types](#available-shape-types)
- [Free-Hand Pen Drawing](#free-hand-pen-drawing)
- [Shape Customization (PenSettings)](#shape-customization-pensettings)
  - [Fill Property](#fill-property)
  - [Stroke Properties](#stroke-properties)
  - [Opacity and Bounds](#opacity-and-bounds)
  - [IsResizable Property](#isresizable-property)
- [Adding Text Annotations](#adding-text-annotations)
  - [Using Toolbar](#using-toolbar-1)
  - [Programmatic Text Addition](#programmatic-text-addition)
- [Text Customization (TextSettings)](#text-customization-textsettings)
  - [Font Properties](#font-properties)
  - [Color and Background](#color-and-background)
  - [TextAlignment](#textalignment)
  - [TextEffects](#texteffects)
  - [Advanced Properties](#advanced-properties)
- [Editing and Deleting Annotations](#editing-and-deleting-annotations)
- [Selection Events](#selection-events)
- [Complete Working Examples](#complete-working-examples)
- [Troubleshooting](#troubleshooting)

---

## Overview

The Syncfusion WPF ImageEditor control provides powerful annotation capabilities that enable users to add visual elements to images. You can add shapes (rectangles, circles, arrows), free-hand pen drawings, and richly formatted text annotations to enhance and mark up images.

Key annotation features:
- **Shapes**: Rectangle, Circle, Arrow with full customization
- **Pen Tool**: Free-hand drawing with customizable appearance
- **Text**: Rich text annotations with font, color, and effect controls
- **Customization**: Complete control over fill, stroke, opacity, and bounds
- **Interaction**: Select, resize, reposition, and delete annotations
- **Events**: ItemSelected and ItemUnselected for tracking user interactions

---

## Adding Shapes

### Using Toolbar

The built-in toolbar provides an easy way for users to add shapes to images:

1. Click the **Shapes** icon in the toolbar
2. Select the desired shape type from the dropdown:
   - **Rectangle**
   - **Circle**
   - **Arrow**
3. Click on the image to place the shape
4. Resize and position as needed using the handles

### Programmatic Shape Addition

You can add shapes programmatically using the `AddShape()` method:

**Method Signature:**
```csharp
public void AddShape(ShapeType shape, new PenSettings())
```

**Parameters:**
- `shape`: The type of shape (Rectangle, Circle, or Arrow)
- `PenSettings`: The pen settings of the shape

**Basic Example:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

editor.AddShape(ShapeType.Rectangle, new PenSettings());
```

### Available Shape Types

The `AnnotationShape` enumeration provides three shape types:

1. **Rectangle** - Rectangular shape
2. **Circle** - Circular or elliptical shape
3. **Arrow** - Arrow shape for indicating direction

**Adding Different Shapes:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
        }

        private void AddRectangle_Click(object sender, RoutedEventArgs e)
        {
            editor.AddShape(ShapeType.Rectangle, new PenSettings());
        }

        private void AddCircle_Click(object sender, RoutedEventArgs e)
        {
            editor.AddShape(ShapeType.Circle, new PenSettings());
        }

        private void AddArrow_Click(object sender, RoutedEventArgs e)
        {
            editor.AddShape(ShapeType.Arrow, new PenSettings());
        }
    }
}
```

---

## Free-Hand Pen Drawing

The Pen tool allows users to draw free-hand paths on images:

### Using Toolbar

1. Click the **Pen** icon in the toolbar
2. Draw on the image by clicking and dragging
3. The path will be created following your mouse movement
4. Release the mouse to complete the path

### Pen Drawing Characteristics

- Free-form drawing capability
- Follows mouse cursor movement
- Customizable appearance through PenSettings
- Can be selected, moved, and deleted like other shapes

**Note:** Pen drawings are treated as shape annotations and can be customized using the same PenSettings properties.

---

## Shape Customization (PenSettings)

The `PenSettings` class provides comprehensive control over shape and pen appearance. Configure these settings before adding shapes to apply your customizations.

### Fill Property

Controls the interior color of shapes:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows.Media;

// Set fill color to semi-transparent blue
PenSettings penSettings = new PenSettings()
{
    Fill = new SolidColorBrush(Color.FromArgb(180, 0, 120, 215))
};

// Add shape with the configured fill
editor.AddShape(AnnotationShape.Rectangle, penSettings);
```

### Stroke Properties

Control the outline of shapes:

**StrokeWidth** - Width of the shape outline
**Stroke** - Color of the shape outline

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows.Media;

// Configure stroke properties
PenSettings penSettings = new PenSettings()
{
    Stroke = new SolidColorBrush(Colors.Red),
    StrokeWidth = 5
};

// Add shape with thick red outline
editor.AddShape(AnnotationShape.Circle, penSettings);
```

### Opacity and Bounds

**Opacity** - Transparency level (0.0 to 1.0)  
**Bounds** - Position and size of the shape

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using System.Windows.Media;

// Create semi-transparent shape
PenSettings penSettings = new PenSettings()
{
    Fill = new SolidColorBrush(Colors.Yellow),
    Stroke = new SolidColorBrush(Colors.Orange),
    StrokeWidth = 3,
    Opacity = 0.6f,
    Bounds = new Rect(150, 150, 250, 200)
};

// Bounds in PenSettings defines default size
editor.AddShape(AnnotationShape.Rectangle, penSettings);
```

### IsResizable Property

Controls whether shapes can be resized by users:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;

// Create non-resizable shape
PenSettings penSettings = new PenSettings()
{
    IsResizable = false
};

// This shape cannot be resized by users
editor.AddShape(AnnotationShape.Rectangle, penSettings);
```

### Complete PenSettings Example

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using System.Windows.Media;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
        }

        private void AddCustomizedShape_Click(object sender, RoutedEventArgs e)
        {
            // Configure complete pen settings
            PenSettings penSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Color.FromArgb(200, 100, 200, 255)),
                Stroke = new SolidColorBrush(Colors.DarkBlue),
                StrokeWidth = 4,
                Opacity = 0.8,
                IsResizable = true
            };

            // Add shape with all customizations
            editor.AddShape(AnnotationShape.Circle, penSettings);
        }

        private void AddHighlightBox_Click(object sender, RoutedEventArgs e)
        {
            // Create semi-transparent highlight box
            PenSettings penSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Color.FromArgb(100, 255, 255, 0)),
                Stroke = new SolidColorBrush(Colors.Transparent),
                StrokeWidth = 0,
                Opacity = 0.5,
                EnableDrag = true,
                PathStrokeWidth = 2
            };

            editor.AddShape(AnnotationShape.Rectangle, penSettings);
        }
    }
}
```

---

## Adding Text Annotations

### Using Toolbar

1. Click the **Text** icon (T) in the toolbar
2. Click on the image where you want to add text
3. A text box will appear with editing capabilities
4. Type your text
5. Click outside or press Escape to finish editing

### Programmatic Text Addition

Use the `AddText()` method to add text annotations programmatically:

**Method Signature:**
```csharp
public void AddText(string text, TextSettings textSettings)
```

**Parameters:**
- `text`: The string content to display
- `textSettings`: Allows customization of the text’s appearance, including options for background, font family, font size, and font color.

**Basic Example:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

editor.AddText("Sample Text", new TextSettings());
```

**Complete Example:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
        }

        private void AddWatermark_Click(object sender, RoutedEventArgs e)
        {
            // Add watermark text
            editor.AddText("© 2026 Company Name", new TextSettings());
        }

        private void AddLabel_Click(object sender, RoutedEventArgs e)
        {
            // Add descriptive label
            editor.AddText("Important Section", new TextSettings());
        }

        private void AddMultipleTexts_Click(object sender, RoutedEventArgs e)
        {
            // Add multiple text annotations
            editor.AddText("Title", new TextSettings());
            editor.AddText("Description here", new TextSettings());
            editor.AddText("Footer note", new TextSettings());
        }
    }
}
```

---

## Text Customization (TextSettings)

The `TextSettings` class provides extensive control over text annotation appearance. Configure these settings before adding text to apply your customizations.

### Font Properties

**FontFamily** - The font family for text  
**FontSize** - The size of the text

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using System.Windows.Media;

// Configure font properties
TextSettings textSettings = new TextSettings()
{
    FontFamily = new FontFamily("Arial"),
    FontSize = 24
};

// Add text with custom font
editor.AddText("Custom Font Text", textSettings);
```

### Color and Background

**Color** - Text foreground color  
**Background** - Text background brush

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using System.Windows.Media;

// Configure colors
TextSettings textSettings = new TextSettings()
{
    Color = Colors.White,
    Background = new SolidColorBrush(Color.FromArgb(200, 0, 0, 0)),
    FontSize = 20
};

// Add text with colored background
editor.AddText("Highlighted Text", textSettings);
```

### TextAlignment

Controls horizontal alignment of text:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

// Configure text alignment
TextSettings textSettings = new TextSettings()
{
    TextAlignment = TextAlignment.Center,
    FontSize = 18
};

// Add centered text
editor.AddText("Centered Text", textSettings);
```

**Available Alignments:**
- `TextAlignment.Left` - Left-aligned (default)
- `TextAlignment.Center` - Center-aligned
- `TextAlignment.Right` - Right-aligned

### TextEffects

Apply visual effects to text:

**Available Effects:**
- `TextEffects.Bold` - Bold text
- `TextEffects.Italic` - Italic text
- `TextEffects.Underline` - Underlined text

**Single Effect:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

// Add bold text
TextSettings textSettings = new TextSettings()
{
    TextEffects = TextEffects.Bold,
    FontSize = 22
};

editor.AddText("Bold Text", textSettings);
```

**Multiple Effects (Combined with | operator):**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

// Add bold, italic, underlined text
TextSettings textSettings = new TextSettings()
{
    TextEffects = TextEffects.Bold | TextEffects.Italic | TextEffects.Underline,
    FontSize = 20
};

editor.AddText("Bold Italic Underlined", textSettings);
```

### Advanced Properties

**Opacity** - Transparency level (0.0 to 1.0)  
**Angle** - Rotation angle in degrees  
**Bounds** - Position and size of text box

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using System.Windows.Media;

// Configure advanced properties
TextSettings textSettings = new TextSettings()
{
    FontFamily = new FontFamily("Arial"),
    FontSize = 28,
    Color = new SolidColorBrush(Colors.Red),
    Opacity = 0.7f,
    Angle = 45,
    Bounds = new Rect(150, 150, 300, 100)
};

// Add rotated, semi-transparent text
editor.AddText("Rotated Text", new Point(150, 150));
```

### Complete TextSettings Example

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using System.Windows.Media;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
        }

        private void AddStyledText_Click(object sender, RoutedEventArgs e)
        {
            // Configure comprehensive text settings
            TextSettings textSettings = new TextSettings()
            {
                FontFamily = new FontFamily("Segoe UI"),
                FontSize = 26,
                Color = new SolidColorBrush(Colors.White),
                Background = new SolidColorBrush(Color.FromArgb(180, 50, 50, 200)),
                TextAlignment = TextAlignment.Center,
                TextEffects = TextEffects.Bold,
                Opacity = 0.9f
            };

            // Add styled text
            editor.AddText("Important Notice", textSettings);
        }

        private void AddWatermarkText_Click(object sender, RoutedEventArgs e)
        {
            // Create diagonal watermark
            TextSettings textsettings = new TextSettings()
            {
                FontFamily = new FontFamily("Arial"),
                FontSize = 48,
                Color = new SolidColorBrush(Colors.Gray),
                Opacity = 0.3f,
                Angle = 315, // Diagonal angle
                TextEffects = TextEffects.Bold
            };

            editor.AddText("CONFIDENTIAL", textSettings);
        }

        private void AddCaptionText_Click(object sender, RoutedEventArgs e)
        {
            // Create image caption
            TextSettings textSettings = new TextSettings()
            {
                FontFamily = new FontFamily("Georgia"),
                FontSize = 18,
                Color = new SolidColorBrush(Colors.Black),
                Background = new SolidColorBrush(Color.FromArgb(230, 255, 255, 255)),
                TextAlignment = TextAlignment.Left,
                TextEffects = TextEffects.Italic
            };

            editor.AddText("Figure 1: Sample Image Description", textSettings);
        }
    }
}
```

---

## Editing and Deleting Annotations

### Selecting Annotations

Click on any shape or text to select it. Selected annotations show resize handles and can be:
- **Moved**: Click and drag to reposition
- **Resized**: Drag the corner or edge handles
- **Rotated**: Use rotation handle (for supported annotations)

### Deleting Shapes Programmatically

Use the `Delete()` method to remove selected annotations:

```csharp
// Delete the currently selected annotation
editor.Delete();
```

**Complete Example with Selection:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
        }

        private void DeleteSelected_Click(object sender, RoutedEventArgs e)
        {
            // Delete currently selected shape or text
            editor.Delete();
        }

        private void AddAndDelete_Click(object sender, RoutedEventArgs e)
        {
            // Add a shape
            editor.AddShape(ShapeType.Rectangle, new PenSettings());

            // Note: To delete it programmatically, it must first be selected
            // In real scenarios, user would click to select before deletion
        }
    }
}
```

### Resizable Elements

Control which annotations can be resized using the `ResizableElements` property:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;

// Allow only shapes to be resized, not text
editor.ResizableElements = ImageEditorResizableElements.Shapes;

// Allow custom view to be resized
editor.ResizableElements = ImageEditorResizableElements.CustomView;

// Disable all resizing
editor.ResizableElements = ImageEditorResizableElements.None;
```

---

## Selection Events

The ImageEditor provides events to track when annotations are selected or deselected.

### ItemSelected Event

Raised when a shape or text annotation is selected:

**XAML Declaration:**

```xaml
<editor:SfImageEditor x:Name="editor"
                      ImageSource="Assets/BuildingImage.jpeg"
                      ItemSelected="Editor_ItemSelected">
</editor:SfImageEditor>
```

**Event Handler:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

private void Editor_ItemSelected(object sender, ItemSelectedEventArgs e)
{
    // Handle selection
    MessageBox.Show("An annotation was selected");
}
```

### ItemUnselected Event

Raised when a shape or text annotation is deselected:

**XAML Declaration:**

```xaml
<editor:SfImageEditor x:Name="editor"
                      ImageSource="Assets/BuildingImage.jpeg"
                      ItemSelected="Editor_ItemSelected"
                      ItemUnselected="Editor_ItemUnselected">
</editor:SfImageEditor>
```

**Event Handler:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

private void Editor_ItemUnselected(object sender, ItemUnselectedEventArgs e)
{
    // Handle deselection
    MessageBox.Show("Annotation was deselected");
}
```

### Complete Event Example

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();

            // Subscribe to events
            editor.ItemSelected += Editor_ItemSelected;
            editor.ItemUnselected += Editor_ItemUnselected;
        }

        private void Editor_ItemSelected(object sender, ItemSelectedEventArgs e)
        {
            // Enable delete button when item is selected
            DeleteButton.IsEnabled = true;
            StatusTextLabel.Text = "Annotation selected";
        }

        private void Editor_ItemUnselected(object sender, ItemUnselectedEventArgs e)
        {
            // Disable delete button when nothing is selected
            DeleteButton.IsEnabled = false;
            StatusTextLabel.Text = "No selection";
        }
    }
}
```

---

## Complete Working Examples

### Example 1: Image Markup Tool

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using System.Windows.Media;

namespace ImageMarkupTool
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
        }

        // Add highlight rectangle
        private void HighlightArea_Click(object sender, RoutedEventArgs e)
        {
            PenSettings penSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Color.FromArgb(100, 255, 255, 0)),
                Stroke = new SolidColorBrush(Colors.Yellow),
                StrokeWidth = 2,
                Opacity = 0.5f
            };

            editor.AddShape(AnnotationShape.Rectangle, penSettings);
        }

        // Add callout arrow
        private void AddCallout_Click(object sender, RoutedEventArgs e)
        {
            PenSettings penSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Colors.Red),
                Stroke = new SolidColorBrush(Colors.DarkRed),
                StrokeWidth = 3
            };

            editor.AddShape(AnnotationShape.Arrow, penSettings);

            // Add label text
            TextSettings textSettingss = new TextSettings()
            {
                FontFamily = new FontFamily("Arial"),
                FontSize = 18,
                Color = new SolidColorBrush(Colors.Red),
                TextEffects = TextEffects.Bold
            };

            editor.AddText("Important!", textSettings);
        }
    }
}
```

### Example 2: Photo Annotation App

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System;
using System.Windows;
using System.Windows.Media;

namespace PhotoAnnotationApp
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();

            // Subscribe to events
            editor.ItemSelected += Editor_ItemSelected;
            editor.ItemUnselected += Editor_ItemUnselected;
        }

        // Add timestamp watermark
        private void AddTimestamp_Click(object sender, RoutedEventArgs e)
        {
            string timestamp = DateTime.Now.ToString("yyyy-MM-dd HH:mm");

            TextSettings textSettings = new TextSettings()
            {
                FontFamily = new FontFamily("Courier New"),
                FontSize = 16,
                Color = new SolidColorBrush(Colors.White),
                Background = new SolidColorBrush(Color.FromArgb(180, 0, 0, 0)),
                TextAlignment = TextAlignment.Left
            };

            editor.AddText(timestamp, textSettings);
        }

        // Add face detection rectangle
        private void MarkFace_Click(object sender, RoutedEventArgs e)
        {
            PenSettings penSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Colors.Transparent),
                Stroke = new SolidColorBrush(Colors.LimeGreen),
                StrokeWidth = 3
            };

            editor.AddShape(AnnotationShape.Rectangle, penSettings);
        }

        // Add copyright notice
        private void AddCopyright_Click(object sender, RoutedEventArgs e)
        {
            TextSettings textSettings = new TextSettings()
            {
                FontFamily = new FontFamily("Arial"),
                FontSize = 14,
                Color = Colors.Gray,
                Opacity = 0.7f,
                Angle = 0
            };

            editor.AddText("© 2026 All Rights Reserved", textSettings);
        }

        private void Editor_ItemSelected(object sender, ItemSelectedEventArgs e)
        {
            // Enable annotation tools
            DeleteButton.IsEnabled = true;
            MoveButton.IsEnabled = true;
        }

        private void Editor_ItemUnselected(object sender, ItemUnselectedEventArgs e)
        {
            // Disable annotation tools
            DeleteButton.IsEnabled = false;
            MoveButton.IsEnabled = false;
        }

        private void DeleteAnnotation_Click(object sender, RoutedEventArgs e)
        {
            editor.Delete();
        }
    }
}
```

---

## Troubleshooting

### Common Issues and Solutions

**1. Shapes Not Appearing**

**Problem:** Added shapes don't appear on the image.

**Solutions:**
- Verify the bounds are within the image dimensions
- Check that PenSettings fill and stroke are not transparent
- Ensure opacity is not set to 0
- Verify the image is loaded before adding shapes

```csharp
// Verify image is loaded
if (editor.ImageSource != null)
{
    editor.AddShape(ShapeType.Rectangle, new PenSettings());
}
```

**2. Text Not Visible**

**Problem:** Added text annotations are not visible.

**Solutions:**
- Check text color contrasts with image background
- Verify text is within image bounds
- Ensure opacity is sufficient
- Check font size is appropriate

```csharp
// Use contrasting colors
TextSettings textSettings = new TextSettings()
{
    Color = new SolidColorBrush(Colors.White),
    Background = new SolidColorBrush(Color.FromArgb(200, 0, 0, 0)),
    FontSize = 20
};
```

**3. Cannot Resize Annotations**

**Problem:** Shapes or text cannot be resized.

**Solutions:**
- Check `IsResizable` property in PenSettings/TextSettings
- Verify `ResizableElements` property allows resizing
- Ensure annotation is selected

```csharp
// Enable resizing
PenSettings penSettings = new PenSettings()
{
    IsResizable = true
};

editor.ResizableElements = ResizableElements.Shapes;
```

**4. Events Not Firing**

**Problem:** ItemSelected or ItemUnselected events don't fire.

**Solutions:**
- Verify event handlers are properly subscribed
- Check XAML event names match code-behind
- Ensure annotations are interactive (not disabled)

**5. PenSettings Not Applied**

**Problem:** PenSettings configurations don't affect added shapes.

**Solutions:**
- Set PenSettings BEFORE calling AddShape()
- Verify all properties are correctly assigned
- Check for null or default values

```csharp
// Correct order: Configure THEN add
PenSettings penSettings = new PenSettings()
{
    Fill = new SolidColorBrush(Colors.Blue),
    Stroke = new SolidColorBrush(Colors.Red),
    StrokeWidth = 3
};

// Now add shape with configured settings
editor.AddShape(ShapeType.Rectangle, penSettings);
```

**6. Delete Method Not Working**

**Problem:** Calling Delete() doesn't remove annotations.

**Solutions:**
- Ensure an annotation is selected first
- Programmatically, you may need to track and select before deleting
- User must click annotation to select it before deletion

**7. TextEffects Not Applying**

**Problem:** Bold, italic, or underline effects don't appear.

**Solutions:**
- Verify font family supports the requested effects
- Use bitwise OR (|) for multiple effects
- Check that effects are properly combined

```csharp
// Correct multiple effects
TextSettings textSettings = new TextSettings()
{
    TextEffects = TextEffects.Bold | TextEffects.Italic,
    FontFamily = new FontFamily("Arial") // Font that supports effects
};
```

### Best Practices

1. **Configure Before Adding**: Always set PenSettings or TextSettings before adding annotations
2. **Provide Visual Feedback**: Use selection events to update UI state
3. **Validate Bounds**: Ensure annotation bounds are within image dimensions
4. **Use Contrasting Colors**: Make text and shapes visible against image backgrounds
5. **Enable Resizing Selectively**: Control which annotations users can resize
6. **Handle Events**: Subscribe to ItemSelected/ItemUnselected for better UX
7. **Test Transparency**: Use appropriate opacity values for overlays
8. **Provide Delete Options**: Give users clear ways to remove unwanted annotations

---

## See Also

**Related Topics:**
- Image transformations (crop, rotate, flip)
- Annotation management (undo, redo, z-ordering)
- Serialization (saving and loading annotations)
- Toolbar customization
- Custom views and overlays

---

*This documentation covers adding and customizing annotations in the Syncfusion WPF ImageEditor control. For managing annotations (undo/redo, layering, serialization), refer to the annotation-management documentation.*

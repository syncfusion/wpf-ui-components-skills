# Annotation Management

## Table of Contents
- [Overview](#overview)
- [Undo and Redo](#undo-and-redo)
  - [Supported Operations](#supported-operations)
  - [Undo Operation](#undo-operation)
  - [Redo Operation](#redo-operation)
  - [Limitations](#limitations)
- [Z-Ordering](#z-ordering)
  - [BringToFront](#bringtofront)
  - [BringForward](#bringforward)
  - [SendToBack](#sendtoback)
  - [SendBackward](#sendbackward)
  - [Z-Ordering Use Cases](#z-ordering-use-cases)
- [Serialization](#serialization)
  - [Serialize Method](#serialize-method)
  - [Deserialize Method](#deserialize-method)
  - [Annotations Collection](#annotations-collection)
  - [Filtering Annotations](#filtering-annotations)
- [Complete Working Examples](#complete-working-examples)
- [Troubleshooting](#troubleshooting)

---

## Overview

The Syncfusion WPF ImageEditor provides comprehensive annotation management capabilities including undo/redo operations, z-ordering controls for layering annotations, and serialization for saving and loading annotation states.

Key annotation management features:
- **Undo/Redo**: Revert or reapply annotation changes
- **Z-Ordering**: Control the layering order of overlapping annotations
- **Serialization**: Save annotation state to XML streams
- **Deserialization**: Load saved annotation state
- **Annotations Collection**: Access all annotations programmatically

---

## Undo and Redo

The undo and redo functionality helps transition the edited image from the current state to a previous state or from the current state to the next state. All editing operations related to shapes and text are stored in the history, allowing easy navigation between states.

### Supported Operations

Undo and redo support the following operations:

| Operation | Description |
|-----------|-------------|
| **Add Shapes/Text** | Adding new annotations |
| **Delete Shapes/Text** | Removing existing annotations |
| **Position Changes** | Moving annotations to different locations |
| **Fill Changes** | Modifying shape fill colors |
| **Stroke Changes** | Changing shape stroke properties |

**Important Note:** Currently, undo and redo support is available **only for shapes and text annotations**. Image transformations (crop, rotate, flip) are not included in the undo/redo history.

### Undo Operation

#### Using Toolbar

The Undo icon appears on the left side of the toolbar:
- Disabled by default when no changes have been made
- Enabled automatically when annotations are added or modified
- Click to undo the last change
- Continue clicking to step backward through all changes

#### Programmatic Undo

Use the `Undo()` method to programmatically undo the last operation:

**Basic Example:**

```csharp
// Undo the last annotation change
editor.Undo();
```

**Complete Example with Button:**

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

        private void UndoButton_Click(object sender, RoutedEventArgs e)
        {
            // Undo the last change
            editor.Undo();
        }

        private void AddShapeButton_Click(object sender, RoutedEventArgs e)
        {
            // Add a shape
            editor.AddShape(AnnotationShape.Rectangle, new Rect(100, 100, 200, 150));

            // Now undo button can reverse this action
        }
    }
}
```

### Redo Operation

#### Using Toolbar

The Redo icon appears next to the Undo icon:
- Enabled only after an undo operation has been performed
- Disabled when there are no undone changes to redo
- Click to reapply the last undone change
- Continue clicking to step forward through undone changes

#### Programmatic Redo

Use the `Redo()` method to programmatically redo the last undone operation:

**Basic Example:**

```csharp
// Redo the last undone change
editor.Redo();
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

        private void RedoButton_Click(object sender, RoutedEventArgs e)
        {
            // Redo the last undone change
            editor.Redo();
        }

        private void UndoButton_Click(object sender, RoutedEventArgs e)
        {
            // First undo something
            editor.Undo();

            // Now redo button can reapply it
        }
    }
}
```

### Limitations

**Current Limitations of Undo/Redo:**

1. **Annotations Only**: Undo/redo works only for shapes and text annotations
2. **No Transformation Support**: Image transformations (crop, rotate, flip, zoom) are not included in undo/redo history
3. **No Save/Load History**: Undo/redo history is lost when the application closes
4. **Linear History**: No branching - performing a new action after undo clears the redo stack

**Example Demonstrating Limitations:**

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

        private void TestUndoLimitations_Click(object sender, RoutedEventArgs e)
        {
            // Add shape - CAN be undone
            editor.AddShape(AnnotationShape.Rectangle, new Rect(100, 100, 200, 150));

            // Add text - CAN be undone
            editor.AddText("Sample Text", new Point(150, 250));

            // Crop image - CANNOT be undone
            editor.Crop(new Rect(0, 0, 500, 500));

            // Undo will only affect text and shape, not crop
            editor.Undo(); // Removes text
            editor.Undo(); // Removes shape
            // Crop remains applied
        }
    }
}
```

---

## Z-Ordering

Z-ordering allows you to control the layering of overlapping annotations. The ImageEditor provides four methods to arrange shapes in the z-axis (depth order):

### BringToFront

Brings the selected annotation to the front of all other annotations, making it appear on top.

**Method:**
```csharp
public void BringToFront()
```

**Example:**

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

        private void BringToFrontButton_Click(object sender, RoutedEventArgs e)
        {
            // Bring currently selected shape to the front
            editor.BringToFront();
        }
    }
}
```

**Use Case:** When you have overlapping shapes and want a specific shape to appear on top of all others.

### BringForward

Moves the selected annotation one layer forward in the z-order.

**Method:**
```csharp
public void BringForward()
```

**Example:**

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

        private void BringForwardButton_Click(object sender, RoutedEventArgs e)
        {
            // Move selected shape one layer forward
            editor.BringForward();
        }
    }
}
```

**Use Case:** Fine-tune layering by moving a shape one level at a time.

### SendToBack

Sends the selected annotation behind all other annotations, making it appear at the bottom of the stack.

**Method:**
```csharp
public void SendToBack()
```

**Example:**

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

        private void SendToBackButton_Click(object sender, RoutedEventArgs e)
        {
            // Send selected shape to the back
            editor.SendToBack();
        }
    }
}
```

**Use Case:** Push a shape behind all others, useful for background elements.

### SendBackward

Moves the selected annotation one layer backward in the z-order.

**Method:**
```csharp
public void SendBackward()
```

**Example:**

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

        private void SendBackwardButton_Click(object sender, RoutedEventArgs e)
        {
            // Move selected shape one layer backward
            editor.SendBackward();
        }
    }
}
```

**Use Case:** Gradually adjust layering by moving a shape one level back at a time.

### Z-Ordering Use Cases

**Example: Creating Layered Annotations**

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

        private void CreateLayeredAnnotations_Click(object sender, RoutedEventArgs e)
        {
            // Add background rectangle (should be at back)
            editor.PenSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Color.FromArgb(150, 200, 200, 200)),
                Stroke = new SolidColorBrush(Colors.Gray),
                StrokeWidth = 2
            };
            editor.AddShape(AnnotationShape.Rectangle, new Rect(100, 100, 400, 300));

            // Add circle (middle layer)
            editor.PenSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Color.FromArgb(200, 100, 150, 255)),
                Stroke = new SolidColorBrush(Colors.Blue),
                StrokeWidth = 3
            };
            editor.AddShape(AnnotationShape.Circle, new Rect(200, 150, 200, 200));

            // Add arrow on top
            editor.PenSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Colors.Red),
                Stroke = new SolidColorBrush(Colors.DarkRed),
                StrokeWidth = 4
            };
            editor.AddShape(AnnotationShape.Arrow, new Rect(250, 200, 150, 80));

            // Now user can select and reorder these using z-order methods
        }
    }
}
```

---

## Serialization

Serialization allows you to save the current state of all annotations (shapes, text, pen drawings, and custom views) to a stream and load them back later.

### Serialize Method

The `Serialize()` method saves all current annotations to an XML stream.

**Method Signature:**
```csharp
public void Serialize(Stream stream)
```

**Basic Example:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.IO;

// Serialize to file
using (FileStream stream = new FileStream("annotations.xml", FileMode.Create))
{
    editor.Serialize(stream);
}
```

**Complete Example with SaveFileDialog:**

```csharp
using Microsoft.Win32;
using Syncfusion.UI.Xaml.ImageEditor;
using System.IO;
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

        private void SaveAnnotations_Click(object sender, RoutedEventArgs e)
        {
            SaveFileDialog dialog = new SaveFileDialog();
            dialog.Title = "Save Annotations";
            dialog.Filter = "XML File (*.xml)|*.xml";
            dialog.DefaultExt = "xml";

            if (dialog.ShowDialog() == true)
            {
                try
                {
                    using (Stream stream = File.Open(dialog.FileName, FileMode.CreateNew))
                    {
                        editor.Serialize(stream);
                    }

                    MessageBox.Show("Annotations saved successfully!",
                                   "Success",
                                   MessageBoxButton.OK,
                                   MessageBoxImage.Information);
                }
                catch (System.Exception ex)
                {
                    MessageBox.Show($"Error saving annotations: {ex.Message}",
                                   "Error",
                                   MessageBoxButton.OK,
                                   MessageBoxImage.Error);
                }
            }
        }
    }
}
```

### Deserialize Method

The `Deserialize()` method loads previously saved annotations from an XML stream.

**Method Signature:**
```csharp
public void Deserialize(Stream stream)
```

**Basic Example:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.IO;

// Deserialize from file
using (FileStream stream = new FileStream("annotations.xml", FileMode.Open))
{
    editor.Deserialize(stream);
}
```

**Complete Example with OpenFileDialog:**

```csharp
using Microsoft.Win32;
using Syncfusion.UI.Xaml.ImageEditor;
using System.IO;
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

        private void LoadAnnotations_Click(object sender, RoutedEventArgs e)
        {
            OpenFileDialog dialog = new OpenFileDialog();
            dialog.Title = "Load Annotations";
            dialog.Filter = "XML File (*.xml)|*.xml";

            if (dialog.ShowDialog() == true)
            {
                try
                {
                    using (Stream myStream = dialog.OpenFile())
                    {
                        editor.Deserialize(myStream);
                    }

                    MessageBox.Show("Annotations loaded successfully!",
                                   "Success",
                                   MessageBoxButton.OK,
                                   MessageBoxImage.Information);
                }
                catch (System.Exception ex)
                {
                    MessageBox.Show($"Error loading annotations: {ex.Message}",
                                   "Error",
                                   MessageBoxButton.OK,
                                   MessageBoxImage.Error);
                }
            }
        }
    }
}
```

### Annotations Collection

The `Annotations` property provides read-only access to all currently visible annotations in the image editor.

**Property:**
```csharp
public ReadOnlyCollection<ImageEditorAnnotationsInfo> Annotations { get; }
```

**Important Note:** The annotations collection will be reset if the background image is changed.

**Accessing All Annotations:**

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

        private void ShowAnnotationCount_Click(object sender, RoutedEventArgs e)
        {
            int count = editor.Annotations.Count;
            MessageBox.Show($"Current annotations: {count}",
                           "Annotation Count",
                           MessageBoxButton.OK,
                           MessageBoxImage.Information);
        }
    }
}
```

### Filtering Annotations

You can filter annotations by type using LINQ:

**Example: Get Only Rectangle Annotations**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Linq;
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

        private void GetRectangles_Click(object sender, RoutedEventArgs e)
        {
            // Filter only rectangle annotations
            var rectangleAnnotations = editor.Annotations
                .ToList()
                .Where(item => item.Type == ShapeType.Rectangle);

            MessageBox.Show($"Rectangle count: {rectangleAnnotations.Count()}");
        }

        private void ReapplyRectangles_Click(object sender, RoutedEventArgs e)
        {
            // Get rectangle annotations
            var rectangleAnnotations = editor.Annotations
                .ToList()
                .Where(item => item.Type == ShapeType.Rectangle);

            // Re-add them (example: duplicate rectangles)
            foreach (var item in rectangleAnnotations)
            {
                editor.AddShape(item.Type, item.PenSettings);
            }
        }
    }
}
```

---

## Complete Working Examples

### Example 1: Annotation Manager Application

```csharp
using Microsoft.Win32;
using Syncfusion.UI.Xaml.ImageEditor;
using System.IO;
using System.Windows;

namespace AnnotationManagerApp
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
        }

        // Undo last change
        private void Undo_Click(object sender, RoutedEventArgs e)
        {
            editor.Undo();
        }

        // Redo last undone change
        private void Redo_Click(object sender, RoutedEventArgs e)
        {
            editor.Redo();
        }

        // Bring selected to front
        private void BringToFront_Click(object sender, RoutedEventArgs e)
        {
            editor.BringToFront();
        }

        // Send selected to back
        private void SendToBack_Click(object sender, RoutedEventArgs e)
        {
            editor.SendToBack();
        }

        // Save annotations
        private void SaveAnnotations_Click(object sender, RoutedEventArgs e)
        {
            SaveFileDialog dialog = new SaveFileDialog();
            dialog.Filter = "XML File (*.xml)|*.xml";

            if (dialog.ShowDialog() == true)
            {
                using (Stream stream = File.Open(dialog.FileName, FileMode.CreateNew))
                {
                    editor.Serialize(stream);
                }
                MessageBox.Show("Annotations saved!");
            }
        }

        // Load annotations
        private void LoadAnnotations_Click(object sender, RoutedEventArgs e)
        {
            OpenFileDialog dialog = new OpenFileDialog();
            dialog.Filter = "XML File (*.xml)|*.xml";

            if (dialog.ShowDialog() == true)
            {
                using (Stream stream = dialog.OpenFile())
                {
                    editor.Deserialize(stream);
                }
                MessageBox.Show("Annotations loaded!");
            }
        }

        // Show annotation statistics
        private void ShowStats_Click(object sender, RoutedEventArgs e)
        {
            int total = editor.Annotations.Count;
            int rectangles = editor.Annotations.Count(a => a.Type == ShapeType.Rectangle);
            int circles = editor.Annotations.Count(a => a.Type == ShapeType.Circle);
            int arrows = editor.Annotations.Count(a => a.Type == ShapeType.Arrow);

            string stats = $"Total Annotations: {total}\n" +
                          $"Rectangles: {rectangles}\n" +
                          $"Circles: {circles}\n" +
                          $"Arrows: {arrows}";

            MessageBox.Show(stats, "Annotation Statistics");
        }
    }
}
```

### Example 2: Layer Manager

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using System.Windows.Media;

namespace LayerManagerApp
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();

            // Subscribe to selection events for enabling layer controls
            editor.ItemSelected += Editor_ItemSelected;
            editor.ItemUnselected += Editor_ItemUnselected;
        }

        private void Editor_ItemSelected(object sender, ItemSelectedEventArgs e)
        {
            // Enable layer controls when annotation is selected
            BringToFrontButton.IsEnabled = true;
            BringForwardButton.IsEnabled = true;
            SendToBackButton.IsEnabled = true;
            SendBackwardButton.IsEnabled = true;
        }

        private void Editor_ItemUnselected(object sender, ItemUnselectedEventArgs e)
        {
            // Disable layer controls when no selection
            BringToFrontButton.IsEnabled = false;
            BringForwardButton.IsEnabled = false;
            SendToBackButton.IsEnabled = false;
            SendBackwardButton.IsEnabled = false;
        }

        // Create overlapping layers
        private void CreateLayers_Click(object sender, RoutedEventArgs e)
        {
            // Background layer
            editor.PenSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Colors.LightGray)
            };
            editor.AddShape(AnnotationShape.Rectangle, new Rect(100, 100, 300, 200));

            // Middle layer
            editor.PenSettings = new PenSettings()
            {
                Fill = new SolidColorBrush(Colors.LightBlue)
            };
            editor.AddShape(AnnotationShape.Circle, new Rect(200, 150, 200, 200));

            // Top layer
            editor.TextSettings = new TextSettings()
            {
                FontSize = 24,
                Color = Colors.Red,
                TextEffects = TextEffects.Bold
            };
            editor.AddText("TOP LAYER", new Point(250, 200));
        }

        private void BringToFront_Click(object sender, RoutedEventArgs e)
        {
            editor.BringToFront();
        }

        private void BringForward_Click(object sender, RoutedEventArgs e)
        {
            editor.BringForward();
        }

        private void SendToBack_Click(object sender, RoutedEventArgs e)
        {
            editor.SendToBack();
        }

        private void SendBackward_Click(object sender, RoutedEventArgs e)
        {
            editor.SendBackward();
        }
    }
}
```

---

## Troubleshooting

### Common Issues and Solutions

**1. Undo/Redo Not Working**

**Problem:** Undo or Redo methods don't seem to work.

**Solutions:**
- Verify that annotations (shapes/text) have been added
- Check that you're not trying to undo image transformations (not supported)
- Ensure the control is properly initialized
- Verify no errors in the output window

**2. Z-Ordering Methods Not Affecting Annotations**

**Problem:** BringToFront, SendToBack, etc. don't change layer order.

**Solutions:**
- Ensure an annotation is selected before calling z-order methods
- Verify overlapping annotations exist (z-order only matters when they overlap)
- Check that the annotation is not locked or disabled

**3. Serialization Fails**

**Problem:** Serialize() throws an exception or produces empty file.

**Solutions:**
- Ensure the stream is writable
- Verify sufficient disk space
- Check file permissions
- Ensure annotations exist before serializing

```csharp
// Check before serializing
if (editor.Annotations.Count > 0)
{
    using (FileStream stream = new FileStream("annotations.xml", FileMode.Create))
    {
        editor.Serialize(stream);
    }
}
else
{
    MessageBox.Show("No annotations to save!");
}
```

**4. Deserialization Doesn't Load Annotations**

**Problem:** Deserialize() completes but no annotations appear.

**Solutions:**
- Verify the XML file is valid and not corrupted
- Ensure the background image is loaded before deserializing
- Check that file path is correct
- Verify the XML format matches expected structure

**5. Annotations Collection is Empty**

**Problem:** Annotations property returns empty collection despite visible annotations.

**Solutions:**
- Check if background image was changed (resets collection)
- Verify annotations were added programmatically or via toolbar
- Ensure you're checking the collection after annotations are added

**6. Redo Not Enabled After Undo**

**Problem:** Redo button/method doesn't work after performing undo.

**Solutions:**
- Verify undo operation completed successfully
- Check that no new annotations were added after undo (clears redo stack)
- Ensure you're calling Redo() and not Undo() again

### Best Practices

1. **Check Annotation Count**: Always verify annotations exist before serialization
2. **Handle Exceptions**: Wrap serialize/deserialize operations in try-catch blocks
3. **User Feedback**: Provide visual feedback for undo/redo availability
4. **Save Regularly**: Encourage users to serialize annotations frequently
5. **Validate Files**: Check XML validity before deserialization
6. **Selection States**: Track selection for enabling/disabling z-order controls
7. **Document Limitations**: Inform users that transformations can't be undone
8. **Clear Instructions**: Explain z-ordering requires annotation selection

---

## See Also

**Related Topics:**
- Adding annotations (shapes and text)
- Customizing annotation appearance
- Image transformations
- Toolbar customization
- Save and reset functionality

---

*This documentation covers annotation management features in the Syncfusion WPF ImageEditor control. For adding and customizing annotations, refer to the annotations-shapes-and-text documentation.*

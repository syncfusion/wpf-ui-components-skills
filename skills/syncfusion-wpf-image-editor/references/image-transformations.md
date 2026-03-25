# Image Transformations

## Table of Contents

- [Overview](#overview)
- [Cropping](#cropping)
  - [Toolbar Cropping](#toolbar-cropping)
  - [Programmatic Cropping](#programmatic-cropping)
    - [Toggle Cropping](#toggle-cropping)
    - [Crop Area Selection](#crop-area-selection)
    - [Ratio-Based Cropping](#ratio-based-cropping)
    - [Rect-Based Cropping](#rect-based-cropping)
    - [Circle Cropping](#circle-cropping)
    - [Circle Cropping with Ratio](#circle-cropping-with-ratio)
    - [Crop Method](#crop-method)
    - [Manual Cropping](#manual-cropping)
  - [Continuous Cropping](#continuous-cropping)
- [Flipping](#flipping)
  - [Toolbar Flip](#toolbar-flip)
  - [Programmatic Flip](#programmatic-flip)
  - [Horizontal Flip](#horizontal-flip)
  - [Vertical Flip](#vertical-flip)
- [Rotating](#rotating)
  - [Toolbar Rotate](#toolbar-rotate)
  - [Programmatic Rotate](#programmatic-rotate)
- [Zooming](#zooming)
  - [EnableZooming Property](#enablezooming-property)
  - [Toolbar Zooming](#toolbar-zooming)
  - [Zoom Level Range](#zoom-level-range)
  - [Mouse Wheel Zooming](#mouse-wheel-zooming)
  - [Reset Zoom](#reset-zoom)
- [Panning](#panning)
  - [EnablePanning Property](#enablepanning-property)
  - [Toolbar Panning](#toolbar-panning)
  - [Select vs Pan Mode](#select-vs-pan-mode)
- [Troubleshooting](#troubleshooting)
  - [Common Issues](#common-issues)
  - [Best Practices](#best-practices)

## Overview

The Syncfusion WPF ImageEditor (SfImageEditor) control provides comprehensive image transformation capabilities that allow users to manipulate images in various ways. This document covers all the transformation features available in the image editor, including:

- **Cropping**: Select and crop specific portions of an image using toolbar or programmatic methods
- **Flipping**: Mirror images horizontally or vertically
- **Rotating**: Rotate images in 90-degree increments
- **Zooming**: Magnify images for detailed viewing (50-400% range)
- **Panning**: Navigate around zoomed images to view hidden portions

These transformations can be performed either through the interactive toolbar UI or programmatically via code, providing flexibility for different use cases and user experiences.

## Cropping

An image can be cropped using toolbar and programmatically. Cropping allows you to select and extract a specific portion of an image, removing unwanted areas and focusing on the region of interest.

### Toolbar Cropping

To crop an image using the toolbar, follow these steps:

1. Click the `Crop` icon in the toolbar
2. Cropping handles will be added on the image
3. Resize the handles to select the required portion
4. A sub toolbar will be displayed below the main toolbar
5. After selecting the cropping portion, click `OK` in the sub toolbar to crop that selected portion

The sub toolbar contains the following icons:

- **Width** - Specifies the width of the cropping area
- **Height** - Specifies the height of the cropping area
- **OK** - Crops the selected portion when clicking OK
- **Cancel** - Removes the cropping handles when clicking Cancel

> **Note:** On specifying width and height, the cropping area is located from the left-top corner.

You can also perform continuous cropping on an image.


#### Rectangle and Circle Format

An image can be cropped in rectangle and circle format. By default, rectangle format is enabled in the toolbar. You can also enable the circle format by using the drop-down button in the crop icon.

### Programmatic Cropping

Cropping can be done programmatically using the following two methods in the image editor:

- ToggleCropping- Selects the cropping area
- Crop - Crops the selected area in an image

### Toggle Cropping

The ToggleCropping method selects the cropping area based on the specified parameters.

### Crop Area Selection

The following method selects the full image area for cropping, and you can resize the image as needed:

```csharp
editor.ToggleCropping();
```

This enables the cropping mode with handles covering the entire image, allowing the user to manually adjust the selection.

### Ratio-Based Cropping

The following method gets the ratio as the parameter to select the cropping area:

```csharp
editor.ToggleCropping(3, 1);
```

This creates a cropping area with a 3:1 aspect ratio, which is useful for maintaining specific proportions.

### Rect-Based Cropping

The following method gets the rect in terms of percentage to select the cropping area:

```csharp
Rect rect = new Rect(20, 20, 50, 50);
editor.ToggleCropping(rect);
```

In this example:
- The first two parameters (20, 20) represent the X and Y position as a percentage from the top-left corner
- The last two parameters (50, 50) represent the width and height as a percentage of the image dimensions

### Circle Cropping

Specify the ToggleCropping optional parameter as `true` as shown in the following code sample to enable the circle or elliptical format:

```csharp
editor.ToggleCropping(new Rect(25, 25, 50, 50), true);
```

This creates an elliptical cropping area starting at 25% from the top-left corner, with a width and height of 50% of the image dimensions.


### Circle Cropping with Ratio

To crop an image in a circle or an ellipse with a specific ratio, use ToggleCropping with a ratio argument and an optional parameter of `true`, which specifies whether the cropping panel should be added in an elliptical or rectangle shape. The default value is `false`.

```csharp
editor.ToggleCropping(1, 1, true);
```

This creates a circular cropping area (1:1 ratio) that maintains a perfect circle shape.

### Crop Method

After selecting the crop area, use the Crop method in the image editor to crop the selected portion as demonstrated in the following method:

```csharp
editor.Crop(new Rect(0, 0, 0, 0));
```

When called with a `Rect(0, 0, 0, 0)`, this method crops the image to the currently selected cropping area (as defined by a previous `ToggleCropping` call).

### Manual Cropping

To manually select and crop a location, use the same Crop method, but specify the portion to be cropped in terms of rect as in the following code:

```csharp
editor.Crop(new Rect(0, 0, 0, 0));
```

For a more specific example with actual coordinates:

```csharp
// Crop a specific region: x=100, y=100, width=300, height=200
editor.Crop(new Rect(100, 100, 300, 200));
```

This bypasses the interactive cropping handles and directly crops the image to the specified rectangular area.

### Continuous Cropping

You can also perform continuous cropping on an image. This means that after cropping an image once, you can crop it again without having to reload or reset the image. Each crop operation works on the result of the previous crop.

### Complete Cropping Example

Here's a complete example demonstrating various cropping techniques:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

public class ImageCroppingExample
{
    private SfImageEditor editor;

    public void PerformVariousCrops()
    {
        // Example 1: Simple full-image cropping mode
        editor.ToggleCropping();
        
        // Example 2: Aspect ratio cropping (16:9 for video)
        editor.ToggleCropping(16, 9);
        
        // Example 3: Percentage-based rectangular cropping
        // Crop center 60% of image
        Rect centerCrop = new Rect(20, 20, 60, 60);
        editor.ToggleCropping(centerCrop);
        
        // Example 4: Circular crop with custom position and size
        Rect circleCrop = new Rect(25, 25, 50, 50);
        editor.ToggleCropping(circleCrop, true);
        
        // Example 5: Perfect circular crop (1:1 ratio)
        editor.ToggleCropping(1, 1, true);
        
        // Example 6: Apply the crop to current selection
        editor.Crop(new Rect(0, 0, 0, 0));
        
        // Example 7: Direct manual crop without selection UI
        // Crop 200x200 pixels starting at position (50, 50)
        editor.Crop(new Rect(50, 50, 200, 200));
    }
}
```

## Flipping

The image editor control provides flipping transformation that allows you to mirror images either horizontally or vertically. This is useful for creating mirror effects or correcting image orientation.

### Toolbar Flip

To flip an image using the toolbar:

1. Click the **Flip** icon in the toolbar
2. A popup will be displayed prompting for whether horizontal flip or vertical flip
3. Select the required flip direction to flip the image

### Programmatic Flip

Programmatically, you can flip an image using the `Flip` method. This method takes the FlipDirection as the parameter to specify whether it is a horizontal flip or vertical flip.

### Horizontal Flip

By default, images will be flipped in horizontal direction. To perform a horizontal flip programmatically:

```csharp
editor.Flip(FlipDirection.Horizontal);
```

A horizontal flip mirrors the image along the vertical axis, making left become right and vice versa.

### Vertical Flip

To perform a vertical flip programmatically:

```csharp
editor.Flip(FlipDirection.Vertical);
```

A vertical flip mirrors the image along the horizontal axis, making top become bottom and vice versa.

### Complete Flipping Example

Here's a complete example demonstrating flipping operations:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using Syncfusion.UI.Xaml.ImageEditor.Enums;

public class ImageFlippingExample
{
    private SfImageEditor editor;

    public void PerformFlipOperations()
    {
        // Flip horizontally (mirror left-right)
        editor.Flip(FlipDirection.Horizontal);
        
        // Flip vertically (mirror top-bottom)
        editor.Flip(FlipDirection.Vertical);
        
        // Double flip returns to original orientation
        editor.Flip(FlipDirection.Horizontal);
        editor.Flip(FlipDirection.Horizontal);
        
        // Flip both directions (equivalent to 180-degree rotation)
        editor.Flip(FlipDirection.Horizontal);
        editor.Flip(FlipDirection.Vertical);
    }
}
```

## Rotating

The image editor control provides rotation transformation that allows you to rotate images in 90-degree increments. This is useful for correcting image orientation or creating different viewing angles.

### Toolbar Rotate

To rotate an image using the toolbar:

1. Click the **Rotate** icon in the toolbar
2. This rotates the image to 90 degrees clockwise from the current state
3. By continuous clicking, the angle will be increased since it is rotated from the current state
4. After 4 clicks (360 degrees), the image returns to its original orientation

### Programmatic Rotate

Use the `Rotate` method to rotate an image to 90 degrees from the current state:

```csharp
editor.Rotate();
```

Each call to the `Rotate()` method rotates the image 90 degrees clockwise.

### Complete Rotation Example

Here's a complete example demonstrating rotation operations:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;

public class ImageRotationExample
{
    private SfImageEditor editor;

    public void PerformRotateOperations()
    {
        // Rotate 90 degrees clockwise
        editor.Rotate();
        
        // Rotate another 90 degrees (now 180 degrees total)
        editor.Rotate();
        
        // Rotate another 90 degrees (now 270 degrees total)
        editor.Rotate();
        
        // Rotate one more time (now 360 degrees - back to original)
        editor.Rotate();
    }
    
    public void RotateToSpecificAngle()
    {
        // To rotate to 90 degrees
        editor.Rotate();
        
        // To rotate to 180 degrees
        editor.Rotate();
        editor.Rotate();
        
        // To rotate to 270 degrees
        editor.Rotate();
        editor.Rotate();
        editor.Rotate();
    }
}
```

### Combining Rotation and Flipping

You can combine rotation and flipping operations for more complex transformations:

```csharp
public void CombineTransformations()
{
    // Rotate 90 degrees, then flip horizontally
    editor.Rotate();
    editor.Flip(FlipDirection.Horizontal);
    
    // This creates a unique orientation that cannot be achieved by rotation alone
}
```

## Zooming

The image editor control provides zooming capabilities that allow users to magnify images for better viewing and detailed inspection. Zooming is particularly useful when working with high-resolution images or when precision editing is required.

### EnableZooming Property

Images can be zoomed in or zoomed out for better viewing, and this can be enabled using the EnableZooming property.

```csharp
editor.EnableZooming = true;
```

When zooming is enabled, users can use various methods to zoom in and out of the image.

### Toolbar Zooming

In the footer toolbar, you can find a slider which helps in increasing the zoom level of an image. The slider provides an intuitive way to control the zoom level interactively.

There will be **Increase** and **Decrease** icons on both sides of the slider. These icons help in increasing or decreasing the zoom level gradually.

At a time, this Increase/Decrease icon can increase/decrease the level up to **10 percent**.

### Zoom Level Range

Zoom level ranges from **50 to 400 percent**. This provides sufficient range for both zooming out to see the complete image in context and zooming in for detailed work:

- **Minimum zoom**: 50% (half the original size)
- **Maximum zoom**: 400% (four times the original size)
- **Default zoom**: 100% (original size)

You can move the slider to increase the zoom level within this range.

### Reset Zoom

To reset the zoom level back to 100%, click the **ResetZoom** icon, which is placed at the left of the DecreaseZoom icon. This instantly returns the image to its original size.

### Mouse Wheel Zooming

You can also zoom an image using the mouse wheel. Based on the mouse wheel delta, images will be zoomed from the cursor position. This provides a natural and intuitive zooming experience:

- **Scroll up**: Zoom in
- **Scroll down**: Zoom out
- **Zoom center**: The point under the cursor

This cursor-centered zooming allows you to quickly zoom into specific areas of interest.

### Complete Zooming Example

Here's a complete example demonstrating zooming operations:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;

public class ImageZoomingExample
{
    private SfImageEditor editor;

    public void ConfigureZooming()
    {
        // Enable zooming functionality
        editor.EnableZooming = true;
    }
    
    public void DisableZooming()
    {
        // Disable zooming functionality
        editor.EnableZooming = false;
    }
}
```

### Zooming in XAML

You can also enable zooming directly in XAML:

```xml
<syncfusion:SfImageEditor 
    x:Name="editor" 
    EnableZooming="True"
    ImageSource="Assets/SampleImage.jpg" />
```

### Zooming Best Practices

1. **Enable zooming for detailed work**: When users need to make precise edits, enable zooming
2. **Use mouse wheel zooming**: It provides the most intuitive experience for users
3. **Combine with panning**: Always enable panning when zooming is enabled so users can navigate the zoomed image
4. **Reset zoom appropriately**: Provide a way to reset zoom after transformations
5. **Consider performance**: Very high zoom levels (>400%) may impact performance on large images

## Panning

The image editor control provides panning capabilities that allow users to navigate around zoomed images to view hidden portions. Panning is essential when working with zoomed images that don't fit entirely within the viewport.

### EnablePanning Property

Panning can be enabled using the `EnablePanning` property. When panning is enabled, shapes and texts cannot be selected. By default, the property is set to `false`.

```csharp
editor.EnablePanning = true;
```

### Toolbar Panning

A zoomed image can be panned to view the hidden portion. To enable pan using the toolbar:

1. Click the **Pan** icon in the top toolbar
2. This enables the panning operation on the image
3. Click and drag on the image to pan around
4. When panning is enabled, shapes or text added in the image cannot be resized or repositioned

### Select vs Pan Mode

To resize shapes or reposition elements, enable the **Select** icon in the toolbar; it will disable the pan operation. Select and Pan operations work like toggle functions:

- **Select Mode**: Allows selection, resizing, and repositioning of shapes and text. Panning is disabled.
- **Pan Mode**: Allows panning around the image. Shape selection is disabled.

This toggle behavior ensures that user interactions are predictable and don't accidentally modify elements when trying to pan, or vice versa.

### Complete Panning Example

Here's a complete example demonstrating panning operations:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;

public class ImagePanningExample
{
    private SfImageEditor editor;

    public void ConfigurePanning()
    {
        // Enable panning functionality
        editor.EnablePanning = true;
    }
    
    public void DisablePanning()
    {
        // Disable panning functionality
        editor.EnablePanning = false;
    }
    
    public void SetupZoomAndPan()
    {
        // Enable both zooming and panning for best user experience
        editor.EnableZooming = true;
        editor.EnablePanning = true;
    }
}
```

### Panning in XAML

You can also enable panning directly in XAML:

```xml
<syncfusion:SfImageEditor 
    x:Name="editor" 
    EnableZooming="True"
    EnablePanning="True"
    ImageSource="Assets/SampleImage.jpg" />
```

### Panning Best Practices

1. **Enable with zooming**: Panning is most useful when zooming is enabled
2. **Use toggle modes**: Implement clear UI to switch between Select and Pan modes
3. **Provide visual feedback**: Show cursor changes or mode indicators when pan mode is active
4. **Consider touch devices**: Panning works well with touch gestures on touch-enabled devices
5. **Default to select mode**: Start with select mode enabled and let users explicitly enable pan mode when needed

## Troubleshooting

### Common Issues

#### Issue 1: Cropping Handles Not Appearing

**Problem**: When calling `ToggleCropping()`, the cropping handles don't appear on the image.

**Solution**:
- Ensure the image is loaded before calling `ToggleCropping()`
- Check that the image editor is properly initialized
- Verify that no other operation is currently active

```csharp
// Wait for image to load
editor.ImageLoaded += (s, e) =>
{
    editor.ToggleCropping();
};
```

#### Issue 2: Crop Method Does Nothing

**Problem**: Calling `Crop()` method doesn't crop the image.

**Solution**:
- Ensure `ToggleCropping()` was called first to select a crop area
- Verify the Rect parameters are within valid ranges (0-100 for percentages)
- Check that the crop area is not zero-sized

```csharp
// Correct usage
editor.ToggleCropping(new Rect(20, 20, 50, 50));
// ... user adjusts if needed ...
editor.Crop(new Rect(0, 0, 0, 0)); // Crops to selected area
```

#### Issue 3: Zoom Level Not Changing

**Problem**: Zoom level doesn't change when using mouse wheel or toolbar.

**Solution**:
- Ensure `EnableZooming` is set to `true`
- Check that the image editor has focus
- Verify no other control is capturing mouse wheel events

```csharp
editor.EnableZooming = true;
editor.Focus(); // Ensure editor has focus
```

#### Issue 4: Cannot Select Shapes After Enabling Panning

**Problem**: After enabling panning, shapes and text cannot be selected or modified.

**Solution**:
- This is expected behavior. Pan and Select modes are mutually exclusive
- Disable panning or use the toolbar to switch to Select mode
- Toggle between modes as needed

```csharp
// Switch to select mode
editor.EnablePanning = false;

// Or toggle in toolbar
// Click the Select icon to disable pan mode
```

#### Issue 5: Flip/Rotate Not Applying

**Problem**: Calling `Flip()` or `Rotate()` doesn't change the image.

**Solution**:
- Ensure the image is loaded
- Check that no crop operation is active
- Verify that you're calling the methods on the correct instance

```csharp
// Ensure image is loaded
if (editor.ImageSource != null)
{
    editor.Rotate();
    editor.Flip(FlipDirection.Horizontal);
}
```

#### Issue 6: Circle Cropping Appears as Rectangle

**Problem**: When specifying `true` for circle cropping, the crop area still appears rectangular.

**Solution**:
- Ensure you're using the correct overload of `ToggleCropping()`
- Check that the boolean parameter is passed correctly
- Verify that aspect ratio parameters are equal for perfect circles

```csharp
// Correct circle cropping
editor.ToggleCropping(1, 1, true); // Perfect circle
editor.ToggleCropping(new Rect(25, 25, 50, 50), true); // Ellipse
```

### Best Practices

#### 1. Image Loading

Always ensure the image is fully loaded before performing transformations:

```csharp
editor.ImageLoaded += (sender, args) =>
{
    // Safe to perform transformations here
    editor.ToggleCropping();
};
```

#### 2. Combining Transformations

When performing multiple transformations, apply them in a logical order:

```csharp
// Good practice: crop first, then transform
editor.ToggleCropping(16, 9);
editor.Crop(new Rect(0, 0, 0, 0));
editor.Rotate();
editor.Flip(FlipDirection.Horizontal);
```

#### 3. Aspect Ratio Preservation

When cropping with specific aspect ratios, use ratio-based cropping instead of rect-based:

```csharp
// Better for maintaining aspect ratio
editor.ToggleCropping(16, 9); // Maintains 16:9 ratio

// Instead of
editor.ToggleCropping(new Rect(0, 0, 100, 56.25)); // Manual calculation
```

#### 4. User Experience

Provide clear feedback and controls:

```csharp
public void SetupUserFriendlyEditor()
{
    // Enable essential features
    editor.EnableZooming = true;
    editor.EnablePanning = true;
    
    // Provide status updates
    editor.ImageLoaded += (s, e) => 
        StatusText.Text = "Image loaded successfully";
    
    editor.ImageSaving += (s, e) => 
        StatusText.Text = "Saving image...";
    
    editor.ImageSaved += (s, e) => 
        StatusText.Text = "Image saved successfully";
}
```

#### 5. Performance Optimization

For large images, consider zoom performance:

```csharp
// Optimize for large images
public void OptimizeForLargeImages()
{
    // Limit maximum zoom for very large images
    if (imageWidth > 4000 || imageHeight > 4000)
    {
        // Use moderate zoom levels
        // Maximum zoom of 200% instead of 400%
    }
}
```

#### 6. Error Handling

Implement proper error handling for transformation operations:

```csharp
try
{
    editor.ToggleCropping(aspectRatioX, aspectRatioY);
    editor.Crop(new Rect(0, 0, 0, 0));
}
catch (Exception ex)
{
    MessageBox.Show($"Cropping failed: {ex.Message}");
    // Optionally reset to a known state
    editor.Reset();
}
```

#### 7. Programmatic vs UI Control

Choose the appropriate method based on your use case:

```csharp
// For automated workflows: Use programmatic methods
public void AutomatedCropping(string inputPath, string outputPath)
{
    editor.LoadImage(inputPath);
    editor.ToggleCropping(16, 9);
    editor.Crop(new Rect(0, 0, 0, 0));
    editor.Save(outputPath);
}

// For user interaction: Enable toolbar
public void InteractiveCropping()
{
    editor.ToolbarSettings.IsVisible = true;
    // Let user interact with toolbar
}
```

---

## Summary

The Syncfusion WPF ImageEditor provides comprehensive transformation capabilities:

- **Cropping**: Rectangle and circular cropping with aspect ratios and precise positioning
- **Flipping**: Horizontal and vertical mirroring
- **Rotating**: 90-degree incremental rotation
- **Zooming**: 50-400% zoom range with multiple input methods
- **Panning**: Navigate zoomed images with toggle mode controls

All transformations can be performed both through the interactive toolbar UI and programmatically via code, offering flexibility for various application scenarios.

# Setup and Basic Operations

This guide covers the essential setup and basic operations for the Syncfusion WPF ImageEditor control, including installation, configuration, loading images, saving, resetting, theme support, and troubleshooting.

---

## Assembly References

To use the SfImageEditor control, you need to add the following assembly references to your WPF project:

### Required Assembly

- **Syncfusion.SfImageEditor.WPF**

### Installation via NuGet

The recommended way to add the required assemblies is through NuGet Package Manager:

1. Right-click on your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for `Syncfusion.SfImageEditor.WPF`
4. Install the package

**Package Manager Console:**
```powershell
Install-Package Syncfusion.SfImageEditor.WPF
```

### Manual Assembly Reference

If you prefer to add assemblies manually:

1. Right-click on "References" in Solution Explorer
2. Select "Add Reference"
3. Browse to the Syncfusion installation directory (typically: `C:\Program Files (x86)\Syncfusion\Essential Studio\WPF\{version}\Assemblies`)
4. Select `Syncfusion.SfImageEditor.WPF.dll`
5. Click OK

**Required assemblies:**
- `Syncfusion.SfImageEditor.WPF.dll`
- `Syncfusion.Tools.WPF.dll`
- `Syncfusion.Shared.WPF.dll`

---

## Creating the Image Editor

### Basic XAML Setup

**Step 1: Add Namespace**

Add the ImageEditor namespace to your XAML file:

```xaml
xmlns:editor="clr-namespace:Syncfusion.UI.Xaml.ImageEditor;assembly=Syncfusion.SfImageEditor.WPF"
```

**Step 2: Add the Control**

```xaml
<Window x:Class="ImageEditorDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editor="clr-namespace:Syncfusion.UI.Xaml.ImageEditor;assembly=Syncfusion.SfImageEditor.WPF"
        Title="Image Editor" Height="600" Width="800">
    <Grid>
        <editor:SfImageEditor x:Name="editor">
        </editor:SfImageEditor>
    </Grid>
</Window>
```

### Creating in Code-Behind

You can also create the ImageEditor control programmatically:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create ImageEditor instance
            SfImageEditor editor = new SfImageEditor();
            
            // Add to the window
            this.Content = editor;
        }
    }
}
```

---

## Loading Images

The SfImageEditor control provides multiple ways to load images for editing.

### Loading Image Using ImageSource Property (XAML)

The simplest way to load an image is by setting the `ImageSource` property in XAML:

```xaml
<editor:SfImageEditor x:Name="editor" ImageSource="Assets/BuildingImage.jpeg">
</editor:SfImageEditor>
```

**Note:** Ensure the image file's Build Action is set to "Content" or "Resource" in Visual Studio.

### Loading Image Using ImageSource Property (Code)

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System;
using System.Windows;
using System.Windows.Media.Imaging;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
            
            BitmapImage image = new BitmapImage();
            image.BeginInit();
            image.UriSource = new Uri(@"Assets/Buldingimage.jpeg", UriKind.Relative);
            image.EndInit();
            editor.ImageSource = image;
        }
    }
}
```

### Loading Image Using Load Method

The `Load()` method provides a more explicit way to load images. It returns a boolean indicating success or failure:

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
            
            // Load image using Load method
            string imagePath = @"D:\Images\Building.jpg";
            bool loadSuccess = editor.Load(imagePath);
            
            if (loadSuccess)
            {
                MessageBox.Show("Image loaded successfully!");
            }
            else
            {
                MessageBox.Show("Failed to load image.");
            }
        }
    }
}
```

### Loading Image Using Image Property

The `Image` property accepts a `BitmapImage` object:

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System;
using System.Windows;
using System.Windows.Media.Imaging;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
            
            // Create BitmapImage
            BitmapImage bitmap = new BitmapImage();
            bitmap.BeginInit();
            bitmap.UriSource = new Uri(@"D:\Images\Building.jpg", UriKind.Absolute);
            bitmap.EndInit();
            
            // Set Image property
            editor.Image = bitmap;
        }
    }
}
```

### Loading Image with User Selection (OpenFileDialog)

Provide a file browser for users to select images:

```csharp
using Microsoft.Win32;
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;
using System.Windows.Media.Imaging;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;

        public MainWindow()
        {
            InitializeComponent();
        }

        private void LoadImage_Click(object sender, RoutedEventArgs e)
        {
            OpenFileDialog openFileDialog = new OpenFileDialog();
            openFileDialog.Filter = "Image Files|*.jpg;*.jpeg;*.png;*.bmp;*.gif";
            openFileDialog.Title = "Select an Image";

            if (openFileDialog.ShowDialog() == true)
            {
                try
                {
                    BitmapImage image = new BitmapImage(new Uri(openFileDialog.FileName));
                    editor.ImageSource = image;
                }
                catch (System.Exception ex)
                {
                    MessageBox.Show($"Error loading image: {ex.Message}",
                                    "Error",
                                    MessageBoxButton.OK,
                                    MessageBoxImage.Error);
                }
            }
        }
    }
}
```

---

## Saving Images

The SfImageEditor provides flexible options for saving edited images, both through the built-in toolbar and programmatically with full control over format, size, and location.

### Saving Using Toolbar

The simplest way to save an image is using the built-in toolbar:

1. Click the **Save** icon in the toolbar
2. A save file dialog will appear
3. Choose the desired location and file name
4. Click Save to save the edited image

### Saving Programmatically

The `Save()` method provides programmatic control over the saving process with three optional parameters:

**Method Signature:**
```csharp
public void Save(string imageFormat = null, Size size = default(Size), string filePath = null)
```

**Basic Save (Shows Dialog):**

```csharp
// Save with default settings - shows save dialog
editor.Save();
```

### Specifying Image Format

You can specify the format in which the image should be saved. Supported formats include:

- `"png"` - PNG format
- `"jpg"` or `"jpeg"` - JPEG format
- `"bmp"` - Bitmap format
- `"gif"` - GIF format

**Example:**

```csharp
// Save as PNG format
editor.Save("png", default(Size), null);

// Save as JPEG format
editor.Save("jpeg", default(Size), null);

// Save as BMP format
editor.Save("bmp", default(Size), null);
```

### Specifying Image Size

You can save the image with a specific size by providing a `Size` parameter:

```csharp
using System.Windows;

// Save image with width 750 and height 500
editor.Save(null, new Size(750, 500), null);

// Save as PNG with specific size
editor.Save("png", new Size(1024, 768), null);

// Save as JPEG with specific size
editor.Save("jpeg", new Size(1920, 1080), null);
```

### Specifying File Location

You can specify the directory or full file path where the image should be saved:

**Save to Specific Directory:**

```csharp
// Save to E:\Images\ directory (shows dialog for filename)
editor.Save(null, default(Size), @"E:\Images\");

// Save as PNG to specific directory
editor.Save("png", default(Size), @"C:\MyImages\");
```

**Complete Save Examples:**

```csharp
// Example 1: Save as PNG, size 800x600, to specific directory
editor.Save("png", new Size(800, 600), @"E:\Images\");

// Example 2: Save as JPEG, original size, to specific directory
editor.Save("jpeg", default(Size), @"C:\Photos\");

// Example 3: Save in original format, resize to 1280x720, show dialog
editor.Save(null, new Size(1280, 720), null);
```

**Complete Working Example:**

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

        private void SaveOriginalSize_Click(object sender, RoutedEventArgs e)
        {
            // Save with default settings
            editor.Save();
        }

        private void SaveAsPNG_Click(object sender, RoutedEventArgs e)
        {
            // Save as PNG format
            editor.Save("png", default(Size), null);
        }

        private void SaveResized_Click(object sender, RoutedEventArgs e)
        {
            // Save with custom size
            editor.Save("jpeg", new Size(1024, 768), null);
        }

        private void SaveToFolder_Click(object sender, RoutedEventArgs e)
        {
            // Save to specific directory
            editor.Save("png", default(Size), @"C:\SavedImages\");
        }

        private void SaveCustom_Click(object sender, RoutedEventArgs e)
        {
            // Save with all custom parameters
            editor.Save("jpeg", new Size(1920, 1080), @"E:\Photos\");
        }
    }
}
```

---

## Save Events

The SfImageEditor provides two events that allow you to monitor and control the saving process: `ImageSaving` (before save) and `ImageSaved` (after save).

### ImageSaving Event

The `ImageSaving` event occurs **before** the image is saved to the destination location. This event allows you to:

- Cancel the default saving behavior
- Access the image stream
- Modify the file name
- Implement custom saving logic

**Event Arguments:**
The event uses `ImageSavingEventArgs` which provides:
- `Cancel` (bool): Set to `true` to cancel the default save operation
- `Stream`: Access to the image stream being saved
- `FileName` (string): Get or set the file name

**XAML Declaration:**

```xaml
<editor:SfImageEditor x:Name="editor"
                      ImageSource="Assets/BuildingImage.jpeg"
                      ImageSaving="Editor_ImageSaving">
</editor:SfImageEditor>
```

**Basic Example:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.IO;

private void Editor_ImageSaving(object sender, ImageSavingEventArgs args)
{
    // Cancel default saving and implement custom logic
    args.Cancel = true;

    // Save to custom location
    FileStream stream = new FileStream(@"E:\Images\Resized.jpg", FileMode.Create);
    args.Stream.CopyTo(stream);
    stream.Seek(0, 0);
    stream.Close();

    // Set custom file name
    args.FileName = "SavedImage";
}
```

**Advanced Example with Format Validation:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.IO;
using System.Windows;

private void Editor_ImageSaving(object sender, ImageSavingEventArgs args)
{
    // Validate or modify filename
    if (string.IsNullOrEmpty(args.FileName))
    {
        args.FileName = $"Image_{DateTime.Now:yyyyMMdd_HHmmss}";
    }

    // Option 1: Allow default save but with custom filename
    // (Don't set Cancel = true)

    // Option 2: Implement completely custom save
    args.Cancel = true;

    try
    {
        string customPath = @"C:\MyImages\";
        if (!Directory.Exists(customPath))
        {
            Directory.CreateDirectory(customPath);
        }

        string fullPath = Path.Combine(customPath, args.FileName + ".jpg");

        using (FileStream fileStream = new FileStream(fullPath, FileMode.Create))
        {
            args.Stream.Seek(0, SeekOrigin.Begin);
            args.Stream.CopyTo(fileStream);
        }

        MessageBox.Show($"Image saved successfully to: {fullPath}");
    }
    catch (System.Exception ex)
    {
        MessageBox.Show($"Error saving image: {ex.Message}");
    }
}
```

### ImageSaved Event

The `ImageSaved` event occurs **after** the image has been successfully saved to the destination location. This event is useful for:

- Logging save operations
- Performing post-save actions
- Updating UI to reflect saved status
- Opening the saved file location

**Event Arguments:**
The event uses `ImageSavedEventArgs` which provides:
- `Location` (string): The full path where the image was saved

**XAML Declaration:**

```xaml
<editor:SfImageEditor x:Name="editor"
                      ImageSource="Assets/BuildingImage.jpeg"
                      ImageSaving="Editor_ImageSaving"
                      ImageSaved="Editor_ImageSaved">
</editor:SfImageEditor>
```

**Basic Example:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

private void Editor_ImageSaved(object sender, ImageSavedEventArgs args)
{
    string filePath = args.Location;
    MessageBox.Show($"Image saved to: {filePath}");
}
```

**Advanced Example with File Operations:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Diagnostics;
using System.IO;
using System.Windows;

private void Editor_ImageSaved(object sender, ImageSavedEventArgs args)
{
    string filePath = args.Location;

    // Log the save operation
    LogSaveOperation(filePath);

    // Show notification
    MessageBox.Show($"Image successfully saved!\n\nLocation: {filePath}",
                   "Save Complete",
                   MessageBoxButton.OK,
                   MessageBoxImage.Information);

    // Ask if user wants to open the folder
    MessageBoxResult result = MessageBox.Show(
        "Would you like to open the folder containing the saved image?",
        "Open Folder",
        MessageBoxButton.YesNo,
        MessageBoxImage.Question);

    if (result == MessageBoxResult.Yes)
    {
        OpenContainingFolder(filePath);
    }
}

private void LogSaveOperation(string filePath)
{
    string logFile = "save_log.txt";
    string logEntry = $"{DateTime.Now:yyyy-MM-dd HH:mm:ss} - Saved: {filePath}\n";
    File.AppendAllText(logFile, logEntry);
}

private void OpenContainingFolder(string filePath)
{
    if (File.Exists(filePath))
    {
        string argument = $"/select, \"{filePath}\"";
        Process.Start("explorer.exe", argument);
    }
}
```

**Complete Working Example with Both Events:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System;
using System.Diagnostics;
using System.IO;
using System.Windows;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;
        private bool customSavePerformed = false;

        public MainWindow()
        {
            InitializeComponent();

            // Subscribe to events
            editor.ImageSaving += Editor_ImageSaving;
            editor.ImageSaved += Editor_ImageSaved;
        }

        private void Editor_ImageSaving(object sender, ImageSavingEventArgs args)
        {
            // Apply custom filename with timestamp
            string timestamp = DateTime.Now.ToString("yyyyMMdd_HHmmss");
            args.FileName = $"EditedImage_{timestamp}";

            // Log the save attempt
            Debug.WriteLine($"Saving image with filename: {args.FileName}");

            // You can cancel and implement custom save if needed
            // args.Cancel = true;
            // customSavePerformed = true;
            // ... custom save logic ...
        }

        private void Editor_ImageSaved(object sender, ImageSavedEventArgs args)
        {
            string filePath = args.Location;

            Debug.WriteLine($"Image saved successfully to: {filePath}");

            // Update UI or perform post-save actions
            MessageBox.Show($"Image saved successfully!\n\nLocation: {filePath}",
                           "Success",
                           MessageBoxButton.OK,
                           MessageBoxImage.Information);

            // Optionally open the folder
            if (MessageBox.Show("Open folder?", "Question",
                               MessageBoxButton.YesNo) == MessageBoxResult.Yes)
            {
                if (File.Exists(filePath))
                {
                    Process.Start("explorer.exe", $"/select, \"{filePath}\"");
                }
            }
        }
    }
}
```

---

## Reset Functionality

The Reset functionality allows you to discard all edits and restore the image to its original state. The SfImageEditor provides both UI-based and programmatic reset options, along with events to monitor the reset process.

### Reset Using Toolbar

The simplest way to reset an image:

1. Click the **Reset** icon in the built-in toolbar
2. All changes will be discarded
3. The image will return to its original state

### Reset Programmatically

You can programmatically reset the image using the `Reset()` method:

**Basic Reset:**

```csharp
// Reset all changes
editor.Reset();
```

**Reset with User Confirmation:**

```csharp
using System.Windows;

private void ResetButton_Click(object sender, RoutedEventArgs e)
{
    MessageBoxResult result = MessageBox.Show(
        "Are you sure you want to reset all changes?",
        "Confirm Reset",
        MessageBoxButton.YesNo,
        MessageBoxImage.Question);

    if (result == MessageBoxResult.Yes)
    {
        editor.Reset();
    }
}
```

### BeginReset Event

The `BeginReset` event occurs **before** the reset operation is executed. This event allows you to:

- Cancel the reset operation
- Confirm with the user
- Save current state before resetting

**Event Arguments:**
The event uses `BeginResetEventArgs` which provides:
- `Cancel` (bool): Set to `true` to prevent the reset operation

**XAML Declaration:**

```xaml
<editor:SfImageEditor x:Name="editor"
                      ImageSource="Assets/BuildingImage.jpeg"
                      BeginReset="Editor_BeginReset">
</editor:SfImageEditor>
```

**Basic Example (Cancel Reset):**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;

private void Editor_BeginReset(object sender, BeginResetEventArgs e)
{
    // Cancel the reset operation
    e.Cancel = true;
}
```

**Example with User Confirmation:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

private void Editor_BeginReset(object sender, BeginResetEventArgs e)
{
    MessageBoxResult result = MessageBox.Show(
        "All changes will be lost. Do you want to continue?",
        "Confirm Reset",
        MessageBoxButton.YesNo,
        MessageBoxImage.Warning);

    if (result == MessageBoxResult.No)
    {
        e.Cancel = true;
    }
}
```

**Advanced Example with Save Option:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

private void Editor_BeginReset(object sender, BeginResetEventArgs e)
{
    MessageBoxResult result = MessageBox.Show(
        "You have unsaved changes. Would you like to save before resetting?",
        "Unsaved Changes",
        MessageBoxButton.YesNoCancel,
        MessageBoxImage.Question);

    switch (result)
    {
        case MessageBoxResult.Yes:
            // Save before resetting
            editor.Save();
            // Allow reset to continue
            break;

        case MessageBoxResult.No:
            // Don't save, allow reset to continue
            break;

        case MessageBoxResult.Cancel:
            // Cancel the reset operation
            e.Cancel = true;
            break;
    }
}
```

### EndReset Event

The `EndReset` event occurs **after** the reset operation has been completed successfully. This event is useful for:

- Updating UI to reflect reset state
- Logging reset operations
- Re-enabling controls
- Notifying the user

**XAML Declaration:**

```xaml
<editor:SfImageEditor x:Name="editor"
                      ImageSource="Assets/BuildingImage.jpeg"
                      BeginReset="Editor_BeginReset"
                      EndReset="Editor_EndReset">
</editor:SfImageEditor>
```

**Basic Example:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System.Windows;

private void Editor_EndReset(object sender, EndResetEventArgs e)
{
    MessageBox.Show("Image has been reset to original state.");
}
```

**Complete Working Example with All Reset Features:**

```csharp
using Syncfusion.UI.Xaml.ImageEditor;
using System;
using System.Diagnostics;
using System.Windows;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        private SfImageEditor editor;
        private bool hasUnsavedChanges = false;

        public MainWindow()
        {
            InitializeComponent();

            // Subscribe to reset events
            editor.BeginReset += Editor_BeginReset;
            editor.EndReset += Editor_EndReset;
        }

        private void Editor_BeginReset(object sender, BeginResetEventArgs e)
        {
            Debug.WriteLine("BeginReset event triggered");

            if (hasUnsavedChanges)
            {
                MessageBoxResult result = MessageBox.Show(
                    "You have unsaved changes that will be lost.\n\n" +
                    "Would you like to save your changes before resetting?",
                    "Unsaved Changes",
                    MessageBoxButton.YesNoCancel,
                    MessageBoxImage.Warning);

                switch (result)
                {
                    case MessageBoxResult.Yes:
                        // Save changes first
                        editor.Save();
                        // Allow reset to continue
                        hasUnsavedChanges = false;
                        break;

                    case MessageBoxResult.No:
                        // Don't save, just reset
                        hasUnsavedChanges = false;
                        break;

                    case MessageBoxResult.Cancel:
                        // Cancel the reset operation
                        e.Cancel = true;
                        Debug.WriteLine("Reset operation cancelled by user");
                        break;
                }
            }
            else
            {
                // No unsaved changes, confirm reset
                MessageBoxResult result = MessageBox.Show(
                    "Are you sure you want to reset the image?",
                    "Confirm Reset",
                    MessageBoxButton.YesNo,
                    MessageBoxImage.Question);

                if (result == MessageBoxResult.No)
                {
                    e.Cancel = true;
                }
            }
        }

        private void Editor_EndReset(object sender, EndResetEventArgs e)
        {
            hasUnsavedChanges = false;

            // Show notification
            MessageBox.Show("Image has been reset to its original state.",
                           "Reset Complete",
                           MessageBoxButton.OK,
                           MessageBoxImage.Information);
        }

        private void ManualResetButton_Click(object sender, RoutedEventArgs e)
        {
            // Programmatically trigger reset
            editor.Reset();
        }
    }
}
```

---

## Theme Support

The SfImageEditor control supports various built-in themes that can be applied to match your application's visual style. Syncfusion provides a comprehensive theming system for consistent styling across all controls.

### Available Themes

Syncfusion provides the following built-in themes:
- **MaterialLight** - Google Material Design light theme
- **MaterialDark** - Google Material Design dark theme
- **MaterialLightBlue** - Material light blue variant
- **MaterialDarkBlue** - Material dark blue variant
- **Office2019Colorful** - Office 2019 colorful theme
- **Office2019Black** - Office 2019 black theme
- **Office2019White** - Office 2019 white theme
- **Office2019DarkGray** - Office 2019 dark gray theme
- **Office2019HighContrast** - High contrast theme for accessibility
- **FluentLight** - Microsoft Fluent light theme
- **FluentDark** - Microsoft Fluent dark theme

### Applying Themes Using SfSkinManager

**Step 1: Add Reference**
Add reference to `Syncfusion.Themes.{ThemeName}.WPF` assembly (e.g., `Syncfusion.Themes.MaterialLight.WPF`)

**Step 2: Add Namespace**

```xaml
xmlns:syncfusion="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
```

**Step 3: Apply Theme**

```xaml
<Window x:Class="ImageEditorDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editor="clr-namespace:Syncfusion.UI.Xaml.ImageEditor;assembly=Syncfusion.SfImageEditor.WPF"
        xmlns:syncfusion="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=MaterialLight}"
        Title="Image Editor" Height="600" Width="800">
    <Grid>
        <editor:SfImageEditor x:Name="editor" ImageSource="Assets/BuildingImage.jpeg">
        </editor:SfImageEditor>
    </Grid>
</Window>
```

**Applying Theme in Code-Behind:**

```csharp
using Syncfusion.SfSkinManager;
using System.Windows;

namespace ImageEditorDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();

            // Set theme for the window and all its children
            SfSkinManager.SetTheme(this, new Theme("MaterialDark"));
        }
    }
}
```

**Applying Theme to Specific Control:**

```csharp
using Syncfusion.SfSkinManager;

// Apply theme to specific control
SfSkinManager.SetTheme(editor, new Theme("FluentLight"));
```

### Creating Custom Themes

For advanced customization, you can create custom themes using ThemeStudio:

1. Visit the [ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)
2. Customize colors, fonts, and styles
3. Download the generated theme assembly
4. Reference it in your project
5. Apply the custom theme using SfSkinManager

**Resources:**
- [Apply theme using SfSkinManager](https://help.syncfusion.com/wpf/themes/skin-manager)
- [Create a custom theme using ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)

---

## Troubleshooting

### Common Issues and Solutions

**1. Image Not Loading**

**Problem:** Image doesn't appear when setting ImageSource or Image property.

**Solutions:**
- Verify the image path is correct and the file exists
- Ensure the image file is included in the project with appropriate Build Action
- For relative paths, set Build Action to "Content" or "Resource"
- Check image format is supported (PNG, JPEG, BMP, GIF)
- Verify file permissions

```csharp
// Check if file exists before loading
string imagePath = @"Assets/BuildingImage.jpeg";
if (System.IO.File.Exists(imagePath))
{
    BitmapImage image = new BitmapImage(new Uri(imagePath, UriKind.Relative));
    editor.ImageSource = image;
}
else
{
    MessageBox.Show("Image file not found!");
}
```

**2. Save Dialog Not Appearing**

**Problem:** Calling `editor.Save()` doesn't show the save dialog.

**Solutions:**
- Ensure you're not canceling the save in the `ImageSaving` event
- Check that no exceptions are being thrown
- Verify the control is properly initialized

**3. Events Not Firing**

**Problem:** ImageSaving, ImageSaved, BeginReset, or EndReset events are not triggered.

**Solutions:**
- Verify event handlers are properly attached
- Check XAML event names match exactly
- Ensure the control reference is correct

```csharp
// Proper event subscription
editor.ImageSaving += Editor_ImageSaving;
editor.ImageSaved += Editor_ImageSaved;
editor.BeginReset += Editor_BeginReset;
editor.EndReset += Editor_EndReset;
```

**4. Theme Not Applying**

**Problem:** Theme doesn't apply to the ImageEditor control.

**Solutions:**
- Ensure the theme assembly is referenced
- Verify the theme name is spelled correctly
- Check that SfSkinManager is properly imported

**5. Assembly Reference Errors**

**Problem:** Cannot find Syncfusion assemblies or types.

**Solutions:**
- Install the appropriate NuGet package: `Syncfusion.SfImageEditor.WPF`
- Verify the version matches across all Syncfusion packages
- Clean and rebuild the solution
- Check target framework compatibility

**6. Stream Already Disposed Error**

**Problem:** Error when trying to access stream in ImageSaving event.

**Solutions:**
- Don't dispose the stream in your event handler
- Copy the stream to a new stream if you need to persist it
- Seek to position 0 before reading/writing

```csharp
private void Editor_ImageSaving(object sender, ImageSavingEventArgs args)
{
    args.Cancel = true;

    // Seek to beginning before copying
    args.Stream.Seek(0, SeekOrigin.Begin);

    using (FileStream fs = new FileStream("output.jpg", FileMode.Create))
    {
        args.Stream.CopyTo(fs);
    }
}
```

**7. High Memory Usage**

**Problem:** Application uses too much memory when working with large images.

**Solutions:**
- Dispose of BitmapImage objects when no longer needed
- Consider resizing large images before loading
- Use appropriate image formats (JPEG for photographs, PNG for graphics)

---

**Related Topics:**
- Image transformations (crop, rotate, flip)
- Adding annotations and shapes
- Undo and redo functionality
- Zoom and pan operations
- Toolbar customization

---

*This documentation covers the setup and basic operations for the Syncfusion WPF ImageEditor control. For advanced features like annotations, transformations, and custom toolbar configurations, refer to the specific documentation sections.*

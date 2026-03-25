# State Persistence in DocumentContainer

## Table of Contents
- [Overview](#overview)
- [Storage Formats](#storage-formats)
- [Registry Persistence](#registry-persistence)
- [Isolated Storage Persistence](#isolated-storage-persistence)
- [XML File Persistence](#xml-file-persistence)
- [Binary File Persistence](#binary-file-persistence)
- [State Management Methods](#state-management-methods)
- [Formatters](#formatters)
- [Best Practices](#best-practices)
- [Complete Examples](#complete-examples)

## Overview

State persistence allows you to save and restore the DocumentContainer's layout,

 including document positions, sizes, window states, tab orders, and more. This enables users to close your application and return later to find their workspace exactly as they left it.

**What Gets Saved:**
- Document/window positions (MDI mode)
- Document/window sizes (MDI mode)
- Window states (minimized, maximized, normal)
- Tab order (TDI mode)
- Tab groups (TDI mode)
- Pinned tabs (TDI mode)
- Active document selection

**Storage Options:**
- Windows Registry
- Isolated Storage
- XML files
- Binary files

## Storage Formats

The DocumentContainer supports multiple storage locations and formats:

| Storage Type | Format Options | Best For |
|--------------|----------------|----------|
| **Registry** | Binary, SOAP | Quick persistence, no file management |
| **Isolated Storage** | Binary | Automatic per-user storage |
| **XML Files** | Binary, SOAP | Human-readable, portable, version control |
| **Binary Files** | Binary, SOAP | Compact size, faster I/O |

### Format Comparison

**Binary Formatter:**
- Faster serialization
- Smaller file size
- Not human-readable
- Recommended for production

**SOAP Formatter:**
- Human-readable XML
- Larger file size
- Slower serialization
- Good for debugging
- **Note:** Requires `System.Runtime.Serialization.Formatters.Soap` assembly

## Registry Persistence

Save and load state directly to/from the Windows Registry.

### Saving to Registry

```csharp
using System.Runtime.Serialization.Formatters.Binary;

// Save state to Registry using Binary formatter
BinaryFormatter formatter = new BinaryFormatter();
documentContainer.SaveDockState(formatter);
```

```csharp
using System.Runtime.Serialization.Formatters.Soap;

// Save state to Registry using SOAP formatter
SoapFormatter formatter = new SoapFormatter();
documentContainer.SaveDockState(formatter);
```

### Loading from Registry

```csharp
using System.Runtime.Serialization.Formatters.Binary;

// Load state from Registry using Binary formatter
BinaryFormatter formatter = new BinaryFormatter();
documentContainer.LoadDockState(formatter);
```

```csharp
using System.Runtime.Serialization.Formatters.Soap;

// Load state from Registry using SOAP formatter
SoapFormatter formatter = new SoapFormatter();
documentContainer.LoadDockState(formatter);
```

### Registry Location

State is saved to:
```
HKEY_CURRENT_USER\Software\[YourCompany]\[YourApplication]\DocumentContainer
```

## Isolated Storage Persistence

Isolated Storage provides automatic per-user storage without manual file management.

### Saving to Isolated Storage

```csharp
// Save state to Isolated Storage (default location)
documentContainer.SaveDockState();
```

**What Happens:**
- Creates isolated storage specific to current user and application
- No file paths needed
- Automatic storage management

### Loading from Isolated Storage

```csharp
// Load state from Isolated Storage
documentContainer.LoadDockState();
```

### Resetting Isolated Storage

```csharp
// Reset state by deleting from Isolated Storage
documentContainer.DeleteInternalIsolatedStorage();
```

### Advantages of Isolated Storage

- ✅ Automatic per-user separation
- ✅ No file path management
- ✅ Secure sandboxed storage
- ✅ Works in ClickOnce deployments
- ✅ No file system permissions needed

## XML File Persistence

Save state as XML for human-readable, portable, version-controllable storage.

### Saving to XML (Binary Formatter)

```csharp
using System.Runtime.Serialization.Formatters.Binary;

// Save to XML file using Binary formatter
BinaryFormatter formatter = new BinaryFormatter();
documentContainer.SaveDockState(formatter, StorageFormat.Xml, @"d:\layouts\document_layout.xml");
```

### Saving to XML (SOAP Formatter)

```csharp
using System.Runtime.Serialization.Formatters.Soap;

// Save to XML file using SOAP formatter
SoapFormatter formatter = new SoapFormatter();
documentContainer.SaveDockState(formatter, StorageFormat.Xml, @"d:\layouts\document_layout.xml");
```

### Loading from XML (Binary Formatter)

```csharp
using System.Runtime.Serialization.Formatters.Binary;

// Load from XML file using Binary formatter
BinaryFormatter formatter = new BinaryFormatter();
documentContainer.LoadDockState(formatter, StorageFormat.Xml, @"d:\layouts\document_layout.xml");
```

### Loading from XML (SOAP Formatter)

```csharp
using System.Runtime.Serialization.Formatters.Soap;

// Load from XML file using SOAP formatter
SoapFormatter formatter = new SoapFormatter();
documentContainer.LoadDockState(formatter, StorageFormat.Xml, @"d:\layouts\document_layout.xml");
```

### XML File Benefits

- ✅ Human-readable (with SOAP formatter)
- ✅ Can be version controlled (Git, SVN)
- ✅ Portable across machines
- ✅ Easy to backup and restore
- ✅ Can be edited manually (with caution)

## Binary File Persistence

Save state as binary files for compact, fast storage.

### Saving to Binary (Binary Formatter)

```csharp
using System.Runtime.Serialization.Formatters.Binary;

// Save to binary file using Binary formatter
BinaryFormatter formatter = new BinaryFormatter();
documentContainer.SaveDockState(formatter, StorageFormat.Binary, @"d:\layouts\document_layout.bin");
```

### Saving to Binary (SOAP Formatter)

```csharp
using System.Runtime.Serialization.Formatters.Soap;

// Save to binary file using SOAP formatter
SoapFormatter formatter = new SoapFormatter();
documentContainer.SaveDockState(formatter, StorageFormat.Binary, @"d:\layouts\document_layout.bin");
```

### Loading from Binary (Binary Formatter)

```csharp
using System.Runtime.Serialization.Formatters.Binary;

// Load from binary file using Binary formatter
BinaryFormatter formatter = new BinaryFormatter();
documentContainer.LoadDockState(formatter, StorageFormat.Binary, @"d:\layouts\document_layout.bin");
```

### Loading from Binary (SOAP Formatter)

```csharp
using System.Runtime.Serialization.Formatters.Soap;

// Load from binary file using SOAP formatter
SoapFormatter formatter = new SoapFormatter();
documentContainer.LoadDockState(formatter, StorageFormat.Binary, @"d:\layouts\document_layout.bin");
```

### Binary File Benefits

- ✅ Smallest file size
- ✅ Fastest I/O performance
- ✅ Portable across machines
- ✅ Good for frequent save/load operations

## State Management Methods

### ResetState Method

Resets the DocumentContainer to its default state:

```csharp
// Reset all document positions, sizes, and states
documentContainer.ResetState();
```

**What Gets Reset:**
- All window/tab positions
- Window sizes (MDI)
- Window states (minimized, maximized)
- Tab orders (TDI)
- Tab groups (TDI)
- Active selection

### DeleteDockState Method

Deletes saved state from files:

```csharp
// Delete XML state file
documentContainer.DeleteDockState(@"d:\layouts\document_layout.xml");

// Delete binary state file
documentContainer.DeleteDockState(@"d:\layouts\document_layout.bin");

// Delete from Isolated Storage
documentContainer.DeleteDockState();
```

### DeleteInternalIsolatedStorage Method

Specifically deletes state from Isolated Storage:

```csharp
// Remove state from Isolated Storage
documentContainer.DeleteInternalIsolatedStorage();
```

## Formatters

### Binary Formatter

**Namespace:** `System.Runtime.Serialization.Formatters.Binary`

**Characteristics:**
- Fast serialization
- Compact binary output
- Not human-readable
- Cross-machine compatible

**Usage:**
```csharp
using System.Runtime.Serialization.Formatters.Binary;

BinaryFormatter formatter = new BinaryFormatter();
documentContainer.SaveDockState(formatter, StorageFormat.Xml, @"layout.xml");
```

### SOAP Formatter

**Namespace:** `System.Runtime.Serialization.Formatters.Soap`

**Required Assembly:** `System.Runtime.Serialization.Formatters.Soap.dll`

**Characteristics:**
- Produces XML output
- Human-readable
- Larger file size
- Slower serialization
- Good for debugging

**Usage:**
```csharp
using System.Runtime.Serialization.Formatters.Soap;

SoapFormatter formatter = new SoapFormatter();
documentContainer.SaveDockState(formatter, StorageFormat.Xml, @"layout.xml");
```

**Note:** Add reference to `System.Runtime.Serialization.Formatters.Soap` assembly to use SOAP formatter.

## Best Practices

### 1. Save on Application Exit

```csharp
protected override void OnClosing(CancelEventArgs e)
{
    base.OnClosing(e);
    
    try
    {
        // Save state before closing
        BinaryFormatter formatter = new BinaryFormatter();
        string layoutPath = Path.Combine(
            Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
            "MyApp",
            "layout.xml");
        
        Directory.CreateDirectory(Path.GetDirectoryName(layoutPath));
        documentContainer.SaveDockState(formatter, StorageFormat.Xml, layoutPath);
    }
    catch (Exception ex)
    {
        // Log error but don't prevent closing
        Debug.WriteLine($"Failed to save layout: {ex.Message}");
    }
}
```

### 2. Load on Application Start

```csharp
private void Window_Loaded(object sender, RoutedEventArgs e)
{
    try
    {
        string layoutPath = Path.Combine(
            Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
            "MyApp",
            "layout.xml");
        
        if (File.Exists(layoutPath))
        {
            BinaryFormatter formatter = new BinaryFormatter();
            documentContainer.LoadDockState(formatter, StorageFormat.Xml, layoutPath);
        }
    }
    catch (Exception ex)
    {
        // Log error and continue with default layout
        Debug.WriteLine($"Failed to load layout: {ex.Message}");
    }
}
```

### 3. Handle Errors Gracefully

```csharp
private bool TryLoadState(string filePath)
{
    try
    {
        if (!File.Exists(filePath))
            return false;
        
        BinaryFormatter formatter = new BinaryFormatter();
        documentContainer.LoadDockState(formatter, StorageFormat.Xml, filePath);
        return true;
    }
    catch (Exception ex)
    {
        Debug.WriteLine($"State load failed: {ex.Message}");
        
        // Optionally delete corrupt file
        try
        {
            File.Delete(filePath);
        }
        catch { }
        
        return false;
    }
}
```

### 4. Version Your State Files

```csharp
private const int CurrentLayoutVersion = 2;

private void SaveStateWithVersion()
{
    string layoutPath = GetLayoutPath();
    
    // Save version info separately
    File.WriteAllText(layoutPath + ".version", CurrentLayoutVersion.ToString());
    
    // Save layout
    BinaryFormatter formatter = new BinaryFormatter();
    documentContainer.SaveDockState(formatter, StorageFormat.Xml, layoutPath);
}

private bool LoadStateWithVersionCheck()
{
    string layoutPath = GetLayoutPath();
    string versionPath = layoutPath + ".version";
    
    if (!File.Exists(versionPath) || !File.Exists(layoutPath))
        return false;
    
    int savedVersion = int.Parse(File.ReadAllText(versionPath));
    
    if (savedVersion != CurrentLayoutVersion)
    {
        // Version mismatch - use default layout
        File.Delete(layoutPath);
        File.Delete(versionPath);
        return false;
    }
    
    BinaryFormatter formatter = new BinaryFormatter();
    documentContainer.LoadDockState(formatter, StorageFormat.Xml, layoutPath);
    return true;
}
```

### 5. Provide Reset Option

```xaml
<Menu>
    <MenuItem Header="View">
        <MenuItem Header="Reset Layout" Click="ResetLayout_Click"/>
        <MenuItem Header="Save Layout As..." Click="SaveLayoutAs_Click"/>
        <MenuItem Header="Load Layout..." Click="LoadLayout_Click"/>
    </MenuItem>
</Menu>
```

```csharp
private void ResetLayout_Click(object sender, RoutedEventArgs e)
{
    var result = MessageBox.Show(
        "Reset layout to default?",
        "Reset Layout",
        MessageBoxButton.YesNo,
        MessageBoxImage.Question);
    
    if (result == MessageBoxResult.Yes)
    {
        documentContainer.ResetState();
    }
}
```

### 6. Auto-Save Periodically

```csharp
private DispatcherTimer autoSaveTimer;

private void SetupAutoSave()
{
    autoSaveTimer = new DispatcherTimer();
    autoSaveTimer.Interval = TimeSpan.FromMinutes(5);
    autoSaveTimer.Tick += (s, e) => SaveState();
    autoSaveTimer.Start();
}

private void SaveState()
{
    try
    {
        string layoutPath = GetLayoutPath();
        BinaryFormatter formatter = new BinaryFormatter();
        documentContainer.SaveDockState(formatter, StorageFormat.Xml, layoutPath);
    }
    catch (Exception ex)
    {
        Debug.WriteLine($"Auto-save failed: {ex.Message}");
    }
}
```

## Complete Examples

### Example 1: Simple Isolated Storage Persistence

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.Loaded += MainWindow_Loaded;
        this.Closing += MainWindow_Closing;
    }
    
    private void MainWindow_Loaded(object sender, RoutedEventArgs e)
    {
        try
        {
            // Load state from Isolated Storage
            documentContainer.LoadDockState();
        }
        catch
        {
            // Use default layout if load fails
        }
    }
    
    private void MainWindow_Closing(object sender, CancelEventArgs e)
    {
        try
        {
            // Save state to Isolated Storage
            documentContainer.SaveDockState();
        }
        catch
        {
            // Continue closing even if save fails
        }
    }
}
```

### Example 2: XML File with User Choice

```csharp
public partial class MainWindow : Window
{
    private string LayoutPath => Path.Combine(
        Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
        "MyApp",
        "DocumentLayout.xml");
    
    public MainWindow()
    {
        InitializeComponent();
        this.Loaded += MainWindow_Loaded;
        this.Closing += MainWindow_Closing;
    }
    
    private void MainWindow_Loaded(object sender, RoutedEventArgs e)
    {
        LoadLayout();
    }
    
    private void MainWindow_Closing(object sender, CancelEventArgs e)
    {
        SaveLayout();
    }
    
    private void SaveLayout()
    {
        try
        {
            Directory.CreateDirectory(Path.GetDirectoryName(LayoutPath));
            
            BinaryFormatter formatter = new BinaryFormatter();
            documentContainer.SaveDockState(formatter, StorageFormat.Xml, LayoutPath);
            
            StatusMessage.Text = "Layout saved successfully";
        }
        catch (Exception ex)
        {
            StatusMessage.Text = $"Failed to save layout: {ex.Message}";
        }
    }
    
    private void LoadLayout()
    {
        try
        {
            if (File.Exists(LayoutPath))
            {
                BinaryFormatter formatter = new BinaryFormatter();
                documentContainer.LoadDockState(formatter, StorageFormat.Xml, LayoutPath);
                
                StatusMessage.Text = "Layout loaded successfully";
            }
        }
        catch (Exception ex)
        {
            StatusMessage.Text = $"Failed to load layout: {ex.Message}";
        }
    }
    
    private void ResetLayout_Click(object sender, RoutedEventArgs e)
    {
        documentContainer.ResetState();
        
        if (File.Exists(LayoutPath))
        {
            File.Delete(LayoutPath);
        }
        
        StatusMessage.Text = "Layout reset to default";
    }
    
    private void SaveLayoutAs_Click(object sender, RoutedEventArgs e)
    {
        var dialog = new Microsoft.Win32.SaveFileDialog
        {
            Filter = "Layout Files (*.xml)|*.xml|All Files (*.*)|*.*",
            DefaultExt = ".xml"
        };
        
        if (dialog.ShowDialog() == true)
        {
            BinaryFormatter formatter = new BinaryFormatter();
            documentContainer.SaveDockState(formatter, StorageFormat.Xml, dialog.FileName);
            StatusMessage.Text = $"Layout saved to {dialog.FileName}";
        }
    }
    
    private void LoadLayoutFrom_Click(object sender, RoutedEventArgs e)
    {
        var dialog = new Microsoft.Win32.OpenFileDialog
        {
            Filter = "Layout Files (*.xml)|*.xml|All Files (*.*)|*.*"
        };
        
        if (dialog.ShowDialog() == true)
        {
            BinaryFormatter formatter = new BinaryFormatter();
            documentContainer.LoadDockState(formatter, StorageFormat.Xml, dialog.FileName);
            StatusMessage.Text = $"Layout loaded from {dialog.FileName}";
        }
    }
}
```

### Example 3: Multiple Layout Profiles

```csharp
public class LayoutManager
{
    private readonly DocumentContainer container;
    private readonly string layoutDirectory;
    
    public LayoutManager(DocumentContainer container)
    {
        this.container = container;
        this.layoutDirectory = Path.Combine(
            Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
            "MyApp",
            "Layouts");
        
        Directory.CreateDirectory(layoutDirectory);
    }
    
    public void SaveProfile(string profileName)
    {
        string path = GetProfilePath(profileName);
        BinaryFormatter formatter = new BinaryFormatter();
        container.SaveDockState(formatter, StorageFormat.Xml, path);
    }
    
    public bool LoadProfile(string profileName)
    {
        string path = GetProfilePath(profileName);
        
        if (!File.Exists(path))
            return false;
        
        try
        {
            BinaryFormatter formatter = new BinaryFormatter();
            container.LoadDockState(formatter, StorageFormat.Xml, path);
            return true;
        }
        catch
        {
            return false;
        }
    }
    
    public List<string> GetProfiles()
    {
        return Directory.GetFiles(layoutDirectory, "*.xml")
            .Select(Path.GetFileNameWithoutExtension)
            .ToList();
    }
    
    public void DeleteProfile(string profileName)
    {
        string path = GetProfilePath(profileName);
        if (File.Exists(path))
        {
            File.Delete(path);
        }
    }
    
    private string GetProfilePath(string profileName)
    {
        return Path.Combine(layoutDirectory, $"{profileName}.xml");
    }
}

// Usage
private LayoutManager layoutManager;

private void SetupLayoutProfiles()
{
    layoutManager = new LayoutManager(documentContainer);
    
    // Populate profile dropdown
    profileComboBox.ItemsSource = layoutManager.GetProfiles();
    profileComboBox.SelectedItem = "Default";
}

private void SaveProfile_Click(object sender, RoutedEventArgs e)
{
    string profileName = profileNameTextBox.Text;
    if (!string.IsNullOrWhiteSpace(profileName))
    {
        layoutManager.SaveProfile(profileName);
        profileComboBox.ItemsSource = layoutManager.GetProfiles();
        MessageBox.Show($"Profile '{profileName}' saved successfully");
    }
}

private void LoadProfile_Click(object sender, RoutedEventArgs e)
{
    string profileName = profileComboBox.SelectedItem as string;
    if (profileName != null)
    {
        if (layoutManager.LoadProfile(profileName))
        {
            MessageBox.Show($"Profile '{profileName}' loaded successfully");
        }
        else
        {
            MessageBox.Show($"Failed to load profile '{profileName}'");
        }
    }
}
```

## Troubleshooting

**Issue:** State doesn't save/load
- **Solution:** Ensure formatters are properly initialized and file paths are valid

**Issue:** Corrupt state file prevents loading
- **Solution:** Wrap load operations in try-catch and delete corrupt files

**Issue:** State loads but layout looks wrong
- **Solution:** Version your state files; delete old versions after major changes

**Issue:** Permission errors when saving to files
- **Solution:** Use ApplicationData folder or Isolated Storage instead of Program Files

**Issue:** State doesn't persist between users
- **Solution:** Use per-user storage (ApplicationData or Isolated Storage) not shared locations

## Related Documentation

- **Container Modes:** See `container-modes.md` for what state is saved in TDI vs MDI
- **Window Management:** See `window-management.md` for MDI window positions/sizes
- **Tab Management:** See `tab-management.md` for TDI tab orders and groups

# State Persistence

## Table of Contents
- [Overview](#overview)
- [Enabling State Persistence](#enabling-state-persistence)
- [Saving State](#saving-state)
- [Loading State](#loading-state)
- [Serialization Formats](#serialization-formats)
- [Controlling Serialization](#controlling-serialization)
- [Advanced Scenarios](#advanced-scenarios)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

State Persistence allows DockingManager to save and restore the complete layout configuration, including window positions, sizes, states (Dock/Float/AutoHidden/Document), and tab orders. This enables users to maintain their customized workspace across application sessions.

## Enabling State Persistence

### Basic Enable

Set `PersistState` property to enable automatic persistence:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          PersistState="True">
</syncfusion:DockingManager>
```

**C#:**
```csharp
dockManager.PersistState = true;
```

When enabled, DockingManager automatically saves state to IsolatedStorage on application close and restores on startup.

### Automatic vs Manual Persistence

**Automatic (PersistState=True):**
- Saves on application close
- Loads on startup
- Uses IsolatedStorage
- Minimal code required

**Manual (PersistState=False):**
- Explicit save/load control
- Custom storage location
- Save at specific points
- Format selection

## Saving State

### Save to IsolatedStorage

Use `SaveDockState()` to save to IsolatedStorage:

**C#:**
```csharp
// Save current layout
dockManager.SaveDockState();
```

This creates a file in IsolatedStorage with serialized layout data.

### Save to Custom Location

Save to specific file path:

**C#:**
```csharp
// Save to binary format
string savePath = Path.Combine(
    Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
    "MyApp",
    "layout.xml"
);

Directory.CreateDirectory(Path.GetDirectoryName(savePath));

using (FileStream stream = new FileStream(savePath, FileMode.Create))
{
    dockManager.SaveDockState(stream, AppMode.Binary);
}
```

### Save on Application Close

**C#:**
```csharp
protected override void OnClosing(CancelEventArgs e)
{
    // Save layout before closing
    dockManager.SaveDockState();
    
    base.OnClosing(e);
}
```

### Save on User Action

```csharp
private void SaveLayout_Click(object sender, RoutedEventArgs e)
{
    try
    {
        dockManager.SaveDockState();
        MessageBox.Show("Layout saved successfully!");
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Failed to save layout: {ex.Message}");
    }
}
```

## Loading State

### Load from IsolatedStorage

Use `LoadDockState()` to restore from IsolatedStorage:

**C#:**
```csharp
// Restore saved layout
dockManager.LoadDockState();
```

### Load from Custom Location

Load from specific file:

**C#:**
```csharp
string loadPath = Path.Combine(
    Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
    "MyApp",
    "layout.xml"
);

if (File.Exists(loadPath))
{
    using (FileStream stream = new FileStream(loadPath, FileMode.Open))
    {
        dockManager.LoadDockState(stream, AppMode.Binary);
    }
}
```

### Load on Application Startup

**C#:**
```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Load after initialization
    Loaded += (s, e) =>
    {
        try
        {
            dockManager.LoadDockState();
        }
        catch
        {
            // Ignore if no saved state exists
        }
    };
}
```

### Reset to Default Layout

```csharp
public void ResetLayout()
{
    // Clear saved state
    dockManager.ResetState();
    
    // Or reload default layout from resource
    LoadDefaultLayout();
}
```

## Serialization Formats

DockingManager supports multiple serialization formats:

| Format | Usage | Pros | Cons |
|--------|-------|------|------|
| **Binary** | `AppMode.Binary` | Compact, fast | Not human-readable |
| **XML** | `AppMode.XML` | Human-readable, debuggable | Larger file size |
| **IsolatedStorage** | (default) | Automatic, no code needed | Can't customize path |

**Example - XML serialization:**
```csharp
// Save to XML
using (FileStream stream = new FileStream(path, FileMode.Create))
{
    dockManager.SaveDockState(stream, AppMode.XML);
}

// Load from XML
using (FileStream stream = new FileStream(path, FileMode.Open))
{
    dockManager.LoadDockState(stream, AppMode.XML);
}
```

Use `XmlWriter`/`XmlReader` for custom XML handling with more control.

## Controlling Serialization

### Exclude Windows from Serialization

Prevent specific windows from being saved:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Temporary Window"
                syncfusion:DockingManager.State="Dock"
                syncfusion:DockingManager.SideInDockedMode="Left"
                syncfusion:DockingManager.CanSerialize="False" />
```

**C#:**
```csharp
// Exclude window from serialization
DockingManager.SetCanSerialize(tempWindow, false);
```

### Serialize Custom Properties

Store additional metadata:

**C#:**
```csharp
// Before saving, store custom data in Tag
foreach (var child in dockManager.Children.OfType<FrameworkElement>())
{
    var customData = new Dictionary<string, object>
    {
        ["IsModified"] = GetModifiedState(child),
        ["FilePath"] = GetFilePath(child)
    };
    
    child.Tag = customData;
}

// Save
dockManager.SaveDockState();

// After loading, retrieve custom data
dockManager.Loaded += (s, e) =>
{
    dockManager.LoadDockState();
    
    foreach (var child in dockManager.Children.OfType<FrameworkElement>())
    {
        if (child.Tag is Dictionary<string, object> customData)
        {
            RestoreCustomState(child, customData);
        }
    }
};
```

## Advanced Scenarios

### Multiple Layout Profiles

Save and load different layout configurations:

**C#:**
```csharp
public class LayoutManager
{
    private DockingManager _dockManager;
    private string _layoutFolder;
    
    public LayoutManager(DockingManager dockManager)
    {
        _dockManager = dockManager;
        _layoutFolder = Path.Combine(
            Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
            "MyApp",
            "Layouts"
        );
        Directory.CreateDirectory(_layoutFolder);
    }
    
    public void SaveLayout(string name)
    {
        string path = Path.Combine(_layoutFolder, $"{name}.xml");
        
        using (FileStream stream = new FileStream(path, FileMode.Create))
        {
            _dockManager.SaveDockState(stream, AppMode.XML);
        }
    }
    
    public void LoadLayout(string name)
    {
        string path = Path.Combine(_layoutFolder, $"{name}.xml");
        
        if (File.Exists(path))
        {
            using (FileStream stream = new FileStream(path, FileMode.Open))
            {
                _dockManager.LoadDockState(stream, AppMode.XML);
            }
        }
    }
    
    public List<string> GetSavedLayouts()
    {
        return Directory.GetFiles(_layoutFolder, "*.xml")
            .Select(Path.GetFileNameWithoutExtension)
            .ToList();
    }
    
    public void DeleteLayout(string name)
    {
        string path = Path.Combine(_layoutFolder, $"{name}.xml");
        if (File.Exists(path))
        {
            File.Delete(path);
        }
    }
}
```

**Usage:**
```csharp
LayoutManager layoutMgr = new LayoutManager(dockManager);

// Save current as "Debug Layout"
layoutMgr.SaveLayout("Debug Layout");

// Save another profile
layoutMgr.SaveLayout("Editing Layout");

// Load specific layout
layoutMgr.LoadLayout("Debug Layout");

// List all layouts
var layouts = layoutMgr.GetSavedLayouts();
```

### Version-Safe Serialization

Handle layout format changes between versions:

**C#:**
```csharp
public void SaveLayoutWithVersion()
{
    string path = GetLayoutPath();
    
    using (FileStream stream = new FileStream(path, FileMode.Create))
    using (XmlWriter writer = XmlWriter.Create(stream, new XmlWriterSettings { Indent = true }))
    {
        writer.WriteStartDocument();
        writer.WriteStartElement("LayoutData");
        
        // Write version info
        writer.WriteElementString("Version", "2.0");
        writer.WriteElementString("AppVersion", Assembly.GetExecutingAssembly().GetName().Version.ToString());
        writer.WriteElementString("SavedDate", DateTime.Now.ToString("o"));
        
        // Write layout
        writer.WriteStartElement("DockingLayout");
        dockManager.SaveDockState(writer);
        writer.WriteEndElement();
        
        writer.WriteEndElement();
        writer.WriteEndDocument();
    }
}

public bool LoadLayoutWithVersion()
{
    string path = GetLayoutPath();
    if (!File.Exists(path)) return false;
    
    using (FileStream stream = new FileStream(path, FileMode.Open))
    using (XmlReader reader = XmlReader.Create(stream))
    {
        reader.ReadToFollowing("LayoutData");
        
        // Read version
        reader.ReadToFollowing("Version");
        string version = reader.ReadElementContentAsString();
        
        // Check compatibility
        if (IsCompatibleVersion(version))
        {
            reader.ReadToFollowing("DockingLayout");
            dockManager.LoadDockState(reader);
            return true;
        }
        else
        {
            MessageBox.Show("Layout version incompatible. Using default layout.");
            return false;
        }
    }
}

private bool IsCompatibleVersion(string version)
{
    // Implement version check logic
    Version savedVer = new Version(version);
    Version currentVer = new Version("2.0");
    return savedVer.Major == currentVer.Major;
}
```

### Clear Saved State

Remove persisted layout:

**C#:**
```csharp
public void ClearSavedState()
{
    // Reset to default
    dockManager.ResetState();
    
    // Delete from IsolatedStorage
    // (internal, handled by ResetState)
    
    // Or delete custom file
    string customPath = GetLayoutPath();
    if (File.Exists(customPath))
    {
        File.Delete(customPath);
    }
}
```

## Common Patterns

**Auto-Save with Debounce:** Use `DispatcherTimer` to delay saves after layout changes (prevents rapid successive saves).

**Layout Profiles:** Save multiple named layouts to allow users to switch between workspace configurations. Hook `WindowStateChanged` and `ActiveWindowChanged` events to detect layout modifications.

## Troubleshooting

### State Not Persisting

❌ **Problem:** Layout not saving/loading automatically.

✅ **Solutions:**
```csharp
// Ensure PersistState is enabled
dockManager.PersistState = true;

// Manually save on close
protected override void OnClosing(CancelEventArgs e)
{
    dockManager.SaveDockState();
    base.OnClosing(e);
}

// Manually load on startup
public MainWindow()
{
    InitializeComponent();
    Loaded += (s, e) =>
    {
        try
        {
            dockManager.LoadDockState();
        }
        catch { /* Ignore if no saved state */ }
    };
}
```

### Windows Missing After Load

❌ **Problem:** Some windows don't appear after loading state.

✅ **Check:**
```csharp
// Ensure windows are created BEFORE loading state
public MainWindow()
{
    InitializeComponent();
    
    // Create all windows first
    CreateAllWindows();
    
    // Then load state
    Loaded += (s, e) => dockManager.LoadDockState();
}

// Verify CanSerialize is not false
DockingManager.SetCanSerialize(window, true);
```

### Load Fails Silently

❌ **Problem:** LoadDockState doesn't restore layout.

✅ **Debug:**
```csharp
try
{
    dockManager.LoadDockState();
}
catch (FileNotFoundException)
{
    Console.WriteLine("No saved state found");
}
catch (InvalidOperationException ex)
{
    Console.WriteLine($"Layout data corrupted: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Failed to load: {ex.Message}");
}
```

### File Access Errors

❌ **Problem:** Exception when saving/loading from file.

✅ **Solution:**
```csharp
public void SafeSaveLayout(string path)
{
    try
    {
        // Ensure directory exists
        Directory.CreateDirectory(Path.GetDirectoryName(path));
        
        // Use FileShare.None for exclusive access
        using (FileStream stream = new FileStream(
            path, 
            FileMode.Create, 
            FileAccess.Write, 
            FileShare.None))
        {
            dockManager.SaveDockState(stream, AppMode.XML);
        }
    }
    catch (IOException ex)
    {
        MessageBox.Show($"Cannot save layout: {ex.Message}");
    }
    catch (UnauthorizedAccessException)
    {
        MessageBox.Show("Permission denied. Cannot save layout to this location.");
    }
}
```

### Layout Corrupted

❌ **Problem:** Saved layout causes errors on load.

✅ **Solution:**
```csharp
public void LoadLayoutSafely()
{
    try
    {
        dockManager.LoadDockState();
    }
    catch
    {
        // Clear corrupted state
        dockManager.ResetState();
        
        // Delete bad file
        string layoutFile = GetLayoutPath();
        if (File.Exists(layoutFile))
        {
            try
            {
                File.Delete(layoutFile);
            }
            catch { /* Ignore */ }
        }
        
        MessageBox.Show("Layout was corrupted and has been reset.");
    }
}
```

### Dynamic Windows Not Persisting

❌ **Problem:** Dynamically created windows lose state.

✅ **Solution:**
```csharp
// Set Name property for dynamic windows
ContentControl dynamicWindow = new ContentControl();
dynamicWindow.Name = "DynamicWindow_" + Guid.NewGuid().ToString("N").Substring(0, 8);

DockingManager.SetHeader(dynamicWindow, "Dynamic");
DockingManager.SetState(dynamicWindow, DockState.Dock);
DockingManager.SetSideInDockedMode(dynamicWindow, DockSide.Left);

// Important: Register name in scope
RegisterName(dynamicWindow.Name, dynamicWindow);

dockManager.Children.Add(dynamicWindow);
```

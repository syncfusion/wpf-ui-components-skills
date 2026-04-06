# MDI and TDI Functionalities

## Table of Contents
- [Overview](#overview)
- [Enabling Document Container](#enabling-document-container)
- [MDI vs TDI Modes](#mdi-vs-tdi-modes)
- [Document Window States](#document-window-states)
- [Window Layouts](#window-layouts)
- [Tab Groups](#tab-groups)
- [Pin and Unpin Tabs](#pin-and-unpin-tabs)
- [Closing Documents](#closing-documents)
- [Document Events](#document-events)
- [Full-Screen Mode](#full-screen-mode)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

DockingManager supports MDI (Multiple Document Interface) and TDI (Tabbed Document Interface) for document management. Use MDI for traditional windowed documents or TDI for modern tabbed document experiences similar to Visual Studio.

## Enabling Document Container

### Basic Setup

Enable document container to host document windows:

**XAML:**
```xml
<syncfusion:DockingManager x:Name="dockManager"
                          UseDocumentContainer="True">
    <ContentControl syncfusion:DockingManager.Header="Document1.cs"
                    syncfusion:DockingManager.State="Document" />
    
    <ContentControl syncfusion:DockingManager.Header="Document2.xaml"
                    syncfusion:DockingManager.State="Document" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
dockManager.UseDocumentContainer = true;

ContentControl doc1 = new ContentControl();
DockingManager.SetHeader(doc1, "Document1.cs");
DockingManager.SetState(doc1, DockState.Document);

ContentControl doc2 = new ContentControl();
DockingManager.SetHeader(doc2, "Document2.xaml");
DockingManager.SetState(doc2, DockState.Document);

dockManager.Children.Add(doc1);
dockManager.Children.Add(doc2);
```

## MDI vs TDI Modes

### Container Mode

Switch between MDI and TDI:

**XAML:**
```xml
<!-- TDI (Tabbed) Mode - Default -->
<syncfusion:DockingManager UseDocumentContainer="True"
                          ContainerMode="TDI">
</syncfusion:DockingManager>

<!-- MDI (Windowed) Mode -->
<syncfusion:DockingManager UseDocumentContainer="True"
                          ContainerMode="MDI">
</syncfusion:DockingManager>
```

**C#:**
```csharp
// Set to TDI mode
dockManager.ContainerMode = DocumentContainerMode.TDI;

// Set to MDI mode
dockManager.ContainerMode = DocumentContainerMode.MDI;
```

### TDI Mode (Tabbed)

**Characteristics:**
- Documents appear as tabs
- One document visible at a time
- Tab switching for navigation
- Modern UI similar to browsers/VS Code

**Best for:**
- Code editors
- Multi-file editing
- Modern applications

### MDI Mode (Windowed)

**Characteristics:**
- Documents in separate child windows
- Multiple documents visible simultaneously
- Windows can overlap
- Traditional Office-style interface

**Best for:**
- Image viewers
- Data comparison
- Multi-view applications

## Document Window States

### Basic Document State

Create documents with Document state:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="README.md"
                syncfusion:DockingManager.State="Document" />
```

**C#:**
```csharp
DockingManager.SetState(document, DockState.Document);
```

### Control Document Abilities

Restrict document interactions:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Read Only"
                syncfusion:DockingManager.State="Document"
                syncfusion:DockingManager.CanDocument="False"
                syncfusion:DockingManager.CanDock="False"
                syncfusion:DockingManager.CanFloat="False" />
```

**C#:**
```csharp
// Prevent changing from document state
DockingManager.SetCanDocument(window, false);
DockingManager.SetCanDock(window, false);
DockingManager.SetCanFloat(window, false);
```

## Window Layouts

Arrange MDI documents in different patterns:

**Available layouts (set MDIWindowLayout property):**
- `Cascade`: Overlapping windows (default)
- `HorizontalSplit`, `VerticalSplit`: Tiled arrangements
- `TileHorizontal`, `TileVertical`: Grid layouts

**XAML:**
```xml
<syncfusion:DockingManager ContainerMode="MDI" MDIWindowLayout="Cascade">
</syncfusion:DockingManager>
```

**C# (programmatic or via menu):**
```csharp
dockManager.MDIWindowLayout = MDIWindowLayout.Cascade;
dockManager.MDIWindowLayout = MDIWindowLayout.TileHorizontal;
// ... other layouts
```

## Tab Groups

### Creating Tab Groups

Split document area into multiple tab groups:

**Horizontal Tab Groups:**
```csharp
// Get document container
var docContainer = dockManager.DocContainer;

// Create horizontal tab group (side-by-side)
docContainer.CreateHorizontalTabGroup();
```

**Vertical Tab Groups:**
```csharp
// Create vertical tab group (top and bottom)
docContainer.CreateVerticalTabGroup();
```

### Moving Documents Between Groups

Drag and drop documents between tab groups:

1. Drag document tab
2. Drop on another tab group
3. Document moves to new group

**Programmatic Move:**
```csharp
// Move document to specific tab group
var tabGroup = docContainer.Items[0] as DocumentTabControl;
if (tabGroup != null)
{
    // Implementation depends on Syncfusion version
    // Typically involves manipulating tab group items
}
```

### Tab Group Orientation

Control how tab groups are split:

**XAML:**
```xml
<syncfusion:DockingManager UseDocumentContainer="True"
                          TabGroupOrientation="Horizontal">
    <!-- Horizontal: Side-by-side groups
         Vertical: Top and bottom groups -->
</syncfusion:DockingManager>
```

### Remove Tab Groups

**C#:**
```csharp
// Merge all tab groups back to single group
var docContainer = dockManager.DocContainer;

// Close all groups except first
while (docContainer.Items.Count > 1)
{
    docContainer.Items.RemoveAt(docContainer.Items.Count - 1);
}
```

## Pin and Unpin Tabs

### Enable Pin/Unpin Feature

Allow users to pin important documents:

**XAML:**
```xml
<syncfusion:DockingManager UseDocumentContainer="True"
                          EnableDocumentTabPinning="True">
    <ContentControl syncfusion:DockingManager.Header="Pinned Doc"
                    syncfusion:DockingManager.State="Document"
                    syncfusion:DockingManager.IsPinnedTab="True" />
</syncfusion:DockingManager>
```

**C#:**
```csharp
// Enable pinning feature
dockManager.EnableDocumentTabPinning = true;

// Pin specific document
DockingManager.SetIsPinnedTab(document, true);

// Unpin document
DockingManager.SetIsPinnedTab(document, false);
```

### Pin Tab Behavior

**Pinned tabs:**
- Appear at the start of tab list
- Visual indicator (usually pin icon)
- Stay open when closing other tabs
- Prevent accidental closure

### Programmatic Pin Management

```csharp
// Get all pinned documents
var pinnedDocs = dockManager.Children
    .OfType<FrameworkElement>()
    .Where(e => DockingManager.GetState(e) == DockState.Document &&
                DockingManager.GetIsPinnedTab(e))
    .ToList();

// Pin all modified documents
foreach (var doc in dockManager.Children.OfType<FrameworkElement>())
{
    if (DockingManager.GetState(doc) == DockState.Document)
    {
        // Check if modified (custom logic)
        bool isModified = CheckIfModified(doc);
        DockingManager.SetIsPinnedTab(doc, isModified);
    }
}
```

## Closing Documents

### Close Single Document

**XAML:**
```xml
<ContentControl x:Name="myDoc"
                syncfusion:DockingManager.Header="Closeable Doc"
                syncfusion:DockingManager.State="Document"
                syncfusion:DockingManager.CanClose="True" />
```

**C#:**
```csharp
// Enable close button
DockingManager.SetCanClose(document, true);

// Programmatically close
dockManager.Children.Remove(document);
```

### Close All Documents

```csharp
public void CloseAllDocuments()
{
    var documents = dockManager.Children
        .OfType<FrameworkElement>()
        .Where(e => DockingManager.GetState(e) == DockState.Document)
        .ToList();
    
    foreach (var doc in documents)
    {
        dockManager.Children.Remove(doc);
    }
}
```

### Close All Except Active

```csharp
public void CloseAllExceptActive()
{
    var activeDoc = dockManager.ActiveWindow;
    
    var documents = dockManager.Children
        .OfType<FrameworkElement>()
        .Where(e => DockingManager.GetState(e) == DockState.Document && 
                    e != activeDoc)
        .ToList();
    
    foreach (var doc in documents)
    {
        dockManager.Children.Remove(doc);
    }
}
```

### Prevent Document Closure

Handle closing event to confirm:

**C#:**
```csharp
dockManager.CloseButtonClick += (s, e) =>
{
    var header = DockingManager.GetHeader(e.TargetItem);
    
    var result = MessageBox.Show(
        $"Save changes to {header}?",
        "Confirm Close",
        MessageBoxButton.YesNoCancel
    );
    
    if (result == MessageBoxResult.Yes)
    {
        SaveDocument(e.TargetItem);
    }
    else if (result == MessageBoxResult.Cancel)
    {
        e.Cancel = true; // Prevent closing
    }
};
```

## Document Events

### Active Document Changed

Track which document is currently active:

**C#:**
```csharp
dockManager.ActiveWindowChanged += (s, e) =>
{
    if (e.NewValue != null)
    {
        var header = DockingManager.GetHeader(e.NewValue);
        Console.WriteLine($"Active document: {header}");
        
        // Update UI, load document-specific tools, etc.
    }
};
```

### Document Tab Order Changed

Detect when user reorders tabs:

**C#:**
```csharp
dockManager.DocumentTabOrderChanged += (s, e) =>
{
    // e.OldIndex: Previous position
    // e.NewIndex: New position
    Console.WriteLine($"Tab moved from {e.OldIndex} to {e.NewIndex}");
};
```

### Document Context Menu

Customize tab context menu:

**XAML:**
```xml
<ContentControl syncfusion:DockingManager.Header="Custom Menu Doc"
                syncfusion:DockingManager.State="Document">
    <syncfusion:DockingManager.DocumentContextMenu>
        <ContextMenu>
            <MenuItem Header="Save" Click="Save_Click" />
            <MenuItem Header="Save As..." Click="SaveAs_Click" />
            <Separator />
            <MenuItem Header="Close" Click="Close_Click" />
            <MenuItem Header="Close All" Click="CloseAll_Click" />
            <MenuItem Header="Close All Except This" Click="CloseAllExceptThis_Click" />
        </ContextMenu>
    </syncfusion:DockingManager.DocumentContextMenu>
</ContentControl>
```

## Full-Screen Mode

### Enable Full-Screen Mode

Hide tab panel for distraction-free viewing:

**XAML:**
```xml
<syncfusion:DockingManager UseDocumentContainer="True"
                          TDIFullScreenMode="WindowMode">
    <!-- TDIFullScreenMode options:
         - None: Disabled (default)
         - WindowMode: Full window, tab panel hidden (mouse-over shows)
         - ControlMode: Normal window, tab panel hidden (mouse-over shows) -->
</syncfusion:DockingManager>
```

**C#:**
```csharp
// Window mode - full screen
dockManager.TDIFullScreenMode = FullScreenMode.WindowMode;

// Control mode - hide tabs only
dockManager.TDIFullScreenMode = FullScreenMode.ControlMode;

// Disable
dockManager.TDIFullScreenMode = FullScreenMode.None;
```

### Toggle Full-Screen

```csharp
public void ToggleFullScreen()
{
    if (dockManager.TDIFullScreenMode == FullScreenMode.None)
    {
        dockManager.TDIFullScreenMode = FullScreenMode.WindowMode;
    }
    else
    {
        dockManager.TDIFullScreenMode = FullScreenMode.None;
    }
}
```

## Troubleshooting

### Document Container Not Visible

❌ **Problem:** Document area doesn't appear.

✅ **Solution:**
```csharp
// Ensure UseDocumentContainer is true
dockManager.UseDocumentContainer = true;

// Add at least one document
ContentControl doc = new ContentControl();
DockingManager.SetState(doc, DockState.Document);
dockManager.Children.Add(doc);
```

### Tab Groups Not Creating

❌ **Problem:** CreateHorizontalTabGroup() doesn't work.

✅ **Check:**
```csharp
// Ensure in TDI mode
if (dockManager.ContainerMode != DocumentContainerMode.TDI)
{
    dockManager.ContainerMode = DocumentContainerMode.TDI;
}

// Get DocContainer properly
var docContainer = dockManager.DocContainer;
if (docContainer != null)
{
    docContainer.CreateHorizontalTabGroup();
}
```

### Pinned Tabs Not Working

❌ **Problem:** Pin feature not available.

✅ **Solution:**
```csharp
// Enable pinning feature
dockManager.EnableDocumentTabPinning = true;

// Set on documents
DockingManager.SetIsPinnedTab(document, true);
```

### MDI Layout Not Applying

❌ **Problem:** Window layout doesn't change.

✅ **Check:**
```csharp
// Ensure in MDI mode (not TDI)
if (dockManager.ContainerMode != DocumentContainerMode.MDI)
{
    dockManager.ContainerMode = DocumentContainerMode.MDI;
}

// Then apply layout
dockManager.MDIWindowLayout = MDIWindowLayout.Cascade;
```

### Can't Close Document

❌ **Problem:** Close button doesn't appear.

✅ **Solution:**
```csharp
// Enable close button
DockingManager.SetCanClose(document, true);

// Verify state is Document
if (DockingManager.GetState(document) != DockState.Document)
{
    DockingManager.SetState(document, DockState.Document);
}
```

### Full-Screen Mode Not Working

❌ **Problem:** TDIFullScreenMode has no effect.

✅ **Check:**
```csharp
// Only works in TDI mode
dockManager.ContainerMode = DocumentContainerMode.TDI;

// Then enable full-screen
dockManager.TDIFullScreenMode = FullScreenMode.WindowMode;
```

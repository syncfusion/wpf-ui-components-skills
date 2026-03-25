# Window Management in MDI Mode

## Table of Contents
- [Overview](#overview)
- [Minimizing MDI Windows](#minimizing-mdi-windows)
- [Maximizing MDI Windows](#maximizing-mdi-windows)
- [Window States](#window-states)
- [Resizing MDI Windows](#resizing-mdi-windows)
- [Setting MDI Bounds](#setting-mdi-bounds)
- [Window State Events](#window-state-events)
- [Keyboard Window Management](#keyboard-window-management)
- [Common Patterns](#common-patterns)

## Overview

When using MDI (Multiple Document Interface) mode, the DocumentContainer provides comprehensive window management features. Each document appears as an independent window with its own title bar, borders, and window controls. Users can minimize, maximize, resize, and position these windows freely within the container.

**Note:** Window management features are **only available in MDI mode**. TDI mode uses tabs instead of windows.

## Minimizing MDI Windows

### Enabling Minimize Feature

By default, MDI windows cannot be minimized. Enable this feature using the `CanMDIMinimize` property:

```xaml
<syncfusion:DocumentContainer Name="DocContainer"
                              CanMDIMinimize="True" 
                              Mode="MDI">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Features">
        <FlowDocument>
            <Paragraph>This window can be minimized</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Window1">
        <FlowDocument>
            <Paragraph>Another minimizable window</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document Container">
        <FlowDocument>
            <Paragraph>Third minimizable window</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
DocumentContainer docContainer = new DocumentContainer();
docContainer.Mode = DocumentContainerMode.MDI;
docContainer.CanMDIMinimize = true;

// Add windows
FlowDocumentScrollViewer window1 = new FlowDocumentScrollViewer();
DocumentContainer.SetHeader(window1, "Window 1");
docContainer.Items.Add(window1);
```

### Minimized Window Behavior

When windows are minimized:
- They collapse to a small title bar
- They are arranged in the bottom-left corner of the container
- They stack one by one, side by side
- Users can restore them by clicking the title bar
- They remain accessible via window switcher (CTRL+TAB)

### Disabling Minimize Feature

To prevent users from minimizing windows, set `CanMDIMinimize` to `False` (this is the default):

```xaml
<syncfusion:DocumentContainer Name="DocContainer"
                              CanMDIMinimize="False" 
                              Mode="MDI">
    <!-- Windows here -->
</syncfusion:DocumentContainer>
```

## Maximizing MDI Windows

### Enabling Maximize Feature

Enable window maximization using the `CanMDIMaximize` property:

```xaml
<syncfusion:DocumentContainer Name="DocContainer"
                              CanMDIMaximize="True" 
                              Mode="MDI">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Features">
        <FlowDocument>
            <Paragraph>This window can be maximized</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Window1">
        <FlowDocument>
            <Paragraph>Another maximizable window</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
DocumentContainer docContainer = new DocumentContainer();
docContainer.Mode = DocumentContainerMode.MDI;
docContainer.CanMDIMaximize = true;

// Add windows
FlowDocumentScrollViewer window1 = new FlowDocumentScrollViewer();
DocumentContainer.SetHeader(window1, "Window 1");
docContainer.Items.Add(window1);
```

### Maximized Window Behavior

When a window is maximized:
- It fills the entire container area
- Window borders are hidden
- Only the active window's controls remain visible
- Users can restore it to normal size
- Other windows remain accessible via window switcher

### Disabling Maximize Feature

To prevent maximization, set `CanMDIMaximize` to `False` (this is the default):

```xaml
<syncfusion:DocumentContainer Name="DocContainer"
                              CanMDIMaximize="False" 
                              Mode="MDI">
    <!-- Windows here -->
</syncfusion:DocumentContainer>
```

### Combining Minimize and Maximize

Enable both features for full window control:

```xaml
<syncfusion:DocumentContainer Name="DocContainer"
                              Mode="MDI"
                              CanMDIMinimize="True"
                              CanMDIMaximize="True">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Flexible Window">
        <FlowDocument>
            <Paragraph>This window supports all states</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

## Window States

### Available States

MDI windows support three states:
- **Normal:** Default size and position
- **Minimized:** Collapsed to title bar in bottom-left corner
- **Maximized:** Expanded to fill entire container

### Setting Initial Window State

Use the `MDIWindowState` attached property to set the initial state of a window:

```xaml
<syncfusion:DocumentContainer Name="DocContainer" Mode="MDI">
    <!-- Minimized window -->
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.MDIWindowState="Minimized"
                             syncfusion:DocumentContainer.Header="Minimized Window">
        <FlowDocument>
            <Paragraph>This window starts minimized</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <!-- Maximized window -->
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.MDIWindowState="Maximized"
                             syncfusion:DocumentContainer.Header="Maximized Window">
        <FlowDocument>
            <Paragraph>This window starts maximized</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <!-- Normal window -->
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.MDIWindowState="Normal"
                             syncfusion:DocumentContainer.Header="Normal Window">
        <FlowDocument>
            <Paragraph>This window starts at normal size</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
// Create window
FlowDocumentScrollViewer window1 = new FlowDocumentScrollViewer();
DocumentContainer.SetHeader(window1, "My Window");

// Set initial state
DocumentContainer.SetMDIWindowState(window1, MDIWindowState.Maximized);

// Add to container
docContainer.Items.Add(window1);
```

### MDIWindowState Enum

```csharp
public enum MDIWindowState
{
    Normal,      // Default size and position
    Minimized,   // Collapsed to title bar
    Maximized    // Fills entire container
}
```

### Changing Window State Programmatically

```csharp
// Get the window element
var window = docContainer.Items[0] as FrameworkElement;

// Change state
DocumentContainer.SetMDIWindowState(window, MDIWindowState.Maximized);

// Or minimize it
DocumentContainer.SetMDIWindowState(window, MDIWindowState.Minimized);

// Or restore to normal
DocumentContainer.SetMDIWindowState(window, MDIWindowState.Normal);
```

## Resizing MDI Windows

### Enabling Window Resizing

By default, users can resize MDI windows by dragging their borders. Control this behavior with the `IsAllowMDIResize` property:

```xaml
<syncfusion:DocumentContainer Name="DocContainer" 
                              IsAllowMDIResize="True"  
                              Mode="MDI">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Resizable Window">
        <FlowDocument>
            <Paragraph>This window can be resized</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
docContainer.IsAllowMDIResize = true;
```

### Disabling Window Resizing

To prevent users from resizing windows:

```xaml
<syncfusion:DocumentContainer Name="DocContainer" 
                              IsAllowMDIResize="False"  
                              Mode="MDI">
    <!-- Windows with fixed size -->
</syncfusion:DocumentContainer>
```

### Resize Behavior

When resizing is enabled:
- Users can drag window borders and corners
- Windows maintain their aspect ratio (optional)
- Minimum and maximum sizes can be enforced
- Resize cursor appears when hovering over borders

## Setting MDI Bounds

### MDIBounds Property

The `MDIBounds` attached property controls the position and size of individual windows within the container.

**Syntax:** `MDIBounds="X,Y,Width,Height"`

Where:
- **X:** Horizontal position (left coordinate)
- **Y:** Vertical position (top coordinate)
- **Width:** Window width
- **Height:** Window height

### Setting Window Position and Size

```xaml
<syncfusion:DocumentContainer Name="DocContainer" Mode="MDI">
    <!-- Window at position (0,0) with size 200x300 -->
    <FlowDocumentScrollViewer 
        syncfusion:DocumentContainer.MDIBounds="0,0,200,300"
        syncfusion:DocumentContainer.Header="Top-Left Window">
        <FlowDocument>
            <Paragraph>Positioned at top-left</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <!-- Window at position (220,0) with size 250,350 -->
    <FlowDocumentScrollViewer 
        syncfusion:DocumentContainer.MDIBounds="220,0,250,350"
        syncfusion:DocumentContainer.Header="Top-Right Window">
        <FlowDocument>
            <Paragraph>Positioned at top-right</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <!-- Window at position (0,320) with size 470,300 -->
    <FlowDocumentScrollViewer 
        syncfusion:DocumentContainer.MDIBounds="0,320,470,300"
        syncfusion:DocumentContainer.Header="Bottom Window">
        <FlowDocument>
            <Paragraph>Positioned at bottom</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
// Create window
FlowDocumentScrollViewer window1 = new FlowDocumentScrollViewer();
DocumentContainer.SetHeader(window1, "Positioned Window");

// Set bounds: X=100, Y=50, Width=300, Height=400
DocumentContainer.SetMDIBounds(window1, new Rect(100, 50, 300, 400));

// Add to container
docContainer.Items.Add(window1);
```

### Arranging Multiple Windows

```csharp
private void ArrangeWindowsCascade()
{
    int x = 0, y = 0;
    int offsetX = 30, offsetY = 30;
    int width = 400, height = 300;
    
    foreach (FrameworkElement item in docContainer.Items)
    {
        DocumentContainer.SetMDIBounds(item, new Rect(x, y, width, height));
        x += offsetX;
        y += offsetY;
    }
}

private void ArrangeWindowsTileHorizontal()
{
    int windowCount = docContainer.Items.Count;
    if (windowCount == 0) return;
    
    double containerWidth = docContainer.ActualWidth;
    double containerHeight = docContainer.ActualHeight;
    double windowWidth = containerWidth / windowCount;
    
    for (int i = 0; i < windowCount; i++)
    {
        var item = docContainer.Items[i] as FrameworkElement;
        DocumentContainer.SetMDIBounds(item, 
            new Rect(i * windowWidth, 0, windowWidth, containerHeight));
    }
}

private void ArrangeWindowsTileVertical()
{
    int windowCount = docContainer.Items.Count;
    if (windowCount == 0) return;
    
    double containerWidth = docContainer.ActualWidth;
    double containerHeight = docContainer.ActualHeight;
    double windowHeight = containerHeight / windowCount;
    
    for (int i = 0; i < windowCount; i++)
    {
        var item = docContainer.Items[i] as FrameworkElement;
        DocumentContainer.SetMDIBounds(item, 
            new Rect(0, i * windowHeight, containerWidth, windowHeight));
    }
}
```

## Window State Events

### MDIWindowStateChanging Event

The `MDIWindowStateChanging` event fires before a window's state changes, allowing you to cancel or modify the change:

```xaml
<syncfusion:DocumentContainer Name="DocContainer" 
                              Mode="MDI" 
                              MDIWindowStateChanging="DocContainer_MDIWindowStateChanging">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Window1">
        <FlowDocument>
            <Paragraph>Window with state change handling</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Window2">
        <FlowDocument>
            <Paragraph>Another window</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

**C# Event Handler:**
```csharp
private void DocContainer_MDIWindowStateChanging(object sender, MDIWindowStateChangingEventArgs e)
{
    // Prevent maximizing windows
    if (e.NewState == MDIWindowState.Maximized)
    {
        e.Cancel = true;
        MessageBox.Show("Maximizing windows is not allowed in this application.");
        return;
    }
    
    // Log state changes
    string header = DocumentContainer.GetHeader(e.Item);
    Console.WriteLine($"Window '{header}' changing from {e.OldState} to {e.NewState}");
    
    // Warn before minimizing important windows
    if (e.NewState == MDIWindowState.Minimized && header == "Important Window")
    {
        var result = MessageBox.Show(
            "Are you sure you want to minimize this important window?",
            "Confirm Minimize",
            MessageBoxButton.YesNo);
            
        if (result == MessageBoxResult.No)
        {
            e.Cancel = true;
        }
    }
}
```

### Event Arguments

```csharp
public class MDIWindowStateChangingEventArgs : RoutedEventArgs
{
    public MDIWindowState OldState { get; }    // Current state
    public MDIWindowState NewState { get; }    // State being changed to
    public object Item { get; }                 // Window element
    public bool Cancel { get; set; }           // Set to true to prevent change
}
```

## Keyboard Window Management

### Keyboard Shortcuts

DocumentContainer supports keyboard-based window management:

- **CTRL+TAB:** Switch between windows (requires SwitchMode configuration)
- **Alt+F4:** Close active window (if enabled)
- **Alt+Space:** Open window system menu
- **Arrow keys:** Resize and move windows (when supported)

### Enabling Keyboard Resizing

```csharp
// Enable keyboard support for window operations
docContainer.IsAllowMDIResize = true;
docContainer.CanMDIMinimize = true;
docContainer.CanMDIMaximize = true;
```

## Common Patterns

### Pattern 1: Full Window Control

Enable all window management features for power users:

```xaml
<syncfusion:DocumentContainer Name="DocContainer"
                              Mode="MDI"
                              CanMDIMinimize="True"
                              CanMDIMaximize="True"
                              IsAllowMDIResize="True"
                              SwitchMode="VS2005">
    <!-- Windows -->
</syncfusion:DocumentContainer>
```

### Pattern 2: Fixed Layout

Prevent window manipulation for controlled layouts:

```xaml
<syncfusion:DocumentContainer Name="DocContainer"
                              Mode="MDI"
                              CanMDIMinimize="False"
                              CanMDIMaximize="False"
                              IsAllowMDIResize="False">
    <!-- Windows with predefined positions -->
</syncfusion:DocumentContainer>
```

### Pattern 3: Dashboard Layout

Arrange windows in a grid for dashboard applications:

```csharp
private void SetupDashboardLayout()
{
    // Container 600x400
    double halfWidth = 300;
    double halfHeight = 200;
    
    var window1 = docContainer.Items[0] as FrameworkElement;
    var window2 = docContainer.Items[1] as FrameworkElement;
    var window3 = docContainer.Items[2] as FrameworkElement;
    var window4 = docContainer.Items[3] as FrameworkElement;
    
    // Top-left
    DocumentContainer.SetMDIBounds(window1, new Rect(0, 0, halfWidth, halfHeight));
    // Top-right
    DocumentContainer.SetMDIBounds(window2, new Rect(halfWidth, 0, halfWidth, halfHeight));
    // Bottom-left
    DocumentContainer.SetMDIBounds(window3, new Rect(0, halfHeight, halfWidth, halfHeight));
    // Bottom-right
    DocumentContainer.SetMDIBounds(window4, new Rect(halfWidth, halfHeight, halfWidth, halfHeight));
}
```

### Pattern 4: Cascade New Windows

Automatically cascade new windows as they're added:

```csharp
private int windowCount = 0;
private const int CascadeOffset = 30;
private const int InitialX = 10;
private const int InitialY = 10;
private const int WindowWidth = 400;
private const int WindowHeight = 300;

private void AddNewWindow()
{
    // Create window
    var newWindow = new TextBox { AcceptsReturn = true, TextWrapping = TextWrapping.Wrap };
    DocumentContainer.SetHeader(newWindow, $"Document {windowCount + 1}");
    
    // Set cascaded position
    int x = InitialX + (windowCount * CascadeOffset);
    int y = InitialY + (windowCount * CascadeOffset);
    DocumentContainer.SetMDIBounds(newWindow, new Rect(x, y, WindowWidth, WindowHeight));
    
    // Add to container
    docContainer.Items.Add(newWindow);
    windowCount++;
}
```

## Best Practices

1. **Enable minimize for many windows:** If users work with 5+ windows, enable `CanMDIMinimize` to reduce clutter
2. **Use MDIBounds for initial layout:** Set meaningful initial positions rather than letting windows stack randomly
3. **Provide window arrangement commands:** Add toolbar buttons for Cascade, Tile Horizontal, Tile Vertical
4. **Handle state change events:** Validate state changes to prevent unwanted behavior
5. **Consider disabling maximize:** For applications where side-by-side viewing is essential
6. **Test with keyboard navigation:** Ensure power users can manage windows without a mouse
7. **Save window positions:** Use state persistence to remember user's window arrangements

## Troubleshooting

**Issue:** Windows overlap and are hard to manage
- **Solution:** Set initial MDIBounds for each window or provide arrangement commands

**Issue:** Users accidentally minimize important windows
- **Solution:** Use MDIWindowStateChanging event to prevent minimizing specific windows

**Issue:** Resizing doesn't work
- **Solution:** Ensure `IsAllowMDIResize="True"` is set

**Issue:** Window controls (min/max buttons) don't appear
- **Solution:** Check that `CanMDIMinimize` and `CanMDIMaximize` are set to `True`

## Related Documentation

- **Container Modes:** See `container-modes.md` for TDI vs MDI overview
- **Window Switchers:** See `window-switchers.md` for CTRL+TAB navigation
- **State Persistence:** See `state-persistence.md` to save window positions and states

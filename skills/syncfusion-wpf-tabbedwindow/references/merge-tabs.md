# Merge Tabs Between Windows in WPF Tabbed Window

## Table of Contents
- [Overview](#overview)
- [Tear-Off Windows](#tear-off-windows)
- [Floating Window Lifecycle](#floating-window-lifecycle)
- [Cross-Window Tab Merging](#cross-window-tab-merging)
- [PreviewTabMerge Event](#previewtabmerge-event)
- [Merge Validation](#merge-validation)
- [Item Transformation During Merge](#item-transformation-during-merge)
- [Multi-Window Scenarios](#multi-window-scenarios)
- [Common Patterns](#common-patterns)
- [Edge Cases and Gotchas](#edge-cases-and-gotchas)

## Overview

The Tabbed Window supports advanced multi-window scenarios through tear-off and tab merging functionality. Users can:

- **Tear off tabs** to create floating windows
- **Merge tabs** between windows via drag-drop
- **Validate merges** before they occur
- **Transform items** during the merge process
- **Organize workspaces** flexibly across multiple windows

This enables professional multi-monitor workflows similar to Visual Studio, Chrome, and other modern applications.

**Key Features:**
- Drag tabs outside window to create floating windows
- Drag tabs between windows to merge
- Event-driven merge validation
- Automatic window cleanup when empty
- Full control over merge operations

## Tear-Off Windows

Tear-off functionality allows users to detach tabs and create independent floating windows by dragging tabs outside the tab control boundary.

### Enabling Tear-Off

Tear-off requires `AllowDragDrop` to be enabled:

**XAML:**

```xml
<syncfusion:SfChromelessWindow 
    Title="Main Window" 
    WindowType="Tabbed"
    Height="600" 
    Width="900">
    
    <syncfusion:SfTabControl AllowDragDrop="True">
        <syncfusion:SfTabItem 
            Header="Document 1" 
            CloseButtonVisibility="Visible">
            <TextBlock Text="Drag this tab outside to tear it off" 
                      Margin="20"/>
        </syncfusion:SfTabItem>
        
        <syncfusion:SfTabItem 
            Header="Document 2" 
            CloseButtonVisibility="Visible">
            <TextBlock Text="Each tab can be independently floated" 
                      Margin="20"/>
        </syncfusion:SfTabItem>
    </syncfusion:SfTabControl>
    
</syncfusion:SfChromelessWindow>
```

**Code-Behind:**

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Enable drag-drop (required for tear-off)
    MainTabControl.AllowDragDrop = true;
}
```

### How Tear-Off Works

**User Actions:**

1. **Click and hold** on a tab header
2. **Drag the tab** outside the window boundary
3. **Release mouse** to create floating window

**System Behavior:**

1. Tab is removed from source window
2. New `SfChromelessWindow` is created automatically
3. New window contains the dragged tab
4. Floating window is positioned near drop point
5. Source window continues operating normally

### Tear-Off Example

```xml
<syncfusion:SfChromelessWindow 
    x:Class="MultiWindowApp.MainWindow"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    Title="Document Manager"
    WindowType="Tabbed"
    Height="600" 
    Width="900">
    
    <syncfusion:SfTabControl 
        x:Name="MainTabControl"
        AllowDragDrop="True"
        EnableNewTabButton="True"
        NewTabRequested="OnNewTabRequested">
        
        <syncfusion:SfTabItem Header="Home" CloseButtonVisibility="Visible">
            <TextBlock Text="Main window - drag me out!" Margin="20"/>
        </syncfusion:SfTabItem>
        
        <syncfusion:SfTabItem Header="Document 1" CloseButtonVisibility="Visible">
            <TextBox AcceptsReturn="True" Text="Document content" Margin="10"/>
        </syncfusion:SfTabItem>
        
        <syncfusion:SfTabItem Header="Document 2" CloseButtonVisibility="Visible">
            <TextBox AcceptsReturn="True" Text="More content" Margin="10"/>
        </syncfusion:SfTabItem>
    </syncfusion:SfTabControl>
    
</syncfusion:SfChromelessWindow>
```

### Visual Feedback During Tear-Off

While dragging, the system provides visual cues:

- **Drag cursor** changes to indicate tear-off is possible
- **Tab follows cursor** outside window bounds
- **Drop preview** shows where tab will be placed
- **Window outline** appears at drop location

No additional code required - this is built-in behavior.

## Floating Window Lifecycle

### Creation

When a tab is torn off:

**Automatic Creation:**

```csharp
// Syncfusion handles this automatically
// A new SfChromelessWindow is created with:
// - WindowType matching source window
// - Similar size and styling
// - The torn-off tab as content
// - AllowDragDrop enabled
```

**Properties Inherited:**

The floating window inherits certain properties from the source:
- `WindowType` (Tabbed or Normal)
- Theme and styling
- Tab control configuration (close buttons, drag-drop)

### Positioning

Floating windows are positioned:
- Near the drop point (cursor location)
- Adjusted to stay within screen bounds
- With reasonable default size
- Can be moved/resized normally

### Reattaching Floating Tabs

To reattach a tab from a floating window back to the main window:

**User Actions:**

1. **Click and drag** tab from floating window
2. **Drag into** main window's tab area
3. **Release mouse** over tab strip

**System Behavior:**

1. Tab is removed from floating window
2. Tab is added to target window at drop position
3. Floating window closes if empty (no tabs remain)
4. Tab becomes active in target window

**Example:**

```
┌─────────────────────┐
│ Main Window         │
│ [Tab1] [Tab2] [Tab3]│ ← Drop here
└─────────────────────┘

┌──────────────┐
│ Floating     │
│ [Document] ×─┤ ← Drag from here
└──────────────┘
```

### Empty Window Cleanup

When the last tab is removed from a floating window:

**Automatic Behavior:**
- Floating window automatically closes
- No empty windows remain
- Focus returns to previous window

**Preventing Auto-Close:**

To keep a window open even when empty:

```csharp
// In floating window
private void OnTabRemoving(object sender, EventArgs e)
{
    if (MainTabControl.Items.Count == 1)
    {
        // Add a placeholder tab
        var placeholder = new SfTabItem
        {
            Header = "Empty",
            Content = new TextBlock { Text = "No documents open" }
        };
        MainTabControl.Items.Add(placeholder);
    }
}
```

## Cross-Window Tab Merging

Merge tabs between multiple windows through drag-and-drop operations.

### Basic Merge Setup

**Scenario:** Two tabbed windows where tabs can be dragged between them.

**Window 1:**

```xml
<syncfusion:SfChromelessWindow 
    x:Class="App.Window1"
    Title="Window 1" 
    WindowType="Tabbed">
    
    <syncfusion:SfTabControl 
        AllowDragDrop="True"
        PreviewTabMerge="OnPreviewTabMerge">
        <syncfusion:SfTabItem Header="Doc A" Content="Content A"/>
        <syncfusion:SfTabItem Header="Doc B" Content="Content B"/>
    </syncfusion:SfTabControl>
    
</syncfusion:SfChromelessWindow>
```

**Window 2:**

```xml
<syncfusion:SfChromelessWindow 
    x:Class="App.Window2"
    Title="Window 2" 
    WindowType="Tabbed">
    
    <syncfusion:SfTabControl 
        AllowDragDrop="True"
        PreviewTabMerge="OnPreviewTabMerge">
        <syncfusion:SfTabItem Header="Doc X" Content="Content X"/>
        <syncfusion:SfTabItem Header="Doc Y" Content="Content Y"/>
    </syncfusion:SfTabControl>
    
</syncfusion:SfChromelessWindow>
```

**Usage:**
1. Drag "Doc A" from Window 1
2. Drop onto Window 2's tab strip
3. "Doc A" now appears in Window 2
4. Window 1 continues with "Doc B"

### Merge Between Application Windows

**Create multiple windows programmatically:**

```csharp
// In main window
private void OpenNewWindow_Click(object sender, RoutedEventArgs e)
{
    var newWindow = new MainWindow
    {
        Title = $"Window {_windowCounter++}",
        WindowType = WindowType.Tabbed,
        Height = 600,
        Width = 900
    };
    
    newWindow.Show();
    
    // Track windows
    _openWindows.Add(newWindow);
}

private List<MainWindow> _openWindows = new List<MainWindow>();
private int _windowCounter = 2;
```

Now tabs can be dragged between any of these windows.

### Merge Behavior

**Default Behavior:**

- Tab is **removed** from source window
- Tab is **added** to target window at drop position
- Tab becomes **selected** in target window
- Source window adjusts remaining tabs
- **No confirmation** is required by default

## PreviewTabMerge Event

The `PreviewTabMerge` event fires **before** a tab is moved between windows, allowing validation and customization.

### Event Basics

**XAML:**

```xml
<syncfusion:SfTabControl 
    AllowDragDrop="True"
    PreviewTabMerge="OnPreviewTabMerge">
    <!-- Tabs -->
</syncfusion:SfTabControl>
```

**Code-Behind:**

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    // Access merge information
    var draggedItem = e.DraggedItem;
    var sourceControl = e.SourceControl;
    var targetControl = e.TargetControl;
    
    // Allow or deny the merge
    e.Allow = true; // or false
    
    // Optionally transform the item
    e.ResultingItem = draggedItem; // or new item
}
```

### TabMergePreviewEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `DraggedItem` | object | The tab item being dragged |
| `SourceControl` | SfTabControl | Tab control where drag originated |
| `TargetControl` | SfTabControl | Tab control receiving the item |
| `Allow` | bool | Set to `false` to cancel merge (default: `true`) |
| `ResultingItem` | object | Item to be inserted (default: `DraggedItem`) |

### Event Execution Flow

```
1. User drags tab to another window
2. Mouse hovers over target window's tab area
3. PreviewTabMerge event fires
4. Your event handler executes
5. If e.Allow == true:
   - Item removed from source
   - ResultingItem added to target
6. If e.Allow == false:
   - Merge cancelled
   - Item stays in source
```

## Merge Validation

Control which tabs can be merged and between which windows.

### Deny All Merges

Prevent any cross-window merging:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    e.Allow = false;
    MessageBox.Show("Tab merging is disabled");
}
```

### Validate Based on Tab Content

Allow only certain tab types to merge:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    if (e.DraggedItem is SfTabItem tabItem)
    {
        // Check if tab contains a document that can be moved
        if (tabItem.Tag is Document doc)
        {
            if (doc.IsLocked)
            {
                e.Allow = false;
                MessageBox.Show("Cannot move locked documents");
                return;
            }
            
            if (doc.IsReadOnly && !IsEditorWindow(e.TargetControl))
            {
                e.Allow = false;
                MessageBox.Show("Read-only documents can only go to editor windows");
                return;
            }
        }
    }
    
    e.Allow = true;
}

private bool IsEditorWindow(SfTabControl control)
{
    // Check if target window is an editor
    var window = Window.GetWindow(control);
    return window is EditorWindow;
}
```

### Validate Based on Window Type

Restrict merging between certain window types:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    var sourceWindow = Window.GetWindow(e.SourceControl);
    var targetWindow = Window.GetWindow(e.TargetControl);
    
    // Don't allow moving from primary to secondary windows
    if (sourceWindow is MainWindow && targetWindow is SecondaryWindow)
    {
        e.Allow = false;
        MessageBox.Show("Cannot move tabs from main to secondary windows");
        return;
    }
    
    // Allow same-type window merges
    if (sourceWindow.GetType() == targetWindow.GetType())
    {
        e.Allow = true;
        return;
    }
    
    // Deny other combinations
    e.Allow = false;
}
```

### Confirm Before Merge

Prompt user for confirmation:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    if (e.DraggedItem is SfTabItem tabItem)
    {
        var sourceWindow = Window.GetWindow(e.SourceControl);
        var targetWindow = Window.GetWindow(e.TargetControl);
        
        var result = MessageBox.Show(
            $"Move '{tabItem.Header}' from '{sourceWindow.Title}' to '{targetWindow.Title}'?",
            "Confirm Move",
            MessageBoxButton.YesNo);
        
        e.Allow = (result == MessageBoxResult.Yes);
    }
}
```

### Validate Maximum Tabs

Limit number of tabs in target window:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    const int MaxTabsPerWindow = 10;
    
    if (e.TargetControl.Items.Count >= MaxTabsPerWindow)
    {
        e.Allow = false;
        MessageBox.Show($"Target window already has maximum of {MaxTabsPerWindow} tabs");
        return;
    }
    
    e.Allow = true;
}
```

## Item Transformation During Merge

Transform or clone items during the merge operation using `ResultingItem`.

### Clone Item During Merge

Keep original in source, add copy to target:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    if (e.DraggedItem is SfTabItem originalTab)
    {
        // Create a copy instead of moving
        var clonedTab = new SfTabItem
        {
            Header = originalTab.Header + " (Copy)",
            Content = CloneContent(originalTab.Content),
            CloseButtonVisibility = originalTab.CloseButtonVisibility,
            Tag = CloneTag(originalTab.Tag)
        };
        
        e.ResultingItem = clonedTab;
        e.Allow = true;
        
        // Original stays in source because we provided a different ResultingItem
    }
}

private UIElement CloneContent(object content)
{
    // Implement content cloning logic
    if (content is TextBox textBox)
    {
        return new TextBox
        {
            Text = textBox.Text,
            AcceptsReturn = textBox.AcceptsReturn,
            FontFamily = textBox.FontFamily
        };
    }
    
    return new TextBlock { Text = content?.ToString() };
}
```

### Update Item Metadata

Modify item properties during merge:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    if (e.DraggedItem is SfTabItem tabItem && tabItem.Tag is Document doc)
    {
        // Update document metadata
        doc.LastMovedTime = DateTime.Now;
        doc.TargetWindow = Window.GetWindow(e.TargetControl).Title;
        
        // Create new tab with updated document
        var newTab = new SfTabItem
        {
            Header = tabItem.Header,
            Content = tabItem.Content,
            Tag = doc,
            CloseButtonVisibility = Visibility.Visible
        };
        
        e.ResultingItem = newTab;
        e.Allow = true;
    }
}
```

### Wrap Content in New Container

Add additional UI during merge:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    if (e.DraggedItem is SfTabItem originalTab)
    {
        // Wrap content in a border with label
        var wrapper = new Grid();
        wrapper.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
        wrapper.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star) });
        
        var label = new TextBlock
        {
            Text = $"Moved from: {Window.GetWindow(e.SourceControl).Title}",
            Background = Brushes.LightYellow,
            Padding = new Thickness(5)
        };
        Grid.SetRow(label, 0);
        wrapper.Children.Add(label);
        
        var content = originalTab.Content as UIElement;
        Grid.SetRow(content, 1);
        wrapper.Children.Add(content);
        
        var newTab = new SfTabItem
        {
            Header = originalTab.Header,
            Content = wrapper,
            CloseButtonVisibility = Visibility.Visible
        };
        
        e.ResultingItem = newTab;
        e.Allow = true;
    }
}
```

## Multi-Window Scenarios

### Scenario 1: Primary and Secondary Windows

```csharp
public class WindowManager
{
    private MainWindow _primaryWindow;
    private List<SecondaryWindow> _secondaryWindows = new List<SecondaryWindow>();
    
    public void CreateSecondaryWindow()
    {
        var secondary = new SecondaryWindow
        {
            Title = $"Window {_secondaryWindows.Count + 2}",
            Owner = _primaryWindow
        };
        
        secondary.Closed += (s, e) => _secondaryWindows.Remove(secondary);
        secondary.Show();
        
        _secondaryWindows.Add(secondary);
    }
    
    public void CloseAllSecondary()
    {
        foreach (var window in _secondaryWindows.ToList())
        {
            window.Close();
        }
    }
}
```

### Scenario 2: Multi-Monitor Workspace

```csharp
public void ArrangeWindowsAcrossMonitors()
{
    var screens = System.Windows.Forms.Screen.AllScreens;
    
    for (int i = 0; i < screens.Length && i < _openWindows.Count; i++)
    {
        var screen = screens[i];
        var window = _openWindows[i];
        
        window.Left = screen.WorkingArea.Left;
        window.Top = screen.WorkingArea.Top;
        window.Width = screen.WorkingArea.Width;
        window.Height = screen.WorkingArea.Height;
        window.WindowState = WindowState.Maximized;
    }
}
```

### Scenario 3: Context-Specific Windows

```csharp
// Editor window - only accepts document tabs
public class EditorWindow : SfChromelessWindow
{
    protected override void OnPreviewTabMerge(TabMergePreviewEventArgs e)
    {
        if (e.DraggedItem is SfTabItem tab && tab.Tag is Document doc)
        {
            e.Allow = doc.Type == DocumentType.Editable;
        }
        else
        {
            e.Allow = false;
        }
        
        base.OnPreviewTabMerge(e);
    }
}

// Viewer window - only accepts read-only tabs
public class ViewerWindow : SfChromelessWindow
{
    protected override void OnPreviewTabMerge(TabMergePreviewEventArgs e)
    {
        if (e.DraggedItem is SfTabItem tab && tab.Tag is Document doc)
        {
            e.Allow = doc.IsReadOnly;
        }
        else
        {
            e.Allow = false;
        }
        
        base.OnPreviewTabMerge(e);
    }
}
```

## Common Patterns

### Pattern 1: Unrestricted Multi-Window

```xml
<!-- Any window -->
<syncfusion:SfChromelessWindow WindowType="Tabbed">
    <syncfusion:SfTabControl AllowDragDrop="True">
        <!-- Tabs can move freely between windows -->
    </syncfusion:SfTabControl>
</syncfusion:SfChromelessWindow>
```

### Pattern 2: Validated Merging

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    // Get document from tab
    var doc = (e.DraggedItem as SfTabItem)?.Tag as Document;
    
    // Validation rules
    if (doc == null)
    {
        e.Allow = false;
        return;
    }
    
    if (doc.IsLocked)
    {
        e.Allow = false;
        ShowNotification("Document is locked");
        return;
    }
    
    if (e.TargetControl.Items.Count >= 10)
    {
        e.Allow = false;
        ShowNotification("Target window is full");
        return;
    }
    
    e.Allow = true;
}
```

### Pattern 3: Logging Merges

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    var tabItem = e.DraggedItem as SfTabItem;
    var sourceWindow = Window.GetWindow(e.SourceControl);
    var targetWindow = Window.GetWindow(e.TargetControl);
    
    // Log the merge
    Logger.LogInfo($"Tab '{tabItem?.Header}' moved from '{sourceWindow.Title}' to '{targetWindow.Title}'");
    
    // Allow merge
    e.Allow = true;
}
```

## Edge Cases and Gotchas

### Last Tab in Window

**Issue:** Tearing off or merging the last tab might close the window

**Solution:** Keep at least one tab or handle window closure:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    // Don't allow moving last tab from main window
    if (e.SourceControl.Items.Count == 1 && 
        Window.GetWindow(e.SourceControl) is MainWindow)
    {
        e.Allow = false;
        MessageBox.Show("Cannot remove last tab from main window");
        return;
    }
    
    e.Allow = true;
}
```

### Data Binding with Merges

**Issue:** Merged tabs might lose data binding context

**Solution:** Ensure DataContext is preserved:

```csharp
private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
{
    if (e.DraggedItem is SfTabItem tabItem)
    {
        // Preserve DataContext
        var dataContext = tabItem.DataContext;
        
        var newTab = new SfTabItem
        {
            Header = tabItem.Header,
            Content = tabItem.Content,
            DataContext = dataContext // Restore context
        };
        
        e.ResultingItem = newTab;
        e.Allow = true;
    }
}
```

### Event Handler Conflicts

**Issue:** Multiple windows with same event handler might conflict

**Solution:** Use instance-specific handlers:

```csharp
public class DocumentWindow : SfChromelessWindow
{
    private Guid _windowId = Guid.NewGuid();
    
    private void OnPreviewTabMerge(object sender, TabMergePreviewEventArgs e)
    {
        // Window-specific logic using _windowId
        LogMerge(_windowId, e);
        
        e.Allow = ValidateMerge(_windowId, e);
    }
}
```

### Circular References

**Issue:** Complex content might have circular references during clone

**Solution:** Implement deep clone carefully:

```csharp
private SfTabItem CloneTab(SfTabItem original)
{
    return new SfTabItem
    {
        Header = original.Header,
        // Create new content instance, don't reference original
        Content = CreateNewContentInstance(original.Content),
        Tag = CloneTag(original.Tag) // Clone tag data
    };
}
```

This comprehensive guide covers all aspects of tab merging and multi-window functionality in the Syncfusion WPF Tabbed Window!

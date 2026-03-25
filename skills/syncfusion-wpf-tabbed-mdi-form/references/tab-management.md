# Tab Management in TDI Mode

## Table of Contents
- [Overview](#overview)
- [Adding and Removing Items](#adding-and-removing-items)
- [Creating Tab Groups](#creating-tab-groups)
- [Pin and Unpin Tabs](#pin-and-unpin-tabs)
- [Tab Reordering](#tab-reordering)
- [Tab Drag and Drop](#tab-drag-and-drop)
- [Tab Navigation](#tab-navigation)
- [Dynamic Tab Management](#dynamic-tab-management)

## Overview

In TDI (Tabbed Document Interface) mode, the DocumentContainer displays documents as tabs in a horizontal tab strip. This guide covers all tab management features including adding/removing tabs, creating tab groups for split views, pinning important tabs, and controlling tab behavior.

**Note:** Tab management features are **only available in TDI mode**. MDI mode uses windows instead of tabs.

## Adding and Removing Items

### Adding Items in XAML

Add framework elements directly as children of the DocumentContainer:

```xaml
<syncfusion:DocumentContainer x:Name="documentContainer" Mode="TDI">
    <!-- Tab 1: Button -->
    <Button syncfusion:DocumentContainer.Header="Button Tab"
            Content="Button Content"/>
    
    <!-- Tab 2: TextBox -->
    <TextBox syncfusion:DocumentContainer.Header="Editor"
             AcceptsReturn="True"
             TextWrapping="Wrap"/>
    
    <!-- Tab 3: Grid with content -->
    <Grid syncfusion:DocumentContainer.Header="Dashboard">
        <TextBlock Text="Dashboard Content" 
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"/>
    </Grid>
</syncfusion:DocumentContainer>
```

### Adding Items Programmatically

Use the `Items.Add()` method to add tabs dynamically:

```csharp
// Create a new tab element
Button button1 = new Button { Content = "Button Content" };

// Set the tab header
DocumentContainer.SetHeader(button1, "Button Tab");

// Add to container
documentContainer.Items.Add(button1);

// Add multiple items
TextBox editor = new TextBox { AcceptsReturn = true, TextWrapping = TextWrapping.Wrap };
DocumentContainer.SetHeader(editor, "Editor");
documentContainer.Items.Add(editor);

Grid dashboard = new Grid();
dashboard.Children.Add(new TextBlock 
{ 
    Text = "Dashboard",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
});
DocumentContainer.SetHeader(dashboard, "Dashboard");
documentContainer.Items.Add(dashboard);
```

### Removing Items

Remove all tabs or specific tabs using the Items collection:

**Remove All Items:**
```csharp
// Clear all tabs
documentContainer.Items.Clear();
```

**Remove Specific Item:**
```csharp
// Remove by reference
Button buttonToRemove = documentContainer.Items[0] as Button;
documentContainer.Items.Remove(buttonToRemove);

// Remove by index
documentContainer.Items.RemoveAt(2);
```

**Remove with Confirmation:**
```csharp
private void RemoveTab(object tabElement)
{
    if (tabElement == null) return;
    
    string header = DocumentContainer.GetHeader(tabElement);
    
    var result = MessageBox.Show(
        $"Are you sure you want to close '{header}'?",
        "Confirm Close",
        MessageBoxButton.YesNo,
        MessageBoxImage.Question);
    
    if (result == MessageBoxResult.Yes)
    {
        documentContainer.Items.Remove(tabElement);
    }
}
```

## Creating Tab Groups

Tab groups allow you to split the DocumentContainer view to display multiple tabs simultaneously (side-by-side or top-and-bottom).

### Creating Horizontal Tab Groups

**Using Context Menu:**
1. Right-click on any tab
2. Select "New Horizontal Tab Group"
3. The tab moves to a new horizontal group

**Programmatically:**
```csharp
// Create horizontal tab group for a specific tab
documentContainer.CreateHorizontalTabGroup(item1 as UIElement);
```

**Complete Example:**
```xaml
<syncfusion:DocumentContainer Mode="TDI" 
                              Loaded="DocumentContainer_Loaded"
                              x:Name="documentContainer">
    <ContentControl syncfusion:DocumentContainer.Header="item1"
                    x:Name="item1" />
    <ContentControl syncfusion:DocumentContainer.Header="item2"
                    x:Name="item2" />
    <ContentControl syncfusion:DocumentContainer.Header="item3"
                    x:Name="item3" />
</syncfusion:DocumentContainer>
```

```csharp
private void DocumentContainer_Loaded(object sender, RoutedEventArgs e)
{
    // Create horizontal split with item1 in new group
    documentContainer.CreateHorizontalTabGroup(item1 as UIElement);
}
```

### Creating Vertical Tab Groups

**Using Context Menu:**
1. Right-click on any tab
2. Select "New Vertical Tab Group"
3. The tab moves to a new vertical group

**Programmatically:**
```csharp
// Create vertical tab group for a specific tab
documentContainer.CreateVerticalTabGroup(item2 as UIElement);
```

### Creating Tab Groups by Dragging

1. Click and hold a tab
2. Drag it into the document area (not the tab strip)
3. A context menu appears with options:
   - **New Tab Group:** Creates a horizontal tab group
   - **Cancel:** Cancels the operation
4. Select "New Tab Group" to create the split

### Tab Group Layout Examples

**Two-Pane Layout (Horizontal):**
```csharp
private void SetupTwoPaneHorizontal()
{
    // Assumes documentContainer has at least 2 items
    var item1 = documentContainer.Items[0] as UIElement;
    documentContainer.CreateHorizontalTabGroup(item1);
}
```

**Two-Pane Layout (Vertical):**
```csharp
private void SetupTwoPaneVertical()
{
    // Assumes documentContainer has at least 2 items
    var item1 = documentContainer.Items[0] as UIElement;
    documentContainer.CreateVerticalTabGroup(item1);
}
```

**Three-Pane Layout:**
```csharp
private void SetupThreePaneLayout()
{
    if (documentContainer.Items.Count < 3) return;
    
    // Create horizontal split
    var item1 = documentContainer.Items[0] as UIElement;
    documentContainer.CreateHorizontalTabGroup(item1);
    
    // Create vertical split in bottom group
    var item2 = documentContainer.Items[1] as UIElement;
    documentContainer.CreateVerticalTabGroup(item2);
}
```

## Pin and Unpin Tabs

Pinning tabs keeps important documents at the front of the tab strip, separated from unpinned tabs.

### Enabling Pin Feature

Use the `AllowPin` attached property to enable pinning for specific tabs:

```xaml
<syncfusion:DocumentContainer Name="DocContainer" Mode="TDI">
    <!-- Pinnable tab -->
    <FlowDocumentScrollViewer 
        syncfusion:DocumentContainer.AllowPin="True"
        syncfusion:DocumentContainer.Header="Important Document">
        <FlowDocument>
            <Paragraph>This tab can be pinned</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <!-- Another pinnable tab -->
    <FlowDocumentScrollViewer 
        syncfusion:DocumentContainer.AllowPin="True"
        syncfusion:DocumentContainer.Header="Features">
        <FlowDocument>
            <Paragraph>Another pinnable tab</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <!-- Non-pinnable tab -->
    <FlowDocumentScrollViewer 
        syncfusion:DocumentContainer.AllowPin="False"
        syncfusion:DocumentContainer.Header="Temporary">
        <FlowDocument>
            <Paragraph>This tab cannot be pinned</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

### Showing Pin Buttons

Enable the `ShowPin` property to display pin buttons on tabs:

```xaml
<syncfusion:DocumentContainer Mode="TDI">
    <!-- Pin button visible and enabled -->
    <FlowDocumentScrollViewer 
        syncfusion:DocumentContainer.AllowPin="True"
        syncfusion:DocumentContainer.ShowPin="True"
        syncfusion:DocumentContainer.Header="Document 1">
        <FlowDocument>
            <Paragraph>Pin button visible and enabled</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <!-- Pin button visible but disabled -->
    <FlowDocumentScrollViewer 
        syncfusion:DocumentContainer.AllowPin="False"
        syncfusion:DocumentContainer.ShowPin="True"
        syncfusion:DocumentContainer.Header="Document 2">
        <FlowDocument>
            <Paragraph>Pin button visible but disabled</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

**Pin Button States:**
- `AllowPin="True"` + `ShowPin="True"` → Pin button visible and enabled
- `AllowPin="False"` + `ShowPin="True"` → Pin button visible but disabled
- `ShowPin="False"` → Pin button hidden (regardless of AllowPin)

### Pinning and Unpinning Behavior

**When a tab is pinned:**
- It moves to the beginning of the tab strip
- It stays with other pinned tabs (separated by a divider)
- It maintains its position relative to other pinned tabs

**When a tab is unpinned:**
- It moves out of the pinned section
- It appears as the first unpinned tab
- The divider adjusts automatically

### Pinning Tabs Programmatically

Use the `IsPinned` attached property:

```csharp
// Pin a tab
var document1 = documentContainer.Items[0] as FrameworkElement;
DocumentContainer.SetIsPinned(document1, true);

// Unpin a tab
DocumentContainer.SetIsPinned(document1, false);

// Check if a tab is pinned
bool isPinned = DocumentContainer.GetIsPinned(document1);
if (isPinned)
{
    Console.WriteLine("Tab is pinned");
}
```

### Pinning via Context Menu

When `AllowPin="True"`, right-clicking a tab shows:
- **"Pin Tab"** if the tab is not currently pinned
- **"Unpin Tab"** if the tab is currently pinned

### Practical Pin Scenarios

**Auto-pin important documents:**
```csharp
private void AddImportantDocument(UIElement content, string header)
{
    DocumentContainer.SetHeader(content, header);
    DocumentContainer.SetAllowPin(content, true);
    DocumentContainer.SetShowPin(content, true);
    DocumentContainer.SetIsPinned(content, true); // Auto-pin
    
    documentContainer.Items.Add(content);
}
```

**Pin on first view:**
```csharp
private void PinOnFirstView(UIElement tab)
{
    // Check if it's the user's first time viewing this tab
    if (IsFirstView(tab))
    {
        DocumentContainer.SetIsPinned(tab, true);
        MessageBox.Show("This important tab has been pinned for easy access.");
    }
}
```

## Tab Reordering

Users can reorder tabs by dragging them along the tab strip.

### Enabling/Disabling Drag and Drop

Control tab reordering with the `IsTDIDragDropEnabled` property:

**Enable reordering (default):**
```xaml
<syncfusion:DocumentContainer Name="documentContainer" 
                              Mode="TDI"
                              IsTDIDragDropEnabled="True">
    <ContentControl syncfusion:DocumentContainer.Header="Tab1"/>
    <ContentControl syncfusion:DocumentContainer.Header="Tab2"/>
    <ContentControl syncfusion:DocumentContainer.Header="Tab3"/>
</syncfusion:DocumentContainer>
```

**Disable reordering:**
```xaml
<syncfusion:DocumentContainer Name="documentContainer" 
                              Mode="TDI"
                              IsTDIDragDropEnabled="False">
    <!-- Tabs cannot be reordered by dragging -->
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
// Enable drag and drop
documentContainer.IsTDIDragDropEnabled = true;

// Disable drag and drop
documentContainer.IsTDIDragDropEnabled = false;
```

### Auto-Scroll During Drag

Enable auto-scrolling when dragging tabs in overflow scenarios:

```xaml
<syncfusion:DocumentContainer x:Name="documentContainer" 
                              EnableAutoScroll="True" 
                              Mode="TDI">
    <ContentControl x:Name="Content1" syncfusion:DocumentContainer.Header="Document1"/>
    <ContentControl x:Name="Content2" syncfusion:DocumentContainer.Header="Document2"/>
    <ContentControl x:Name="Content3" syncfusion:DocumentContainer.Header="Document3"/>
    <ContentControl x:Name="Content4" syncfusion:DocumentContainer.Header="Document4"/>
    <ContentControl x:Name="Content5" syncfusion:DocumentContainer.Header="Document5"/>
    <ContentControl x:Name="Content6" syncfusion:DocumentContainer.Header="Document6"/>
    <ContentControl x:Name="Content7" syncfusion:DocumentContainer.Header="Document7"/>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
documentContainer.EnableAutoScroll = true;
```

With auto-scroll enabled, dragging a tab over the overflow button or scroll arrows automatically scrolls the tab strip.

### Tab Order Changed Event

The `DocumentTabOrderChanged` event fires when tab order changes:

```xaml
<syncfusion:DocumentContainer DocumentTabOrderChanged="DocumentContainer_DocumentTabOrderChanged"
                              Name="documentContainer"
                              Mode="TDI">
    <Grid syncfusion:DocumentContainer.Header="Tab1"/>
    <Grid syncfusion:DocumentContainer.Header="Tab2"/>
    <Grid syncfusion:DocumentContainer.Header="Tab3"/>
</syncfusion:DocumentContainer>
```

**Event Handler:**
```csharp
private void DocumentContainer_DocumentTabOrderChanged(
    object sender, 
    Syncfusion.Windows.Tools.Controls.DocumentTabOrderChangedEventArgs e)
{
    // Get the moved tab item
    var movedItem = e.SourceTabItem;
    
    // Get old and new positions
    int oldIndex = e.OldIndex;
    int newIndex = e.NewIndex;
    
    // Get old and new tab groups
    var sourceGroup = e.SourceTabGroup;
    var targetGroup = e.TargetTabGroup;
    
    // Log the change
    string header = DocumentContainer.GetHeader(movedItem);
    Console.WriteLine($"Tab '{header}' moved from index {oldIndex} to {newIndex}");
    
    // Save the new order
    SaveTabOrder();
}
```

### Restricting Tab Reordering

Use the `DocumentTabOrderChanging` event to prevent reordering:

```xaml
<syncfusion:DocumentContainer DocumentTabOrderChanging="DocumentContainer_DocumentTabOrderChanging"
                              Name="documentContainer"
                              Mode="TDI">
    <Grid syncfusion:DocumentContainer.Header="Tab1"/>
    <Grid syncfusion:DocumentContainer.Header="Tab2"/>
    <Grid syncfusion:DocumentContainer.Header="Tab3"/>
</syncfusion:DocumentContainer>
```

**Event Handler:**
```csharp
private void DocumentContainer_DocumentTabOrderChanging(
    object sender, 
    Syncfusion.Windows.Tools.Controls.DocumentTabOrderChangingEventArgs e)
{
    // Prevent all reordering
    e.Cancel = true;
    
    // Or conditionally prevent reordering
    string header = DocumentContainer.GetHeader(e.SourceTabItem);
    if (header == "Important Tab")
    {
        e.Cancel = true;
        MessageBox.Show("This tab cannot be moved.");
    }
}
```

## Tab Drag and Drop

### Drag to Create Tab Groups

Dragging a tab into the document area (not the tab strip) triggers tab group creation:

1. Click and hold a tab
2. Drag into the document content area
3. Release to see context menu
4. Select "New Tab Group" or "Cancel"

### Disabling Drag and Drop

```csharp
// Disable all drag and drop operations
documentContainer.IsTDIDragDropEnabled = false;
```

## Tab Navigation

### Keyboard Navigation

- **CTRL+TAB:** Next tab (requires window switcher configuration)
- **CTRL+SHIFT+TAB:** Previous tab
- **CTRL+W:** Close active tab (if enabled)
- **CTRL+TAB (hold):** Open window switcher

### Programmatic Navigation

```csharp
// Select a specific tab by index
documentContainer.SelectedIndex = 2;

// Select a specific tab by element
var tab = documentContainer.Items[1];
documentContainer.SelectedItem = tab;

// Get currently selected tab
var currentTab = documentContainer.SelectedItem;
string header = DocumentContainer.GetHeader(currentTab);
Console.WriteLine($"Current tab: {header}");
```

## Dynamic Tab Management

### Adding Tabs with Close Buttons

```csharp
private void AddClosableTab(string header, UIElement content)
{
    // Set header
    DocumentContainer.SetHeader(content, header);
    
    // Enable context menu with close option
    DocumentContainer.SetAllowPin(content, true);
    
    // Add to container
    documentContainer.Items.Add(content);
    
    // Select the new tab
    documentContainer.SelectedItem = content;
}
```

### Closing Tabs with Validation

```csharp
private void CloseTab(UIElement tab)
{
    string header = DocumentContainer.GetHeader(tab);
    
    // Check if tab has unsaved changes
    if (HasUnsavedChanges(tab))
    {
        var result = MessageBox.Show(
            $"'{header}' has unsaved changes. Close anyway?",
            "Unsaved Changes",
            MessageBoxButton.YesNoCancel,
            MessageBoxImage.Warning);
        
        if (result != MessageBoxResult.Yes)
            return;
    }
    
    // Remove the tab
    documentContainer.Items.Remove(tab);
}
```

### Tab Life Cycle Management

```csharp
private void ManageTabLifeCycle()
{
    // Limit maximum tabs
    const int MaxTabs = 10;
    
    if (documentContainer.Items.Count >= MaxTabs)
    {
        // Close the least recently used tab
        var oldestTab = FindLeastRecentlyUsedTab();
        documentContainer.Items.Remove(oldestTab);
    }
    
    // Add new tab
    AddNewTab();
}

private UIElement FindLeastRecentlyUsedTab()
{
    // Track tab access times and return the oldest
    // Implementation depends on your tracking mechanism
    return documentContainer.Items[0] as UIElement;
}
```

## Best Practices

1. **Enable pinning for frequently used tabs:** Use `AllowPin` for documents users access often
2. **Show pin buttons visually:** Set `ShowPin="True"` so users know pinning is available
3. **Use tab groups for comparisons:** Create horizontal or vertical splits when users need side-by-side views
4. **Enable auto-scroll for many tabs:** Set `EnableAutoScroll="True"` for better drag experience
5. **Handle tab order events:** Track changes for state persistence
6. **Validate before closing:** Check for unsaved changes before removing tabs
7. **Limit tab count:** Prevent performance issues by limiting maximum tabs
8. **Provide clear headers:** Use descriptive tab titles for easy identification

## Troubleshooting

**Issue:** Tabs cannot be reordered by dragging
- **Solution:** Ensure `IsTDIDragDropEnabled="True"`

**Issue:** Pin button doesn't appear
- **Solution:** Set both `AllowPin="True"` and `ShowPin="True"`

**Issue:** Tab groups not working
- **Solution:** Ensure container is in TDI mode (`Mode="TDI"`)

**Issue:** Too many tabs cause overflow issues
- **Solution:** Enable `EnableAutoScroll="True"` or limit tab count

## Related Documentation

- **Container Modes:** See `container-modes.md` for TDI vs MDI overview
- **Window Switchers:** See `window-switchers.md` for CTRL+TAB navigation
- **State Persistence:** See `state-persistence.md` to save tab arrangements

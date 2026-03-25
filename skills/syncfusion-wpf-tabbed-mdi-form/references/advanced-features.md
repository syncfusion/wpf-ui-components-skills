# Advanced Features

This guide covers advanced DocumentContainer features including full screen mode, drag and drop configuration, MDI window sizing, and integration with other Syncfusion controls.

## Full Screen Mode in TDI

The `TDIFullScreenMode` property enables full screen display for TDI documents, with the tab header appearing only on mouse hover.

### Full Screen Modes

**Available modes:**
- **None** (default): Normal display, no full screen behavior
- **ControlMode**: Tab header visible only on mouseover
- **WindowMode**: Parent window goes full screen, tab header visible on mouseover

### Setting Full Screen Mode

```xaml
<!-- WindowMode: Full screen with tab header on hover -->
<syncfusion:DocumentContainer Name="documentContainer" 
                              Mode="TDI" 
                              TDIFullScreenMode="WindowMode">
    <TextBox syncfusion:DocumentContainer.Header="Document 1"
             AcceptsReturn="True"
             TextWrapping="Wrap"/>
    <TextBox syncfusion:DocumentContainer.Header="Document 2"
             AcceptsReturn="True"
             TextWrapping="Wrap"/>
</syncfusion:DocumentContainer>

<!-- ControlMode: Tab header on hover, window stays normal size -->
<syncfusion:DocumentContainer Name="documentContainer" 
                              Mode="TDI" 
                              TDIFullScreenMode="ControlMode">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
// Enable full screen mode
documentContainer.TDIFullScreenMode = FullScreenMode.WindowMode;

// Or control mode
documentContainer.TDIFullScreenMode = FullScreenMode.ControlMode;

// Disable full screen
documentContainer.TDIFullScreenMode = FullScreenMode.None;
```

### Full Screen Behavior

**WindowMode:**
- Maximizes the parent window
- Hides window title bar and borders
- Tab header auto-hides
- Move mouse to top edge to reveal tab header
- Press ESC to exit full screen

**ControlMode:**
- Window size remains unchanged
- Tab header auto-hides
- Move mouse to top edge to reveal tab header
- Provides distraction-free document viewing

### Use Cases for Full Screen

**Presentations:**
```csharp
private void StartPresentation()
{
    documentContainer.TDIFullScreenMode = FullScreenMode.WindowMode;
    MessageBox.Show("Full screen mode activated. Press ESC to exit.");
}

private void ExitPresentation()
{
    documentContainer.TDIFullScreenMode = FullScreenMode.None;
}
```

**Reading Mode:**
```csharp
private void EnableReadingMode()
{
    // Control mode for distraction-free reading
    documentContainer.TDIFullScreenMode = FullScreenMode.ControlMode;
    
    // Hide toolbar if present
    documentContainer.TDIToolBarTray = null;
}
```

## Drag and Drop Configuration

Control how TDI tabs can be dragged and dropped.

### Disabling Drag and Drop

Prevent users from reordering tabs or creating tab groups:

```xaml
<syncfusion:DocumentContainer Name="documentContainer" 
                              Mode="TDI"
                              IsTDIDragDropEnabled="False">
    <Grid syncfusion:DocumentContainer.Header="Tab1"/>
    <Grid syncfusion:DocumentContainer.Header="Tab2"/>
    <Grid syncfusion:DocumentContainer.Header="Tab3"/>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
// Disable drag and drop
documentContainer.IsTDIDragDropEnabled = false;

// Enable drag and drop (default)
documentContainer.IsTDIDragDropEnabled = true;
```

### Auto-Scroll During Drag

Enable automatic scrolling when dragging tabs in overflow scenarios:

```xaml
<syncfusion:DocumentContainer Name="documentContainer" 
                              EnableAutoScroll="True" 
                              Mode="TDI">
    <!-- Many documents that may overflow -->
    <ContentControl syncfusion:DocumentContainer.Header="Document1"/>
    <ContentControl syncfusion:DocumentContainer.Header="Document2"/>
    <ContentControl syncfusion:DocumentContainer.Header="Document3"/>
    <ContentControl syncfusion:DocumentContainer.Header="Document4"/>
    <ContentControl syncfusion:DocumentContainer.Header="Document5"/>
    <ContentControl syncfusion:DocumentContainer.Header="Document6"/>
    <ContentControl syncfusion:DocumentContainer.Header="Document7"/>
    <ContentControl syncfusion:DocumentContainer.Header="Document8"/>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
documentContainer.EnableAutoScroll = true;
```

### Tab Order Change Notifications

Monitor when tabs are reordered:

```xaml
<syncfusion:DocumentContainer DocumentTabOrderChanged="DocumentContainer_DocumentTabOrderChanged"
                              DocumentTabOrderChanging="DocumentContainer_DocumentTabOrderChanging"
                              Name="documentContainer"
                              Mode="TDI">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

**Event Handlers:**
```csharp
// Before tab order changes (can cancel)
private void DocumentContainer_DocumentTabOrderChanging(
    object sender, 
    DocumentTabOrderChangingEventArgs e)
{
    string header = DocumentContainer.GetHeader(e.SourceTabItem);
    
    // Prevent moving specific tabs
    if (header == "Locked Tab")
    {
        e.Cancel = true;
        MessageBox.Show("This tab cannot be moved.");
        return;
    }
    
    Console.WriteLine($"Tab '{header}' moving from index {e.OldIndex} to {e.NewIndex}");
}

// After tab order changes
private void DocumentContainer_DocumentTabOrderChanged(
    object sender, 
    DocumentTabOrderChangedEventArgs e)
{
    string header = DocumentContainer.GetHeader(e.SourceTabItem);
    
    // Log the change
    Console.WriteLine($"Tab '{header}' moved from index {e.OldIndex} to {e.NewIndex}");
    
    // Save new tab order
    SaveTabOrder();
}
```

## MDI Window Sizing

Control how MDI windows respond to their content size.

### SizeToContent in MDI

Make MDI windows automatically size to their content:

```xaml
<syncfusion:DocumentContainer Mode="MDI">
    <!-- Window that sizes to content -->
    <Grid Name="grid1" 
          syncfusion:DocumentContainer.SizetoContentInMDI="True" 
          Width="200" 
          Height="200"
          syncfusion:DocumentContainer.Header="Auto-Sized Window">
        <TextBlock Text="This window sizes to its content" 
                   TextWrapping="Wrap"
                   Padding="10"/>
    </Grid>
    
    <!-- Regular MDI window -->
    <Grid syncfusion:DocumentContainer.Header="Normal Window">
        <TextBlock Text="This window has standard MDI sizing"/>
    </Grid>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
// Create window
Grid content = new Grid { Width = 200, Height = 200 };
content.Children.Add(new TextBlock { Text = "Auto-sized content" });

// Enable size to content
DocumentContainer.SetSizetoContentInMDI(content, true);

// Set header and add
DocumentContainer.SetHeader(content, "Auto-Sized Window");
documentContainer.Items.Add(content);
```

### Dynamic Size Adjustment

```csharp
private void AdjustWindowToContent(FrameworkElement window)
{
    // Enable size to content temporarily
    DocumentContainer.SetSizetoContentInMDI(window, true);
    
    // Force layout update
    window.UpdateLayout();
    
    // Disable size to content to allow manual resizing
    DocumentContainer.SetSizetoContentInMDI(window, false);
}
```

## Integration with DockingManager

DocumentContainer can work alongside DockingManager for comprehensive window management.

### Basic Integration

```xaml
<syncfusion:DockingManager Name="dockingManager" 
                          UseDocumentContainer="True"
                          IsTDIDragDropEnabled="True">
    <!-- Docked tool windows -->
    <Grid syncfusion:DockingManager.Header="Solution Explorer" 
          syncfusion:DockingManager.State="Dock"
          syncfusion:DockingManager.SideInDockedMode="Right"/>
    
    <Grid syncfusion:DockingManager.Header="Properties" 
          syncfusion:DockingManager.State="Dock"
          syncfusion:DockingManager.SideInDockedMode="Right"/>
    
    <!-- Document windows -->
    <Grid syncfusion:DockingManager.Header="Document1" 
          syncfusion:DockingManager.State="Document"/>
    
    <Grid syncfusion:DockingManager.Header="Document2" 
          syncfusion:DockingManager.State="Document"/>
</syncfusion:DockingManager>
```

**Why Integrate:**
- DockingManager provides tool windows (dockable panels)
- DocumentContainer provides document management
- Together they create IDE-like interfaces

## Event Handling

### Document Selection Events

Track when users switch between documents:

```csharp
private void DocumentContainer_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (documentContainer.SelectedItem != null)
    {
        string header = DocumentContainer.GetHeader(documentContainer.SelectedItem);
        statusBar.Text = $"Active document: {header}";
        
        // Update UI based on active document
        UpdateToolbarForDocument(documentContainer.SelectedItem);
    }
}
```

### Document Addition/Removal Events

Monitor when documents are added or removed:

```csharp
private void SetupDocumentMonitoring()
{
    // Monitor Items collection changes
    ((INotifyCollectionChanged)documentContainer.Items).CollectionChanged += 
        DocumentContainer_ItemsChanged;
}

private void DocumentContainer_ItemsChanged(object sender, NotifyCollectionChangedEventArgs e)
{
    if (e.Action == NotifyCollectionChangedAction.Add)
    {
        foreach (var item in e.NewItems)
        {
            string header = DocumentContainer.GetHeader(item);
            Console.WriteLine($"Document added: {header}");
        }
    }
    else if (e.Action == NotifyCollectionChangedAction.Remove)
    {
        foreach (var item in e.OldItems)
        {
            string header = DocumentContainer.GetHeader(item);
            Console.WriteLine($"Document removed: {header}");
        }
    }
    
    // Update document count
    documentCountLabel.Text = $"{documentContainer.Items.Count} documents";
}
```

## Keyboard Shortcuts

Enhance productivity with keyboard shortcuts:

```csharp
private void Window_KeyDown(object sender, KeyEventArgs e)
{
    // Ctrl+T: New document
    if (e.Key == Key.T && Keyboard.Modifiers == ModifierKeys.Control)
    {
        CreateNewDocument();
        e.Handled = true;
    }
    
    // Ctrl+W: Close current document
    else if (e.Key == Key.W && Keyboard.Modifiers == ModifierKeys.Control)
    {
        CloseCurrentDocument();
        e.Handled = true;
    }
    
    // Ctrl+Shift+T: Reopen last closed
    else if (e.Key == Key.T && Keyboard.Modifiers == (ModifierKeys.Control | ModifierKeys.Shift))
    {
        ReopenLastClosed();
        e.Handled = true;
    }
    
    // Ctrl+1 through Ctrl+9: Switch to document by number
    else if (Keyboard.Modifiers == ModifierKeys.Control && 
             e.Key >= Key.D1 && e.Key <= Key.D9)
    {
        int index = e.Key - Key.D1;
        if (index < documentContainer.Items.Count)
        {
            documentContainer.SelectedIndex = index;
        }
        e.Handled = true;
    }
}

private Stack<object> closedDocuments = new Stack<object>();

private void CloseCurrentDocument()
{
    if (documentContainer.SelectedItem != null)
    {
        var current = documentContainer.SelectedItem;
        closedDocuments.Push(current);
        documentContainer.Items.Remove(current);
    }
}

private void ReopenLastClosed()
{
    if (closedDocuments.Count > 0)
    {
        var document = closedDocuments.Pop();
        documentContainer.Items.Add(document);
        documentContainer.SelectedItem = document;
    }
}
```

## Performance Optimization

### Virtualization for Many Documents

```csharp
// Limit number of simultaneously loaded documents
private const int MaxLoadedDocuments = 10;

private void OptimizeDocumentLoading()
{
    // Unload documents that aren't visible
    for (int i = 0; i < documentContainer.Items.Count; i++)
    {
        var item = documentContainer.Items[i];
        
        // Unload if not in recent documents
        if (i > MaxLoadedDocuments && item != documentContainer.SelectedItem)
        {
            UnloadDocument(item);
        }
    }
}

private void UnloadDocument(object document)
{
    // Implementation depends on your document type
    // Could involve clearing content, releasing resources, etc.
}
```

### Lazy Loading Documents

```csharp
public class LazyDocument
{
    public string Header { get; set; }
    public string FilePath { get; set; }
    private UIElement _content;
    
    public UIElement Content
    {
        get
        {
            if (_content == null)
            {
                // Load content on first access
                _content = LoadContentFromFile(FilePath);
            }
            return _content;
        }
    }
    
    private UIElement LoadContentFromFile(string path)
    {
        // Load document content
        TextBox editor = new TextBox
        {
            Text = File.ReadAllText(path),
            AcceptsReturn = true,
            TextWrapping = TextWrapping.Wrap
        };
        return editor;
    }
}
```

## Troubleshooting Common Issues

### Issue: Full Screen Mode Not Working

**Problem:** TDIFullScreenMode doesn't activate full screen

**Solutions:**
- Ensure container is in TDI mode (`Mode="TDI"`)
- Check that parent window is not constrained by layout
- Verify `TDIFullScreenMode` is set to `WindowMode` or `ControlMode`

### Issue: Drag and Drop Doesn't Work

**Problem:** Cannot reorder tabs by dragging

**Solutions:**
- Verify `IsTDIDragDropEnabled="True"` (default)
- Check that container is in TDI mode
- Ensure tabs are not locked via event handlers

### Issue: MDI Windows Don't Size Correctly

**Problem:** SizetoContentInMDI doesn't work

**Solutions:**
- Ensure container is in MDI mode (`Mode="MDI"`)
- Verify child element has explicit Width and Height set
- Check that content is loaded before setting property

### Issue: Events Not Firing

**Problem:** DocumentTabOrderChanged event doesn't fire

**Solutions:**
- Ensure event is properly subscribed in XAML or code
- Verify `IsTDIDragDropEnabled="True"`
- Check that tabs are actually being reordered

## Best Practices

1. **Full screen mode:** Use for presentations or reading mode; provide clear exit instructions
2. **Drag and drop:** Disable only when document order is critical to workflow
3. **Auto-scroll:** Enable when you have many tabs that may overflow
4. **Event handling:** Always check for null before accessing selected items
5. **Performance:** Implement lazy loading for documents with heavy content
6. **Keyboard shortcuts:** Provide shortcuts for power users, document them clearly
7. **State persistence:** Save full screen mode preference with other layout settings
8. **Integration:** When using DockingManager, clearly separate tool windows from documents

## Advanced Scenarios

### Scenario 1: Presentation Mode

```csharp
private bool isPresentationMode = false;

private void TogglePresentationMode()
{
    isPresentationMode = !isPresentationMode;
    
    if (isPresentationMode)
    {
        // Enter presentation mode
        documentContainer.TDIFullScreenMode = FullScreenMode.WindowMode;
        documentContainer.TDIToolBarTray = null;
        documentContainer.IsTDIDragDropEnabled = false;
        
        // Hide window chrome
        this.WindowStyle = WindowStyle.None;
        this.WindowState = WindowState.Maximized;
        
        ShowPresentationHelpOverlay();
    }
    else
    {
        // Exit presentation mode
        documentContainer.TDIFullScreenMode = FullScreenMode.None;
        RestoreToolbar();
        documentContainer.IsTDIDragDropEnabled = true;
        
        // Restore window chrome
        this.WindowStyle = WindowStyle.SingleBorderWindow;
        this.WindowState = WindowState.Normal;
    }
}

private void ShowPresentationHelpOverlay()
{
    var overlay = new TextBlock
    {
        Text = "Presentation Mode\nPress ESC to exit\nCTRL+TAB to switch documents",
        FontSize = 18,
        Foreground = Brushes.White,
        Background = new SolidColorBrush(Color.FromArgb(200, 0, 0, 0)),
        Padding = new Thickness(20),
        HorizontalAlignment = HorizontalAlignment.Center,
        VerticalAlignment = VerticalAlignment.Top,
        Margin = new Thickness(0, 50, 0, 0)
    };
    
    // Add to main grid and auto-hide after 3 seconds
    mainGrid.Children.Add(overlay);
    
    var timer = new DispatcherTimer { Interval = TimeSpan.FromSeconds(3) };
    timer.Tick += (s, e) => 
    {
        mainGrid.Children.Remove(overlay);
        timer.Stop();
    };
    timer.Start();
}
```

### Scenario 2: IDE-Style Document Management

```csharp
public class IDEDocumentManager
{
    private DocumentContainer container;
    private Dictionary<string, object> openDocuments = new Dictionary<string, object>();
    
    public IDEDocumentManager(DocumentContainer container)
    {
        this.container = container;
        SetupKeyboardShortcuts();
    }
    
    public void OpenDocument(string filePath)
    {
        // Check if already open
        if (openDocuments.ContainsKey(filePath))
        {
            container.SelectedItem = openDocuments[filePath];
            return;
        }
        
        // Load and add new document
        var content = LoadDocument(filePath);
        string fileName = Path.GetFileName(filePath);
        
        DocumentContainer.SetHeader(content, fileName);
        container.Items.Add(content);
        openDocuments[filePath] = content;
        
        container.SelectedItem = content;
    }
    
    public void CloseDocument(object document)
    {
        // Find and remove from tracking
        var entry = openDocuments.FirstOrDefault(kvp => kvp.Value == document);
        if (entry.Key != null)
        {
            openDocuments.Remove(entry.Key);
        }
        
        container.Items.Remove(document);
    }
    
    public void CloseAllDocuments()
    {
        container.Items.Clear();
        openDocuments.Clear();
    }
    
    public void CloseAllButCurrent()
    {
        var current = container.SelectedItem;
        var toClose = container.Items.Cast<object>()
            .Where(item => item != current)
            .ToList();
        
        foreach (var item in toClose)
        {
            CloseDocument(item);
        }
    }
    
    private UIElement LoadDocument(string filePath)
    {
        // Load based on file type
        string ext = Path.GetExtension(filePath).ToLower();
        
        switch (ext)
        {
            case ".txt":
            case ".cs":
            case ".xaml":
                return LoadTextDocument(filePath);
            
            case ".jpg":
            case ".png":
                return LoadImageDocument(filePath);
            
            default:
                return new TextBlock { Text = $"Unsupported file type: {ext}" };
        }
    }
    
    private UIElement LoadTextDocument(string filePath)
    {
        return new TextBox
        {
            Text = File.ReadAllText(filePath),
            AcceptsReturn = true,
            TextWrapping = TextWrapping.NoWrap,
            FontFamily = new FontFamily("Consolas"),
            FontSize = 12,
            VerticalScrollBarVisibility = ScrollBarVisibility.Auto,
            HorizontalScrollBarVisibility = ScrollBarVisibility.Auto
        };
    }
    
    private UIElement LoadImageDocument(string filePath)
    {
        var image = new Image
        {
            Source = new BitmapImage(new Uri(filePath)),
            Stretch = Stretch.Uniform
        };
        
        var scrollViewer = new ScrollViewer
        {
            Content = image,
            HorizontalScrollBarVisibility = ScrollBarVisibility.Auto,
            VerticalScrollBarVisibility = ScrollBarVisibility.Auto
        };
        
        return scrollViewer;
    }
    
    private void SetupKeyboardShortcuts()
    {
        // Implementation would hook into main window's KeyDown event
    }
}
```

## Related Documentation

- **Container Modes:** See `container-modes.md` for TDI vs MDI overview
- **Tab Management:** See `tab-management.md` for TDI tab operations
- **Window Management:** See `window-management.md` for MDI window operations
- **State Persistence:** See `state-persistence.md` for saving full screen preferences
- **Customization:** See `customization.md` for styling and theming

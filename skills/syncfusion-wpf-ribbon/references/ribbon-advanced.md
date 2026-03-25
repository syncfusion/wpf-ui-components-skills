# Advanced Features

## Table of Contents
- [Ribbon Merge](#ribbon-merge)
- [State Persistence](#state-persistence)
- [Dynamic Ribbon Changes](#dynamic-ribbon-changes)
- [Ribbon Reduce Layout](#ribbon-reduce-layout)
- [Contextual Tabs](#contextual-tabs)
- [QAT Customization](#qat-customization)
- [Performance Optimization](#performance-optimization)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Ribbon Merge

### Purpose

Ribbon merge allows different ribbon configurations to merge and switch based on context (e.g., multi-document interface with document-specific commands).

### Basic Ribbon Merge Setup

**Main Window with Base Ribbon:**

```csharp
// MainWindow.xaml.cs
public partial class MainWindow : RibbonWindow
{
    private Dictionary<string, Ribbon> _ribbons = new();

    public MainWindow()
    {
        InitializeComponent();
    }

    public void RegisterRibbon(string key, Ribbon ribbon)
    {
        _ribbons[key] = ribbon;
    }

    public void SwitchRibbon(string key)
    {
        if (_ribbons.TryGetValue(key, out var ribbon))
        {
            // Switch active ribbon
            this.SetRibbon(ribbon);
        }
    }

    private void SetRibbon(Ribbon newRibbon)
    {
        var currentRibbon = this.FindName("MainRibbon") as Ribbon;
        if (currentRibbon != null)
        {
            this.RibbonContent.Child = newRibbon;
        }
    }
}
```

**Main Ribbon:**

```xml
<!-- MainWindow.xaml -->
<syncfusion:RibbonWindow x:Name="MainRibbon">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <syncfusion:Ribbon x:Name="MainRibbon" Grid.Row="0">
            <!-- Common tabs -->
            <syncfusion:RibbonTab Caption="Home">
                <!-- Common commands -->
            </syncfusion:RibbonTab>
        </syncfusion:Ribbon>
        
        <!-- Content area for documents -->
        <Grid x:Name="DocumentContainer" Grid.Row="1" />
    </Grid>
</syncfusion:RibbonWindow>
```

**Document-Specific Ribbon:**

```xml
<!-- DocumentRibbon.xaml (as a Resource or ResourceDictionary) -->
<syncfusion:Ribbon x:Key="DocumentRibbon">
    <syncfusion:RibbonTab Caption="Home">
        <!-- Common commands -->
    </syncfusion:RibbonTab>
    
    <!-- Document-specific tab -->
    <syncfusion:RibbonTab Caption="Document">
        <syncfusion:RibbonBar Header="Sections">
            <syncfusion:RibbonButton Label="New Section" />
            <syncfusion:RibbonButton Label="Delete Section" />
        </syncfusion:RibbonBar>
    </syncfusion:RibbonTab>
</syncfusion:Ribbon>
```

### Switching Ribbons on Document Change

```csharp
private void OnDocumentOpened(object sender, DocumentOpenedEventArgs e)
{
    var ribbon = new Ribbon();
    ribbon.Items.Add(CreateDocumentTab());
    
    // Merge or switch ribbons
    MergeRibbons(MainRibbon, ribbon);
}

private RibbonTab CreateDocumentTab()
{
    var tab = new RibbonTab { Caption = e.DocumentType };
    var bar = new RibbonBar { Header = "Operations" };
    
    var button = new RibbonButton { Label = "Save" };
    bar.Items.Add(button);
    tab.Items.Add(bar);
    
    return tab;
}
```

## State Persistence

#### Automatic State Persistence

Enable automatic state persistence using `AutoPersist`:

```xml
<syncfusion:Ribbon x:Name="MainRibbon" AutoPersist="True">
    <!-- Ribbon content -->
</syncfusion:Ribbon>
```

When `AutoPersist` is enabled, the ribbon automatically saves and restores its state (QAT items, minimized state, etc.) across application sessions.

#### Manual State Persistence

For manual control, use the built-in methods:

```csharp
// Save ribbon state to default isolated storage
MainRibbon.SaveRibbonState();

// Load ribbon state from default isolated storage
MainRibbon.LoadRibbonState();
```

#### Custom Storage Location

Save and load from a custom isolated storage file:

```csharp
using System.IO.IsolatedStorage;

// Get isolated storage
IsolatedStorageFile isoStorage = IsolatedStorageFile.GetUserStoreForAssembly();

// Save to custom file
MainRibbon.SaveRibbonState(isoStorage, "MyAppRibbon.state");

// Load from custom file
MainRibbon.LoadRibbonState(isoStorage, "MyAppRibbon.state");
```

### Save and Restore in Application Lifecycle

```csharp
// In MainWindow
public partial class MainWindow : RibbonWindow
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Restore state on load
        Loaded += OnWindowLoaded;
        Closing += OnWindowClosing;
    }

    private void OnWindowLoaded(object sender, RoutedEventArgs e)
    {
        // Restore saved ribbon state
        MainRibbon.LoadRibbonState();
    }

    private void OnWindowClosing(object sender, CancelEventArgs e)
    {
        // Save ribbon state before closing
        MainRibbon.SaveRibbonState();
        base.OnClosed(e);
    }
}
```

## Dynamic Ribbon Changes

### Add Tabs Programmatically

```csharp
public void AddDocumentTab(string documentType)
{
    var tab = new RibbonTab { Caption = documentType };
    
    var bar = new RibbonBar { Header = "Tools" };
    bar.Items.Add(new RibbonButton { Label = "Tool 1" });
    bar.Items.Add(new RibbonButton { Label = "Tool 2" });
    
    tab.Items.Add(bar);
    MainRibbon.Items.Add(tab);
}
```

### Remove Tabs Programmatically

```csharp
public void RemoveTab(string tabName)
{
    var tab = MainRibbon.Items.OfType<RibbonTab>()
        .FirstOrDefault(t => t.Caption?.ToString() == tabName);
    
    if (tab != null)
    {
        MainRibbon.Items.Remove(tab);
    }
}
```

### Show/Hide Tabs Dynamically

```csharp
public void SetTabVisibility(string tabName, bool visible)
{
    var tab = MainRibbon.Items.OfType<RibbonTab>()
        .FirstOrDefault(t => t.Caption?.ToString() == tabName);
    
    if (tab != null)
    {
        tab.IsVisible = visible;
    }
}
```

### Update Group Content

```csharp
public void UpdateGroupButtons(string tabName, string groupName, List<string> buttonLabels)
{
    var tab = MainRibbon.Items.OfType<RibbonTab>()
        .FirstOrDefault(t => t.Caption?.ToString() == tabName);
    
    if (tab == null) return;
    
    var bar = tab.Items.OfType<RibbonBar>()
        .FirstOrDefault(g => g.Header?.ToString() == groupName);
    
    if (bar == null) return;
    
    bar.Items.Clear();
    
    foreach (var label in buttonLabels)
    {
        bar.Items.Add(new RibbonButton { Label = label });
    }
}
```

## Ribbon Reduce Layout

### Auto-Collapsing Groups

Ribbon automatically reduces to smaller sizes at narrow widths:

```xml
<syncfusion:RibbonBar Header="File Operations">
    <!-- Highest priority: always visible -->
    <syncfusion:RibbonButton Label="New" SizeForm="Large" />   
    <syncfusion:RibbonButton Label="Open" SizeForm="Normal" />
    <syncfusion:RibbonButton Label="Save" SizeForm="Small" />
</syncfusion:RibbonBar>

<syncfusion:RibbonBar Header="Styles">
    <!-- Lower priority: reduced/hidden when space tight -->
    <syncfusion:RibbonButton Label="Bold" />
    <syncfusion:RibbonButton Label="Italic" />
</syncfusion:RibbonBar>
```

### Resize Behavior

```csharp
// Programmatically collapse bar
var bar = ribbon.Items.OfType<RibbonTab>()
    .FirstOrDefault()?.Items.OfType<RibbonBar>()
    .FirstOrDefault();

if (bar != null)
{
    bar.IsDropDownOpen = true; // Force to dropdown
}
```

## Contextual Tabs

### Show Tabs on Demand

Display tabs only when relevant:

```xml
<syncfusion:RibbonTab Caption="Table Tools" 
                      IsContextual="True"
                      IsVisible="False">
    <syncfusion:RibbonBar Header="Table Design">
        <syncfusion:RibbonButton Label="Insert Row" />
        <syncfusion:RibbonButton Label="Delete Row" />
    </syncfusion:RibbonBar>
</syncfusion:RibbonTab>
```

### Show Tab When Table Selected

```csharp
private void Document_TableSelected(object sender, TableSelectedEventArgs e)
{
    var tableToolsTab = MainRibbon.Items.OfType<RibbonTab>()
        .FirstOrDefault(t => t.Caption?.ToString() == "Table Tools");
    
    if (tableToolsTab != null)
    {
        tableToolsTab.IsVisible = true;
        tableToolsTab.IsSelected = true;
    }
}

private void Document_TableDeselected(object sender, EventArgs e)
{
    var tableToolsTab = MainRibbon.Items.OfType<RibbonTab>()
        .FirstOrDefault(t => t.Caption?.ToString() == "Table Tools");
    
    if (tableToolsTab != null)
    {
        tableToolsTab.IsVisible = false;
    }
}
```

## QAT Customization

### Programmatic QAT Items

```csharp
public void AddToQAT(string buttonLabel)
{
    var ribbon = this.FindName("MainRibbon") as Ribbon;
    if (ribbon == null) return;
    
    var button = new RibbonButton { Label = buttonLabel };
    ribbon.QuickAccessToolBar.Items.Add(button);
}

public void RemoveFromQAT(string buttonLabel)
{
    var ribbon = this.FindName("MainRibbon") as Ribbon;
    if (ribbon == null) return;
    
    var button = ribbon.QuickAccessToolBar.Items.OfType<RibbonButton>()
        .FirstOrDefault(b => b.Label?.ToString() == buttonLabel);
    
    if (button != null)
    {
        ribbon.QuickAccessToolBar.Items.Remove(button);
    }
}
```

### Allow User QAT Customization

```xml
<syncfusion:Ribbon IsQATCustomizationEnabled="True">
    <!-- Right-click buttons to add/remove from QAT -->
</syncfusion:Ribbon>
```

## Performance Optimization

### Lazy Load Tabs

Don't create all tab content upfront:

```csharp
public class LazyRibbonTab
{
    private RibbonTab _tab;
    private bool _loaded = false;

    public RibbonTab Tab
    {
        get
        {
            if (!_loaded)
            {
                LoadContent();
                _loaded = true;
            }
            return _tab;
        }
    }

    private void LoadContent()
    {
        _tab = new RibbonTab { Caption = "Lazy Tab" };
        var bar = new RibbonBar { Header = "Content" };
        
        // Add many items
        for (int i = 0; i < 100; i++)
        {
            bar.Items.Add(new RibbonButton { Label = $"Button {i}" });
        }
        
        _tab.Items.Add(bar);
    }
}
```

### Virtualization for Large Lists

For galleries with many items:

```xml
<syncfusion:RibbonGallery Label="Large Gallery"
                          VirtualizingStackPanel.IsVirtualizing="True"
                          VirtualizingStackPanel.VirtualizationMode="Recycling">
    <!-- Items will be virtualized for performance -->
</syncfusion:RibbonGallery>
```

## Common Patterns  

### Pattern 1: Context-Sensitive Ribbon
```csharp
public void UpdateRibbonForSelection(object selectedItem)
{
    if (selectedItem is Table)
    {
        ShowContextualTab("Table Tools");
    }
    else if (selectedItem is Image)
    {
        ShowContextualTab("Image Tools");
    }
    else
    {
        HideAllContextualTabs();
    }
}
```

### Pattern 2: MDI with Ribbon Switching
```csharp
public void OnChildDocumentActivated(Document document)
{
    var ribbon = CreateRibbonForDocument(document);
    SwitchRibbon(ribbon);
}

private Ribbon CreateRibbonForDocument(Document document)
{
    var ribbon = new Ribbon();
    // Configure based on document type
    return ribbon;
}
```

### Pattern 3: Remember User Customizations
```csharp
public void OnApplicationShutdown()
{
    _stateManager.SaveRibbonState(MainRibbon);
    _stateManager.SaveQATCustomizations();
    _stateManager.SaveTabVisibility();
}
```

## Troubleshooting

### Issue: Ribbon merge not working
**Solution:** Ensure both ribbons are properly initialized. Check that merge operation occurs after both ribbons are created.

### Issue: State not persisting
**Solution:** Verify file permissions for saving. Check serialization logic. Ensure restoration code runs on application startup.

### Issue: Dynamic tabs not appearing
**Solution:** Ensure `IsVisible` is set to `true`. Verify tab is added to `Items` collection. Check data binding if using ItemsSource.

### Issue: Contextual tabs not hiding
**Solution:** Set `IsVisible = false` when condition no longer met. Ensure event handlers are properly unsubscribed.

### Issue: Performance degradation with many items
**Solution:** Use virtualization for galleries. Implement lazy loading for tabs. Use `VirtualizingStackPanel`.

### Issue: QAT customization not saving
**Solution:** Ensure `AllowQATCustomization="True"`. Persist customizations to storage. Restore on application startup.

# Advanced Features (Part 1)

## Table of Contents
- [Ribbon Merge](#ribbon-merge)
- [State Persistence](#state-persistence)
- [Continued](#continued)

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
        // Do not call OnClosed from a Closing event handler; saving is sufficient here.
    }
}
```

---

Continued in [references/ribbon-advanced-continued.md](references/ribbon-advanced-continued.md)


# Advanced Features and Interactions

## Table of Contents
- [Context Menus](#context-menus)
- [Custom Context Menu Items](#custom-context-menu-items)
- [Tab Reordering](#tab-reordering)
- [New Tab Button](#new-tab-button)
- [Tab Navigation and Scrolling](#tab-navigation-and-scrolling)
- [Localization Support](#localization-support)
- [Advanced Patterns](#advanced-patterns)

## Context Menus

The TabControl provides built-in context menu support at two levels: tab control level and tab item level.

### Tab Control Context Menu (Tab List Menu)

The tab list menu appears in the top-right corner and shows all available tabs for quick navigation.

**Enable/Disable Tab List Menu:**
```xml
<syncfusion:TabControlExt ShowTabListContextMenu="True" Name="tabControl" />
```

```csharp
tabControl.ShowTabListContextMenu = true;
```

**Customize What Appears in the Menu:**

```xml
<syncfusion:TabControlExt ShowTabListContextMenu="True"
                          TabListContextMenuOptions="Default, ShowHiddenItems, ShowDisabledItems"
                          Name="tabControl" />
```

```csharp
tabControl.ShowTabListContextMenu = true;
tabControl.TabListContextMenuOptions = 
    TabListContextMenuOptions.Default | 
    TabListContextMenuOptions.ShowHiddenItems | 
    TabListContextMenuOptions.ShowDisabledItems;
```

**Menu Options:**
- `Default` - Show only visible, enabled tabs
- `ShowHiddenItems` - Include hidden (collapsed) tabs
- `ShowDisabledItems` - Include disabled tabs

### Tab Item Context Menu (Built-in)

Right-click on a tab header to show the built-in context menu with standard options:

**Enable/Disable Tab Item Context Menu:**
```xml
<syncfusion:TabControlExt ShowTabItemContextMenu="True" Name="tabControl" />
```

```csharp
tabControl.ShowTabItemContextMenu = true;
```

**Built-in Menu Items:**
- Close - Close the selected tab
- Close All But This - Close all other tabs
- Close All - Close all tabs

## Custom Context Menu Items

Add custom menu items to individual tab items:

### Basic Custom Menu Item

```xml
<syncfusion:TabControlExt IsCustomTabItemContextMenuEnabled="True" 
                          Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1">
        <syncfusion:TabItemExt.ContextMenuItems>
            <syncfusion:CustomMenuItem Header="Save" />
            <syncfusion:CustomMenuItem Header="Export" />
            <Separator />
            <syncfusion:CustomMenuItem Header="Properties" />
        </syncfusion:TabItemExt.ContextMenuItems>
    </syncfusion:TabItemExt>
</syncfusion:TabControlExt>
```

### Custom Menu with Command Binding

```xml
<syncfusion:TabControlExt IsCustomTabItemContextMenuEnabled="True" Name="tabControl">
    <syncfusion:TabItemExt Header="Tab 1">
        <syncfusion:TabItemExt.ContextMenuItems>
            <syncfusion:CustomMenuItem Header="Save" 
                                        Command="{Binding SaveCommand}"
                                        CommandParameter="{Binding RelativeSource={RelativeSource Self}}" />
            <syncfusion:CustomMenuItem Header="Delete" 
                                        Command="{Binding DeleteCommand}" />
        </syncfusion:TabItemExt.ContextMenuItems>
    </syncfusion:TabItemExt>
</syncfusion:TabControlExt>
```

### C# Approach for Custom Menu Items

```csharp
// Enable custom context menus
tabControl.IsCustomTabItemContextMenuEnabled = true;

// Create and add custom menu item
CustomMenuItem saveMenuItem = new CustomMenuItem() { Header = "Save" };
saveMenuItem.Click += SaveMenuItem_Click;

tabItem.ContextMenuItems.Add(saveMenuItem);

private void SaveMenuItem_Click(object sender, RoutedEventArgs e)
{
    // Handle save action
    MessageBox.Show("Save clicked for: " + tabItem.Header);
}
```

### Advanced Custom Menu with Icons

```xml
<syncfusion:CustomMenuItem Header="Save">
    <syncfusion:CustomMenuItem.Icon>
        <Image Source="save_icon.png" Width="16" Height="16" />
    </syncfusion:CustomMenuItem.Icon>
</syncfusion:CustomMenuItem>
```

## Tab Reordering

Enable users to reorder tabs by dragging and dropping:

### Enabling Drag and Drop

The TabControl supports drag-drop reordering by default. Users can click and drag tab headers to reorder them.

### Custom Drag Marker

Customize the appearance of the drag indicator:

```csharp
public void CustomizeDragMarker()
{
    // The drag marker color changes based on theme
    // Styling is theme-dependent
}
```

### Programmatic Reordering

Move tabs programmatically:

```csharp
public void MoveTabToPosition(TabItemExt tab, int newIndex)
{
    int currentIndex = tabControl.Items.IndexOf(tab);
    
    if (currentIndex >= 0 && newIndex >= 0 && newIndex < tabControl.Items.Count)
    {
        tabControl.Items.RemoveAt(currentIndex);
        tabControl.Items.Insert(newIndex, tab);
    }
}

// Usage
MoveTabToPosition(tabItem, 0);  // Move to first position
```

### Reordering with Constraints

Prevent certain tabs from being moved:

```csharp
private HashSet<TabItemExt> _lockedTabs = new HashSet<TabItemExt>();

public void LockTabFromReordering(TabItemExt tab)
{
    _lockedTabs.Add(tab);
}

public void UnlockTabFromReordering(TabItemExt tab)
{
    _lockedTabs.Remove(tab);
}

// Custom logic to prevent drag-drop of locked tabs
private void OnTabDragStarted(TabItemExt tab)
{
    if (_lockedTabs.Contains(tab))
    {
        // Prevent drag operation
    }
}
```

## New Tab Button

Add a button to create new tabs:

### Enabling New Tab Button

```xml
<syncfusion:TabControlExt IsNewButtonEnabled="True" 
                          NewButtonClick="TabControl_NewButtonClick"
                          Name="tabControl" />
```

```csharp
tabControl.IsNewButtonEnabled = true;
tabControl.NewButtonClick += TabControl_NewButtonClick;
```

### Handling New Tab Button Click

```csharp
private void TabControl_NewButtonClick(object sender, EventArgs e)
{
    // Create new tab
    TabItemExt newTab = new TabItemExt()
    {
        Header = $"Tab {tabControl.Items.Count + 1}",
        Content = new TextBlock() { Text = "New tab content" }
    };
    
    // Add to control
    tabControl.Items.Add(newTab);
    
    // Select the new tab
    tabControl.SelectedItem = newTab;
}
```

### Advanced: New Tab with Dialog

```csharp
private void TabControl_NewButtonClick(object sender, EventArgs e)
{
    // Show dialog to get tab name
    string tabName = PromptForTabName();
    
    if (!string.IsNullOrEmpty(tabName))
    {
        TabItemExt newTab = new TabItemExt()
        {
            Header = tabName,
            Content = new StackPanel() { Background = Brushes.White }
        };
        
        tabControl.Items.Add(newTab);
        tabControl.SelectedItem = newTab;
    }
}

private string PromptForTabName()
{
    InputDialog dialog = new InputDialog("Enter tab name:", "New Tab");
    if (dialog.ShowDialog() == true)
    {
        return dialog.Input;
    }
    return null;
}
```

## Tab Navigation and Scrolling

### Navigation Buttons

Show navigation buttons for scrolling through tabs:

```xml
<syncfusion:TabControlExt TabScrollButtonVisibility="Visible"
                          TabScrollStyle="Extended"
                          Name="tabControl" />
```

**TabScrollStyle Options:**
- `Standard` - Previous and Next buttons
- `Extended` - First, Previous, Next, and Last buttons

### Programmatic Navigation

```csharp
public void NavigateToNextTab()
{
    int nextIndex = (tabControl.SelectedIndex + 1) % tabControl.Items.Count;
    tabControl.SelectedIndex = nextIndex;
}

public void NavigateToPreviousTab()
{
    int prevIndex = tabControl.SelectedIndex - 1;
    if (prevIndex < 0) prevIndex = tabControl.Items.Count - 1;
    tabControl.SelectedIndex = prevIndex;
}

public void ScrollTabIntoView(TabItemExt tab)
{
    tabControl.SelectedItem = tab;
    tab.BringIntoView();
}
```

### Tab Scrolling Events

Monitor scrolling behavior:

```csharp
// Custom implementation to track tab scrolling
private int _firstVisibleTabIndex = 0;

public void HandleTabScroll(int direction)
{
    _firstVisibleTabIndex += direction;
    // Update visible tabs based on scroll position
}
```

## Localization Support

Support multiple languages in your TabControl application:

### Using Resource Files

1. Create resource files (.resx) for each language:
   - Strings.en-US.resx
   - Strings.fr-FR.resx
   - Strings.de-DE.resx

2. Reference strings in XAML:

```xml
<syncfusion:TabItemExt Header="{x:Static local:Strings.HomeTab}" />
<syncfusion:TabItemExt Header="{x:Static local:Strings.SettingsTab}" />
```

### Syncfusion Localization

Syncfusion provides built-in localization for context menu items. Register localized strings:

```csharp
using Syncfusion.Windows;

public partial class App : Application
{
    public App()
    {
        // Register localizer
        LocalizationProvider.Register(new CustomLocalizer());
    }
}

public class CustomLocalizer : ILocalizationProvider
{
    public string GetLocalizedString(string resourceKey)
    {
        switch (resourceKey)
        {
            case "Close":
                return CultureInfo.CurrentCulture.Name == "fr-FR" ? "Fermer" : "Close";
            case "CloseAllButThis":
                return CultureInfo.CurrentCulture.Name == "fr-FR" ? "Fermer tous sauf celui-ci" : "Close All But This";
            default:
                return resourceKey;
        }
    }
}
```

### Dynamic Language Switching

```csharp
public void ChangeLanguage(string cultureName)
{
    CultureInfo.CurrentCulture = new CultureInfo(cultureName);
    CultureInfo.CurrentUICulture = new CultureInfo(cultureName);
    
    // Refresh UI with new culture
    UpdateTabHeaders();
    UpdateContextMenus();
}

private void UpdateTabHeaders()
{
    foreach (TabItemExt item in tabControl.Items)
    {
        // Update headers based on new culture
    }
}
```

## Advanced Patterns

### Pattern 1: Tab Lifecycle Management

```csharp
public class TabLifecycleManager
{
    private Dictionary<TabItemExt, TabState> _tabStates = 
        new Dictionary<TabItemExt, TabState>();
    
    private enum TabState { Created, Active, Inactive, Closed }
    
    public void OnTabCreated(TabItemExt tab)
    {
        _tabStates[tab] = TabState.Created;
    }
    
    public void OnTabSelected(TabItemExt tab)
    {
        _tabStates[tab] = TabState.Active;
        LoadTabContent(tab);
    }
    
    public void OnTabDeselected(TabItemExt tab)
    {
        _tabStates[tab] = TabState.Inactive;
        UnloadTabContent(tab);
    }
    
    public void OnTabClosed(TabItemExt tab)
    {
        _tabStates[tab] = TabState.Closed;
        CleanupTabResources(tab);
    }
    
    private void LoadTabContent(TabItemExt tab) { }
    private void UnloadTabContent(TabItemExt tab) { }
    private void CleanupTabResources(TabItemExt tab) { }
}
```

### Pattern 2: Tab State Persistence

```csharp
public class TabStatePersistence
{
    private const string ConfigFile = "tab_state.json";
    
    public void SaveTabState(TabControlExt tabControl)
    {
        var state = new
        {
            SelectedIndex = tabControl.SelectedIndex,
            Tabs = tabControl.Items.Cast<TabItemExt>()
                .Select(t => new { Header = t.Header, IsPinned = t.IsPinned })
                .ToList()
        };
        
        string json = JsonConvert.SerializeObject(state);
        File.WriteAllText(ConfigFile, json);
    }
    
    public void RestoreTabState(TabControlExt tabControl)
    {
        if (File.Exists(ConfigFile))
        {
            string json = File.ReadAllText(ConfigFile);
            var state = JsonConvert.DeserializeObject<dynamic>(json);
            
            tabControl.SelectedIndex = state.SelectedIndex;
        }
    }
}
```

### Pattern 3: Tab Search and Filter

```csharp
public void SearchTabs(string searchText)
{
    foreach (TabItemExt tab in tabControl.Items)
    {
        string header = tab.Header?.ToString() ?? "";
        
        if (header.IndexOf(searchText, StringComparison.OrdinalIgnoreCase) >= 0)
        {
            tab.Visibility = Visibility.Visible;
        }
        else
        {
            tab.Visibility = Visibility.Collapsed;
        }
    }
}
```

## Best Practices for Advanced Features

1. **Context Menus**: Provide meaningful, context-specific options
2. **Drag and Drop**: Give clear visual feedback during reordering
3. **New Tab Button**: Provide sensible defaults for new tabs
4. **Localization**: Test thoroughly with different language strings
5. **Performance**: Monitor performance with many tabs and custom menus
6. **Accessibility**: Ensure all features are keyboard-accessible
7. **State Management**: Persist important tab states for users

## Troubleshooting Advanced Features

| Issue | Solution |
|-------|----------|
| Custom menu items not appearing | Verify `IsCustomTabItemContextMenuEnabled=True` |
| Drag-drop not working | Check for event handlers preventing drag operations |
| Localization strings not updating | Force UI refresh after culture change |
| New tab button disabled | Verify `IsNewButtonEnabled=True` |
| Navigation buttons hidden | Check `TabScrollButtonVisibility` property |

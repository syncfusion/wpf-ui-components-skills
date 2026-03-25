# Tab Management in WPF Tabbed Window

## Table of Contents
- [Overview](#overview)
- [Close Button Support](#close-button-support)
- [New Tab Button](#new-tab-button)
- [Tab Selection](#tab-selection)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Mouse Interactions](#mouse-interactions)
- [Programmatic Tab Operations](#programmatic-tab-operations)
- [Common Patterns](#common-patterns)
- [Edge Cases and Gotchas](#edge-cases-and-gotchas)

## Overview

The Tabbed Window provides comprehensive tab management capabilities for dynamic, interactive tab-based interfaces. This guide covers:

- **Close buttons** on individual tabs
- **New tab button** for user-initiated tab creation
- **Tab selection** programmatic and user-driven
- **Keyboard shortcuts** for power users
- **Mouse interactions** including middle-click close
- **Programmatic operations** for add, remove, and select

These features enable flexible, responsive tab management similar to modern web browsers and IDEs.

## Close Button Support

Display close buttons on tabs to allow users to close individual tabs. Each tab can independently control whether its close button is visible.

### Basic Close Button Configuration

**XAML:**

```xml
<syncfusion:SfTabControl>
    <syncfusion:SfTabItem 
        Header="Document 1" 
        CloseButtonVisibility="Visible">
        <TextBlock Text="This tab has a close button" />
    </syncfusion:SfTabItem>
    
    <syncfusion:SfTabItem 
        Header="Document 2" 
        CloseButtonVisibility="Visible">
        <TextBlock Text="This tab can also be closed" />
    </syncfusion:SfTabItem>
    
    <syncfusion:SfTabItem 
        Header="Permanent Tab" 
        CloseButtonVisibility="Collapsed">
        <TextBlock Text="This tab cannot be closed" />
    </syncfusion:SfTabItem>
</syncfusion:SfTabControl>
```

**Code-Behind:**

```csharp
// Create tab with close button
var closableTab = new SfTabItem
{
    Header = "Closable Document",
    Content = new TextBlock { Text = "Content here" },
    CloseButtonVisibility = Visibility.Visible
};
tabControl.Items.Add(closableTab);

// Create tab without close button
var permanentTab = new SfTabItem
{
    Header = "Home",
    Content = new TextBlock { Text = "Home content" },
    CloseButtonVisibility = Visibility.Collapsed
};
tabControl.Items.Add(permanentTab);
```

### Close Button Behavior

When a user clicks the close button:

1. **Tab is automatically removed** from the tab control
2. **Next tab is selected** if available (or previous if closing last tab)
3. **No confirmation dialog** is shown by default
4. **Collection updates** automatically (including bound collections)

### Implementing Close Confirmation

Add confirmation before closing:

```csharp
// Subscribe to selection changing event to intercept closes
tabControl.SelectionChanging += OnSelectionChanging;

private void OnSelectionChanging(object sender, SelectionChangingEventArgs e)
{
    // This approach requires additional tracking
    // Better approach: use custom close button with command
}

// Better approach: Custom close button in header template
```

**Custom Close with Confirmation:**

```xml
<syncfusion:SfTabItem>
    <syncfusion:SfTabItem.Header>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="Document 1" VerticalAlignment="Center"/>
            <Button Content="×" 
                    Margin="8,0,0,0"
                    Click="OnCloseButtonClick"
                    Style="{StaticResource CloseButtonStyle}"/>
        </StackPanel>
    </syncfusion:SfTabItem.Header>
    <syncfusion:SfTabItem.Content>
        <!-- Content -->
    </syncfusion:SfTabItem.Content>
</syncfusion:SfTabItem>
```

```csharp
private void OnCloseButtonClick(object sender, RoutedEventArgs e)
{
    var button = sender as Button;
    var tabItem = FindParent<SfTabItem>(button);
    
    var result = MessageBox.Show(
        "Are you sure you want to close this tab?",
        "Confirm Close",
        MessageBoxButton.YesNo);
    
    if (result == MessageBoxResult.Yes)
    {
        tabControl.Items.Remove(tabItem);
    }
}
```

### Close Button Visibility Options

| Value | Description | Use Case |
|-------|-------------|----------|
| `Visible` | Close button shown | Documents that can be closed |
| `Collapsed` | Close button hidden | Permanent tabs (Home, Settings) |
| `Hidden` | Takes up space but invisible | Rarely used |

### Setting Close Button for All Tabs

Apply to all tabs via style:

```xml
<syncfusion:SfTabControl>
    <syncfusion:SfTabControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfTabItem">
            <Setter Property="CloseButtonVisibility" Value="Visible"/>
        </Style>
    </syncfusion:SfTabControl.ItemContainerStyle>
    
    <!-- All tabs will have close buttons -->
    <syncfusion:SfTabItem Header="Tab 1" Content="Content 1"/>
    <syncfusion:SfTabItem Header="Tab 2" Content="Content 2"/>
</syncfusion:SfTabControl>
```

### Preventing Last Tab Close

Ensure at least one tab remains:

```csharp
private void OnTabItemClosing(object sender, RoutedEventArgs e)
{
    if (tabControl.Items.Count <= 1)
    {
        e.Handled = true; // Prevent close
        MessageBox.Show("Cannot close the last tab");
    }
}
```

## New Tab Button

Enable the new tab button (+ button) to allow users to dynamically create tabs. This feature is essential for document-centric applications.

### Enabling the New Tab Button

**XAML:**

```xml
<syncfusion:SfTabControl 
    EnableNewTabButton="True"
    NewTabRequested="OnNewTabRequested">
    <syncfusion:SfTabItem Header="Existing Tab" Content="Content"/>
</syncfusion:SfTabControl>
```

**Code-Behind:**

```csharp
// Enable in code
tabControl.EnableNewTabButton = true;
tabControl.NewTabRequested += OnNewTabRequested;

private void OnNewTabRequested(object sender, NewTabRequestedEventArgs e)
{
    // Create new tab
    var newTab = new SfTabItem
    {
        Header = $"Document {tabControl.Items.Count + 1}",
        Content = new TextBlock 
        { 
            Text = $"New document content",
            Margin = new Thickness(20)
        },
        CloseButtonVisibility = Visibility.Visible
    };
    
    // Set the tab to be added
    e.Item = newTab;
}
```

### NewTabRequested Event

The `NewTabRequested` event is fired when:
- User clicks the + button
- User presses Ctrl+T (if implemented)

**Event Arguments:**

```csharp
public class NewTabRequestedEventArgs : EventArgs
{
    // Set this property to the tab item to be added
    public object Item { get; set; }
}
```

### Advanced New Tab Scenarios

**Create Tab with User Input:**

```csharp
private void OnNewTabRequested(object sender, NewTabRequestedEventArgs e)
{
    // Prompt for tab name
    var dialog = new InputDialog("Enter document name:");
    if (dialog.ShowDialog() == true)
    {
        var newTab = new SfTabItem
        {
            Header = dialog.InputText,
            Content = CreateDocumentEditor(),
            CloseButtonVisibility = Visibility.Visible
        };
        e.Item = newTab;
    }
}

private UIElement CreateDocumentEditor()
{
    return new TextBox
    {
        AcceptsReturn = true,
        TextWrapping = TextWrapping.Wrap,
        FontFamily = new FontFamily("Consolas"),
        Margin = new Thickness(10)
    };
}
```

**Create Tab from Template:**

```csharp
private void OnNewTabRequested(object sender, NewTabRequestedEventArgs e)
{
    var viewModel = new DocumentViewModel
    {
        Title = $"Untitled {_documentCounter++}",
        Content = string.Empty,
        IsModified = false
    };
    
    var newTab = new SfTabItem
    {
        Header = viewModel.Title,
        Content = new DocumentView { DataContext = viewModel },
        CloseButtonVisibility = Visibility.Visible,
        Tag = viewModel // Store for later access
    };
    
    e.Item = newTab;
}
```

### Customizing the New Tab Button

Use `NewTabButtonStyle` to customize the appearance:

**XAML:**

```xml
<syncfusion:SfTabControl 
    EnableNewTabButton="True"
    NewTabRequested="OnNewTabRequested">
    
    <syncfusion:SfTabControl.NewTabButtonStyle>
        <Style TargetType="Button">
            <Setter Property="Background" Value="#0078D4"/>
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="BorderBrush" Value="#0078D4"/>
            <Setter Property="BorderThickness" Value="1"/>
            <Setter Property="MinWidth" Value="30"/>
            <Setter Property="MinHeight" Value="30"/>
            <Setter Property="FontSize" Value="16"/>
            <Setter Property="FontWeight" Value="Bold"/>
            <Setter Property="Margin" Value="4,0,0,0"/>
            <Setter Property="Cursor" Value="Hand"/>
            <Setter Property="ToolTip" Value="Add new tab (Ctrl+T)"/>
        </Style>
    </syncfusion:SfTabControl.NewTabButtonStyle>
    
    <syncfusion:SfTabItem Header="Tab 1" Content="Content 1"/>
</syncfusion:SfTabControl>
```

**With Hover Effects:**

```xml
<syncfusion:SfTabControl.NewTabButtonStyle>
    <Style TargetType="Button">
        <Setter Property="Background" Value="Transparent"/>
        <Setter Property="BorderBrush" Value="Gray"/>
        <Setter Property="MinWidth" Value="28"/>
        <Setter Property="MinHeight" Value="28"/>
        <Style.Triggers>
            <Trigger Property="IsMouseOver" Value="True">
                <Setter Property="Background" Value="#E0E0E0"/>
            </Trigger>
            <Trigger Property="IsPressed" Value="True">
                <Setter Property="Background" Value="#C0C0C0"/>
            </Trigger>
        </Style.Triggers>
    </Style>
</syncfusion:SfTabControl.NewTabButtonStyle>
```

### New Tab Button with Icon

Replace the default + with custom content:

```xml
<syncfusion:SfTabControl.NewTabButtonStyle>
    <Style TargetType="Button">
        <Setter Property="Content">
            <Setter.Value>
                <Image Source="/Images/add-icon.png" Width="16" Height="16"/>
            </Setter.Value>
        </Setter>
        <Setter Property="MinWidth" Value="30"/>
        <Setter Property="MinHeight" Value="30"/>
    </Style>
</syncfusion:SfTabControl.NewTabButtonStyle>
```

## Tab Selection

Control which tab is active programmatically or respond to user selections.

### Setting Active Tab

**By Item:**

```csharp
// Select specific tab
tabControl.SelectedItem = myTab;
```

**By Index:**

```csharp
// Select first tab
tabControl.SelectedIndex = 0;

// Select last tab
tabControl.SelectedIndex = tabControl.Items.Count - 1;
```

### Getting Active Tab

```csharp
// Get current tab
var currentTab = tabControl.SelectedItem as SfTabItem;
if (currentTab != null)
{
    string header = currentTab.Header.ToString();
    var content = currentTab.Content;
}

// Get index
int currentIndex = tabControl.SelectedIndex;
```

### Selection Changed Event

Respond to tab changes:

```csharp
tabControl.SelectionChanged += OnTabSelectionChanged;

private void OnTabSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (e.AddedItems.Count > 0)
    {
        var newTab = e.AddedItems[0] as SfTabItem;
        Console.WriteLine($"Switched to: {newTab?.Header}");
    }
    
    if (e.RemovedItems.Count > 0)
    {
        var oldTab = e.RemovedItems[0] as SfTabItem;
        Console.WriteLine($"Left: {oldTab?.Header}");
    }
}
```

### Cycling Through Tabs

Navigate tabs programmatically:

```csharp
// Next tab
private void SelectNextTab()
{
    int current = tabControl.SelectedIndex;
    int next = (current + 1) % tabControl.Items.Count;
    tabControl.SelectedIndex = next;
}

// Previous tab
private void SelectPreviousTab()
{
    int current = tabControl.SelectedIndex;
    int previous = (current - 1 + tabControl.Items.Count) % tabControl.Items.Count;
    tabControl.SelectedIndex = previous;
}
```

## Keyboard Shortcuts

The Tabbed Window supports built-in keyboard shortcuts for efficient tab navigation.

### Built-In Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `Ctrl + Tab` | Next tab | Move to the next tab |
| `Ctrl + Shift + Tab` | Previous tab | Move to the previous tab |
| `Ctrl + T` | New tab | Create new tab (requires implementation) |
| Middle-click | Close tab | Close the clicked tab |

### Implementing Ctrl+T for New Tab

```csharp
// In window constructor or initialization
this.PreviewKeyDown += OnPreviewKeyDown;

private void OnPreviewKeyDown(object sender, KeyEventArgs e)
{
    // Ctrl+T for new tab
    if (e.Key == Key.T && Keyboard.Modifiers == ModifierKeys.Control)
    {
        CreateNewTab();
        e.Handled = true;
    }
    
    // Ctrl+W for close current tab
    if (e.Key == Key.W && Keyboard.Modifiers == ModifierKeys.Control)
    {
        CloseCurrentTab();
        e.Handled = true;
    }
}

private void CreateNewTab()
{
    var args = new NewTabRequestedEventArgs();
    OnNewTabRequested(tabControl, args);
    
    if (args.Item != null)
    {
        tabControl.Items.Add(args.Item);
        tabControl.SelectedItem = args.Item;
    }
}

private void CloseCurrentTab()
{
    var currentTab = tabControl.SelectedItem as SfTabItem;
    if (currentTab != null && currentTab.CloseButtonVisibility == Visibility.Visible)
    {
        tabControl.Items.Remove(currentTab);
    }
}
```

### Custom Keyboard Shortcuts

Add additional shortcuts:

```csharp
private void OnPreviewKeyDown(object sender, KeyEventArgs e)
{
    // Ctrl+1 through Ctrl+9 for direct tab selection
    if (Keyboard.Modifiers == ModifierKeys.Control)
    {
        if (e.Key >= Key.D1 && e.Key <= Key.D9)
        {
            int tabIndex = e.Key - Key.D1;
            if (tabIndex < tabControl.Items.Count)
            {
                tabControl.SelectedIndex = tabIndex;
                e.Handled = true;
            }
        }
    }
    
    // Ctrl+Shift+T to reopen last closed tab
    if (e.Key == Key.T && 
        Keyboard.Modifiers == (ModifierKeys.Control | ModifierKeys.Shift))
    {
        ReopenLastClosedTab();
        e.Handled = true;
    }
}
```

## Mouse Interactions

### Middle-Click to Close

The control supports middle-click (mouse wheel click) to close tabs by default.

**Behavior:**
- Middle-click on tab header closes the tab
- Works only if `CloseButtonVisibility` is `Visible`
- Automatically selects next/previous tab

**No additional code required** - this is built-in functionality.

### Custom Mouse Interactions

Add additional mouse handling:

```csharp
// Double-click to rename tab
private void OnTabMouseDoubleClick(object sender, MouseButtonEventArgs e)
{
    var tabItem = sender as SfTabItem;
    if (tabItem != null)
    {
        RenameTab(tabItem);
    }
}

private void RenameTab(SfTabItem tab)
{
    var dialog = new InputDialog("Enter new name:", tab.Header.ToString());
    if (dialog.ShowDialog() == true)
    {
        tab.Header = dialog.InputText;
    }
}
```

### Right-Click Context Menu

Add context menu to tabs:

```xml
<syncfusion:SfTabItem Header="Document">
    <syncfusion:SfTabItem.ContextMenu>
        <ContextMenu>
            <MenuItem Header="Close" Click="CloseTab_Click"/>
            <MenuItem Header="Close Others" Click="CloseOthers_Click"/>
            <MenuItem Header="Close All" Click="CloseAll_Click"/>
            <Separator/>
            <MenuItem Header="Rename" Click="RenameTab_Click"/>
        </ContextMenu>
    </syncfusion:SfTabItem.ContextMenu>
    <syncfusion:SfTabItem.Content>
        <!-- Content -->
    </syncfusion:SfTabItem.Content>
</syncfusion:SfTabItem>
```

```csharp
private void CloseOthers_Click(object sender, RoutedEventArgs e)
{
    var menuItem = sender as MenuItem;
    var contextMenu = menuItem?.Parent as ContextMenu;
    var targetTab = contextMenu?.PlacementTarget as SfTabItem;
    
    if (targetTab != null)
    {
        var tabsToRemove = tabControl.Items.Cast<SfTabItem>()
            .Where(t => t != targetTab)
            .ToList();
        
        foreach (var tab in tabsToRemove)
        {
            tabControl.Items.Remove(tab);
        }
    }
}
```

## Programmatic Tab Operations

### Adding Tabs

```csharp
// Add simple tab
public void AddSimpleTab(string header, string content)
{
    var tab = new SfTabItem
    {
        Header = header,
        Content = new TextBlock { Text = content, Margin = new Thickness(20) },
        CloseButtonVisibility = Visibility.Visible
    };
    
    tabControl.Items.Add(tab);
    tabControl.SelectedItem = tab;
}

// Add complex tab
public void AddComplexTab(string header, UIElement content)
{
    var tab = new SfTabItem
    {
        Header = header,
        Content = content,
        CloseButtonVisibility = Visibility.Visible
    };
    
    tabControl.Items.Add(tab);
    tabControl.SelectedItem = tab;
}

// Insert at specific position
public void InsertTab(int index, SfTabItem tab)
{
    tabControl.Items.Insert(index, tab);
}
```

### Removing Tabs

```csharp
// Remove current tab
public void RemoveCurrentTab()
{
    var current = tabControl.SelectedItem as SfTabItem;
    if (current != null)
    {
        tabControl.Items.Remove(current);
    }
}

// Remove by index
public void RemoveTabAt(int index)
{
    if (index >= 0 && index < tabControl.Items.Count)
    {
        tabControl.Items.RemoveAt(index);
    }
}

// Remove all closable tabs
public void RemoveAllClosableTabs()
{
    var tabsToRemove = tabControl.Items.Cast<SfTabItem>()
        .Where(t => t.CloseButtonVisibility == Visibility.Visible)
        .ToList();
    
    foreach (var tab in tabsToRemove)
    {
        tabControl.Items.Remove(tab);
    }
}

// Clear all tabs
public void ClearAllTabs()
{
    tabControl.Items.Clear();
}
```

### Finding Tabs

```csharp
// Find by header
public SfTabItem FindTabByHeader(string header)
{
    return tabControl.Items.Cast<SfTabItem>()
        .FirstOrDefault(t => t.Header.ToString() == header);
}

// Find by tag
public SfTabItem FindTabByTag(object tag)
{
    return tabControl.Items.Cast<SfTabItem>()
        .FirstOrDefault(t => t.Tag == tag);
}

// Get all tabs
public List<SfTabItem> GetAllTabs()
{
    return tabControl.Items.Cast<SfTabItem>().ToList();
}
```

## Common Patterns

### Pattern 1: Document Editor with Full Features

```xml
<syncfusion:SfTabControl 
    x:Name="DocumentTabs"
    AllowDragDrop="True"
    EnableNewTabButton="True"
    NewTabRequested="OnNewTabRequested">
    
    <syncfusion:SfTabControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfTabItem">
            <Setter Property="CloseButtonVisibility" Value="Visible"/>
        </Style>
    </syncfusion:SfTabControl.ItemContainerStyle>
    
    <syncfusion:SfTabItem Header="Welcome.txt">
        <TextBox AcceptsReturn="True" FontFamily="Consolas"/>
    </syncfusion:SfTabItem>
</syncfusion:SfTabControl>
```

### Pattern 2: Session Management

```csharp
// Save session
public void SaveSession(string filePath)
{
    var session = new SessionData
    {
        OpenTabs = tabControl.Items.Cast<SfTabItem>()
            .Select(t => new TabData
            {
                Header = t.Header.ToString(),
                Content = GetTabContent(t),
                IsSelected = t.IsSelected
            })
            .ToList()
    };
    
    File.WriteAllText(filePath, JsonSerializer.Serialize(session));
}

// Restore session
public void RestoreSession(string filePath)
{
    var json = File.ReadAllText(filePath);
    var session = JsonSerializer.Deserialize<SessionData>(json);
    
    tabControl.Items.Clear();
    
    foreach (var tabData in session.OpenTabs)
    {
        var tab = new SfTabItem
        {
            Header = tabData.Header,
            Content = RestoreTabContent(tabData),
            CloseButtonVisibility = Visibility.Visible
        };
        
        tabControl.Items.Add(tab);
        
        if (tabData.IsSelected)
        {
            tabControl.SelectedItem = tab;
        }
    }
}
```

### Pattern 3: Modified Tab Indicator

```csharp
// Mark tab as modified
public void MarkTabModified(SfTabItem tab, bool isModified)
{
    var header = tab.Header.ToString();
    
    if (isModified && !header.EndsWith("*"))
    {
        tab.Header = header + "*";
    }
    else if (!isModified && header.EndsWith("*"))
    {
        tab.Header = header.TrimEnd('*');
    }
}
```

## Edge Cases and Gotchas

### Closing Last Tab

**Problem:** Closing the last tab may leave an empty window

**Solution:** Prevent or handle gracefully:

```csharp
private void OnTabClosing(object sender, RoutedEventArgs e)
{
    if (tabControl.Items.Count == 1)
    {
        // Option 1: Prevent close
        e.Handled = true;
        
        // Option 2: Close window
        this.Close();
        
        // Option 3: Show welcome screen
        ShowWelcomeScreen();
    }
}
```

### Tab Selection After Remove

**Issue:** When removing the selected tab, ensure another tab is selected

**Solution:** Control is handled automatically, but you can customize:

```csharp
private void RemoveTabSafely(SfTabItem tab)
{
    int index = tabControl.Items.IndexOf(tab);
    tabControl.Items.Remove(tab);
    
    // Manually select adjacent tab if needed
    if (tabControl.SelectedItem == null && tabControl.Items.Count > 0)
    {
        int newIndex = Math.Min(index, tabControl.Items.Count - 1);
        tabControl.SelectedIndex = newIndex;
    }
}
```

### Memory Leaks with Complex Content

**Issue:** Tab content not properly disposed

**Solution:** Clean up when removing tabs:

```csharp
private void RemoveTabWithCleanup(SfTabItem tab)
{
    // Dispose content if implements IDisposable
    if (tab.Content is IDisposable disposable)
    {
        disposable.Dispose();
    }
    
    // Clear references
    tab.Content = null;
    tab.Tag = null;
    
    // Remove from collection
    tabControl.Items.Remove(tab);
}
```

### NewTabRequested Not Firing

**Issue:** New tab button doesn't create tabs

**Solution:** Ensure event is subscribed and Item is set:

```csharp
// Always set e.Item
private void OnNewTabRequested(object sender, NewTabRequestedEventArgs e)
{
    e.Item = new SfTabItem { Header = "New Tab", Content = "Content" };
    // Without setting e.Item, nothing happens!
}
```

This comprehensive guide covers all aspects of tab management in the Syncfusion WPF Tabbed Window. Combine these features to create powerful, user-friendly tabbed interfaces!

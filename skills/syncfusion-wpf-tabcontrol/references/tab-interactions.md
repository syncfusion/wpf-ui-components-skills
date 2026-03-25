# Tab Interactions and Selection

Learn how to handle tab selection, keyboard navigation, and user interactions with TabControl items.

## Tab Selection Basics

By default, the first TabItem is automatically selected when the TabControl loads. You can change the selected tab programmatically or through user interaction.

### Getting the Selected Item
```csharp
// Get the currently selected TabItem
TabItemExt selectedTab = tabControl.SelectedItem as TabItemExt;

// Access the selected tab's header
string headerText = selectedTab?.Header?.ToString();
```

### Programmatically Setting Selection
```csharp
// Select a specific TabItem by reference
tabControl.SelectedItem = tabItem2;

// Select by index
tabControl.SelectedIndex = 1;  // Selects the second tab (0-based indexing)
```

## Selection Changed Event

Handle the `SelectedItemChangedEvent` to respond when the user changes the selected tab:

### XAML Declaration
```xml
<syncfusion:TabControlExt SelectedItemChangedEvent="TabControl_SelectedItemChanged" 
                          Name="tabControl" />
```

### Event Handler
```csharp
private void TabControl_SelectedItemChanged(object sender, SelectedItemChangedEventArgs e)
{
    // Get the newly selected item
    TabItemExt newTab = e.NewSelectedItem as TabItemExt;
    string newHeader = newTab?.Header?.ToString() ?? "Unknown";
    
    // Get the previously selected item (may be null on first selection)
    TabItemExt oldTab = e.OldSelectedItem as TabItemExt;
    string oldHeader = oldTab?.Header?.ToString() ?? "None";
    
    MessageBox.Show($"Tab changed from '{oldHeader}' to '{newHeader}'");
}
```

### Advanced Event Handling
```csharp
private void TabControl_SelectedItemChanged(object sender, SelectedItemChangedEventArgs e)
{
    if (e.NewSelectedItem != null)
    {
        // Perform cleanup on old tab
        if (e.OldSelectedItem != null)
        {
            SaveTabState(e.OldSelectedItem as TabItemExt);
        }
        
        // Initialize new tab
        LoadTabContent(e.NewSelectedItem as TabItemExt);
    }
}

private void SaveTabState(TabItemExt tab)
{
    // Save any unsaved data from the tab
}

private void LoadTabContent(TabItemExt tab)
{
    // Load content for the newly selected tab
}
```

## Keyboard Navigation

The TabControl supports keyboard shortcuts for tab navigation:

### Built-in Keyboard Shortcuts
| Keyboard Shortcut | Action |
|-------------------|--------|
| `Ctrl + Tab` | Select next tab (when control not focused) |
| `Ctrl + Shift + Tab` | Select previous tab (when control not focused) |
| `Left Arrow` | Select previous tab (when control focused) |
| `Right Arrow` | Select next tab (when control focused) |
| `Up Arrow` | Select previous tab (vertical orientation) |
| `Down Arrow` | Select next tab (vertical orientation) |

### Example: Custom Keyboard Handling
```csharp
public partial class MainWindow : Window {
    public MainWindow() {
        InitializeComponent();
        
        this.KeyDown += MainWindow_KeyDown;
    }
    
    private void MainWindow_KeyDown(object sender, KeyEventArgs e)
    {
        if (e.Key == Key.Tab && Keyboard.Modifiers == ModifierKeys.Control)
        {
            // Ctrl+Tab: move to next tab
            int nextIndex = (tabControl.SelectedIndex + 1) % tabControl.Items.Count;
            tabControl.SelectedIndex = nextIndex;
            e.Handled = true;
        }
    }
}
```

## Tab List Context Menu

The TabControl includes a built-in tab list menu that appears in the top-right corner, allowing quick navigation to any tab.

### Enable/Disable Tab List Menu
```xml
<!-- Enable (default is true) -->
<syncfusion:TabControlExt ShowTabListContextMenu="True" Name="tabControl" />

<!-- Disable -->
<syncfusion:TabControlExt ShowTabListContextMenu="False" Name="tabControl" />
```

```csharp
// In C#
tabControl.ShowTabListContextMenu = true;
```

### Show Hidden Items in Tab List
By default, only visible tabs are shown in the list. To include hidden tabs:

```xml
<syncfusion:TabControlExt ShowTabListContextMenu="True"
                          TabListContextMenuOptions="Default, ShowHiddenItems"
                          Name="tabControl" />
```

```csharp
// In C#
tabControl.ShowTabListContextMenu = true;
tabControl.TabListContextMenuOptions = TabListContextMenuOptions.Default | 
                                       TabListContextMenuOptions.ShowHiddenItems;
```

### Handling Tab List Selection
The selection is handled automatically by the TabControl. When a user clicks a tab in the list menu, the TabControl navigates to that tab and fires the `SelectedItemChangedEvent`.

## Manually Navigating Through Tabs

```csharp
// Get current index
int currentIndex = tabControl.SelectedIndex;

// Move to next tab
if (currentIndex < tabControl.Items.Count - 1)
{
    tabControl.SelectedIndex = currentIndex + 1;
}

// Move to previous tab
if (currentIndex > 0)
{
    tabControl.SelectedIndex = currentIndex - 1;
}

// Move to first tab
tabControl.SelectedIndex = 0;

// Move to last tab
tabControl.SelectedIndex = tabControl.Items.Count - 1;
```

## Tab Visibility and Interaction

### Hiding Tabs
```xml
<syncfusion:TabItemExt Header="Hidden Tab" Visibility="Collapsed" />
```

```csharp
tabItem.Visibility = Visibility.Collapsed;
```

### Disabling Tab Interaction
```xml
<syncfusion:TabItemExt Header="Disabled Tab" IsEnabled="False" />
```

```csharp
tabItem.IsEnabled = false;
```

### Showing Hidden Tabs in Menu
When tabs are hidden, they can optionally appear in the tab list context menu:

```xml
<syncfusion:TabControlExt TabListContextMenuOptions="Default, ShowHiddenItems, ShowDisabledItems"
                          ShowTabListContextMenu="True"
                          Name="tabControl" />
```

## Tab Selection Patterns

### Pattern 1: Tab-Based Navigation in MVVM
```csharp
// ViewModel
public class MainViewModel : INotifyPropertyChanged
{
    private int _selectedTabIndex = 0;
    
    public int SelectedTabIndex
    {
        get { return _selectedTabIndex; }
        set
        {
            if (_selectedTabIndex != value)
            {
                _selectedTabIndex = value;
                OnPropertyChanged(nameof(SelectedTabIndex));
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

// XAML
<syncfusion:TabControlExt SelectedIndex="{Binding SelectedTabIndex}" />
```

### Pattern 2: Validating Before Tab Switch
```csharp
private void TabControl_SelectedItemChanged(object sender, SelectedItemChangedEventArgs e)
{
    if (e.OldSelectedItem != null)
    {
        // Validate old tab before switching
        if (!ValidateTabContent(e.OldSelectedItem as TabItemExt))
        {
            MessageBox.Show("Please fix errors before switching tabs");
            tabControl.SelectedItem = e.OldSelectedItem;
            return;
        }
    }
    
    // Switch is valid, proceed
}

private bool ValidateTabContent(TabItemExt tab)
{
    // Implement validation logic
    return true;
}
```

### Pattern 3: Dynamic Tab Activation Based on Data
```csharp
public void NavigateToTab(string tabName)
{
    foreach (TabItemExt item in tabControl.Items)
    {
        if (item.Header?.ToString() == tabName)
        {
            tabControl.SelectedItem = item;
            return;
        }
    }
}
```

## Best Practices

1. **Always check for null**: `OldSelectedItem` can be null on initial selection
2. **Keyboard Support**: Test keyboard navigation in your application
3. **Visual Feedback**: Provide clear indication of the active tab
4. **Validation**: Validate tab content before allowing switches if needed
5. **Performance**: Avoid heavy operations in `SelectedItemChangedEvent` to maintain responsiveness
6. **Accessibility**: Ensure tab headers are descriptive for screen readers

## Common Issues

**Issue: SelectedItemChangedEvent not firing**
- Ensure the event is properly wired in XAML or C#
- Verify the event handler signature matches exactly
- Check that tabs are actually being changed

**Issue: Keyboard navigation not working**
- Verify that the TabControl or its parent has focus
- Check that no other controls are intercepting keyboard events
- Try using `Ctrl+Tab` if arrow keys are not responding

**Issue: Tab list menu not appearing**
- Ensure `ShowTabListContextMenu` is set to `true`
- Verify the TabControl has sufficient width to display the menu button
- Check that multiple tabs exist

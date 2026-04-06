---
name: syncfusion-wpf-tabbedwindow
description: Comprehensive guide for implementing Syncfusion WPF Tabbed Window control that combines SfChromelessWindow with SfTabControl for document-based applications. Use this skill when implementing chromeless tabbed windows, browser-style tabs, document management windows, or Visual Studio-style tabs in WPF. Covers tear-off tabs, floating tab windows, tab merging between windows, drag-drop tab reordering, and MVVM tab binding scenarios.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Tabbed Windows

A comprehensive guide for implementing the Syncfusion WPF Tabbed Window control, which combines `SfChromelessWindow` with `SfTabControl` to create professional document-based applications with advanced tab management, tear-off windows, and cross-window tab merging capabilities.

## When to Use This Skill

Use this skill for document-based applications with multiple tabs, browser-style interfaces, tear-off windows, tab merging, and MVVM binding. Covers setup, window modes, tab management, data binding, floating windows, and customization.

## Component Overview

The **Tabbed Window** is a powerful combination of `SfChromelessWindow` and `SfTabControl` that enables professional document-based applications with:

- **Drag-and-Drop Reordering** - Intuitive tab reordering within the tab bar or between windows
- **Tear-Off Windows** - Create floating windows by dragging tabs outside window boundaries
- **Tab Merging** - Move tabs between multiple TabbedWindow instances with validation
- **Flexible Tab Management** - Dynamic add, remove, and select operations with close/new tab buttons
- **MVVM Support** - Full data binding through ItemsSource for data-driven scenarios
- **Window Type Modes** - Tabbed mode (chrome-integrated) or Normal mode (content-based)
- **Content Customization** - Templates for headers, icons, and content
- **Event-Driven Architecture** - Comprehensive events for all tab operations

**Key Assemblies Required:**
- `Syncfusion.SfChromelessWindow.WPF`
- `Syncfusion.Shared.WPF`

**Namespace:**
```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install and reference required assemblies
- Create your first tabbed window in XAML or code-behind
- Understand Window Type Modes (Tabbed vs Normal)
- Choose the right mode for your application
- Create basic tab items
- Configure essential properties

**Key Topics:** Installation, basic window setup, `WindowType` property, Tabbed vs Normal mode comparison, assembly references, namespace imports

---

### Tab Management
📄 **Read:** [references/tab-management.md](references/tab-management.md)

When you need to:
- Add close buttons to tabs
- Enable the new tab (+) button
- Handle NewTabRequested event for dynamic tab creation
- Customize the new tab button appearance
- Implement keyboard shortcuts (Ctrl+Tab, Ctrl+Shift+Tab, Ctrl+T)
- Support middle-click to close tabs
- Programmatically add, remove, or select tabs

**Key Topics:** `CloseButtonVisibility`, `EnableNewTabButton`, `NewTabRequested` event, `NewTabButtonStyle`, keyboard shortcuts, tab selection

---

### Data Binding (MVVM)
📄 **Read:** [references/data-binding.md](references/data-binding.md)

When you need to:
- Bind tabs to a ViewModel collection
- Implement MVVM pattern with tabbed windows
- Use `ItemsSource` for data-driven tab scenarios
- Create tab models and ViewModels
- Customize tab headers and content through templates
- Configure `ItemTemplate` and `ContentTemplate`
- Use `ItemContainerStyle` for tab styling
- Handle dynamic collection updates with ObservableCollection

**Key Topics:** MVVM pattern, `ItemsSource` binding, `ItemTemplate`, `ContentTemplate`, `ItemContainerStyle`, ObservableCollection, tab models

---

### Tab Merging and Floating Windows
📄 **Read:** [references/merge-tabs.md](references/merge-tabs.md)

When you need to:
- Enable tear-off functionality to create floating windows
- Understand how tabs become independent windows
- Reattach floating tabs back to main window
- Merge tabs between multiple windows
- Validate merge operations with `PreviewTabMerge` event
- Control which tabs can be moved
- Transform items during merge operations
- Handle multi-window workspace scenarios

**Key Topics:** Tear-off windows, drag-outside behavior, floating window lifecycle, `PreviewTabMerge` event, `TabMergePreviewEventArgs`, cross-window merging, merge validation

---

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)

When you need to:
- Apply Syncfusion themes
- Customize window chrome appearance
- Style tab headers and content areas
- Create custom header and content templates
- Customize close button appearance
- Configure drag-drop visual feedback
- Set colors, fonts, and spacing
- Implement responsive design
- Follow best practices for professional appearance

**Key Topics:** Theme application, style customization, template customization, visual appearance, `AllowDragDrop`, responsive design, professional styling

---

## Quick Start Example

### Basic Tabbed Window with Close Buttons

**XAML:**
```xml
<Window x:Class="TabbedWindowApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    
    <syncfusion:SfChromelessWindow 
        Title="Document Manager"
        WindowType="Tabbed"
        Height="600" 
        Width="900">
        
        <syncfusion:SfTabControl 
            AllowDragDrop="True"
            EnableNewTabButton="True"
            NewTabRequested="OnNewTabRequested">
            
            <syncfusion:SfTabItem 
                Header="Document 1" 
                CloseButtonVisibility="Visible">
                <TextBlock Text="Welcome to Document 1" 
                          Margin="20"/>
            </syncfusion:SfTabItem>
            
            <syncfusion:SfTabItem 
                Header="Document 2" 
                CloseButtonVisibility="Visible">
                <TextBlock Text="Welcome to Document 2" 
                          Margin="20"/>
            </syncfusion:SfTabItem>
            
        </syncfusion:SfTabControl>
        
    </syncfusion:SfChromelessWindow>
    
</Window>
```

**Code-Behind:**
```csharp
using Syncfusion.Windows.Controls;

public partial class MainWindow : SfChromelessWindow
{
    public MainWindow()
    {
        InitializeComponent();
    }

    private void OnNewTabRequested(object sender, NewTabRequestedEventArgs e)
    {
        var tabCount = ((SfTabControl)sender).Items.Count;
        var newTab = new SfTabItem 
        { 
            Header = $"Document {tabCount + 1}",
            Content = new TextBlock 
            { 
                Text = $"New Document {tabCount + 1}",
                Margin = new Thickness(20)
            },
            CloseButtonVisibility = Visibility.Visible
        };
        
        e.Item = newTab;
    }
}
```

## Common Patterns

### Pattern 1: Document Editor with Drag-Drop and New Tab

```xml
<syncfusion:SfChromelessWindow WindowType="Tabbed">
    <syncfusion:SfTabControl 
        AllowDragDrop="True"
        EnableNewTabButton="True"
        NewTabRequested="OnNewTabRequested">
        <!-- Tabs here -->
    </syncfusion:SfTabControl>
</syncfusion:SfChromelessWindow>
```

### Pattern 2: MVVM Data-Bound Tabs

```xml
<syncfusion:SfTabControl ItemsSource="{Binding OpenDocuments}">
    <syncfusion:SfTabControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfTabItem">
            <Setter Property="HeaderTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <TextBlock Text="{Binding Title}" />
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfTabControl.ItemContainerStyle>
    
    <syncfusion:SfTabControl.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Content}" />
        </DataTemplate>
    </syncfusion:SfTabControl.ContentTemplate>
</syncfusion:SfTabControl>
```

## Key Properties and Events

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| `WindowType` | WindowType | `Tabbed` (chrome-integrated) or `Normal` (content-based) |
| `AllowDragDrop` | bool | Enable drag-drop reordering and tear-off |
| `EnableNewTabButton` | bool | Show/hide the new tab (+) button |
| `SelectedItem` | object | Currently active tab |
| `SelectedIndex` | int | Index of active tab |
| `ItemsSource` | IEnumerable | Bind tabs to a collection (MVVM) |
| `CloseButtonVisibility` | Visibility | Show/hide close button per tab |

### Important Events

| Event | Description |
|-------|-------------|
| `NewTabRequested` | Fired when user clicks the + button; set `e.Item` to create tab |
| `PreviewTabMerge` | Fired before tab moves between windows; set `e.Allow` to validate |
| `SelectionChanged` | Fired when active tab changes |

### Styling Properties

| Property | Description |
|----------|-------------|
| `NewTabButtonStyle` | Style for the new tab button |
| `ItemTemplate` | DataTemplate for tab headers |
| `ContentTemplate` | DataTemplate for tab content |
| `ItemContainerStyle` | Style for SfTabItem containers |



## Best Practices

1. **Choose the Right Mode:** Use Tabbed mode for document-centric apps, Normal mode when tabs are secondary navigation
2. **Enable Drag-Drop:** Set `AllowDragDrop="True"` for intuitive reordering and tear-off functionality
3. **Handle NewTabRequested:** Always implement this event handler when `EnableNewTabButton="True"`
4. **MVVM Pattern:** Use `ItemsSource` binding for data-driven scenarios with clean separation of concerns
5. **Validate Merges:** Use `PreviewTabMerge` to control which tabs can be moved between windows
6. **Keyboard Support:** Document keyboard shortcuts for users (Ctrl+Tab, Ctrl+Shift+Tab, Ctrl+T)
7. **Visual Feedback:** Ensure drag-drop operations have clear visual indicators for user guidance



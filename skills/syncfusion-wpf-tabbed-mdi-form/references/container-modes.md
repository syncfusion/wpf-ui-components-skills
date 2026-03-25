# Container Modes: TDI vs MDI

## Table of Contents
- [Overview](#overview)
- [TDI - Tabbed Document Interface](#tdi---tabbed-document-interface)
- [MDI - Multiple Document Interface](#mdi---multiple-document-interface)
- [When to Use TDI vs MDI](#when-to-use-tdi-vs-mdi)
- [Setting the Mode Property](#setting-the-mode-property)
- [Mode-Specific Features](#mode-specific-features)
- [Switching Modes at Runtime](#switching-modes-at-runtime)
- [Visual Comparison](#visual-comparison)

## Overview

The DocumentContainer supports two distinct interface modes that fundamentally change how documents are displayed and managed:

- **TDI (Tabbed Document Interface):** Documents appear as tabs in a tab strip, similar to modern web browsers or Visual Studio Code
- **MDI (Multiple Document Interface):** Documents appear as independent floating windows within a container, similar to classic Windows applications or older versions of Visual Studio

## TDI - Tabbed Document Interface

### What is TDI?

TDI displays all documents as tabs in a horizontal tab strip. Only one document is visible at a time, and users switch between documents by clicking tabs or using keyboard shortcuts (CTRL+TAB).

### TDI Characteristics

**Visual Layout:**
- Tab strip at the top showing all document headers
- Single content area displaying the active document
- Close buttons on tabs (optional)
- Tab overflow handling for many documents

**Behavior:**
- Only one document visible at a time
- Fast switching between documents
- Tabs can be reordered by dragging
- Support for tab groups (split view)
- Pin/unpin tabs for persistent placement

**Best For:**
- Modern applications with clean, simple interfaces
- Applications where users work on one document at a time
- Limited screen space scenarios
- Touch-friendly interfaces

### TDI Example

```xaml
<syncfusion:DocumentContainer Name="DocContainer" Mode="TDI">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 1">
        <FlowDocument>
            <Paragraph>TDI Document 1 Content</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 2">
        <FlowDocument>
            <Paragraph>TDI Document 2 Content</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 3">
        <FlowDocument>
            <Paragraph>TDI Document 3 Content</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
DocumentContainer docContainer = new DocumentContainer();
docContainer.Mode = DocumentContainerMode.TDI;

// Add documents
Button doc1 = new Button { Content = "Document 1" };
DocumentContainer.SetHeader(doc1, "Document 1");
docContainer.Items.Add(doc1);

Button doc2 = new Button { Content = "Document 2" };
DocumentContainer.SetHeader(doc2, "Document 2");
docContainer.Items.Add(doc2);
```

## MDI - Multiple Document Interface

### What is MDI?

MDI displays each document as an independent floating window within the container. Users can resize, move, minimize, and maximize windows independently, creating a more flexible but potentially more complex interface.

### MDI Characteristics

**Visual Layout:**
- Multiple visible windows simultaneously
- Each window has its own title bar, borders, and window controls
- Windows can overlap and be arranged freely
- Minimized windows appear in the bottom-left corner

**Behavior:**
- Multiple documents visible simultaneously
- Windows can be moved, resized, minimized, maximized
- Z-order management (bring to front/send to back)
- Cascade, tile, and arrange window options
- Full keyboard navigation support

**Best For:**
- Applications where users compare multiple documents side-by-side
- Complex workflows requiring simultaneous document viewing
- Power users who need flexible window arrangements
- Applications with a traditional desktop feel

### MDI Example

```xaml
<syncfusion:DocumentContainer Name="DocContainer" 
                              Mode="MDI"
                              CanMDIMinimize="True">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Window 1">
        <FlowDocument>
            <Paragraph>MDI Window 1 Content</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Window 2">
        <FlowDocument>
            <Paragraph>MDI Window 2 Content</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Window 3">
        <FlowDocument>
            <Paragraph>MDI Window 3 Content</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

**C# Code:**
```csharp
DocumentContainer docContainer = new DocumentContainer();
docContainer.Mode = DocumentContainerMode.MDI;
docContainer.CanMDIMinimize = true;

// Add documents
Button doc1 = new Button { Content = "Window 1" };
DocumentContainer.SetHeader(doc1, "Window 1");
docContainer.Items.Add(doc1);

Button doc2 = new Button { Content = "Window 2" };
DocumentContainer.SetHeader(doc2, "Window 2");
docContainer.Items.Add(doc2);
```

## When to Use TDI vs MDI

### Use TDI When:

✅ **Modern UI design:** Building contemporary applications with clean, minimal interfaces  
✅ **Single-focus workflow:** Users primarily work on one document at a time  
✅ **Limited screen space:** Mobile devices, laptops, or compact application windows  
✅ **Simple navigation:** Users need to quickly switch between documents without complexity  
✅ **Touch interfaces:** Applications designed for touch input  
✅ **Consistency with modern apps:** Matching the behavior of browsers, VS Code, modern IDEs  

**Example Scenarios:**
- Code editors with tabbed file navigation
- Web browsers with multiple pages
- Configuration tools with category tabs
- Document viewers with multiple files

### Use MDI When:

✅ **Side-by-side comparison:** Users need to view multiple documents simultaneously  
✅ **Complex workflows:** Power users require flexible document arrangements  
✅ **Data analysis:** Comparing charts, tables, or reports side-by-side  
✅ **Traditional applications:** Maintaining consistency with legacy desktop applications  
✅ **Power user features:** Advanced users who benefit from manual window management  

**Example Scenarios:**
- Financial applications comparing multiple reports
- CAD/design tools with multiple views
- Medical imaging applications
- Data analysis dashboards
- Classic desktop applications (Word, Excel style)

### Decision Matrix

| Factor | TDI | MDI |
|--------|-----|-----|
| Screen real estate | Efficient | Requires more space |
| Learning curve | Easy | Moderate |
| Modern feel | High | Low |
| Flexibility | Limited | High |
| Touch-friendly | Yes | No |
| Multi-document viewing | No | Yes |
| Visual complexity | Low | High |

## Setting the Mode Property

### XAML Configuration

```xaml
<!-- TDI Mode -->
<syncfusion:DocumentContainer Name="DocContainer" Mode="TDI">
    <!-- Documents here -->
</syncfusion:DocumentContainer>

<!-- MDI Mode -->
<syncfusion:DocumentContainer Name="DocContainer" Mode="MDI">
    <!-- Documents here -->
</syncfusion:DocumentContainer>
```

### C# Configuration

```csharp
// Create container
DocumentContainer docContainer = new DocumentContainer();

// Set TDI mode
docContainer.Mode = DocumentContainerMode.TDI;

// Or set MDI mode
docContainer.Mode = DocumentContainerMode.MDI;
```

### Mode Enum Values

```csharp
public enum DocumentContainerMode
{
    TDI,  // Tabbed Document Interface
    MDI   // Multiple Document Interface
}
```

## Mode-Specific Features

### TDI-Specific Features

**Tab Management:**
- Tab reordering via drag-and-drop
- Close buttons on tabs
- Tab overflow scrolling
- Pin/unpin tabs
- Tab groups for split views

**Example:**
```xaml
<syncfusion:DocumentContainer Mode="TDI" 
                              SwitchMode="QuickTabs"
                              ShowTDICloseButton="True">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

### MDI-Specific Features

**Window Management:**
- Minimize, maximize, restore windows
- Move and resize windows independently
- Set MDI bounds for window positioning
- Window state management
- Cascade and tile arrangements

**Example:**
```xaml
<syncfusion:DocumentContainer Mode="MDI" 
                              CanMDIMinimize="True"
                              SwitchMode="VS2005">
    <!-- Windows -->
</syncfusion:DocumentContainer>
```

## Switching Modes at Runtime

You can change the container mode dynamically based on user preferences or application state:

```csharp
// Toggle between TDI and MDI
private void ToggleMode_Click(object sender, RoutedEventArgs e)
{
    if (documentContainer.Mode == DocumentContainerMode.TDI)
    {
        documentContainer.Mode = DocumentContainerMode.MDI;
        documentContainer.CanMDIMinimize = true;
    }
    else
    {
        documentContainer.Mode = DocumentContainerMode.TDI;
    }
}

// Let user choose mode
private void SetModeFromComboBox(object sender, SelectionChangedEventArgs e)
{
    ComboBoxItem selected = (ComboBoxItem)modeComboBox.SelectedItem;
    string mode = selected.Content.ToString();
    
    if (mode == "Tabbed (TDI)")
    {
        documentContainer.Mode = DocumentContainerMode.TDI;
    }
    else if (mode == "Windowed (MDI)")
    {
        documentContainer.Mode = DocumentContainerMode.MDI;
        documentContainer.CanMDIMinimize = true;
    }
}
```

**XAML for mode switcher:**
```xaml
<StackPanel Orientation="Horizontal" Margin="10">
    <Label Content="Display Mode:" VerticalAlignment="Center"/>
    <ComboBox x:Name="modeComboBox" 
              Width="150"
              SelectionChanged="SetModeFromComboBox">
        <ComboBoxItem Content="Tabbed (TDI)" IsSelected="True"/>
        <ComboBoxItem Content="Windowed (MDI)"/>
    </ComboBox>
</StackPanel>
```

## Visual Comparison

### TDI Appearance
- Clean tab strip at top
- Single document visible
- Minimal visual clutter
- No window borders or title bars
- Modern, streamlined look

### MDI Appearance
- Individual windows with title bars
- Multiple documents visible simultaneously
- Window borders and controls (min/max/close)
- Can overlap and be arranged
- Traditional desktop application look

## Best Practices

### For TDI Mode:
1. **Limit the number of tabs:** Too many tabs creates scrolling and navigation issues
2. **Use meaningful tab headers:** Keep them short but descriptive
3. **Consider tab groups:** For related documents that need side-by-side viewing
4. **Implement proper tab closing:** Handle unsaved changes before closing tabs
5. **Use QuickTabs switcher:** Provides the best navigation experience for TDI

### For MDI Mode:
1. **Enable minimize feature:** Set `CanMDIMinimize="True"` for better window management
2. **Set initial window positions:** Use MDIBounds to control initial window placement
3. **Provide window arrangement commands:** Cascade, tile horizontal/vertical options
4. **Handle window z-order:** Allow users to bring windows to front easily
5. **Use VS2005 switcher:** Best matches MDI user expectations

## Common Scenarios

### Scenario 1: Code Editor (Use TDI)
```csharp
documentContainer.Mode = DocumentContainerMode.TDI;
documentContainer.SwitchMode = SwitchMode.QuickTabs;
// Add code file tabs
```

### Scenario 2: Financial Dashboard (Use MDI)
```csharp
documentContainer.Mode = DocumentContainerMode.MDI;
documentContainer.CanMDIMinimize = true;
documentContainer.SwitchMode = SwitchMode.VS2005;
// Add charts and reports as windows
```

### Scenario 3: User-Configurable (Allow switching)
```csharp
// Save user preference
Settings.Default.PreferredMode = documentContainer.Mode;
Settings.Default.Save();

// Restore on startup
documentContainer.Mode = Settings.Default.PreferredMode;
```

## Related Documentation

- **Window Management:** See `window-management.md` for MDI-specific window operations
- **Tab Management:** See `tab-management.md` for TDI-specific tab operations
- **Window Switchers:** See `window-switchers.md` for navigation in both modes

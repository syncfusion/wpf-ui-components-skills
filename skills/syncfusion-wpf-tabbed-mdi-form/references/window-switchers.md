# Window Switchers and Navigation

## Table of Contents
- [Overview](#overview)
- [CTRL+TAB Navigation](#ctrltab-navigation)
- [Switcher Modes](#switcher-modes)
- [Configuring Switcher Mode](#configuring-switcher-mode)
- [Mode Comparison](#mode-comparison)
- [Choosing the Right Switcher](#choosing-the-right-switcher)
- [Customizing Switcher Behavior](#customizing-switcher-behavior)

## Overview

Window switchers enable users to navigate between documents using keyboard shortcuts, primarily CTRL+TAB. The DocumentContainer supports five different switcher modes, each providing a unique navigation experience suitable for different application types and user preferences.

**Key Feature:** Press CTRL+TAB to activate the window switcher and quickly navigate between documents without using the mouse.

## CTRL+TAB Navigation

### Basic Usage

1. **Hold CTRL** and **press TAB** to activate the switcher
2. **Keep holding CTRL** and **press TAB repeatedly** to cycle through documents
3. **Release CTRL** to switch to the selected document

### Additional Shortcuts

- **CTRL+SHIFT+TAB:** Cycle backward through documents
- **CTRL+TAB (hold):** Keep switcher open for visual selection
- **Arrow keys:** Navigate within the switcher (mode-dependent)
- **ESC:** Cancel and return to current document

## Switcher Modes

The DocumentContainer provides five window switcher modes through the `SwitchMode` property:

### 1. Immediate Mode

**Behavior:**
- Switches instantly to the next document
- No visual switcher interface appears
- Fastest but provides no preview

**Best For:**
- Users who memorize document order
- Applications with few documents
- Users who prefer minimal UI interruption

**Characteristics:**
- No visual feedback
- Instant switching
- Simplest mode

### 2. List Mode

**Behavior:**
- Displays a list of all documents
- Shows document headers in a vertical list
- Highlights the next document as you press TAB

**Best For:**
- Applications with 5-15 documents
- Users who prefer text-based navigation
- Quick scanning of document names

**Characteristics:**
- Clean, minimal interface
- Text-only display
- Compact vertical list

### 3. QuickTabs Mode

**Behavior:**
- Shows thumbnail previews of documents
- Displays in a horizontal strip
- Visual preview helps identify documents quickly

**Best For:**
- Modern applications (recommended for TDI)
- Users who prefer visual navigation
- Applications with visual content (editors, viewers)

**Characteristics:**
- Thumbnail previews
- Modern, polished look
- Intuitive for visual thinkers

### 4. VS2005 Mode

**Behavior:**
- Mimics Visual Studio 2005's switcher
- Shows a list with icons and document details
- Traditional IDE-style navigation

**Best For:**
- IDE-like applications
- MDI applications (recommended for MDI)
- Users familiar with Visual Studio
- Professional development tools

**Characteristics:**
- Icon + text display
- Detailed document information
- Classic IDE appearance

### 5. VistaFlip Mode

**Behavior:**
- 3D flip animation between documents
- Similar to Windows Vista's Alt+Tab
- Visually impressive but slower

**Best For:**
- Applications prioritizing visual appeal
- Presentations or demos
- Users who enjoy animated transitions

**Characteristics:**
- 3D flip animation
- Visually striking
- Slower than other modes

## Configuring Switcher Mode

### XAML Configuration

```xaml
<!-- QuickTabs Mode (Recommended for TDI) -->
<syncfusion:DocumentContainer Name="DocContainer" 
                              Mode="TDI"
                              SwitchMode="QuickTabs">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 1">
        <FlowDocument><Paragraph>Content 1</Paragraph></FlowDocument>
    </FlowDocumentScrollViewer>
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 2">
        <FlowDocument><Paragraph>Content 2</Paragraph></FlowDocument>
    </FlowDocumentScrollViewer>
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 3">
        <FlowDocument><Paragraph>Content 3</Paragraph></FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>

<!-- VS2005 Mode (Recommended for MDI) -->
<syncfusion:DocumentContainer Name="DocContainer" 
                              Mode="MDI"
                              SwitchMode="VS2005"
                              CanMDIMinimize="True">
    <!-- Documents here -->
</syncfusion:DocumentContainer>

<!-- Immediate Mode (No UI) -->
<syncfusion:DocumentContainer Name="DocContainer" 
                              Mode="TDI"
                              SwitchMode="Immediate">
    <!-- Documents here -->
</syncfusion:DocumentContainer>

<!-- List Mode -->
<syncfusion:DocumentContainer Name="DocContainer" 
                              Mode="TDI"
                              SwitchMode="List">
    <!-- Documents here -->
</syncfusion:DocumentContainer>

<!-- VistaFlip Mode -->
<syncfusion:DocumentContainer Name="DocContainer" 
                              Mode="TDI"
                              SwitchMode="VistaFlip">
    <!-- Documents here -->
</syncfusion:DocumentContainer>
```

### C# Configuration

```csharp
// Create DocumentContainer
DocumentContainer docContainer = new DocumentContainer();
docContainer.Mode = DocumentContainerMode.TDI;

// Set switcher mode
docContainer.SwitchMode = SwitchMode.QuickTabs;  // Recommended for TDI

// Or use other modes
docContainer.SwitchMode = SwitchMode.VS2005;     // Recommended for MDI
docContainer.SwitchMode = SwitchMode.Immediate;  // Fastest
docContainer.SwitchMode = SwitchMode.List;       // Text-based
docContainer.SwitchMode = SwitchMode.VistaFlip;  // Animated
```

### SwitchMode Enum

```csharp
public enum SwitchMode
{
    Immediate,   // No UI, instant switching
    List,        // Vertical list of documents
    QuickTabs,   // Thumbnail previews (best for TDI)
    VS2005,      // Visual Studio style (best for MDI)
    VistaFlip    // 3D flip animation
}
```

## Mode Comparison

### Feature Comparison Table

| Mode | Visual UI | Previews | Speed | Best For | TDI/MDI |
|------|-----------|----------|-------|----------|---------|
| **Immediate** | ❌ | ❌ | ⚡⚡⚡ | Power users | Both |
| **List** | ✅ | ❌ | ⚡⚡ | Text preference | Both |
| **QuickTabs** | ✅ | ✅ | ⚡⚡ | Modern apps | **TDI** |
| **VS2005** | ✅ | ✅ | ⚡⚡ | IDE apps | **MDI** |
| **VistaFlip** | ✅ | ✅ | ⚡ | Visual appeal | Both |

### User Experience Comparison

**Immediate Mode:**
- ✅ Fastest performance
- ✅ No UI clutter
- ❌ No visual feedback
- ❌ Hard to target specific document

**List Mode:**
- ✅ Simple and clear
- ✅ Text-based selection
- ✅ Compact display
- ❌ No visual previews

**QuickTabs Mode:**
- ✅ Thumbnail previews
- ✅ Modern appearance
- ✅ Easy to identify documents
- ✅ Best for TDI applications
- ❌ Requires more screen space

**VS2005 Mode:**
- ✅ Detailed information
- ✅ Icon + text display
- ✅ Professional appearance
- ✅ Best for MDI applications
- ❌ More complex UI

**VistaFlip Mode:**
- ✅ Visually impressive
- ✅ 3D animations
- ✅ Good for demos
- ❌ Slowest performance
- ❌ Can be distracting

## Choosing the Right Switcher

### Recommended Configurations

**Modern TDI Application (Code Editor, Browser-style):**
```xaml
<syncfusion:DocumentContainer Mode="TDI" SwitchMode="QuickTabs">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

**Traditional MDI Application (IDE, Financial Software):**
```xaml
<syncfusion:DocumentContainer Mode="MDI" SwitchMode="VS2005" CanMDIMinimize="True">
    <!-- Windows -->
</syncfusion:DocumentContainer>
```

**Minimalist Application (Focus on content):**
```xaml
<syncfusion:DocumentContainer Mode="TDI" SwitchMode="Immediate">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

**Business Application (Text-heavy documents):**
```xaml
<syncfusion:DocumentContainer Mode="TDI" SwitchMode="List">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

**Presentation or Demo Application:**
```xaml
<syncfusion:DocumentContainer Mode="TDI" SwitchMode="VistaFlip">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

### Decision Flowchart

```
Start
├─ Using TDI Mode?
│  ├─ Yes
│  │  ├─ Need visual previews? → QuickTabs (RECOMMENDED)
│  │  ├─ Text documents only? → List
│  │  └─ Minimal UI desired? → Immediate
│  └─ No (Using MDI)
│     ├─ IDE-style app? → VS2005 (RECOMMENDED)
│     ├─ Visual appeal priority? → VistaFlip
│     └─ Fast switching needed? → Immediate
└─ Special Requirements?
   ├─ Touch interface → QuickTabs or List
   ├─ Accessibility → List or VS2005
   └─ Performance critical → Immediate
```

## Customizing Switcher Behavior

### Complete Configuration Example

```xaml
<Window x:Class="SwitcherDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Window Switcher Demo" Height="600" Width="800">
    <Grid>
        <!-- Toolbar for switching modes -->
        <StackPanel DockPanel.Dock="Top" Orientation="Horizontal" Margin="10">
            <Label Content="Switcher Mode:" VerticalAlignment="Center"/>
            <ComboBox x:Name="switcherModeCombo" 
                      Width="150"
                      SelectionChanged="SwitcherMode_Changed">
                <ComboBoxItem Content="QuickTabs" IsSelected="True" Tag="QuickTabs"/>
                <ComboBoxItem Content="VS2005" Tag="VS2005"/>
                <ComboBoxItem Content="List" Tag="List"/>
                <ComboBoxItem Content="Immediate" Tag="Immediate"/>
                <ComboBoxItem Content="VistaFlip" Tag="VistaFlip"/>
            </ComboBox>
            <TextBlock Margin="20,0,0,0" VerticalAlignment="Center">
                Press CTRL+TAB to test
            </TextBlock>
        </StackPanel>
        
        <!-- DocumentContainer -->
        <syncfusion:DocumentContainer x:Name="documentContainer"
                                     Mode="TDI"
                                     SwitchMode="QuickTabs">
            <TextBox syncfusion:DocumentContainer.Header="Document 1"
                     Text="Content of Document 1"
                     AcceptsReturn="True"
                     TextWrapping="Wrap"
                     Padding="10"/>
            
            <TextBox syncfusion:DocumentContainer.Header="Document 2"
                     Text="Content of Document 2"
                     AcceptsReturn="True"
                     TextWrapping="Wrap"
                     Padding="10"/>
            
            <TextBox syncfusion:DocumentContainer.Header="Document 3"
                     Text="Content of Document 3"
                     AcceptsReturn="True"
                     TextWrapping="Wrap"
                     Padding="10"/>
            
            <TextBox syncfusion:DocumentContainer.Header="Document 4"
                     Text="Content of Document 4"
                     AcceptsReturn="True"
                     TextWrapping="Wrap"
                     Padding="10"/>
            
            <TextBox syncfusion:DocumentContainer.Header="Document 5"
                     Text="Content of Document 5"
                     AcceptsReturn="True"
                     TextWrapping="Wrap"
                     Padding="10"/>
        </syncfusion:DocumentContainer>
    </Grid>
</Window>
```

```csharp
using System.Windows;
using System.Windows.Controls;
using Syncfusion.Windows.Tools.Controls;

namespace SwitcherDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
        
        private void SwitcherMode_Changed(object sender, SelectionChangedEventArgs e)
        {
            if (documentContainer == null) return;
            
            ComboBoxItem selected = switcherModeCombo.SelectedItem as ComboBoxItem;
            string mode = selected.Tag.ToString();
            
            switch (mode)
            {
                case "QuickTabs":
                    documentContainer.SwitchMode = SwitchMode.QuickTabs;
                    break;
                case "VS2005":
                    documentContainer.SwitchMode = SwitchMode.VS2005;
                    break;
                case "List":
                    documentContainer.SwitchMode = SwitchMode.List;
                    break;
                case "Immediate":
                    documentContainer.SwitchMode = SwitchMode.Immediate;
                    break;
                case "VistaFlip":
                    documentContainer.SwitchMode = SwitchMode.VistaFlip;
                    break;
            }
            
            MessageBox.Show(
                $"Switcher mode changed to {mode}.\nPress CTRL+TAB to test.",
                "Mode Changed",
                MessageBoxButton.OK,
                MessageBoxImage.Information);
        }
    }
}
```

### Saving User Preference

```csharp
// Save user's preferred switcher mode
private void SaveSwitcherPreference()
{
    Properties.Settings.Default.SwitcherMode = documentContainer.SwitchMode.ToString();
    Properties.Settings.Default.Save();
}

// Load user's preferred switcher mode
private void LoadSwitcherPreference()
{
    if (!string.IsNullOrEmpty(Properties.Settings.Default.SwitcherMode))
    {
        SwitchMode mode = (SwitchMode)Enum.Parse(
            typeof(SwitchMode), 
            Properties.Settings.Default.SwitcherMode);
        documentContainer.SwitchMode = mode;
    }
}

// Call in Window_Loaded
private void Window_Loaded(object sender, RoutedEventArgs e)
{
    LoadSwitcherPreference();
}

// Call in Window_Closing
private void Window_Closing(object sender, System.ComponentModel.CancelEventArgs e)
{
    SaveSwitcherPreference();
}
```

## Best Practices

1. **Match mode to container type:** Use QuickTabs for TDI, VS2005 for MDI
2. **Provide mode selection:** Let users choose their preferred switcher
3. **Test with real content:** Different modes work better with different content types
4. **Consider document count:** More documents favor visual modes (QuickTabs, VS2005)
5. **Educate users:** Show CTRL+TAB hint in UI or documentation
6. **Save preferences:** Remember user's choice across sessions
7. **Accessibility:** List and VS2005 modes work better with screen readers
8. **Performance:** Use Immediate mode if switching performance is critical

## Troubleshooting

**Issue:** CTRL+TAB doesn't work
- **Solution:** Ensure SwitchMode is set to any value except "None" (not a valid enum value)

**Issue:** Switcher shows blank previews
- **Solution:** QuickTabs generates thumbnails; complex controls may not render well

**Issue:** VistaFlip mode is laggy
- **Solution:** This mode uses animations; switch to QuickTabs or VS2005 for better performance

**Issue:** Switcher appears in wrong position
- **Solution:** This is handled automatically; check window size and DPI settings

## Related Documentation

- **Container Modes:** See `container-modes.md` for TDI vs MDI overview
- **Tab Management:** See `tab-management.md` for TDI tab operations
- **Window Management:** See `window-management.md` for MDI window operations

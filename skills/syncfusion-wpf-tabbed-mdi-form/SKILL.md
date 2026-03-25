---
name: syncfusion-wpf-tabbed-mdi-form
description: Implement Syncfusion WPF DocumentContainer (Tabbed MDI Form) control for creating MDI and TDI interfaces. Use this skill when working with DocumentContainer, Multiple Document Interface (MDI), Tabbed Document Interface (TDI), or Visual Studio-style document management. Covers document windows, tab groups, window switchers, state persistence for document layouts, and dockable document windows in WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF DocumentContainer

The Syncfusion WPF DocumentContainer (Tabbed MDI Form) is a control for holding documents, controls, and panels within your application. It enables you to create both MDI (Multiple Document Interface) and TDI (Tabbed Document Interface) layouts, making it easy to build navigable applications similar to Visual Studio or modern IDE document management systems.

## When to Use This Skill

Use the DocumentContainer control when you need to:

- **Multi-document interfaces**: Create applications that manage multiple documents simultaneously (like Visual Studio, Word, or Excel)
- **Tabbed interfaces**: Implement tab-based navigation for documents and panels
- **MDI applications**: Build traditional Multiple Document Interface applications with floating/resizable windows
- **TDI applications**: Create modern Tabbed Document Interface applications with pinnable tabs
- **Document management**: Handle multiple views, editors, or panels within a single application window
- **Window switching**: Enable easy navigation between multiple documents using keyboard shortcuts (CTRL+TAB)
- **State persistence**: Save and restore document layouts, positions, and states across application sessions
- **Professional layouts**: Create applications with Visual Studio-style document management and window organization

## Component Overview

**Key Capabilities:**
- Dual mode support: TDI (Tabbed) and MDI (Multiple Document) interfaces
- Five window switcher styles for navigation (Immediate, List, QuickTabs, VS2005, VistaFlip)
- Window state management (minimize, maximize, resize)
- Tab groups for organizing related documents
- Pin/unpin tabs for persistent vs temporary documents
- State persistence (Registry, XML, Binary, Isolated Storage)
- Full screen mode support
- Drag and drop customization
- Keyboard navigation and shortcuts
- Theme and skin support

**Assembly Requirements:**
- Syncfusion.Tools.WPF
- Syncfusion.Shared.WPF

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and NuGet packages
- Adding DocumentContainer via designer or code
- Adding document windows to the container
- Setting headers for documents
- Basic TDI/MDI setup with complete examples

### Container Modes (TDI vs MDI)
📄 **Read:** [references/container-modes.md](references/container-modes.md)
- Understanding TDI (Tabbed Document Interface)
- Understanding MDI (Multiple Document Interface)
- Setting the Mode property
- When to use each mode
- Switching between modes at runtime
- Mode-specific features and behavior

### Window Management
📄 **Read:** [references/window-management.md](references/window-management.md)
- Minimizing MDI windows (CanMDIMinimize property)
- Maximizing MDI windows
- Setting window states (Normal, Minimized, Maximized)
- Resizing windows and setting MDI bounds
- Keyboard-based window resizing
- Window arrangement patterns

### Tab Management
📄 **Read:** [references/tab-management.md](references/tab-management.md)
- Adding and removing document items
- Creating and organizing tab groups
- Pin and unpin tabs
- Tab navigation and ordering
- Dynamic tab management
- Items collection operations

### Window Switchers
📄 **Read:** [references/window-switchers.md](references/window-switchers.md)
- CTRL+TAB navigation overview
- SwitchMode property and options
- Immediate, List, QuickTabs modes
- VS2005 and VistaFlip modes
- Choosing the right switcher for your application
- Keyboard shortcuts for window navigation

### State Persistence
📄 **Read:** [references/state-persistence.md](references/state-persistence.md)
- SaveDockState and LoadDockState methods
- Saving to Registry (Binary/SOAP formatter)
- Saving to Isolated Storage
- Saving to XML files
- Saving to Binary files
- Resetting and deleting states
- Best practices for state management

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)
- Setting document headers
- Custom header templates
- Theme and skin support
- Visual customization options
- Container appearance styling
- Custom layouts

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Full screen mode
- Disabling drag and drop
- MDI window arrangement
- Event handling
- Integration with DockingManager
- Performance optimization
- Troubleshooting common issues

## Quick Start

### Basic TDI Implementation

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="DocumentContainer Demo" Height="600" Width="800">
    <Grid>
        <!-- DocumentContainer in TDI mode -->
        <syncfusion:DocumentContainer x:Name="documentContainer" Mode="TDI">
            <!-- Document 1 -->
            <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 1">
                <FlowDocument>
                    <Paragraph>Content of Document 1</Paragraph>
                </FlowDocument>
            </FlowDocumentScrollViewer>
            
            <!-- Document 2 -->
            <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 2">
                <FlowDocument>
                    <Paragraph>Content of Document 2</Paragraph>
                </FlowDocument>
            </FlowDocumentScrollViewer>
            
            <!-- Document 3 -->
            <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 3">
                <FlowDocument>
                    <Paragraph>Content of Document 3</Paragraph>
                </FlowDocument>
            </FlowDocumentScrollViewer>
        </syncfusion:DocumentContainer>
    </Grid>
</Window>
```

### Basic MDI Implementation (C#)

```csharp
using Syncfusion.Windows.Tools.Controls;
using System.Windows;
using System.Windows.Controls;

namespace DocumentContainerDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create DocumentContainer in MDI mode
            DocumentContainer documentContainer = new DocumentContainer();
            documentContainer.Mode = DocumentContainerMode.MDI;
            documentContainer.SwitchMode = SwitchMode.VS2005;
            
            // Add document windows
            Button doc1 = new Button { Content = "Document 1 Content" };
            Button doc2 = new Button { Content = "Document 2 Content" };
            Button doc3 = new Button { Content = "Document 3 Content" };
            
            // Set headers
            DocumentContainer.SetHeader(doc1, "Document 1");
            DocumentContainer.SetHeader(doc2, "Document 2");
            DocumentContainer.SetHeader(doc3, "Document 3");
            
            // Add to container
            documentContainer.Items.Add(doc1);
            documentContainer.Items.Add(doc2);
            documentContainer.Items.Add(doc3);
            
            // Set as window content
            this.Content = documentContainer;
        }
    }
}
```

## Common Patterns

### Pattern 1: TDI with QuickTabs Switcher

Ideal for modern tabbed applications with Visual Studio-style navigation.

```xaml
<syncfusion:DocumentContainer Name="DocContainer" 
                              Mode="TDI" 
                              SwitchMode="QuickTabs">
    <!-- Add documents here -->
</syncfusion:DocumentContainer>
```

### Pattern 2: MDI with Window Management

Enable minimize, maximize, and resize for traditional MDI applications.

```xaml
<syncfusion:DocumentContainer Name="DocContainer" 
                              Mode="MDI" 
                              CanMDIMinimize="True"
                              SwitchMode="VS2005">
    <!-- Add documents here -->
</syncfusion:DocumentContainer>
```

### Pattern 3: State Persistence (XML)

Save and restore document layouts across sessions.

```csharp
using System.Runtime.Serialization.Formatters.Binary;

// Save state to XML
BinaryFormatter formatter = new BinaryFormatter();
documentContainer.SaveDockState(formatter, StorageFormat.Xml, @"layout.xml");

// Load state from XML
BinaryFormatter formatter = new BinaryFormatter();
documentContainer.LoadDockState(formatter, StorageFormat.Xml, @"layout.xml");
```

### Pattern 4: Dynamic Document Management

Add and remove documents programmatically.

```csharp
// Add a new document
TextBox newDoc = new TextBox { Text = "New Document Content" };
DocumentContainer.SetHeader(newDoc, "New Document");
documentContainer.Items.Add(newDoc);

// Remove all documents
documentContainer.Items.Clear();

// Remove specific document
documentContainer.Items.Remove(newDoc);
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Mode` | DocumentContainerMode | Sets TDI or MDI mode |
| `SwitchMode` | SwitchMode | Window switcher style (Immediate, List, QuickTabs, VS2005, VistaFlip) |
| `CanMDIMinimize` | bool | Enables MDI window minimization |
| `Items` | ItemCollection | Collection of documents in the container |

## Key Methods

| Method | Description |
|--------|-------------|
| `SaveDockState()` | Saves document state to storage (Registry, XML, Binary) |
| `LoadDockState()` | Loads document state from storage |
| `ResetState()` | Resets the container to default state |
| `DeleteDockState()` | Deletes saved state from storage |
| `Items.Add()` | Adds a document to the container |
| `Items.Clear()` | Removes all documents from the container |

## Common Use Cases

1. **Code Editors and IDEs**: Create multi-document code editing environments with tabbed or MDI layouts
2. **Data Entry Applications**: Manage multiple data entry forms simultaneously
3. **Document Viewers**: Display multiple documents (PDF, images, text) in a tabbed interface
4. **Dashboard Applications**: Organize multiple dashboard panels and charts
5. **Reporting Tools**: Manage multiple report views and editors
6. **Configuration Tools**: Handle multiple configuration panels or property editors
7. **Medical Applications**: Manage patient records with multiple document views
8. **Financial Applications**: Display multiple charts, reports, and data grids simultaneously

---

**Next Steps:** Choose the appropriate reference file above based on your specific implementation needs. Start with `getting-started.md` for initial setup, then explore specific features as needed.

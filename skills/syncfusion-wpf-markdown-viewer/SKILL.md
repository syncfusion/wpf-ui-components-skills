---
name: syncfusion-wpf-markdown-viewer
description: Implement Syncfusion WPF SfMarkdownViewer for rendering and displaying Markdown content in WPF applications. Use this when rendering Markdown text, loading Markdown from strings, files, or URLs, handling hyperlink clicks, or displaying Mermaid diagrams. Covers the Source property, HyperlinkClicked events, and MermaidBlockTemplate customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF SfMarkdownViewer

The `SfMarkdownViewer` renders Markdown content — headings, lists, links, images, tables, code blocks, and block quotes — into a styled WPF visual. Use it to display documentation, release notes, help content, README files, or any Markdown-based content without external rendering engines.

## When to Use This Skill

- User needs to render or preview Markdown content in a WPF app
- Loading Markdown from a string, local file, or remote URL
- Displaying documentation, changelogs, help text, or README files
- Intercepting or disabling hyperlink navigation from Markdown
- Rendering Mermaid flowchart diagrams embedded in Markdown

## Quick Start

```xaml
<Window
    x:Class="GettingStarted.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:markdown="clr-namespace:Syncfusion.UI.Xaml.Markdown;assembly=Syncfusion.SfMarkdownViewer.WPF"
    xmlns:system="clr-namespace:System;assembly=mscorlib">
    <Grid>
        <markdown:SfMarkdownViewer Source="# Hello World&#10;&#10;This is **bold** and *italic* text." />
    </Grid>
</Window>
```

```csharp
using Syncfusion.UI.Xaml.Markdown;

SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
markdownViewer.Source = "# Hello World\n\nThis is **bold** and *italic* text.";
this.Content = markdownViewer;
```

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (Syncfusion.SfMarkdownViewer.Wpf)
- Required assembly references (3 assemblies)
- XAML namespace import
- Adding control via XAML and C#
- Inline Markdown string via Source property
- SfSkinManager theme support

### Loading Content
📄 **Read:** [references/loading-content.md](references/loading-content.md)
- Loading from raw Markdown string
- Loading from a local file path
- Loading from a remote HTTP/HTTPS URL
- XAML CDATA string source pattern
- Decision guide: which source type to use

### Events
📄 **Read:** [references/events.md](references/events.md)
- HyperlinkClicked event
- MarkdownHyperlinkClickedEventArgs (Url, Cancel)
- Disabling hyperlink navigation (Cancel = true)
- Retrieving the clicked URL for in-app routing

### Mermaid Diagrams
📄 **Read:** [references/mermaid-diagrams.md](references/mermaid-diagrams.md)
- MermaidBlockTemplate property
- Embedding SfDiagram in a DataTemplate
- FlowchartLayout configuration
- LoadDiagramFromMermaid() usage
- Required dependencies for Mermaid support

## Key Properties

| Property | Type | Purpose |
|---|---|---|
| `Source` | `string` | Raw Markdown string, file path, or HTTP/HTTPS URL |
| `MermaidBlockTemplate` | `DataTemplate` | Custom template for rendering mermaid code blocks |

## Key Events

| Event | Args | Purpose |
|---|---|---|
| `HyperlinkClicked` | `MarkdownHyperlinkClickedEventArgs` | Fires when user clicks a hyperlink in rendered Markdown |

## Common Use Cases

- **In-app documentation:** Bind `Source` to a URL pointing to a GitHub raw README
- **Release notes panel:** Load a local `.md` file and assign its text to `Source`
- **Help viewer:** Render contextual Markdown help strings alongside your UI
- **Diagram viewer:** Use `MermaidBlockTemplate` with `SfDiagram` to render flowcharts
- **Controlled navigation:** Handle `HyperlinkClicked` to open links in an embedded browser or custom dialog

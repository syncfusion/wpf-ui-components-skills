---
name: syncfusion-wpf-markdown-viewer
description: Implement Syncfusion WPF SfMarkdownViewer for rendering and displaying Markdown content in WPF applications. Use this when rendering Markdown text, loading Markdown from strings or application-controlled local files, handling hyperlink clicks with URL validation, or displaying Mermaid diagrams with input sanitization. Covers the Source property, HyperlinkClicked events with allowlist-based navigation, and MermaidBlockTemplate customization with input validation. Always validate and sanitize Markdown content before assigning to Source, especially when the content originates from remote or user-supplied sources.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF SfMarkdownViewer

The `SfMarkdownViewer` renders Markdown content — headings, lists, links, images, tables, code blocks, and block quotes — into a styled WPF visual. Use it to display documentation, release notes, help content, README files, or any Markdown-based content without external rendering engines.

## When to Use This Skill

- User needs to render or preview Markdown content in a WPF app
- Loading Markdown from a hard-coded string, application-bundled resource, or application-controlled local file
- Displaying documentation, changelogs, or help text that originates from trusted, application-owned sources
- Intercepting hyperlink navigation with URL validation and allowlisting
- Rendering Mermaid flowchart diagrams with input length and type validation

> ⚠️ **Out of scope:** Do not use this skill to load Markdown directly from arbitrary third-party URLs or unvalidated user input without fetching, validating, and sanitizing the content in application code first.

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
- Loading from a local application-controlled file path
- Loading from a remote URL via application code (fetch → validate origin → check content-type → assign string)
- XAML CDATA string source pattern
- Decision guide: which source type to use
- Security requirements for remote content loading

### Events
📄 **Read:** [references/events.md](references/events.md)
- HyperlinkClicked event
- MarkdownHyperlinkClickedEventArgs (Url, Cancel)
- Cancel-by-default pattern — always cancel first, then selectively permit validated URLs
- Allowlist-based in-app route validation for `app://` links
- Blocking non-HTTPS external schemes

### Mermaid Diagrams
📄 **Read:** [references/mermaid-diagrams.md](references/mermaid-diagrams.md)
- MermaidBlockTemplate property
- Embedding SfDiagram in a DataTemplate
- Validating DataContext (Mermaid string) before rendering — length limit and type allowlist
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

- **In-app documentation:** Embed Markdown as a hard-coded string or application-bundled resource file; assign to `Source` directly
- **Release notes panel:** Load a local `.md` file bundled with the application (not a path from user input) and assign its text to `Source`
- **Remote documentation:** Fetch Markdown from an application-owned HTTPS endpoint in application code, validate the HTTP response origin and content-type, then assign the sanitized string to `Source` — never pass a third-party URL directly to `Source`
- **Help viewer:** Render contextual Markdown help strings from trusted, application-controlled sources
- **Diagram viewer:** Use `MermaidBlockTemplate` with `SfDiagram` to render flowcharts; validate the Mermaid `DataContext` string (length and type prefix) before calling `LoadDiagramFromMermaid()`
- **Controlled navigation:** Handle `HyperlinkClicked` with cancel-by-default; validate `app://` routes against an explicit allowlist and only permit `https://` external links

## Security Considerations

| Risk | Mitigation |
|---|---|
| Remote URL loads untrusted third-party Markdown | Fetch content in app code; validate origin (allowlisted base URL) and content-type before assigning to `Source` |
| Hyperlink in Markdown drives unintended navigation | Use cancel-by-default in `HyperlinkClicked`; validate `app://` routes against a fixed allowlist; only permit `https://` external links |
| Mermaid block from untrusted Markdown drives diagram rendering | Validate `DataContext` string: check non-null, enforce length limit, allowlist diagram type prefixes before calling `LoadDiagramFromMermaid()` |
| User-supplied URL passed to `Source` | Never accept URLs from user input or third-party data as-is; always validate origin in application code first |

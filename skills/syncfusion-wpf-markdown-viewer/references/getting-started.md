# Getting Started

## Installation

Install the NuGet package in your WPF project:

```
Syncfusion.SfMarkdownViewer.Wpf
```

Or add assembly references manually:
- `Syncfusion.SfMarkdownViewer.WPF`
- `Syncfusion.Markdown`
- `Syncfusion.Shared.WPF`

> **Note:** Unlike most other Syncfusion WPF controls, `SfMarkdownViewer` requires **three** assembly references. `Syncfusion.Markdown` handles the parsing and `Syncfusion.Shared.WPF` provides shared infrastructure.

## XAML Namespace

Import the Markdown namespace in your XAML page:

```xaml
xmlns:markdown="clr-namespace:Syncfusion.UI.Xaml.Markdown;assembly=Syncfusion.SfMarkdownViewer.WPF"
```

## Adding via XAML

```xaml
<Window
    x:Class="GettingStarted.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:markdown="clr-namespace:Syncfusion.UI.Xaml.Markdown;assembly=Syncfusion.SfMarkdownViewer.WPF"
    xmlns:system="clr-namespace:System;assembly=mscorlib">
    <Grid>
        <markdown:SfMarkdownViewer />
    </Grid>
</Window>
```

## Adding via C#

```csharp
using Syncfusion.UI.Xaml.Markdown;

namespace MarkdownViewerGettingStarted
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
            this.Content = markdownViewer;
        }
    }
}
```

## Setting Inline Markdown Content

Use the `Source` property with a CDATA string in XAML for multi-line inline Markdown:

```xaml
<markdown:SfMarkdownViewer>
    <markdown:SfMarkdownViewer.Source>
        <system:String xml:space="preserve">
            <![CDATA[
# What is the Markdown Viewer?
The MarkdownViewer control renders Markdown files into a clean, readable format.

## Features
- Headings, lists, links, images
- Code blocks and tables
- Block quotes

![WPF SfMarkdownViewer](Images/wpf-markdown-viewer-gettingstarted.png)
            ]]>
        </system:String>
    </markdown:SfMarkdownViewer.Source>
</markdown:SfMarkdownViewer>
```

In code-behind, assign a multiline string directly:

```csharp
private const string markdownContent = @"
# What is the Markdown Viewer?
The MarkdownViewer control renders Markdown files into a clean, readable format.

## Features
- Headings, lists, links, images
- Code blocks and tables
- Block quotes";

public MainWindow()
{
    InitializeComponent();
    SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
    markdownViewer.Source = markdownContent;
    this.Content = markdownViewer;
}
```

## Gotchas

- **Three assemblies required** — unlike most Syncfusion WPF controls that use a single assembly, `SfMarkdownViewer` needs `Syncfusion.SfMarkdownViewer.WPF`, `Syncfusion.Markdown`, and `Syncfusion.Shared.WPF`
- **`xml:space="preserve"` is required** for CDATA inline strings in XAML — without it, whitespace and newlines are collapsed
- **`Source` is a string property** — it accepts raw Markdown text, a file path, or a URL; the control auto-detects which one to use (see loading-content.md)
- **No separate `Text` property** — always use `Source` to provide content; there is no separate binding for Markdown text vs file path

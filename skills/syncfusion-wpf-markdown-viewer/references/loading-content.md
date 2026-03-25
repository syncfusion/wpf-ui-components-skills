# Loading Content

The `Source` property is the single entry point for all content. It intelligently detects the input type — raw Markdown string, file path, or HTTP/HTTPS URL — and handles loading automatically.

## Decision Guide

```
Where is the Markdown content?
  → In-memory string (hard-coded or generated)
      Use: markdownViewer.Source = markdownString;
  → Local file on disk
      Use: markdownViewer.Source = File.ReadAllText(filePath);
  → Remote URL (GitHub, web server, API)
      Use: markdownViewer.Source = "https://...";
```

---

## Loading from a Raw Markdown String

Assign any Markdown-formatted string directly to `Source`:

**XAML (inline CDATA):**
```xaml
<Grid>
    <markdown:SfMarkdownViewer>
        <markdown:SfMarkdownViewer.Source>
            <system:String xml:space="preserve">
                <![CDATA[
# What is the Markdown Viewer?
The Markdown Viewer control renders and previews Markdown files.

# Header 1
Used for the main title or top-level heading.

## Header 2
Used to define major sections.
                ]]>
            </system:String>
        </markdown:SfMarkdownViewer.Source>
    </markdown:SfMarkdownViewer>
</Grid>
```

**C#:**
```csharp
using Syncfusion.UI.Xaml.Markdown;

public partial class MainWindow : Window
{
    private const string markdownContent = @"
# What is the Markdown Viewer?
The Markdown Viewer control renders and previews Markdown files.

# Header 1
Used for the main title or top-level heading.

## Header 2
Used to define major sections.";

    public MainWindow()
    {
        InitializeComponent();
        SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
        markdownViewer.Source = markdownContent;
        this.Content = markdownViewer;
    }
}
```

---

## Loading from a Local File

Read the file content and assign it to `Source`:

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        SfMarkdownViewer markdownViewer = new SfMarkdownViewer();

        string filePath = @"D:\MyApp\Docs\MarkdownContent.md";
        string markdownContent = File.ReadAllText(filePath);
        markdownViewer.Source = markdownContent;

        this.Content = markdownViewer;
    }
}
```

For MVVM, expose the file content as a string property and bind:

```xaml
<markdown:SfMarkdownViewer Source="{Binding MarkdownText}" />
```

```csharp
// ViewModel
public string MarkdownText { get; set; }

public ViewModel()
{
    MarkdownText = File.ReadAllText(@"D:\MyApp\Docs\content.md");
}
```

---

## Loading from a URL

Assign an HTTP or HTTPS URL string directly to `Source`. The control fetches and renders the remote Markdown:

**XAML:**
```xaml
<markdown:SfMarkdownViewer
    Source="https://raw.githubusercontent.com/SyncfusionExamples/wpf-tabsplitter-example/refs/heads/master/README.md" />
```

**C#:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
        markdownViewer.Source = "https://raw.githubusercontent.com/SyncfusionExamples/wpf-tabsplitter-example/refs/heads/master/README.md";
        this.Content = markdownViewer;
    }
}
```

**Common URL sources:**
- GitHub raw content: `https://raw.githubusercontent.com/{user}/{repo}/refs/heads/main/README.md`
- Any publicly accessible `.md` file URL

---

## Gotchas

- **`Source` auto-detects type** — the control determines whether the value is a Markdown string, a file path, or a URL automatically; no separate property is needed
- **File reading is your responsibility** — for local files, call `File.ReadAllText()` before assigning; the control does not read files from paths directly
- **URL loading is async** — remote URL loading happens asynchronously; the control renders as soon as the content arrives; no additional await needed
- **Encoding** — use `File.ReadAllText(path, Encoding.UTF8)` if your Markdown files contain non-ASCII characters to avoid garbled output
- **XAML CDATA requires `xml:space="preserve"`** — omitting it collapses whitespace and breaks Markdown structure

# Loading Content

The `Source` property is the single entry point for all content. It intelligently detects the input type — raw Markdown string, file path, or HTTP/HTTPS URL — and handles loading automatically.

## Decision Guide

```
Where is the Markdown content?
  → In-memory string (hard-coded or generated)
      Use: markdownViewer.Source = markdownString;
  → Local file on disk
      Use: markdownViewer.Source = File.ReadAllText(filePath);
  → Remote URL — ONLY use application-controlled, trusted endpoints.
      Fetch the content yourself, validate/sanitize it, then assign the string:
      Use: markdownViewer.Source = await FetchAndSanitizeAsync("https://your-own-domain/content.md");
```

> ⚠️ **Security note:** Never bind `Source` directly to a user-supplied or third-party URL. Always fetch remote content via application code, validate the origin, and sanitize the Markdown string before assigning it to `Source`. Rendering untrusted remote Markdown can expose the app to prompt injection via malicious link targets or embedded directives.

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

## Loading from a Remote URL (Application-Controlled Endpoints Only)

> ⚠️ **Security requirement:** Do **not** pass third-party or user-supplied URLs directly to `Source`. Fetch content yourself over HTTPS from an application-owned endpoint, validate the HTTP response, and assign the sanitized string to `Source`.

**Safe pattern — fetch, validate, then assign:**

```csharp
public partial class MainWindow : Window
{
    private static readonly HttpClient _httpClient = new HttpClient();

    // Only allow URLs from your own trusted domain
    private static readonly Uri _allowedBase = new Uri("https://docs.your-app.com/");

    public MainWindow()
    {
        InitializeComponent();
        LoadDocumentationAsync();
    }

    private async void LoadDocumentationAsync()
    {
        try
        {
            var targetUri = new Uri(_allowedBase, "release-notes.md");

            // Enforce origin — reject anything outside the allowed base
            if (!targetUri.AbsoluteUri.StartsWith(_allowedBase.AbsoluteUri, StringComparison.Ordinal))
                throw new InvalidOperationException("URL is outside the allowed origin.");

            var response = await _httpClient.GetAsync(targetUri);
            response.EnsureSuccessStatusCode();

            // Verify content type is plain text / markdown — reject HTML etc.
            var mediaType = response.Content.Headers.ContentType?.MediaType ?? "";
            if (!mediaType.StartsWith("text/", StringComparison.OrdinalIgnoreCase))
                throw new InvalidOperationException($"Unexpected content type: {mediaType}");

            string markdownContent = await response.Content.ReadAsStringAsync();

            SfMarkdownViewer markdownViewer = new SfMarkdownViewer();
            markdownViewer.Source = markdownContent;
            this.Content = markdownViewer;
        }
        catch (Exception ex)
        {
            // Handle fetch or validation failure — show fallback content
            MessageBox.Show($"Failed to load documentation: {ex.Message}");
        }
    }
}
```

**What this pattern enforces:**
- URL is constructed from an application-owned base — never from user input or third-party data
- HTTP response content-type is validated before rendering
- Any fetch or validation failure falls back safely

**Do NOT do this:**
```csharp
// ❌ Never bind Source directly to a remote URL — content is untrusted
markdownViewer.Source = userProvidedUrl;

// ❌ Never use arbitrary third-party URLs directly
markdownViewer.Source = "https://raw.githubusercontent.com/some-user/some-repo/main/README.md";
```

---

## Gotchas

- **`Source` auto-detects type** — the control determines whether the value is a Markdown string, a file path, or a URL automatically; no separate property is needed
- **File reading is your responsibility** — for local files, call `File.ReadAllText()` before assigning; the control does not read files from paths directly
- **Fetch remote content in application code** — do not rely on the control's built-in URL fetching for production use; fetch content yourself so you can validate origin and content type before rendering
- **Never use third-party or user-supplied URLs** — always load from application-controlled, allowlisted endpoints; treat all remote Markdown as potentially untrusted until sanitized
- **Encoding** — use `File.ReadAllText(path, Encoding.UTF8)` if your Markdown files contain non-ASCII characters to avoid garbled output
- **XAML CDATA requires `xml:space="preserve"`** — omitting it collapses whitespace and breaks Markdown structure

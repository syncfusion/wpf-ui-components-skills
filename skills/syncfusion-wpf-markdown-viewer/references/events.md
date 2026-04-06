# Events

## HyperlinkClicked

Fires whenever a user clicks a hyperlink in the rendered Markdown content. Use this event to intercept navigation, retrieve the URL, open a custom browser, or cancel the default behavior.

### Event Args: `MarkdownHyperlinkClickedEventArgs`

| Property | Type | Description |
|---|---|---|
| `Url` | `string` | The URL of the clicked hyperlink |
| `Cancel` | `bool` | Set to `true` to cancel default navigation |

---

## Wiring the Event

**XAML:**
```xaml
<markdown:SfMarkdownViewer
    x:Name="markdownViewer"
    HyperlinkClicked="MarkdownViewer_HyperlinkClicked"
    Source="{Binding MarkdownText}" />
```

**C#:**
```csharp
markdownViewer.HyperlinkClicked += MarkdownViewer_HyperlinkClicked;
```

---

## Retrieving the Clicked URL

```csharp
private void MarkdownViewer_HyperlinkClicked(object sender, MarkdownHyperlinkClickedEventArgs args)
{
    string clickedUrl = args.Url;
    // Use the URL: log it, display it, open custom browser, etc.
    MessageBox.Show($"Navigating to: {clickedUrl}");
}
```

---

## Disabling Hyperlink Navigation

Set `Cancel = true` to prevent the default browser from opening:

```csharp
private void MarkdownViewer_HyperlinkClicked(object sender, MarkdownHyperlinkClickedEventArgs args)
{
    args.Cancel = true; // Suppresses default OS browser navigation
}
```

---

## In-App Navigation Pattern

> ⚠️ **Security requirement:** Always validate and allowlist the URL before acting on it. Markdown content can originate from untrusted sources; a malicious `app://` link in rendered Markdown can trigger unintended in-app navigation if routes are not validated.

Handle links that point to internal routes (e.g., `app://page/settings`) while allowing external links to open normally:

```csharp
// Define an explicit allowlist of valid internal routes
private static readonly HashSet<string> _allowedRoutes = new HashSet<string>(StringComparer.OrdinalIgnoreCase)
{
    "settings", "help", "dashboard", "profile"
};

private void MarkdownViewer_HyperlinkClicked(object sender, MarkdownHyperlinkClickedEventArgs args)
{
    // Always cancel first — explicitly decide what to allow
    args.Cancel = true;

    if (args.Url.StartsWith("app://", StringComparison.OrdinalIgnoreCase))
    {
        // Extract and validate the route against the allowlist
        string route = args.Url.Substring("app://".Length).Trim('/');

        if (_allowedRoutes.Contains(route))
        {
            NavigateTo(route);
        }
        // If route is not in the allowlist, navigation is silently blocked (Cancel = true)
    }
    else if (Uri.TryCreate(args.Url, UriKind.Absolute, out Uri uri)
             && (uri.Scheme == Uri.UriSchemeHttps))
    {
        // Only allow HTTPS external links — block HTTP and other schemes
        args.Cancel = false; // Permit safe external navigation
    }
    // All other schemes (http, ftp, file, javascript, etc.) remain blocked
}

private void NavigateTo(string route)
{
    // Your in-app navigation logic — route is already validated
}
```

**What this pattern enforces:**
- All navigation is cancelled by default; only explicitly allowed routes and HTTPS URLs are permitted
- Internal `app://` routes are validated against a fixed allowlist — arbitrary routes from Markdown content cannot be acted on
- HTTP and non-HTTPS external links are blocked to reduce phishing risk
- Schemes like `javascript:`, `file:`, and `data:` are blocked implicitly

**Do NOT do this:**
```csharp
// ❌ Never act on app:// routes without allowlist validation
if (args.Url.StartsWith("app://"))
{
    string route = args.Url.Replace("app://", "");
    NavigateTo(route); // Untrusted route from Markdown content
}

// ❌ Never allow all external links without scheme check
args.Cancel = false; // Opens any URL including http:// and file://
```

---

## Gotchas

- **Default behavior opens OS browser** — if `Cancel` is not set to `true`, clicking a link opens the system default browser
- **Cancel all by default** — set `args.Cancel = true` first, then selectively re-enable only trusted URLs; this is safer than cancelling only specific cases
- **`Cancel` must be set synchronously** — set `args.Cancel` before the event handler returns; async patterns may not suppress navigation in time
- **Validate `app://` routes against an allowlist** — the `Url` value comes directly from Markdown source text; treat it as untrusted input and never pass it to navigation logic without validation
- **Block non-HTTPS schemes** — only permit `https://` for external links; block `http://`, `file://`, `javascript:`, and `data:` schemes explicitly
- **Relative links** — the `Url` value is exactly what appears in the Markdown source; relative paths like `./page.md` are not resolved against a base URL automatically

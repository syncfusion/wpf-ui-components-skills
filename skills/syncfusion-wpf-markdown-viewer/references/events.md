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

Handle links that point to internal routes (e.g., `app://page/settings`) while allowing external links to open normally:

```csharp
private void MarkdownViewer_HyperlinkClicked(object sender, MarkdownHyperlinkClickedEventArgs args)
{
    if (args.Url.StartsWith("app://"))
    {
        // Internal navigation — cancel default and handle in-app
        args.Cancel = true;
        string route = args.Url.Replace("app://", "");
        NavigateTo(route);
    }
    // External links proceed normally (Cancel stays false)
}

private void NavigateTo(string route)
{
    // Your in-app navigation logic
}
```

---

## Gotchas

- **Default behavior opens OS browser** — if `Cancel` is not set to `true`, clicking a link opens the system default browser
- **`Cancel` must be set synchronously** — set `args.Cancel = true` before the event handler returns; async patterns may not suppress navigation in time
- **Relative links** — the `Url` value is exactly what appears in the Markdown source; relative paths like `./page.md` are not resolved against a base URL automatically

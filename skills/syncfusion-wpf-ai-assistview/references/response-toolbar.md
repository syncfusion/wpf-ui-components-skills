# Response Toolbar

## Table of Contents
- [Overview](#overview)
- [Built-In Toolbar Items](#built-in-toolbar-items)
- [Visibility Control](#visibility-control)
- [ResponseToolbarItem Configuration](#responsetoolbaritem-configuration)
- [ResponseToolbarItemClicked Event](#responsetoolbaritemclicked-event)
- [Custom Item Template](#custom-item-template)

---

## Overview

The response toolbar appears below each AI response message and provides action buttons for interacting with that response. By default, it shows built-in Copy, Regenerate, Like, and Dislike buttons.

**Default state:** `IsResponseToolbarVisible` is `true` — the toolbar shows automatically.

---

## Built-In Toolbar Items

`SfAIAssistView` includes four built-in response toolbar items out of the box:

| Item | Purpose |
|---|---|
| **Copy** | Copies the AI response text to clipboard |
| **Regenerate** | Requests a new AI response for the same prompt |
| **Like** | Marks the response as helpful (thumbs up) |
| **Dislike** | Marks the response as unhelpful (thumbs down) |

No configuration needed — they appear automatically when `IsResponseToolbarVisible="True"`.

---

## Visibility Control

```xaml
<!-- Show response toolbar (default) -->
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    IsResponseToolbarVisible="True" />

<!-- Hide response toolbar -->
<syncfusion:SfAIAssistView
    IsResponseToolbarVisible="False" />
```

Or from code-behind:
```csharp
sfAIAssistView.IsResponseToolbarVisible = false;
```

---

## ResponseToolbarItem Configuration

Add custom items alongside (or instead of) built-in items using `ResponseToolbarItems`:

```xaml
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    IsResponseToolbarVisible="True"
    ResponseToolbarItemClicked="OnResponseToolbarItemClicked">
    <syncfusion:SfAIAssistView.ResponseToolbarItems>
        <!-- Custom item at index 0 (appears before built-ins) -->
        <syncfusion:ResponseToolbarItem Index="0">
            <syncfusion:ResponseToolbarItem.ItemTemplate>
                <DataTemplate>
                    <Button Content="🔖" ToolTip="Bookmark" Background="Transparent" />
                </DataTemplate>
            </syncfusion:ResponseToolbarItem.ItemTemplate>
        </syncfusion:ResponseToolbarItem>

        <!-- Custom item at index 4 (appears after built-ins) -->
        <syncfusion:ResponseToolbarItem Index="4">
            <syncfusion:ResponseToolbarItem.ItemTemplate>
                <DataTemplate>
                    <Button Content="📤" ToolTip="Share" Background="Transparent" />
                </DataTemplate>
            </syncfusion:ResponseToolbarItem.ItemTemplate>
        </syncfusion:ResponseToolbarItem>
    </syncfusion:SfAIAssistView.ResponseToolbarItems>
</syncfusion:SfAIAssistView>
```

**`ResponseToolbarItem` properties:**

| Property | Type | Description |
|---|---|---|
| `Index` | `int` | Position in the toolbar (0-based) |
| `ItemType` | `ResponseToolbarItemType` | Type identifier for click handling |
| `ItemTemplate` | `DataTemplate` | Custom visual for the toolbar item |

---

## ResponseToolbarItemClicked Event

Handle clicks on both built-in and custom toolbar items:

```xaml
<syncfusion:SfAIAssistView
    ResponseToolbarItemClicked="OnResponseToolbarItemClicked" />
```

```csharp
private void OnResponseToolbarItemClicked(object sender, ResponseToolbarItemClickedEventArgs e)
{
    // e.Item — the ResponseToolbarItem that was clicked
    // e.Message — the AIMessage associated with this toolbar

    var message = e.Message as AIMessage;

    // Check which item was clicked by its ItemType or template content
    if (e.Item.ItemType == ResponseToolbarItemType.Copy)
    {
        Clipboard.SetText(message?.Text ?? string.Empty);
    }
    else if (e.Item.ItemType == ResponseToolbarItemType.Regenerate)
    {
        // Trigger a new AI request with the original prompt
        RegenerateResponse(message);
    }
    else if (e.Item.ItemType == ResponseToolbarItemType.Like)
    {
        LogFeedback(message, positive: true);
    }
    else if (e.Item.ItemType == ResponseToolbarItemType.Dislike)
    {
        LogFeedback(message, positive: false);
    }
}
```

---

## Custom Item Template

Replace built-in item visuals with custom styled buttons:

```xaml
<Window.Resources>
    <Style x:Key="ToolbarButtonStyle" TargetType="Button">
        <Setter Property="Background" Value="Transparent" />
        <Setter Property="BorderThickness" Value="0" />
        <Setter Property="Padding" Value="6,4" />
        <Setter Property="Cursor" Value="Hand" />
    </Style>
</Window.Resources>

<syncfusion:SfAIAssistView.ResponseToolbarItems>
    <syncfusion:ResponseToolbarItem Index="0" ItemType="Copy">
        <syncfusion:ResponseToolbarItem.ItemTemplate>
            <DataTemplate>
                <Button Style="{StaticResource ToolbarButtonStyle}"
                        ToolTip="Copy response">
                    <Image Source="/Assets/copy-icon.png" Width="16" Height="16" />
                </Button>
            </DataTemplate>
        </syncfusion:ResponseToolbarItem.ItemTemplate>
    </syncfusion:ResponseToolbarItem>
</syncfusion:SfAIAssistView.ResponseToolbarItems>
```

---

## Gotchas

- **`IsResponseToolbarVisible` defaults to `true`** — unlike the input toolbar, the response toolbar is on by default.
- **`Index` determines position** — items at lower index values appear first in the toolbar.
- **Built-in items remain unless replaced** — adding custom `ResponseToolbarItem` objects adds to the existing built-in items unless you explicitly replace all items.
- **`e.Message` in click handler** — always cast to `AIMessage` before accessing `Text` since `Messages` is `ObservableCollection<object>`.

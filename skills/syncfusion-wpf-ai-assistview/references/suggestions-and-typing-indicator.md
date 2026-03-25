# Suggestions and Typing Indicator

## Table of Contents
- [Suggestions Overview](#suggestions-overview)
- [Binding Suggestions](#binding-suggestions)
- [Updating Suggestions After AI Response](#updating-suggestions-after-ai-response)
- [Typing Indicator Overview](#typing-indicator-overview)
- [ViewModel Setup for TypingIndicator](#viewmodel-setup-for-typingindicator)
- [Binding TypingIndicator in XAML](#binding-typingindicator-in-xaml)
- [Toggling with Async AI Calls](#toggling-with-async-ai-calls)

---

## Suggestions Overview

The `Suggestions` property displays clickable suggestion chips below the chat area. Suggestions help users quickly send common follow-up prompts without typing. They appear at the bottom-right of the chat and are updated dynamically after AI responses.

---

## Binding Suggestions

Declare an `IEnumerable<string>` property in the ViewModel:

```csharp
private IEnumerable<string> suggestions;

public IEnumerable<string> Suggestions
{
    get => suggestions;
    set { suggestions = value; RaisePropertyChanged(nameof(Suggestions)); }
}
```

Bind in XAML:

```xaml
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    Suggestions="{Binding Suggestions}" />
```

Initialize with starter suggestions:

```csharp
Suggestions = new List<string>
{
    "What is WPF?",
    "How do I bind data?",
    "Explain MVVM pattern"
};
```

---

## Updating Suggestions After AI Response

Update `Suggestions` whenever an AI response is received so the chips stay relevant to the conversation context:

```csharp
private async void OnChatsCollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
{
    if (e.Action != NotifyCollectionChangedAction.Add) return;
    if (e.NewItems[0] is not TextMessage msg) return;
    if (msg.Author.Name != CurrentUser.Name) return;

    ShowTypingIndicator = true;
    Suggestions = null; // Clear suggestions while AI is responding

    var response = await aiService.GetResponseAsync(msg.Text);

    Application.Current.Dispatcher.Invoke(() =>
    {
        Chats.Add(new AIMessage
        {
            Author = new Author { Name = "AI" },
            Text = response
        });

        // Set context-relevant suggestions after response
        Suggestions = new List<string>
        {
            "Tell me more",
            "Give an example",
            "Summarize this"
        };
    });

    ShowTypingIndicator = false;
}
```

**Pattern:** Clear suggestions when the user sends → repopulate with relevant follow-ups after AI responds.

---

## Typing Indicator Overview

The typing indicator shows an animated "AI is typing" display in the chat while the AI processes a response. It requires:
1. A `TypingIndicator` object (with an `Author`) bound to the `TypingIndicator` property
2. The `ShowTypingIndicator` boolean set to `true` while processing

---

## ViewModel Setup for TypingIndicator

```csharp
private TypingIndicator typingIndicator;
private bool showTypingIndicator;

public TypingIndicator TypingIndicator
{
    get => typingIndicator;
    set { typingIndicator = value; RaisePropertyChanged(nameof(TypingIndicator)); }
}

public bool ShowTypingIndicator
{
    get => showTypingIndicator;
    set { showTypingIndicator = value; RaisePropertyChanged(nameof(ShowTypingIndicator)); }
}

// Initialize in constructor
public ViewModel()
{
    // ...
    TypingIndicator = new TypingIndicator
    {
        Author = new Author { Name = "AI Assistant" }
    };
    ShowTypingIndicator = false;
}
```

---

## Binding TypingIndicator in XAML

```xaml
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    Suggestions="{Binding Suggestions}"
    ShowTypingIndicator="{Binding ShowTypingIndicator}"
    TypingIndicator="{Binding TypingIndicator}" />
```

- `TypingIndicator` — defines **who** is typing (Author name/avatar shown in the indicator)
- `ShowTypingIndicator` — controls **visibility** of the indicator

Both must be bound for the indicator to work correctly.

---

## Toggling with Async AI Calls

Always toggle `ShowTypingIndicator` around the async AI call:

```csharp
ShowTypingIndicator = true;
try
{
    var response = await aiService.GetResponseAsync(userMessage);
    // Add AI message to Chats...
}
catch (Exception ex)
{
    // Handle error — add error message to Chats
    Chats.Add(new TextMessage
    {
        Author = new Author { Name = "System" },
        Text = $"Error: {ex.Message}"
    });
}
finally
{
    ShowTypingIndicator = false; // Always hide, even on error
}
```

**Use `finally`** to ensure the indicator is always hidden even when the AI call throws an exception.

---

## Gotchas

- **`TypingIndicator` object must be initialized** — if it's `null`, the animation won't display even when `ShowTypingIndicator` is `true`.
- **`Suggestions = null` clears chips** — setting to `null` removes all suggestion chips from the UI.
- **Suggestions are not automatically cleared** when the user clicks one — clear them manually in `CollectionChanged` when a new user message is added.
- **`ShowTypingIndicator` without binding** — you can also set it as a static `True` value in XAML for testing: `ShowTypingIndicator="True"`.

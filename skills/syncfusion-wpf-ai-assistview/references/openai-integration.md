# OpenAI Integration with SfAIAssistView

## Table of Contents
- [Required NuGet Packages](#required-nuget-packages)
- [AIMessage Class](#aimessage-class)
- [Setting Up the AI Service](#setting-up-the-ai-service)
- [ViewModel with CollectionChanged Trigger](#viewmodel-with-collectionchanged-trigger)
- [ViewTemplateSelector for Markdown Rendering](#viewtemplateselector-for-markdown-rendering)
- [Coordinating Typing Indicator](#coordinating-typing-indicator)

---

## Required NuGet Packages

```
Syncfusion.SfChat.Wpf
Microsoft.SemanticKernel
MdXaml  (optional, for Markdown rendering)
```

---

## AIMessage Class

Use `AIMessage` (instead of `TextMessage`) for AI-generated responses. It supports:
- `Author` — the AI author with optional `ContentTemplate` (icon)
- `Text` — plain text response
- `Solution` — alternative property for response text
- `DateTime` — timestamp
- `ContentTemplate` — custom rendering template

```csharp
// AI author with an icon
DataTemplate aiIconTemplate = (DataTemplate)FindResource("AIIconTemplate");
var aiAuthor = new Author
{
    Name = "AI Assistant",
    ContentTemplate = aiIconTemplate
};

// Add AI response
Chats.Add(new AIMessage
{
    Author = aiAuthor,
    Text = "Here is the response from the AI.",
    DateTime = DateTime.Now
});
```

---

## Setting Up the AI Service

Create a service class that wraps SemanticKernel:

```csharp
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.ChatCompletion;

public class AIAssistChatService
{
    private readonly IChatCompletionService chatService;
    private readonly ChatHistory chatHistory;

    public AIAssistChatService(string apiKey, string modelId = "gpt-4o")
    {
        var kernel = Kernel.CreateBuilder()
            .AddOpenAIChatCompletion(modelId, apiKey)
            .Build();

        chatService = kernel.GetRequiredService<IChatCompletionService>();
        chatHistory = new ChatHistory("You are a helpful AI assistant.");
    }

    public async Task<string> GetResponseAsync(string userMessage)
    {
        chatHistory.AddUserMessage(userMessage);
        var result = await chatService.GetChatMessageContentAsync(chatHistory);
        var response = result.Content ?? string.Empty;
        chatHistory.AddAssistantMessage(response);
        return response;
    }
}
```

---

## ViewModel with CollectionChanged Trigger

Subscribe to `CollectionChanged` on the `Chats` collection to automatically invoke the AI service when the user sends a message:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> chats;
    private bool showTypingIndicator;
    private Author currentUser;
    private AIAssistChatService aiService;

    public ObservableCollection<object> Chats
    {
        get => chats;
        set { chats = value; RaisePropertyChanged(nameof(Chats)); }
    }

    public bool ShowTypingIndicator
    {
        get => showTypingIndicator;
        set { showTypingIndicator = value; RaisePropertyChanged(nameof(ShowTypingIndicator)); }
    }

    public Author CurrentUser
    {
        get => currentUser;
        set { currentUser = value; RaisePropertyChanged(nameof(CurrentUser)); }
    }

    public ViewModel()
    {
        CurrentUser = new Author { Name = "User" };
        Chats = new ObservableCollection<object>();
        aiService = new AIAssistChatService(apiKey: "YOUR_API_KEY");

        // Subscribe to trigger AI responses
        Chats.CollectionChanged += OnChatsCollectionChanged;
    }

    private async void OnChatsCollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
    {
        if (e.Action != NotifyCollectionChangedAction.Add) return;
        if (e.NewItems[0] is not TextMessage msg) return;
        if (msg.Author.Name != CurrentUser.Name) return; // Only react to user messages

        ShowTypingIndicator = true;
        try
        {
            var response = await aiService.GetResponseAsync(msg.Text);
            Application.Current.Dispatcher.Invoke(() =>
            {
                Chats.Add(new AIMessage
                {
                    Author = new Author { Name = "AI Assistant" },
                    Text = response,
                    DateTime = DateTime.Now
                });
            });
        }
        finally
        {
            ShowTypingIndicator = false;
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void RaisePropertyChanged(string propName)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
}
```

**Key pattern:**
1. User types → `TextMessage` added to `Chats` → `CollectionChanged` fires
2. Check `msg.Author.Name == CurrentUser.Name` to avoid infinite loop
3. Enable `ShowTypingIndicator` → call AI → disable indicator → add `AIMessage`
4. Always dispatch UI updates to the main thread with `Application.Current.Dispatcher.Invoke`

---

## ViewTemplateSelector for Markdown Rendering

When AI responses contain Markdown, use a `DataTemplateSelector` to render them properly via MdXaml:

```csharp
// MessageTemplateSelector.cs
public class MessageTemplateSelector : DataTemplateSelector
{
    public DataTemplate TextMessageTemplate { get; set; }
    public DataTemplate AIMessageTemplate { get; set; }

    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        return item is AIMessage ? AIMessageTemplate : TextMessageTemplate;
    }
}
```

```xaml
<!-- MainWindow.xaml Resources -->
<Window.Resources>
    <!-- Requires MdXaml NuGet package -->
    <DataTemplate x:Key="AIMessageTemplate">
        <mdxaml:MarkdownScrollViewer
            Markdown="{Binding Text}"
            MarkdownStyle="{StaticResource MdXamlStyle}" />
    </DataTemplate>

    <DataTemplate x:Key="TextMessageTemplate">
        <TextBlock Text="{Binding Text}" TextWrapping="Wrap" />
    </DataTemplate>

    <local:MessageTemplateSelector
        x:Key="MessageTemplateSelector"
        AIMessageTemplate="{StaticResource AIMessageTemplate}"
        TextMessageTemplate="{StaticResource TextMessageTemplate}" />
</Window.Resources>

<!-- Bind the selector -->
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    ViewTemplateSelector="{StaticResource MessageTemplateSelector}" />
```

---

## Coordinating Typing Indicator

Full XAML binding for the typing indicator alongside OpenAI:

```xaml
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    ShowTypingIndicator="{Binding ShowTypingIndicator}"
    TypingIndicator="{Binding TypingIndicator}"
    ViewTemplateSelector="{StaticResource MessageTemplateSelector}" />
```

```csharp
// Initialize TypingIndicator in ViewModel constructor
TypingIndicator = new TypingIndicator
{
    Author = new Author { Name = "AI Assistant" }
};
```

The typing indicator shows an animated "AI is typing" indicator in the chat area whenever `ShowTypingIndicator` is `true`.

---

## Gotchas

- **Dispatcher required** — `CollectionChanged` may fire on a background thread; always dispatch `Chats.Add()` to the UI thread.
- **Avoid infinite loops** — check `msg.Author.Name == CurrentUser.Name` before triggering AI, otherwise AI responses will trigger more AI calls.
- **`AIMessage` vs `TextMessage`** — use `AIMessage` for AI responses when using `ViewTemplateSelector` to differentiate rendering.
- **API key security** — never hardcode API keys in production; use configuration providers or environment variables.

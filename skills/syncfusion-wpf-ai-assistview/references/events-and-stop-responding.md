# Events and Stop Responding

## Table of Contents
- [PromptRequest Event](#promptrequest-event)
- [EnableStopResponding](#enablestopresponding)
- [StopResponding Event](#stopresponding-event)
- [StopRespondingCommand (MVVM)](#stoprespondingcommand-mvvm)
- [StopRespondingTemplate](#stoprespondingtemplate)
- [Full MVVM Stop Pattern](#full-mvvm-stop-pattern)

---

## PromptRequest Event

The `PromptRequest` event fires when the user submits a message from the input field. Use it to intercept, validate, or process the user's input before it is added to the chat.

```xaml
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    PromptRequest="OnPromptRequest" />
```

```csharp
private void OnPromptRequest(object sender, PromptRequestEventArgs e)
{
    // e.InputMessage — the text the user typed
    string userInput = e.InputMessage;

    // Validate input
    if (string.IsNullOrWhiteSpace(userInput))
    {
        e.Handled = true; // Prevents the message from being added to the chat
        return;
    }

    // Optional: modify or log the input
    Console.WriteLine($"User prompt: {userInput}");

    // e.Handled = false (default) — allows normal processing to continue
}
```

**`PromptRequestEventArgs` properties:**

| Property | Type | Description |
|---|---|---|
| `InputMessage` | `string` | The user's input text |
| `Handled` | `bool` | Set to `true` to suppress default message-add behavior |

**When to use `e.Handled = true`:**
- Input validation (empty/too short)
- Custom routing of messages to different handlers
- Preventing duplicate submissions

---

## EnableStopResponding

The Stop Responding button lets users cancel an ongoing AI response. It is hidden by default.

```xaml
<!-- Enable Stop Responding button -->
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    EnableStopResponding="True" />
```

```csharp
// Code-behind
sfAIAssistView.EnableStopResponding = true;
```

When enabled, a Stop Responding button appears in the input area while `ShowTypingIndicator` is `true` (i.e., while AI is processing). The button disappears when the AI finishes.

---

## StopResponding Event

Handle the Stop Responding button click in code-behind:

```xaml
<syncfusion:SfAIAssistView
    EnableStopResponding="True"
    StopResponding="OnStopResponding" />
```

```csharp
private CancellationTokenSource cts;

private void OnStopResponding(object sender, EventArgs e)
{
    // Cancel the ongoing AI request
    cts?.Cancel();
    viewModel.ShowTypingIndicator = false;

    // Optionally notify the user
    viewModel.Chats.Add(new TextMessage
    {
        Author = new Author { Name = "System" },
        Text = "Response cancelled."
    });
}
```

**Integration with async AI calls:**
```csharp
// In ViewModel — pass CancellationToken to AI service
cts = new CancellationTokenSource();
try
{
    ShowTypingIndicator = true;
    var response = await aiService.GetResponseAsync(msg.Text, cts.Token);
    // ...
}
catch (OperationCanceledException)
{
    // Silently handle cancellation
}
finally
{
    ShowTypingIndicator = false;
}
```

---

## StopRespondingCommand (MVVM)

For MVVM, use `StopRespondingCommand` instead of the code-behind event:

```xaml
<syncfusion:SfAIAssistView
    EnableStopResponding="True"
    StopRespondingCommand="{Binding StopRespondingCommand}" />
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private CancellationTokenSource cts;

    public ICommand StopRespondingCommand { get; }

    public ViewModel()
    {
        StopRespondingCommand = new RelayCommand(ExecuteStopResponding);
    }

    private void ExecuteStopResponding()
    {
        cts?.Cancel();
        ShowTypingIndicator = false;

        Chats.Add(new AIMessage
        {
            Author = new Author { Name = "AI" },
            Text = "Response was stopped."
        });
    }

    private async Task SendMessageAsync(string userMessage)
    {
        cts = new CancellationTokenSource();
        ShowTypingIndicator = true;
        try
        {
            var response = await aiService.GetResponseAsync(userMessage, cts.Token);
            Chats.Add(new AIMessage { Author = aiAuthor, Text = response });
        }
        catch (OperationCanceledException) { /* cancelled by user */ }
        finally
        {
            ShowTypingIndicator = false;
        }
    }
}
```

---

## StopRespondingTemplate

Customize the appearance of the Stop Responding button:

```xaml
<Window.Resources>
    <DataTemplate x:Key="StopTemplate">
        <Grid Background="Transparent">
            <Button Background="#CC3333"
                    Foreground="White"
                    Padding="12,6"
                    Content="⏹ Stop AI"
                    FontSize="13"
                    HorizontalAlignment="Center"
                    VerticalAlignment="Center" />
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:SfAIAssistView
    EnableStopResponding="True"
    StopRespondingTemplate="{StaticResource StopTemplate}" />
```

```csharp
// Code-behind
if (TryFindResource("StopTemplate") is DataTemplate template)
{
    sfAIAssistView.StopRespondingTemplate = template;
}
```

---

## Full MVVM Stop Pattern

Complete implementation combining `EnableStopResponding`, `StopRespondingCommand`, and `CancellationToken`:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private CancellationTokenSource cts;
    public ICommand StopRespondingCommand { get; }
    public bool ShowTypingIndicator { get; set; }
    public ObservableCollection<object> Chats { get; set; }

    public ViewModel()
    {
        Chats = new ObservableCollection<object>();
        StopRespondingCommand = new RelayCommand(
            execute: () =>
            {
                cts?.Cancel();
                cts = null;
                ShowTypingIndicator = false;
                Chats.Add(new AIMessage
                {
                    Author = new Author { Name = "AI" },
                    Text = "You stopped the response."
                });
            },
            canExecute: () => ShowTypingIndicator // Only enabled while AI is processing
        );
    }
}
```

---

## Gotchas

- **`EnableStopResponding` must be `true`** — the button only appears when explicitly enabled.
- **Stop button visibility is linked to `ShowTypingIndicator`** — if you forget to set `ShowTypingIndicator=True` during AI processing, the button won't appear.
- **`e.Handled = true` in `PromptRequest`** prevents `TextMessage` from being added to `Chats` — only set this if you want full control over message insertion.
- **`CancellationToken` must be passed to the AI service** — `StopRespondingCommand` only cancels the token; the AI service must respect it with `await task.WithCancellation(token)` or similar.

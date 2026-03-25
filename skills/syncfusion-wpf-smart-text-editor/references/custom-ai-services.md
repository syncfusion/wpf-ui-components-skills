# Custom AI Service Integration

## Table of Contents
- [Overview](#overview)
- [IChatInferenceService Interface](#ichatinferenceservice-interface)
- [Claude AI](#claude-ai)
- [DeepSeek](#deepseek)
- [Gemini](#gemini)
- [Groq](#groq)
- [Choosing the Right Custom Provider](#choosing-the-right-custom-provider)
- [Troubleshooting Custom AI Services](#troubleshooting-custom-ai-services)

## Overview

While the Syncfusion WPF Smart Components work seamlessly with built-in providers like Azure OpenAI, OpenAI, and Ollama (see [ai-service-configuration.md](ai-service-configuration.md)), you can also integrate custom AI services using the `IChatInferenceService` interface. This allows you to:

**Use Alternative AI Providers:**
- Claude AI (Anthropic)
- DeepSeek AI
- Google Gemini AI
- Groq AI
- Any custom or proprietary AI service

**Benefits of Custom Integration:**
- Flexibility to use any AI provider
- Control over API communication and response parsing
- Support for specialized or domain-specific models
- Integration with internal AI infrastructure

This guide covers the `IChatInferenceService` interface pattern and provides complete implementation examples for Claude, DeepSeek, Gemini, and Groq.

## IChatInferenceService Interface

When using a provider not supported by `Microsoft.Extensions.AI` out of the box, implement `IChatInferenceService` to bridge the editor and your AI backend. The interface has a single method:

```csharp
using Syncfusion.UI.Xaml.SmartComponents;

public interface IChatInferenceService
{
    Task<string> GenerateResponseAsync(List<ChatMessage> chatMessages);
}
```

- **`chatMessages`** — contains the user's text and context history
- **Return value** — the AI-generated completion string inserted as a suggestion

**Key difference from built-in providers:** When you register an `IChatInferenceService` implementation, you do **not** call `ConfigureSyncfusionAIServices()` in `App.xaml.cs` — the custom service takes over entirely.

**Registration pattern (all custom services):**
```csharp
SyncfusionAIExtension.Services.AddSingleton<IChatInferenceService, YourCustomService>();
```
---

## Claude AI

Use Anthropic's Claude models for high-quality, context-aware completions.

**Setup:**
1. Create an account at [console.anthropic.com](https://console.anthropic.com)
2. Generate an API key under **API Keys**
3. Review available models at [docs.anthropic.com/claude/docs/models-overview](https://docs.anthropic.com/claude/docs/models-overview)

### Request / Response Models (`ClaudeModels.cs`)

```csharp
public class ClaudeChatRequest
{
    public string? Model { get; set; }
    public int Max_tokens { get; set; }
    public List<ClaudeMessage>? Messages { get; set; }
    public List<string>? Stop_sequences { get; set; }
}

public class ClaudeMessage
{
    public string? Role { get; set; }
    public string? Content { get; set; }
}

public class ClaudeChatResponse
{
    public List<ClaudeContentBlock>? Content { get; set; }
}

public class ClaudeContentBlock
{
    public string? Text { get; set; }
}
```

### AI Service (`ClaudeAIService.cs`)

```csharp
using System.Net;
using System.Text;
using System.Text.Json;
using Microsoft.Extensions.AI;

public class ClaudeAIService
{
    private readonly string _apiKey    = "YOUR_CLAUDE_API_KEY";
    private readonly string _modelName = "claude-3-5-sonnet-20241022";
    private readonly string _endpoint  = "https://api.anthropic.com/v1/messages";

    private static readonly HttpClient HttpClient = new(new SocketsHttpHandler
    {
        PooledConnectionLifetime = TimeSpan.FromMinutes(30),
        EnableMultipleHttp2Connections = true
    }) { DefaultRequestVersion = HttpVersion.Version20 };

    private static readonly JsonSerializerOptions JsonOptions = new()
    {
        PropertyNamingPolicy = JsonNamingPolicy.CamelCase
    };

    public ClaudeAIService()
    {
        if (!HttpClient.DefaultRequestHeaders.Contains("x-api-key"))
        {
            HttpClient.DefaultRequestHeaders.Clear();
            HttpClient.DefaultRequestHeaders.Add("x-api-key", _apiKey);
            HttpClient.DefaultRequestHeaders.Add("anthropic-version", "2023-06-01");
        }
    }

    public async Task<string> CompleteAsync(List<ChatMessage> chatMessages)
    {
        var request = new ClaudeChatRequest
        {
            Model       = _modelName,
            Max_tokens  = 1000,
            Messages    = chatMessages.Select(m => new ClaudeMessage
            {
                Role    = m.Role == ChatRole.User ? "user" : "assistant",
                Content = m.Text
            }).ToList(),
            Stop_sequences = new List<string> { "END_INSERTION", "NEED_INFO", "END_RESPONSE" }
        };

        var content = new StringContent(
            JsonSerializer.Serialize(request, JsonOptions), Encoding.UTF8, "application/json");

        var response = await HttpClient.PostAsync(_endpoint, content);
        response.EnsureSuccessStatusCode();
        var json = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<ClaudeChatResponse>(json, JsonOptions);
        return result?.Content?.FirstOrDefault()?.Text ?? "No response from Claude.";
    }
}
```

### Inference Service + Registration

```csharp
// ClaudeInferenceService.cs
using Syncfusion.UI.Xaml.SmartComponents;

public class ClaudeInferenceService : IChatInferenceService
{
    private readonly ClaudeAIService _claude;
    public ClaudeInferenceService(ClaudeAIService claude) => _claude = claude;

    public Task<string> GenerateResponseAsync(List<ChatMessage> messages)
        => _claude.CompleteAsync(messages);
}
```

```csharp
// App.xaml.cs — no ConfigureSyncfusionAIServices() needed
SyncfusionAIExtension.Services.AddSingleton<ClaudeAIService>();
SyncfusionAIExtension.Services.AddSingleton<IChatInferenceService, ClaudeInferenceService>();
```

---

## DeepSeek

DeepSeek offers cost-effective models via an OpenAI-compatible Chat Completions endpoint.

**Setup:** Create an account at [platform.deepseek.com](https://platform.deepseek.com) and generate an API key.

### Request / Response Models (`DeepSeekModels.cs`)

```csharp
public class DeepSeekMessage      { public string? Role { get; set; } public string? Content { get; set; } }
public class DeepSeekChatRequest  { public string? Model { get; set; } public float Temperature { get; set; } public List<DeepSeekMessage>? Messages { get; set; } }
public class DeepSeekChatResponse { public List<DeepSeekChoice>? Choices { get; set; } }
public class DeepSeekChoice       { public DeepSeekMessage? Message { get; set; } }
```

### AI Service + Registration

```csharp
// DeepSeekAIService.cs
using Microsoft.Extensions.AI;
using System.Net; using System.Text; using System.Text.Json;

public class DeepSeekAIService
{
    private readonly string _apiKey    = "YOUR_DEEPSEEK_KEY";
    private readonly string _modelName = "deepseek-chat";
    private readonly string _endpoint  = "https://api.deepseek.com/v1/chat/completions";

    private static readonly HttpClient HttpClient = new(new SocketsHttpHandler
        { PooledConnectionLifetime = TimeSpan.FromMinutes(30) })
        { DefaultRequestVersion = HttpVersion.Version20 };
    private static readonly JsonSerializerOptions JsonOptions = new()
        { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };

    public DeepSeekAIService()
    {
        if (!HttpClient.DefaultRequestHeaders.Contains("Authorization"))
        {
            HttpClient.DefaultRequestHeaders.Clear();
            HttpClient.DefaultRequestHeaders.Add("Authorization", $"Bearer {_apiKey}");
        }
    }

    public async Task<string> CompleteAsync(List<ChatMessage> messages)
    {
        var request = new DeepSeekChatRequest
        {
            Model       = _modelName,
            Temperature = 0.7f,
            Messages    = messages.Select(m => new DeepSeekMessage
            {
                Role    = m.Role == ChatRole.User ? "user" : "system",
                Content = m.Text
            }).ToList()
        };
        var payload  = new StringContent(JsonSerializer.Serialize(request, JsonOptions), Encoding.UTF8, "application/json");
        var response = await HttpClient.PostAsync(_endpoint, payload);
        response.EnsureSuccessStatusCode();
        var json   = await response.Content.ReadAsStringAsync();
        var result = JsonSerializer.Deserialize<DeepSeekChatResponse>(json, JsonOptions);
        return result?.Choices?.FirstOrDefault()?.Message?.Content ?? "No response from DeepSeek.";
    }
}
```

```csharp
// DeepSeekInferenceService.cs + App.xaml.cs registration
public class DeepSeekInferenceService : IChatInferenceService
{
    private readonly DeepSeekAIService _service;
    public DeepSeekInferenceService(DeepSeekAIService service) => _service = service;
    public Task<string> GenerateResponseAsync(List<ChatMessage> messages) => _service.CompleteAsync(messages);
}

// App.xaml.cs — no ConfigureSyncfusionAIServices() needed
SyncfusionAIExtension.Services.AddSingleton<DeepSeekAIService>();
SyncfusionAIExtension.Services.AddSingleton<IChatInferenceService, DeepSeekInferenceService>();
```

---

## Gemini

Google's Gemini models via the Generative Language API. Includes configurable safety settings.

**Setup:** Get an API key from [Google AI Studio](https://ai.google.dev/gemini-api/docs/api-key).

### AI Service + Registration

```csharp
// GeminiService.cs
using System.Net; using System.Text; using System.Text.Json;
using Microsoft.Extensions.AI;

public class GeminiService
{
    private readonly string _apiKey    = "YOUR_GEMINI_KEY";
    private readonly string _modelName = "gemini-2.0-flash";
    private readonly string _endpoint  = "https://generativelanguage.googleapis.com/v1beta/models/";

    private static readonly HttpClient HttpClient = new(new SocketsHttpHandler
        { PooledConnectionLifetime = TimeSpan.FromMinutes(30) })
        { DefaultRequestVersion = HttpVersion.Version20 };
    private static readonly JsonSerializerOptions JsonOptions = new()
        { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };

    public GeminiService()
    {
        HttpClient.DefaultRequestHeaders.Clear();
        HttpClient.DefaultRequestHeaders.Add("x-goog-api-key", _apiKey);
    }

    public async Task<string> CompleteAsync(List<ChatMessage> messages)
    {
        var requestUri = $"{_endpoint}{_modelName}:generateContent";
        var contents   = messages.Select(m => new
        {
            role  = m.Role == ChatRole.User ? "user" : "model",
            parts = new[] { new { text = m.Text } }
        }).ToList();

        var parameters = new
        {
            contents,
            generationConfig = new { maxOutputTokens = 2000, stopSequences = new[] { "END_INSERTION", "NEED_INFO" } },
            safetySettings   = new[]
            {
                new { category = "HARM_CATEGORY_HARASSMENT",        threshold = "BLOCK_ONLY_HIGH" },
                new { category = "HARM_CATEGORY_HATE_SPEECH",       threshold = "BLOCK_ONLY_HIGH" },
                new { category = "HARM_CATEGORY_SEXUALLY_EXPLICIT", threshold = "BLOCK_ONLY_HIGH" },
                new { category = "HARM_CATEGORY_DANGEROUS_CONTENT", threshold = "BLOCK_ONLY_HIGH" }
            }
        };

        var payload  = new StringContent(JsonSerializer.Serialize(parameters, JsonOptions), Encoding.UTF8, "application/json");
        var response = await HttpClient.PostAsync(requestUri, payload);
        response.EnsureSuccessStatusCode();
        var json     = await response.Content.ReadAsStringAsync();
        using var doc = JsonDocument.Parse(json);
        return doc.RootElement
            .GetProperty("candidates")[0]
            .GetProperty("content")
            .GetProperty("parts")[0]
            .GetProperty("text")
            .GetString() ?? "No response from Gemini.";
    }
}
```

```csharp
// App.xaml.cs
public class GeminiInferenceService : IChatInferenceService
{
    private readonly GeminiService _service;
    public GeminiInferenceService(GeminiService service) => _service = service;
    public Task<string> GenerateResponseAsync(List<ChatMessage> messages) => _service.CompleteAsync(messages);
}

SyncfusionAIExtension.Services.AddSingleton<GeminiService>();
SyncfusionAIExtension.Services.AddSingleton<IChatInferenceService, GeminiInferenceService>();
```

---

## Groq

Groq delivers very low-latency inference using an OpenAI-compatible Chat Completions API.

**Setup:** Create an API key at [console.groq.com](https://console.groq.com). Review available models at [console.groq.com/docs/models](https://console.groq.com/docs/models).

### AI Service + Registration

```csharp
// GroqService.cs
using Microsoft.Extensions.AI;
using System.Net; using System.Text; using System.Text.Json;

public class GroqService
{
    private readonly string _apiKey    = "YOUR_GROQ_KEY";
    private readonly string _modelName = "llama3-8b-8192";
    private readonly string _endpoint  = "https://api.groq.com/openai/v1/chat/completions";

    private static readonly HttpClient HttpClient = new(new SocketsHttpHandler
        { PooledConnectionLifetime = TimeSpan.FromMinutes(30) })
        { DefaultRequestVersion = HttpVersion.Version20 };
    private static readonly JsonSerializerOptions JsonOptions = new()
        { PropertyNamingPolicy = JsonNamingPolicy.CamelCase };

    public GroqService()
    {
        if (!HttpClient.DefaultRequestHeaders.Contains("Authorization"))
        {
            HttpClient.DefaultRequestHeaders.Clear();
            HttpClient.DefaultRequestHeaders.Add("Authorization", $"Bearer {_apiKey}");
        }
    }

    public async Task<string> CompleteAsync(List<ChatMessage> messages)
    {
        var request = new
        {
            model    = _modelName,
            messages = messages.Select(m => new
            {
                role    = m.Role == ChatRole.User ? "user" : "assistant",
                content = m.Text
            }).ToList(),
            stop = new[] { "END_INSERTION", "NEED_INFO", "END_RESPONSE" }
        };
        var payload  = new StringContent(JsonSerializer.Serialize(request, JsonOptions), Encoding.UTF8, "application/json");
        var response = await HttpClient.PostAsync(_endpoint, payload);
        response.EnsureSuccessStatusCode();
        var json     = await response.Content.ReadAsStringAsync();
        using var doc = JsonDocument.Parse(json);
        return doc.RootElement
            .GetProperty("choices")[0]
            .GetProperty("message")
            .GetProperty("content")
            .GetString() ?? "No response from Groq.";
    }
}
```

```csharp
// App.xaml.cs
public class GroqInferenceService : IChatInferenceService
{
    private readonly GroqService _service;
    public GroqInferenceService(GroqService service) => _service = service;
    public Task<string> GenerateResponseAsync(List<ChatMessage> messages) => _service.CompleteAsync(messages);
}

SyncfusionAIExtension.Services.AddSingleton<GroqService>();
SyncfusionAIExtension.Services.AddSingleton<IChatInferenceService, GroqInferenceService>();
```

---

## Choosing the Right Custom Provider

### Comparison Matrix

| Provider | Speed | Cost | Quality | Best For |
|----------|-------|------|---------|----------|
| **Claude** | Moderate | $$$ | Excellent | Complex reasoning, long-form content |
| **DeepSeek** | Fast | $ | Good | Cost-effective applications |
| **Gemini** | Fast | $$ | Excellent | Multimodal applications, Google ecosystem |
| **Groq** | Ultra-fast | $$ | Good | Low-latency, real-time applications |

### Use Claude When:
- Need superior reasoning and analysis
- Long-form content generation
- Code understanding and generation
- Ethical AI considerations are important

### Use DeepSeek When:
- Budget constraints are critical
- Good-enough quality is acceptable
- High volume of requests
- Testing and development

### Use Gemini When:
- Integrating with Google services
- Need multimodal capabilities (future)
- Prefer Google's AI ecosystem
- Want fast, high-quality responses

### Use Groq When:
- Ultra-low latency is critical
- Real-time applications (chat, live suggestions)
- Need fastest possible inference
- Using open-source models (Llama, Mistral)

## Troubleshooting Custom AI Services

### No Suggestions Displayed

**Problem:** Custom AI service registered but no suggestions appear.

**Solutions:**
- Verify `IChatInferenceService` implementation is registered correctly
- Check API key validity
- Ensure `GenerateResponseAsync` returns valid responses
- Add debug logging to track method calls
- Verify network connectivity

### Invalid API Response

**Problem:** Receiving errors or null responses from AI API.

**Solutions:**
- Check API endpoint URL is correct
- Verify request/response models match API documentation
- Inspect HTTP status codes and error messages
- Test API directly with tools like Postman
- Review API provider's status page for outages

### Slow Suggestion Performance

**Problem:** Suggestions take too long to appear.

**Solutions:**
- Use faster models (smaller parameter counts)
- Reduce `Max_tokens` or `MaxOutputTokens`
- Implement request caching for common patterns
- Consider switching to faster provider (e.g., Groq)
- Check network latency

### Authentication Errors

**Problem:** Receiving 401 Unauthorized or 403 Forbidden errors.

**Solutions:**
- Verify API key is correct and active
- Check API key permissions and quotas
- Ensure proper authentication header format
- Regenerate API key if compromised
- Verify account billing is active

### JSON Serialization Errors

**Problem:** Errors deserializing API responses.

**Solutions:**
- Verify response models match API documentation
- Use `JsonSerializerOptions` with proper naming policy
- Handle nullable properties appropriately
- Log raw JSON response for debugging
- Update models if API has changed

### Memory Leaks or Performance Degradation

**Problem:** Application slows down or uses excessive memory.

**Solutions:**
- Reuse HttpClient instances (static shared instance)
- Properly dispose of HTTP responses
- Implement connection pooling with `SocketsHttpHandler`
- Set `PooledConnectionLifetime` to avoid stale connections
- Monitor memory usage and profile application

### Rate Limiting

**Problem:** Receiving 429 Too Many Requests errors.

**Solutions:**
- Implement exponential backoff retry logic
- Throttle request rate
- Upgrade API plan for higher limits
- Cache frequent suggestions
- Debounce user input before triggering AI

---

**Related Topics:**
- Built-in AI providers (Azure OpenAI, OpenAI, Ollama): [ai-service-configuration.md](ai-service-configuration.md)
- Initial setup and installation: [getting-started.md](getting-started.md)
- Customize suggestion appearance: [customization.md](customization.md)

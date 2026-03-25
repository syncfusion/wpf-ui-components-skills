---
name: syncfusion-wpf-smart-text-editor
description: Implements Syncfusion WPF Smart Text Editor (SfSmartTextEditor), an AI-powered multiline input control with intelligent text completion. Use this when implementing smart text editors, AI-powered text input, or predictive text suggestions in WPF applications. The control provides context-aware typing assistance through integration with AI services including Azure OpenAI, OpenAI, Ollama, Claude, Gemini, Groq, and DeepSeek. Features include dual suggestion display modes (inline/popup), custom phrase libraries for offline suggestions, full text customization, and comprehensive command handling for workflow integration.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Smart Text Editor

## When to Use This Skill

Use this skill when users need to:
- **Implement AI-powered text input** with predictive suggestions and context-aware completions
- **Add intelligent autocomplete** to multiline text editors in WPF applications
- **Integrate AI services** (Azure OpenAI, OpenAI, Ollama, or custom providers) for text generation
- **Create smart text fields** for customer support, email composition, or content creation
- **Build responsive text editors** with inline or popup suggestion display modes
- **Customize suggestion behavior** with user roles, custom phrases, and styling options
- **Handle text change events** and implement command patterns for text editors
- **Support offline suggestions** with custom phrase libraries when AI is unavailable

The Smart Text Editor (SfSmartTextEditor) is ideal for scenarios requiring intelligent typing assistance, branded response templates, and AI-enhanced user input.

## Component Overview

The **Syncfusion WPF Smart Text Editor** is a multiline input control that uses predictive suggestions to speed up typing. It integrates with AI inference services for context-aware completions and provides inline or popup suggestion display modes. Key capabilities include:

**Core Features:**
- AI-powered suggestions via IChatInferenceService
- Inline and popup suggestion display modes
- Custom phrase library for offline suggestions
- Keyboard integration (Tab, Right Arrow to accept)
- Gesture support (tap/click for popup suggestions)
- Maximum length validation
- Placeholder text with customizable styling
- Full UI customization (fonts, colors, sizes)
- Command support (TextChangedCommand)

**Supported AI Providers:**
- Azure OpenAI
- OpenAI
- Ollama (self-hosted models)
- Custom services via IChatInferenceService interface
- Claude AI (Anthropic)
- DeepSeek AI
- Google Gemini AI
- Groq AI

## Documentation and Navigation Guide

### Getting Started and Installation
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing Syncfusion.SfSmartComponents.WPF NuGet package
- Adding control via Designer, XAML, or C#
- Configuring UserRole and UserPhrases for suggestions
- Registering AI services in App.xaml.cs
- Running the application and verifying AI features

**When to read:** Starting a new Smart Text Editor implementation, setting up the development environment, or adding the control to an existing WPF project.

### Suggestion Display Modes
📄 **Read:** [references/suggestion-display-modes.md](references/suggestion-display-modes.md)
- Inline mode: Suggestions rendered in place after caret
- Popup mode: Compact hint overlay near caret
- Keyboard shortcuts (Tab, Right Arrow)
- Gesture support (tap/click)
- Choosing between Inline and Popup modes

**When to read:** User needs to configure how suggestions appear, switch between display modes, or optimize for desktop vs touch devices.

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)
- Text style customization (FontSize, Foreground, fonts)
- Placeholder text and color customization
- Suggestion text color (SuggestionInlineStyle)
- Suggestion popup background (SuggestionPopupStyle)
- Maximum input length (MaxLength property)
- Complete styling examples

**When to read:** Customizing appearance to match application themes, changing text styles, configuring placeholders, or setting character limits.

### Commands 
📄 **Read:** [references/commands.md](references/commands.md)
- TextChangedCommand overview and implementation
- Command binding in XAML
- ViewModel pattern for command handling
- Event handling patterns
- Use cases for tracking text changes

**When to read:** Implementing text change notifications, MVVM command patterns, or custom event handling logic.

### AI Service Configuration (Built-in Providers)
📄 **Read:** [references/ai-service-configuration.md](references/ai-service-configuration.md)
- Registering chat clients in App.xaml.cs
- Azure OpenAI configuration (deployment, API key, endpoint)
- OpenAI configuration (API key, model selection)
- Ollama self-hosted models (installation, local setup)
- Required NuGet packages for each provider
- Troubleshooting built-in AI providers

**When to read:** Configuring Azure OpenAI, OpenAI, or Ollama for AI-powered suggestions. Use this for official/built-in AI provider setup.

### Custom AI Service Integration
📄 **Read:** [references/custom-ai-services.md](references/custom-ai-services.md)
- `IChatInferenceService` interface and when to use it
- Claude AI — request/response models, service class, registration
- DeepSeek — chat completions integration and registration
- Gemini — Google AI Studio setup, safety settings, registration
- Groq — low-latency OpenAI-compatible endpoint, registration
- No need to call `ConfigureSyncfusionAIServices()` when using custom services
- Choosing the right custom provider
- Troubleshooting custom AI implementations

**When to read:** Integrating custom AI services, implementing Claude/DeepSeek/Gemini/Groq, or creating a custom IChatInferenceService adapter.

## Quick Start Example

Here's a minimal example to get started with the Smart Text Editor:

### 1. Install NuGet Package

```powershell
Install-Package Syncfusion.SfSmartComponents.WPF
```

### 2. Add Control in XAML

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="YourApp.MainWindow">
    <Grid>
        <syncfusion:SfSmartTextEditor 
            x:Name="smartTextEditor"
            Placeholder="Type your response..."
            UserRole="Customer support agent responding to inquiries">
            <syncfusion:SfSmartTextEditor.UserPhrases>
                <x:String>Thank you for reaching out.</x:String>
                <x:String>We'll investigate and get back to you shortly.</x:String>
            </syncfusion:SfSmartTextEditor.UserPhrases>
        </syncfusion:SfSmartTextEditor>
    </Grid>
</Window>
```

### 3. Configure AI Service (App.xaml.cs)

```csharp
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using Microsoft.Extensions.DependencyInjection;
using Syncfusion.UI.Xaml.SmartComponents;
using System.ClientModel;
using System.Windows;

public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        base.OnStartup(e);
        
        // Configure Azure OpenAI
        string azureApiKey = "<YOUR-API-KEY>";
        Uri azureEndpoint = new Uri("<YOUR-ENDPOINT>");
        string deploymentName = "<YOUR-DEPLOYMENT>";

        AzureOpenAIClient azureClient = new AzureOpenAIClient(
            azureEndpoint, 
            new ApiKeyCredential(azureApiKey)
        );
        IChatClient chatClient = azureClient
            .GetChatClient(deploymentName)
            .AsIChatClient();
        
        SyncfusionAIExtension.Services.AddSingleton<IChatClient>(chatClient);
        SyncfusionAIExtension.ConfigureSyncfusionAIServices();
    }
}
```

## Common Patterns

### Pattern 1: Support Ticket Response Editor

**Scenario:** Customer support application with branded responses.

```xaml
<syncfusion:SfSmartTextEditor
    Placeholder="Write your response..."
    UserRole="Technical support engineer helping customers with product issues"
    MaxLength="2000"
    SuggestionDisplayMode="Inline">
    <syncfusion:SfSmartTextEditor.UserPhrases>
        <x:String>Thanks for contacting Syncfusion support.</x:String>
        <x:String>Could you please share a reproducible sample?</x:String>
        <x:String>We've logged this as a bug and will update you soon.</x:String>
        <x:String>Let us know if you need further assistance.</x:String>
    </syncfusion:SfSmartTextEditor.UserPhrases>
</syncfusion:SfSmartTextEditor>
```

### Pattern 2: Email Composition with Custom Styling

**Scenario:** Email client with custom theme and popup suggestions.

```xaml
<syncfusion:SfSmartTextEditor
    Placeholder="Compose your email..."
    UserRole="Professional email author"
    SuggestionDisplayMode="Popup"
    MaxLength="5000">
    <syncfusion:SfSmartTextEditor.Style>
        <Style TargetType="{x:Type syncfusion:SfSmartTextEditor}">
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="Foreground" Value="#333333"/>
            <Setter Property="FontFamily" Value="Segoe UI"/>
        </Style>
    </syncfusion:SfSmartTextEditor.Style>
    <syncfusion:SfSmartTextEditor.SuggestionPopupStyle>
        <Style TargetType="syncfusion:SuggestionPopup">
            <Setter Property="Background" Value="#0078D4"/>
            <Setter Property="Foreground" Value="White"/>
        </Style>
    </syncfusion:SfSmartTextEditor.SuggestionPopupStyle>
</syncfusion:SfSmartTextEditor>
```

### Pattern 3: Text Editor with Change Tracking

**Scenario:** Track text changes for auto-save or validation.

```xaml
<syncfusion:SfSmartTextEditor
    x:Name="smartTextEditor"
    TextChangedCommand="{Binding TextChangedCommand}"
    Placeholder="Start typing..."/>
```

```csharp
public class EditorViewModel : INotifyPropertyChanged
{
    public ICommand TextChangedCommand { get; }

    public EditorViewModel()
    {
        TextChangedCommand = new RelayCommand(OnTextChanged);
    }

    private void OnTextChanged()
    {
        // Auto-save, validation, or tracking logic
        Debug.WriteLine("Text changed - triggering auto-save");
    }
}
```

### Pattern 4: Offline-First with Custom Phrases

**Scenario:** App that works without AI connectivity.

```xaml
<syncfusion:SfSmartTextEditor
    Placeholder="Type your message..."
    UserRole="Sales representative responding to leads">
    <syncfusion:SfSmartTextEditor.UserPhrases>
        <x:String>Thank you for your interest in our products.</x:String>
        <x:String>I'd be happy to schedule a demo.</x:String>
        <x:String>Our pricing starts at $99 per month.</x:String>
        <x:String>Let me connect you with our solutions team.</x:String>
        <x:String>Feel free to reach out with any questions.</x:String>
    </syncfusion:SfSmartTextEditor.UserPhrases>
</syncfusion:SfSmartTextEditor>
```

**Note:** If no AI service is configured, the editor automatically falls back to UserPhrases for suggestions.

## Key Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Text` | `string` | Gets or sets the text content | `""` |
| `Placeholder` | `string` | Placeholder text when editor is empty | `null` |
| `UserRole` | `string` | Describes who is typing and intent (shapes AI suggestions) | `null` |
| `UserPhrases` | `List<string>` | Custom phrase library for offline suggestions | `null` |
| `SuggestionDisplayMode` | `SuggestionDisplayMode` | Display mode: `Inline` or `Popup` | `Inline` |
| `MaxLength` | `int` | Maximum character limit | `0` (unlimited) |
| `TextChangedCommand` | `ICommand` | Command triggered when text changes | `null` |
| `Style` | `Style` | Text style (FontSize, Foreground, etc.) | Default |
| `PlaceholderStyle` | `Style` | Placeholder styling | Default |
| `SuggestionInlineStyle` | `Style` | Inline suggestion text style | Default |
| `SuggestionPopupStyle` | `Style` | Popup suggestion background style | Default |

## Common Use Cases

### When User Needs Inline Suggestions (Desktop)
- **Action:** Set `SuggestionDisplayMode="Inline"`
- **Why:** Natural typing flow on keyboard-driven devices
- **Reference:** [suggestion-display-modes.md](references/suggestion-display-modes.md)

### When User Needs Popup Suggestions (Touch Devices)
- **Action:** Set `SuggestionDisplayMode="Popup"`
- **Why:** Easier to tap/click on mobile/touch screens
- **Reference:** [suggestion-display-modes.md](references/suggestion-display-modes.md)

### When User Needs Azure OpenAI Integration
- **Action:** Configure `AzureOpenAIClient` in App.xaml.cs
- **Why:** Enterprise-grade AI with Azure security and compliance
- **Reference:** [ai-service-configuration.md](references/ai-service-configuration.md)

### When User Needs Custom AI Provider (Claude, Gemini, etc.)
- **Action:** Implement `IChatInferenceService` interface
- **Why:** Use alternative AI providers or custom inference logic
- **Reference:** [custom-ai-services.md](references/custom-ai-services.md)

### When User Needs Character Limit
- **Action:** Set `MaxLength` property
- **Why:** Enforce input validation and prevent excessive text
- **Reference:** [customization.md](references/customization.md)

### When User Needs Text Change Notifications
- **Action:** Bind `TextChangedCommand` to ViewModel
- **Why:** Track changes for auto-save, validation, or analytics
- **Reference:** [commands.md](references/commands.md)

### When User Needs Offline Suggestions
- **Action:** Configure `UserPhrases` without AI service
- **Why:** Provide suggestions when AI is unavailable or disabled
- **Reference:** [getting-started.md](references/getting-started.md)

### When User Needs Custom Theme
- **Action:** Apply `Style`, `PlaceholderStyle`, `SuggestionInlineStyle`
- **Why:** Match application branding and design system
- **Reference:** [customization.md](references/customization.md)

---

**Next Steps:**
1. For initial setup and installation → Read [getting-started.md](references/getting-started.md)
2. For AI provider configuration → Read [ai-service-configuration.md](references/ai-service-configuration.md)
3. For custom AI services → Read [custom-ai-services.md](references/custom-ai-services.md)
4. For appearance customization → Read [customization.md](references/customization.md)

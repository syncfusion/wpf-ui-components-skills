# AI Service Configuration (Built-in Providers)

## Table of Contents
- [Overview](#overview)
- [Azure OpenAI Configuration](#azure-openai-configuration)
- [OpenAI Configuration](#openai-configuration)
- [Ollama Configuration (Self-Hosted)](#ollama-configuration-self-hosted)
- [Choosing the Right Provider](#choosing-the-right-provider)
- [Troubleshooting](#troubleshooting)

## Overview

The WPF Smart Text Editor uses AI chat inference services to generate intelligent, context-aware text suggestions. The component supports three official/built-in AI providers out of the box:

**Supported Built-in Providers:**
1. **Azure OpenAI** - Enterprise-grade AI with Azure security and compliance
2. **OpenAI** - Direct integration with OpenAI's GPT models
3. **Ollama** - Self-hosted open-source models running locally

All built-in providers use the `IChatClient` interface from the `Microsoft.Extensions.AI` package, which is registered in your `App.xaml.cs` file using dependency injection. The Smart Components resolve the chat client automatically and use it for generating suggestions.


## Azure OpenAI Configuration

Azure OpenAI provides enterprise-grade AI capabilities with Azure's security, compliance, and regional availability. This is the recommended option for production applications requiring data privacy and SLA guarantees.

### Install Required NuGet Packages

Install the following packages via NuGet Package Manager or Package Manager Console:

**Package Manager Console:**
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
Install-Package Azure.AI.OpenAI
```

**.NET CLI:**
```bash
dotnet add package Microsoft.Extensions.AI
dotnet add package Microsoft.Extensions.AI.OpenAI
dotnet add package Azure.AI.OpenAI
```

### Configure Azure OpenAI in App.xaml.cs

Add the following configuration to your `App.xaml.cs` file:

```csharp
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using Microsoft.Extensions.DependencyInjection;
using Syncfusion.UI.Xaml.SmartComponents;
using System;
using System.ClientModel;
using System.Windows;

namespace YourAppNamespace
{
    /// <summary>
    /// Interaction logic for App.xaml
    /// </summary>
    public partial class App : Application
    {
        protected override void OnStartup(StartupEventArgs e)
        {
            base.OnStartup(e);

            // Azure OpenAI configuration
            string azureApiKey = "YOUR_AZURE_OPENAI_KEY";
            Uri azureEndpoint = new Uri("YOUR_AZURE_OPENAI_ENDPOINT");
            string deploymentName = "YOUR_DEPLOYMENT_NAME";

            // Create Azure OpenAI client
            AzureOpenAIClient azureClient = new AzureOpenAIClient(
                azureEndpoint, 
                new ApiKeyCredential(azureApiKey)
            );
            
            // Get chat client and convert to IChatClient
            IChatClient azureChatClient = azureClient
                .GetChatClient(deploymentName)
                .AsIChatClient();
            
            // Register with Syncfusion AI Extension
            SyncfusionAIExtension.Services.AddSingleton<IChatClient>(azureChatClient);
            
            // Configure Syncfusion AI Services
            SyncfusionAIExtension.ConfigureSyncfusionAIServices();
        }
    }
}
```

> Deploy an Azure OpenAI resource and model via the `Azure Portal` first. The `azureEndpoint`, `azureKey`, and `deploymentName` values are available in your resource's **Keys and Endpoint** blade.

## OpenAI Configuration

OpenAI provides direct access to GPT models without requiring Azure infrastructure. This is suitable for development, testing, or applications without specific compliance requirements.

### Install Required NuGet Packages

**Package Manager Console:**
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
```

**.NET CLI:**
```bash
dotnet add package Microsoft.Extensions.AI
dotnet add package Microsoft.Extensions.AI.OpenAI
```

### Configure OpenAI in App.xaml.cs

```csharp
using Microsoft.Extensions.AI;
using Microsoft.Extensions.DependencyInjection;
using OpenAI;
using Syncfusion.UI.Xaml.SmartComponents;
using System;
using System.ClientModel;
using System.Windows;

namespace YourAppNamespace
{
    public partial class App : Application
    {
        protected override void OnStartup(StartupEventArgs e)
        {
            base.OnStartup(e);

            // OpenAI configuration
            string openAIApiKey = "YOUR_OPENAI_API_KEY";
            string openAIModel = "gpt-4o-mini"; // or gpt-4, gpt-3.5-turbo

            // Create OpenAI client
            var openAIClient = new OpenAIClient(
                new ApiKeyCredential(openAIApiKey),
                new OpenAIClientOptions
                {
                    // Default OpenAI endpoint; include /v1 if your SDK expects it
                    Endpoint =  new Uri("YOUR_API_KEY") // "https://api.openai.com/v1/"
                }
            );

            // Get chat client and convert to IChatClient
            IChatClient openAIChatClient = openAIClient
                .GetChatClient(openAIModel)
                .AsIChatClient();
            
            // Register with Syncfusion AI Extension
            SyncfusionAIExtension.Services.AddSingleton<IChatClient>(openAIChatClient);
            
            // Configure Syncfusion AI Services
            SyncfusionAIExtension.ConfigureSyncfusionAIServices();
        }
    }
}
```

> Get your API key from `platform.openai.com/api-keys`. Recommended models: `gpt-4o-mini` (cost-efficient) or `gpt-4o` (higher quality).

## Ollama Configuration (Self-Hosted)

Ollama allows you to run open-source language models locally on your machine. This is ideal for offline scenarios, data privacy, or avoiding cloud API costs.

**Setup steps:**
1. Download and install Ollama from `ollama.com`
2. Pull a model: `ollama pull llama3` (or any model from `ollama.com/library`)
3. Ollama runs at `http://localhost:11434` by default

### Install Required NuGet Packages

**Package Manager Console:**
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package OllamaSharp
```

**.NET CLI:**
```bash
dotnet add package Microsoft.Extensions.AI
dotnet add package OllamaSharp
```

### Configure Ollama in App.xaml.cs

```csharp
using Microsoft.Extensions.AI;
using Microsoft.Extensions.DependencyInjection;
using OllamaSharp;
using Syncfusion.UI.Xaml.SmartComponents;
using System.Windows;

namespace YourAppNamespace
{
    public partial class App : Application
    {
        protected override void OnStartup(StartupEventArgs e)
        {
            base.OnStartup(e);

            // Ollama configuration
            string ollamaEndpoint = "http://localhost:11434";
            string modelName = "llama2:13b"; // Model you installed

            // Create Ollama chat client
            IChatClient chatClient = new OllamaApiClient(ollamaEndpoint, modelName);
            
            // Register with Syncfusion AI Extension
            SyncfusionAIExtension.Services.AddSingleton<IChatClient>(chatClient);
            
            // Configure Syncfusion AI Services
            SyncfusionAIExtension.ConfigureSyncfusionAIServices();
        }
    }
}
```

### Ollama Best Practices

**1. Choose the Right Model Size:**
- Smaller models (7B parameters): Faster, less accurate, lower RAM usage
- Larger models (13B+ parameters): Slower, more accurate, higher RAM usage

**2. Verify Ollama Service:**
- Ensure Ollama is running before starting your application
- Check with: `ollama serve` (starts Ollama server if not running)

**3. Performance Optimization:**
- Use GPU acceleration if available (Ollama auto-detects)
- Close other memory-intensive applications
- Consider quantized models for better performance

**4. Offline Capability:**
- Once models are downloaded, no internet connection is required
- Perfect for air-gapped environments or sensitive data

## Choosing the Right Provider

### Decision Matrix

| Factor | Azure OpenAI | OpenAI | Ollama |
|--------|-------------|---------|---------|
| **Cost** | Pay-as-you-go (Azure pricing) | Pay-as-you-go (OpenAI pricing) | Free (local compute) |
| **Data Privacy** | High (enterprise controls) | Moderate (OpenAI policies) | Highest (fully local) |
| **Setup Complexity** | Moderate (Azure deployment) | Easy (API key only) | Moderate (install + models) |
| **Performance** | High (cloud infrastructure) | High (cloud infrastructure) | Variable (depends on hardware) |
| **Offline Support** | No | No | Yes |
| **Compliance** | Azure compliance certifications | OpenAI policies | Full control |
| **Best For** | Enterprise, production | Development, small-scale | Offline, privacy-sensitive |

### Use Azure OpenAI When:
- Building enterprise applications with compliance requirements
- Need SLA guarantees and dedicated support
- Data residency is important (Azure regions)
- Budget allows for premium service

### Use OpenAI When:
- Rapid prototyping or development
- Small to medium-scale applications
- Simplicity is a priority (minimal setup)
- No specific compliance requirements

### Use Ollama When:
- Offline capability is required
- Data privacy is critical (no cloud transmission)
- Budget is constrained (no API costs)
- Have sufficient local compute resources
- Building for air-gapped environments

## Troubleshooting

### No Suggestions Appear

**Possible Causes:**
1. AI service not registered correctly in App.xaml.cs
2. Invalid API key or endpoint
3. Network connectivity issues (Azure OpenAI, OpenAI)
4. Ollama service not running (Ollama)

**Solutions:**
- Verify AI service registration code
- Check API keys and endpoints in configuration
- Test network connectivity
- For Ollama: Run `ollama serve` to start service
- Check Debug console for error messages

### "Unauthorized" or "Authentication Failed" Errors

**Azure OpenAI:**
- Verify API key is correct in Azure Portal
- Check that endpoint URL matches your resource
- Ensure deployment name is correct

**OpenAI:**
- Verify API key from OpenAI Platform
- Check API key hasn't expired or been revoked
- Ensure billing is set up (OpenAI requires payment method)

**Ollama:**
- No authentication required for local Ollama

### Slow Suggestion Generation

**Azure OpenAI / OpenAI:**
- Check network latency
- Consider using smaller models (gpt-4o-mini)
- Reduce suggestion length with stop sequences

**Ollama:**
- Use smaller models (7B instead of 13B)
- Close memory-intensive applications
- Upgrade hardware (more RAM, GPU)
- Use quantized models

### Rate Limiting Errors

**Azure OpenAI:**
- Check Azure OpenAI rate limits in Portal
- Request quota increase if needed

**OpenAI:**
- Monitor usage in OpenAI Dashboard
- Upgrade to higher tier for increased limits
- Implement request throttling

**Ollama:**
- No rate limits (local execution)

### Connection Timeout

**Azure OpenAI / OpenAI:**
- Verify internet connectivity
- Check firewall/proxy settings
- Increase timeout values in HTTP client configuration

**Ollama:**
- Verify Ollama is running: `ollama list`
- Check endpoint URL (default: http://localhost:11434)
- Ensure model is downloaded: `ollama pull <model-name>`

### Application Crashes on Startup

**Common Issues:**
- Missing NuGet packages
- Incorrect package versions
- NullReferenceException in AI configuration

**Solutions:**
- Verify all required NuGet packages are installed
- Restore NuGet packages: Right-click solution → Restore NuGet Packages
- Wrap AI configuration in try-catch for debugging:

```csharp
protected override void OnStartup(StartupEventArgs e)
{
    base.OnStartup(e);

    try
    {
        // AI configuration code here
    }
    catch (Exception ex)
    {
        MessageBox.Show($"AI configuration error: {ex.Message}", "Error", 
            MessageBoxButton.OK, MessageBoxImage.Error);
    }
}
```

---

**Related Topics:**
- Initial setup and installation: [getting-started.md](getting-started.md)
- Customize suggestion appearance: [customization.md](customization.md)

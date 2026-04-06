# AI Service Setup

## Overview

The Smart DataGrid's AI-powered features require integration with Azure OpenAI services. This guide covers how to register and configure the AI service to enable natural language commands for sorting, filtering, grouping, and highlighting.

**Without AI service configuration, smart actions will not work.** You must set up the Azure OpenAI client and register it with the Syncfusion AI extension before the Smart DataGrid can process natural language prompts.

## Prerequisites

Before setting up the AI service, ensure you have:

1. **Azure OpenAI Account** – Active Azure subscription with OpenAI service enabled
2. **API Key** – Your Azure OpenAI API key
3. **Endpoint URL** – Your Azure OpenAI service endpoint
4. **Deployment Name** – The name of your deployed model

### Getting Azure OpenAI Credentials

1. Sign in to the [Azure Portal](https://portal.azure.com/)
2. Navigate to your Azure OpenAI resource
3. Go to **Keys and Endpoint** section
4. Copy:
   - **Key 1** or **Key 2** (API key)
   - **Endpoint** (URL)
   - **Deployment name** (from Deployments section)

## Registering the AI Service

The AI service must be registered in the `App.xaml.cs` file before the application starts. This ensures the service is available when the Smart DataGrid initializes.

### Step 1: Add Required Namespaces

Add these using statements to your `App.xaml.cs` file:

```csharp
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using Microsoft.Extensions.DependencyInjection;
using Syncfusion.UI.Xaml.SmartComponents;
using System.ClientModel;
using System.Windows;
```

### Step 2: Configure AI Service in App Constructor

Register the AI service in the `App` class constructor:

```csharp
namespace WpfApplication1
{
    public partial class App : Application
    {
        public App()
        {
            // Step 1: Set your Azure OpenAI credentials
            string azureApiKey = "<MENTION-YOUR-KEY>";
            Uri azureEndpoint = new Uri("<MENTION-YOUR-URL>");
            string deploymentName = "<MENTION-YOUR-DEPLOYMENT-NAME>";

            // Step 2: Create the Azure OpenAI client
            AzureOpenAIClient azureClient = new AzureOpenAIClient(
                azureEndpoint,
                new ApiKeyCredential(azureApiKey)
            );

            // Step 3: Get the chat client
            IChatClient azureChatClient = azureClient
                .GetChatClient(deploymentName)
                .AsIChatClient();

            // Step 4: Register the chat client with Syncfusion AI Extension
            SyncfusionAIExtension.Services.AddSingleton<IChatClient>(azureChatClient);
            
            // Step 5: Configure Syncfusion AI services
            SyncfusionAIExtension.ConfigureSyncfusionAIServices();
        }
    }
}
```

### Configuration Breakdown

| Step | Code | Purpose |
|------|------|---------|
| **1. Set Credentials** | `string azureApiKey = "..."` | Store your Azure OpenAI credentials |
| **2. Create Client** | `new AzureOpenAIClient(...)` | Initialize the Azure OpenAI client |
| **3. Get Chat Client** | `GetChatClient(...).AsIChatClient()` | Create a chat client for AI interactions |
| **4. Register Service** | `AddSingleton<IChatClient>(...)` | Register the client with dependency injection |
| **5. Configure Syncfusion AI** | `ConfigureSyncfusionAIServices()` | Initialize Syncfusion AI services |

## Complete App.xaml.cs Example

Here's a complete example with proper error handling and best practices:

```csharp
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using Microsoft.Extensions.DependencyInjection;
using Syncfusion.UI.Xaml.SmartComponents;
using System;
using System.ClientModel;
using System.Windows;

namespace WpfApplication1
{
    /// <summary>
    /// Interaction logic for App.xaml
    /// </summary>
    public partial class App : Application
    {
        public App()
        {
            try
            {
                // Azure OpenAI configuration
                string azureApiKey = "<YOUR-API-KEY-HERE>";
                Uri azureEndpoint = new Uri("<YOUR-ENDPOINT-URL-HERE>");
                string deploymentName = "<YOUR-DEPLOYMENT-NAME-HERE>";

                // Validate credentials
                if (string.IsNullOrWhiteSpace(azureApiKey) || 
                    azureApiKey.Contains("<YOUR-") ||
                    azureEndpoint.ToString().Contains("<YOUR-"))
                {
                    throw new InvalidOperationException(
                        "Azure OpenAI credentials not configured. " +
                        "Please update App.xaml.cs with valid API key, endpoint, and deployment name."
                    );
                }

                // Create Azure OpenAI client
                AzureOpenAIClient azureClient = new AzureOpenAIClient(
                    azureEndpoint,
                    new ApiKeyCredential(azureApiKey)
                );

                // Get chat client for AI interactions
                IChatClient azureChatClient = azureClient
                    .GetChatClient(deploymentName)
                    .AsIChatClient();

                // Register with Syncfusion AI Extension
                SyncfusionAIExtension.Services.AddSingleton<IChatClient>(azureChatClient);
                SyncfusionAIExtension.ConfigureSyncfusionAIServices();
            }
            catch (Exception ex)
            {
                MessageBox.Show(
                    $"Failed to configure AI service: {ex.Message}",
                    "AI Configuration Error",
                    MessageBoxButton.OK,
                    MessageBoxImage.Error
                );
            }
        }
    }
}
```

## Configuration Parameters

### API Key
- **Type:** String
- **Format:** Long alphanumeric string from Azure portal
- **Location:** Azure Portal → Your OpenAI Resource → Keys and Endpoint → Key 1 or Key 2
- **Example:** `"a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6"`

### Endpoint URL
- **Type:** Uri
- **Format:** `https://<resource-name>.openai.azure.com/`
- **Location:** Azure Portal → Your OpenAI Resource → Keys and Endpoint → Endpoint
- **Example:** `new Uri("https://my-openai-resource.openai.azure.com/")`

### Deployment Name
- **Type:** String
- **Format:** Name you assigned when deploying the model
- **Location:** Azure Portal → Your OpenAI Resource → Deployments → Deployment name
- **Example:** `"gpt-4"` or `"gpt-35-turbo"`

## Required NuGet Packages

Ensure these NuGet packages are installed:

```xml
<PackageReference Include="Azure.AI.OpenAI" Version="*" />
<PackageReference Include="Microsoft.Extensions.AI" Version="*" />
<PackageReference Include="Microsoft.Extensions.AI.Abstractions" Version="*" />
<PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="*" />
<PackageReference Include="Syncfusion.SfSmartComponents.WPF" Version="*" />
```

Install via Package Manager Console:
```powershell
Install-Package Azure.AI.OpenAI
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.DependencyInjection
Install-Package Syncfusion.SfSmartComponents.WPF
```

## Troubleshooting

### Issue 1: "AI service not configured" Error

**Symptoms:**
- Smart actions don't execute
- Natural language prompts have no effect

**Solution:**
- Verify `App.xaml.cs` has AI service registration code
- Ensure `ConfigureSyncfusionAIServices()` is called
- Check that the constructor runs before MainWindow loads

### Issue 2: Authentication Failures

**Symptoms:**
- Errors like "Invalid API key" or "Unauthorized"
- Exception when executing prompts

**Solution:**
- Verify API key is correct (copy from Azure Portal)
- Ensure endpoint URL matches your Azure resource
- Check that deployment name exists and is active
- Verify Azure subscription is active

### Issue 3: "Deployment not found" Error

**Symptoms:**
- Error mentioning deployment name not found

**Solution:**
- Go to Azure Portal → Your OpenAI Resource → Deployments
- Verify the deployment name exactly matches
- Ensure the model is deployed and not deleted
- Check deployment status is "Succeeded"

### Issue 4: Network or Timeout Errors

**Symptoms:**
- Slow response or timeout exceptions
- Network connectivity errors

**Solution:**
- Check internet connectivity
- Verify firewall/proxy settings allow Azure OpenAI traffic
- Ensure Azure OpenAI service region is accessible
- Check Azure service health status

### Issue 5: Dependency Injection Errors

**Symptoms:**
- "IChatClient not registered" exception
- Service resolution failures

**Solution:**
- Ensure `AddSingleton<IChatClient>()` is called before `ConfigureSyncfusionAIServices()`
- Verify all using statements are added
- Check NuGet packages are installed correctly

## Security Best Practices

### 1. Never Hardcode Credentials in Source Code

**Bad Practice:**
```csharp
string azureApiKey = "a1b2c3d4e5f6..."; // DON'T DO THIS
```

**Good Practice:**
```csharp
// Store in app.config or user secrets
string azureApiKey = ConfigurationManager.AppSettings["AzureOpenAIKey"];
```

### 2. Use Configuration Files

**app.config:**
```xml
<configuration>
  <appSettings>
    <add key="AzureOpenAIKey" value="your-key-here"/>
    <add key="AzureOpenAIEndpoint" value="https://your-resource.openai.azure.com/"/>
    <add key="AzureOpenAIDeployment" value="gpt-4"/>
  </appSettings>
</configuration>
```

**Load in App.xaml.cs:**
```csharp
string azureApiKey = ConfigurationManager.AppSettings["AzureOpenAIKey"];
Uri azureEndpoint = new Uri(ConfigurationManager.AppSettings["AzureOpenAIEndpoint"]);
string deploymentName = ConfigurationManager.AppSettings["AzureOpenAIDeployment"];
```

### 3. Use Environment Variables

```csharp
string azureApiKey = Environment.GetEnvironmentVariable("AZURE_OPENAI_KEY");
Uri azureEndpoint = new Uri(Environment.GetEnvironmentVariable("AZURE_OPENAI_ENDPOINT"));
string deploymentName = Environment.GetEnvironmentVariable("AZURE_OPENAI_DEPLOYMENT");
```

### 4. Implement Proper Error Handling

Always wrap service registration in try-catch blocks to handle configuration errors gracefully.

## Verifying AI Service Setup

After configuration, verify the service is working:

1. **Run the application**
2. **Open the Smart DataGrid**
3. **Try a simple prompt:** `"sort by OrderID descending"`
4. **Check for response:** Grid should sort accordingly

If the grid responds to prompts, AI service is configured correctly.

## Next Steps

After configuring the AI service:
1. Enable smart actions: `EnableSmartActions="True"`
2. Test natural language commands
3. Add suggested prompts for user guidance
4. Implement error handling for AI responses
5. Monitor AI service usage and costs in Azure Portal

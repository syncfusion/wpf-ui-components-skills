# Getting Started with WPF Smart Text Editor

## Table of Contents
- [Overview](#overview)
- [Step 1: Install the NuGet Package](#step-1-install-the-nuget-package)
- [Step 2: Adding Control via Designer](#step-2-adding-control-via-designer)
- [Step 3: Adding Control Manually in XAML](#step-3-adding-control-manually-in-xaml)
- [Step 4: Adding Control Manually in C#](#step-4-adding-control-manually-in-c)
- [Step 5: Configure UserRole and UserPhrases](#step-5-configure-userrole-and-userphrases)
- [Step 6: Register the AI Service](#step-6-register-the-ai-service)
- [Step 7: Running the Application](#step-7-running-the-application)

## Overview

This guide explains how to add the WPF Smart Text Editor (SfSmartTextEditor) control to your application. The Smart Text Editor is an AI-powered multiline input control that provides intelligent, context-aware suggestions as users type. It covers the basic setup needed to get started with installation, configuration, and AI service integration.

**Note:** The Smart Text Editor is distributed as part of the `Syncfusion.SfSmartComponents.WPF` package, which provides advanced AI-assisted features to enhance text editing and content management. Ensure your application has the required AI service configuration to enable these features.

## Step 1: Install the NuGet Package

The Smart Text Editor is part of the `Syncfusion.SfSmartComponents.WPF` package.

**Package Manager Console:**
```bash
Install-Package Syncfusion.SfSmartComponents.WPF
```

**CLI:**
```bash
dotnet add package Syncfusion.SfSmartComponents.WPF
```

Or search for `Syncfusion.SfSmartComponents.WPF` in the NuGet Package Manager UI and install the latest version.

> The `Syncfusion.Data.WPF`, `Syncfusion.Shared.WPF`,`Syncfusion.SfGrid.WPF`,`Syncfusion.SfChat.WPF`,`Microsoft.Extensions.AI.Abstractions`,`Microsoft.Extensions.DependencyInjection`  packages is a required dependency and is installed automatically.

## Step 2: Adding Control via Designer

The SfSmartTextEditor control can be added to the application by dragging it from the Toolbox and dropping it in the Designer view:

1. Open the WPF Designer for your window or user control
2. Locate **SfSmartTextEditor** in the Toolbox
   - If not visible, ensure the NuGet package is installed
   - Try rebuilding the solution
3. Drag the control from the Toolbox to the Designer surface
4. The required assembly references will be added automatically

The Designer will automatically generate the XAML markup with the necessary namespace declarations.

## Step 3: Adding Control Manually in XAML

To add the control manually in XAML, follow these steps:

### 1. Add Assembly Reference

Ensure the following assembly reference is added to your project:
- `Syncfusion.SfSmartComponents.WPF`
- `Syncfusion.Data.WPF`
- `Syncfusion.Shared.WPF`
- `Syncfusion.SfGrid.WPF`
- `Syncfusion.SfChat.WPF`
- `Microsoft.Extensions.AI.Abstractions`
- `Microsoft.Extensions.DependencyInjection`

### 2. Import Syncfusion Namespace

Import the Syncfusion WPF schema or the SfSmartTextEditor control namespace in your XAML page.

**Option A: Using Syncfusion WPF Schema (Recommended)**

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

**Option B: Using CLR Namespace**

```xaml
xmlns:smarttexteditor="clr-namespace:Syncfusion.UI.Xaml.SmartComponents;assembly=Syncfusion.SfSmartComponents.WPF"
```

### 3. Declare SfSmartTextEditor Control

Add the control to your XAML layout:

**Using Syncfusion Schema:**

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="WpfApplication1.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <syncfusion:SfSmartTextEditor x:Name="smartTextEditor"/>
    </Grid>
</Window>
```

**Using CLR Namespace:**

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:smarttexteditor="clr-namespace:Syncfusion.UI.Xaml.SmartComponents;assembly=Syncfusion.SfSmartComponents.WPF"
        x:Class="WpfApplication1.MainWindow"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <smarttexteditor:SfSmartTextEditor x:Name="smartTextEditor"/>
    </Grid>
</Window>
```

## Step 4: Adding Control Manually in C#

To add the control programmatically in C#, follow these steps:

### 1. Add Assembly Reference

Ensure the following assembly reference is added to your project:
- `Syncfusion.SfSmartComponents.WPF`
- `Syncfusion.Data.WPF`
- `Syncfusion.Shared.WPF`
- `Syncfusion.SfGrid.WPF`
- `Syncfusion.SfChat.WPF`
- `Microsoft.Extensions.AI.Abstractions`
- `Microsoft.Extensions.DependencyInjection`

### 2. Import Namespace

Import the SfSmartTextEditor namespace in your code-behind file:

```csharp
using Syncfusion.UI.Xaml.SmartComponents;
```

### 3. Create and Add Control Instance

Create an instance of the SfSmartTextEditor control and add it to your layout:

```csharp
using Syncfusion.UI.Xaml.SmartComponents;

namespace WpfApplication1
{ 
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create Smart Text Editor instance
            SfSmartTextEditor smartTextEditor = new SfSmartTextEditor();
            
            // Optional: Configure properties
            smartTextEditor.Placeholder = "Type your message...";
            smartTextEditor.UserRole = "Customer support representative";
            
            // Add to grid (assumes a Grid named Root_Grid exists)
            Root_Grid.Children.Add(smartTextEditor);
        }
    }
}
```

## Step 5: Configure UserRole and UserPhrases

Set the writing context and preferred expressions to guide AI completions. These properties shape the tone, relevance, and content of suggestions.

### UserRole Property (Required)

The **UserRole** property describes who is typing and the intent. This shapes the tone and relevance of AI-generated suggestions.

**Examples:**
- "Customer support engineer responding to technical inquiries"
- "Sales representative following up with leads"
- "Content writer creating blog posts"
- "Email author responding to professional inquiries"

### UserPhrases Property (Optional)

The **UserPhrases** property is a list of reusable statements that reflect your brand voice or frequent responses. These phrases are used for:
- **Offline suggestions:** When AI is unavailable, the editor suggests from this list
- **Biasing AI completions:** AI considers these phrases when generating context-aware suggestions

**Configuration in XAML:**

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:smarttexteditor="clr-namespace:Syncfusion.UI.Xaml.SmartComponents;assembly=Syncfusion.SfSmartComponents.WPF">

    <smarttexteditor:SfSmartTextEditor
        Placeholder="Type your reply..."
        UserRole="Support engineer responding to customer tickets">
        <smarttexteditor:SfSmartTextEditor.UserPhrases>
            <x:String>Thanks for reaching out.</x:String>
            <x:String>Please share a minimal reproducible sample.</x:String>
            <x:String>We'll update you as soon as we have more details.</x:String>
            <x:String>Let us know if you need further assistance.</x:String>
        </smarttexteditor:SfSmartTextEditor.UserPhrases>
    </smarttexteditor:SfSmartTextEditor>
</Window>
```

**Configuration in C#:**

```csharp
SfSmartTextEditor smartTextEditor = new SfSmartTextEditor
{
    Placeholder = "Type your reply...",
    UserRole = "Support engineer responding to customer tickets",
    UserPhrases = new List<string>
    {
        "Thanks for reaching out.",
        "Please share a minimal reproducible sample.",
        "We'll update you as soon as we have more details.",
        "Let us know if you need further assistance."
    }
};
```

**Note:** If no AI inference service is configured, the editor generates offline suggestions from your UserPhrases automatically.

## Step 6: Register the AI Service

To enable AI-powered suggestions, configure an AI service in the `App.xaml.cs` file. This example uses Azure OpenAI, but you can also use OpenAI, Ollama, or custom AI providers.

### Azure OpenAI Configuration

```csharp
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using Microsoft.Extensions.DependencyInjection;
using Syncfusion.UI.Xaml.SmartComponents;
using System.ClientModel;
using System.Windows;

namespace WpfApplication1
{
    /// <summary>
    /// Interaction logic for App.xaml
    /// </summary>
    public partial class App : Application
    {
        protected override void OnStartup(StartupEventArgs e)
        {
            base.OnStartup(e);

            // Azure OpenAI credentials
            string azureApiKey = "<MENTION-YOUR-KEY>";
            Uri azureEndpoint = new Uri("<MENTION-YOUR-URL>");
            string deploymentName = "<MENTION-YOUR-DEPLOYMENT-NAME>";

            // Create Azure OpenAI client
            AzureOpenAIClient azureClient = new AzureOpenAIClient(
                azureEndpoint, 
                new ApiKeyCredential(azureApiKey)
            );
            
            // Get chat client
            IChatClient azureChatClient = azureClient
                .GetChatClient(deploymentName)
                .AsIChatClient();
            
            // Register with Syncfusion AI Extension
            SyncfusionAIExtension.Services.AddSingleton<IChatClient>(azureChatClient);
            SyncfusionAIExtension.ConfigureSyncfusionAIServices();
        }
    }
}
```

**Required NuGet Packages for Azure OpenAI:**
- Microsoft.Extensions.AI
- Microsoft.Extensions.AI.OpenAI
- Azure.AI.OpenAI

**For other AI providers:**
- See [ai-service-configuration.md](ai-service-configuration.md) for OpenAI and Ollama setup
- See [custom-ai-services.md](custom-ai-services.md) for Claude, DeepSeek, Gemini, and Groq integration

## Step 7: Running the Application

1. Press **F5** or click **Start** to build and run the application
2. Once compiled, the Smart Text Editor will be displayed in the window
3. Start typing to see AI-powered suggestions appear
4. Accept suggestions using:
   - **Tab** key (inline and popup modes)
   - **Right Arrow** key (inline and popup modes)
   - **Tap/Click** on suggestion (popup mode only)

**Expected Behavior:**
- As you type, the AI generates context-aware suggestions based on your UserRole and UserPhrases
- Suggestions appear inline (default) or in a popup, depending on SuggestionDisplayMode
- If AI is unavailable, offline suggestions from UserPhrases are displayed

**Troubleshooting:**
- **No suggestions appear:** Verify AI service is registered correctly in App.xaml.cs
- **Compilation errors:** Ensure NuGet packages are installed and project is restored
- **AI connection errors:** Check API keys, endpoints, and network connectivity
- **Offline mode:** Add UserPhrases to enable suggestions without AI

---

**Next Steps:**
- Configure suggestion display modes: [suggestion-display-modes.md](suggestion-display-modes.md)
- Customize appearance and styling: [customization.md](customization.md)
- Set up additional AI providers: [ai-service-configuration.md](ai-service-configuration.md) or [custom-ai-services.md](custom-ai-services.md)

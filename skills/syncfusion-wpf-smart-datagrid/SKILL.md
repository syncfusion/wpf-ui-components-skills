---
name: syncfusion-wpf-smart-datagrid
description: Guide for implementing Syncfusion WPF Smart DataGrid (SfSmartDataGrid) with AI-powered natural language commands. Use this when creating AI-assisted data grids with natural language sorting, filtering, grouping, and highlighting in WPF applications. Covers Azure OpenAI integration, data binding, and natural language prompt handling for grid operations.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  platform: "WPF"
---

# Implementing Syncfusion WPF Smart DataGrid

This skill guides you in implementing the Syncfusion WPF Smart DataGrid (`SfSmartDataGrid`), an AI-powered data grid control that enables natural language-driven operations for sorting, filtering, grouping, and highlighting. The Smart DataGrid interprets user prompts and applies context-aware actions, making complex data operations intuitive and efficient.

## When to Use This Skill

Use this skill when the user needs to:
- **Implement AI-powered data grids** in WPF applications with natural language command support
- **Enable natural language sorting, filtering, grouping, or highlighting** without manual configuration
- **Set up Azure OpenAI integration** for grid intelligence features
- **Create data-driven applications** where users interact with grids using conversational prompts
- **Apply multi-column operations** (sort, filter, group) through single text commands
- **Build intuitive data exploration interfaces** with AssistView and suggested prompts
- **Customize AI behaviors** such as highlight colors, initial prompts, or enable/disable smart actions

This skill is essential for modern WPF data applications that leverage AI to simplify user interactions with complex datasets.

## Component Overview

The **SfSmartDataGrid** enhances traditional data grid functionality with:
- **AI Sorting** – Sort data using natural language (e.g., "sort by city descending")
- **Intelligent Filtering** – Apply filters with plain-language conditions (e.g., "filter Country equals Germany AND Revenue > 1000")
- **Smart Grouping** – Organize data with AI prompts (e.g., "group by Country and ShipCity")
- **Row and Cell Highlighting** – Emphasize data by describing conditions (e.g., "highlight rows where Total > 1000 color LightPink")
- **AssistView Integration** – Chat-like interface for entering prompts and viewing AI responses
- **Customizable Suggestions** – Predefined prompts to guide users toward common actions

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Creating a new WPF project for Smart DataGrid
- Installing required NuGet packages (Syncfusion.SfSmartComponents.WPF)
- Understanding assembly dependencies (Syncfusion.Data.WPF, Syncfusion.SfGrid.WPF, etc.)
- Adding the control via Designer, XAML, or C#
- Basic grid initialization and setup

### Data Model and Binding
📄 **Read:** [references/data-model-binding.md](references/data-model-binding.md)
- Creating data model classes with INotifyPropertyChanged
- Building ViewModels with ObservableCollection
- Binding ItemsSource to data collections
- Configuring columns (GridTextColumn, GridNumericColumn, GridDateTimeColumn)
- AutoGenerateColumns vs manual column definition
- Setting CurrentUser for AssistView message differentiation

### AI Service Setup
📄 **Read:** [references/ai-service-setup.md](references/ai-service-setup.md)
- Registering Azure OpenAI services in App.xaml.cs
- Configuring AzureOpenAIClient with API keys and endpoints
- Setting up IChatClient for AI integration
- Using SyncfusionAIExtension to configure AI services
- Troubleshooting AI service connection issues

### AI Sorting and Grouping
📄 **Read:** [references/ai-sorting-grouping.md](references/ai-sorting-grouping.md)
- Using natural language for single and multi-column sorting
- Applying ascending and descending sort orders via prompts
- Clearing sorting with commands
- Creating single-level and multi-level grouping with AI
- Combining multiple grouping operations
- Removing grouping through clear commands

### AI Filtering and Highlighting
📄 **Read:** [references/ai-filtering-highlighting.md](references/ai-filtering-highlighting.md)
- Applying filters with natural language conditions
- Using filter operators (equals, contains, greaterThan, lessThan, between)
- Combining filters with AND/OR logic
- Clearing applied filters
- Highlighting rows and cells based on conditions
- Specifying custom colors (named, hex, RGB) for highlights
- Using default highlight brush
- Clearing highlights selectively (all, row-only, cell-only)

### Customization and Events
📄 **Read:** [references/customization-events.md](references/customization-events.md)
- Setting CurrentUser for message differentiation
- Configuring Suggestions for predefined prompts
- Using Prompt property for auto-executed initial commands
- Enabling or disabling smart actions with EnableSmartActions
- Customizing HighlightBrush for visual emphasis
- Executing prompts programmatically with ExecutePrompt method
- Handling AssistViewRequest event to intercept or cancel requests

## Quick Start Example

Here's a minimal example to set up an AI-powered Smart DataGrid:

**XAML:**
```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        xmlns:local="clr-namespace:WpfApplication1"
        Title="Smart DataGrid Demo" Height="600" Width="900">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>

    <Grid>
        <syncfusion:SfSmartDataGrid x:Name="SmartGrid" 
                                    ItemsSource="{Binding OrderInfoCollection}" 
                                    CurrentUser="{Binding CurrentUser}" 
                                    Suggestions="{Binding AiSuggestions}" 
                                    EnableSmartActions="True"
                                    ColumnSizer="Star">
            <syncfusion:SfSmartDataGrid.Columns>
                <syncfusion:GridNumericColumn MappingName="OrderID" HeaderText="Order ID"/>
                <syncfusion:GridTextColumn MappingName="CustomerName" HeaderText="Customer"/>
                <syncfusion:GridTextColumn MappingName="ProductName" HeaderText="Product"/>
                <syncfusion:GridNumericColumn MappingName="Quantity"/>
                <syncfusion:GridNumericColumn MappingName="Freight"/>
            </syncfusion:SfSmartDataGrid.Columns>
        </syncfusion:SfSmartDataGrid>
    </Grid>
</Window>
```

**App.xaml.cs (AI Service Registration):**
```csharp
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using Microsoft.Extensions.DependencyInjection;
using Syncfusion.UI.Xaml.SmartComponents;
using System.ClientModel;

public partial class App : Application
{
    public App()
    {
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

### Pattern 1: Natural Language Sorting
**User Need:** Sort data by multiple columns without manual configuration.

**Prompt Examples:**
- `"sort by city in descending"`
- `"sort by city column in descending and Revenue ascending"`
- `"clear sorting"`

**When to Use:** User wants quick, intuitive sorting via text commands.

### Pattern 2: Intelligent Filtering with Conditions
**User Need:** Apply complex filters using plain language.

**Prompt Examples:**
- `"filter Country equals Germany"`
- `"filter Country equals Germany AND Revenue greaterThan 1000"`
- `"filter OrderID between 20251001 and 20251020"`
- `"clear filters"`

**When to Use:** User needs conditional data filtering without building predicates manually.

### Pattern 3: Multi-Level Grouping
**User Need:** Organize data hierarchically.

**Prompt Examples:**
- `"group by Country"`
- `"group by Country and ShipCity"`
- `"clear grouping"`

**When to Use:** User wants to analyze data in organized, nested groups.

### Pattern 4: Conditional Highlighting
**User Need:** Visually emphasize important data.

**Prompt Examples:**
- `"highlight rows where Total > 1000 color LightPink"`
- `"highlight cells in OrderID lessThan 1005 color Red"`
- `"highlight rows where Total > 1000 color LightPink AND highlight cells in OrderID lessThan 1005 color Red"`
- `"clear highlight"` / `"clear row highlight"` / `"clear cell highlight"`

**When to Use:** User wants visual indicators for critical data points.

### Pattern 5: Programmatic Prompt Execution
**User Need:** Execute AI actions without opening AssistView.

**Implementation:**
```csharp
// Execute prompt directly from code
SmartGrid.ExecutePrompt("Sort the OrderID by Descending");
SmartGrid.ExecutePrompt("filter PaymentStatus equals Not Paid");
```

**When to Use:** Programmatic workflows where AI actions are triggered by application logic.

### Pattern 6: Intercepting and Validating Requests
**User Need:** Control or validate AI requests before execution.

**Implementation:**
```csharp
SmartGrid.AssistViewRequest += (sender, e) =>
{
    // Access the prompt
    string userPrompt = e.Prompt;
    
    // Optionally cancel the request
    if (userPrompt.Contains("forbidden"))
    {
        e.Cancel = true;
    }
};
```

**When to Use:** Need to validate, log, or prevent certain AI operations.

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `ItemsSource` | IEnumerable | Data source for the grid |
| `CurrentUser` | Author | Identifies user for AssistView message alignment (required) |
| `Suggestions` | ObservableCollection\<string\> | Predefined prompts shown in AssistView |
| `Prompt` | string | Initial prompt auto-executed on AssistView open |
| `EnableSmartActions` | bool | Enable/disable AI-powered actions (default: false) |
| `HighlightBrush` | SolidColorBrush | Default brush for highlighting without color specified |
| `AutoGenerateColumns` | bool | Auto-generate columns from data model properties |
| `ColumnSizer` | GridLengthUnitType | Column sizing mode (Star, Auto, None) |

## Key Methods

| Method | Purpose |
|--------|---------|
| `ExecutePrompt(string prompt)` | Execute AI action programmatically without AssistView |

## Key Events

| Event | Purpose |
|-------|---------|
| `AssistViewRequest` | Triggered when user sends a request; provides Cancel option |

## Common Use Cases

1. **Business Intelligence Dashboards** – Enable analysts to explore data with natural language queries
2. **Order Management Systems** – Allow users to filter, sort, and highlight orders using conversational commands
3. **Financial Reporting** – Apply complex filters and grouping for financial data analysis
4. **Customer Relationship Management (CRM)** – Query customer data with intuitive prompts
5. **Inventory Management** – Quickly identify critical inventory levels with highlighting
6. **Sales Analytics** – Group and sort sales data by region, product, or time period
7. **Log Viewers** – Filter logs with natural language conditions
8. **Data Export Tools** – Pre-filter data before export using AI commands

## Next Steps

After implementing the Smart DataGrid:
1. Configure suggested prompts to guide users toward common operations
2. Test AI service connectivity and response accuracy
3. Customize highlight colors to match application theme
4. Add validation for user prompts via AssistViewRequest event
5. Explore programmatic prompt execution for automated workflows

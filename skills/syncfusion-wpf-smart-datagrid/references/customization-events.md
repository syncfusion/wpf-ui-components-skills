# Customization and Events

## Table of Contents
- [Overview](#overview)
- [CurrentUser Property](#currentuser-property)
- [Suggestions Property](#suggestions-property)
- [Prompt Property](#prompt-property)
- [EnableSmartActions Property](#enablesmartactions-property)
- [HighlightBrush Property](#highlightbrush-property)
- [ExecutePrompt Method](#executeprompt-method)
- [AssistViewRequest Event](#assistviewrequest-event)
- [Complete Customization Example](#complete-customization-example)

## Overview

The Smart DataGrid provides extensive customization options to control AI behavior, appearance, and user interactions. This guide covers properties, methods, and events that allow you to:

- Personalize the AssistView experience
- Provide predefined prompts for common actions
- Execute prompts programmatically
- Intercept and validate user requests
- Customize highlight colors
- Enable or disable smart actions dynamically

## CurrentUser Property

The `CurrentUser` property identifies the active user and is **required** for proper AssistView message differentiation.

### Purpose

The AssistView displays messages in a chat-like interface:
- **Outgoing messages** (user requests) – Aligned to one side
- **Incoming messages** (AI responses) – Aligned to the opposite side

Without `CurrentUser`, the control cannot distinguish between these message types, and all messages appear with identical alignment and styling.

### Type and Structure

**Type:** `Author`

**Properties:**
- `Name` – User's display name (string)

### Setting CurrentUser

**In ViewModel:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    private Author currentUser;

    public Author CurrentUser
    {
        get => currentUser;
        set
        {
            currentUser = value;
            RaisePropertyChanged(nameof(CurrentUser));
        }
    }

    public ViewModel()
    {
        CurrentUser = new Author { Name = "John" };
    }
}
```

**XAML Binding:**
```xml
<syncfusion:SfSmartDataGrid CurrentUser="{Binding CurrentUser}"/>
```

**Code-Behind:**
```csharp
SmartGrid.CurrentUser = new Author { Name = "John" };
```

### Dynamic User Changes

Update CurrentUser when the active user changes:

```csharp
private void OnUserChanged(string newUserName)
{
    viewModel.CurrentUser = new Author { Name = newUserName };
}
```

**Use Case:** Multi-user applications where different users interact with the same grid.

## Suggestions Property

The `Suggestions` property provides a predefined list of prompt suggestions that appear in the AssistView, helping users quickly select common actions without typing commands manually.

### Purpose

Suggestions guide users toward:
- Common operations for the dataset
- Proper prompt syntax
- Relevant queries for the data context

### Type and Structure

**Type:** `ObservableCollection<string>`

**Contains:** Array of prompt strings shown as clickable suggestions.

### Setting Suggestions

**In ViewModel:**
```csharp
public class ViewModel
{
    public ObservableCollection<string> AiSuggestions { get; } = new ObservableCollection<string>
    {
        "Which orders have a payment status of Not Paid?",
        "What are the top 10 orders with the highest freight cost?",
        "Which customers have placed the most orders?",
        "What are the orders shipped to Brazil?",
        "What is the total quantity of products ordered across all orders?",
    };
}
```

**XAML Binding:**
```xml
<syncfusion:SfSmartDataGrid Suggestions="{Binding AiSuggestions}"/>
```

**Code-Behind:**
```csharp
SmartGrid.Suggestions = new ObservableCollection<string>
{
    "Sort by OrderID descending",
    "Filter Country equals Germany",
    "Group by PaymentStatus"
};
```

### Dynamic Suggestions

Update suggestions based on context or user actions:

```csharp
public void UpdateSuggestionsForPaymentTracking()
{
    AiSuggestions.Clear();
    AiSuggestions.Add("Show all unpaid orders");
    AiSuggestions.Add("Filter PaymentStatus equals Not Paid");
    AiSuggestions.Add("Highlight unpaid high-value orders");
}
```

**Use Case:** Change suggestions based on application mode or current data state.

### Suggestions Best Practices

1. **Provide 3-7 suggestions** – Enough variety without overwhelming users
2. **Use natural language questions** – More intuitive than command syntax
3. **Align with user's workflow** – Suggest actions relevant to current context
4. **Mix query types** – Include sorting, filtering, grouping, highlighting examples
5. **Update dynamically** – Adjust suggestions based on data or user role

## Prompt Property

The `Prompt` property defines an initial prompt that is automatically executed when the AssistView opens for the first time.

### Purpose

Auto-execute a command to:
- Set initial grid state (sorted, filtered, grouped)
- Demonstrate AI capabilities on first load
- Apply default data view

### Setting Prompt

**XAML:**
```xml
<syncfusion:SfSmartDataGrid Prompt="Sort the Quantity column"/>
```

**Code-Behind:**
```csharp
SmartGrid.Prompt = "Sort the Quantity column";
```

### Behavior

- Prompt executes automatically when AssistView opens for the first time
- Only runs once (not on every AssistView open)
- User sees the result immediately

### Use Cases

**1. Default Sort:**
```xml
<syncfusion:SfSmartDataGrid Prompt="Sort by OrderDate descending"/>
```
Shows most recent orders first.

**2. Default Filter:**
```xml
<syncfusion:SfSmartDataGrid Prompt="Filter PaymentStatus equals Not Paid"/>
```
Shows unpaid orders by default.

**3. Default Grouping:**
```xml
<syncfusion:SfSmartDataGrid Prompt="Group by Country"/>
```
Organizes data by country on load.

## EnableSmartActions Property

The `EnableSmartActions` property determines whether AI actions can be applied to the DataGrid.

### Purpose

Controls whether natural language commands actually execute actions on the grid.

### Default Value

**Default:** `false`

When `false`:
- Natural language prompts are received but not executed
- Grid state does not change based on AI commands
- AssistView still displays messages

When `true`:
- Prompts are executed and actions are applied
- Grid state updates based on AI commands
- Sorting, filtering, grouping, and highlighting become active

### Setting EnableSmartActions

**XAML:**
```xml
<syncfusion:SfSmartDataGrid EnableSmartActions="True"/>
```

**Code-Behind:**
```csharp
SmartGrid.EnableSmartActions = true;
```

### Dynamic Enable/Disable

Toggle smart actions based on application state:

```csharp
private void OnAdminModeChanged(bool isAdminMode)
{
    // Enable AI features only for admins
    SmartGrid.EnableSmartActions = isAdminMode;
}
```

**Use Case:** Restrict AI features to specific user roles or application modes.

### Use Cases

**1. Disable During Data Loading:**
```csharp
SmartGrid.EnableSmartActions = false;  // Disable while loading
await LoadDataAsync();
SmartGrid.EnableSmartActions = true;   // Re-enable after load
```

**2. Role-Based Access:**
```csharp
SmartGrid.EnableSmartActions = (currentUser.Role == "Admin" || currentUser.Role == "Analyst");
```

**3. Feature Toggle:**
```csharp
// Enable based on user preference or license
SmartGrid.EnableSmartActions = userSettings.AiFeaturesEnabled;
```

## HighlightBrush Property

The `HighlightBrush` property controls the default brush used when smart actions apply visual highlights to rows or cells without a specific color in the prompt.

### Purpose

Provides a fallback color for highlights when the user doesn't specify a color in the command.

### Type

**Type:** `SolidColorBrush`

### Setting HighlightBrush

**XAML:**
```xml
<syncfusion:SfSmartDataGrid HighlightBrush="Orange"/>
```

**Code-Behind:**
```csharp
SmartGrid.HighlightBrush = new SolidColorBrush(Colors.Orange);
```

**Using Hex:**
```csharp
SmartGrid.HighlightBrush = new SolidColorBrush((Color)ColorConverter.ConvertFromString("#FFA500"));
```

**Using RGB:**
```csharp
SmartGrid.HighlightBrush = new SolidColorBrush(Color.FromRgb(255, 165, 0));
```

### Behavior

**When color is NOT specified in prompt:**
```
highlight rows where Total > 1000
```
Uses the `HighlightBrush` color (e.g., Orange).

**When color IS specified in prompt:**
```
highlight rows where Total > 1000 color Red
```
Uses the specified color (Red), ignoring `HighlightBrush`.

### Dynamic Highlight Color

Change highlight color based on theme or user preference:

```csharp
private void ApplyTheme(string theme)
{
    if (theme == "Dark")
        SmartGrid.HighlightBrush = new SolidColorBrush(Colors.DarkOrange);
    else
        SmartGrid.HighlightBrush = new SolidColorBrush(Colors.LightYellow);
}
```

## ExecutePrompt Method

The `ExecutePrompt` method executes AI actions programmatically without opening the AssistView.

### Purpose

Trigger AI commands from application code:
- Automated workflows
- Button click handlers
- Application initialization
- Scheduled actions

### Signature

```csharp
public void ExecutePrompt(string prompt)
```

### Usage

**Basic Execution:**
```csharp
SmartGrid.ExecutePrompt("Sort the OrderID by Descending");
```

**From Button Click:**
```csharp
private void ShowUnpaidOrders_Click(object sender, RoutedEventArgs e)
{
    SmartGrid.ExecutePrompt("filter PaymentStatus equals Not Paid");
}
```

**Multiple Sequential Commands:**
```csharp
private void ApplyDefaultView()
{
    SmartGrid.ExecutePrompt("clear filters");
    SmartGrid.ExecutePrompt("clear grouping");
    SmartGrid.ExecutePrompt("group by Country");
    SmartGrid.ExecutePrompt("sort by Freight descending");
}
```

### Use Cases

**1. Dashboard Initialization:**
```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Apply default view on startup
    SmartGrid.ExecutePrompt("group by PaymentStatus");
    SmartGrid.ExecutePrompt("sort by Freight descending");
}
```

**2. Quick Filter Buttons:**
```xml
<StackPanel Orientation="Horizontal">
    <Button Content="Show Unpaid" Click="ShowUnpaid_Click"/>
    <Button Content="Show High Value" Click="ShowHighValue_Click"/>
    <Button Content="Reset" Click="Reset_Click"/>
</StackPanel>
```

```csharp
private void ShowUnpaid_Click(object sender, RoutedEventArgs e)
{
    SmartGrid.ExecutePrompt("filter PaymentStatus equals Not Paid");
}

private void ShowHighValue_Click(object sender, RoutedEventArgs e)
{
    SmartGrid.ExecutePrompt("filter Freight greaterThan 1000");
}

private void Reset_Click(object sender, RoutedEventArgs e)
{
    SmartGrid.ExecutePrompt("clear filters");
}
```

**3. Scheduled Reports:**
```csharp
private void GenerateMonthlyReport()
{
    SmartGrid.ExecutePrompt("filter OrderDate greaterThan 2026-03-01");
    SmartGrid.ExecutePrompt("group by Country and PaymentStatus");
    SmartGrid.ExecutePrompt("sort by Freight descending");
    
    // Export or process filtered/grouped data
}
```

## AssistViewRequest Event

The `AssistViewRequest` event is triggered whenever a user sends a request through the AssistView. This event allows you to intercept, log, validate, or cancel requests before they are processed.

### Purpose

Intercept requests to:
- Validate prompts
- Log user actions
- Implement custom business logic
- Cancel inappropriate or unauthorized requests

### Event Arguments

**Type:** `AssistViewRequestEventArgs`

**Properties:**
- `Prompt` (string) – The user's natural language prompt
- `Cancel` (bool) – Set to `true` to prevent request from being processed

### Subscribing to the Event

**XAML:**
```xml
<syncfusion:SfSmartDataGrid AssistViewRequest="OnAssistRequest"/>
```

**Code-Behind:**
```csharp
private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    var prompt = e.Prompt;
    
    // Custom logic here
    
    // Optionally cancel the request
    e.Cancel = true;
}
```

**Pure Code:**
```csharp
SmartGrid.AssistViewRequest += OnAssistRequest;
```

### Use Cases

#### Use Case 1: Logging User Actions

Track all AI commands for audit or analytics:

```csharp
private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    // Log the prompt
    Logger.LogInfo($"User: {viewModel.CurrentUser.Name}, Prompt: {e.Prompt}");
}
```

#### Use Case 2: Validating Prompts

Prevent certain types of requests:

```csharp
private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    string prompt = e.Prompt.ToLower();
    
    // Block certain operations
    if (prompt.Contains("delete") || prompt.Contains("remove data"))
    {
        MessageBox.Show("Delete operations are not allowed.", "Unauthorized Action");
        e.Cancel = true;
        return;
    }
    
    // Require admin role for certain commands
    if (prompt.Contains("filter") && currentUser.Role != "Admin")
    {
        MessageBox.Show("Only admins can apply filters.", "Permission Denied");
        e.Cancel = true;
    }
}
```

#### Use Case 3: Custom Business Logic

Add application-specific logic before processing:

```csharp
private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    string prompt = e.Prompt;
    
    // Transform or enhance the prompt
    if (prompt.Contains("expensive"))
    {
        // Replace with specific business rule
        e.Cancel = true;
        SmartGrid.ExecutePrompt("filter Freight greaterThan 1000");
        return;
    }
    
    // Check data state
    if (IsDataLoading)
    {
        MessageBox.Show("Please wait for data to finish loading.", "Data Loading");
        e.Cancel = true;
    }
}
```

#### Use Case 4: Preprocessing Prompts

Modify prompts before execution:

```csharp
private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    string originalPrompt = e.Prompt;
    
    // Add context or normalize prompt
    if (!originalPrompt.Contains("filter") && 
        !originalPrompt.Contains("sort") && 
        !originalPrompt.Contains("group"))
    {
        // User asked a question instead of command - guide them
        MessageBox.Show(
            "Try commands like:\n" +
            "- filter Country equals Germany\n" +
            "- sort by Freight descending\n" +
            "- group by PaymentStatus",
            "AI Command Help"
        );
        e.Cancel = true;
    }
}
```

#### Use Case 5: Rate Limiting

Prevent too many rapid requests:

```csharp
private DateTime lastRequestTime = DateTime.MinValue;
private const int MinRequestIntervalMs = 1000;

private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    var timeSinceLastRequest = (DateTime.Now - lastRequestTime).TotalMilliseconds;
    
    if (timeSinceLastRequest < MinRequestIntervalMs)
    {
        MessageBox.Show("Please wait before sending another request.", "Rate Limit");
        e.Cancel = true;
        return;
    }
    
    lastRequestTime = DateTime.Now;
}
```

#### Use Case 6: Confirmation for Destructive Actions

Require confirmation for certain operations:

```csharp
private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    string prompt = e.Prompt.ToLower();
    
    if (prompt.Contains("clear") || prompt.Contains("remove"))
    {
        var result = MessageBox.Show(
            $"Are you sure you want to execute: {e.Prompt}?",
            "Confirm Action",
            MessageBoxButton.YesNo,
            MessageBoxImage.Question
        );
        
        if (result == MessageBoxResult.No)
        {
            e.Cancel = true;
        }
    }
}
```

## Complete Customization Example

Here's a complete example demonstrating all customization features:

**MainWindow.xaml:**
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
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <!-- Quick Action Buttons -->
        <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="10">
            <Button Content="Show Unpaid" Click="ShowUnpaid_Click" Margin="5"/>
            <Button Content="High Value Orders" Click="ShowHighValue_Click" Margin="5"/>
            <Button Content="Reset View" Click="ResetView_Click" Margin="5"/>
            <CheckBox Content="Enable AI Actions" 
                      IsChecked="{Binding ElementName=SmartGrid, Path=EnableSmartActions}"
                      Margin="10,0"/>
        </StackPanel>
        
        <!-- Smart DataGrid -->
        <syncfusion:SfSmartDataGrid x:Name="SmartGrid" 
                                    Grid.Row="1"
                                    ItemsSource="{Binding OrderInfoCollection}" 
                                    CurrentUser="{Binding CurrentUser}" 
                                    Suggestions="{Binding AiSuggestions}"
                                    Prompt="Sort the Quantity column"
                                    EnableSmartActions="True"
                                    HighlightBrush="Orange"
                                    AssistViewRequest="OnAssistRequest"
                                    AutoGenerateColumns="False"
                                    ColumnSizer="Star">
            <syncfusion:SfSmartDataGrid.Columns>
                <syncfusion:GridNumericColumn MappingName="OrderID" HeaderText="Order ID"/>
                <syncfusion:GridTextColumn MappingName="CustomerName" HeaderText="Customer"/>
                <syncfusion:GridTextColumn MappingName="ProductName" HeaderText="Product"/>
                <syncfusion:GridNumericColumn MappingName="Quantity"/>
                <syncfusion:GridNumericColumn MappingName="Freight"/>
                <syncfusion:GridTextColumn MappingName="PaymentStatus" HeaderText="Status"/>
            </syncfusion:SfSmartDataGrid.Columns>
        </syncfusion:SfSmartDataGrid>
    </Grid>
</Window>
```

**MainWindow.xaml.cs:**
```csharp
using System;
using System.Windows;
using Syncfusion.UI.Xaml.SmartComponents;

namespace WpfApplication1
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        // Quick action: Show unpaid orders
        private void ShowUnpaid_Click(object sender, RoutedEventArgs e)
        {
            SmartGrid.ExecutePrompt("filter PaymentStatus equals Not Paid");
            SmartGrid.ExecutePrompt("highlight rows where Freight > 500 color Red");
        }

        // Quick action: Show high-value orders
        private void ShowHighValue_Click(object sender, RoutedEventArgs e)
        {
            SmartGrid.ExecutePrompt("filter Freight greaterThan 1000");
            SmartGrid.ExecutePrompt("sort by Freight descending");
        }

        // Quick action: Reset to default view
        private void ResetView_Click(object sender, RoutedEventArgs e)
        {
            SmartGrid.ExecutePrompt("clear filters");
            SmartGrid.ExecutePrompt("clear grouping");
            SmartGrid.ExecutePrompt("clear sorting");
            SmartGrid.ExecutePrompt("clear highlight");
        }

        // Event: Intercept and validate requests
        private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
        {
            string prompt = e.Prompt.ToLower();
            
            // Log the request
            Console.WriteLine($"User prompt: {e.Prompt}");
            
            // Validate: Block forbidden operations
            if (prompt.Contains("forbidden") || prompt.Contains("delete"))
            {
                MessageBox.Show(
                    "This operation is not allowed.",
                    "Unauthorized Action",
                    MessageBoxButton.OK,
                    MessageBoxImage.Warning
                );
                e.Cancel = true;
                return;
            }
            
            // Validate: Check if data is loaded
            if (SmartGrid.ItemsSource == null)
            {
                MessageBox.Show(
                    "No data available. Please load data first.",
                    "No Data",
                    MessageBoxButton.OK,
                    MessageBoxImage.Information
                );
                e.Cancel = true;
            }
        }
    }
}
```

**ViewModel.cs:**
```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class ViewModel : INotifyPropertyChanged
{
    public ObservableCollection<OrderInfo> OrderInfoCollection { get; set; }
    
    private Author currentUser;
    public Author CurrentUser
    {
        get => currentUser;
        set
        {
            currentUser = value;
            RaisePropertyChanged(nameof(CurrentUser));
        }
    }

    public ObservableCollection<string> AiSuggestions { get; } = new ObservableCollection<string>
    {
        "Which orders have a payment status of Not Paid?",
        "What are the top 10 orders with the highest freight cost?",
        "Which customers have placed the most orders?",
        "What are the orders shipped to Brazil?",
        "What is the total quantity of products ordered across all orders?",
    };

    public event PropertyChangedEventHandler PropertyChanged;
    
    private void RaisePropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public ViewModel()
    {
        CurrentUser = new Author { Name = "John" };
        OrderInfoCollection = new ObservableCollection<OrderInfo>();
        GenerateOrders();
    }

    private void GenerateOrders()
    {
        // Generate sample data (see data-model-binding.md)
    }
}
```

## Best Practices

### 1. Always Set CurrentUser

**Required for proper AssistView functionality:**
```csharp
CurrentUser = new Author { Name = "John" };
```

Without this, message alignment in AssistView breaks.

### 2. Provide Helpful Suggestions

**Good suggestions:**
- Aligned with user's workflow
- Natural language questions
- Cover common operations
- 3-7 suggestions (not too many)

**Example:**
```csharp
AiSuggestions.Add("Which orders have a payment status of Not Paid?");
AiSuggestions.Add("What are the top 10 orders with the highest freight cost?");
```

### 3. Use Prompt for Default Views

Set initial state that makes sense for the data:
```xml
<syncfusion:SfSmartDataGrid Prompt="group by PaymentStatus"/>
```

### 4. Enable Smart Actions Explicitly

**Always set to true** to enable AI features:
```xml
<syncfusion:SfSmartDataGrid EnableSmartActions="True"/>
```

### 5. Customize HighlightBrush for Theme Consistency

Match your application theme:
```csharp
SmartGrid.HighlightBrush = new SolidColorBrush(MyApp.Theme.AccentColor);
```

### 6. Use ExecutePrompt for Programmatic Workflows

Don't force users to type commands for common operations:
```csharp
// Provide button for quick access
private void ShowCriticalOrders_Click(object sender, RoutedEventArgs e)
{
    SmartGrid.ExecutePrompt("filter PaymentStatus equals Not Paid AND Freight > 1000");
    SmartGrid.ExecutePrompt("highlight rows color Red");
}
```

### 7. Implement Request Validation

**Always validate in AssistViewRequest:**
- Check user permissions
- Validate data state
- Log for audit trails
- Cancel unauthorized actions

### 8. Handle Edge Cases

Consider scenarios like:
- Empty data source
- AI service disconnected
- Invalid column names in prompts
- Network timeouts

## Advanced Scenarios

### Scenario 1: Role-Based AI Features

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Enable AI based on user role
    var user = GetCurrentUser();
    SmartGrid.CurrentUser = new Author { Name = user.Name };
    SmartGrid.EnableSmartActions = (user.Role == "Admin" || user.Role == "Analyst");
    
    // Update suggestions based on role
    if (user.Role == "Admin")
    {
        viewModel.AiSuggestions.Add("Show all unpaid high-value orders");
    }
}
```

### Scenario 2: Context-Aware Suggestions

Update suggestions based on current data state:

```csharp
private void UpdateSuggestionsBasedOnData()
{
    var unpaidCount = OrderInfoCollection.Count(o => o.PaymentStatus == "Not Paid");
    
    if (unpaidCount > 10)
    {
        viewModel.AiSuggestions.Clear();
        viewModel.AiSuggestions.Add("Show all unpaid orders (Alert: " + unpaidCount + " unpaid)");
        viewModel.AiSuggestions.Add("Group unpaid orders by Country");
    }
}
```

### Scenario 3: Guided Workflows

Guide users through multi-step analysis:

```csharp
private int workflowStep = 0;

private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    // Track workflow progress
    if (e.Prompt.Contains("group by Country"))
        workflowStep = 1;
    
    if (workflowStep == 1)
    {
        // Suggest next step
        viewModel.AiSuggestions.Clear();
        viewModel.AiSuggestions.Add("Now sort by Freight descending");
        viewModel.AiSuggestions.Add("Highlight high-value orders");
    }
}
```

### Scenario 4: Prompt Templates

Create reusable prompt templates:

```csharp
public class PromptTemplates
{
    public static string UnpaidHighValue => "filter PaymentStatus equals Not Paid AND Freight > 1000";
    public static string RecentOrders => "filter OrderDate greaterThan " + DateTime.Today.AddDays(-7).ToString("yyyy-MM-dd");
    public static string TopCustomers => "group by CustomerName AND sort by Quantity descending";
}

// Usage
SmartGrid.ExecutePrompt(PromptTemplates.UnpaidHighValue);
```

## Troubleshooting

### Issue: AssistView Messages All Look the Same

**Cause:** `CurrentUser` not set.

**Solution:**
```csharp
SmartGrid.CurrentUser = new Author { Name = "John" };
```

### Issue: ExecutePrompt Has No Effect

**Possible Causes:**
- `EnableSmartActions` is false
- AI service not configured
- Invalid prompt syntax

**Solution:**
- Set `EnableSmartActions="True"`
- Verify AI service setup in App.xaml.cs
- Test prompt in AssistView first to verify syntax

### Issue: AssistViewRequest Event Not Firing

**Cause:** Event not subscribed.

**Solution:**
```xml
<syncfusion:SfSmartDataGrid AssistViewRequest="OnAssistRequest"/>
```

Or in code:
```csharp
SmartGrid.AssistViewRequest += OnAssistRequest;
```

### Issue: Cancel Property Not Preventing Execution

**Cause:** Setting Cancel after the fact.

**Solution:** Set `e.Cancel = true` BEFORE the event handler returns:

```csharp
private void OnAssistRequest(object sender, AssistViewRequestEventArgs e)
{
    if (ShouldCancel(e.Prompt))
    {
        e.Cancel = true;  // Set immediately
        return;           // Exit handler
    }
}
```

### Issue: Suggestions Not Appearing

**Cause:** Suggestions property not bound.

**Solution:**
```xml
<syncfusion:SfSmartDataGrid Suggestions="{Binding AiSuggestions}"/>
```

And ensure ViewModel has:
```csharp
public ObservableCollection<string> AiSuggestions { get; } = new ObservableCollection<string>();
```

## Summary

The Smart DataGrid customization features enable you to:

1. **CurrentUser** – Differentiate user messages from AI responses in AssistView
2. **Suggestions** – Guide users with predefined prompts
3. **Prompt** – Auto-execute initial commands on first load
4. **EnableSmartActions** – Control when AI features are active
5. **HighlightBrush** – Set default highlight color for visual consistency
6. **ExecutePrompt** – Trigger AI commands programmatically from code
7. **AssistViewRequest** – Intercept, validate, log, or cancel user requests

Use these features together to create intuitive, powerful, and controlled AI-assisted data grid experiences.

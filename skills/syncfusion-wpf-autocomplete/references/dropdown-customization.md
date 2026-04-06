# Dropdown Customization

Complete guide for customizing the suggestion popup appearance and behavior in WPF Autocomplete (SfTextBoxExt).

## Table of Contents
- [Customize Background](#customize-background)
- [Dropdown Placement](#dropdown-placement)
- [Maximum Height](#maximum-height)
- [Show Suggestions on Focus](#show-suggestions-on-focus)
- [Popup Delay](#popup-delay)
- [Selection Background Color](#selection-background-color)
- [Auto-Highlight Matched Item](#auto-highlight-matched-item)
- [Suggestion Popup Events](#suggestion-popup-events)

## Customize Background

Change the background color of the suggestion dropdown using `DropDownBackground`.

### Setting Background Color

```xaml
<editors:SfTextBoxExt 
    Width="300"
    Height="40"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    DropDownBackground="AliceBlue"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
// Solid color
textBoxExt.DropDownBackground = new SolidColorBrush(Colors.AliceBlue);

// Custom color
textBoxExt.DropDownBackground = new SolidColorBrush(Color.FromRgb(240, 248, 255));

// Gradient background
textBoxExt.DropDownBackground = new LinearGradientBrush
{
    StartPoint = new Point(0, 0),
    EndPoint = new Point(1, 1),
    GradientStops = new GradientStopCollection
    {
        new GradientStop(Colors.White, 0),
        new GradientStop(Colors.LightBlue, 1)
    }
};
```

## Dropdown Placement

Control where the suggestion popup appears relative to the textbox.

### SuggestionBoxPlacement Options

| Value | Description |
|-------|-------------|
| **Bottom** (default) | Popup opens below the textbox |
| **Top** | Popup opens above the textbox |
| **None** | Popup displays in default location |

### Bottom Placement

```xaml
<editors:SfTextBoxExt 
    SuggestionBoxPlacement="Bottom"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}"/>
```

```csharp
textBoxExt.SuggestionBoxPlacement = SuggestionBoxPlacement.Bottom;
```

### Top Placement

Useful when textbox is near the bottom of the window:

```xaml
<editors:SfTextBoxExt 
    SuggestionBoxPlacement="Top"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}"/>
```

```csharp
textBoxExt.SuggestionBoxPlacement = SuggestionBoxPlacement.Top;
```

### Dynamic Placement

```csharp
// Adjust based on available space
private void AdjustPopupPlacement()
{
    var screenHeight = SystemParameters.PrimaryScreenHeight;
    var controlPosition = textBoxExt.PointToScreen(new Point(0, 0));
    
    // If textbox is in bottom half, open upward
    if (controlPosition.Y > screenHeight / 2)
    {
        textBoxExt.SuggestionBoxPlacement = SuggestionBoxPlacement.Top;
    }
    else
    {
        textBoxExt.SuggestionBoxPlacement = SuggestionBoxPlacement.Bottom;
    }
}
```

## Maximum Height

Limit the popup height to prevent it from occupying too much screen space.

### MaxDropDownHeight Property

```xaml
<editors:SfTextBoxExt 
    MaxDropDownHeight="300"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}"/>
```

```csharp
// Set maximum height in pixels
textBoxExt.MaxDropDownHeight = 300;

// Calculate based on item count
int itemHeight = 30;
int maxVisibleItems = 10;
textBoxExt.MaxDropDownHeight = itemHeight * maxVisibleItems;

// Responsive based on screen
textBoxExt.MaxDropDownHeight = SystemParameters.PrimaryScreenHeight * 0.4;
```

**Default:** Auto (no limit)

**Best Practices:**
- **Small lists** (<10 items): 200-300px
- **Medium lists** (10-50 items): 300-400px
- **Large lists** (50+ items): 400-500px
- **Very large lists**: 500px max + scrolling

## Show Suggestions on Focus

Display the complete suggestion list when the control receives focus.

### ShowSuggestionsOnFocus Property

```xaml
<editors:SfTextBoxExt 
    ShowSuggestionsOnFocus="True"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}"/>
```

```csharp
textBoxExt.ShowSuggestionsOnFocus = true;
```

**Default:** `false`

**When Enabled:**
- Focus textbox → Complete data source displays
- User can browse all options without typing
- Behaves like a combo box dropdown

**Use Cases:**
- **Small datasets**: User can see all options
- **Browsing mode**: User exploring available choices
- **Keyboard navigation**: Immediate access to arrow keys
- **Touch devices**: Easier selection without typing

### Example with Focus Behavior

```csharp
private void SetupAutoCompleteWithFocus()
{
    textBoxExt.ShowSuggestionsOnFocus = true;
    textBoxExt.MinimumPrefixCharacters = 0; // Allow suggestions immediately
    
    // Optionally auto-select first item
    textBoxExt.GotFocus += (s, e) =>
    {
        if (string.IsNullOrEmpty(textBoxExt.Text))
        {
            textBoxExt.AutoHighlightMatchedItem = true;
        }
    };
}
```

## Popup Delay

Add a delay before the suggestion popup opens, useful for debouncing and reducing API calls.

### PopupDelay Property

```xaml
<editors:SfTextBoxExt 
    PopupDelay="00:00:00.5"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}"/>
```

```csharp
// Half second delay
textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(500);

// 2 second delay
textBoxExt.PopupDelay = new TimeSpan(0, 0, 2);

// 300ms delay (common for typeahead)
textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(300);

// No delay (immediate)
textBoxExt.PopupDelay = TimeSpan.Zero;
```

**Default:** `TimeSpan.Zero` (no delay)

**Recommended Delays:**
- **Local data**: 0-100ms (instant feel)
- **In-memory filtering**: 100-200ms (smooth typing)
- **Remote API**: 300-500ms (reduce API calls)
- **Expensive operations**: 500-1000ms (debounce effect)

### Use Cases

**1. Reduce API Calls:**

```csharp
// Delay to avoid calling API on every keystroke
textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(300);
textBoxExt.MinimumPrefixCharacters = 2;
```

**2. Improve Performance:**

```csharp
// Large datasets - delay filtering
textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(200);
```

**3. Better User Experience:**

```csharp
// Wait for user to finish typing
textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(400);
```

## Selection Background Color

Customize the background color of the selected/hovered item in the dropdown.

### SelectionBackgroundColor Property

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SelectionBackgroundColor="LightBlue"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
// Solid color
textBoxExt.SelectionBackgroundColor = new SolidColorBrush(Colors.LightBlue);

// Custom color
textBoxExt.SelectionBackgroundColor = new SolidColorBrush(Color.FromRgb(135, 206, 250));

// Theme-aware color
textBoxExt.SelectionBackgroundColor = SystemColors.HighlightBrush;
```

### Coordinating with Theme

```csharp
// Match your application theme
public void ApplyThemeColors()
{
    var accentColor = (Color)Application.Current.Resources["AccentColor"];
    textBoxExt.SelectionBackgroundColor = new SolidColorBrush(accentColor);
    textBoxExt.DropDownBackground = Brushes.White;
}
```

## Auto-Highlight Matched Item

Automatically highlight the first matching item when the dropdown opens.

### AutoHighlightMatchedItem Property

```xaml
<editors:SfTextBoxExt 
    AutoHighlightMatchedItem="True"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
textBoxExt.AutoHighlightMatchedItem = true;
```

**Default:** `false`

**When Enabled:**
- First matched item is highlighted automatically
- User can press Enter immediately to select
- Improves keyboard navigation
- Speeds up selection process

**Benefits:**
- **Keyboard users**: Quick selection without arrow keys
- **Power users**: Faster workflow
- **Accessibility**: Better screen reader support
- **Touch devices**: Clear visual focus

### Combined with Other Features

```csharp
// Optimal keyboard experience
textBoxExt.AutoHighlightMatchedItem = true;
textBoxExt.ShowSuggestionsOnFocus = true;
textBoxExt.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
```

## Suggestion Popup Events

Track when the suggestion popup opens and closes for custom logic.

### SuggestionPopupOpened Event

Raised when the dropdown opens.

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SuggestionPopupOpened="OnPopupOpened"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
private void OnPopupOpened(object sender, EventArgs e)
{
    var textBox = sender as SfTextBoxExt;
    
    // Log event
    Console.WriteLine("Suggestion popup opened");
    
    // Update UI
    StatusLabel.Text = "Browse suggestions...";
    
    // Analytics
    TrackPopupOpen(textBox.Text);
}
```

### SuggestionPopupClosed Event

Raised when the dropdown closes.

```xaml
<editors:SfTextBoxExt 
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SuggestionPopupClosed="OnPopupClosed"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
private void OnPopupClosed(object sender, EventArgs e)
{
    var textBox = sender as SfTextBoxExt;
    
    // Log event
    Console.WriteLine("Suggestion popup closed");
    
    // Update UI
    StatusLabel.Text = "";
    
    // Cleanup
    PerformCleanup();
}
```

### Combined Event Handling

```csharp
public MainWindow()
{
    InitializeComponent();
    
    textBoxExt.SuggestionPopupOpened += OnPopupOpened;
    textBoxExt.SuggestionPopupClosed += OnPopupClosed;
}

private void OnPopupOpened(object sender, EventArgs e)
{
    // Show loading indicator for remote data
    LoadingSpinner.Visibility = Visibility.Visible;
    
    // Adjust window if popup would be cut off
    EnsurePopupVisible();
}

private void OnPopupClosed(object sender, EventArgs e)
{
    // Hide loading indicator
    LoadingSpinner.Visibility = Visibility.Collapsed;
    
    // Save state
    SaveSearchHistory();
}
```

### Use Cases

**1. Analytics Tracking:**

```csharp
private void OnPopupOpened(object sender, EventArgs e)
{
    var textBox = sender as SfTextBoxExt;
    Analytics.Track("AutoComplete_PopupOpened", new
    {
        SearchText = textBox.Text,
        Timestamp = DateTime.Now
    });
}
```

**2. Dynamic Data Loading:**

```csharp
private void OnPopupOpened(object sender, EventArgs e)
{
    var textBox = sender as SfTextBoxExt;
    
    // Load additional data when popup opens
    if (ShouldLoadMoreData())
    {
        LoadAdditionalSuggestions();
    }
}
```

**3. UI State Management:**

```csharp
private bool isPopupOpen = false;

private void OnPopupOpened(object sender, EventArgs e)
{
    isPopupOpen = true;
    ExpandButton.IsEnabled = false;
}

private void OnPopupClosed(object sender, EventArgs e)
{
    isPopupOpen = false;
    ExpandButton.IsEnabled = true;
}
```

**4. Accessibility Announcements:**

```csharp
private void OnPopupOpened(object sender, EventArgs e)
{
    var textBox = sender as SfTextBoxExt;
    
    // Announce for screen readers
    AutomationProperties.SetLiveSetting(textBox, AutomationLiveSetting.Assertive);
    AutomationProperties.SetName(textBox, $"{SuggestionCount} suggestions available");
}
```

## Complete Configuration Example

```xaml
<editors:SfTextBoxExt 
    Width="400"
    Height="40"
    SearchItemPath="Name"
    AutoCompleteMode="Suggest"
    SuggestionMode="Contains"
    DropDownBackground="WhiteSmoke"
    MaxDropDownHeight="350"
    SuggestionBoxPlacement="Bottom"
    ShowSuggestionsOnFocus="True"
    PopupDelay="00:00:00.3"
    SelectionBackgroundColor="LightSkyBlue"
    AutoHighlightMatchedItem="True"
    SuggestionPopupOpened="OnPopupOpened"
    SuggestionPopupClosed="OnPopupClosed"
    AutoCompleteSource="{Binding Employees}" />
```

```csharp
private void SetupAutoComplete()
{
    // Background and styling
    textBoxExt.DropDownBackground = new SolidColorBrush(Colors.WhiteSmoke);
    textBoxExt.SelectionBackgroundColor = new SolidColorBrush(Colors.LightSkyBlue);
    
    // Popup behavior
    textBoxExt.MaxDropDownHeight = 350;
    textBoxExt.SuggestionBoxPlacement = SuggestionBoxPlacement.Bottom;
    textBoxExt.ShowSuggestionsOnFocus = true;
    textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(300);
    textBoxExt.AutoHighlightMatchedItem = true;
    
    // Event handlers
    textBoxExt.SuggestionPopupOpened += (s, e) =>
    {
        StatusText.Text = "Suggestions available";
    };
    
    textBoxExt.SuggestionPopupClosed += (s, e) =>
    {
        StatusText.Text = "";
    };
}
```

## Best Practices

### Performance

```csharp
// Large datasets: Add delay and limit height
textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(300);
textBoxExt.MaxDropDownHeight = 400;
textBoxExt.MaximumSuggestionsCount = 15;
```

### User Experience

```csharp
// Optimize for user workflow
textBoxExt.AutoHighlightMatchedItem = true;  // Quick selection
textBoxExt.ShowSuggestionsOnFocus = true;    // Browse on focus
textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(200); // Smooth typing
```

### Accessibility

```csharp
// Better accessibility
textBoxExt.AutoHighlightMatchedItem = true; // Clear focus
textBoxExt.SelectionBackgroundColor = SystemColors.HighlightBrush; // System colors
// Set AutomationProperties for screen readers
```

### Responsive Design

```csharp
// Adapt to screen size
private void AdaptToScreenSize()
{
    var screenHeight = SystemParameters.PrimaryScreenHeight;
    
    if (screenHeight <= 768) // Laptop/tablet
    {
        textBoxExt.MaxDropDownHeight = 250;
    }
    else if (screenHeight <= 1080) // Standard monitor
    {
        textBoxExt.MaxDropDownHeight = 400;
    }
    else // Large monitor
    {
        textBoxExt.MaxDropDownHeight = 600;
    }
}
```

## Troubleshooting

### Popup Not Appearing

```csharp
// Check these settings
textBoxExt.AutoCompleteMode = AutoCompleteMode.Suggest; // Not None
textBoxExt.AutoCompleteSource = data; // Not null
textBoxExt.SearchItemPath = "Name"; // Valid property
```

### Popup Cut Off at Screen Edge

```csharp
// Use Top placement or adjust window
textBoxExt.SuggestionBoxPlacement = SuggestionBoxPlacement.Top;
```

### Popup Opening Too Frequently

```csharp
// Add delay
textBoxExt.PopupDelay = TimeSpan.FromMilliseconds(300);
textBoxExt.MinimumPrefixCharacters = 2;
```

## Next Steps

- **TextBox Customization**: Watermarks and buttons → [textbox-customization.md](textbox-customization.md)
- **Advanced Features**: Highlighting and diacritic search → [advanced-features.md](advanced-features.md)
- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)

## Resources

- **GitHub Sample**: https://github.com/SyncfusionExamples/wpf-textboxext-examples/tree/master/Samples/Dropdown-customization
- **API Documentation**: https://help.syncfusion.com/cr/wpf/Syncfusion.Windows.Controls.Input.SfTextBoxExt.html

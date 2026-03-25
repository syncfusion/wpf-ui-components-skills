# Page Navigation in WPF DataPager

## Overview

SfDataPager provides comprehensive page navigation capabilities through methods and events. You can navigate programmatically using built-in methods, or allow users to navigate through the UI. The navigation system also provides events for validation and custom logic before and after page changes.

## Navigation Methods

SfDataPager exposes five navigation methods for programmatic page control:

### MoveToFirstPage()

Moves the current page index to the first page (page 0) and displays the first page data.

**Syntax:**
```csharp
sfDataPager.MoveToFirstPage();
```

**Example:**
```csharp
private void FirstPageButton_Click(object sender, RoutedEventArgs e)
{
    sfDataPager.MoveToFirstPage();
}
```

**Use when:**
- Implementing custom "Reset" functionality
- Returning to start after search/filter
- Keyboard shortcuts (e.g., Ctrl+Home)

### MoveToLastPage()

Moves the current page index to the last page and displays the last page data.

**Syntax:**
```csharp
sfDataPager.MoveToLastPage();
```

**Example:**
```csharp
private void LastPageButton_Click(object sender, RoutedEventArgs e)
{
    sfDataPager.MoveToLastPage();
}
```

**Use when:**
- Jump to most recent items
- View end of dataset
- Keyboard shortcuts (e.g., Ctrl+End)

### MoveToNextPage()

Moves the current page index to the next page and displays the next page data. If already on the last page, this method has no effect.

**Syntax:**
```csharp
sfDataPager.MoveToNextPage();
```

**Example:**
```csharp
private void NextButton_Click(object sender, RoutedEventArgs e)
{
    if (sfDataPager.PageIndex < sfDataPager.PageCount - 1)
    {
        sfDataPager.MoveToNextPage();
    }
}
```

**Use when:**
- Sequential navigation (e.g., wizard)
- Keyboard shortcuts (arrow keys, Page Down)
- Automated slideshow/demo
- Touch gestures (swipe)

### MoveToPreviousPage()

Moves the current page index to the previous page and displays the previous page data. If already on the first page, this method has no effect.

**Syntax:**
```csharp
sfDataPager.MoveToPreviousPage();
```

**Example:**
```csharp
private void PreviousButton_Click(object sender, RoutedEventArgs e)
{
    if (sfDataPager.PageIndex > 0)
    {
        sfDataPager.MoveToPreviousPage();
    }
}
```

**Use when:**
- Backward navigation
- Keyboard shortcuts (arrow keys, Page Up)
- Undo/back functionality
- Touch gestures (swipe)

### MoveToPage(int pageIndex)

Moves the current page index to the specified page index (0-based).

**Syntax:**
```csharp
sfDataPager.MoveToPage(int pageIndex);
```

**Parameters:**
- `pageIndex`: Zero-based page index to navigate to

**Example:**
```csharp
private void JumpToPage_Click(object sender, RoutedEventArgs e)
{
    int targetPage = int.Parse(pageNumberTextBox.Text) - 1; // Convert 1-based to 0-based
    
    if (targetPage >= 0 && targetPage < sfDataPager.PageCount)
    {
        sfDataPager.MoveToPage(targetPage);
    }
    else
    {
        MessageBox.Show($"Please enter a page number between 1 and {sfDataPager.PageCount}");
    }
}
```

**Use when:**
- Direct page input from user
- Jump to specific page
- Bookmarking/deep linking
- Search result navigation

## Navigation Method Examples

### Keyboard Navigation

```xml
<Window KeyDown="Window_KeyDown">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        
        <sfgrid:SfDataGrid ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
        
        <datapager:SfDataPager x:Name="sfDataPager"
                               Grid.Row="1"
                               PageSize="10"
                               Source="{Binding DataCollection}"/>
    </Grid>
</Window>
```

```csharp
private void Window_KeyDown(object sender, KeyEventArgs e)
{
    switch (e.Key)
    {
        case Key.Home:
            if (Keyboard.Modifiers == ModifierKeys.Control)
                sfDataPager.MoveToFirstPage();
            break;
            
        case Key.End:
            if (Keyboard.Modifiers == ModifierKeys.Control)
                sfDataPager.MoveToLastPage();
            break;
            
        case Key.PageUp:
            sfDataPager.MoveToPreviousPage();
            e.Handled = true;
            break;
            
        case Key.PageDown:
            sfDataPager.MoveToNextPage();
            e.Handled = true;
            break;
            
        case Key.Left:
            if (Keyboard.Modifiers == ModifierKeys.Control)
                sfDataPager.MoveToPreviousPage();
            break;
            
        case Key.Right:
            if (Keyboard.Modifiers == ModifierKeys.Control)
                sfDataPager.MoveToNextPage();
            break;
    }
}
```

### Custom Navigation UI

```xml
<StackPanel Orientation="Horizontal" Margin="5">
    <Button Content="First" Click="FirstPage_Click" Width="60" Margin="2"/>
    <Button Content="Previous" Click="PreviousPage_Click" Width="60" Margin="2"/>
    
    <TextBlock Text="Page" VerticalAlignment="Center" Margin="10,0,5,0"/>
    <TextBox x:Name="pageNumberTextBox" 
             Width="40" 
             Text="{Binding ElementName=sfDataPager, Path=PageIndex, Converter={StaticResource PageIndexConverter}}"
             KeyDown="PageNumber_KeyDown"/>
    <TextBlock VerticalAlignment="Center" Margin="5,0">
        <Run Text="of"/>
        <Run Text="{Binding ElementName=sfDataPager, Path=PageCount}"/>
    </TextBlock>
    
    <Button Content="Next" Click="NextPage_Click" Width="60" Margin="2"/>
    <Button Content="Last" Click="LastPage_Click" Width="60" Margin="2"/>
</StackPanel>

<datapager:SfDataPager x:Name="sfDataPager" Source="{Binding DataCollection}"/>
```

```csharp
private void FirstPage_Click(object sender, RoutedEventArgs e)
{
    sfDataPager.MoveToFirstPage();
}

private void PreviousPage_Click(object sender, RoutedEventArgs e)
{
    sfDataPager.MoveToPreviousPage();
}

private void NextPage_Click(object sender, RoutedEventArgs e)
{
    sfDataPager.MoveToNextPage();
}

private void LastPage_Click(object sender, RoutedEventArgs e)
{
    sfDataPager.MoveToLastPage();
}

private void PageNumber_KeyDown(object sender, KeyEventArgs e)
{
    if (e.Key == Key.Enter)
    {
        if (int.TryParse(pageNumberTextBox.Text, out int pageNumber))
        {
            int pageIndex = pageNumber - 1; // Convert to 0-based
            if (pageIndex >= 0 && pageIndex < sfDataPager.PageCount)
            {
                sfDataPager.MoveToPage(pageIndex);
            }
        }
    }
}
```

### Auto-Navigation (Slideshow)

```csharp
private DispatcherTimer navigationTimer;

private void StartAutoNavigation()
{
    navigationTimer = new DispatcherTimer();
    navigationTimer.Interval = TimeSpan.FromSeconds(3);
    navigationTimer.Tick += NavigationTimer_Tick;
    navigationTimer.Start();
}

private void NavigationTimer_Tick(object sender, EventArgs e)
{
    if (sfDataPager.PageIndex < sfDataPager.PageCount - 1)
    {
        sfDataPager.MoveToNextPage();
    }
    else
    {
        sfDataPager.MoveToFirstPage(); // Loop back to start
    }
}

private void StopAutoNavigation()
{
    if (navigationTimer != null)
    {
        navigationTimer.Stop();
        navigationTimer = null;
    }
}
```

## PageIndexChanging Event

The `PageIndexChanging` event is triggered **before** the page index changes. This event is **cancelable**, allowing you to validate conditions or prompt users before allowing navigation.

### Event Arguments

| Property | Type | Description |
|----------|------|-------------|
| `OldPageIndex` | int | Current page index |
| `NewPageIndex` | int | Target page index |
| `Cancel` | bool | Set to true to cancel navigation |

### Basic Usage

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       PageIndexChanging="sfDataPager_PageIndexChanging"
                       PageSize="16"
                       Source="{Binding OrdersDetails}"/>
```

```csharp
private void sfDataPager_PageIndexChanging(object sender, PageIndexChangingEventArgs e)
{
    // Log navigation attempt
    Debug.WriteLine($"Navigating from page {e.OldPageIndex} to {e.NewPageIndex}");
    
    // Optionally cancel navigation
    // e.Cancel = true;
}
```

### Validation Before Page Change

Prompt user before leaving page with unsaved changes:

```csharp
private bool hasUnsavedChanges = false;

private void sfDataPager_PageIndexChanging(object sender, PageIndexChangingEventArgs e)
{
    if (hasUnsavedChanges)
    {
        MessageBoxResult result = MessageBox.Show(
            "You have unsaved changes. Do you want to change the page?",
            "Confirm",
            MessageBoxButton.YesNo,
            MessageBoxImage.Warning);
        
        if (result == MessageBoxResult.No)
        {
            e.Cancel = true; // Cancel page navigation
        }
        else
        {
            // User confirmed, clear unsaved changes flag
            hasUnsavedChanges = false;
        }
    }
}
```

### Conditional Navigation

Allow navigation only if validation passes:

```csharp
private void sfDataPager_PageIndexChanging(object sender, PageIndexChangingEventArgs e)
{
    // Check if current page has valid data
    if (!ValidateCurrentPage())
    {
        MessageBox.Show(
            "Please correct the errors on this page before navigating.",
            "Validation Error",
            MessageBoxButton.OK,
            MessageBoxImage.Error);
        
        e.Cancel = true;
        return;
    }
    
    // Check if moving forward in a wizard-like scenario
    if (e.NewPageIndex > e.OldPageIndex)
    {
        if (!IsStepComplete(e.OldPageIndex))
        {
            MessageBox.Show("Please complete the current step.");
            e.Cancel = true;
        }
    }
}

private bool ValidateCurrentPage()
{
    // Implement validation logic
    return true;
}

private bool IsStepComplete(int stepIndex)
{
    // Check if step is complete
    return true;
}
```

### Loading Indicator

Show loading indicator during navigation:

```csharp
private void sfDataPager_PageIndexChanging(object sender, PageIndexChangingEventArgs e)
{
    // Show loading indicator
    loadingOverlay.Visibility = Visibility.Visible;
    
    // Optionally add delay for data loading
    // Use async/await pattern for real scenarios
}
```

## PageIndexChanged Event

The `PageIndexChanged` event is triggered **after** the page index has changed. This event is **not cancelable** and is used for logging, analytics, or updating UI state.

### Event Arguments

| Property | Type | Description |
|----------|------|-------------|
| `OldPageIndex` | int | Previous page index |
| `NewPageIndex` | int | Current page index |

### Basic Usage

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       PageIndexChanged="sfDataPager_PageIndexChanged"
                       PageSize="16"
                       Source="{Binding OrdersDetails}"/>
```

```csharp
private void sfDataPager_PageIndexChanged(object sender, PageIndexChangedEventArgs e)
{
    // Update status bar
    statusTextBlock.Text = $"Page {e.NewPageIndex + 1} of {sfDataPager.PageCount}";
    
    // Log navigation
    Debug.WriteLine($"Page changed from {e.OldPageIndex} to {e.NewPageIndex}");
}
```

### Common Use Cases

**1. Update Status Display:**

```csharp
private void sfDataPager_PageIndexChanged(object sender, PageIndexChangedEventArgs e)
{
    pageInfoTextBlock.Text = $"Showing page {e.NewPageIndex + 1} of {sfDataPager.PageCount}";
    itemRangeTextBlock.Text = $"Items {e.NewPageIndex * sfDataPager.PageSize + 1} - " +
                              $"{Math.Min((e.NewPageIndex + 1) * sfDataPager.PageSize, totalItemCount)}";
}
```

**2. Analytics Tracking:**

```csharp
private void sfDataPager_PageIndexChanged(object sender, PageIndexChangedEventArgs e)
{
    // Track page view in analytics
    Analytics.TrackEvent("PageView", new Dictionary<string, string>
    {
        { "PageNumber", (e.NewPageIndex + 1).ToString() },
        { "TotalPages", sfDataPager.PageCount.ToString() },
        { "Direction", e.NewPageIndex > e.OldPageIndex ? "Forward" : "Backward" }
    });
}
```

**3. Reset Selection:**

```csharp
private void sfDataPager_PageIndexChanged(object sender, PageIndexChangedEventArgs e)
{
    // Clear selection when page changes
    dataGrid.SelectedItem = null;
    
    // Reset scroll position
    scrollViewer.ScrollToTop();
}
```

**4. Hide Loading Indicator:**

```csharp
private void sfDataPager_PageIndexChanged(object sender, PageIndexChangedEventArgs e)
{
    // Hide loading indicator
    loadingOverlay.Visibility = Visibility.Collapsed;
    
    // Reset unsaved changes flag
    hasUnsavedChanges = false;
}
```

## Best Practices

### Navigation Methods
- Check bounds before calling navigation methods
- Use try-catch for robust error handling
- Provide visual feedback during navigation
- Disable navigation buttons when at boundaries

### PageIndexChanging Event
- Keep validation logic fast (< 100ms)
- Show clear messages when canceling navigation
- Save data automatically if possible (instead of blocking)
- Consider async validation for complex scenarios

### PageIndexChanged Event
- Update UI elements that depend on page state
- Reset transient state (selections, scroll position)
- Log navigation for analytics
- Avoid heavy processing in this event

### User Experience
- Provide multiple navigation methods (buttons, keyboard, direct input)
- Show current page and total page count
- Enable keyboard shortcuts (Home, End, Page Up/Down)
- Give feedback for invalid navigation attempts
- Consider touch gestures on touch devices

### Performance
- Avoid unnecessary processing in events
- Use async/await for data loading
- Cache frequently accessed pages
- Implement virtual scrolling for very large datasets

## Troubleshooting

### Navigation not working
- Check that PageCount > 1
- Verify PageSize is set correctly
- Ensure Source has data
- Check for event handlers canceling navigation

### PageIndexChanging event not firing
- Verify event is wired up in XAML or code
- Check that navigation is triggered correctly
- Ensure no exceptions in event handler

### Cancel not preventing navigation
- Verify e.Cancel is set to true
- Check that you're using PageIndexChanging (not PageIndexChanged)
- Ensure no code is overriding the cancel

### Events firing multiple times
- Check for duplicate event registrations
- Remove event handlers in Unloaded event
- Use -= to unregister before += registering

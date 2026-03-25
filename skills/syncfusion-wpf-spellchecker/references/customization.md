# Customization and Theming

This guide covers customization options for SfSpellChecker, including event handling and theme support.

## SpellCheckCompleted Event

The `SpellCheckCompleted` event fires when spell checking finishes. Use it to customize the completion notification or perform post-check actions.

### Default Behavior

By default, when spell checking completes, a message box appears:

```
"Spell check is completed"
[OK]
```

### Event Overview

```csharp
public event EventHandler<SpellCheckCompletedEventArgs> SpellCheckCompleted;
```

**SpellCheckCompletedEventArgs Properties:**
- `ShowMessageBox` (bool) - Controls whether default message box appears

### Basic Usage

```csharp
spellChecker.SpellCheckCompleted += SpellChecker_SpellCheckCompleted;

private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    // Your custom logic here
    MessageBox.Show("Spell checking finished!");
}
```

### Suppressing Default Message Box

To hide the default "Spell check is completed" message:

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    SpellCheckCompleted="SpellChecker_SpellCheckCompleted"/>
```

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    // Suppress default message box
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Show custom notification
    MessageBox.Show("Spell check completed!", "Done", 
                    MessageBoxButton.OK, MessageBoxImage.Information);
}
```

### Silent Completion

To complete spell checking without any notification:

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    // Suppress message box - silent completion
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
}
```

## Common Customization Patterns

### Pattern 1: Custom Completion Message

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Show custom styled notification
    var result = MessageBox.Show(
        "Spell checking completed successfully!\n\nWould you like to save the document?",
        "Spell Check Complete",
        MessageBoxButton.YesNo,
        MessageBoxImage.Question
    );
    
    if (result == MessageBoxResult.Yes)
    {
        SaveDocument();
    }
}
```

### Pattern 2: Status Bar Notification

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Update status bar instead of message box
    statusBarText.Text = "Spell check completed at " + DateTime.Now.ToShortTimeString();
    
    // Auto-hide after 3 seconds
    var timer = new DispatcherTimer { Interval = TimeSpan.FromSeconds(3) };
    timer.Tick += (s, args) =>
    {
        statusBarText.Text = "Ready";
        timer.Stop();
    };
    timer.Start();
}
```

### Pattern 3: Toast Notification

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Show toast-style notification
    ShowToastNotification("Spell check completed!");
}

private void ShowToastNotification(string message)
{
    var toast = new Border
    {
        Background = Brushes.Green,
        CornerRadius = new CornerRadius(5),
        Padding = new Thickness(10),
        Child = new TextBlock
        {
            Text = message,
            Foreground = Brushes.White
        }
    };
    
    // Add to UI and animate
    // ... implementation
}
```

### Pattern 4: Error Count Report

```csharp
private int errorCount = 0;

private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Show summary
    string message = errorCount == 0
        ? "No spelling errors found!"
        : $"Fixed {errorCount} spelling error(s).";
    
    MessageBox.Show(message, "Spell Check Summary");
    errorCount = 0; // Reset for next check
}
```

### Pattern 5: Auto-Save on Completion

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Auto-save document
    SaveDocument();
    
    // Update UI
    lastSavedLabel.Content = $"Saved at {DateTime.Now:HH:mm:ss}";
}
```

### Pattern 6: Log Completion

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Log to file
    File.AppendAllText("spellcheck.log",
        $"{DateTime.Now}: Spell check completed for {documentName}\n");
}
```

## Theme Support

SfSpellChecker supports various built-in themes through Syncfusion's theming system.

### Available Themes

- **FluentLight** / **FluentDark**
- **MaterialLight** / **MaterialDark**
- **Office2019Colorful** / **Office2019Black** / **Office2019White**
- **Office2016Colorful** / **Office2016Black** / **Office2016White**
- **VisualStudio2013** / **VisualStudio2015**
- **Metro**
- **Blend**
- **Lime**
- **Saffron**
- And more...

### Applying Themes with SfSkinManager

#### Method 1: Apply to Entire Window

```csharp
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    
    // Apply theme to entire window
    SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
}
```

#### Method 2: Apply to Specific Control

```csharp
// Apply theme only to spell checker dialog
SfSkinManager.SetTheme(spellChecker, new Theme("FluentDark"));
```

#### Method 3: XAML Declaration

```xml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
    
    <Grid>
        <TextBox>
            <syncfusion:SfSpellChecker.SpellChecker>
                <syncfusion:SfSpellChecker x:Name="spellChecker"/>
            </syncfusion:SfSpellChecker.SpellChecker>
        </TextBox>
    </Grid>
</Window>
```

### Theme Examples

#### Material Light Theme

```csharp
SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

#### Fluent Dark Theme

```csharp
SfSkinManager.SetTheme(this, new Theme("FluentDark"));
```

#### Office 2019 Colorful Theme

```csharp
SfSkinManager.SetTheme(this, new Theme("Office2019Colorful"));
```

![Theme Example](../../../../../../../docs/gettingstarted-images/Theme.png)

### Dynamic Theme Switching

Allow users to switch themes at runtime:

```csharp
private void LightTheme_Click(object sender, RoutedEventArgs e)
{
    SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
}

private void DarkTheme_Click(object sender, RoutedEventArgs e)
{
    SfSkinManager.SetTheme(this, new Theme("MaterialDark"));
}

private void Office2019_Click(object sender, RoutedEventArgs e)
{
    SfSkinManager.SetTheme(this, new Theme("Office2019Colorful"));
}
```

### Theme Selector UI

```xml
<ComboBox SelectionChanged="ThemeSelector_Changed">
    <ComboBoxItem Content="Material Light" Tag="MaterialLight"/>
    <ComboBoxItem Content="Material Dark" Tag="MaterialDark"/>
    <ComboBoxItem Content="Fluent Light" Tag="FluentLight"/>
    <ComboBoxItem Content="Fluent Dark" Tag="FluentDark"/>
    <ComboBoxItem Content="Office 2019" Tag="Office2019Colorful"/>
</ComboBox>
```

```csharp
private void ThemeSelector_Changed(object sender, SelectionChangedEventArgs e)
{
    if (sender is ComboBox combo && combo.SelectedItem is ComboBoxItem item)
    {
        string themeName = item.Tag.ToString();
        SfSkinManager.SetTheme(this, new Theme(themeName));
    }
}
```

## Custom Theme with ThemeStudio

Create custom themes using Syncfusion's ThemeStudio:

1. **Open ThemeStudio:** Visit [ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio)
2. **Customize Colors:** Select primary, accent, and background colors
3. **Export Theme:** Download custom theme assembly
4. **Reference Assembly:** Add custom theme DLL to your project
5. **Apply Custom Theme:**

```csharp
// Apply custom theme
SfSkinManager.SetTheme(this, new Theme("MyCustomTheme"));
```

### ThemeStudio Benefits

- **Brand Matching:** Match your company colors
- **Accessibility:** Ensure proper contrast ratios
- **Consistency:** Apply across all Syncfusion controls
- **Professional:** Create polished, cohesive UI

## Visual Customization

### Error Highlighting Color

While the default red underline for errors is not directly customizable via API, you can:

1. Use themes that provide different error colors
2. Create custom theme with ThemeStudio
3. Apply custom styles to the text editor control

### Dialog Size and Position

The spell checker dialog size is automatic, but you can:

```csharp
// Dialog opens at center of parent window by default
// Position is automatically calculated
spellChecker.PerformSpellCheckUsingDialog();
```

## Advanced Customization

### Custom Spell Check Workflow

```csharp
private async void CustomSpellCheck_Click(object sender, RoutedEventArgs e)
{
    // Pre-check actions
    ShowLoadingIndicator();
    await Task.Delay(100); // Allow UI to update
    
    // Perform spell check
    spellChecker.PerformSpellCheckUsingDialog();
}

private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Post-check actions
    HideLoadingIndicator();
    UpdateDocumentStatistics();
    NotifyUser("Spell check completed");
}
```

### Integration with Document Workflow

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Mark document as reviewed
    document.IsSpellChecked = true;
    document.LastSpellCheckDate = DateTime.Now;
    
    // Update UI indicators
    spellCheckIcon.Visibility = Visibility.Visible;
    
    // Enable next workflow step
    submitButton.IsEnabled = true;
}
```

## Best Practices

### 1. Provide User Feedback

Always inform users when spell check completes:

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Provide feedback
    statusBar.Text = "Spell check completed";
}
```

### 2. Handle Completion Gracefully

Don't interrupt user workflow:

```csharp
private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Non-intrusive notification
    ShowToast("Spell check done");
}
```

### 3. Save User Theme Preference

```csharp
// Save preference
Settings.Default.Theme = "MaterialDark";
Settings.Default.Save();

// Load on startup
public MainWindow()
{
    InitializeComponent();
    SfSkinManager.SetTheme(this, new Theme(Settings.Default.Theme));
}
```

### 4. Test Themes for Accessibility

Ensure themes meet accessibility standards:

- Sufficient color contrast
- Clear error indicators
- Readable text at all sizes

## Troubleshooting

### Event Not Firing

**Issue:** SpellCheckCompleted event doesn't trigger

**Solutions:**
- Verify event handler is attached
- Check if spell check actually runs
- Ensure EnableSpellCheck is true

### Theme Not Applying

**Issue:** Theme doesn't change appearance

**Solutions:**
- Reference Syncfusion.SfSkinManager.WPF assembly
- Verify theme name is correct (case-sensitive)
- Check if theme assembly is included
- Apply theme after InitializeComponent()

### Custom Message Box Not Showing

**Issue:** Custom completion message doesn't appear

**Solutions:**
- Verify ShowMessageBox is set to false
- Check if event handler is correct type
- Ensure MessageBox call is after ShowMessageBox = false

## Related Topics

- **Spell Check Methods:** Read [spell-check-methods.md](spell-check-methods.md)
- **Dictionary Support:** Read [dictionary-support.md](dictionary-support.md)
- **Getting Started:** Read [getting-started.md](getting-started.md)

## Additional Resources

- [SfSkinManager Documentation](https://help.syncfusion.com/wpf/themes/skin-manager)
- [ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio)
- [WPF Themes Overview](https://help.syncfusion.com/wpf/themes/overview)

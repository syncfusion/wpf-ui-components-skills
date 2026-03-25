# Accessibility & Touch Support

## Table of Contents
- [Keyboard Navigation](#keyboard-navigation)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Tooltip Support](#tooltip-support)
- [Touch Input Handling](#touch-input-handling)
- [Screen Reader Support](#screen-reader-support)
- [WCAG Compliance](#wcag-compliance)
- [Focus Management](#focus-management)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Keyboard Navigation

### Tab Navigation

Navigate ribbon elements with Tab key:

```xml
<!-- Elements are naturally focusable -->
<syncfusion:RibbonTab Caption="Home" />
<syncfusion:RibbonButton Label="Save" />
<syncfusion:RibbonComboBox Label="Font" />
```

Tab order follows the order in XAML. Customize with `TabIndex`:

```xml
<syncfusion:RibbonButton Label="New" TabIndex="1" />
<syncfusion:RibbonButton Label="Open" TabIndex="2" />
<syncfusion:RibbonButton Label="Save" TabIndex="3" />
```

### Arrow Key Navigation

Navigate within groups and menus:

```csharp
// Arrow keys automatically navigate menu items
// Left/Right: Move between tabs
// Up/Down: Navigate menu items
// Enter: Select/Activate
```

### Enter/Space for Activation

Activate buttons and menus with Enter or Space:

```xml
<syncfusion:RibbonButton Label="Save" />
<!-- Tab to focus, then press Enter or Space to activate -->
```

### Alt Key for Access Keys

Define keyboard shortcuts for direct access:

```xml
<syncfusion:RibbonButton Label="Save" ToolTip="Save (Ctrl+S)" />
<!-- Ctrl+S handled in code -->
```

## Keyboard Shortcuts

### Define Global Shortcuts

```csharp
public partial class MainWindow : RibbonWindow
{
    public MainWindow()
    {
        InitializeComponent();
        RegisterKeyBindings();
    }

    private void RegisterKeyBindings()
    {
        // Ctrl+N: New
        InputBindings.Add(new KeyBinding(
            new RelayCommand(() => OnNewClick(null, null)),
            Key.N, ModifierKeys.Control));

        // Ctrl+O: Open
        InputBindings.Add(new KeyBinding(
            new RelayCommand(() => OnOpenClick(null, null)),
            Key.O, ModifierKeys.Control));

        // Ctrl+S: Save
        InputBindings.Add(new KeyBinding(
            new RelayCommand(() => OnSaveClick(null, null)),
            Key.S, ModifierKeys.Control));

        // Alt+F4: Exit
        InputBindings.Add(new KeyBinding(
            new RelayCommand(() => Close()),
            Key.F4, ModifierKeys.Alt));
    }
}
```

### Display Shortcuts in UI

Show keyboard shortcuts in tooltips:

```xml
<syncfusion:RibbonButton Label="Save" 
                         ToolTip="Save document (Ctrl+S)" />

<syncfusion:RibbonMenuItem Header="Cut" 
                           InputGestureText="Ctrl+X" />

<syncfusion:RibbonMenuItem Header="Copy" 
                           InputGestureText="Ctrl+C" />

<syncfusion:RibbonMenuItem Header="Paste" 
                           InputGestureText="Ctrl+V" />
```

### Function Keys

Map function keys for common operations:

```csharp
protected override void OnKeyDown(KeyEventArgs e)
{
    switch (e.Key)
    {
        case Key.F1:
            ShowHelp();
            e.Handled = true;
            break;
        case Key.F5:
            RefreshView();
            e.Handled = true;
            break;
        case Key.F12:
            OpenDevTools();
            e.Handled = true;
            break;
    }
    base.OnKeyDown(e);
}
```

## Tooltip Support

### Basic Tooltips

Add tooltips to ribbon elements:

```xml
<syncfusion:RibbonButton Label="Save" 
                         ToolTip="Save the current document" />

<syncfusion:RibbonButton Label="Cut" 
                         ToolTip="Cut selected content" />

<syncfusion:RibbonComboBox Label="Font" 
                           ToolTip="Select font family" />
```

### Rich Tooltips with Descriptions

```xml
<syncfusion:RibbonButton Label="Find And Replace">
    <syncfusion:RibbonButton.ToolTip>
        <StackPanel>
            <TextBlock FontWeight="Bold" Text="Find And Replace" />
            <TextBlock Text="Find and replace text (Ctrl+H)" Margin="0,5,0,0" />
            <TextBlock Text="Keyboard: Ctrl+H" Foreground="Gray" FontSize="10" Margin="0,5,0,0" />
        </StackPanel>
    </syncfusion:RibbonButton.ToolTip>
</syncfusion:RibbonButton>
```

### Tooltip Delay and Duration

```csharp
// In App.xaml.cs or initialization code
ToolTipService.SetInitialShowDelay(5000); // 5 seconds
ToolTipService.SetShowDuration(30000);   // 30 seconds

// Per control
ToolTipService.SetInitialShowDelay(myButton, 2000);
```

## Touch Input Handling

### Enable Touch Support

Syncfusion ribbon supports touch by default. Ensure touch-friendly sizing:

```xml
<!-- Larger touch targets -->
<syncfusion:RibbonButton Label="Save" 
                         Height="60"
                         Width="60"
                         Padding="5" />
```

### Touch-Friendly Button Sizing

```xml
<syncfusion:RibbonBar Header="File Operations">
    <!-- Large buttons for touch -->
    <syncfusion:RibbonButton Label="New" 
                             SizeForm="Large"
                             Margin="5" />
    
    <syncfusion:RibbonButton Label="Open" 
                             SizeForm="Large"
                             Margin="5" />
    
    <syncfusion:RibbonButton Label="Save" 
                             SizeForm="Large"
                             Margin="5" />
</syncfusion:RibbonBar>
```

### Handle Touch Events

```csharp
public partial class MainWindow : RibbonWindow
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Enable touch events
        IsManipulationEnabled = true;
        ManipulationStarted += OnManipulationStarted;
        ManipulationDelta += OnManipulationDelta;
        ManipulationCompleted += OnManipulationCompleted;
    }

    private void OnManipulationStarted(object sender, ManipulationStartedEventArgs e)
    {
        // Touch started
    }

    private void OnManipulationDelta(object sender, ManipulationDeltaEventArgs e)
    {
        // Touch in progress (dragging, pinching, etc.)
    }

    private void OnManipulationCompleted(object sender, ManipulationCompletedEventArgs e)
    {
        // Touch completed
    }
}
```

### Swipe Gestures

```csharp
private void OnManipulationDelta(object sender, ManipulationDeltaEventArgs e)
{
    var deltaX = e.DeltaManipulation.Translation.X;
    var deltaY = e.DeltaManipulation.Translation.Y;

    if (Math.Abs(deltaX) > Math.Abs(deltaY))
    {
        // Horizontal swipe
        if (deltaX > 50)
        {
            OnSwipeRight();
        }
        else if (deltaX < -50)
        {
            OnSwipeLeft();
        }
    }
}

private void OnSwipeRight()
{
    // Navigate to previous tab
    var currentTab = MainRibbon.SelectedIndex;
    if (currentTab > 0)
        MainRibbon.SelectedIndex = currentTab - 1;
}

private void OnSwipeLeft()
{
    // Navigate to next tab
    var currentTab = MainRibbon.SelectedIndex;
    if (currentTab < MainRibbon.Items.Count - 1)
        MainRibbon.SelectedIndex = currentTab + 1;
}
```

## Screen Reader Support

### ARIA Attributes

Add accessibility names and descriptions:

```xml
<syncfusion:RibbonButton Label="Save" 
                         AutomationProperties.Name="Save Document"
                         AutomationProperties.HelpText="Save the current document to disk" />

<syncfusion:RibbonComboBox Label="Font" 
                           AutomationProperties.Name="Font Selection"
                           AutomationProperties.HelpText="Select text font family" />
```

### Descriptive Labels

```xml
<!-- Good: Clear description -->
<syncfusion:RibbonButton Label="Delete All Items" 
                         ToolTip="Permanently delete all selected items" />

<!-- Poor: Unclear -->
<syncfusion:RibbonButton Label="Del" />
```

### Custom Control Text

```csharp
// Set accessibility descriptions programmatically
AutomationProperties.SetName(myButton, "Save Document");
AutomationProperties.SetHelpText(myButton, "Saves the current document");
```

## WCAG Compliance

### Color Contrast

Ensure sufficient color contrast (WCAG AA requires 4.5:1 for normal text):

```xml
<!-- Good contrast (dark on light) -->
<TextBlock Foreground="DarkBlue" Background="LightGray" Text="Important" />

<!-- Poor contrast -->
<TextBlock Foreground="LightGray" Background="White" Text="Invisible" />
```

### Alternative Text for Icons

```xml
<!-- Icon button with tooltip -->
<syncfusion:RibbonButton Label="Save" 
                         SmallIcon="pack://application:,,,/Resources/save.png"
                         ToolTip="Save document"
                         AutomationProperties.Name="Save" />
```

### Focus Indicators

Ensure focus is clearly visible:

```xml
<Window.Resources>
    <Style x:Key="AccessibleButtonStyle" TargetType="syncfusion:RibbonButton">
        <Setter Property="FocusVisualStyle">
            <Setter.Value>
                <Style>
                    <Setter Property="Control.Template">
                        <Setter.Value>
                            <ControlTemplate>
                                <Rectangle StrokeThickness="2" 
                                           Stroke="Blue"
                                           StrokeDashArray="1 2" />
                            </ControlTemplate>
                        </Setter.Value>
                    </Setter>
                </Style>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>
```

### Text Scaling

Ensure text can scale without breaking layout:

```xml
<!-- Use relative sizing -->
<TextBlock FontSize="{DynamicResource {x:Static SystemFonts.MenuFontSizeKey}}" />

<!-- Avoid fixed sizes -->
<TextBlock FontSize="11" />
```

## Focus Management

### Default Focus

Set initial focus:

```xml
<syncfusion:RibbonButton Label="Save" FocusManager.FocusedElement="{Binding RelativeSource={RelativeSource Self}}" />
```

Or in code:

```csharp
public MainWindow()
{
    InitializeComponent();
    Loaded += (s, e) => SaveButton.Focus();
}
```

### Focus Containment

Keep focus within ribbon when navigating:

```csharp
private void RibbonBar_KeyDown(object sender, KeyEventArgs e)
{
    if (e.Key == Key.Tab)
    {
        var bar = sender as RibbonBar;
        var lastControl = bar?.Items.OfType<UIElement>().LastOrDefault();
        
        if (Keyboard.FocusedElement == lastControl && !e.IsDown)
        {
            // Move to first control in bar
            var firstControl = bar?.Items.OfType<UIElement>().FirstOrDefault();
            firstControl?.Focus();
            e.Handled = true;
        }
    }
}
```

## Common Patterns

### Pattern 1: Comprehensive Keyboard Support
```xml
<syncfusion:RibbonBar Header="File">
    <syncfusion:RibbonButton Label="New" ToolTip="New Document (Ctrl+N)" />
    <syncfusion:RibbonButton Label="Open" ToolTip="Open File (Ctrl+O)" />
    <syncfusion:RibbonButton Label="Save" ToolTip="Save (Ctrl+S)" />
</syncfusion:RibbonBar>
```

### Pattern 2: Accessible Form Controls
```xml
<syncfusion:RibbonBar Header="Search">
    <syncfusion:RibbonTextBox Text="Find:" 
                              AutomationProperties.Name="Search Text Input"
                              Width="150" />
    <syncfusion:RibbonButton Label="Find All" 
                             ToolTip="Find all occurrences (Ctrl+F)" />
</syncfusion:RibbonBar>
```

### Pattern 3: Screen Reader Friendly Application
```csharp
// Initialize accessibility
public App()
{
    InitializeComponent();
    ConfigureAccessibility();
}

private void ConfigureAccessibility()
{
    // Set application name for screen readers
    AutomationProperties.SetName(this, "Word Processor Application");
}
```

## Troubleshooting

### Issue: Focus not moving in ribbon
**Solution:** Ensure `Focusable="True"` on elements. Check `TabIndex` order.

### Issue: Tooltip not appearing
**Solution:** Set `ToolTipService.ShowOnDisabled="True"` if button is disabled. Check tooltip content isn't empty.

### Issue: Screen reader not reading ribbon
**Solution:** Add `AutomationProperties.Name` to key elements. Ensure labels are descriptive.

### Issue: Touch targets too small
**Solution:** Increase button size. Use `SizeForm="Large"`. Add padding/margin.

### Issue: Keyboard shortcuts not working
**Solution:** Verify `InputBindings` are registered. Check modifiers (Ctrl, Alt, Shift). Ensure focus is on window.

### Issue: Color contrast failing WCAG
**Solution:** Use online contrast checkers. Increase contrast ratio. Avoid light text on light backgrounds.

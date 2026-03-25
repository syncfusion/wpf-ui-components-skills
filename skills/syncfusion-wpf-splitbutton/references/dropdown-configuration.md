# Dropdown Configuration and Events

This guide covers dropdown direction configuration, event handling, and multiline text support for the Split Button control.

## Table of Contents
- [Dropdown Direction](#dropdown-direction)
- [Dropdown Events](#dropdown-events)
- [Dropdown Menu Item Events](#dropdown-menu-item-events)
- [Multiline Text Support](#multiline-text-support)

## Dropdown Direction

The dropdown direction controls the position of the popup when the dropdown arrow is clicked. Use the `DropDirection` enumeration to set the direction.

### Available Directions

The `DropDirection` enumeration contains the following values:

- **Left** - Popup appears to the left of the button
- **Right** - Popup appears to the right of the button
- **BottomLeft** - Popup appears below and aligned to the left (default)
- **BottomRight** - Popup appears below and aligned to the right
- **TopLeft** - Popup appears above and aligned to the left
- **TopRight** - Popup appears above and aligned to the right

**Default Value:** `BottomLeft`

### BottomLeft Direction

```xaml
<syncfusion:SplitButtonAdv DropDirection="BottomLeft" 
                            Label="Colors"
                            SizeMode="Normal">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Orange"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="SkyBlue"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Yellow"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Red"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Black"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
splitButton.Label = "Colors";
splitButton.DropDirection = DropDirection.BottomLeft;
splitButton.SizeMode = SizeMode.Normal;

DropDownMenuGroup menu = new DropDownMenuGroup();
menu.Items.Add(new DropDownMenuItem() { Header = "Orange", HorizontalAlignment = HorizontalAlignment.Left });
menu.Items.Add(new DropDownMenuItem() { Header = "Skyblue", HorizontalAlignment = HorizontalAlignment.Left });
menu.Items.Add(new DropDownMenuItem() { Header = "Yellow", HorizontalAlignment = HorizontalAlignment.Left });
menu.Items.Add(new DropDownMenuItem() { Header = "Red", HorizontalAlignment = HorizontalAlignment.Left });
menu.Items.Add(new DropDownMenuItem() { Header = "Black", HorizontalAlignment = HorizontalAlignment.Left });

splitButton.Content = menu;
```

### BottomRight Direction

```xaml
<syncfusion:SplitButtonAdv DropDirection="BottomRight" 
                            Label="Colors"
                            SizeMode="Normal">
    <!-- Menu items -->
</syncfusion:SplitButtonAdv>
```

```csharp
splitButton.DropDirection = DropDirection.BottomRight;
```

### Right Direction

```xaml
<syncfusion:SplitButtonAdv DropDirection="Right" 
                            Label="Colors"
                            SizeMode="Normal">
    <!-- Menu items -->
</syncfusion:SplitButtonAdv>
```

```csharp
splitButton.DropDirection = DropDirection.Right;
```

### Left Direction

```xaml
<syncfusion:SplitButtonAdv DropDirection="Left" 
                            Label="Colors"
                            SizeMode="Normal">
    <!-- Menu items -->
</syncfusion:SplitButtonAdv>
```

```csharp
splitButton.DropDirection = DropDirection.Left;
```

### TopLeft Direction

```xaml
<syncfusion:SplitButtonAdv DropDirection="TopLeft" 
                            Label="Colors"
                            SizeMode="Normal">
    <!-- Menu items -->
</syncfusion:SplitButtonAdv>
```

```csharp
splitButton.DropDirection = DropDirection.TopLeft;
```

### TopRight Direction

```xaml
<syncfusion:SplitButtonAdv DropDirection="TopRight" 
                            Label="Colors"
                            SizeMode="Normal">
    <!-- Menu items -->
</syncfusion:SplitButtonAdv>
```

```csharp
splitButton.DropDirection = DropDirection.TopRight;
```

### Use Cases

**BottomLeft / BottomRight:**
- Standard dropdown menus
- Top toolbars and ribbons
- Headers and title bars

**TopLeft / TopRight:**
- Bottom toolbars
- Context menus near bottom of window
- Floating buttons at bottom of screen

**Left / Right:**
- Side panels
- Vertical toolbars
- Compact layouts with horizontal space constraints

## Dropdown Events

The Split Button provides various events for handling dropdown popup lifecycle.

### DropDownOpening

Occurs before the dropdown menu popup opens. Use this event to perform actions or cancel the opening.

**Event Type:** `CancelEventHandler`  
**Can Cancel:** Yes

**XAML:**

```xaml
<syncfusion:SplitButtonAdv x:Name="splitButton" 
                            DropDownOpening="SplitButton_DropDownOpening"/>
```

**C# Event Handler:**

```csharp
using System.ComponentModel;

private void SplitButton_DropDownOpening(object sender, CancelEventArgs e)
{
    // Perform pre-opening logic
    // Example: Validate state before opening
    if (!IsValidState())
    {
        e.Cancel = true; // Cancel the opening
        MessageBox.Show("Cannot open dropdown at this time");
    }
}
```

**Code-Behind Registration:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
splitButton.DropDownOpening += new CancelEventHandler(SplitButton_DropDownOpening);
```

### DropDownOpened

Occurs after the dropdown menu popup has opened. Use this event to perform actions after the popup is visible.

**Event Type:** `RoutedEventHandler`  
**Can Cancel:** No

**XAML:**

```xaml
<syncfusion:SplitButtonAdv x:Name="splitButton" 
                            DropDownOpened="SplitButton_DropDownOpened"/>
```

**C# Event Handler:**

```csharp
private void SplitButton_DropDownOpened(object sender, RoutedEventArgs e)
{
    // Perform post-opening logic
    // Example: Log opening event or update UI
    Console.WriteLine("Dropdown has opened");
}
```

**Code-Behind Registration:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
splitButton.DropDownOpened += new RoutedEventHandler(SplitButton_DropDownOpened);
```

### DropDownClosing

Occurs before the dropdown menu popup closes. Use this event to perform actions or cancel the closing.

**Event Type:** `CancelEventHandler`  
**Can Cancel:** Yes

**XAML:**

```xaml
<syncfusion:SplitButtonAdv x:Name="splitButton" 
                            DropDownClosing="SplitButton_DropDownClosing"/>
```

**C# Event Handler:**

```csharp
private void SplitButton_DropDownClosing(object sender, CancelEventArgs e)
{
    // Perform pre-closing logic
    // Example: Validate selection before closing
    if (!HasValidSelection())
    {
        e.Cancel = true; // Cancel the closing
        MessageBox.Show("Please make a selection");
    }
}
```

**Code-Behind Registration:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
splitButton.DropDownClosing += new CancelEventHandler(SplitButton_DropDownClosing);
```

### DropDownClosed

Occurs after the dropdown menu popup has closed. Use this event to perform cleanup or update logic.

**Event Type:** `RoutedEventHandler`  
**Can Cancel:** No

**XAML:**

```xaml
<syncfusion:SplitButtonAdv x:Name="splitButton" 
                            DropDownClosed="SplitButton_DropDownClosed"/>
```

**C# Event Handler:**

```csharp
private void SplitButton_DropDownClosed(object sender, RoutedEventArgs e)
{
    // Perform post-closing logic
    // Example: Save state or clean up resources
    SaveCurrentState();
    Console.WriteLine("Dropdown has closed");
}
```

**Code-Behind Registration:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
splitButton.DropDownClosed += new RoutedEventHandler(SplitButton_DropDownClosed);
```

### Click Event

Occurs when the Split Button control is clicked (primary button, not dropdown arrow).

**Event Type:** `RoutedEventHandler`

**XAML:**

```xaml
<syncfusion:SplitButtonAdv x:Name="splitButton" 
                            Click="SplitButton_Click"/>
```

**C# Event Handler:**

```csharp
private void SplitButton_Click(object sender, RoutedEventArgs e)
{
    // Perform primary action
    MessageBox.Show("Primary button clicked");
}
```

**Code-Behind Registration:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
splitButton.Click += new RoutedEventHandler(SplitButton_Click);
```

### Complete Event Example

```xaml
<Window x:Class="SplitButton_Events.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:SplitButtonAdv x:Name="splitButton" 
                                    Label="Colors"
                                    Click="SplitButton_Click"
                                    DropDownOpening="SplitButton_DropDownOpening"
                                    DropDownOpened="SplitButton_DropDownOpened"
                                    DropDownClosing="SplitButton_DropDownClosing"
                                    DropDownClosed="SplitButton_DropDownClosed">
            <syncfusion:DropDownMenuGroup>
                <syncfusion:DropDownMenuItem Header="Red"/>
                <syncfusion:DropDownMenuItem Header="Green"/>
                <syncfusion:DropDownMenuItem Header="Blue"/>
            </syncfusion:DropDownMenuGroup>
        </syncfusion:SplitButtonAdv>
    </Grid>
</Window>
```

```csharp
using System.ComponentModel;
using System.Windows;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }

    private void SplitButton_Click(object sender, RoutedEventArgs e)
    {
        MessageBox.Show("Button clicked");
    }

    private void SplitButton_DropDownOpening(object sender, CancelEventArgs e)
    {
        Console.WriteLine("Opening dropdown...");
        // Optionally cancel: e.Cancel = true;
    }

    private void SplitButton_DropDownOpened(object sender, RoutedEventArgs e)
    {
        Console.WriteLine("Dropdown opened");
    }

    private void SplitButton_DropDownClosing(object sender, CancelEventArgs e)
    {
        Console.WriteLine("Closing dropdown...");
        // Optionally cancel: e.Cancel = true;
    }

    private void SplitButton_DropDownClosed(object sender, RoutedEventArgs e)
    {
        Console.WriteLine("Dropdown closed");
    }
}
```

## Dropdown Menu Item Events

### Click Event

Occurs when a dropdown menu item is clicked.

**Event Type:** `RoutedEventHandler`

**XAML:**

```xaml
<syncfusion:DropDownMenuItem x:Name="dropDownMenuItem" 
                              Header="Option 1"
                              Click="DropDownMenuItem_Click"/>
```

**C# Event Handler:**

```csharp
private void DropDownMenuItem_Click(object sender, RoutedEventArgs e)
{
    var menuItem = sender as DropDownMenuItem;
    MessageBox.Show($"Selected: {menuItem.Header}");
}
```

**Code-Behind Registration:**

```csharp
DropDownMenuItem dropDownMenuItem = new DropDownMenuItem();
dropDownMenuItem.Click += new RoutedEventHandler(DropDownMenuItem_Click);
```

### IsCheckedChanged Event

Occurs when a dropdown menu item is checked or unchecked. This event only fires when `IsCheckable` is set to `true`.

**Event Type:** `DependencyPropertyChangedEventHandler`

**XAML:**

```xaml
<syncfusion:DropDownMenuItem x:Name="dropDownMenuItem" 
                              Header="Option 1"
                              IsCheckable="True" 
                              IsCheckedChanged="DropDownMenuItem_IsCheckedChanged"/>
```

**C# Event Handler:**

```csharp
private void DropDownMenuItem_IsCheckedChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    var menuItem = d as DropDownMenuItem;
    bool isChecked = (bool)e.NewValue;
    
    MessageBox.Show($"{menuItem.Header} is now {(isChecked ? "checked" : "unchecked")}");
}
```

**Code-Behind Registration:**

```csharp
DropDownMenuItem dropDownMenuItem = new DropDownMenuItem();
dropDownMenuItem.IsCheckable = true;
dropDownMenuItem.IsCheckedChanged += new RoutedEventHandler(DropDownMenuItem_IsCheckedChanged);
```

**Complete Checkable Example:**

```xaml
<syncfusion:SplitButtonAdv Label="Options">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem Header="Auto-save" 
                                     IsCheckable="True" 
                                     IsChecked="True"
                                     IsCheckedChanged="MenuItem_IsCheckedChanged"/>
        
        <syncfusion:DropDownMenuItem Header="Auto-backup" 
                                     IsCheckable="True"
                                     IsCheckedChanged="MenuItem_IsCheckedChanged"/>
        
        <syncfusion:DropDownMenuItem Header="Show notifications" 
                                     IsCheckable="True" 
                                     IsChecked="True"
                                     IsCheckedChanged="MenuItem_IsCheckedChanged"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

```csharp
private void MenuItem_IsCheckedChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    var menuItem = d as DropDownMenuItem;
    bool isChecked = (bool)e.NewValue;
    
    // Update settings based on menu item
    string option = menuItem.Header.ToString();
    UpdateSetting(option, isChecked);
}
```

## Multiline Text Support

The `IsMultiLine` property enables displaying text content across multiple lines for precise viewing. This is particularly useful for long button labels in large size mode.

**Important:** This property only works with `SizeMode="Large"`.

### Basic Multiline Example

**XAML:**

```xaml
<syncfusion:SplitButtonAdv Label="Sign in with your Syncfusion Account"  
                            SizeMode="Large" 
                            IsMultiLine="True"/>
```

**C#:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
splitButton.Label = "Sign in with your Syncfusion Account";
splitButton.IsMultiLine = true;
splitButton.SizeMode = SizeMode.Large;
```

### Multiline with Icon

```xaml
<Window.Resources>
    <DataTemplate x:Key="loginIcon">
        <Grid Width="32" Height="32">
            <Path Data="M10,0 C15.5,0 20,4.5 20,10 C20,15.5 15.5,20 10,20 C4.5,20 0,15.5 0,10 C0,4.5 4.5,0 10,0 Z"
                  Fill="#FF3A3A38"
                  Stretch="Fill"/>
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:SplitButtonAdv Label="Sign in with your Syncfusion Account"  
                            SizeMode="Large" 
                            IsMultiLine="True"
                            IconTemplate="{StaticResource loginIcon}"/>
```

### Text Wrapping Behavior

**Default (IsMultiLine="False"):**
- Text appears on single line
- Long text may be truncated with ellipsis (...)
- Button width adjusts to text length

**With IsMultiLine="True":**
- Text wraps to multiple lines based on button width
- No truncation or ellipsis
- Button height adjusts to accommodate wrapped text
- Works only in Large size mode

### Complete Multiline Example

```xaml
<StackPanel Margin="20">
    <TextBlock Text="Single Line (Default):" Margin="0,0,0,5"/>
    <syncfusion:SplitButtonAdv Label="Sign in with your Syncfusion Account"  
                                SizeMode="Large" 
                                IsMultiLine="False"
                                Margin="0,0,0,20"/>
    
    <TextBlock Text="Multiline (IsMultiLine=True):" Margin="0,0,0,5"/>
    <syncfusion:SplitButtonAdv Label="Sign in with your Syncfusion Account"  
                                SizeMode="Large" 
                                IsMultiLine="True"
                                Width="150"/>
</StackPanel>
```

### Use Cases for Multiline

**When to use:**
- Descriptive button labels that are too long for single line
- Bilingual or localized text that may be longer in some languages
- Buttons in narrow panels or sidebars
- Accessibility requirements for larger, more readable text

**When not to use:**
- Small or Normal size modes (not supported)
- Short labels that fit on one line
- Dense UI layouts where vertical space is limited
- Toolbars or ribbons with many buttons

## Event Handling Best Practices

### 1. Use Commands over Events in MVVM

```csharp
// Preferred: Command binding
<syncfusion:SplitButtonAdv Command="{Binding SaveCommand}"/>

// Avoid in MVVM: Event handlers in code-behind
<syncfusion:SplitButtonAdv Click="SplitButton_Click"/>
```

### 2. Cancel Events When Needed

```csharp
private void SplitButton_DropDownOpening(object sender, CancelEventArgs e)
{
    if (!CanOpenDropdown())
    {
        e.Cancel = true;
        NotifyUser("Dropdown cannot be opened");
    }
}
```

### 3. Unsubscribe from Events

```csharp
// Subscribe
splitButton.DropDownOpening += SplitButton_DropDownOpening;

// Unsubscribe when done
splitButton.DropDownOpening -= SplitButton_DropDownOpening;
```

### 4. Handle Null References

```csharp
private void DropDownMenuItem_Click(object sender, RoutedEventArgs e)
{
    var menuItem = sender as DropDownMenuItem;
    if (menuItem != null)
    {
        ProcessMenuItem(menuItem);
    }
}
```

## Troubleshooting

**Problem:** Multiline text not wrapping  
**Solution:** Ensure `SizeMode="Large"` and `IsMultiLine="True"` are both set

**Problem:** DropDownOpening event not canceling  
**Solution:** Set `e.Cancel = true` in the event handler

**Problem:** Events firing multiple times  
**Solution:** Check for duplicate event handler subscriptions

**Problem:** IsCheckedChanged not firing  
**Solution:** Verify `IsCheckable="True"` is set on the menu item

## Summary

This guide covered:
- Dropdown direction options (Left, Right, BottomLeft, BottomRight, TopLeft, TopRight)
- Dropdown lifecycle events (Opening, Opened, Closing, Closed)
- Menu item events (Click, IsCheckedChanged)
- Multiline text support for long labels in Large size mode
- Best practices for event handling

## Related Topics

- [Command Binding](command-binding.md) - Preferred alternative to events in MVVM
- [Dropdown Menu Items](dropdown-menu-items.md) - Menu item configuration
- [Getting Started](getting-started.md) - Size modes and basic setup

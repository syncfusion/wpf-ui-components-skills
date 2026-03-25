# Keyboard and Accessibility in SfMultiColumnDropDownControl

## Overview

SfMultiColumnDropDownControl provides comprehensive keyboard navigation and accessibility features, including full keyboard shortcut support, UI automation for testing, and compliance with accessibility standards.

## Keyboard Shortcuts

The control supports various keyboard shortcuts for efficient navigation and interaction.

### Keyboard Shortcut Table

| Shortcut | Action | Description |
|----------|--------|-------------|
| **Enter** | Confirm Selection | Selects the currently focused item and closes the popup |
| **Esc** | Cancel | Closes the popup without changing selection, restores previous value |
| **F4** | Toggle Popup | Opens the popup if closed, closes if open |
| **Alt + Down Arrow** | Open Popup | Opens the dropdown popup |
| **Alt + Up Arrow** | Close Popup | Closes the dropdown popup |
| **Down Arrow** | Navigate Down | Moves focus to the next item in the dropdown (when popup is open) |
| **Up Arrow** | Navigate Up | Moves focus to the previous item in the dropdown (when popup is open) |
| **Home** | First Item | Moves focus to the first item in the dropdown |
| **End** | Last Item | Moves focus to the last item in the dropdown |
| **Page Up** | Page Up | Scrolls up one page in the dropdown |
| **Page Down** | Page Down | Scrolls down one page in the dropdown |
| **Tab** | Next Control | Moves focus to the next control in tab order |
| **Shift + Tab** | Previous Control | Moves focus to the previous control in tab order |
| **Space** | Toggle Selection | Toggles selection in multi-selection mode |

### Keyboard Navigation Example

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    SelectedIndex="0"/>
```

**User Workflow:**
1. **Tab** to focus the control
2. **F4** or **Alt+Down** to open popup
3. **Down Arrow** / **Up Arrow** to navigate items
4. **Enter** to select and close
5. **Esc** to cancel and close

### Multi-Selection Keyboard Support

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    SelectionMode="Multiple"/>
```

**Multi-Selection Workflow:**
1. **F4** to open popup
2. **Down Arrow** to navigate
3. **Space** to toggle selection of focused item
4. **Down Arrow** to next item
5. **Space** to select additional items
6. **Enter** to confirm all selections

### Handling Keyboard Events

```csharp
// PreviewKeyDown for custom keyboard handling
sfMultiColumn.PreviewKeyDown += (sender, e) =>
{
    if (e.Key == Key.F2)
    {
        // Custom action for F2
        OpenAdvancedSearch();
        e.Handled = true;
    }
    
    if (e.Key == Key.Delete && sfMultiColumn.SelectedItem != null)
    {
        // Delete selected item
        DeleteSelectedItem();
        e.Handled = true;
    }
};
```

## Accessibility Features

### Screen Reader Support

The control is compatible with screen readers and provides appropriate accessibility information.

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AutomationProperties.Name="Order Selection Dropdown"
    AutomationProperties.HelpText="Select an order from the list"/>
```

### Setting Automation Properties

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    AutomationProperties.AutomationId="OrderDropDown"
    AutomationProperties.Name="Order Selector"
    AutomationProperties.HelpText="Choose an order to view details"
    AutomationProperties.LabeledBy="{Binding ElementName=OrderLabel}"/>

<TextBlock x:Name="OrderLabel" Text="Select Order:"/>
```

### AutomationProperties Best Practices

```csharp
// Set properties in code
AutomationProperties.SetName(sfMultiColumn, "Customer Order Dropdown");
AutomationProperties.SetHelpText(sfMultiColumn, "Select a customer order from the dropdown list");
AutomationProperties.SetAutomationId(sfMultiColumn, "MainOrderDropDown");
```

## UI Automation Support

### Coded UI Test

SfMultiColumnDropDownControl supports Coded UI Test automation for creating automated tests.

#### Level 1: Record and Detect

- Records user actions on the control
- Detects UI elements during playback
- Captures interactions automatically

#### Level 2: Custom Properties

Access custom properties when inspecting the control with the Coded UI Test Builder.

**Available Properties:**

| Property | Type | Description |
|----------|------|-------------|
| **AllowAutoComplete** | bool | Autocomplete enabled status |
| **AllowNullInput** | bool | Null input allowed status |
| **AllowImmediatePopup** | bool | Immediate popup enabled status |
| **AllowIncrementalFiltering** | bool | Incremental filtering enabled status |
| **AllowCaseSensitiveFiltering** | bool | Case-sensitive filtering enabled |
| **AllowSpinOnMouseWheel** | bool | Mouse wheel spin enabled |
| **DisplayMember** | string | Display member property name |
| **IsDropDownOpen** | bool | Popup open/closed state |
| **SelectedIndex** | int | Currently selected index |
| **ValueMember** | string | Value member property name |

#### Level 3: Simplified Code Generation

Coded UI Test Builder generates simplified code using custom classes.

### Example Coded UI Test

```csharp
[TestMethod]
public void TestOrderSelection()
{
    // Get the control
    SfMultiColumnDropDownControl dropdown = 
        this.UIMap.UIMainWindowWindow.UISfMultiColumnDropDownControl;
    
    // Verify initial state
    Assert.AreEqual(-1, dropdown.SelectedIndex);
    Assert.IsFalse(dropdown.IsDropDownOpen);
    
    // Open dropdown
    dropdown.IsDropDownOpen = true;
    
    // Select item
    dropdown.SelectedIndex = 2;
    
    // Verify selection
    Assert.AreEqual(2, dropdown.SelectedIndex);
    Assert.AreEqual("OrderID", dropdown.DisplayMember);
}
```

## Quick Test Professional (QTP)

SfMultiColumnDropDownControl supports QTP automation for test recording and playback.

### Available QTP Methods

| Method | Parameters | Return Type | Description |
|--------|------------|-------------|-------------|
| **SetSelectedIndex** | int index | void | Sets the selected index from the popup |
| **ShowPopup** | None | void | Opens the dropdown popup |
| **HidePopup** | None | void | Closes the dropdown popup |

### QTP Test Example

```vbscript
' Get the control
Set multiColumnDropDown = Window("MainWindow").SfMultiColumnDropDownControl("sfMultiColumn")

' Open the popup
multiColumnDropDown.ShowPopup

' Wait for popup to open
Wait(1)

' Set selection
multiColumnDropDown.SetSelectedIndex(3)

' Wait for selection
Wait(1)

' Close popup
multiColumnDropDown.HidePopup

' Verify selection
selectedIndex = multiColumnDropDown.GetROProperty("SelectedIndex")
If selectedIndex = 3 Then
    Reporter.ReportEvent micPass, "Selection", "Item selected successfully"
Else
    Reporter.ReportEvent micFail, "Selection", "Item selection failed"
End If
```

### QTP Test Recording

```vbscript
' QTP automatically records these actions:
' 1. Click on dropdown
Syncfusion.UI.Xaml.Grid.SfMultiColumnDropDownControl("sfMultiColumn").ShowPopup

' 2. Select item
Syncfusion.UI.Xaml.Grid.SfMultiColumnDropDownControl("sfMultiColumn").SetSelectedIndex(2)

' 3. Close dropdown
Syncfusion.UI.Xaml.Grid.SfMultiColumnDropDownControl("sfMultiColumn").HidePopup
```

## Focus Management

### Tab Order

```xml
<StackPanel>
    <TextBox TabIndex="0" Text="First Name"/>
    <TextBox TabIndex="1" Text="Last Name"/>
    
    <syncfusion:SfMultiColumnDropDownControl 
        x:Name="sfMultiColumn"
        TabIndex="2"
        ItemsSource="{Binding Orders}"
        DisplayMember="CustomerName"/>
    
    <Button TabIndex="3" Content="Submit"/>
</StackPanel>
```

### Focus on Load

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Set focus to dropdown on load
    this.Loaded += (s, e) => 
    {
        sfMultiColumn.Focus();
    };
}
```

### Handling Focus Events

```csharp
// GotFocus event
sfMultiColumn.GotFocus += (sender, e) =>
{
    // Highlight or prepare control
    sfMultiColumn.BorderBrush = Brushes.Blue;
};

// LostFocus event
sfMultiColumn.LostFocus += (sender, e) =>
{
    // Validate or process
    sfMultiColumn.BorderBrush = Brushes.Gray;
    ValidateSelection();
};
```

## Accessibility Compliance

### WCAG 2.1 Considerations

#### Keyboard Accessibility (A)
- All functionality available via keyboard ✓
- No keyboard traps ✓
- Visible focus indicators ✓

#### Focus Visible (AA)
```xml
<Style TargetType="syncfusion:SfMultiColumnDropDownControl">
    <Setter Property="FocusVisualStyle">
        <Setter.Value>
            <Style>
                <Setter Property="Control.Template">
                    <Setter.Value>
                        <ControlTemplate>
                            <Rectangle Stroke="Blue" StrokeThickness="2" 
                                       StrokeDashArray="1 2" SnapsToDevicePixels="True"/>
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </Setter.Value>
    </Setter>
</Style>
```

#### Label Association
```xml
<StackPanel>
    <Label Content="Select Customer Order:" Target="{Binding ElementName=sfMultiColumn}"/>
    <syncfusion:SfMultiColumnDropDownControl 
        x:Name="sfMultiColumn"
        ItemsSource="{Binding Orders}"/>
</StackPanel>
```

### High Contrast Support

```csharp
// Detect high contrast mode
if (SystemParameters.HighContrast)
{
    // Adjust colors for high contrast
    sfMultiColumn.PopupBorderBrush = SystemColors.WindowTextBrush;
    sfMultiColumn.PopupBackground = SystemColors.WindowBrush;
}
```

## Best Practices

### 1. Provide Clear Labels

```xml
<Label Content="Customer:" Target="{Binding ElementName=sfMultiColumn}"/>
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    AutomationProperties.Name="Customer Selector"/>
```

### 2. Set Appropriate Tab Order

```xml
<!-- Ensure logical tab sequence -->
<syncfusion:SfMultiColumnDropDownControl TabIndex="2"/>
```

### 3. Add Helpful Tooltips

```xml
<syncfusion:SfMultiColumnDropDownControl 
    ToolTip="Select a customer order. Use F4 or Alt+Down to open dropdown."/>
```

### 4. Implement Keyboard Shortcuts

```csharp
// Add custom shortcuts
sfMultiColumn.PreviewKeyDown += (sender, e) =>
{
    if (e.KeyboardDevice.Modifiers == ModifierKeys.Control && e.Key == Key.F)
    {
        // Ctrl+F for search
        FocusSearchBox();
        e.Handled = true;
    }
};
```

### 5. Test with Screen Readers

- Test with NVDA or JAWS
- Verify all content is announced
- Ensure navigation is logical

### 6. Support High Contrast Themes

```csharp
SystemEvents.UserPreferenceChanged += (sender, e) =>
{
    if (e.Category == UserPreferenceCategory.Accessibility)
    {
        UpdateHighContrastColors();
    }
};
```

## Troubleshooting

### Keyboard Shortcuts Not Working

**Problem:** Keyboard shortcuts don't respond

**Solutions:**
```csharp
// Ensure control has focus
sfMultiColumn.Focus();

// Check if another control is handling the key
sfMultiColumn.PreviewKeyDown += (sender, e) =>
{
    Console.WriteLine($"Key pressed: {e.Key}");
};
```

### Tab Navigation Skips Control

**Problem:** Tab key doesn't focus the control

**Solutions:**
```xml
<!-- Verify control is focusable -->
<syncfusion:SfMultiColumnDropDownControl 
    Focusable="True"
    IsTabStop="True"
    TabIndex="1"/>
```

### Screen Reader Not Announcing

**Problem:** Screen reader doesn't read control information

**Solutions:**
```xml
<syncfusion:SfMultiColumnDropDownControl 
    AutomationProperties.Name="Order Dropdown"
    AutomationProperties.HelpText="Select an order from the list"
    AutomationProperties.AutomationId="OrderSelector"/>
```

### Coded UI Test Not Finding Control

**Problem:** Automated test can't locate the control

**Solutions:**
```xml
<!-- Set unique AutomationId -->
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    AutomationProperties.AutomationId="MainOrderDropDown"/>
```

```csharp
// Search by AutomationId in test
var control = window.SearchFor<SfMultiColumnDropDownControl>(
    new { AutomationId = "MainOrderDropDown" });
```

### Focus Visual Not Showing

**Problem:** No visual indicator when control is focused

**Solution:**
```xml
<syncfusion:SfMultiColumnDropDownControl>
    <syncfusion:SfMultiColumnDropDownControl.FocusVisualStyle>
        <Style>
            <Setter Property="Control.Template">
                <Setter.Value>
                    <ControlTemplate>
                        <Rectangle Stroke="{DynamicResource {x:Static SystemColors.HighlightBrushKey}}" 
                                   StrokeThickness="2" 
                                   SnapsToDevicePixels="True"/>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfMultiColumnDropDownControl.FocusVisualStyle>
</syncfusion:SfMultiColumnDropDownControl>
```

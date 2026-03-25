# Toggle State in WPF ButtonAdv

ButtonAdv supports toggle (on/off) behavior through the `IsCheckable` property, making it behave
like a `ToggleButton`. This is useful for Bold/Italic formatting buttons, sidebar toggles, or any
scenario where a button represents a persistent on/off state.

## Table of Contents
- [Enabling Toggle Behavior](#enabling-toggle-behavior)
- [Setting Initial Checked State](#setting-initial-checked-state)
- [Checked and Unchecked Events](#checked-and-unchecked-events)
- [Click Event](#click-event)
- [Combining Toggle with MVVM](#combining-toggle-with-mvvm)
- [Common Patterns](#common-patterns)

---

## Enabling Toggle Behavior

Set `IsCheckable="True"` to make the button act as a toggle. When clicked, it alternates between
checked and unchecked states, visually highlighting when active.

```xaml
<syncfusion:ButtonAdv Label="Bold"
                      SizeMode="Normal"
                      SmallIcon="Images/bold.png"
                      IsCheckable="True"/>
```

```csharp
ButtonAdv button = new ButtonAdv();
button.Label = "Bold";
button.SizeMode = SizeMode.Normal;
button.SmallIcon = new BitmapImage(new Uri("Images/bold.png", UriKind.RelativeOrAbsolute));
button.IsCheckable = true;
```

> **Default:** `IsCheckable` is `false`. Without it, ButtonAdv behaves as a standard push button
> and does not retain any pressed state.

---

## Setting Initial Checked State

Use `IsChecked` to define the button's state at startup. When `IsChecked="True"`, the button
renders in its "on" state immediately on load.

```xaml
<syncfusion:ButtonAdv Label="Pin"
                      SizeMode="Normal"
                      SmallIcon="Images/pin.png"
                      IsCheckable="True"
                      IsChecked="True"/>
```

```csharp
button.IsCheckable = true;
button.IsChecked = true;  // Starts in checked/on state
```

---

## Checked and Unchecked Events

Subscribe to `Checked` and `Unchecked` to respond when the toggle state changes.

**XAML:**
```xaml
<syncfusion:ButtonAdv Label="Bold"
                      IsCheckable="True"
                      Checked="Bold_Checked"
                      Unchecked="Bold_Unchecked"/>
```

**Code-behind:**
```csharp
private void Bold_Checked(object sender, RoutedEventArgs e)
{
    // Apply bold formatting
    richTextBox.Selection.ApplyPropertyValue(
        TextElement.FontWeightProperty, FontWeights.Bold);
}

private void Bold_Unchecked(object sender, RoutedEventArgs e)
{
    // Remove bold formatting
    richTextBox.Selection.ApplyPropertyValue(
        TextElement.FontWeightProperty, FontWeights.Normal);
}
```

> **Note:** `Checked` and `Unchecked` only fire when `IsCheckable="True"`. On a standard
> (non-checkable) button, these events never trigger.

---

## Click Event

The `Click` event fires on every button click regardless of `IsCheckable` state.

**XAML:**
```xaml
<syncfusion:ButtonAdv Label="Submit" Click="Submit_Click"/>
```

**Code-behind:**
```csharp
private void Submit_Click(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Button clicked!");
}
```

**C# subscription:**
```csharp
ButtonAdv button = new ButtonAdv();
button.Click += new RoutedEventHandler(Button_Click);

private void Button_Click(object sender, RoutedEventArgs e)
{
    // Handle click
}
```

---

## Combining Toggle with MVVM

Bind `IsChecked` to a ViewModel property for clean MVVM toggle state management.

**ViewModel:**
```csharp
public class EditorViewModel : INotifyPropertyChanged
{
    private bool _isBold;
    public bool IsBold
    {
        get => _isBold;
        set { _isBold = value; OnPropertyChanged(nameof(IsBold)); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**XAML:**
```xaml
<Window.DataContext>
    <local:EditorViewModel/>
</Window.DataContext>

<syncfusion:ButtonAdv Label="Bold"
                      SizeMode="Normal"
                      SmallIcon="Images/bold.png"
                      IsCheckable="True"
                      IsChecked="{Binding IsBold, Mode=TwoWay}"/>
```

---

## Common Patterns

### Toolbar Formatting Buttons

```xaml
<StackPanel Orientation="Horizontal">
    <syncfusion:ButtonAdv Label="B" SizeMode="Small" IsCheckable="True"
                          IsChecked="{Binding IsBold, Mode=TwoWay}" Margin="2"/>
    <syncfusion:ButtonAdv Label="I" SizeMode="Small" IsCheckable="True"
                          IsChecked="{Binding IsItalic, Mode=TwoWay}" Margin="2"/>
    <syncfusion:ButtonAdv Label="U" SizeMode="Small" IsCheckable="True"
                          IsChecked="{Binding IsUnderline, Mode=TwoWay}" Margin="2"/>
</StackPanel>
```

### Toggle Sidebar Panel

```xaml
<syncfusion:ButtonAdv x:Name="sidebarToggle"
                      Label="Sidebar"
                      SizeMode="Normal"
                      SmallIcon="Images/sidebar.png"
                      IsCheckable="True"
                      Checked="ShowSidebar"
                      Unchecked="HideSidebar"/>
```

```csharp
private void ShowSidebar(object sender, RoutedEventArgs e) =>
    SidebarColumn.Width = new GridLength(250);

private void HideSidebar(object sender, RoutedEventArgs e) =>
    SidebarColumn.Width = new GridLength(0);
```

### Read Current State in Code

```csharp
bool isActive = (bool)myButton.IsChecked;
// or
if (myButton.IsChecked == true)
{
    // Button is in checked/on state
}
```

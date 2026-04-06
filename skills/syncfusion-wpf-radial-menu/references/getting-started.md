# Getting Started with WPF Radial Menu

This guide covers the basics of setting up and using the Syncfusion WPF Radial Menu (SfRadialMenu) control in your WPF applications.

## Namespace and Assembly

**Namespace:** `Syncfusion.Windows.Controls.Navigation`  
**Assembly:** `Syncfusion.SfRadialMenu.WPF` (in Syncfusion.SfRadialMenu.WPF.dll)

To use the SfRadialMenu control, add the namespace declaration in your XAML:

```xaml
<Window xmlns:navigation="clr-namespace:Syncfusion.Windows.Controls.Navigation;assembly=Syncfusion.SfRadialMenu.Wpf">
```

In code-behind, add the using directive:

```csharp
using Syncfusion.Windows.Controls.Navigation;
```

---

## Basic Implementation

### XAML Implementation

The simplest way to create a radial menu is in XAML by adding `SfRadialMenu` with `SfRadialMenuItem` children:

```xaml
<Window xmlns:navigation="clr-namespace:Syncfusion.Windows.Controls.Navigation;assembly=Syncfusion.SfRadialMenu.Wpf">
    <Grid>
        <navigation:SfRadialMenu>
            <navigation:SfRadialMenuItem Header="Bold"/>
            <navigation:SfRadialMenuItem Header="Cut"/>
            <navigation:SfRadialMenuItem Header="Copy"/>
            <navigation:SfRadialMenuItem Header="Paste"/>
        </navigation:SfRadialMenu>
    </Grid>
</Window>
```

### Code-Behind Implementation

You can also create the radial menu programmatically in code:

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();

SfRadialMenuItem bold = new SfRadialMenuItem() { Header = "Bold" };
SfRadialMenuItem cut = new SfRadialMenuItem() { Header = "Cut" };
SfRadialMenuItem copy = new SfRadialMenuItem() { Header = "Copy" };
SfRadialMenuItem paste = new SfRadialMenuItem() { Header = "Paste" };

radialMenu.Items.Add(bold);
radialMenu.Items.Add(cut);
radialMenu.Items.Add(copy);
radialMenu.Items.Add(paste);

// Add to the visual tree
this.Content = radialMenu;
```

---

## Opening and Closing the Menu

### Show() Method

The `Show()` method is used to programmatically open/display the radial menu at runtime. This is useful when you want to show the menu in response to a button click or other user action.

**XAML:**
```xaml
<Grid>
    <navigation:SfRadialMenu x:Name="radialMenu">
        <navigation:SfRadialMenuItem Header="Bold"/>
        <navigation:SfRadialMenuItem Header="Cut"/>
        <navigation:SfRadialMenuItem Header="Copy"/>
        <navigation:SfRadialMenuItem Header="Paste"/>
    </navigation:SfRadialMenu>
    
    <Button Content="Open Menu" 
            Width="100" 
            Height="30"
            VerticalAlignment="Bottom" 
            HorizontalAlignment="Center"
            Click="OpenButton_Click"/>
</Grid>
```

**Code-Behind:**
```csharp
private void OpenButton_Click(object sender, RoutedEventArgs e)
{
    radialMenu.Show();
}
```

### Close() Method

The `Close()` method is used to programmatically close/hide the radial menu at runtime.

**XAML:**
```xaml
<Grid>
    <navigation:SfRadialMenu x:Name="radialMenu" IsOpen="True">
        <navigation:SfRadialMenuItem Header="Bold"/>
        <navigation:SfRadialMenuItem Header="Cut"/>
        <navigation:SfRadialMenuItem Header="Copy"/>
        <navigation:SfRadialMenuItem Header="Paste"/>
    </navigation:SfRadialMenu>
    
    <Button Content="Close Menu" 
            Width="100" 
            Height="30"
            VerticalAlignment="Bottom" 
            HorizontalAlignment="Center"
            Click="CloseButton_Click"/>
</Grid>
```

**Code-Behind:**
```csharp
private void CloseButton_Click(object sender, RoutedEventArgs e)
{
    radialMenu.Close();
}
```

### IsOpen Property

The `IsOpen` property can be set to `true` to display the radial menu immediately when the application loads:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Cut"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

You can also bind `IsOpen` to a property in your ViewModel for MVVM scenarios:

```xaml
<navigation:SfRadialMenu IsOpen="{Binding IsMenuOpen}">
    <!-- Menu items -->
</navigation:SfRadialMenu>
```

---

## Event Handling

### Opened Event

The `Opened` event is triggered when the radial menu is opened/displayed. Use this to perform actions when the menu becomes visible.

**XAML:**
```xaml
<navigation:SfRadialMenu x:Name="radialMenu" Opened="RadialMenu_Opened">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Cut"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

**Code-Behind:**
```csharp
// Subscribe in XAML or code-behind
radialMenu.Opened += RadialMenu_Opened;

private void RadialMenu_Opened(object sender, EventArgs e)
{
    MessageBox.Show("Radial Menu Opened");
    // Perform initialization or state updates
}
```

### Closed Event

The `Closed` event is triggered when the radial menu is closed/hidden. Use this for cleanup or state updates when the menu is dismissed.

**XAML:**
```xaml
<navigation:SfRadialMenu x:Name="radialMenu" 
                         IsOpen="True" 
                         Closed="RadialMenu_Closed">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Cut"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

**Code-Behind:**
```csharp
// Subscribe in XAML or code-behind
radialMenu.Closed += RadialMenu_Closed;

private void RadialMenu_Closed(object sender, EventArgs e)
{
    MessageBox.Show("Radial Menu Closed");
    // Save state, clear selections, etc.
}
```

### Navigating Event

The `Navigating` event is triggered when the user navigates into a sub-level of the radial menu (drills down into a menu item that has children). The event provides access to the target item and can be cancelled.

**XAML:**
```xaml
<navigation:SfRadialMenu x:Name="radialMenu" 
                         IsOpen="True" 
                         Navigating="RadialMenu_Navigating">
    <navigation:SfRadialMenuItem Header="Bold">
        <navigation:SfRadialMenuItem Header="Bold 1"/>
        <navigation:SfRadialMenuItem Header="Bold 2"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Cut"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

**Code-Behind:**
```csharp
// Subscribe in XAML or code-behind
radialMenu.Navigating += RadialMenu_Navigating;

private void RadialMenu_Navigating(object sender, NavigatingEventArgs e)
{
    // Access the item being navigated into
    SfRadialMenuItem targetItem = e.TargetMenuItem as SfRadialMenuItem;
    string header = targetItem?.Header?.ToString();
    
    MessageBox.Show($"Navigating to: {header}");
    
    // Optionally cancel navigation
    // e.Cancel = true;
}
```

**Use case for cancelling navigation:**
```csharp
private void RadialMenu_Navigating(object sender, NavigatingEventArgs e)
{
    SfRadialMenuItem targetItem = sender as SfRadialMenuItem;
    
    // Prevent navigation to certain items based on condition
    if (targetItem?.Header?.ToString() == "Restricted Item")
    {
        e.Cancel = true;
        MessageBox.Show("You don't have permission to access this menu.");
    }
}
```

---

## Theme Support

The Radial Menu supports various built-in themes for consistent styling with other Syncfusion controls.

### Using SfSkinManager

Apply themes using the `SfSkinManager`:

```csharp
// In your application startup or window constructor
SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

Available themes include:
- MaterialLight
- MaterialDark
- Office2019Colorful
- Office2019Black
- Office2019White
- FluentLight
- FluentDark

### Custom Themes with ThemeStudio

You can create custom themes using Syncfusion's ThemeStudio tool:
1. Visit the ThemeStudio
2. Customize colors and styles
3. Export the theme
4. Apply to your application

---

## Common Patterns and Tips

### Pattern 1: Context Menu on Right-Click

Show the radial menu when the user right-clicks on an element:

```xaml
<TextBox x:Name="textBox" MouseRightButtonDown="TextBox_RightClick">
    <TextBox.ContextMenu>
        <ContextMenu Visibility="Collapsed"/>
    </TextBox.ContextMenu>
</TextBox>

<navigation:SfRadialMenu x:Name="radialMenu">
    <navigation:SfRadialMenuItem Header="Cut"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
private void TextBox_RightClick(object sender, MouseButtonEventArgs e)
{
    radialMenu.Show();
    e.Handled = true;
}
```

### Pattern 2: Toggle Menu with IsOpen Binding

Use two-way binding to toggle the menu from a ViewModel:

```xaml
<navigation:SfRadialMenu IsOpen="{Binding IsMenuOpen, Mode=TwoWay}">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>

<Button Content="Toggle Menu" Command="{Binding ToggleMenuCommand}"/>
```

```csharp
// ViewModel
public class MainViewModel : INotifyPropertyChanged
{
    private bool _isMenuOpen;
    public bool IsMenuOpen
    {
        get => _isMenuOpen;
        set
        {
            _isMenuOpen = value;
            OnPropertyChanged(nameof(IsMenuOpen));
        }
    }
    
    public ICommand ToggleMenuCommand { get; }
    
    public MainViewModel()
    {
        ToggleMenuCommand = new RelayCommand(() => IsMenuOpen = !IsMenuOpen);
    }
}
```

### Pattern 3: Hierarchical Navigation

Create nested menus with sub-items:

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Format">
        <navigation:SfRadialMenuItem Header="Bold"/>
        <navigation:SfRadialMenuItem Header="Italic"/>
        <navigation:SfRadialMenuItem Header="Underline"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Edit">
        <navigation:SfRadialMenuItem Header="Cut"/>
        <navigation:SfRadialMenuItem Header="Copy"/>
        <navigation:SfRadialMenuItem Header="Paste"/>
    </navigation:SfRadialMenuItem>
</navigation:SfRadialMenu>
```

---

## Troubleshooting

### Issue: Menu Doesn't Appear

**Problem:** The radial menu is added but doesn't show up.

**Solution:** Ensure `IsOpen` is set to `true` or call `Show()` method:
```csharp
radialMenu.IsOpen = true;
// or
radialMenu.Show();
```

### Issue: Assembly Not Found

**Problem:** Compiler error about missing assembly reference.

**Solution:** Add reference to `Syncfusion.SfRadialMenu.WPF.dll` in your project and ensure the namespace declaration is correct.

### Issue: Events Not Firing

**Problem:** Opened, Closed, or Navigating events are not being triggered.

**Solution:** 
1. Verify event handler is properly subscribed
2. Check that the method signature matches the event delegate
3. Ensure the control is in the visual tree

```csharp
// Correct event signature
private void RadialMenu_Opened(object sender, EventArgs e) { }
private void RadialMenu_Closed(object sender, EventArgs e) { }
private void RadialMenu_Navigating(object sender, NavigatingEventArgs e) { }
```

### Issue: Menu Appears in Wrong Position

**Problem:** The radial menu doesn't appear at the expected location.

**Solution:** The radial menu appears at its location in the visual tree. To position it relative to a specific point, place it in an appropriate container (Grid, Canvas) with proper layout properties.

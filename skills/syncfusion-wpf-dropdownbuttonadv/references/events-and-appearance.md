# Events and Appearance

## Table of Contents
- [Dropdown Popup Events](#dropdown-popup-events)
- [DropDownMenuItem Events](#dropdownmenuitem-events)
- [Cancelling Popup Open/Close](#cancelling-popup-openclose)
- [Editing Control Templates](#editing-control-templates)
- [Themes with SfSkinManager](#themes-with-sfskinmanager)
- [Custom Themes with ThemeStudio](#custom-themes-with-themestudio)
- [Gotchas](#gotchas)

---

## Dropdown Popup Events

Four events fire around the popup lifecycle:

| Event | Handler Type | Fires |
|---|---|---|
| `DropDownOpening` | `CancelEventHandler` | Before popup opens (cancellable) |
| `DropDownOpened` | `RoutedEventHandler` | After popup opens |
| `DropDownClosing` | `CancelEventHandler` | Before popup closes (cancellable) |
| `DropDownClosed` | `RoutedEventHandler` | After popup closes |

```xaml
<syncfusion:DropDownButtonAdv x:Name="dropdownButton"
                               DropDownOpening="dropdownButton_DropDownOpening"
                               DropDownOpened="dropdownButton_DropDownOpened"
                               DropDownClosing="dropdownButton_DropDownClosing"
                               DropDownClosed="dropdownButton_DropDownClosed"/>
```

```csharp
DropDownButtonAdv dropdownButton = new DropDownButtonAdv();
dropdownButton.DropDownOpening += dropdownButton_DropDownOpening;
dropdownButton.DropDownOpened  += dropdownButton_DropDownOpened;
dropdownButton.DropDownClosing += dropdownButton_DropDownClosing;
dropdownButton.DropDownClosed  += dropdownButton_DropDownClosed;

private void dropdownButton_DropDownOpening(object sender, CancelEventArgs e) { }
private void dropdownButton_DropDownOpened(object sender, RoutedEventArgs e)  { }
private void dropdownButton_DropDownClosing(object sender, CancelEventArgs e) { }
private void dropdownButton_DropDownClosed(object sender, RoutedEventArgs e)  { }
```

---

## DropDownMenuItem Events

### Click Event

Fires when the user clicks a menu item:

```xaml
<syncfusion:DropDownMenuItem x:Name="indiaItem"
                              Header="India"
                              HorizontalAlignment="Left"
                              Click="indiaItem_Click"/>
```

```csharp
DropDownMenuItem item = new DropDownMenuItem();
item.Click += indiaItem_Click;

private void indiaItem_Click(object sender, RoutedEventArgs e)
{
    var item = sender as DropDownMenuItem;
    MessageBox.Show($"{item.Header} clicked");
}
```

### IsCheckedChanged Event

Fires when a checkable item's checked state changes. Only fires when `IsCheckable="True"`:

```xaml
<syncfusion:DropDownMenuItem x:Name="indiaItem"
                              Header="India"
                              IsCheckable="True"
                              IsCheckedChanged="indiaItem_IsCheckedChanged"/>
```

```csharp
DropDownMenuItem item = new DropDownMenuItem();
item.IsCheckable = true;
item.IsCheckedChanged += indiaItem_IsCheckedChanged;

private void indiaItem_IsCheckedChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    bool isChecked = (bool)e.NewValue;
    // React to check state change
}
```

---

## Cancelling Popup Open/Close

Use `CancelEventArgs.Cancel = true` to prevent the popup from opening or closing:

```csharp
// Prevent popup from opening under certain conditions
private void dropdownButton_DropDownOpening(object sender, CancelEventArgs e)
{
    if (!IsUserAuthorized())
    {
        e.Cancel = true; // Popup will not open
    }
}

// Prevent popup from closing (e.g., during validation)
private void dropdownButton_DropDownClosing(object sender, CancelEventArgs e)
{
    if (IsValidationPending())
    {
        e.Cancel = true; // Popup stays open
    }
}
```

---

## Editing Control Templates

To customize the visual structure of `DropDownButtonAdv`, edit its `ControlTemplate`.

### In Expression Blend

1. Select the `DropDownButtonAdv` control
2. Right-click → **Edit Template**
3. Choose **Edit a Copy...** (modifies default style) or **Create Empty...** (blank template)
4. The `Create ControlTemplate Resource` dialog lets you name the template and choose its location (Window, Page, or Application resources)
5. The generated XAML appears in the Resources section — edit in XAML view or in Blend's visual editor

### In Visual Studio

1. Select `DropDownButtonAdv` in designer view
2. Right-click → **Edit Template** → **Edit a Copy...**
3. Choose resource dictionary location in the dialog
4. Edit the generated XAML in the resource section

> **Tip:** Always use "Edit a Copy..." rather than "Create Empty..." to start from the existing template structure and avoid missing required template parts.

---

## Themes with SfSkinManager

Apply built-in Syncfusion themes via `SfSkinManager`:

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<syncfusion:DropDownButtonAdv
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentLight}"
    Label="Country"/>
```

To apply a theme to all controls in the window:

```xaml
<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
```

**Available built-in themes:**

| Theme Name | Style |
|---|---|
| `FluentLight` | Windows 11 Fluent light |
| `FluentDark` | Windows 11 Fluent dark |
| `MaterialLight` | Material Design light |
| `MaterialDark` | Material Design dark |
| `Office2019Colorful` | Office 2019 colorful |
| `Office2019Black` | Office 2019 black |
| `Windows11Light` | Windows 11 light |
| `Windows11Dark` | Windows 11 dark |
| `SystemTheme` | Matches OS system theme |

---

## Custom Themes with ThemeStudio

To create a custom branded theme:

1. Open [Syncfusion ThemeStudio](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)
2. Select a base theme and customize colors, fonts, and borders
3. Export the generated resource dictionary
4. Reference it in `App.xaml` or individual windows

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `IsCheckedChanged` doesn't fire | `IsCheckable` is `false` | Set `IsCheckable="True"` on the item |
| Popup still opens after `Cancel = true` | Handler attached to `DropDownOpened` instead of `DropDownOpening` | Use `DropDownOpening` (with `CancelEventArgs`) to cancel |
| Theme has no effect | `SfSkinManager` assembly not referenced | Add `Syncfusion.SfSkinManager.WPF` to project references |
| Template edit breaks control | Using "Create Empty..." removes required template parts | Use "Edit a Copy..." to preserve the working template structure |
| `Click` event fires but command does not | Both `Click` and `Command` attached; event conflicts | Prefer one approach — either `Click` (code-behind) or `Command` (MVVM) |

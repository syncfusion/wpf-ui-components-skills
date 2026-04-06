# Customization and Theming

This guide covers common customization patterns and theming for `SfSpellChecker`. It removes repetitive patterns and presents representative examples for customization.

## SpellCheckCompleted Event (Representative Patterns)

The `SpellCheckCompleted` event fires when spell checking finishes. Use `ShowMessageBox` on `SpellCheckCompletedEventArgs` to control the default behavior and implement custom post-check actions.

### Event Overview

```csharp
public event EventHandler<SpellCheckCompletedEventArgs> SpellCheckCompleted;
```

### Basic Example (suppress default message and show custom notification)

```csharp
spellChecker.SpellCheckCompleted += SpellChecker_SpellCheckCompleted;

private void SpellChecker_SpellCheckCompleted(object sender, EventArgs e)
{
    var args = e as SpellCheckCompletedEventArgs;
    if (args != null)
    {
        // Suppress default message box
        args.ShowMessageBox = false;

        // Example: update status bar
        statusBarText.Text = $"Spell check completed at {DateTime.Now:t}";
    }
}
```

### Non-Intrusive Notification (toast / status)

Use a non-blocking notification if you prefer not to interrupt the user flow.

```csharp
private void SpellChecker_SpellCheckCompleted_StatusBar(object sender, EventArgs e)
{
    var args = e as SpellCheckCompletedEventArgs;
    if (args != null)
    {
        args.ShowMessageBox = false;
        statusBarText.Text = "Spell check completed";
        // Optional: auto-hide logic
    }
}
```

### Advanced: Auto-save or Workflow Integration

Perform lightweight workflow actions on completion (auto-save, mark document reviewed):

```csharp
private void SpellChecker_SpellCheckCompleted_Workflow(object sender, EventArgs e)
{
    var args = e as SpellCheckCompletedEventArgs;
    if (args != null)
    {
        args.ShowMessageBox = false;
        SaveDocument();
        document.IsSpellChecked = true;
    }
}
```

## Theme Support (Concise)

Apply Syncfusion themes via `SfSkinManager`. Common themes include `MaterialLight`, `MaterialDark`, `FluentLight`, `FluentDark`, and Office themes.

```csharp
using Syncfusion.SfSkinManager;

public MainWindow()
{
    InitializeComponent();
    SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
}
```

Apply to a specific control:

```csharp
SfSkinManager.SetTheme(spellChecker, new Theme("FluentDark"));
```

### Dynamic Theme Switching (example)

```csharp
private void ThemeSelector_Changed(object sender, SelectionChangedEventArgs e)
{
    if (sender is ComboBox combo && combo.SelectedItem is ComboBoxItem item)
    {
        SfSkinManager.SetTheme(this, new Theme(item.Tag.ToString()));
    }
}
```

## Visual Customization

For error highlighting color or dialog appearance, prefer creating a custom theme with ThemeStudio or override styles at the editor control level.

## Best Practices (short)

- Use non-intrusive notifications for background checks.
- Save user theme preferences and reapply on startup.
- Test themes for accessibility and contrast.

## Troubleshooting (short)

- If `SpellCheckCompleted` doesn't fire, verify the handler is attached and `EnableSpellCheck` is `true`.
- If a theme doesn't apply, confirm the `SfSkinManager` assembly reference and exact theme name.

## Related Topics

- `spell-check-methods.md`
- `dictionary-support.md`
- `getting-started.md`

## Additional Resources

- SfSkinManager Documentation: https://help.syncfusion.com/wpf/themes/skin-manager
- ThemeStudio: https://help.syncfusion.com/wpf/themes/theme-studio


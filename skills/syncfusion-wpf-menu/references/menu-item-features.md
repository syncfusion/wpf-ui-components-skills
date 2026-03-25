# Menu Item Features

## Table of Contents
- [Icons](#icons)
- [InputGestureText (Keyboard Shortcut Labels)](#inputgesturetext-keyboard-shortcut-labels)
- [MenuItemSeparator](#menuitemseparator)
- [CheckBox Support](#checkbox-support)
- [RadioButton Support](#radiobutton-support)
- [Gotchas](#gotchas)

---

## Icons

Display an image on the left side of a `MenuItemAdv` using the `Icon` property.

### XAML

```xaml
<syncfusion:MenuAdv Height="25" VerticalAlignment="Top">
    <syncfusion:MenuItemAdv Header="File">
        <syncfusion:MenuItemAdv Header="New">
            <syncfusion:MenuItemAdv.Icon>
                <Image Source="Assets/NewIcon.png" Width="15" Height="15"/>
            </syncfusion:MenuItemAdv.Icon>
        </syncfusion:MenuItemAdv>
        <syncfusion:MenuItemAdv Header="Open">
            <syncfusion:MenuItemAdv.Icon>
                <Image Source="Assets/OpenIcon.png" Width="15" Height="15"/>
            </syncfusion:MenuItemAdv.Icon>
        </syncfusion:MenuItemAdv>
        <syncfusion:MenuItemAdv Header="Save">
            <syncfusion:MenuItemAdv.Icon>
                <Image Source="Assets/SaveIcon.png" Width="15" Height="15"/>
            </syncfusion:MenuItemAdv.Icon>
        </syncfusion:MenuItemAdv>
    </syncfusion:MenuItemAdv>
</syncfusion:MenuAdv>
```

### C#

```csharp
MenuItemAdv newItem = new MenuItemAdv { Header = "New" };
newItem.Icon = new Image
{
    Source = new BitmapImage(new Uri("Assets/NewIcon.png", UriKind.Relative)),
    Width  = 15,
    Height = 15
};
```

**`Icon` accepts any `object`** — `Image`, `Path`, `Ellipse`, or any UIElement.

---

## InputGestureText (Keyboard Shortcut Labels)

`InputGestureText` displays a shortcut key label (e.g., `"Ctrl+N"`) on the right side of the menu item header. It is **display-only** — it does not register the keyboard shortcut itself.

```xaml
<syncfusion:MenuAdv Height="25" VerticalAlignment="Top">
    <syncfusion:MenuItemAdv Header="File">
        <syncfusion:MenuItemAdv Header="New"   InputGestureText="Ctrl+N"/>
        <syncfusion:MenuItemAdv Header="Cut"   InputGestureText="Ctrl+X"/>
        <syncfusion:MenuItemAdv Header="Copy"  InputGestureText="Ctrl+C"/>
        <syncfusion:MenuItemAdv Header="Paste" InputGestureText="Ctrl+V"/>
        <syncfusion:MenuItemAdv Header="Undo"  InputGestureText="Ctrl+Z"/>
        <syncfusion:MenuItemAdv Header="Redo"  InputGestureText="Ctrl+Y"/>
    </syncfusion:MenuItemAdv>
</syncfusion:MenuAdv>
```

> **Note:** To also register the actual keyboard shortcut, combine with `Command="{x:Static ApplicationCommands.Copy}"` — WPF will auto-display the shortcut text from the built-in command.

---

## MenuItemSeparator

`MenuItemSeparator` renders a horizontal divider line between groups of menu items.

```xaml
<syncfusion:MenuItemAdv Header="File">
    <syncfusion:MenuItemAdv Header="New"/>
    <syncfusion:MenuItemAdv Header="Open"/>
    <syncfusion:MenuItemSeparator/>
    <syncfusion:MenuItemAdv Header="Save"/>
    <syncfusion:MenuItemAdv Header="Save As"/>
    <syncfusion:MenuItemSeparator/>
    <syncfusion:MenuItemAdv Header="Exit"/>
</syncfusion:MenuItemAdv>
```

```csharp
fileMenu.Items.Add(new MenuItemAdv { Header = "New" });
fileMenu.Items.Add(new MenuItemSeparator());
fileMenu.Items.Add(new MenuItemAdv { Header = "Exit" });
```

---

## CheckBox Support

Make a `MenuItemAdv` toggleable like a checkbox by setting `IsCheckable="True"` and `CheckIconType="CheckBox"`.

```xaml
<syncfusion:MenuItemAdv Header="View">
    <syncfusion:MenuItemAdv Header="Immediate"
                             IsCheckable="True"
                             CheckIconType="CheckBox"
                             IsChecked="True"/>
    <syncfusion:MenuItemAdv Header="Call Stack"
                             IsCheckable="True"
                             CheckIconType="CheckBox"
                             IsChecked="False"/>
    <syncfusion:MenuItemAdv Header="Output"
                             IsCheckable="True"
                             CheckIconType="CheckBox"
                             IsChecked="False"/>
</syncfusion:MenuItemAdv>
```

**Properties:**

| Property | Type | Description |
|---|---|---|
| `IsCheckable` | `bool` | Enables check/uncheck behavior |
| `CheckIconType` | `CheckIconType` enum | `CheckBox` (default) or `RadioButton` |
| `IsChecked` | `bool` | Current checked state |

---

## RadioButton Support

Group menu items so only one can be selected at a time using `CheckIconType="RadioButton"` and a shared `GroupName`.

```xaml
<syncfusion:MenuItemAdv Header="View">
    <syncfusion:MenuItemSeparator/>
    <syncfusion:MenuItemAdv Header="Solution Explorer"
                             IsCheckable="True"
                             CheckIconType="RadioButton"
                             GroupName="panels"
                             IsChecked="True"/>
    <syncfusion:MenuItemAdv Header="Team Explorer"
                             IsCheckable="True"
                             CheckIconType="RadioButton"
                             GroupName="panels"
                             IsChecked="False"/>
    <syncfusion:MenuItemAdv Header="Server Explorer"
                             IsCheckable="True"
                             CheckIconType="RadioButton"
                             GroupName="panels"
                             IsChecked="False"/>
</syncfusion:MenuItemAdv>
```

**Key rule:** All radio button items in a mutually exclusive group must share the **same `GroupName`** value.

### Combined Example (CheckBox + RadioButton)

```xaml
<syncfusion:MenuItemAdv Header="View">
    <!-- Checkboxes — each independently toggle -->
    <syncfusion:MenuItemAdv Header="Immediate"  IsCheckable="True" CheckIconType="CheckBox" IsChecked="True"/>
    <syncfusion:MenuItemAdv Header="Call Stack" IsCheckable="True" CheckIconType="CheckBox" IsChecked="False"/>
    <syncfusion:MenuItemSeparator/>
    <!-- Radio buttons — mutually exclusive within "panels" group -->
    <syncfusion:MenuItemAdv Header="Solution Explorer" IsCheckable="True" CheckIconType="RadioButton" GroupName="panels" IsChecked="True"/>
    <syncfusion:MenuItemAdv Header="Team Explorer"     IsCheckable="True" CheckIconType="RadioButton" GroupName="panels" IsChecked="False"/>
    <syncfusion:MenuItemAdv Header="Server Explorer"   IsCheckable="True" CheckIconType="RadioButton" GroupName="panels" IsChecked="False"/>
</syncfusion:MenuItemAdv>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `InputGestureText` does nothing | It's label-only, not a key binding | Register key binding separately via `InputBindings` or `Command` |
| Multiple radio items checked simultaneously | Different `GroupName` values | Ensure all items in same group share **identical** `GroupName` |
| Icon not showing | Image build action not set | Set image file `Build Action = Resource` in project |
| `IsChecked` binding not updating ViewModel | One-way binding | Use `IsChecked="{Binding IsChecked, Mode=TwoWay}"` |

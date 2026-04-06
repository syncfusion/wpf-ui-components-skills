# DateTime Editing — DateTimeEdit (WPF)

## Table of Contents
- [Mask Editing (Default)](#mask-editing-default)
- [Mask Validation Behavior](#mask-validation-behavior)
- [Keyboard Navigation Between Fields](#keyboard-navigation-between-fields)
- [Free-Form Text Editing](#free-form-text-editing)
- [Mouse Wheel Editing](#mouse-wheel-editing)
- [Up/Down Repeat Buttons](#updown-repeat-buttons)
- [Custom Repeat Button UI](#custom-repeat-button-ui)
- [Alpha Key Month Navigation](#alpha-key-month-navigation)
- [BackSpace and Delete Key Editing](#backspace-and-delete-key-editing)
- [Focus Behavior](#focus-behavior)
- [Value Changed Notification](#value-changed-notification)

---

## Mask Editing (Default)

Mask editing is the **default mode**. The date is split into separate fields (day, month, year, hour, minute, second). Each field accepts only valid numeric input — invalid values are auto-corrected or rejected.

```xml
<!-- Mask editing is on by default — no extra property needed -->
<syncfusion:DateTimeEdit Height="25" Width="200" Name="dateTimeEdit"/>
```

> **When to use mask mode:** When you need strict date validation with no chance of an invalid date being entered. Users can't type "13" for month or "32" for day.

---

## Mask Validation Behavior

| Behavior | Detail |
|---|---|
| Auto-navigation after fill | Focus moves to next field automatically when current field is complete |
| Separator shortcut | Pressing the separator character (e.g., `/`) moves focus to next field |
| Immediate correction | Invalid values are corrected or reset immediately on input |
| Leap year awareness | Entering Feb 29 auto-validates and adjusts the year to the nearest leap year |
| Day field | Not independently editable in some patterns — changes via month/year context |

---

## Keyboard Navigation Between Fields

| Key | Action |
|---|---|
| `Up` | Increase selected field value by 1 |
| `Down` | Decrease selected field value by 1 |
| `Left` | Move to previous field (toward start) |
| `Right` | Move to next field (toward end) |
| `Ctrl + Up` | Move calendar view forward (month → year → decade) |
| `Ctrl + Down` | Move calendar view backward (decade → year → month) |
| `PageUp` | Move to previous month / year in current view |
| `PageDown` | Move to next month / year in current view |
| `Home` | Select first date/month/year in current view |
| `End` | Select last date/month/year in current view |

> Click a specific field with the mouse to focus it directly, bypassing `OnFocusBehavior` setting.

---

## Free-Form Text Editing

Set `CanEdit="True"` to allow typing the date like a normal TextBox. Validation occurs on **Enter** or **focus loss** — if invalid, reverts to the previous date.

```xml
<syncfusion:DateTimeEdit CanEdit="True"
                         Height="25" Width="200"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.CanEdit = true;
```

> **When to use free-form mode:** When users need to paste dates or type rapidly. Trade-off: allows invalid intermediate states until validated on Enter/blur.

---

## Mouse Wheel Editing

By default, scrolling the mouse wheel over a focused date field increments/decrements that field. Disable if this causes accidental changes:

```xml
<!-- Enabled (default) -->
<syncfusion:DateTimeEdit EnableMouseWheelEdit="True" Name="dateTimeEdit"/>

<!-- Disabled -->
<syncfusion:DateTimeEdit EnableMouseWheelEdit="False" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.EnableMouseWheelEdit = false;
```

---

## Up/Down Repeat Buttons

Show spinner (up/down) buttons next to the date field. Hidden by default.

```xml
<!-- Show and enable repeat buttons -->
<syncfusion:DateTimeEdit IsVisibleRepeatButton="True"
                         IsEnabledRepeatButton="True"
                         Height="25" Width="200"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.IsVisibleRepeatButton = true;
dateTimeEdit.IsEnabledRepeatButton = true;
```

**Show only spinner (hide calendar dropdown button):**
```xml
<syncfusion:DateTimeEdit IsVisibleRepeatButton="True"
                         IsEnabledRepeatButton="True"
                         IsButtonPopUpEnabled="False"
                         Name="dateTimeEdit"/>
```

**Disable spinner without hiding it:**
```xml
<syncfusion:DateTimeEdit IsVisibleRepeatButton="True"
                         IsEnabledRepeatButton="False"
                         Name="dateTimeEdit"/>
```

---

## Custom Repeat Button UI

Replace the default spinner arrows with custom content using `UpRepeatButtonTemplate` and `DownRepeatButtonTemplate`:

```xml
<syncfusion:DateTimeEdit IsVisibleRepeatButton="True" Name="dateTimeEdit">
    <syncfusion:DateTimeEdit.UpRepeatButtonTemplate>
        <ControlTemplate>
            <TextBlock Text="▲"
                       FontSize="12" FontWeight="Bold"
                       Foreground="White"
                       TextAlignment="Center"
                       Background="#0078D4"/>
        </ControlTemplate>
    </syncfusion:DateTimeEdit.UpRepeatButtonTemplate>
    <syncfusion:DateTimeEdit.DownRepeatButtonTemplate>
        <ControlTemplate>
            <TextBlock Text="▼"
                       FontSize="12" FontWeight="Bold"
                       Foreground="White"
                       TextAlignment="Center"
                       Background="#D83B01"/>
        </ControlTemplate>
    </syncfusion:DateTimeEdit.DownRepeatButtonTemplate>
</syncfusion:DateTimeEdit>
```

**Change spinner background and margins:**
```xml
<syncfusion:DateTimeEdit RepeatButtonBackground="SteelBlue"
                         UpRepeatButtonMargin="1"
                         DownRepeatButtonMargin="1"
                         IsVisibleRepeatButton="True"
                         Name="dateTimeEdit"/>
```

---

## Alpha Key Month Navigation

When the month field is displayed in abbreviated text form (e.g., `LongDate` pattern), allow users to navigate months by pressing the initial letter:

```xml
<syncfusion:DateTimeEdit EnableAlphaKeyNavigation="True"
                         CanEdit="False"
                         Pattern="LongDate"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.EnableAlphaKeyNavigation = true;
dateTimeEdit.CanEdit = false;
dateTimeEdit.Pattern = DateTimePattern.LongDate;
```

**Behavior:** Pressing `J` → January; pressing `J` again → June; pressing `J` again → July.

> **Requires:** `CanEdit="False"` — alpha navigation only works in mask mode.

---

## BackSpace and Delete Key Editing

By default, pressing BackSpace or Delete does nothing in mask mode — the field value is overwritten in-place. Enable these keys to allow deletion followed by new input:

```xml
<syncfusion:DateTimeEdit EnableBackspaceKey="True"
                         EnableDeleteKey="True"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.EnableBackspaceKey = true;
dateTimeEdit.EnableDeleteKey    = true;
```

---

## Focus Behavior

**Control which field is selected when the control receives focus:**

```xml
<!-- Default: cursor on first field (e.g., month in MM/dd/yyyy) -->
<syncfusion:DateTimeEdit OnFocusBehavior="CursorOnFirstCharacter" Name="dateTimeEdit"/>

<!-- Cursor at end: last field selected on focus -->
<syncfusion:DateTimeEdit OnFocusBehavior="CursorAtEnd" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.OnFocusBehavior = OnFocusBehavior.CursorAtEnd;
```

> `OnFocusBehavior` only applies when the control receives focus programmatically or via Tab. Clicking a specific field directly always focuses that field.

**Prevent automatic field-to-field advancement:**

```xml
<syncfusion:DateTimeEdit AutoForwarding="False" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.AutoForwarding = false;
```

---

## Value Changed Notification

```xml
<syncfusion:DateTimeEdit DateTimeChanged="DateTimeEdit_DateTimeChanged"
                         Name="dateTimeEdit"/>
```

```csharp
private void DateTimeEdit_DateTimeChanged(DependencyObject d,
                                          DependencyPropertyChangedEventArgs e)
{
    var oldDate = e.OldValue;
    var newDate = e.NewValue;
    // react to selection change
}
```

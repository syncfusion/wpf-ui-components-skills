# Appearance — DateTimeEdit (WPF)

Customize the visual style of the DateTimeEdit control using brushes, flow direction, repeat button styling, and SfSkinManager themes.

---

## Foreground Color

Change the text color of the date displayed in the input field:

```xml
<syncfusion:DateTimeEdit Foreground="DarkBlue" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.Foreground = Brushes.DarkBlue;
```

Default: `Black`

---

## Background and Selection Brush

```xml
<syncfusion:DateTimeEdit Background="LightYellow"
                         SelectionBrush="OrangeRed"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.Background    = Brushes.LightYellow;
dateTimeEdit.SelectionBrush = Brushes.OrangeRed;
```

| Property | Default | Purpose |
|---|---|---|
| `Background` | `White` | Control background color |
| `SelectionBrush` | `RoyalBlue` | Highlight color when a date field is selected |

---

## Focus Border Color

Change the border color that appears when the control has keyboard focus:

```xml
<syncfusion:DateTimeEdit FocusedBorderBrush="MediumVioletRed" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.FocusedBorderBrush = Brushes.MediumVioletRed;
```

Default: `MediumAquamarine`

---

## Flow Direction (RTL)

For right-to-left languages (Arabic, Hebrew, etc.):

```xml
<syncfusion:DateTimeEdit FlowDirection="RightToLeft" Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.FlowDirection = FlowDirection.RightToLeft;
```

Default: `LeftToRight`

> Also set `CultureInfo` (e.g., `ar-SA`) to get locale-appropriate date ordering.

---

## Repeat Button Background and Margins

Customize the spinner button appearance:

```xml
<syncfusion:DateTimeEdit RepeatButtonBackground="SteelBlue"
                         UpRepeatButtonMargin="2"
                         DownRepeatButtonMargin="2"
                         IsVisibleRepeatButton="True"
                         Name="dateTimeEdit"/>
```

```csharp
dateTimeEdit.RepeatButtonBackground  = Brushes.SteelBlue;
dateTimeEdit.UpRepeatButtonMargin   = new Thickness(2);
dateTimeEdit.DownRepeatButtonMargin = new Thickness(2);
dateTimeEdit.IsVisibleRepeatButton  = true;
```

| Property | Default | Purpose |
|---|---|---|
| `RepeatButtonBackground` | `Gainsboro` | Background brush for both spinner buttons |
| `UpRepeatButtonMargin` | `0,0,0,0` | Margin around the Up button |
| `DownRepeatButtonMargin` | `0,0,0,0` | Margin around the Down button |

---

## SfSkinManager Themes

Apply a built-in visual theme to the control:

```csharp
using Syncfusion.SfSkinManager;

// After InitializeComponent()
SfSkinManager.SetVisualStyle(dateTimeEdit, VisualStyles.FluentLight);
```

**Available Visual Styles:**

| Style | Appearance |
|---|---|
| `FluentLight` | Microsoft Fluent light |
| `FluentDark` | Microsoft Fluent dark |
| `MaterialLight` | Google Material light |
| `MaterialDark` | Google Material dark |
| `MaterialLightBlue` | Material with blue accent |
| `Windows11Light` | Windows 11 light |
| `Windows11Dark` | Windows 11 dark |
| `Office2019Colorful` | Office 2019 colorful |
| `Office2019Black` | Office 2019 black |
| `Office2019White` | Office 2019 white |
| `SystemTheme` | Follows OS theme |

**Apply theme to entire window:**
```csharp
SfSkinManager.SetVisualStyle(this, VisualStyles.Windows11Light);
```

---

## Custom Theme via ThemeStudio

1. Open **ThemeStudio** (Syncfusion tool)
2. Select `DateTimeEdit` control
3. Modify colors, fonts, and control part styles
4. Export the theme
5. Import into the project and apply via `SfSkinManager`

See: [Syncfusion ThemeStudio Guide](https://help.syncfusion.com/wpf/themes/theme-studio)

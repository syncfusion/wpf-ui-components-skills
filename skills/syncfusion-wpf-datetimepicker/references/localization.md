# Localization — DateTimeEdit (WPF)

Translate DateTimeEdit UI strings (Today button, None button) into other languages using .NET resource files.

> **Note:** `CultureInfo` changes the date **format** and field language — it does **not** translate button labels. Use resource files for label translation.

---

## How It Works

DateTimeEdit reads localized strings from a `.resx` resource file named `Syncfusion.Shared.Wpf.{culture}.resx`. When `CurrentUICulture` matches a culture with a matching resource file, button labels are automatically translated.

---

## Step 1: Set the Application Culture

Set `CurrentUICulture` **before** `InitializeComponent()`:

```csharp
public MainWindow()
{
    // Set to French BEFORE InitializeComponent
    System.Threading.Thread.CurrentThread.CurrentUICulture =
        new System.Globalization.CultureInfo("fr-FR");

    InitializeComponent();
}
```

> Set the culture **before** `InitializeComponent()` so the DateTimeEdit control can read localized strings during initialization.

---

## Step 2: Create the Resources Folder

Add a folder named `Resources` to your WPF project root.

---

## Step 3: Add the Resource File

1. Right-click `Resources` folder → **Add** → **New Item**
2. Select **Resource File**
3. Name it: `Syncfusion.Shared.Wpf.{culture-code}.resx`

| Culture | File Name |
|---|---|
| French | `Syncfusion.Shared.Wpf.fr-FR.resx` |
| German | `Syncfusion.Shared.Wpf.de-DE.resx` |
| Spanish | `Syncfusion.Shared.Wpf.es-ES.resx` |
| Arabic | `Syncfusion.Shared.Wpf.ar-SA.resx` |
| Japanese | `Syncfusion.Shared.Wpf.ja-JP.resx` |

---

## Step 4: Add Name/Value Pairs

Open the `.resx` file in the Resource Designer and add the localized strings:

| Name | English | French Example |
|---|---|---|
| `DateTimeEdit_Today` | Today | Aujourd'hui |
| `DateTimeEdit_None` | None | Aucun |

---

## Complete Example (French)

```csharp
// MainWindow.xaml.cs
public MainWindow()
{
    // Set culture BEFORE InitializeComponent
    System.Threading.Thread.CurrentThread.CurrentUICulture =
        new System.Globalization.CultureInfo("fr-FR");

    InitializeComponent();
}
```

Resource file: `Resources/Syncfusion.Shared.Wpf.fr-FR.resx`

| Name | Value |
|---|---|
| `DateTimeEdit_Today` | Aujourd'hui |
| `DateTimeEdit_None` | Aucun |

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Buttons still show English | Culture set after control renders | Set `CurrentUICulture` before `InitializeComponent()` or before loading the window |
| Resource file not picked up | Wrong file name | Must match exactly: `Syncfusion.Shared.Wpf.{culture}.resx` (note: `Wpf` not `WPF`) |
| Resource not applied | Not embedded | Set **Build Action = Embedded Resource** on the `.resx` file |
| Date format still wrong | `CultureInfo` only affects format if set on the control | Set `CultureInfo` on the `DateTimeEdit` in XAML or code-behind |

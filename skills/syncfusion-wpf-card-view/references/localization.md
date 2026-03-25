# Localization — CardView (WPF)

Translate CardView UI strings (e.g., "Clear Filter" button label) into other languages using .NET resource files.

---

## How It Works

CardView reads localized strings from a `.resx` resource file named `Syncfusion.Tools.Wpf.{culture}.resx`. When `CurrentUICulture` matches a culture that has a resource file, CardView automatically uses the translated strings.

---

## Step 1: Set the Application Culture

Set `CurrentUICulture` **after** `InitializeComponent()`:

```csharp
public MainWindow()
{
    InitializeComponent();

    // Change to French
    System.Threading.Thread.CurrentThread.CurrentUICulture =
        new System.Globalization.CultureInfo("fr-FR");
}
```

> Set the culture **before** the CardView is loaded/rendered for it to take effect.

---

## Step 2: Create the Resources Folder

Add a folder named `Resources` to your WPF project root.

---

## Step 3: Add the Resource File

1. Right-click the `Resources` folder → **Add** → **New Item**
2. Select **Resource File**
3. Name it: `Syncfusion.Tools.Wpf.{culture-code}.resx`

| Culture | File Name |
|---|---|
| French | `Syncfusion.Tools.Wpf.fr-FR.resx` |
| German | `Syncfusion.Tools.Wpf.de-DE.resx` |
| Spanish | `Syncfusion.Tools.Wpf.es-ES.resx` |
| Japanese | `Syncfusion.Tools.Wpf.ja-JP.resx` |
| Arabic | `Syncfusion.Tools.Wpf.ar-SA.resx` |

---

## Step 4: Add Name/Value Pairs

Open the `.resx` file in the Resource Designer and add translations:

| Name | Value (French example) |
|---|---|
| `CardView_ClearFilter` | `Effacer le filtre` |

> Check the Syncfusion documentation for the full list of localizable string keys for the CardView control.

---

## Complete Example (French)

```csharp
// MainWindow.xaml.cs
public MainWindow()
{
    InitializeComponent();

    System.Threading.Thread.CurrentThread.CurrentUICulture =
        new System.Globalization.CultureInfo("fr-FR");
}
```

Resource file: `Resources/Syncfusion.Tools.Wpf.fr-FR.resx`

| Name | Value |
|---|---|
| `CardView_ClearFilter` | `Effacer le filtre` |

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Strings still in English | Culture set after CardView renders | Set `CurrentUICulture` before `InitializeComponent()` or restart |
| Resource file not found | Wrong file name | Must match exactly: `Syncfusion.Tools.Wpf.{culture}.resx` |
| Resource not applied | File not in `Resources` folder or not included in build | Set **Build Action = Embedded Resource** on the `.resx` file |
| RTL layout for Arabic | Language only sets strings, not layout direction | Also set `FlowDirection="RightToLeft"` on the `CardView` |

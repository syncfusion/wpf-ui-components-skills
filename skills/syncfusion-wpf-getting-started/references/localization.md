# Localization

## Table of Contents
- [Overview](#overview)
- [How Localization Works](#how-localization-works)
- [Changing Application Culture](#changing-application-culture)
- [Creating .resx Files](#creating-resx-files)
- [Step-by-Step Localization Setup](#step-by-step-localization-setup)
- [Editing Default Culture Strings](#editing-default-culture-strings)
- [Common Culture Codes](#common-culture-codes)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Localization is the process of translating application resources into different languages for specific cultures. Syncfusion® WPF controls support comprehensive localization, allowing you to translate all control strings (button labels, menu items, messages, etc.) into any language.

**When to use localization:**
- Building applications for international markets
- Supporting multiple languages in a single application
- Customizing control strings for specific business terminology
- Meeting regional compliance requirements

## How Localization Works

Syncfusion WPF controls use a resource-based localization system:

1. **Default Resources** - Controls have built-in English strings
2. **Custom .resx Files** - You create resource files for target languages
3. **Culture Detection** - Controls automatically load strings based on `CurrentUICulture`
4. **Fallback Mechanism** - If culture-specific resource is missing, falls back to default English

**Localization Flow:**
```
Application Startup → Set CurrentUICulture → Controls Load → 
Check for .resx files → Load culture-specific strings → Display UI
```

## Changing Application Culture

To enable localization, set the `CurrentUICulture` before initializing UI components.

### Setting Culture in App.xaml.cs

**C# Example:**
```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // Set German culture
        System.Threading.Thread.CurrentThread.CurrentUICulture = 
            new System.Globalization.CultureInfo("de");
        
        base.OnStartup(e);
    }
}
```

### Setting Culture in MainWindow

**C# Example:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow() 
    {
        // Set culture BEFORE InitializeComponent
        System.Threading.Thread.CurrentThread.CurrentUICulture = 
            new System.Globalization.CultureInfo("de");
        
        InitializeComponent();
    }
}
```

**VB.NET Example:**
```vb
Partial Public Class MainWindow
    Public Sub New()
        ' Set culture BEFORE InitializeComponent
        System.Threading.Thread.CurrentThread.CurrentUICulture = _
            New System.Globalization.CultureInfo("de")
        
        InitializeComponent()
    End Sub
End Class
```

**⚠️ CRITICAL:** Set `CurrentUICulture` **before** calling `InitializeComponent()`, otherwise controls will load default English strings.

### Dynamic Culture Switching

Allow users to change language at runtime:

```csharp
private void ChangeLanguage(string cultureCode)
{
    // Set new culture
    Thread.CurrentThread.CurrentUICulture = new CultureInfo(cultureCode);
    
    // Reload window to apply new culture
    var newWindow = new MainWindow();
    newWindow.Show();
    this.Close();
}

// Usage:
// ChangeLanguage("de"); // German
// ChangeLanguage("fr"); // French
// ChangeLanguage("es"); // Spanish
```

## Creating .resx Files

Resource files (.resx) contain key-value pairs for translated strings.

### Prerequisites

You need default resource files from Syncfusion to know the string keys.

**Download from GitHub:**
[https://github.com/syncfusion/wpf-controls-localization-resx-files](https://github.com/syncfusion/wpf-controls-localization-resx-files)

This repository contains default .resx files for all Syncfusion WPF libraries.

### .resx File Naming Convention

Format: `<AssemblyName>.<CultureCode>.resx`

**Examples:**
- `Syncfusion.SfGrid.WPF.resx` - Default (English)
- `Syncfusion.SfGrid.WPF.de.resx` - German
- `Syncfusion.SfGrid.WPF.fr.resx` - French
- `Syncfusion.SfGrid.WPF.es.resx` - Spanish
- `Syncfusion.Tools.WPF.de.resx` - German for Tools assembly

### Understanding Assembly Names

Each Syncfusion control belongs to a specific assembly:

| Control | Assembly Name | .resx File Name |
|---------|---------------|-----------------|
| SfDataGrid | Syncfusion.SfGrid.WPF | Syncfusion.SfGrid.WPF.resx |
| Ribbon | Syncfusion.Tools.WPF | Syncfusion.Tools.WPF.resx |
| SfChart | Syncfusion.SfChart.WPF | Syncfusion.SfChart.WPF.resx |
| DockingManager | Syncfusion.Tools.WPF | Syncfusion.Tools.WPF.resx |
| SfTextBoxExt | Syncfusion.SfInput.WPF | Syncfusion.SfInput.WPF.resx |

## Step-by-Step Localization Setup

### Step 1: Create Resources Folder

1. Right-click your project in Solution Explorer
2. Select **Add** → **New Folder**
3. Name it **Resources**

### Step 2: Add Default .resx Files

1. Download default .resx files from [GitHub](https://github.com/syncfusion/wpf-controls-localization-resx-files)
2. Identify the assemblies you're using (e.g., `Syncfusion.SfGrid.WPF.dll`)
3. Copy corresponding .resx file to your **Resources** folder
4. In Solution Explorer, select the .resx file
5. In Properties window, set **Access Modifier** to **Public**

**Example:** If using SfDataGrid, copy `Syncfusion.SfGrid.WPF.resx` to Resources folder.

**Why add default .resx?** It provides:
- All string keys used by the control
- Default English values
- Template for creating culture-specific versions

### Step 3: Create Culture-Specific .resx Files

1. Right-click **Resources** folder
2. Select **Add** → **New Item**
3. Choose **Resource File** template
4. Name it: `Syncfusion.SfGrid.WPF.de.resx` (for German)
5. Click **Add**
6. In Properties window, set **Access Modifier** to **No code generation**

**⚠️ IMPORTANT:** Use **No code generation** for culture-specific files to avoid conflicts.

### Step 4: Add Translations

1. Open the culture-specific .resx file (e.g., `Syncfusion.SfGrid.WPF.de.resx`)
2. Open the default .resx file side-by-side for reference
3. Copy key names from default file
4. Add translated values

**Example Translations for SfDataGrid (German):**

| Name (Key) | Value (German) | Original (English) |
|------------|----------------|-------------------|
| FilterMenuItemAll | (Alle) | (All) |
| FilterMenuItemBlank | (Leer) | (Blank) |
| FilterMenuItemClear | Löschen | Clear |
| FilterMenuItemOK | OK | OK |
| FilterMenuItemCancel | Abbrechen | Cancel |
| FilterMenuItemSortAscending | Aufsteigend sortieren | Sort Ascending |
| FilterMenuItemSortDescending | Absteigend sortieren | Sort Descending |
| FilterMenuItemClearFilters | Filter löschen | Clear Filters |
| GroupDropAreaText | Ziehen Sie eine Spaltenkopf hierher zum Gruppieren | Drag a column header here to group by that column |

### Step 5: Verify Folder Structure

After setup, your project structure should look like:

```
YourProject/
├── Resources/
│   ├── Syncfusion.SfGrid.WPF.resx (default, Access Modifier: Public)
│   ├── Syncfusion.SfGrid.WPF.de.resx (German, No code generation)
│   ├── Syncfusion.SfGrid.WPF.fr.resx (French, No code generation)
│   └── Syncfusion.Tools.WPF.resx (if using Ribbon, etc.)
├── MainWindow.xaml
└── App.xaml.cs
```

### Step 6: Set Build Action

Ensure .resx files have correct build action:
1. Select .resx file in Solution Explorer
2. Properties window → **Build Action** should be **Embedded Resource**

### Step 7: Test Localization

Run your application and verify:
- ✓ Control strings appear in target language
- ✓ No missing strings (check for English fallbacks)
- ✓ Layout accommodates longer/shorter translations

## Editing Default Culture Strings

You can customize the default English strings without creating culture-specific files.

### Use Case

- Change terminology to match your business domain
- Replace "Customer" with "Client"
- Modify button text to match company standards

### How to Edit Defaults

1. Add default .resx file (without culture code) to Resources folder
   - Example: `Syncfusion.SfGrid.WPF.resx`
2. Set **Access Modifier** to **Public**
3. Modify string values as needed
4. Syncfusion controls will use your modified strings

**Example - Customizing English Strings:**

In `Syncfusion.SfGrid.WPF.resx`:

| Name | Original Value | Custom Value |
|------|----------------|--------------|
| FilterMenuItemAll | (All) | (Show All) |
| FilterMenuItemClear | Clear | Reset |
| GroupDropAreaText | Drag a column header here... | Group by dragging columns here... |

**Result:** All English users will see your custom strings.

## Common Culture Codes

### Widely Used Cultures

| Culture Code | Language | Region |
|--------------|----------|--------|
| `de` | German | Germany |
| `de-AT` | German | Austria |
| `de-CH` | German | Switzerland |
| `fr` | French | France |
| `fr-CA` | French | Canada |
| `es` | Spanish | Spain |
| `es-MX` | Spanish | Mexico |
| `it` | Italian | Italy |
| `ja` | Japanese | Japan |
| `zh-CN` | Chinese | China (Simplified) |
| `zh-TW` | Chinese | Taiwan (Traditional) |
| `ko` | Korean | Korea |
| `ru` | Russian | Russia |
| `pt` | Portuguese | Portugal |
| `pt-BR` | Portuguese | Brazil |
| `ar` | Arabic | Saudi Arabia |
| `he` | Hebrew | Israel |
| `nl` | Dutch | Netherlands |
| `pl` | Polish | Poland |
| `sv` | Swedish | Sweden |
| `tr` | Turkish | Turkey |

### Culture Fallback Mechanism

If you create `Syncfusion.SfGrid.WPF.fr.resx`, it will be used for:
- `fr` (French - France)
- `fr-CA` (French - Canada)
- `fr-BE` (French - Belgium)

Unless you create specific files like `Syncfusion.SfGrid.WPF.fr-CA.resx` for regional variants.

## Best Practices

### ✓ DO

- **Set culture early** - Before InitializeComponent()
- **Use consistent versions** - Same Syncfusion version for all assemblies
- **Test with actual users** - Native speakers should review translations
- **Handle long strings** - Ensure UI accommodates longer translations (German, Russian)
- **Use neutral cultures** - Use `de` instead of `de-DE` unless regional specifics needed
- **Version control .resx files** - Track translation changes over time

### ✗ DON'T

- **Don't hardcode strings** - In XAML or code-behind (use resources)
- **Don't forget default .resx** - Needed to know available keys
- **Don't use code generation** - For culture-specific .resx files
- **Don't mix versions** - Different Syncfusion versions may have different keys
- **Don't skip testing** - Always test with actual culture settings

### Performance Tips

- ✓ Load .resx files as embedded resources (default)
- ✓ Set culture once at startup (not per control)
- ✓ Cache CultureInfo objects if switching dynamically
- ✓ Only include .resx files for controls you actually use

## Troubleshooting

### Strings Still Appear in English

**Possible Causes:**
1. **Culture not set before InitializeComponent()**
   - Solution: Move culture setting to App.xaml.cs OnStartup or before InitializeComponent

2. **.resx file named incorrectly**
   - Solution: Check naming: `<AssemblyName>.<culture>.resx`

3. **Access Modifier incorrect**
   - Solution: Default .resx = Public, Culture-specific = No code generation

4. **Missing keys in .resx file**
   - Solution: Compare with default .resx, add missing keys

5. **Wrong culture code**
   - Solution: Verify culture code matches your setting

### Build Errors

**Error:** "Resource with the same name already exists"
- Solution: Remove duplicate .resx files or rename conflicting ones

**Error:** "Could not load assembly"
- Solution: Check Access Modifier settings on .resx files

### Layout Issues with Long Strings

**German/Russian strings are longer:**
- Use Grid columns with * width instead of fixed widths
- Set `TextWrapping="Wrap"` on TextBlock elements
- Test UI with longest expected translations
- Consider abbreviations where appropriate

### Missing Translations

**Some strings not translated:**
- Check if all keys from default .resx are in culture-specific file
- Verify spelling of key names (case-sensitive)
- Rebuild solution after adding new keys

## Example: Complete Localization Setup

### Scenario: Add German localization to SfDataGrid application

**Step 1:** Set culture in App.xaml.cs
```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de");
        base.OnStartup(e);
    }
}
```

**Step 2:** Create Resources folder with files:
- `Syncfusion.SfGrid.WPF.resx` (Access Modifier: Public)
- `Syncfusion.SfGrid.WPF.de.resx` (No code generation)

**Step 3:** Add German translations in `Syncfusion.SfGrid.WPF.de.resx`:
```
FilterMenuItemAll → (Alle)
FilterMenuItemClear → Löschen
FilterMenuItemOK → OK
GroupDropAreaText → Ziehen Sie eine Spaltenkopf hierher zum Gruppieren
```

**Step 4:** Run application - SfDataGrid now displays German strings

**Result:**
- Filter menu shows "Löschen" instead of "Clear"
- Group area shows German instructions
- All control strings localized

## Additional Resources

- **GitHub Repository:** [wpf-controls-localization-resx-files](https://github.com/syncfusion/wpf-controls-localization-resx-files)
- **Syncfusion Documentation:** [Localization in WPF](https://help.syncfusion.com/wpf/localization)
- **.NET Globalization:** [CultureInfo Class](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo)
- **Download Sample:** [GitHub - SfDataGrid Localization Sample](https://github.com/SyncfusionExamples/wpf-datagrid-localization)

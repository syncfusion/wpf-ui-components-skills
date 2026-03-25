# Localization

This reference guide provides comprehensive information on localizing the Syncfusion WPF ImageEditor control to support multiple languages and cultures.

## Overview

Localization is the process of translating application resources into different languages for specific cultures. The SfImageEditor control can be localized by adding resource files (.resx) for each target culture. This allows you to translate UI elements such as toolbar buttons, menu items, dialogs, and messages into any language.

The localization process involves:
- Setting the application's current UI culture
- Creating culture-specific resource files
- Adding translated Name/Value pairs for all UI strings
- Following Syncfusion's resource file naming conventions

## Setting CurrentUICulture

To localize the SfImageEditor, you must set the `CurrentUICulture` property before calling the `InitializeComponent()` method in your window or page constructor. This determines which culture-specific resource file will be loaded.

### Basic Culture Setting

```csharp
public MainWindow()
{
    Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("fr-FR");
    InitializeComponent();
}
```

### Dynamic Culture Setting

```csharp
public MainWindow()
{
    // Set culture based on user preference or system settings
    string cultureName = Properties.Settings.Default.PreferredLanguage ?? "en-US";
    Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo(cultureName);
    InitializeComponent();
}
```

### Common Culture Codes

- **en-US**: English (United States)
- **fr-FR**: French (France)
- **de-DE**: German (Germany)
- **es-ES**: Spanish (Spain)
- **ja-JP**: Japanese (Japan)
- **zh-CN**: Chinese (Simplified)
- **it-IT**: Italian (Italy)
- **pt-BR**: Portuguese (Brazil)

## Creating Resource Files

Resource files are XML-based files with a `.resx` extension that store localized strings as Name/Value pairs. Each culture requires its own resource file.

### Step 1: Create Resources Folder

1. In your WPF project, create a new folder named **Resources**
2. This folder will contain all localization resource files
3. The folder should be at the root of your project

### Step 2: Add Default Resource File

1. Download the default Syncfusion.SfImageEditor.WPF.resx file from the Syncfusion support portal
2. Add this file to the **Resources** folder
3. This serves as the base/default language resource (typically English)

**Download Link**: [Syncfusion.SfImageEditor.WPF.resx](https://www.syncfusion.com/downloads/support/directtrac/general/ze/Syncfusion.SfImageEditor.WPF-240771729)

### Step 3: Create Culture-Specific Resource Files

1. Right-click on the **Resources** folder
2. Select **Add** > **New Item**
3. In the Add New Item wizard, select **Resource File**
4. Name it according to the culture (see naming conventions below)
5. Click **Add** to create the file

## Resource File Naming Conventions

Syncfusion uses a specific naming pattern for resource files. Following this convention is critical for the localization to work correctly.

### Naming Pattern

```
Syncfusion.SfImageEditor.WPF.{culture}.resx
```

### Examples

- **French**: `Syncfusion.SfImageEditor.WPF.fr.resx` or `Syncfusion.SfImageEditor.WPF.fr-FR.resx`
- **German**: `Syncfusion.SfImageEditor.WPF.de.resx` or `Syncfusion.SfImageEditor.WPF.de-DE.resx`
- **Spanish**: `Syncfusion.SfImageEditor.WPF.es.resx` or `Syncfusion.SfImageEditor.WPF.es-ES.resx`
- **Japanese**: `Syncfusion.SfImageEditor.WPF.ja.resx` or `Syncfusion.SfImageEditor.WPF.ja-JP.resx`
- **Chinese (Simplified)**: `Syncfusion.SfImageEditor.WPF.zh-CN.resx`

### Culture Code Options

You can use either:
- **Neutral culture**: `Syncfusion.SfImageEditor.WPF.fr.resx` (applies to all French variants)
- **Specific culture**: `Syncfusion.SfImageEditor.WPF.fr-FR.resx` (applies only to French-France)

**Best Practice**: Use specific culture codes (e.g., fr-FR) when translations differ between regions, otherwise use neutral culture codes (e.g., fr) for broader applicability.

## Adding Name/Value Pairs in Resource Designer

Once you create a culture-specific resource file, you need to add translated strings for each UI element.

### Opening the Resource Designer

1. Double-click the `.resx` file in Solution Explorer
2. This opens the Resource Designer view
3. You'll see a grid with Name, Value, and Comment columns

### Adding Translations

1. Copy the Name entries from the default `Syncfusion.SfImageEditor.WPF.resx` file
2. In your culture-specific file, add the same Name entries
3. Translate the Value to the target language
4. Optionally add Comments for context

### Example Resource Entries

**English (default - Syncfusion.SfImageEditor.WPF.resx)**:
- Name: `Crop` | Value: `Crop`
- Name: `Save` | Value: `Save`
- Name: `Reset` | Value: `Reset`
- Name: `Brightness` | Value: `Brightness`
- Name: `Contrast` | Value: `Contrast`
- Name: `Saturation` | Value: `Saturation`
- Name: `Rotate` | Value: `Rotate`
- Name: `Flip` | Value: `Flip`
- Name: `Undo` | Value: `Undo`
- Name: `Redo` | Value: `Redo`

**French (Syncfusion.SfImageEditor.WPF.fr.resx)**:
- Name: `Crop` | Value: `Rogner`
- Name: `Save` | Value: `Enregistrer`
- Name: `Reset` | Value: `Réinitialiser`
- Name: `Brightness` | Value: `Luminosité`
- Name: `Contrast` | Value: `Contraste`
- Name: `Saturation` | Value: `Saturation`
- Name: `Rotate` | Value: `Pivoter`
- Name: `Flip` | Value: `Retourner`
- Name: `Undo` | Value: `Annuler`
- Name: `Redo` | Value: `Rétablir`

## Step-by-Step Localization Process

Follow this complete workflow to localize your SfImageEditor application:

### Step 1: Project Setup

```
YourWPFProject/
├── Resources/
│   ├── Syncfusion.SfImageEditor.WPF.resx (default)
│   └── (culture-specific .resx files will go here)
├── MainWindow.xaml
└── MainWindow.xaml.cs
```

### Step 2: Download and Add Default Resource File

1. Download `Syncfusion.SfImageEditor.WPF.resx` from Syncfusion
2. Add it to the **Resources** folder
3. Set Build Action to **Embedded Resource** (in Properties window)

### Step 3: Create Culture-Specific Resource File

For French localization:

1. Right-click **Resources** folder
2. Add > New Item > Resource File
3. Name: `Syncfusion.SfImageEditor.WPF.fr.resx`
4. Set Build Action to **Embedded Resource**

### Step 4: Translate Resource Strings

1. Open the default `Syncfusion.SfImageEditor.WPF.resx`
2. Note all Name entries
3. Open `Syncfusion.SfImageEditor.WPF.fr.resx`
4. Add same Name entries with French Value translations

### Step 5: Set Culture in Code

In your MainWindow or App.xaml.cs:

```csharp
public MainWindow()
{
    Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("fr-FR");
    InitializeComponent();
}
```

### Step 6: Build and Test

1. Build your project
2. Run the application
3. Verify all UI elements display in the target language

## French Localization Example

This complete example demonstrates localizing SfImageEditor to French.

### Project Structure

```
FrenchImageEditorApp/
├── Resources/
│   ├── Syncfusion.SfImageEditor.WPF.resx
│   └── Syncfusion.SfImageEditor.WPF.fr.resx
├── MainWindow.xaml
└── MainWindow.xaml.cs
```

### MainWindow.xaml

```xml
<Window x:Class="FrenchImageEditorApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Éditeur d'Images" Height="600" Width="800">
    <Grid>
        <syncfusion:SfImageEditor x:Name="imageEditor" />
    </Grid>
</Window>
```

### MainWindow.xaml.cs

```csharp
using System.Globalization;
using System.Threading;
using System.Windows;

namespace FrenchImageEditorApp
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            Thread.CurrentThread.CurrentUICulture = new CultureInfo("fr-FR");
            InitializeComponent();
        }
    }
}
```

### Sample French Translations

**Common UI Elements**:
- Save → Enregistrer
- Cancel → Annuler
- OK → OK
- Apply → Appliquer
- Reset → Réinitialiser

**Toolbar Items**:
- Crop → Rogner
- Rotate → Pivoter
- Flip → Retourner
- Brightness → Luminosité
- Contrast → Contraste
- Saturation → Saturation
- Hue → Teinte
- Blur → Flou
- Sharpen → Netteté

**Edit Operations**:
- Undo → Annuler
- Redo → Rétablir
- Delete → Supprimer
- Copy → Copier
- Cut → Couper
- Paste → Coller

### Result

After implementing French localization, all toolbar buttons, menus, and dialogs in the SfImageEditor will display in French, providing a native experience for French-speaking users.

## Troubleshooting

### Issue: Localization Not Working

**Symptoms**: UI still displays in English despite setting CurrentUICulture

**Solutions**:
1. Verify `CurrentUICulture` is set **before** `InitializeComponent()`
2. Check resource file naming matches exactly: `Syncfusion.SfImageEditor.WPF.{culture}.resx`
3. Ensure Build Action is set to **Embedded Resource** for all .resx files
4. Verify culture code is correct (e.g., "fr-FR" not "fr_FR")
5. Rebuild the entire solution

### Issue: Missing Translations

**Symptoms**: Some UI elements are translated, others remain in English

**Solutions**:
1. Ensure all Name entries from default .resx are present in culture-specific .resx
2. Check for typos in Name entries (names must match exactly)
3. Verify no empty Value fields in the resource file
4. Open Resource Designer and check for warnings

### Issue: Build Errors with Resource Files

**Symptoms**: Compiler errors related to .resx files

**Solutions**:
1. Check .resx file is valid XML (open in XML editor)
2. Ensure no duplicate Name entries in same file
3. Remove any invalid characters from Name or Value fields
4. Set Custom Tool to "ResXFileCodeGenerator" in file properties

### Issue: Wrong Culture Loaded

**Symptoms**: Different language than expected is displayed

**Solutions**:
1. Check system locale matches your target culture
2. Use specific culture (fr-FR) instead of neutral (fr) if conflicts exist
3. Verify no fallback to parent culture is occurring
4. Check Thread.CurrentThread.CurrentUICulture value at runtime:

```csharp
System.Diagnostics.Debug.WriteLine($"Current Culture: {Thread.CurrentThread.CurrentUICulture.Name}");
```

### Issue: Resource File Not Found

**Symptoms**: Application can't find the resource file at runtime

**Solutions**:
1. Verify .resx files are in correct folder structure
2. Check namespace matches your project's root namespace
3. Ensure files are included in project (not just in file system)
4. Clean and rebuild solution
5. Check Output window for resource loading errors

### Best Practices

1. **Always test**: Run application with each localized culture
2. **Use consistent naming**: Follow Syncfusion conventions exactly
3. **Complete translations**: Translate all strings, not just some
4. **Plan for growth**: Structure resources folder for multiple controls
5. **Version control**: Track .resx files in source control
6. **Comments**: Add comments in .resx files to provide context for translators
7. **Fallback**: Keep default English .resx as fallback for missing translations

---

# Dictionary Support

## Table of Contents
- [Overview](#overview)
- [Default English Dictionary](#default-english-dictionary)
- [Dictionary Types](#dictionary-types)
- [Hunspell Dictionary](#hunspell-dictionary)
- [Ispell Dictionary](#ispell-dictionary)
- [OpenOffice Dictionary](#openoffice-dictionary)
- [Custom Dictionary](#custom-dictionary)
- [Multi-Language Support](#multi-language-support)
- [Switching Culture at Runtime](#switching-culture-at-runtime)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

SfSpellChecker supports multiple dictionary formats and cultures, allowing spell checking for any language. You can use the default English dictionary or add custom dictionaries in three standard formats:

1. **Hunspell** - Most common format (*.dic, *.aff)
2. **Ispell** - Alternative format (*.dic/*.xlg, *.aff)
3. **OpenOffice** - OpenOffice/LibreOffice format (*.dic, *.aff)

You can also add **Custom Dictionaries** for specialized terms not in standard dictionaries.

## Default English Dictionary

SfSpellChecker includes a built-in English dictionary that works without any configuration.

### Usage

```csharp
// Default English dictionary is automatically used
SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.EnableSpellCheck = true;
// Spell checks English text automatically
```

**Note:** The built-in dictionary is disabled when you add custom dictionaries to the `Dictionaries` collection.

## Dictionary Types

### Dictionary File Structure

All dictionary formats require two files:

1. **Word File** (*.dic) - Contains list of valid words
2. **Grammar/Affix File** (*.aff) - Contains grammar rules, prefixes, suffixes

**Custom Dictionary Exception:** Only requires word file (*.dic or *.txt), no grammar file.

### Choosing a Dictionary Format

| Format | Use When | Advantages |
|--------|----------|------------|
| **Hunspell** | Most scenarios | Widely available, well-supported |
| **Ispell** | Legacy systems | Compatible with older systems |
| **OpenOffice** | Using LibreOffice/OpenOffice | Free, open-source dictionaries |
| **Custom** | Specialized terms | Company names, technical terms |

## Hunspell Dictionary

Most popular dictionary format, used by Firefox, Chrome, LibreOffice, and many other applications.

### File Requirements

- **Word file:** `*.dic` (e.g., `fr-FR.dic`)
- **Grammar file:** `*.aff` (e.g., `fr-FR.aff`)

### Adding Hunspell Dictionary

**Step 1:** Add dictionary files to your project

Place `*.dic` and `*.aff` files in your project (e.g., `/Dictionaries/French/`).

Set **Build Action** to `Resource`:
```
fr-FR.dic → Build Action: Resource
fr-FR.aff → Build Action: Resource
```

![Adding Hunspell Dictionary Files](../../../../../../../docs/Dictionary_images/HunspellAdding.png)

**Step 2:** Create and configure HunspellDictionary

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    Culture="fr-FR"
    EnableSpellCheck="True">
    <syncfusion:SfSpellChecker.Dictionaries>
        <syncfusion:HunspellDictionary 
            DictionaryUri="/MyApp;component/Dictionaries/French/fr-FR.dic"
            GrammarUri="/MyApp;component/Dictionaries/French/fr-FR.aff"
            Culture="fr-FR"/>
    </syncfusion:SfSpellChecker.Dictionaries>
</syncfusion:SfSpellChecker>
```

**Step 3:** Programmatic setup

```csharp
// Create culture
CultureInfo frenchCulture = new CultureInfo("fr-FR");

// Create spell checker
SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.Dictionaries = new DictionaryCollection();

// Add Hunspell dictionary
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = frenchCulture,
        DictionaryUri = new Uri("/MyApp;component/Dictionaries/French/fr-FR.dic", UriKind.Relative),
        GrammarUri = new Uri("/MyApp;component/Dictionaries/French/fr-FR.aff", UriKind.Relative)
    }
);

// Set culture
spellChecker.Culture = frenchCulture;
```

### Example: French Spell Check

```csharp
// French text with errors
textbox.Text = "Nous sommevs heureusre de vous avochir ici";

// Configure French Hunspell dictionary
CultureInfo culture = new CultureInfo("fr-FR");
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = culture,
        DictionaryUri = new Uri("/MyApp;component/French/fr-FR.dic", UriKind.Relative),
        GrammarUri = new Uri("/MyApp;component/French/fr-FR.aff", UriKind.Relative)
    }
);
spellChecker.Culture = culture;

// Perform spell check
spellChecker.PerformSpellCheckUsingDialog();
```

## Ispell Dictionary

Alternative dictionary format, compatible with Unix Ispell spell checker.

### File Requirements

- **Word file:** `*.dic` or `*.xlg`
- **Grammar file:** `*.aff`

### Adding Ispell Dictionary

**Step 1:** Add dictionary files as resources

```
es-ES.dic → Build Action: Resource
es-ES.aff → Build Action: Resource
```

**Step 2:** XAML configuration

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    Culture="es-ES"
    EnableSpellCheck="True">
    <syncfusion:SfSpellChecker.Dictionaries>
        <syncfusion:IspellDictionary 
            DictionaryUri="/MyApp;component/Dictionaries/Spanish/es-ES.dic"
            GrammarUri="/MyApp;component/Dictionaries/Spanish/es-ES.aff"
            Culture="es-ES"/>
    </syncfusion:SfSpellChecker.Dictionaries>
</syncfusion:SfSpellChecker>
```

**Step 3:** Programmatic setup

```csharp
// Create Spanish culture
CultureInfo spanishCulture = new CultureInfo("es-ES");

SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.Dictionaries = new DictionaryCollection();

// Add Ispell dictionary
spellChecker.Dictionaries.Add(
    new IspellDictionary()
    {
        Culture = spanishCulture,
        DictionaryUri = new Uri("/MyApp;component/Spanish/es-ES.dic", UriKind.Relative),
        GrammarUri = new Uri("/MyApp;component/Spanish/es-ES.aff", UriKind.Relative)
    }
);

spellChecker.Culture = spanishCulture;
```

### Example: Spanish Spell Check

```csharp
textbox.Text = "gracsq por venizr por favor ven de nuevoq";

CultureInfo culture = new CultureInfo("es-ES");
spellChecker.Dictionaries.Add(
    new IspellDictionary()
    {
        Culture = culture,
        DictionaryUri = new Uri("/MyApp;component/Spanish/es-ES.dic", UriKind.Relative),
        GrammarUri = new Uri("/MyApp;component/Spanish/es-ES.aff", UriKind.Relative)
    }
);
spellChecker.Culture = culture;
```

## OpenOffice Dictionary

Compatible with OpenOffice and LibreOffice dictionaries, which are freely available.

### File Requirements

- **Word file:** `*.dic`
- **Grammar file:** `*.aff`

### Adding OpenOffice Dictionary

**Step 1:** Add dictionary files as resources

**Step 2:** XAML configuration

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    Culture="es-ES"
    EnableSpellCheck="True">
    <syncfusion:SfSpellChecker.Dictionaries>
        <syncfusion:OpenOfficeDictionary 
            DictionaryUri="/MyApp;component/Dictionaries/Spanish/es-ES.dic"
            GrammarUri="/MyApp;component/Dictionaries/Spanish/es-ES.aff"
            Culture="es-ES"/>
    </syncfusion:SfSpellChecker.Dictionaries>
</syncfusion:SfSpellChecker>
```

**Step 3:** Programmatic setup

```csharp
CultureInfo culture = new CultureInfo("es-ES");

SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.Dictionaries = new DictionaryCollection();

// Add OpenOffice dictionary
spellChecker.Dictionaries.Add(
    new OpenOfficeDictionary()
    {
        Culture = culture,
        DictionaryUri = new Uri("/MyApp;component/Spanish/es-ES.dic", UriKind.Relative),
        GrammarUri = new Uri("/MyApp;component/Spanish/es-ES.aff", UriKind.Relative)
    }
);

spellChecker.Culture = culture;
```

### Where to Get OpenOffice Dictionaries

Download free dictionaries from:
- [LibreOffice Extensions](https://extensions.libreoffice.org/)
- [OpenOffice Extensions](https://extensions.openoffice.org/)

## Custom Dictionary

Add specialized terms, company names, product names, or technical jargon not in standard dictionaries.

### Features

- **No grammar file required** - Only word list
- **User-editable** - Users can add words via "Add to Dictionary"
- **Supplementary** - Works alongside standard dictionaries
- **Persistent** - Words saved across sessions

### File Format

Simple text file with one word per line:

```
Syncfusion
SfSpellChecker
WPF
XAML
NuGet
```

### Adding Custom Dictionary

**Step 1:** Create custom dictionary text file

Create `CustomWords.txt`:
```
Syncfusion
Blazor
MAUI
WinForms
```

**Important:** 
- Set **Build Action:** `None`
- Set **Copy to Output Directory:** `Copy if newer`

![Custom Dictionary Setup](../../../../../../../docs/Dictionary_images/CustomDictionaryAdding.png)

**Step 2:** Add custom dictionary to spell checker

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    Culture="en-US"
    EnableSpellCheck="True">
    <syncfusion:SfSpellChecker.Dictionaries>
        
        <!-- Custom dictionary -->
        <syncfusion:CustomDictionary 
            DictionaryUri="E:/MyApp/bin/Debug/Dictionaries/CustomWords.txt"
            Culture="en-US"/>
        
        <!-- Standard dictionary (required) -->
        <syncfusion:OpenOfficeDictionary 
            DictionaryUri="/MyApp;component/English/en-US.dic"
            GrammarUri="/MyApp;component/English/en-US.aff"
            Culture="en-US"/>
            
    </syncfusion:SfSpellChecker.Dictionaries>
</syncfusion:SfSpellChecker>
```

**Step 3:** Programmatic setup with absolute path

```csharp
CultureInfo culture = new CultureInfo("en-US");

// Get current project directory
Uri customDictUri = new Uri(
    Directory.GetCurrentDirectory() + @"\Dictionaries\CustomWords.txt",
    UriKind.Absolute
);

// Add custom dictionary
spellChecker.Dictionaries.Add(
    new CustomDictionary()
    {
        Culture = culture,
        DictionaryUri = customDictUri
    }
);

// Add standard dictionary (required)
spellChecker.Dictionaries.Add(
    new OpenOfficeDictionary()
    {
        Culture = culture,
        DictionaryUri = new Uri("/MyApp;component/English/en-US.dic", UriKind.Relative),
        GrammarUri = new Uri("/MyApp;component/English/en-US.aff", UriKind.Relative)
    }
);

spellChecker.Culture = culture;
```

### "Add to Dictionary" Functionality

When users click "Add to Dictionary" in dialog or context menu:
- Word is automatically added to CustomDictionary
- Word is no longer flagged as error
- Changes persist across sessions

**Note:** CustomDictionary must be configured for this feature to work.

### Custom Dictionary Best Practices

1. **Always use with standard dictionary** - Custom dictionary supplements, doesn't replace
2. **Use absolute paths** - Ensures file is found at runtime
3. **One word per line** - Simple text format
4. **No duplicates** - Keep list clean
5. **Case-sensitive** - "API" and "api" are different

## Multi-Language Support

Add multiple dictionaries for different languages and switch between them.

### Adding Multiple Language Dictionaries

```csharp
SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.Dictionaries = new DictionaryCollection();

// Add English dictionary
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = new CultureInfo("en-US"),
        DictionaryUri = new Uri("/MyApp;component/Dictionaries/en-US.dic", UriKind.Relative),
        GrammarUri = new Uri("/MyApp;component/Dictionaries/en-US.aff", UriKind.Relative)
    }
);

// Add French dictionary
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = new CultureInfo("fr-FR"),
        DictionaryUri = new Uri("/MyApp;component/Dictionaries/fr-FR.dic", UriKind.Relative),
        GrammarUri = new Uri("/MyApp;component/Dictionaries/fr-FR.aff", UriKind.Relative)
    }
);

// Add Spanish dictionary
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = new CultureInfo("es-ES"),
        DictionaryUri = new Uri("/MyApp;component/Dictionaries/es-ES.dic", UriKind.Relative),
        GrammarUri = new Uri("/MyApp;component/Dictionaries/es-ES.aff", UriKind.Relative)
    }
);

// Set initial culture
spellChecker.Culture = new CultureInfo("en-US");
```

## Switching Culture at Runtime

Change spell check language dynamically based on user selection.

### Implementation

```csharp
// Add multiple dictionaries at startup
AddAllDictionaries();

// Switch to French
private void SwitchToFrench_Click(object sender, RoutedEventArgs e)
{
    spellChecker.Culture = new CultureInfo("fr-FR");
}

// Switch to Spanish
private void SwitchToSpanish_Click(object sender, RoutedEventArgs e)
{
    spellChecker.Culture = new CultureInfo("es-ES");
}

// Switch to English
private void SwitchToEnglish_Click(object sender, RoutedEventArgs e)
{
    spellChecker.Culture = new CultureInfo("en-US");
}
```

### Language Selector UI

```xml
<ComboBox x:Name="languageSelector" SelectionChanged="LanguageSelector_SelectionChanged">
    <ComboBoxItem Content="English" Tag="en-US"/>
    <ComboBoxItem Content="French" Tag="fr-FR"/>
    <ComboBoxItem Content="Spanish" Tag="es-ES"/>
</ComboBox>
```

```csharp
private void LanguageSelector_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (languageSelector.SelectedItem is ComboBoxItem item)
    {
        string cultureName = item.Tag.ToString();
        spellChecker.Culture = new CultureInfo(cultureName);
    }
}
```

## Best Practices

### 1. Load Dictionaries Once

```csharp
// Load during initialization
public MainWindow()
{
    InitializeComponent();
    LoadDictionaries(); // Load once
}

private void LoadDictionaries()
{
    spellChecker.Dictionaries = new DictionaryCollection();
    // Add all dictionaries
}
```

### 2. Use Resource Embedding for Standard Dictionaries

```xml
<!-- Embed in assembly for easy deployment -->
<syncfusion:HunspellDictionary 
    DictionaryUri="/MyApp;component/Dictionaries/en-US.dic"
    GrammarUri="/MyApp;component/Dictionaries/en-US.aff"
    Culture="en-US"/>
```

### 3. Use File System for Custom Dictionaries

```csharp
// Allow users to edit custom dictionary
Uri customDictUri = new Uri(
    Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
                 "MyApp", "CustomWords.txt"),
    UriKind.Absolute
);
```

### 4. Handle Missing Dictionary Files

```csharp
try
{
    spellChecker.Dictionaries.Add(new HunspellDictionary()
    {
        Culture = culture,
        DictionaryUri = dictUri,
        GrammarUri = grammarUri
    });
}
catch (FileNotFoundException ex)
{
    MessageBox.Show($"Dictionary file not found: {ex.Message}");
    // Fall back to default dictionary
}
```

## Troubleshooting

### Dictionary Not Loading

**Issue:** Spell checker not working after adding dictionary

**Solutions:**
- Verify file paths are correct
- Check Build Action is set to `Resource`
- Ensure culture code matches dictionary culture
- Verify both .dic and .aff files are present

### Custom Dictionary Not Saving Words

**Issue:** "Add to Dictionary" doesn't persist words

**Solutions:**
- Use absolute path, not relative
- Verify file has write permissions
- Check file isn't marked Read-Only
- Ensure CustomDictionary is in Dictionaries collection

### Wrong Language Being Used

**Issue:** Spell checker using wrong language

**Solutions:**
- Check `spellChecker.Culture` matches intended language
- Verify dictionary for that culture exists in collection
- Ensure culture code is correct (e.g., "fr-FR" not "fr")

### Multiple Dictionaries Conflicting

**Issue:** Unpredictable behavior with multiple dictionaries

**Solutions:**
- Only one dictionary per culture
- Set explicit `Culture` property
- Don't mix dictionary types for same culture

## Next Steps

- **Learn spell check methods:** Read [spell-check-methods.md](spell-check-methods.md)
- **Configure ignore options:** Read [ignore-options.md](ignore-options.md)
- **Handle events:** Read [customization.md](customization.md)

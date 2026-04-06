# Dictionary Support

This document describes supported dictionary formats and concise examples to configure dictionaries for `SfSpellChecker`. Redundant, near-identical sections have been consolidated.

## Overview

SfSpellChecker supports multiple dictionary formats (Hunspell, Ispell, OpenOffice) and custom dictionaries. Use the built-in English dictionary by default or add language-specific dictionaries to the `Dictionaries` collection.

## Built-in English Dictionary

The default English dictionary is used automatically unless you add custom dictionaries. If you add custom dictionaries, the built-in dictionary may be disabled.

```csharp
// Default usage
SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.EnableSpellCheck = true;
```

## Dictionary Formats (Representative)

- **Hunspell**: Common format (`*.dic`, `*.aff`). Recommended for most scenarios.
- **Ispell / OpenOffice**: Alternative formats with similar setup steps; treat these as alternative dictionary formats and follow the same resource or programmatic registration approach.
- **Custom Dictionary**: Plain text file with one word per line; no grammar file required.

### Add a Hunspell Dictionary (XAML)

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

### Programmatic: Add a Dictionary (Generic)

```csharp
var culture = new CultureInfo("fr-FR");
spellChecker.Dictionaries = new DictionaryCollection();

spellChecker.Dictionaries.Add(new HunspellDictionary
{
    Culture = culture,
    DictionaryUri = new Uri("/MyApp;component/Dictionaries/French/fr-FR.dic", UriKind.Relative),
    GrammarUri = new Uri("/MyApp;component/Dictionaries/French/fr-FR.aff", UriKind.Relative)
});

spellChecker.Culture = culture;
```

### Alternative Formats

For Ispell or OpenOffice dictionaries, the resource or programmatic registration is the same — register the appropriate dictionary type (`IspellDictionary` or `OpenOfficeDictionary`) and provide `DictionaryUri` and `GrammarUri` where applicable.

## Custom Dictionaries

Custom dictionaries are plain text files with one word per line. Configure them with a `CustomDictionary` entry and prefer absolute paths when users must edit them at runtime.

```txt
Syncfusion
SfSpellChecker
WPF
```

```csharp
// Example: add custom dictionary programmatically
var customUri = new Uri(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), "MyApp", "CustomWords.txt"));
spellChecker.Dictionaries.Add(new CustomDictionary { Culture = new CultureInfo("en-US"), DictionaryUri = customUri });
```

## Multi-Language Support

Add multiple dictionaries (one per culture) to `Dictionaries`, and switch `spellChecker.Culture` at runtime to change active language.

```csharp
// Add multiple hunspell dictionaries at startup
spellChecker.Dictionaries.Add(new HunspellDictionary { Culture = new CultureInfo("en-US"), DictionaryUri = new Uri("/MyApp;component/Dictionaries/en-US.dic", UriKind.Relative), GrammarUri = new Uri("/MyApp;component/Dictionaries/en-US.aff", UriKind.Relative) });
spellChecker.Dictionaries.Add(new HunspellDictionary { Culture = new CultureInfo("fr-FR"), DictionaryUri = new Uri("/MyApp;component/Dictionaries/fr-FR.dic", UriKind.Relative), GrammarUri = new Uri("/MyApp;component/Dictionaries/fr-FR.aff", UriKind.Relative) });
spellChecker.Culture = new CultureInfo("en-US");
```

## Best Practices

- Load dictionaries once during initialization.
- Embed standard dictionaries as resources for deployment convenience.
- Use file-system based custom dictionaries when users need to edit them.
- Handle missing dictionary files with a clear fallback and user-facing message.

## Troubleshooting (short)

- Verify `DictionaryUri` and `GrammarUri` paths and build actions.
- Ensure culture codes match dictionary contents (e.g., `fr-FR`).
- Use only one dictionary per culture to avoid conflicts.


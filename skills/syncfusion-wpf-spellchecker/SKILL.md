---
name: syncfusion-wpf-spellchecker
description: Guide for implementing Syncfusion WPF SpellChecker (SfSpellChecker) control in WPF applications. Use this skill when implementing spell checking, text validation, dictionary management, or spelling error detection in WPF. Covers checking spelling mistakes in TextBox/RichTextBox controls, language dictionaries, custom dictionaries, context menu suggestions, and spelling validation requirements.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF SpellChecker

SfSpellChecker provides a simple and intuitive interface to check for spelling errors in WPF text editor controls. It offers suggestions for misspelled words through dialog and context menu, supports multiple languages/cultures, custom dictionaries, and various ignore options.

## When to Use This Skill

Use this skill when you need to:
- Add spell checking capability to WPF TextBox, RichTextBox, or other text editor controls
- Implement dialog-based or context menu-based spell checking
- Support multiple languages and cultures in spell checking
- Add custom dictionaries (Hunspell, Ispell, or OpenOffice formats)
- Configure ignore rules for emails, URLs, HTML tags, mixed case words, etc.
- Provide real-time spelling suggestions to users
- Customize spell checker appearance with themes
- Handle spell check completion events

## Component Overview

**Control:** SfSpellChecker (Syncfusion.Windows.Controls.SfSpellChecker)  
**Platform:** WPF  
**Namespace:** `Syncfusion.Windows.Controls`  
**Assembly:** `Syncfusion.SfSpellChecker.WPF`

**Key Capabilities:**
- Dialog-based spell checking with suggestion list
- Context menu suggestions with right-click on error words
- Multi-language support with custom dictionaries
- Ignore options for special text patterns
- Custom word dictionary support
- Real-time error highlighting
- Replace, Ignore All, Add to Dictionary actions

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and assembly deployment
- Adding SfSpellChecker to an application
- Attaching spell checker to TextBox
- First spell check with dialog
- Context menu setup
- Enable/disable spell checking
- Getting suggestions programmatically

### Spell Check Methods
📄 **Read:** [references/spell-check-methods.md](references/spell-check-methods.md)
- Dialog-based spell checking
- Context menu-based suggestions
- PerformSpellCheckUsingDialog method
- Error word highlighting
- Replace and ignore operations
- Add to Dictionary functionality

### Ignore Options
📄 **Read:** [references/ignore-options.md](references/ignore-options.md)
- Ignoring email addresses
- Ignoring URLs and internet addresses
- Ignoring HTML tags
- Ignoring mixed case words
- Ignoring uppercase words
- Ignoring alphanumeric words
- Configuration examples

### Dictionary Support
📄 **Read:** [references/dictionary-support.md](references/dictionary-support.md)
- Default English dictionary
- Multi-language spell checking
- Hunspell dictionary format
- Ispell dictionary format
- OpenOffice dictionary format
- Adding custom dictionaries
- Custom word lists
- Switching culture at runtime
- DictionaryCollection management

### Customization and Theming
📄 **Read:** [references/customization.md](references/customization.md)
- SpellCheckCompleted event
- Restricting completion message box
- Theme support with SfSkinManager
- ThemeStudio customization
- Visual appearance options

## Quick Start

### Basic Setup with Dialog

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <StackPanel>
            <TextBox 
                Text="Ths is a sampel text with speling errors."
                Name="textbox"
                TextWrapping="Wrap">
                
                <!--Attach SpellChecker to TextBox-->
                <syncfusion:SfSpellChecker.SpellChecker>
                    <syncfusion:SfSpellChecker 
                        x:Name="spellChecker"
                        EnableSpellCheck="True"/>
                </syncfusion:SfSpellChecker.SpellChecker>
            </TextBox>
            
            <Button 
                Content="Spell Check"
                Click="SpellCheck_ButtonClick"
                HorizontalAlignment="Center"/>
        </StackPanel>
    </Grid>
</Window>
```

```csharp
// Code-behind
private void SpellCheck_ButtonClick(object sender, RoutedEventArgs e)
{
    spellChecker.PerformSpellCheckUsingDialog();
}
```

### Quick Setup with Context Menu

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    EnableContextMenu="True"/>
```

Users can right-click on error words (red underlined) to see suggestions.

## Common Patterns

### Pattern 1: Basic Spell Check with Dialog

```csharp
// Create spell checker instance
SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.EnableSpellCheck = true;

// Attach to TextBox
SfSpellChecker.SetSpellChecker(textbox, spellChecker);

// Open spell check dialog
spellChecker.PerformSpellCheckUsingDialog();
```

### Pattern 2: Context Menu with Ignore Options

```csharp
SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.EnableSpellCheck = true;
spellChecker.EnableContextMenu = true;

// Configure ignore options
spellChecker.IgnoreUrl = true;
spellChecker.IgnoreEmailAddress = true;
spellChecker.IgnoreUpperCaseWords = true;

SfSpellChecker.SetSpellChecker(textbox, spellChecker);
```

### Pattern 3: Multi-Language Support

```csharp
// Create culture-specific spell checker
CultureInfo culture = new CultureInfo("fr-FR");
SfSpellChecker spellChecker = new SfSpellChecker();

// Add French dictionary
spellChecker.Dictionaries = new DictionaryCollection();
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = culture,
        GrammarUri = new Uri("/MyApp;component/Dictionaries/fr-FR.aff", UriKind.Relative),
        DictionaryUri = new Uri("/MyApp;component/Dictionaries/fr-FR.dic", UriKind.Relative)
    }
);

spellChecker.Culture = culture;
SfSpellChecker.SetSpellChecker(textbox, spellChecker);
```

### Pattern 4: Custom Dictionary

```csharp
// Add custom words dictionary
Uri customDictUri = new Uri(Directory.GetCurrentDirectory() + 
                           @"\Dictionaries\CustomWords.txt", UriKind.Absolute);

spellChecker.Dictionaries.Add(
    new CustomDictionary()
    {
        Culture = new CultureInfo("en-US"),
        DictionaryUri = customDictUri
    }
);
```

### Pattern 5: Handle Spell Check Completion

```csharp
spellChecker.SpellCheckCompleted += (sender, e) =>
{
    // Suppress default message box
    (e as SpellCheckCompletedEventArgs).ShowMessageBox = false;
    
    // Custom completion logic
    MessageBox.Show("Spell check completed!", "Done");
};
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `EnableSpellCheck` | bool | Enable/disable spell checking (default: true) |
| `EnableContextMenu` | bool | Enable context menu suggestions (default: true) |
| `Culture` | CultureInfo | Culture for spell checking |
| `Dictionaries` | DictionaryCollection | Collection of dictionaries |
| `IgnoreUrl` | bool | Ignore internet addresses |
| `IgnoreEmailAddress` | bool | Ignore email addresses |
| `IgnoreHtmlTags` | bool | Ignore HTML tags |
| `IgnoreMixedCaseWords` | bool | Ignore mixed case words (e.g., AbCDeFH) |
| `IgnoreUpperCaseWords` | bool | Ignore all uppercase words |
| `IgnoreAlphaNumericWords` | bool | Ignore words with numbers |

## Key Methods

| Method | Description |
|--------|-------------|
| `PerformSpellCheckUsingDialog()` | Opens spell check dialog |
| `GetSuggestions(string word)` | Gets suggestion list for error word |
| `GetPhoneticWords(string word)` | Gets phonetic word suggestions |
| `GetAnagrams(string word)` | Gets anagram suggestions |

## Common Use Cases

1. **Text Editor with Spell Check** - Add comprehensive spell checking to WPF text editors
2. **Multi-Language Document Editor** - Support multiple languages with culture-specific dictionaries
3. **Custom Domain Dictionary** - Add industry-specific or company-specific terms
4. **Email Client** - Spell check with email/URL ignore options
5. **HTML Editor** - Spell check while ignoring HTML markup
6. **Form Validation** - Validate text input fields for spelling errors
7. **Accessible Text Input** - Provide context menu for easy error correction

## Sample Applications

GitHub samples: [WPF SpellChecker Examples](https://github.com/SyncfusionExamples/WPF-SpellChecker-examples)

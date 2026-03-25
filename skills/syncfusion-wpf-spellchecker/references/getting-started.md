# Getting Started with SfSpellChecker

This guide covers the basics of adding and configuring SfSpellChecker in WPF applications.

## Assembly Deployment

### Required Assembly

To use SfSpellChecker, add the following assembly reference:

- **Syncfusion.SfSpellChecker.WPF**

### NuGet Package Installation

Install the NuGet package in your WPF application:

```powershell
Install-Package Syncfusion.SfSpellChecker.WPF
```

For more details, see [How to install NuGet packages](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages).

## Adding Namespace

Add the SfSpellChecker namespace to your XAML:

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation">
    <!-- Your content -->
</Window>
```

Or use the traditional namespace declaration:

```xml
xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Controls;assembly=Syncfusion.SfSpellChecker.WPF"
```

## Basic Implementation

### Step 1: Create a WPF Project

Create a new WPF project in Visual Studio and add the required assembly reference.

### Step 2: Add TextBox and Attach SpellChecker

```xml
<Grid>
    <StackPanel>
        <TextBox 
            Text="Natusre is an importsant and integral part of mankind."
            Name="textbox"
            TextWrapping="Wrap">

            <!--Attach SfSpellChecker to the TextBox-->
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
```

### Step 3: Add Code-Behind for Spell Check Button

```csharp
private void SpellCheck_ButtonClick(object sender, RoutedEventArgs e)
{
    spellChecker.PerformSpellCheckUsingDialog();
}
```

This opens the spell checker dialog that highlights error words with suggestions.

## Programmatic Setup

You can also create and attach the spell checker programmatically:

```csharp
// Create spell checker instance
SfSpellChecker spellChecker = new SfSpellChecker();

// Enable spell check
spellChecker.EnableSpellCheck = true;

// Attach to TextBox
SfSpellChecker.SetSpellChecker(textbox, spellChecker);

// Later, perform spell check
spellChecker.PerformSpellCheckUsingDialog();
```

## Control Structure

The spell checker dialog displays:
- **Error word** highlighted in the text editor
- **Suggestions list** with recommended corrections
- **Action buttons**: Change, Change All, Ignore, Ignore All, Add to Dictionary
- **Navigation buttons**: Next error word

![SpellChecker Dialog Structure](../../../../../../../docs/gettingstarted-images/Control_Structure.png)

## First Spell Check

When you run the application and click "Spell Check":

1. The SfSpellChecker dialog opens
2. Error words are highlighted with red foreground
3. Click an error word to see suggestions
4. Double-click a suggestion to replace, or use the action buttons

![First Spell Check](../../../../../../../docs/gettingstarted-images/Getting_Started.png)

## Context Menu Spell Check

Enable context menu for quick corrections without opening the dialog:

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    EnableContextMenu="True"/>
```

**Usage:**
1. Error words are underlined in red
2. Right-click on an error word
3. Select a suggestion from the context menu
4. The word is replaced instantly

```csharp
// Programmatic setup
spellChecker.EnableContextMenu = true;
spellChecker.EnableSpellCheck = true;
```

![Context Menu Spell Check](../../../../../../../docs/gettingstarted-images/contextmenu.gif)

## Enable or Disable Spell Checking

Control spell checking with the `EnableSpellCheck` property:

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="False"/>
```

```csharp
// Disable spell checking
spellChecker.EnableSpellCheck = false;

// Enable spell checking
spellChecker.EnableSpellCheck = true;
```

**When disabled:**
- Context menu suggestions won't appear
- Spell check dialog won't perform checking
- Error highlighting is removed

## Getting Suggestions Programmatically

You can retrieve suggestions for any word programmatically:

### GetSuggestions Method

Returns a list of suggested corrections:

```csharp
string errorWord = "Natusre";
List<string> suggestions = spellChecker.GetSuggestions(errorWord);

foreach (string suggestion in suggestions)
{
    Console.WriteLine(suggestion); // "Nature", "Capture", etc.
}
```

### GetPhoneticWords Method

Returns phonetically similar words:

```csharp
string word = "Natusre";
List<string> phoneticWords = spellChecker.GetPhoneticWords(word);
```

### GetAnagrams Method

Returns anagram suggestions:

```csharp
string word = "listen";
List<string> anagrams = spellChecker.GetAnagrams(word); // "silent", "enlist"
```

## Checking Specific Words

You can check if a word is spelled correctly:

```csharp
string word = "Nature";
bool isCorrect = spellChecker.HasError(word); // Returns false (no error)

string errorWord = "Natusre";
bool hasError = spellChecker.HasError(errorWord); // Returns true (error)
```

## Common Setup Patterns

### Pattern 1: Dialog-Only

```csharp
SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.EnableSpellCheck = true;
spellChecker.EnableContextMenu = false; // Disable context menu
SfSpellChecker.SetSpellChecker(textbox, spellChecker);
```

### Pattern 2: Context Menu-Only

```csharp
SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.EnableSpellCheck = true;
spellChecker.EnableContextMenu = true;
SfSpellChecker.SetSpellChecker(textbox, spellChecker);
// Don't call PerformSpellCheckUsingDialog - users right-click instead
```

### Pattern 3: Both Methods

```csharp
SfSpellChecker spellChecker = new SfSpellChecker();
spellChecker.EnableSpellCheck = true;
spellChecker.EnableContextMenu = true; // Right-click for quick fixes
SfSpellChecker.SetSpellChecker(textbox, spellChecker);
// Also provide button for full dialog experience
```

## Next Steps

- **Learn spell check methods:** Read [spell-check-methods.md](spell-check-methods.md)
- **Configure ignore options:** Read [ignore-options.md](ignore-options.md)
- **Add custom dictionaries:** Read [dictionary-support.md](dictionary-support.md)
- **Customize appearance:** Read [customization.md](customization.md)

## Sample Applications

View complete samples: [WPF SpellChecker Examples on GitHub](https://github.com/SyncfusionExamples/WPF-SpellChecker-examples/tree/master/Samples/SfSpellChecker)

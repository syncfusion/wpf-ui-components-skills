# Spell Check Methods

SfSpellChecker provides two primary methods for spell checking: dialog-based and context menu-based. This guide covers both approaches in detail.

## Dialog-Based Spell Checking

The dialog provides a comprehensive spell checking interface with full control over corrections.

### Opening the Dialog

Use the `PerformSpellCheckUsingDialog()` method:

```csharp
private void SpellCheck_ButtonClick(object sender, RoutedEventArgs e)
{
    spellChecker.PerformSpellCheckUsingDialog();
}
```

**Complete Example:**

```xml
<Grid>
    <StackPanel>
        <TextBox 
            Text="Ths is a sampel text with speling errors."
            Name="textbox"
            TextWrapping="Wrap">
            
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

### Dialog Features

The dialog provides:

1. **Error Word Display** - Current error word highlighted in red
2. **Suggestions ListBox** - Recommended corrections ranked by relevance
3. **Replace Operations** - Change current or all instances
4. **Ignore Operations** - Skip current or all instances
5. **Add to Dictionary** - Add word to custom dictionary
6. **Navigation** - Move to next/previous error

### Dialog Actions

#### Change (Replace)

Replace the current error word with selected suggestion:

```
1. Error word appears in red
2. Select a suggestion from the list
3. Click "Change" button
4. Word is replaced and moves to next error
```

**Double-click shortcut:** Double-click a suggestion to apply it instantly.

#### Change All (Replace All)

Replace all occurrences of the error word:

```
1. Select suggestion
2. Click "Change All"
3. All instances replaced throughout the text
```

#### Ignore

Skip the current instance:

```
1. Click "Ignore"
2. Moves to next error
3. This instance remains unchanged
```

#### Ignore All

Skip all occurrences of this word in the current session:

```
1. Click "Ignore All"
2. All instances of this word are ignored
3. Spell checker moves to next different error
```

#### Add to Dictionary

Add the word to custom dictionary so it's no longer flagged:

```
1. Click "Add to Dictionary"
2. Word is added to custom dictionary
3. Won't be flagged as error in future sessions
```

**Note:** Requires CustomDictionary configured in Dictionaries collection.

### Error Word Highlighting

Error words in the dialog text editor:
- **Red foreground color** - Indicates spelling error
- **Clickable** - Click to see suggestions
- **Auto-navigation** - Dialog automatically highlights next error

## Context Menu-Based Spell Checking

Provides quick in-place corrections without opening a dialog.

### Enabling Context Menu

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    EnableContextMenu="True"/>
```

```csharp
// Programmatic setup
spellChecker.EnableContextMenu = true;
spellChecker.EnableSpellCheck = true;
```

### Context Menu Features

**Visual Indicators:**
- Error words underlined in red
- Right-click to open context menu
- Top items are correction suggestions
- Bottom items are actions

**Menu Items:**
1. **Suggestions** (top 5-10) - Click to replace
2. **Ignore All** - Ignore all instances
3. **Add to Dictionary** - Add to custom dictionary

### Using Context Menu

**Step-by-step:**

```
1. Type text with errors (errors underlined in red)
2. Right-click on underlined word
3. Context menu appears with suggestions
4. Click a suggestion to replace the word
   OR
   Click "Ignore All" to skip this word
   OR
   Click "Add to Dictionary" to add word permanently
```

**Example:**

```xml
<TextBox 
    Text="Natusre is an importsant part of mankind."
    Name="textbox"
    TextWrapping="Wrap"
    VerticalContentAlignment="Top">
    
    <syncfusion:SfSpellChecker.SpellChecker>
        <syncfusion:SfSpellChecker 
            x:Name="spellChecker"
            EnableContextMenu="True"
            EnableSpellCheck="True"/>
    </syncfusion:SfSpellChecker.SpellChecker>
</TextBox>
```

![Context Menu Usage](../../../../../../../docs/gettingstarted-images/contextmenu.gif)

### Disabling Context Menu

To disable context menu while keeping dialog available:

```csharp
spellChecker.EnableContextMenu = false;
spellChecker.EnableSpellCheck = true; // Dialog still works
```

## Choosing Between Methods

### Use Dialog When:
- Performing comprehensive spell check of entire document
- Need to review all errors systematically
- Want full visibility of all suggestions
- User prefers batch correction workflow
- Application has dedicated "Spell Check" action

### Use Context Menu When:
- User wants quick inline corrections
- Real-time error correction during typing
- Minimalist UI preferred
- Faster workflow for experienced users
- Mobile or touch-friendly interface

### Use Both When:
- Provide maximum flexibility
- Support different user preferences
- Power users want context menu, beginners want dialog
- Complex documents benefit from both approaches

## Programmatic Spell Checking

You can perform spell checking programmatically without UI:

### Check if Word Has Error

```csharp
bool hasError = spellChecker.HasError("Natusre"); // Returns true
```

### Get Suggestions for Word

```csharp
string errorWord = "importsant";
List<string> suggestions = spellChecker.GetSuggestions(errorWord);
// Returns: ["important", "importance", "importantly", ...]

// Use first suggestion
if (suggestions.Count > 0)
{
    string correction = suggestions[0];
    // Apply correction programmatically
}
```

### Check All Words in Text

```csharp
string text = textbox.Text;
string[] words = text.Split(' ', '\n', '\r', '\t');

foreach (string word in words)
{
    if (spellChecker.HasError(word))
    {
        List<string> suggestions = spellChecker.GetSuggestions(word);
        Console.WriteLine($"Error: {word}, Suggestions: {string.Join(", ", suggestions)}");
    }
}
```

## Performance Considerations

### Dialog Performance
- Loads all errors on open
- Good for documents up to 10,000 words
- For larger documents, consider processing in chunks

### Context Menu Performance
- On-demand suggestion loading
- Only checks visible text
- Better for real-time checking
- Minimal performance impact

## Common Patterns

### Pattern 1: On-Demand Dialog Check

```csharp
// Show spell check button only when errors exist
private void UpdateSpellCheckButton()
{
    bool hasErrors = CheckForErrors();
    spellCheckButton.IsEnabled = hasErrors;
}

private bool CheckForErrors()
{
    string[] words = textbox.Text.Split(' ');
    return words.Any(word => spellChecker.HasError(word));
}
```

### Pattern 2: Auto-Save Before Spell Check

```csharp
private void SpellCheck_ButtonClick(object sender, RoutedEventArgs e)
{
    // Save current content
    SaveDocument();
    
    // Perform spell check
    spellChecker.PerformSpellCheckUsingDialog();
}
```

### Pattern 3: Keyboard Shortcut for Dialog

```csharp
private void Window_KeyDown(object sender, KeyEventArgs e)
{
    if (e.Key == Key.F7)
    {
        spellChecker.PerformSpellCheckUsingDialog();
        e.Handled = true;
    }
}
```

## Troubleshooting

### Context Menu Not Appearing

**Issue:** Right-clicking doesn't show context menu

**Solutions:**
- Verify `EnableContextMenu = true`
- Verify `EnableSpellCheck = true`
- Check that TextBox has spell checker attached
- Ensure error words exist in text

### Dialog Shows No Errors

**Issue:** Dialog opens but shows "Spell check is completed"

**Solutions:**
- Check if text contains actual errors
- Verify dictionary is loaded correctly
- Check ignore options aren't too broad
- Ensure `EnableSpellCheck = true`

### Suggestions Not Accurate

**Issue:** Suggestions don't match expected words

**Solutions:**
- Verify correct dictionary culture loaded
- Check if custom dictionary is interfering
- Consider using different dictionary format
- Add custom words to improve accuracy

## Next Steps

- **Configure ignore options:** Read [ignore-options.md](ignore-options.md)
- **Add language dictionaries:** Read [dictionary-support.md](dictionary-support.md)
- **Handle events:** Read [customization.md](customization.md)

---
name: syncfusion-wpf-syntax-editor
description: Implement Syncfusion WPF Syntax Editor (EditControl) for code editors and advanced text editing applications. Use this when building syntax-highlighted code editors, IDE-like editors, or source code editing interfaces in WPF. Covers syntax highlighting, IntelliSense, find/replace, programming language support, and Visual Studio-style editing features.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syntax Editor

Guide for implementing Syncfusion® WPF Syntax Editor (EditControl) — an advanced code and text editor control for creating IDE-like editing experiences with syntax highlighting, IntelliSense, find/replace, and comprehensive editing features for multiple programming languages.

## When to Use This Skill

Use this skill when you need to:
- **Create code editors** with syntax highlighting for C#, VB, XAML, XML, and custom languages
- **Build IDE-like applications** with advanced text editing capabilities
- **Implement IntelliSense** with auto-completion and code suggestions
- **Add text editors** to applications with find/replace, line numbers, and folding
- **Support multiple file formats** with syntax-aware editing
- **Create custom language editors** with user-defined syntax rules
- **Implement advanced editing** features like drag-drop, undo/redo, printing
- **Build source code viewers** or editors for development tools
- **Create log file viewers** with syntax highlighting
- **Implement configuration file editors** with validation

This skill covers the complete Syntax Editor implementation including basic setup, language configuration, editing features, customization, and advanced scenarios.

## Component Overview

The **Syntax Editor** (EditControl) is a high-performance text and code editor control that provides:

- **Syntax Highlighting** - Built-in support for C#, VB.NET, XAML, XML, and custom languages
- **IntelliSense** - Auto-completion, parameter info, and code suggestions
- **File Operations** - Open, save, new file with encoding support
- **Advanced Editing** - Multi-level undo/redo, clipboard operations, drag-drop
- **Find and Replace** - Search with regex, match case, whole word
- **Text Navigation** - Go to line, bookmarks, scroll synchronization
- **Line Numbers** - Display with customizable appearance
- **Code Folding** - Expand/collapse code blocks
- **Context Menu** - Fully customizable with built-in commands
- **Status Bar** - Line/column position, insert/overwrite mode
- **Printing** - Print preview and formatted output
- **Customization** - Fonts, colors, backgrounds, margins
- **Read-only Mode** - View-only with protection
- **Single-line Mode** - Use as enhanced TextBox
- **Custom Languages** - Define your own syntax rules

### Control Structure

```
EditControl
├── Editor Area - Main text editing surface
│   ├── Line Numbers (optional)
│   ├── Folding Indicators (optional)
│   └── Text Content with Syntax Highlighting
├── Context Menu - Right-click operations
├── Status Bar (optional) - Position and mode info
└── IntelliSense Popup - Auto-completion suggestions
```

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and dependencies
- Adding EditControl via designer, XAML, and C#
- Loading files into the editor
- Basic configuration
- First code editor example

📄 **Read:** [references/overview.md](references/overview.md)
- Component introduction
- Key features overview
- Real-world scenarios
- Architecture and design
- Performance considerations

📄 **Read:** [references/file-operations.md](references/file-operations.md)
- Opening files (Stream, file path)
- Saving files with encoding
- Creating new documents
- File type detection
- Encoding support (UTF-8, ASCII, Unicode)
- Recent files management

### Syntax and Language Support

📄 **Read:** [references/syntax-highlighting.md](references/syntax-highlighting.md)
- Enabling syntax highlighting
- Built-in language support
- Configuring language-specific settings
- Syntax highlighting customization
- Color schemes and themes
- Token-based highlighting

📄 **Read:** [references/supported-languages.md](references/supported-languages.md)
- C# language support
- Visual Basic support
- XAML language support
- XML language support
- SQL language support
- Language switching at runtime

📄 **Read:** [references/custom-language-support.md](references/custom-language-support.md)
- Creating custom language configurations
- Language base classes (LanguageBase, ProceduralLanguageBase, MarkupLanguageBase)
- Defining lexical states
- Token definitions and patterns
- Custom syntax rules
- Language inheritance

📄 **Read:** [references/intellisense.md](references/intellisense.md)
- Enabling IntelliSense features
- Auto-completion lists
- Parameter info tooltips
- Custom IntelliSense data sources
- Triggering IntelliSense programmatically
- IntelliSense appearance customization

### Editing Features

📄 **Read:** [references/editing-text.md](references/editing-text.md)
- Basic text editing operations
- Programmatic text manipulation
- Text insertion and deletion
- Document navigation
- Text properties and methods
- Line-based operations

📄 **Read:** [references/edit-commands.md](references/edit-commands.md)
- Built-in editing commands
- Undo/Redo operations
- Clipboard operations (Cut, Copy, Paste)
- Select All, Delete
- RoutedUICommands usage
- Custom command implementation
- Command bindings

📄 **Read:** [references/selection.md](references/selection.md)
- Text selection methods
- Selection properties (SelectedText, SelectionStart, SelectionLength)
- Programmatic selection
- Selection appearance customization
- Multi-line selection handling

📄 **Read:** [references/find-and-replace.md](references/find-and-replace.md)
- Find text functionality
- Replace operations
- Find and replace dialog
- Search options (match case, whole word, regex)
- Programmatic search API
- Search result navigation

📄 **Read:** [references/text-navigation.md](references/text-navigation.md)
- Go to line functionality
- Scroll to position
- Bookmark support
- Keyboard navigation
- Line and column position tracking
- Viewport management

### UI Features

📄 **Read:** [references/line-numbers.md](references/line-numbers.md)
- Enabling line numbers
- Line number appearance customization
- Line number margin width
- Line highlighting
- Current line indicator

📄 **Read:** [references/expand-collapse.md](references/expand-collapse.md)
- Code folding regions
- Expand/collapse functionality
- Outlining configuration
- Custom folding rules
- Collapse indicators appearance
- Programmatic expand/collapse

📄 **Read:** [references/context-menu.md](references/context-menu.md)
- Default context menu
- Context menu customization
- Adding custom menu items
- Menu command bindings
- Disabling context menu

📄 **Read:** [references/status-bar.md](references/status-bar.md)
- Enabling status bar
- Line and column position display
- Insert/Overwrite mode indicator
- Custom status bar content
- Status bar appearance

### Customization and Advanced Features

📄 **Read:** [references/customization.md](references/customization.md)
- Font and color customization
- Background customization
- Line background customization
- Margin and padding settings
- Cursor appearance
- Scroll bar customization
- Visual styles and themes

📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Drag and drop text editing
- Read-only mode
- Single-line mode (enhanced TextBox)
- Printing and print preview
- Split view support
- Performance optimization
- Memory management

## Quick Start Example

### Basic Code Editor Setup

```xml
<Window x:Class="CodeEditorApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <!-- Toolbar -->
        <ToolBar Grid.Row="0">
            <Button Content="Open" Click="OpenFile_Click"/>
            <Button Content="Save" Click="SaveFile_Click"/>
            <Separator/>
            <ComboBox Name="languageComboBox" 
                      SelectionChanged="Language_Changed"
                      Width="120">
                <ComboBoxItem Content="C#" Tag="CSharp"/>
                <ComboBoxItem Content="Visual Basic" Tag="VB"/>
                <ComboBoxItem Content="XAML" Tag="XAML"/>
                <ComboBoxItem Content="XML" Tag="XML"/>
            </ComboBox>
        </ToolBar>
        
        <!-- Syntax Editor -->
        <syncfusion:EditControl Name="editControl" 
                                Grid.Row="1"
                                DocumentLanguage="CSharp"
                                ShowLineNumber="True"
                                EnableOutlining="True"
                                EnableIntellisense="True"
                                Background="White"
                                FontSize="12"
                                FontFamily="Consolas"/>
    </Grid>
</Window>
```

```csharp
using Syncfusion.Windows.Edit;
using Microsoft.Win32;
using System.IO;
using System.Windows;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Set initial language
        editControl.DocumentLanguage = Languages.CSharp;
        languageComboBox.SelectedIndex = 0;
    }
    
    private void OpenFile_Click(object sender, RoutedEventArgs e)
    {
        OpenFileDialog dialog = new OpenFileDialog
        {
            Filter = "C# Files (*.cs)|*.cs|VB Files (*.vb)|*.vb|XAML Files (*.xaml)|*.xaml|All Files (*.*)|*.*"
        };
        
        if (dialog.ShowDialog() == true)
        {
            editControl.Load(dialog.FileName);
            
            // Auto-detect language from extension
            string ext = Path.GetExtension(dialog.FileName).ToLower();
            switch (ext)
            {
                case ".cs":
                    editControl.DocumentLanguage = Languages.CSharp;
                    break;
                case ".vb":
                    editControl.DocumentLanguage = Languages.VB;
                    break;
                case ".xaml":
                    editControl.DocumentLanguage = Languages.XAML;
                    break;
                case ".xml":
                    editControl.DocumentLanguage = Languages.XML;
                    break;
            }
        }
    }
    
    private void SaveFile_Click(object sender, RoutedEventArgs e)
    {
        SaveFileDialog dialog = new SaveFileDialog
        {
            Filter = "C# Files (*.cs)|*.cs|VB Files (*.vb)|*.vb|XAML Files (*.xaml)|*.xaml|All Files (*.*)|*.*"
        };
        
        if (dialog.ShowDialog() == true)
        {
            editControl.Save(dialog.FileName);
        }
    }
    
    private void Language_Changed(object sender, SelectionChangedEventArgs e)
    {
        if (editControl == null) return;
        
        var selected = (ComboBoxItem)languageComboBox.SelectedItem;
        string language = selected.Tag.ToString();
        
        switch (language)
        {
            case "CSharp":
                editControl.DocumentLanguage = Languages.CSharp;
                break;
            case "VB":
                editControl.DocumentLanguage = Languages.VB;
                break;
            case "XAML":
                editControl.DocumentLanguage = Languages.XAML;
                break;
            case "XML":
                editControl.DocumentLanguage = Languages.XML;
                break;
        }
    }
}
```

## Common Patterns

### Pattern 1: Load and Save Files with Encoding

```csharp
// Load file with specific encoding
editControl.Load("path/to/file.cs", Encoding.UTF8);

// Save with encoding
editControl.Save("path/to/output.cs", Encoding.UTF8);

// Load from stream
using (FileStream stream = new FileStream("file.cs", FileMode.Open))
{
    editControl.Load(stream, Encoding.UTF8);
}

// Save to stream
using (FileStream stream = new FileStream("output.cs", FileMode.Create))
{
    editControl.Save(stream, Encoding.UTF8);
}
```

### Pattern 2: Custom IntelliSense with Auto-Completion

```csharp
// Enable IntelliSense in Custom mode
editControl.EnableIntellisense = true;
editControl.IntellisenseMode = IntellisenseMode.Custom;

// Business object must implement IIntellisenseItem
public class MyIntellisenseItem : IIntellisenseItem
{
    public ImageSource Icon { get; set; }
    public string Text { get; set; }
    public IEnumerable<IIntellisenseItem> NestedItems { get; set; }
}

// Build custom items collection and assign to IntellisenseCustomItemsSource
var customItems = new ObservableCollection<MyIntellisenseItem>
{
    new MyIntellisenseItem { Text = "CustomMethod" },
    new MyIntellisenseItem { Text = "CustomProperty" },
    new MyIntellisenseItem { Text = "Console" }
};
editControl.IntellisenseCustomItemsSource = customItems;

// Handle IntelliSenseBoxOpening event to filter items at runtime
editControl.IntelliSenseBoxOpening += (s, e) =>
{
    // e.ItemsSource contains current items; set e.Cancel = true to suppress popup
    if (string.IsNullOrEmpty(editControl.Text))
        e.Cancel = true;
};
```

### Pattern 3: Find and Replace with Regex

```csharp
// Simple find
FindResult result = editControl.FindText("SearchTerm", 
    FindOptions.MatchCase | FindOptions.WholeWord);

if (result != null)
{
    Console.WriteLine($"Found at line {result.LineNumber}");
}

// Find with regex
FindResult regexResult = editControl.FindText(@"\d{3}-\d{4}", 
    FindOptions.UseRegularExpressions);

// Replace all occurrences
int replacedCount = editControl.ReplaceAll("oldText", "newText", 
    FindOptions.MatchCase);

// Replace with confirmation
while (editControl.FindNext("oldText", FindOptions.None))
{
    var dialogResult = MessageBox.Show("Replace this occurrence?", 
        "Confirm", MessageBoxButton.YesNoCancel);
    
    if (dialogResult == MessageBoxResult.Yes)
        editControl.SelectedText = "newText";
    else if (dialogResult == MessageBoxResult.Cancel)
        break;
}
```

### Pattern 4: Custom Language Definition

Define formats and lexems as XAML resources, then apply them to a class that inherits `ProceduralLanguageBase`:

```xml
<!-- Define formats in XAML Resources -->
<syncfusion:FormatsCollection x:Key="myLangFormats">
    <syncfusion:EditFormats Foreground="Blue"  FormatName="KeywordFormat"/>
    <syncfusion:EditFormats Foreground="Green" FormatName="CommentFormat"/>
    <syncfusion:EditFormats Foreground="Brown" FormatName="StringFormat"/>
</syncfusion:FormatsCollection>

<!-- Define lexems in XAML Resources -->
<syncfusion:LexemCollection x:Key="myLangLexems">
    <syncfusion:Lexem StartText="function" LexemType="Keyword"  FormatName="KeywordFormat" ContainsEndText="False" IsMultiline="False"/>
    <syncfusion:Lexem StartText="if"       LexemType="Keyword"  FormatName="KeywordFormat" ContainsEndText="False" IsMultiline="False"/>
    <syncfusion:Lexem StartText="while"    LexemType="Keyword"  FormatName="KeywordFormat" ContainsEndText="False" IsMultiline="False"/>
    <syncfusion:Lexem StartText="//"       EndText="\r\n" LexemType="Comment" FormatName="CommentFormat" ContainsEndText="True" IsMultiline="False"/>
    <syncfusion:Lexem StartText="&quot;"  EndText="&quot;" LexemType="Literals" FormatName="StringFormat" ContainsEndText="True" IsMultiline="False"/>
</syncfusion:LexemCollection>
```

```csharp
// Create custom language class inheriting ProceduralLanguageBase
public class MyLanguage : ProceduralLanguageBase
{
    public MyLanguage(EditControl control) : base(control)
    {
        Name = "MyLanguage";
        FileExtension = "mylang";
        ApplyColoring = true;
        SupportsOutlining = false;
        SupportsIntellisense = false;
        TextForeground = Brushes.Black;
    }
}

// Wire up resources and apply the custom language
EditControl editControl = new EditControl();
editControl.DocumentLanguage = Languages.Custom;

MyLanguage customLanguage = new MyLanguage(editControl);
customLanguage.Lexem    = Application.Current.Resources["myLangLexems"]   as LexemCollection;
customLanguage.Formats  = Application.Current.Resources["myLangFormats"]  as FormatsCollection;
editControl.CustomLanguage = customLanguage;
```

### Pattern 5: Code Folding and Outlining

```csharp
// Enable outlining
editControl.EnableOutlining = true;

// Collapse all regions
editControl.CollapseAll();

// Expand all regions
editControl.ExpandAll();

// Toggle specific region
editControl.ToggleOutlining(lineNumber);

// Programmatically add folding region
editControl.AddFoldingRegion(startLine, endLine, "Region Name");

// Handle outlining events
editControl.OutlineCollapsed += (s, e) =>
{
    Console.WriteLine($"Collapsed region at line {e.StartLine}");
};
```

## Key Properties

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Gets/sets the entire text content |
| **DocumentLanguage** | ILanguage | Current language for syntax highlighting |
| **ShowLineNumber** | bool | Shows/hides line numbers |
| **EnableIntellisense** | bool | Enables IntelliSense features |
| **EnableOutlining** | bool | Enables code folding |
| **IsReadOnly** | bool | Makes editor read-only |
| **SingleLineMode** | bool | Uses editor as single-line TextBox |

### Selection Properties

| Property | Type | Description |
|----------|------|-------------|
| **SelectedText** | string | Gets/sets currently selected text |
| **SelectionStart** | int | Selection start position |
| **SelectionLength** | int | Length of selection |
| **CurrentLine** | int | Current cursor line number |
| **CurrentColumn** | int | Current cursor column position |

### Appearance Properties

| Property | Type | Description |
|----------|------|-------------|
| **FontFamily** | FontFamily | Editor font |
| **FontSize** | double | Editor font size |
| **Background** | Brush | Editor background |
| **Foreground** | Brush | Default text color |
| **LineNumberForeground** | Brush | Line number color |
| **SelectionBrush** | Brush | Selection background color |

### Behavior Properties

| Property | Type | Description |
|----------|------|-------------|
| **AllowDragDrop** | bool | Enables text drag-drop |
| **WordWrap** | bool | Enables word wrapping |
| **TabSize** | int | Tab character width |
| **UseSpacesInsteadOfTabs** | bool | Converts tabs to spaces |
| **AutoIndent** | bool | Automatic indentation |

### Events

| Event | Description |
|-------|-------------|
| **TextChanged** | Fired when text content changes |
| **SelectionChanged** | Fired when selection changes |
| **DocumentLanguageChanged** | Fired when language changes |
| **IntellisenseOpening** | Fired before showing IntelliSense popup |
| **OutlineCollapsed** | Fired when outline region collapses |
| **OutlineExpanded** | Fired when outline region expands |

## Common Use Cases

### 1. Source Code Editor for IDEs
Build development environments with syntax highlighting, IntelliSense, and debugging integration.

### 2. Configuration File Editor
Create editors for XML, JSON, YAML configuration files with validation and auto-completion.

### 3. Log File Viewer
Display and analyze log files with syntax highlighting for different log levels and patterns.

### 4. SQL Query Editor
Build database query tools with SQL syntax highlighting and query execution.

### 5. Script Editor
Create scripting environments for PowerShell, Python, or custom scripting languages.

### 6. Markdown Editor
Implement markdown editors with live preview and syntax highlighting.

### 7. Template Editor
Build template editing tools for code generation or document templates.

### 8. Educational Coding Tools
Create learning applications with code highlighting and interactive examples.

## Best Practices

1. **Language Selection**
   - Set `DocumentLanguage` based on file extension
   - Provide UI for manual language switching
   - Support language-specific configurations

2. **Performance**
   - Use `BeginUpdate()`/`EndUpdate()` for bulk text operations
   - Enable virtualization for large files
   - Consider read-only mode for view-only scenarios

3. **User Experience**
   - Always show line numbers for code editing
   - Enable IntelliSense for better productivity
   - Provide find/replace functionality
   - Support standard keyboard shortcuts

4. **File Operations**
   - Always specify encoding when loading/saving
   - Handle encoding detection for existing files
   - Implement auto-save or prompt for unsaved changes

5. **Customization**
   - Use themes for consistent appearance
   - Allow users to customize fonts and colors
   - Provide syntax highlighting customization options

6. **Accessibility**
   - Support keyboard navigation
   - Provide screen reader compatibility
   - Ensure sufficient color contrast

## Troubleshooting

### Syntax Highlighting Not Working
- Verify `DocumentLanguage` is set correctly
- Check if language is properly initialized
- Ensure custom language definitions are complete

### IntelliSense Not Appearing
- Verify `EnableIntellisense` is `true`
- Check `IntellisenseMode` setting
- Ensure IntelliSense items are populated

### Performance Issues with Large Files
- Enable virtualization
- Use `BeginUpdate()`/`EndUpdate()` for batch operations
- Consider pagination or lazy loading for very large files

### Line Numbers Not Showing
- Verify `ShowLineNumber` is `true`
- Check if line number margin has sufficient width
- Ensure visual style doesn't hide line numbers

---

**Next Steps:** Navigate to specific reference documents based on your implementation needs. Start with [getting-started.md](references/getting-started.md) for initial setup, then explore syntax highlighting, IntelliSense, and editing features as needed.

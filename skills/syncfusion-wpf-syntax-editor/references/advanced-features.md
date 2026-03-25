# Advanced Features in WPF Syntax Editor

This guide covers advanced features and modes in the Syncfusion WPF Syntax Editor (EditControl) including printing, read-only mode, single-line mode, and drag-and-drop text editing.

## Table of Contents
- [Printing](#printing)
- [Read-Only Mode](#read-only-mode)
- [Single-Line Mode](#single-line-mode)
- [Drag-and-Drop Text Editing](#drag-and-drop-text-editing)
- [Best Practices](#best-practices)

## Printing

The EditControl provides comprehensive printing support with print preview, page setup, headers/footers, and customization options.

### Basic Printing

```csharp
// Direct print
editControl.Print();
```

```xml
<!-- Print button -->
<Button Content="Print" Click="PrintButton_Click"/>
```

```csharp
private void PrintButton_Click(object sender, RoutedEventArgs e)
{
    editControl.Print();
}
```

### Print Preview

Display a print preview dialog before printing:

```csharp
// Show print preview
editControl.ShowPrintPreview();
```

```xml
<!-- Print Preview button -->
<Button Content="Print Preview..." Click="PrintPreviewButton_Click"/>
```

```csharp
private void PrintPreviewButton_Click(object sender, RoutedEventArgs e)
{
    editControl.ShowPrintPreview();
}
```

The print preview window allows users to:
- Preview pages before printing
- Navigate between pages
- Adjust page orientation
- Change page size
- Modify margins
- Zoom in/out

### Print Settings

Configure printing options using the `PrintSettings` property:

```csharp
editControl.PrintSettings = new PrintSettings();

// Page orientation
editControl.PrintSettings.PageOrientation = PageOrientation.Portrait;
// or PageOrientation.Landscape

// Page size
editControl.PrintSettings.PageWidth = 800;
editControl.PrintSettings.PageHeight = 1000;

// Page margin
editControl.PrintSettings.PageMargin = new Thickness(50); // All sides
editControl.PrintSettings.PageMargin = new Thickness(50, 75, 50, 75); // Left, Top, Right, Bottom

editControl.Print();
```

### Page Orientation

Switch between portrait and landscape:

```csharp
// Portrait orientation (default)
editControl.PrintSettings = new PrintSettings();
editControl.PrintSettings.PageOrientation = PageOrientation.Portrait;
editControl.Print();

// Landscape orientation
editControl.PrintSettings = new PrintSettings();
editControl.PrintSettings.PageOrientation = PageOrientation.Landscape;
editControl.Print();
```

Users can also change orientation in the print preview dialog using the orientation dropdown.

### Custom Page Size

Set custom page dimensions:

```csharp
editControl.PrintSettings = new PrintSettings();
editControl.PrintSettings.PageHeight = 800;
editControl.PrintSettings.PageWidth = 800;
editControl.Print();
```

Users can select from predefined page sizes or enter custom dimensions in the print preview dialog.

### Page Margins

Adjust content margins:

```csharp
editControl.PrintSettings = new PrintSettings();
editControl.PrintSettings.PageMargin = new Thickness(5);
editControl.Print();
```

Users can select from predefined margins or enter custom margins in the print preview dialog.

### Headers and Footers

Add custom headers and footers to printed pages:

**Step 1: Define Header Template**

```xml
<Application.Resources>
    <DataTemplate x:Key="pageHeaderTemplate">
        <Grid Background="Gray">
            <TextBlock Text="Syncfusion" 
                       FontSize="18" 
                       FontWeight="Bold" 
                       Foreground="White" 
                       HorizontalAlignment="Center"/>
        </Grid>
    </DataTemplate>
</Application.Resources>
```

**Step 2: Apply Header Template**

```csharp
editControl.PrintSettings = new PrintSettings();
editControl.PrintSettings.PageHeaderHeight = 30;
editControl.PrintSettings.PageHeaderTemplate = 
    Application.Current.Resources["pageHeaderTemplate"] as DataTemplate;
editControl.ShowPrintPreview();
```

**Footer with Date and Page Number**

```xml
<Application.Resources>
    <DataTemplate x:Key="pageFooterTemplate">
        <Grid>
            <!-- Current Date/Time -->
            <TextBlock HorizontalAlignment="Center" 
                       FontSize="20" 
                       Text="{Binding Source={x:Static system:DateTime.Now}}"/>
            
            <!-- Page Number -->
            <TextBlock Margin="0,0,10,0"
                       HorizontalAlignment="Right"
                       VerticalAlignment="Center" 
                       FontSize="20">
                <TextBlock.Text>
                    <Binding Path="PageIndex"
                             RelativeSource="{RelativeSource Mode=FindAncestor,
                                                              AncestorType={x:Type syncfusion:PrintPageControl}}"
                             StringFormat="Page : {0}" />
                </TextBlock.Text>
            </TextBlock>
        </Grid>
    </DataTemplate>
</Application.Resources>
```

```csharp
editControl.PrintSettings = new PrintSettings();
editControl.PrintSettings.PageFooterHeight = 30;
editControl.PrintSettings.PageFooterTemplate = 
    Application.Current.Resources["pageFooterTemplate"] as DataTemplate;
editControl.ShowPrintPreview();
```

### Complete Printing Example

```csharp
public class PrintHelper
{
    public static void PrintWithCustomSettings(EditControl editor)
    {
        editor.PrintSettings = new PrintSettings
        {
            // Orientation
            PageOrientation = PageOrientation.Portrait,
            
            // Page size (A4: 8.27 x 11.69 inches)
            PageWidth = 816,
            PageHeight = 1056,
            
            // Margins
            PageMargin = new Thickness(72, 72, 72, 72), // 1 inch margins
            
            // Header
            PageHeaderHeight = 40,
            PageHeaderTemplate = Application.Current.Resources["headerTemplate"] as DataTemplate,
            
            // Footer
            PageFooterHeight = 30,
            PageFooterTemplate = Application.Current.Resources["footerTemplate"] as DataTemplate
        };
        
        editor.Print();
    }
    
    public static void PrintPreviewWithDefaults(EditControl editor)
    {
        editor.PrintSettings = new PrintSettings();
        editor.ShowPrintPreview();
    }
}
```

## Read-Only Mode

Read-only mode allows users to view and navigate code without editing capabilities.

### Enable Read-Only Mode

```xml
<syncfusion:EditControl Name="editControl"
                        IsReadOnly="True"/>
```

```csharp
// Enable read-only mode
editControl.IsReadOnly = true;

// Disable read-only mode
editControl.IsReadOnly = false;
```

### Use Cases for Read-Only Mode

1. **Code Viewer**
   - Display code samples
   - Show generated code
   - View file diff comparisons

2. **Documentation Browser**
   - Display API documentation
   - Show code examples
   - View help content

3. **Code Review**
   - Display code for review
   - Show historical versions
   - Compare file versions

4. **Protected Content**
   - Display license files
   - Show system configuration
   - View log files

### Toggle Read-Only Mode

```xml
<CheckBox Content="Read-Only Mode" 
          IsChecked="{Binding ElementName=editControl, Path=IsReadOnly}"/>
```

```csharp
private void ToggleReadOnlyButton_Click(object sender, RoutedEventArgs e)
{
    editControl.IsReadOnly = !editControl.IsReadOnly;
    
    // Update UI
    statusLabel.Content = editControl.IsReadOnly 
        ? "Read-Only Mode" 
        : "Edit Mode";
}
```

### Complete Read-Only Example

```csharp
public class CodeViewer
{
    private EditControl editor;
    
    public void ShowCode(string filePath, bool readOnly = true)
    {
        // Load file
        editor.Load(filePath);
        
        // Set language based on extension
        string extension = Path.GetExtension(filePath);
        switch (extension.ToLower())
        {
            case ".cs":
                editor.DocumentLanguage = Languages.CSharp;
                break;
            case ".vb":
                editor.DocumentLanguage = Languages.VisualBasic;
                break;
            case ".xml":
            case ".xaml":
                editor.DocumentLanguage = Languages.XML;
                break;
        }
        
        // Enable read-only mode
        editor.IsReadOnly = readOnly;
        
        // Optional: Hide modification indicators
        editor.ShowLineNumber = true;
    }
}
```

## Single-Line Mode

Single-line mode transforms the EditControl into a single-line text input, similar to a TextBox.

### Enable Single-Line Mode

```xml
<syncfusion:EditControl Name="editControl"
                        IsMultiLine="False"/>
```

```csharp
// Enable single-line mode
editControl.IsMultiLine = false;

// Enable multi-line mode (default)
editControl.IsMultiLine = true;
```

### Use Cases for Single-Line Mode

1. **Expression Editor**
   - Mathematical expressions
   - Formula input
   - Query builders

2. **Code Snippets**
   - Single-line code input
   - Command line input
   - Script parameters

3. **Search/Replace Fields**
   - Find text input
   - Replace pattern input
   - Regular expression editor

4. **Configuration Values**
   - Path input
   - URL editor
   - Connection string editor

### Complete Single-Line Example

```xml
<StackPanel>
    <Label Content="Enter Expression:"/>
    <syncfusion:EditControl Name="expressionEditor"
                            IsMultiLine="False"
                            Height="30"
                            DocumentLanguage="CSharp"
                            Text="x * 2 + y"/>
    
    <Button Content="Evaluate" Click="EvaluateButton_Click"/>
</StackPanel>
```

```csharp
private void EvaluateButton_Click(object sender, RoutedEventArgs e)
{
    string expression = expressionEditor.Text;
    // Process single-line expression
    var result = EvaluateExpression(expression);
    resultLabel.Content = $"Result: {result}";
}
```

### Single-Line with Syntax Highlighting

```csharp
public class SingleLineCodeEditor
{
    public void ConfigureSingleLineEditor(EditControl editor)
    {
        // Enable single-line mode
        editor.IsMultiLine = false;
        
        // Set language for syntax highlighting
        editor.DocumentLanguage = Languages.CSharp;
        
        // Height for single line
        editor.Height = 30;
        
        // Hide unnecessary features
        editor.ShowLineNumber = false;
        
        // Handle Enter key to execute
        editor.PreviewKeyDown += (s, e) =>
        {
            if (e.Key == Key.Enter)
            {
                ExecuteCode(editor.Text);
                e.Handled = true;
            }
        };
    }
    
    private void ExecuteCode(string code)
    {
        // Execute single-line code
    }
}
```

## Drag-and-Drop Text Editing

Enable drag-and-drop functionality to move or copy text within the editor.

### Enable Drag-and-Drop

```xml
<syncfusion:EditControl Name="editControl"
                        AllowDragDrop="True"/>
```

```csharp
// Enable drag-and-drop
editControl.AllowDragDrop = true;

// Disable drag-and-drop
editControl.AllowDragDrop = false;
```

### Drag-and-Drop Behavior

**Move Operation (Default)**
- Select text
- Drag to new location
- Text is moved from source to destination

**Copy Operation**
- Select text
- Hold Ctrl key while dragging
- Text is copied to destination (original remains)

### Use Cases

1. **Code Refactoring**
   - Reorder methods
   - Move code blocks
   - Reorganize statements

2. **Text Manipulation**
   - Rearrange text content
   - Duplicate code sections
   - Quick code reordering

3. **Content Organization**
   - Reorganize documentation
   - Restructure configuration
   - Reorder list items

### Complete Drag-and-Drop Example

```csharp
public class EditorConfiguration
{
    public void ConfigureEditor(EditControl editor)
    {
        // Enable drag-and-drop
        editor.AllowDragDrop = true;
        
        // Track drag-and-drop operations
        editor.PreviewDragEnter += Editor_PreviewDragEnter;
        editor.PreviewDragOver += Editor_PreviewDragOver;
        editor.PreviewDrop += Editor_PreviewDrop;
    }
    
    private void Editor_PreviewDragEnter(object sender, DragEventArgs e)
    {
        // Check if text data is available
        if (e.Data.GetDataPresent(DataFormats.Text))
        {
            e.Effects = DragDropEffects.Move;
        }
        else
        {
            e.Effects = DragDropEffects.None;
        }
    }
    
    private void Editor_PreviewDragOver(object sender, DragEventArgs e)
    {
        // Show copy cursor when Ctrl is pressed
        if (e.KeyStates.HasFlag(DragDropKeyStates.ControlKey))
        {
            e.Effects = DragDropEffects.Copy;
        }
        else
        {
            e.Effects = DragDropEffects.Move;
        }
        
        e.Handled = true;
    }
    
    private void Editor_PreviewDrop(object sender, DragEventArgs e)
    {
        // Log the operation
        string operation = e.KeyStates.HasFlag(DragDropKeyStates.ControlKey) 
            ? "Copy" 
            : "Move";
        
        Console.WriteLine($"Drag-and-drop {operation} operation completed");
    }
}
```

### Keyboard Shortcuts for Drag-and-Drop

| Operation | Action |
|-----------|--------|
| Drag | Move selected text |
| Ctrl + Drag | Copy selected text |
| Esc (during drag) | Cancel operation |

## Best Practices

### Printing Best Practices

1. **Use Print Preview**
   - Always offer print preview to users
   - Allows verification before printing
   - Users can adjust settings

2. **Set Sensible Defaults**
   - Use standard page sizes (A4, Letter)
   - Apply reasonable margins (1 inch)
   - Use portrait for most code files

3. **Add Context Information**
   - Include filename in header
   - Add page numbers in footer
   - Show print date/time

4. **Consider Paper Size**
   - Long lines may require landscape
   - Adjust font size if needed
   - Test with typical content

### Read-Only Mode Best Practices

1. **Visual Indication**
   - Clearly indicate read-only state
   - Update UI labels/icons
   - Disable editing buttons

2. **Enable Navigation**
   - Allow scrolling and searching
   - Enable copy operations
   - Support find functionality

3. **Provide Exit Path**
   - Offer toggle to edit mode
   - Clear indication of mode
   - Save prompts if needed

### Single-Line Mode Best Practices

1. **Appropriate Height**
   - Set height to accommodate font size
   - Usually 25-35 pixels
   - Test with different fonts

2. **Hide Unnecessary Features**
   - Disable line numbers
   - Remove scrollbars if possible
   - Simplify UI

3. **Handle Enter Key**
   - Define Enter key behavior
   - Execute action or move focus
   - Provide clear feedback

### Drag-and-Drop Best Practices

1. **Clear Visual Feedback**
   - Show cursor changes (move/copy)
   - Highlight drop target
   - Indicate valid drop zones

2. **Support Both Operations**
   - Default drag = move
   - Ctrl + drag = copy
   - Document keyboard shortcuts

3. **Allow Cancellation**
   - Esc key cancels operation
   - Click outside cancels
   - Provide undo support

4. **Performance**
   - Efficient for large selections
   - Smooth drag operations
   - No lag during movement

## Related Topics

- [Getting Started](getting-started.md) - Basic setup and configuration
- [File Operations](file-operations.md) - Loading and saving files
- [Customization](customization.md) - Appearance and styling options
- [Editing Text](editing-text.md) - Text manipulation features

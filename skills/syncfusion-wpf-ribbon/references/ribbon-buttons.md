# Ribbon Buttons & Menus

## Table of Contents
- [RibbonButton Basics](#ribbonbutton-basics)
- [RibbonSplitButton](#ribbonsplitbutton)
- [SimpleMenuButton](#simplemenubuttton)
- [RibbonMenuItem](#ribbonmenuitem)
- [Nested Menus](#nested-menus)
- [Command Binding](#command-binding)
- [Button Styling](#button-styling)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## RibbonButton Basics

### Simple Button

Create a basic clickable button:

```xml
<syncfusion:RibbonButton Label="New" Click="OnNewClick" />
```

```csharp
private void OnNewClick(object sender, EventArgs e)
{
    // Handle click
    MessageBox.Show("New document created");
}
```

### Button with Icon

Add an image/icon to the button:

```xml
<syncfusion:RibbonButton Label="Save" 
                         SmallIcon="pack://application:,,,/Resources/save.png" />
```

### Button Sizing

Control button appearance with `SizeForm`:

```xml
<!-- Large button with icon and label -->
<syncfusion:RibbonButton Label="Bold" 
                         SizeForm="Large"
                         SmallIcon="pack://application:,,,/Resources/bold.png" />

<!-- Normal button (medium) -->
<syncfusion:RibbonButton Label="Italic" 
                         SizeForm="Normal"
                         SmallIcon="pack://application:,,,/Resources/italic.png" />

<!-- Small button (compact, label only) -->
<syncfusion:RibbonButton Label="Underline" 
                         SizeForm="Small" />
```

### Checkable Button (Toggle)

Create a button that stays pressed:

```xml
<syncfusion:RibbonButton Label="Show Grid" 
                         Click="OnToggleGridClick" />
```

```csharp
private void OnToggleGridClick(object sender, EventArgs e)
{
    var button = sender as RibbonButton;
    if (button.IsChecked.HasValue && button.IsChecked.Value)
    {
        // Grid is now visible
        ShowGrid();
    }
    else
    {
        // Grid is now hidden
        HideGrid();
    }
}
```

### Button with Tooltip

Add tooltip for discoverability:

```xml
<syncfusion:RibbonButton Label="Save" 
                         ToolTip="Save the document (Ctrl+S)" />
```

## RibbonSplitButton

### Basic Split Button

A button with a dropdown menu for related commands:

```xml
<syncfusion:SplitButton Label="Paste">
    <!-- Left click: Executes the command -->
    <!-- Right click/dropdown: Shows menu -->
    
    <syncfusion:RibbonMenuItem Header="Paste" Click="OnPasteClick" />
    <syncfusion:RibbonMenuItem Header="Paste Special" Click="OnPasteSpecialClick" />
    <syncfusion:MenuItemSeparator />
    <syncfusion:RibbonMenuItem Header="Paste Formatting" Click="OnPasteFormattingClick" />
</syncfusion:SplitButton>
```

### Split Button with Icons

Add icons to menu items:

```xml
<syncfusion:SplitButton Label="Format" SizeForm="Large">
    <syncfusion:RibbonMenuItem Header="Bold" 
                               Icon="pack://application:,,,/Resources/bold.png" />
    <syncfusion:RibbonMenuItem Header="Italic" 
                               Icon="pack://application:,,,/Resources/italic.png" />
    <syncfusion:RibbonMenuItem Header="Underline" 
                               Icon="pack://application:,,,/Resources/underline.png" />
</syncfusion:SplitButton>
```

### Split Button with Checkable Items

```xml
<syncfusion:SplitButton Label="Layout">
    <syncfusion:RibbonMenuItem Header="Grid Layout" IsCheckable="True" />
    <syncfusion:RibbonMenuItem Header="List Layout" IsCheckable="True" />
    <syncfusion:RibbonMenuItem Header="Card Layout" IsCheckable="True" />
</syncfusion:SplitButton>
```

## SimpleMenuButton

### Basic Menu Button

A button that opens a dropdown without a main command:

```xml
<syncfusion:MenuButton Label="Quick Access">
    <syncfusion:RibbonMenuItem Header="File Operations" />
    <syncfusion:RibbonMenuItem Header="Edit Commands" />
    <syncfusion:RibbonMenuItem Header="View Options" />
</syncfusion:MenuButton>
```

### Menu Button with Nested Items

```xml
<syncfusion:MenuButton Label="Styles">
    <!-- Heading Styles -->
    <syncfusion:RibbonMenuItem Header="Heading 1" />
    <syncfusion:RibbonMenuItem Header="Heading 2" />
    
    <syncfusion:MenuItemSeparator />
    
    <!-- Text Styles -->
    <syncfusion:RibbonMenuItem Header="Normal" />
    <syncfusion:RibbonMenuItem Header="Emphasis" />
</syncfusion:MenuButton>
```

## RibbonMenuItem

### Basic Menu Item

```xml
<syncfusion:RibbonMenuItem Header="Save" Click="OnSaveClick" />
```

### Menu Item with Icon

```xml
<syncfusion:RibbonMenuItem Header="Cut" 
                           Icon="pack://application:,,,/Resources/cut.png" />
```

### Menu Item with Checkable State

```xml
<syncfusion:RibbonMenuItem Header="Show Ruler" 
                           IsCheckable="True"
                           IsChecked="True" />
```

### Menu Item with Keyboard Shortcut Display

```xml
<syncfusion:RibbonMenuItem Header="Save" 
                           InputGestureText="Ctrl+S" />
```

## Nested Menus

### Multi-Level Hierarchy

Create submenus with nested `RibbonMenuItem`:

```xml
<syncfusion:RibbonTab Caption="Format">
    <syncfusion:RibbonBar Header="Styles">
        <syncfusion:MenuButton Label="Styles">
            <!-- Top level -->
            <syncfusion:RibbonMenuItem Header="Text Styles">
                <!-- Second level -->
                <syncfusion:RibbonMenuItem Header="Bold" />
                <syncfusion:RibbonMenuItem Header="Italic" />
                <syncfusion:RibbonMenuItem Header="Underline" />
            </syncfusion:RibbonMenuItem>
            
            <syncfusion:RibbonMenuItem Header="Paragraph Styles">
                <!-- Second level -->
                <syncfusion:RibbonMenuItem Header="Left Align" />
                <syncfusion:RibbonMenuItem Header="Center" />
                <syncfusion:RibbonMenuItem Header="Right Align" />
            </syncfusion:RibbonMenuItem>
            
            <syncfusion:MenuItemSeparator />
            
            <syncfusion:RibbonMenuItem Header="More Styles..." Click="OnMoreStylesClick" />
        </syncfusion:MenuButton>
    </syncfusion:RibbonBar>
</syncfusion:RibbonTab>
```

### Submenu with Commands

```xml
<syncfusion:RibbonMenuItem Header="Export">
    <syncfusion:RibbonMenuItem Header="Export as PDF" Click="OnExportPdfClick" />
    <syncfusion:RibbonMenuItem Header="Export as Word" Click="OnExportWordClick" />
    <syncfusion:RibbonMenuItem Header="Export as Excel" Click="OnExportExcelClick" />
    <syncfusion:MenuItemSeparator />
    <syncfusion:RibbonMenuItem Header="Export Settings..." Click="OnExportSettingsClick" />
</syncfusion:RibbonMenuItem>
```

## Command Binding

### Basic Command Binding

Bind buttons to view model commands for MVVM pattern:

```xml
<syncfusion:RibbonButton Label="Save" 
                         Command="{Binding SaveCommand}" />
```

```csharp
// ViewModel
public class DocumentViewModel : INotifyPropertyChanged
{
    public ICommand SaveCommand { get; }

    public DocumentViewModel()
    {
        SaveCommand = new RelayCommand(
            execute: () => Save(),
            canExecute: () => IsDirty
        );
    }

    private void Save()
    {
        // Save logic
        IsDirty = false;
    }

    private bool _isDirty;
    public bool IsDirty
    {
        get => _isDirty;
        set
        {
            if (_isDirty != value)
            {
                _isDirty = value;
                OnPropertyChanged(nameof(IsDirty));
                (SaveCommand as RelayCommand)?.RaiseCanExecuteChanged();
            }
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Command with Parameters

Pass data to commands:

```xml
<syncfusion:RibbonButton Label="Delete" 
                         Command="{Binding DeleteCommand}"
                         CommandParameter="SelectedItem" />
```

```csharp
public ICommand DeleteCommand { get; }

public DocumentViewModel()
{
    DeleteCommand = new RelayCommand<object>(
        execute: (param) => Delete(param),
        canExecute: (param) => param != null
    );
}

private void Delete(object item)
{
    // Delete logic
}
```

### Disabled Button Example

Command automatically disables button when `canExecute` returns false:

```xml
<!-- Button is disabled until document is modified -->
<syncfusion:RibbonButton Label="Save" 
                         Command="{Binding SaveCommand}" />
```

The button becomes disabled automatically when `IsDirty` is false.

## Button Styling

### Style Application

Define and apply custom button styles:

```xml
<!-- In Window.Resources -->
<Window.Resources>
    <Style x:Key="WarningButtonStyle" TargetType="syncfusion:RibbonButton">
        <Setter Property="Foreground" Value="Red" />
        <Setter Property="FontWeight" Value="Bold" />
    </Style>
</Window.Resources>

<!-- Apply style -->
<syncfusion:RibbonButton Label="Delete All" 
                         Style="{StaticResource WarningButtonStyle}" />
```

### Dynamic Button Appearance

Change appearance based on state:

```xml
<syncfusion:RibbonButton Label="Recording" 
                         Click="OnRecordClick" />
```

```csharp
private void OnRecordClick(object sender, EventArgs e)
{
    var button = sender as RibbonButton;
    if (button.IsChecked == true)
    {
        button.Foreground = Brushes.Red;
        button.Label = "Stop Recording";
    }
    else
    {
        button.Foreground = Brushes.Black;
        button.Label = "Start Recording";
    }
}
```

## Common Patterns

### Pattern 1: Format Toolbar with Split Buttons
```xml
<syncfusion:RibbonBar Header="Formatting">
    <syncfusion:SplitButton Label="Bold" SizeForm="Large">
        <syncfusion:RibbonMenuItem Header="Bold" />
        <syncfusion:RibbonMenuItem Header="Remove Bold" />
    </syncfusion:SplitButton>
    
    <syncfusion:SplitButton Label="Color" SizeForm="Large">
        <syncfusion:RibbonMenuItem Header="Red" />
        <syncfusion:RibbonMenuItem Header="Blue" />
        <syncfusion:RibbonMenuItem Header="Green" />
    </syncfusion:SplitButton>
</syncfusion:RibbonBar>
```

### Pattern 2: MVVM Buttons with Validation
```xml
<syncfusion:RibbonButton Label="Submit" 
                         Command="{Binding SubmitCommand}"
                         ToolTip="Submit form (Ctrl+Enter)" />
```

Command is automatically disabled if form is invalid.

### Pattern 3: Context-Sensitive Menus
```xml
<syncfusion:RibbonMenuItem Header="Cut" 
                           Command="{Binding CutCommand}"
                           IsEnabled="{Binding HasSelection}" />
```

## Troubleshooting

### Issue: Button not responding to clicks
**Solution:** Check that `Click` event handler or `Command` binding is properly configured. Verify event handler method exists in code-behind.

### Issue: Menu items not showing
**Solution:** Ensure you're using `RibbonMenuItem` inside `SplitButton` or `MenuButton`, not regular `MenuItem`.

### Issue: Icon not displaying
**Solution:** Verify image path is correct. Use `pack://application:,,,/YourAssembly;component/Resources/image.png` format for embedded resources.

### Issue: Disabled button not grayed out
**Solution:** If using `IsEnabled` binding, ensure view model property is properly implementing `INotifyPropertyChanged`.

### Issue: Split button dropdown not opening
**Solution:** Ensure you have items (like `RibbonMenuItem`) inside the `SplitButton`.

### Issue: Command not executing
**Solution:** Check that view model is set as `DataContext`. Verify `ICommand` implementation and `RelayCommand` class is available.

# Populating Items in WPF Radial Menu

This guide covers how to populate the radial menu with items using data binding, commands, and various configuration options.

## Table of Contents
- [ItemsSource Data Binding](#itemssource-data-binding)
- [DisplayMemberPath](#displaymemberpath)
- [CommandPath](#commandpath)
- [ItemTemplate](#itemtemplate)
- [DrillDownItem](#drilldownitem)
- [EnableFreeRotation](#enablefreerotation)
- [Children and HasItems](#children-and-hasitems)
- [Command and CommandParameter](#command-and-commandparameter)
- [CloseOnExecute](#closeonexecute)
- [CheckMode](#checkmode)
- [GroupName and IsChecked](#groupname-and-ischecked)
- [SfRadialMenuItem Events](#sfradialmenuitem-events)

---

## ItemsSource Data Binding

Radial menu items can be populated from a business object collection using the `ItemsSource` property. This is ideal for MVVM scenarios where menu items are dynamically generated from data.

### Creating a Business Model

First, create a model class to represent menu items:

```csharp
public class ApplicationCommand
{
    public string Name { get; set; }
    public string ImagePath { get; set; }
    public ICommand Command { get; set; }
}
```

### Creating the Collection

In your ViewModel, create a collection of menu items:

```csharp
private List<ApplicationCommand> options;

public List<ApplicationCommand> Options
{
    get { return options; }
    set { options = value; }
}
```

### Populating the Collection

Initialize the collection with menu items:

```csharp
public MainViewModel()
{
    Options = new List<ApplicationCommand>();
    Options.Add(new ApplicationCommand() { Name = "Bold", ImagePath = "bold.png" });
    Options.Add(new ApplicationCommand() { Name = "Cut", ImagePath = "cut.png" });
    Options.Add(new ApplicationCommand() { Name = "Copy", ImagePath = "copy.png" });
    Options.Add(new ApplicationCommand() { Name = "Paste", ImagePath = "paste.png" });
}
```

### Binding to ItemsSource

Bind the collection to the `ItemsSource` property:

```xaml
<navigation:SfRadialMenu IsOpen="True" ItemsSource="{Binding Options}"/>
```

At this point, the items will be created, but the headers won't display correctly because the control doesn't know which property contains the display text.

---

## DisplayMemberPath

The `DisplayMemberPath` property specifies which property of the business object should be displayed as the header of each radial menu item.

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         ItemsSource="{Binding Options}"
                         DisplayMemberPath="Name"/>
```

Now the `Name` property from each `ApplicationCommand` object will be displayed as the header.

### Without DisplayMemberPath
The menu items will show the object's ToString() representation (often just the type name).

### With DisplayMemberPath
The menu items display the specified property value (e.g., "Bold", "Cut", "Copy").

---

## CommandPath

The `CommandPath` property binds commands from business objects to radial menu items automatically when using data binding.

```xaml
<navigation:SfRadialMenu IsOpen="True" 
                         DisplayMemberPath="Name" 
                         CommandPath="Command"
                         ItemsSource="{Binding Options}"/>
```

### Complete Example with Commands

```csharp
public class ApplicationCommand
{
    public string Name { get; set; }
    public string ImagePath { get; set; }
    public ICommand Command { get; set; }
}

public class EditorViewModel : INotifyPropertyChanged
{
    public List<ApplicationCommand> Options { get; set; }
    
    public EditorViewModel()
    {
        Options = new List<ApplicationCommand>
        {
            new ApplicationCommand 
            { 
                Name = "Bold", 
                ImagePath = "bold.png",
                Command = new RelayCommand(() => ApplyBold())
            },
            new ApplicationCommand 
            { 
                Name = "Cut", 
                ImagePath = "cut.png",
                Command = new RelayCommand(() => CutText())
            },
            new ApplicationCommand 
            { 
                Name = "Copy", 
                ImagePath = "copy.png",
                Command = new RelayCommand(() => CopyText())
            }
        };
    }
    
    private void ApplyBold() => MessageBox.Show("Bold applied");
    private void CutText() => MessageBox.Show("Text cut");
    private void CopyText() => MessageBox.Show("Text copied");
}
```

---

## ItemTemplate

The `ItemTemplate` property allows full customization of how each menu item's content is displayed. Use this when you need more control than `DisplayMemberPath` provides.

### Basic ItemTemplate with Icon and Text

```xaml
<navigation:SfRadialMenu IsOpen="True" ItemsSource="{Binding Options}">
    <navigation:SfRadialMenu.ItemTemplate>
        <DataTemplate>
            <StackPanel>
                <Image Height="15" 
                       Width="15" 
                       Source="{Binding ImagePath}"/>
                <TextBlock Margin="0,5,0,0" 
                          Text="{Binding Name}"/>
            </StackPanel>
        </DataTemplate>
    </navigation:SfRadialMenu.ItemTemplate>
</navigation:SfRadialMenu>
```

### Advanced ItemTemplate with Styling

```xaml
<navigation:SfRadialMenu IsOpen="True" ItemsSource="{Binding Options}">
    <navigation:SfRadialMenu.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                
                <Image Grid.Row="0"
                       Height="20" 
                       Width="20" 
                       Source="{Binding ImagePath}"
                       Stretch="Uniform"/>
                
                <TextBlock Grid.Row="1"
                          Text="{Binding Name}"
                          FontSize="10"
                          FontWeight="Bold"
                          HorizontalAlignment="Center"
                          Margin="0,3,0,0"/>
            </Grid>
        </DataTemplate>
    </navigation:SfRadialMenu.ItemTemplate>
</navigation:SfRadialMenu>
```

### When to Use ItemTemplate vs DisplayMemberPath

**Use DisplayMemberPath when:**
- You only need to display a single property as text
- Simple text display is sufficient

**Use ItemTemplate when:**
- You need to display multiple properties (icon + text)
- You want custom layouts or styling
- You need data converters or complex bindings

---

## DrillDownItem

The `DrillDownItem` property gets the currently active `SfRadialMenuItem` when the user has drilled down (navigated) into sub-level items. This is useful for tracking navigation state.

```xaml
<navigation:SfRadialMenu x:Name="radialMenu" 
                         IsOpen="True" 
                         Navigating="RadialMenu_Navigating">
    <navigation:SfRadialMenuItem Header="Bold">
        <navigation:SfRadialMenuItem Header="Bold 1"/>
        <navigation:SfRadialMenuItem Header="Bold 2"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
private void RadialMenu_Navigating(object sender, NavigatingEventArgs e)
{
    SfRadialMenu menu = sender as SfRadialMenu;
    if (menu.DrillDownItem != null)
    {
        string header = (menu.DrillDownItem as SfRadialMenuItem)?.Header?.ToString();
        MessageBox.Show($"Drilled into: {header}");
    }
}
```

### Use Cases for DrillDownItem

1. **Breadcrumb Display:** Show the current navigation path
2. **Context-Aware Actions:** Enable/disable features based on current level
3. **State Management:** Save/restore the drill-down state

```csharp
// Example: Tracking navigation depth
private void RadialMenu_Navigating(object sender, NavigatingEventArgs e)
{
    int navigationDepth = GetNavigationDepth(radialMenu.DrillDownItem);
    StatusText.Text = $"Navigation Depth: {navigationDepth}";
}

private int GetNavigationDepth(SfRadialMenuItem item)
{
    int depth = 0;
    SfRadialMenuItem current = item;
    
    while (current != null)
    {
        depth++;
        current = GetParentMenuItem(current);
    }
    
    return depth;
}
```

---

## EnableFreeRotation

The `EnableFreeRotation` property allows users to manually rotate the radial menu using touch or mouse manipulation. When set to `true`, the menu can be rotated to any angle.

### XAML Example

```xaml
<navigation:SfRadialMenu IsOpen="True" EnableFreeRotation="True">
    <navigation:SfRadialMenuItem Header="Bold"/>
    <navigation:SfRadialMenuItem Header="Cut"/>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

### Code-Behind Example

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.IsOpen = true;
radialMenu.EnableFreeRotation = true;

radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Bold" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Cut" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Copy" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Paste" });
```

### When to Use EnableFreeRotation

**Enable when:**
- Building touch-first interfaces
- Users need to adjust menu orientation for comfort
- Creating kiosk or tablet applications

**Disable when:**
- Precise item positioning is critical
- Desktop mouse-only interfaces
- Fixed orientation is part of the design

---

## Children and HasItems

### Children Property

The `Children` property returns the collection of child `SfRadialMenuItem` elements under a parent item. Use this to programmatically access or modify sub-items.

### HasItems Property

The `HasItems` property returns `true` if the menu item contains child items, `false` otherwise.

### Example Usage

```xaml
<navigation:SfRadialMenu x:Name="radialMenu" IsOpen="True">
    <navigation:SfRadialMenuItem x:Name="boldItem" Header="Bold">
        <navigation:SfRadialMenuItem Header="Bold 1"/>
        <navigation:SfRadialMenuItem Header="Bold 2"/>
    </navigation:SfRadialMenuItem>
    <navigation:SfRadialMenuItem Header="Copy"/>
    <navigation:SfRadialMenuItem Header="Paste"/>
</navigation:SfRadialMenu>
```

```csharp
// Check if item has children
if (boldItem.HasItems)
{
    MessageBox.Show($"Bold item has {boldItem.Children.Count()} children");
    
    // Iterate through children
    foreach (SfRadialMenuItem child in boldItem.Children)
    {
        MessageBox.Show($"Child item: {child.Header?.ToString()}");
    }
}
```

### Programmatic Creation with Children

```csharp
SfRadialMenu radialMenu = new SfRadialMenu();
radialMenu.IsOpen = true;

SfRadialMenuItem boldItem = new SfRadialMenuItem() { Header = "Bold" };
boldItem.Items.Add(new SfRadialMenuItem() { Header = "Bold 1" });
boldItem.Items.Add(new SfRadialMenuItem() { Header = "Bold 2" });

radialMenu.Items.Add(boldItem);
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Copy" });
radialMenu.Items.Add(new SfRadialMenuItem() { Header = "Paste" });

// Later, check and modify children
if (boldItem.HasItems)
{
    // Add another child dynamically
    boldItem.Items.Add(new SfRadialMenuItem() { Header = "Bold 3" });
    
    // Remove a specific child
    var childToRemove = boldItem.Children.FirstOrDefault(c => 
        (c as SfRadialMenuItem)?.Header?.ToString() == "Bold 2");
    if (childToRemove != null)
    {
        boldItem.Items.Remove(childToRemove);
    }
}
```

---

## Command and CommandParameter

When creating menu items directly in XAML (not using data binding), use the `Command` and `CommandParameter` properties to bind commands.

### Basic Command Binding

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold"
                                 Command="{Binding BoldCommand}"
                                 CommandParameter="BoldParam"/>
    <navigation:SfRadialMenuItem Header="Copy"
                                 Command="{Binding CopyCommand}"
                                 CommandParameter="CopyParam"/>
    <navigation:SfRadialMenuItem Header="Paste"
                                 Command="{Binding PasteCommand}"
                                 CommandParameter="PasteParam"/>
</navigation:SfRadialMenu>
```

### ViewModel Implementation

```csharp
public class EditorViewModel
{
    public ICommand BoldCommand { get; set; }
    public ICommand CopyCommand { get; set; }
    public ICommand PasteCommand { get; set; }
    
    public EditorViewModel()
    {
        BoldCommand = new RelayCommand(param => 
            MessageBox.Show($"Bold executed with: {param}"));
        CopyCommand = new RelayCommand(param => 
            MessageBox.Show($"Copy executed with: {param}"));
        PasteCommand = new RelayCommand(param => 
            MessageBox.Show($"Paste executed with: {param}"));
    }
}

// Simple RelayCommand implementation
public class RelayCommand : ICommand
{
    private readonly Action<object> _execute;
    private readonly Func<object, bool> _canExecute;
    
    public RelayCommand(Action<object> execute, Func<object, bool> canExecute = null)
    {
        _execute = execute ?? throw new ArgumentNullException(nameof(execute));
        _canExecute = canExecute;
    }
    
    public bool CanExecute(object parameter) => _canExecute == null || _canExecute(parameter);
    public void Execute(object parameter) => _execute(parameter);
    public event EventHandler CanExecuteChanged
    {
        add { CommandManager.RequerySuggested += value; }
        remove { CommandManager.RequerySuggested -= value; }
    }
}
```

---

## CloseOnExecute

The `CloseOnExecute` property determines whether the radial menu automatically closes after a menu item is clicked. Default is `false`.

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold" CloseOnExecute="True"/>
    <navigation:SfRadialMenuItem Header="Copy" CloseOnExecute="True"/>
    <navigation:SfRadialMenuItem Header="Paste" CloseOnExecute="False"/>
</navigation:SfRadialMenu>
```

```csharp
SfRadialMenuItem bold = new SfRadialMenuItem() 
{ 
    Header = "Bold", 
    CloseOnExecute = true 
};
```

### When to Use CloseOnExecute

**Set to True when:**
- The action is final (Cut, Copy, Paste)
- User expects menu to disappear after selection
- Single-action scenarios

**Set to False when:**
- Item has sub-items (navigation required)
- User might select multiple items
- Toggle/checkbox scenarios

---

## CheckMode

The `CheckMode` property specifies how menu items behave when clicked. It supports three values:

- **None** – No check behavior (default)
- **CheckBox** – Item can be independently checked/unchecked
- **Radio** – Only one item in the group can be checked

### CheckBox Mode

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold" 
                                 CheckMode="CheckBox" 
                                 IsChecked="True"/>
    <navigation:SfRadialMenuItem Header="Italic" 
                                 CheckMode="CheckBox" 
                                 IsChecked="False"/>
    <navigation:SfRadialMenuItem Header="Underline" 
                                 CheckMode="CheckBox" 
                                 IsChecked="False"/>
</navigation:SfRadialMenu>
```

```csharp
SfRadialMenuItem bold = new SfRadialMenuItem() 
{ 
    Header = "Bold", 
    CheckMode = CheckMode.CheckBox, 
    IsChecked = true 
};

SfRadialMenuItem italic = new SfRadialMenuItem() 
{ 
    Header = "Italic", 
    CheckMode = CheckMode.CheckBox, 
    IsChecked = false 
};
```

### Radio Mode

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Left" 
                                 CheckMode="Radio" 
                                 GroupName="Alignment" 
                                 IsChecked="True"/>
    <navigation:SfRadialMenuItem Header="Center" 
                                 CheckMode="Radio" 
                                 GroupName="Alignment" 
                                 IsChecked="False"/>
    <navigation:SfRadialMenuItem Header="Right" 
                                 CheckMode="Radio" 
                                 GroupName="Alignment" 
                                 IsChecked="False"/>
</navigation:SfRadialMenu>
```

---

## GroupName and IsChecked

### GroupName Property

The `GroupName` property is used with `CheckMode="Radio"` to group radio-style menu items. Only one item within the same group can be checked at a time.

### IsChecked Property

The `IsChecked` property gets or sets whether the menu item is in a checked state. Used with `CheckMode` set to `CheckBox` or `Radio`.

### Complete Example

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <!-- CheckBox mode items (independent) -->
    <navigation:SfRadialMenuItem Header="Bold"
                                 CheckMode="CheckBox" 
                                 IsChecked="True"/>
    <navigation:SfRadialMenuItem Header="Italic"
                                 CheckMode="CheckBox" 
                                 IsChecked="False"/>
    
    <!-- Radio mode items (grouped) -->
    <navigation:SfRadialMenuItem Header="Left"
                                 CheckMode="Radio" 
                                 GroupName="Alignment" 
                                 IsChecked="True"/>
    <navigation:SfRadialMenuItem Header="Center"
                                 CheckMode="Radio" 
                                 GroupName="Alignment" 
                                 IsChecked="False"/>
    <navigation:SfRadialMenuItem Header="Right"
                                 CheckMode="Radio" 
                                 GroupName="Alignment" 
                                 IsChecked="False"/>
</navigation:SfRadialMenu>
```

### Code-Behind with Multiple Radio Groups

```csharp
// Alignment group
SfRadialMenuItem left = new SfRadialMenuItem() 
{ 
    Header = "Left", 
    CheckMode = CheckMode.Radio, 
    GroupName = "Alignment", 
    IsChecked = true 
};

SfRadialMenuItem center = new SfRadialMenuItem() 
{ 
    Header = "Center", 
    CheckMode = CheckMode.Radio, 
    GroupName = "Alignment", 
    IsChecked = false 
};

// Font size group
SfRadialMenuItem small = new SfRadialMenuItem() 
{ 
    Header = "Small", 
    CheckMode = CheckMode.Radio, 
    GroupName = "FontSize", 
    IsChecked = true 
};

SfRadialMenuItem large = new SfRadialMenuItem() 
{ 
    Header = "Large", 
    CheckMode = CheckMode.Radio, 
    GroupName = "FontSize", 
    IsChecked = false 
};
```

---

## SfRadialMenuItem Events

### Click Event

The `Click` event is raised when a user clicks on a radial menu item.

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold" Click="BoldItem_Click"/>
    <navigation:SfRadialMenuItem Header="Copy" Click="CopyItem_Click"/>
    <navigation:SfRadialMenuItem Header="Paste" Click="PasteItem_Click"/>
</navigation:SfRadialMenu>
```

```csharp
private void BoldItem_Click(object sender, EventArgs e)
{
    MessageBox.Show("Bold item clicked");
    // Apply bold formatting
}

private void CopyItem_Click(object sender, EventArgs e)
{
    MessageBox.Show("Copy item clicked");
    // Copy selected text
}

private void PasteItem_Click(object sender, EventArgs e)
{
    MessageBox.Show("Paste item clicked");
    // Paste clipboard content
}
```

### Checked Event

The `Checked` event is raised when the item transitions to a checked state. Applicable when `CheckMode` is `CheckBox` or `Radio`.

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold"
                                 CheckMode="CheckBox"
                                 Checked="BoldItem_Checked"/>
    <navigation:SfRadialMenuItem Header="Italic"
                                 CheckMode="CheckBox"
                                 Checked="ItalicItem_Checked"/>
</navigation:SfRadialMenu>
```

```csharp
private void BoldItem_Checked(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Bold is now Checked");
    // Apply bold formatting
}

private void ItalicItem_Checked(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Italic is now Checked");
    // Apply italic formatting
}
```

### UnChecked Event

The `UnChecked` event is raised when the item transitions from checked to unchecked. Applicable when `CheckMode` is `CheckBox`.

```xaml
<navigation:SfRadialMenu IsOpen="True">
    <navigation:SfRadialMenuItem Header="Bold"
                                 CheckMode="CheckBox"
                                 IsChecked="True"
                                 Checked="BoldItem_Checked"
                                 UnChecked="BoldItem_UnChecked"/>
    <navigation:SfRadialMenuItem Header="Italic"
                                 CheckMode="CheckBox"
                                 IsChecked="True"
                                 Checked="ItalicItem_Checked"
                                 UnChecked="ItalicItem_UnChecked"/>
</navigation:SfRadialMenu>
```

```csharp
private void BoldItem_Checked(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Bold is now Checked");
    ApplyBoldFormatting();
}

private void BoldItem_UnChecked(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Bold is now UnChecked");
    RemoveBoldFormatting();
}

private void ItalicItem_Checked(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Italic is now Checked");
    ApplyItalicFormatting();
}

private void ItalicItem_UnChecked(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Italic is now UnChecked");
    RemoveItalicFormatting();
}
```

### Event Usage Tips

1. **Click vs Checked:** Use `Click` for immediate actions, `Checked`/`UnChecked` for toggle states
2. **Command vs Event:** Prefer Commands for MVVM, Events for code-behind scenarios
3. **Event Chaining:** Events fire in order: Click → Checked/UnChecked

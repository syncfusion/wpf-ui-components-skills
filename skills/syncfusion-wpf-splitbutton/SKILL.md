---
name: syncfusion-wpf-splitbutton
description: Implement Syncfusion WPF SplitButton (SplitButtonAdv) with dropdown menus, command binding, and data binding. Use this when working with split buttons, dropdown buttons with menus, or button-menu combinations. This skill covers SplitButtonAdv setup, default actions with dropdown menus, command binding, MVVM implementation, and data-bound dropdown items in WPF applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF SplitButton

The Syncfusion WPF SplitButton (`SplitButtonAdv`) is a combination of a button and a menu control. The button provides a default action, while clicking the arrow displays a dropdown list for additional selections. This control is ideal for scenarios where you need both a primary action and alternative options.

## When to Use This Skill

Use this skill when you need to:

- **Implement WPF splitbutton controls** with default actions and dropdown menus
- **Create button-dropdown combinations** for multiple action selections
- **Build data-bound dropdown menus** with ItemsSource binding
- **Implement command patterns** with ICommand and MVVM architecture
- **Configure dropdown menu items** with icons, checkboxes, and custom items
- **Handle splitbutton events** like dropdown opening/closing and item clicks
- **Apply size modes** (Small, Normal, Large) with custom icons
- **Customize splitbutton appearance** with themes and templates
- **Support multiline text** in large button modes
- **Enable/disable actions** using CanExecute command logic

## Component Overview

### Key Features

- **Dual Action**: Primary button action + dropdown menu for alternatives
- **Data Binding**: Full ItemsSource and DataTemplate support for MVVM
- **Command Binding**: ICommand support for button and menu items with CanExecute
- **Size Modes**: Small (icon only), Normal (icon + text), Large (large icon + text)
- **Icon Templates**: Support for path data, font icons, and custom templates
- **Customizable Menu Items**: Icons, checkboxes, scrollbars, and custom content
- **Dropdown Direction**: Control popup position (Left, Right, BottomLeft, etc.)
- **Events**: Opening, Opened, Closing, Closed, Click, IsCheckedChanged
- **Theming**: Built-in themes via SfSkinManager and custom theme support
- **Multiline Text**: Display multi-line labels in large size mode

### Control Structure

```
SplitButtonAdv
├── Primary Button (default action)
│   ├── Label (text)
│   ├── Icon (SmallIcon/LargeIcon/IconTemplate)
│   └── Command (ICommand binding)
└── Dropdown Arrow (opens menu)
    └── DropDownMenuGroup (container)
        ├── DropDownMenuItem (standard items)
        │   ├── Header (text)
        │   ├── Icon (image)
        │   ├── IsCheckable (checkbox support)
        │   └── Command (ICommand binding)
        └── MoreItems (custom UIElement items)
```

## Documentation and Navigation Guide

This skill uses progressive disclosure. Read the appropriate reference file based on your implementation needs:

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

When to read: Setting up a new splitbutton, need basic configuration

**Topics covered:**
- Installation and assembly deployment (Syncfusion.Shared.WPF)
- Adding control via designer, XAML, or C# code
- Setting label text and basic properties
- Size modes: Small (icon only), Normal (icon + text), Large (large icon + text)
- Icon templates and icon template selectors for custom icons
- Setting images using SmallIcon and LargeIcon properties
- Configuring icon width and height
- IsDefault mode for Enter key activation
- Adding menu items to DropDownMenuGroup

### Dropdown Menu Items Configuration

📄 **Read:** [references/dropdown-menu-items.md](references/dropdown-menu-items.md)

When to read: Configuring menu item appearance, adding custom items, enabling scrollbars

**Topics covered:**
- Setting icons for dropdown menu items
- Icon bar visibility (IconBarEnabled property)
- Scrollbar visibility for large menu lists
- Checkable dropdown menu items (IsCheckable/IsChecked)
- Resizing dropdown menu with gripper (IsResizable)
- Adding custom dropdown menu items via MoreItems property
- Icon bar visibility for custom items (IsMoreItemsIconTrayEnabled)

### Data Binding

📄 **Read:** [references/data-binding.md](references/data-binding.md)

When to read: Implementing MVVM patterns, binding collections to dropdown items

**Topics covered:**
- Creating model classes for menu item data
- Creating view models with observable collections
- Binding ItemsSource to DropDownMenuGroup
- Using ItemTemplate and DataTemplate for menu items
- Setting DataContext for data binding
- Binding commands from view model to menu items
- Complete MVVM implementation examples
- DelegateCommand pattern for ICommand

### Command Binding

📄 **Read:** [references/command-binding.md](references/command-binding.md)

When to read: Implementing command patterns, handling button/menu item actions

**Topics covered:**
- Command property overview and ICommand interface
- CommandParameter for passing data to command handlers
- DelegateCommand implementation pattern
- Binding commands to SplitButtonAdv primary button
- Binding commands to dropdown menu items
- CanExecute logic for enabling/disabling actions
- RaiseCanExecuteChanged for dynamic command state
- Complete command binding examples with NotificationObject

### Dropdown Configuration and Events

📄 **Read:** [references/dropdown-configuration.md](references/dropdown-configuration.md)

When to read: Controlling dropdown position, handling events, enabling multiline text

**Topics covered:**
- Dropdown direction options (Left, Right, BottomLeft, BottomRight, TopLeft, TopRight)
- DropDownOpening and DropDownOpened events
- DropDownClosing and DropDownClosed events
- Click event for button and menu items
- IsCheckedChanged event for checkable items
- Multiline text support (IsMultiLine property)
- Event handler implementation patterns

### Customization and Theming

📄 **Read:** [references/customization.md](references/customization.md)

When to read: Applying themes, customizing appearance, creating templates

**Topics covered:**
- WPF styles and templates overview
- Editing appearance in Expression Blend
- Editing appearance in Visual Studio
- Creating ControlTemplate resources
- Applying built-in themes with SfSkinManager
- Creating custom themes using ThemeStudio
- Template resource location options

## Quick Start Example

### Basic SplitButton with Dropdown Menu

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:SplitButtonAdv Label="Colors" 
                                    SizeMode="Normal" 
                                    Click="SplitButton_Click">
            <syncfusion:DropDownMenuGroup>
                <syncfusion:DropDownMenuItem Header="Red" 
                                             Click="MenuItem_Click"/>
                <syncfusion:DropDownMenuItem Header="Green" 
                                             Click="MenuItem_Click"/>
                <syncfusion:DropDownMenuItem Header="Blue" 
                                             Click="MenuItem_Click"/>
            </syncfusion:DropDownMenuGroup>
        </syncfusion:SplitButtonAdv>
    </Grid>
</Window>
```

```csharp
using Syncfusion.Windows.Tools.Controls;

private void SplitButton_Click(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Primary action executed");
}

private void MenuItem_Click(object sender, RoutedEventArgs e)
{
    var menuItem = sender as DropDownMenuItem;
    MessageBox.Show($"Selected: {menuItem.Header}");
}
```

## Common Patterns

### Pattern 1: Data-Bound SplitButton with MVVM

```xaml
<syncfusion:SplitButtonAdv Label="Country" SizeMode="Normal">
    <syncfusion:DropDownMenuGroup ItemsSource="{Binding Countries}">
        <syncfusion:DropDownMenuGroup.ItemTemplate>
            <DataTemplate>
                <syncfusion:DropDownMenuItem 
                    Header="{Binding Name}"
                    Command="{Binding DataContext.SelectCountryCommand, 
                             RelativeSource={RelativeSource AncestorType=syncfusion:SplitButtonAdv}}"
                    CommandParameter="{Binding}">
                    <syncfusion:DropDownMenuItem.Icon>
                        <Image Source="{Binding FlagIcon}"/>
                    </syncfusion:DropDownMenuItem.Icon>
                </syncfusion:DropDownMenuItem>
            </DataTemplate>
        </syncfusion:DropDownMenuGroup.ItemTemplate>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

### Pattern 2: Command Binding with CanExecute

```csharp
public class ViewModel : NotificationObject
{
    private bool _canPerformAction = true;
    
    public DelegateCommand<object> ClickCommand { get; set; }
    
    public bool CanPerformAction
    {
        get => _canPerformAction;
        set
        {
            _canPerformAction = value;
            ClickCommand.RaiseCanExecuteChanged();
            RaisePropertyChanged(nameof(CanPerformAction));
        }
    }
    
    public ViewModel()
    {
        ClickCommand = new DelegateCommand<object>(
            ExecuteAction, 
            CanExecuteAction);
    }
    
    private bool CanExecuteAction(object parameter) => CanPerformAction;
    
    private void ExecuteAction(object parameter)
    {
        MessageBox.Show($"Action executed: {parameter}");
    }
}
```

```xaml
<syncfusion:SplitButtonAdv Label="Action" 
                            Command="{Binding ClickCommand}"
                            CommandParameter="Primary Action">
    <!-- Dropdown items -->
</syncfusion:SplitButtonAdv>
```

### Pattern 3: Large Button with Icon Template

```xaml
<Window.Resources>
    <DataTemplate x:Key="customIconTemplate">
        <Grid Width="32" Height="32">
            <Path Data="M10,0 L20,10 L10,20 L0,10 Z" 
                  Fill="#FF3A3A38" 
                  Stretch="Fill"/>
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:SplitButtonAdv Label="Custom Icon" 
                            SizeMode="Large"
                            IconTemplate="{StaticResource customIconTemplate}">
    <!-- Dropdown items -->
</syncfusion:SplitButtonAdv>
```

### Pattern 4: Scrollable Dropdown with Checkable Items

```xaml
<syncfusion:SplitButtonAdv Label="Options" SizeMode="Normal">
    <syncfusion:DropDownMenuGroup MaxHeight="200" 
                                    ScrollBarVisibility="Visible"
                                    IconBarEnabled="True">
        <syncfusion:DropDownMenuItem Header="Option 1" 
                                     IsCheckable="True" 
                                     IsChecked="True"/>
        <syncfusion:DropDownMenuItem Header="Option 2" 
                                     IsCheckable="True"/>
        <syncfusion:DropDownMenuItem Header="Option 3" 
                                     IsCheckable="True"/>
        <!-- More items... -->
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

## Key Properties

### SplitButtonAdv Properties

| Property | Type | Description |
|----------|------|-------------|
| `Label` | string | Text displayed on the button |
| `SizeMode` | SizeMode | Size mode: Small, Normal, or Large |
| `SmallIcon` | ImageSource | Icon for Small/Normal modes |
| `LargeIcon` | ImageSource | Icon for Large mode |
| `IconTemplate` | DataTemplate | Custom icon template (overrides SmallIcon/LargeIcon) |
| `IconTemplateSelector` | DataTemplateSelector | Dynamic icon template selection |
| `IconWidth` | double | Width of the icon |
| `IconHeight` | double | Height of the icon |
| `Command` | ICommand | Command for primary button click |
| `CommandParameter` | object | Parameter passed to Command |
| `DropDirection` | DropDirection | Popup position: Left, Right, BottomLeft, etc. |
| `IsMultiLine` | bool | Enable multiline text in Large mode |
| `IsDefault` | bool | Activate button with Enter key |

### DropDownMenuGroup Properties

| Property | Type | Description |
|----------|------|-------------|
| `ItemsSource` | IEnumerable | Data source for menu items |
| `ItemTemplate` | DataTemplate | Template for menu item rendering |
| `IconBarEnabled` | bool | Show/hide vertical icon bar |
| `ScrollBarVisibility` | ScrollBarVisibility | Scrollbar visibility for long lists |
| `IsResizable` | bool | Enable gripper for resizing popup |
| `MaxHeight` | double | Maximum height of dropdown popup |
| `MoreItems` | ObservableCollection<UIElement> | Custom items at bottom of menu |
| `IsMoreItemsIconTrayEnabled` | bool | Icon bar for custom items |

### DropDownMenuItem Properties

| Property | Type | Description |
|----------|------|-------------|
| `Header` | object | Text or content of menu item |
| `Icon` | object | Icon displayed before header |
| `IsCheckable` | bool | Enable checkbox for item |
| `IsChecked` | bool | Checked state of item |
| `Command` | ICommand | Command for menu item click |
| `CommandParameter` | object | Parameter passed to Command |

## Common Use Cases

### Use Case 1: Save Button with Format Options
Primary action saves in default format, dropdown offers alternative formats (PDF, Excel, CSV).

### Use Case 2: Send Button with Recipient Options
Primary action sends to default recipient, dropdown lists alternative recipients.

### Use Case 3: Filter Button with Preset Filters
Primary action applies last filter, dropdown shows available filter presets.

### Use Case 4: Export Button with Export Types
Primary action exports to default type, dropdown offers various export formats.

## Troubleshooting Tips

**Problem:** Icons not displaying correctly  
**Solution:** Check icon priority: IconTemplate > LargeIcon > SmallIcon. Ensure correct size mode is set.

**Problem:** Commands not executing  
**Solution:** Verify CanExecute returns true. Check DataContext binding for RelativeSource.

**Problem:** Dropdown items not showing  
**Solution:** Ensure DropDownMenuGroup contains DropDownMenuItem elements. Check ItemTemplate binding.

**Problem:** Events not firing  
**Solution:** Verify event handler names match in code-behind. Check for cancellation in Opening/Closing events.

## Additional Resources

- [GitHub Samples](https://github.com/SyncfusionExamples/wpf-splitbutton-examples)
- [Syncfusion WPF Documentation](https://help.syncfusion.com/wpf/splitbutton/overview)
- [Control Dependencies](https://help.syncfusion.com/wpf/control-dependencies#splitbutton)

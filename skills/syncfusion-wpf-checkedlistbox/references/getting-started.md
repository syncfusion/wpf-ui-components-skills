# Getting Started with CheckedListBox

## Table of Contents
- [Overview](#overview)
- [Assembly Deployment](#assembly-deployment)
- [Creating Your First CheckListBox](#creating-your-first-checklistbox)
- [Adding Control via Designer](#adding-control-via-designer)
- [Adding Control in XAML](#adding-control-in-xaml)
- [Adding Control in C#](#adding-control-in-c)
- [Populating Items](#populating-items)
- [Basic Event Handling](#basic-event-handling)
- [Localization Support](#localization-support)

## Overview

This guide walks through creating a WPF application with the CheckListBox control, from installation to basic item population and event handling. The CheckListBox control displays a list of items with checkboxes, enabling multi-item selection in a clean, intuitive interface.

**What you'll learn:**
- How to install and reference required assemblies
- Multiple ways to add the control to your application
- How to populate items statically and dynamically
- Basic event handling for checked state changes
- Getting the list of checked items

## Assembly Deployment

The CheckListBox control requires two Syncfusion assemblies to be referenced in your WPF project.

### Required Assemblies

- **Syncfusion.Shared.WPF** - Core shared functionality
- **Syncfusion.Tools.WPF** - Tools controls including CheckListBox

### Installation Methods

**Method 1: NuGet Package Manager (Recommended)**

Open the Package Manager Console in Visual Studio and run:

```powershell
Install-Package Syncfusion.Tools.WPF
```

This automatically installs both required assemblies and their dependencies.

**Method 2: NuGet Package Manager UI**

1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Tools.WPF"
4. Click Install

**Method 3: Syncfusion Reference Manager**

If you have Syncfusion Essential Studio installed:

# Getting Started with CheckListBox (summary)

This shortened getting-started guide includes the minimal steps to get a CheckListBox into your WPF project. Longer walkthroughs, complete examples, and troubleshooting were moved to an extras file to keep this page concise.

Quick start:
1. Install the NuGet: `Install-Package Syncfusion.Tools.WPF`.
2. Add `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"` to your `Window`.
3. Place a control:

```xml
<syncfusion:CheckListBox x:Name="checkListBox" Width="200" Height="300" Margin="20" />
```

4. Populate items statically or bind via `ItemsSource`.
5. Use `SelectedItems` or `ItemContainerStyle` for checkbox state management (see `data-binding-examples.md`).

For full XAML/C# examples, localization steps, and event handlers see: [getting-started-extras.md](getting-started-extras.md)
checkListBox.Items.Add(item);
```

### Approach 2: Using Data Binding (Dynamic Items)

Best for: Data-driven scenarios with MVVM pattern. Covered in detail in [data-binding.md](data-binding.md).

**Quick example:**

```csharp
// Model
public class Country
{
    public string Name { get; set; }
    public string Description { get; set; }
}

// ViewModel
public class ViewModel
{
    public ObservableCollection<Country> Countries { get; set; }

    public ViewModel()
    {
        Countries = new ObservableCollection<Country>
        {
            new Country { Name = "Mexico", Description = "North America" },
            new Country { Name = "Canada", Description = "North America" },
            new Country { Name = "Bermuda", Description = "Island" },
            new Country { Name = "Belize", Description = "Central America" },
            new Country { Name = "Panama", Description = "Central America" }
        };
    }
}
```

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Countries}"
                         DisplayMemberPath="Name"
                         x:Name="checkListBox">
    <syncfusion:CheckListBox.DataContext>
        <local:ViewModel />
    </syncfusion:CheckListBox.DataContext>
</syncfusion:CheckListBox>
```

**Key properties:**
- `ItemsSource` - Binds to the data collection
- `DisplayMemberPath` - Specifies which property to display (e.g., "Name")

## Basic Event Handling

### Checking and Unchecking Items

Users can check/uncheck items by:
- Clicking the checkbox
- Clicking the item content (if `IsCheckOnFirstClick="True"`)
- Pressing the Space key when an item is focused

### ItemChecked Event

The `ItemChecked` event fires when an item's checked state changes.

**XAML:**

```xml
<syncfusion:CheckListBox x:Name="checkListBox"
                         ItemChecked="CheckListBox_ItemChecked">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
    <syncfusion:CheckListBoxItem Content="Bermuda" />
</syncfusion:CheckListBox>
```

**Code-behind:**

```csharp
private void CheckListBox_ItemChecked(object sender, ItemCheckedEventArgs e)
{
    // Get the item that was checked/unchecked
    var item = e.Item as CheckListBoxItem;
    
    // Check if it was checked or unchecked
    bool isChecked = e.IsChecked;
    
    // Show notification
    MessageBox.Show($"{item.Content} is {(isChecked ? "checked" : "unchecked")}");
}
```

**In C#:**

```csharp
checkListBox.ItemChecked += (sender, e) =>
{
    var item = e.Item as CheckListBoxItem;
    Console.WriteLine($"{item.Content}: {e.IsChecked}");
};
```

### Getting Checked Items

Access all currently checked items using the `SelectedItems` property:

```csharp
// Get all checked items
var checkedItems = checkListBox.SelectedItems;

foreach (var item in checkedItems)
{
    var checkListItem = item as CheckListBoxItem;
    Console.WriteLine($"Checked: {checkListItem.Content}");
}
```

### Getting Current Selection

The `SelectedItem` property returns the currently focused item (whether checked or not):

```csharp
var currentItem = checkListBox.SelectedItem as CheckListBoxItem;
if (currentItem != null)
{
    MessageBox.Show($"Currently focused: {currentItem.Content}");
}
```

### Programmatically Checking Items

Add items to `SelectedItems` to check them programmatically:

```csharp
// Check specific items
checkListBox.SelectedItems.Add(item1);
checkListBox.SelectedItems.Add(item2);
```

## Localization Support

The CheckListBox supports localization for the "Select All" item text (when SelectAll functionality is enabled).

### Setting Up Localization

**Step 1: Create Resource Files**

Create `.resx` files for each culture:
- `Resources.resx` (default/English)
- `Resources.fr-FR.resx` (French)
- `Resources.es-ES.resx` (Spanish)

**Step 2: Add Name/Value Pairs**

In each `.resx` file, add:

| Name | Value (English) | Value (French) | Value (Spanish) |
|------|-----------------|----------------|-----------------|
| SelectAll | Select All | Tout sélectionner | Seleccionar todo |

**Step 3: Change Application Culture**

```csharp
// In App.xaml.cs or MainWindow constructor
System.Threading.Thread.CurrentThread.CurrentUICulture = 
    new System.Globalization.CultureInfo("fr-FR");
```

**Refer to:** [Localization of Syncfusion WPF Controls](https://help.syncfusion.com/wpf/localization) for comprehensive localization setup.

## Complete Example

Here's a complete working example combining all concepts:

**MainWindow.xaml:**

```xml
<Window x:Class="CheckListBoxDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="CheckListBox Getting Started" Height="400" Width="500">
    <Grid Margin="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        
        <syncfusion:CheckListBox x:Name="checkListBox"
                                 Grid.Row="0"
                                 ItemChecked="CheckListBox_ItemChecked">
            <syncfusion:CheckListBoxItem Content="Austria" />
            <syncfusion:CheckListBoxItem Content="Australia" />
            <syncfusion:CheckListBoxItem Content="Canada" IsSelected="True" />
            <syncfusion:CheckListBoxItem Content="Finland" />
            <syncfusion:CheckListBoxItem Content="New Zealand" />
        </syncfusion:CheckListBox>
        
        <Button Grid.Row="1" 
                Content="Show Checked Items" 
                Margin="0,10,0,0"
                Click="ShowCheckedItems_Click"
                Padding="10,5"/>
    </Grid>
</Window>
```

**MainWindow.xaml.cs:**

```csharp
using System.Windows;
using Syncfusion.Windows.Tools.Controls;
using System.Text;

namespace CheckListBoxDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void CheckListBox_ItemChecked(object sender, ItemCheckedEventArgs e)
        {
            var item = e.Item as CheckListBoxItem;
            string status = e.IsChecked ? "checked" : "unchecked";
            
            // Update window title with latest action
            this.Title = $"{item.Content} was {status}";
        }

        private void ShowCheckedItems_Click(object sender, RoutedEventArgs e)
        {
            var sb = new StringBuilder();
            sb.AppendLine("Checked Items:");
            
            foreach (var item in checkListBox.SelectedItems)
            {
                var checkListItem = item as CheckListBoxItem;
                sb.AppendLine($"- {checkListItem.Content}");
            }
            
            MessageBox.Show(sb.ToString(), "Selected Items", 
                            MessageBoxButton.OK, MessageBoxImage.Information);
        }
    }
}
```

## Next Steps

Now that you have a basic CheckListBox working:

- **Data Binding:** Learn MVVM pattern implementation → [data-binding.md](data-binding.md)
- **Selection Control:** Advanced checking/unchecking → [checking-and-selection.md](checking-and-selection.md)
- **Styling:** Customize appearance → [appearance-customization.md](appearance-customization.md)
- **Organization:** Group and sort items → [grouping-and-sorting.md](grouping-and-sorting.md)

## Troubleshooting

**Issue: Control not visible in Toolbox**
- Ensure Syncfusion.Tools.WPF is installed
- Restart Visual Studio
- Check Tools → Options → Toolbox → Reset Toolbox

**Issue: Namespace not recognized**
- Verify assembly references are added
- Clean and rebuild solution
- Check package is correctly installed via NuGet

**Issue: Items not displaying**
- Ensure Width and Height are set
- Check ItemsSource binding (if using data binding)
- Verify items are actually added to collection

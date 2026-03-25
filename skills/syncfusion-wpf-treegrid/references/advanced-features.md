# Advanced Features Reference for WPF TreeGrid

This reference covers advanced features and capabilities of the Syncfusion WPF TreeGrid (SfTreeGrid) control, including MVVM support, customization options, persistence, localization, theming, and accessibility features.

## Table of Contents

1. [MVVM Pattern Implementation](#mvvm-pattern-implementation)
2. [GridLines Customization](#gridlines-customization)
3. [Serialization and Deserialization](#serialization-and-deserialization)
4. [Localization](#localization)
5. [Theming](#theming)
6. [Helper Methods and Utilities](#helper-methods-and-utilities)
7. [NodeCheckBox Features](#nodecheckbox-features)
8. [ToolTip Customization](#tooltip-customization)
9. [UI Automation and Accessibility](#ui-automation-and-accessibility)
10. [Best Practices](#best-practices)
11. [Troubleshooting](#troubleshooting)

---

## MVVM Pattern Implementation

### SelectedItem Binding

Bind the SelectedItem property to your ViewModel for two-way data binding:

**XAML:**
```xml
<syncfusion:SfTreeGrid Name="treeGrid" 
                       Grid.Row="1" 
                       ChildPropertyName="ReportsTo"  
                       AutoExpandMode="AllNodesExpanded"
                       SelectedItem="{Binding SelectedItem, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"
                       AutoGenerateColumns="False"
                       ItemsSource="{Binding Employees}"
                       ParentPropertyName="ID"
                       SelfRelationRootValue="-1"/>
```

**ViewModel:**
```csharp
public class ViewModel : NotificationObject
{
    private object selectedItem;
    
    public object SelectedItem
    {
        get { return selectedItem; }
        set
        {
            selectedItem = value;
            RaisePropertyChanged("SelectedItem");
        }
    }
}
```

### Button Command Binding

Use ElementName binding to bind button commands in TreeGridTemplateColumn:

**XAML:**
```xml
<syncfusion:TreeGridTemplateColumn MappingName="Title" 
                                   syncfusion:FocusManagerHelper.WantsKeyInput="True">
    <syncfusion:TreeGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <Button Content="Click" 
                    syncfusion:FocusManagerHelper.FocusedElement="True" 
                    Command="{Binding Path=DataContext.RowDataCommand, ElementName=treeGrid}" 
                    CommandParameter="{Binding}"/>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.CellTemplate>
</syncfusion:TreeGridTemplateColumn>
```

**ViewModel:**
```csharp
public class ViewModel : NotificationObject
{
    private ICommand rowDataCommand;
    
    public ICommand RowDataCommand
    {
        get { return rowDataCommand; }
        set { rowDataCommand = value; }
    }
    
    public ViewModel()
    {
        rowDataCommand = new RelayCommand(ExecuteCommand);
    }
    
    private void ExecuteCommand(object parameter)
    {
        // Handle command execution
        var dataObject = parameter;
    }
}
```

### ComboBox ItemsSource Binding

Bind ComboBox ItemsSource from ViewModel:

**XAML:**
```xml
<syncfusion:TreeGridComboBoxColumn AllowEditing="True" 
                                   MappingName="Title"
                                   HeaderText="Title"
                                   ItemsSource="{Binding DataContext.TitleList, ElementName=treeGrid}"/>
```

**ViewModel:**
```csharp
private ObservableCollection<string> titleList;

public ObservableCollection<string> TitleList
{
    get { return titleList; }
    set { titleList = value; }
}
```

### ComboBox in Template

Use ElementName binding for ComboBox inside TreeGridTemplateColumn:

**XAML:**
```xml
<syncfusion:TreeGridTemplateColumn MappingName="Title" 
                                   syncfusion:FocusManagerHelper.WantsKeyInput="True">
    <syncfusion:TreeGridTemplateColumn.CellTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Title}"/>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.CellTemplate>
    <syncfusion:TreeGridTemplateColumn.EditTemplate>
        <DataTemplate>
            <ComboBox ItemsSource="{Binding Path=DataContext.TitleList, ElementName=treeGrid}"/>
        </DataTemplate>
    </syncfusion:TreeGridTemplateColumn.EditTemplate>
</syncfusion:TreeGridTemplateColumn>
```

### Columns Binding from ViewModel

Bind entire Columns collection from ViewModel:

**XAML:**
```xml
<syncfusion:SfTreeGrid Name="treeGrid" 
                       Columns="{Binding SfGridColumns, Mode=TwoWay}"
                       AutoGenerateColumns="False"
                       ItemsSource="{Binding Employees}"/>
```

**ViewModel:**
```csharp
public class ViewModel : NotificationObject
{
    private TreeGridColumns sfGridColumns;
    
    public TreeGridColumns SfGridColumns
    {
        get { return sfGridColumns; }
        set { sfGridColumns = value; }
    }
    
    public ViewModel()
    {
        this.sfGridColumns = new TreeGridColumns();
        sfGridColumns.Add(new TreeGridTextColumn() { MappingName = "FirstName" });
        sfGridColumns.Add(new TreeGridTextColumn() { MappingName = "LastName" });
        sfGridColumns.Add(new TreeGridTextColumn() { MappingName = "Title" });
        sfGridColumns.Add(new TreeGridTextColumn() { MappingName = "Salary" });
    }
}
```

---

## GridLines Customization

### GridLinesVisibility Options

Control grid line visibility with four options: Both, Horizontal, Vertical, and None.

**Both (Default):**
```xml
<syncfusion:SfTreeGrid x:Name="sfTreeGrid"
                       GridLinesVisibility="Both"
                       ItemsSource="{Binding Employees}"/>
```

```csharp
this.sfTreeGrid.GridLinesVisibility = GridLinesVisibility.Both;
```

**Horizontal Only:**
```xml
<syncfusion:SfTreeGrid x:Name="sfTreeGrid"
                       GridLinesVisibility="Horizontal"
                       ItemsSource="{Binding Employees}"/>
```

```csharp
this.sfTreeGrid.GridLinesVisibility = GridLinesVisibility.Horizontal;
```

**Vertical Only:**
```xml
<syncfusion:SfTreeGrid x:Name="sfTreeGrid"
                       GridLinesVisibility="Vertical"
                       ItemsSource="{Binding Employees}"/>
```

```csharp
this.sfTreeGrid.GridLinesVisibility = GridLinesVisibility.Vertical;
```

**No Grid Lines:**
```xml
<syncfusion:SfTreeGrid x:Name="sfTreeGrid"
                       GridLinesVisibility="None"
                       ItemsSource="{Binding Employees}"/>
```

```csharp
this.sfTreeGrid.GridLinesVisibility = GridLinesVisibility.None;
```

### Header Lines Visibility

Customize header row grid lines independently:

```xml
<syncfusion:SfTreeGrid x:Name="sfTreeGrid"
                       HeaderLinesVisibility="Horizontal"
                       ItemsSource="{Binding Employees}"/>
```

```csharp
this.sfTreeGrid.HeaderLinesVisibility = GridLinesVisibility.Horizontal;
```

---

## Serialization and Deserialization

### Basic Serialization

Save TreeGrid settings to XML file:

```csharp
using (var file = File.Create("TreeGrid.xml"))
{
    treeGrid.Serialize(file);
}
```

### Serialize to Stream

```csharp
FileStream stream = new FileStream("TreeGrid", FileMode.Create);
this.treeGrid.Serialize(stream);
```

### Serialization Options

Customize serialization with TreeGridSerializationOptions:

```csharp
using (var file = File.Create("TreeGrid.xml"))
{
    TreeGridSerializationOptions options = new TreeGridSerializationOptions();
    options.SerializeSorting = false;
    options.SerializeFiltering = false;
    options.SerializeColumns = true;
    options.SerializeStackedHeaders = true;
    treeGrid.Serialize(file, options);
}
```

### Basic Deserialization

Load TreeGrid settings from XML file:

```csharp
using (var file = File.Open("TreeGrid.xml", FileMode.Open))
{
    treeGrid.Deserialize(file);
}
```

### Deserialize from Stream

```csharp
FileStream fileStream = new FileStream("TreeGrid", FileMode.Open);
this.treeGrid.Deserialize(fileStream);
```

### Deserialization Options

Customize deserialization with TreeGridDeserializationOptions:

```csharp
using (var file = File.Open("TreeGrid.xml", FileMode.Open))
{
    TreeGridDeserializationOptions options = new TreeGridDeserializationOptions();
    options.DeserializeSorting = true;
    options.DeserializeFiltering = true;
    options.DeserializeColumns = true;
    options.DeserializeStackedHeaders = false;
    treeGrid.Deserialize(file, options);
}
```

### Custom Column Serialization

Create custom serialization for custom column types:

**Step 1: Define Serializable Custom Column**
```csharp
[DataContract(Name = "SerializableCustomTreeGridColumn")]
public class SerializableCustomTreeGridColumn : SerializableTreeGridColumn
{
    [DataMember]
    public string DateMappingName { get; set; }
}
```

**Step 2: Create Custom Serialization Controller**
```csharp
public class TreeGridSerializationControllerExt : TreeGridSerializationController
{
    public TreeGridSerializationControllerExt(SfTreeGrid treeGrid)
        : base(treeGrid)
    {
    }
    
    protected override SerializableTreeGridColumn GetSerializableTreeGridColumn(TreeGridColumn column)
    {
        if (column.MappingName == "DOJ")
        {
            return new SerializableCustomTreeGridColumn();
        }
        return base.GetSerializableTreeGridColumn(column);
    }
    
    protected override void StoreTreeGridColumnProperties(TreeGridColumn column, 
                                                          SerializableTreeGridColumn serializableColumn)
    {
        base.StoreTreeGridColumnProperties(column, serializableColumn);
        
        if (column is TreeGridDatePickerColumn)
            (serializableColumn as SerializableCustomTreeGridColumn).DateMappingName = 
                (column as TreeGridDatePickerColumn).DateMappingName;
    }
    
    public override Type[] KnownTypes()
    {
        var types = base.KnownTypes();
        var list = types.Cast<Type>().ToList();
        list.Add(typeof(SerializableCustomTreeGridColumn));
        return list.ToArray();
    }
    
    protected override TreeGridColumn GetTreeGridColumn(SerializableTreeGridColumn serializableColumn)
    {
        if (serializableColumn is SerializableCustomTreeGridColumn)
            return new TreeGridDatePickerColumn();
        return base.GetTreeGridColumn(serializableColumn);
    }
    
    protected override void RestoreColumnProperties(SerializableTreeGridColumn serializableColumn, 
                                                    TreeGridColumn column)
    {
        base.RestoreColumnProperties(serializableColumn, column);
        
        if (column is TreeGridDatePickerColumn)
            (column as TreeGridDatePickerColumn).DateMappingName = 
                (serializableColumn as SerializableCustomTreeGridColumn).DateMappingName;
    }
}
```

**Step 3: Use Custom Controller**
```csharp
treeGrid.SerializationController = new TreeGridSerializationControllerExt(treeGrid);
```

---

## Localization

### Setting Application Culture

Change application culture before initialization:

```csharp
public MainWindow()
{
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo("de");
    InitializeComponent();
}
```

### Creating Resource Files

**Step 1:** Create a Resources folder in your application

**Step 2:** Add default resource file `Syncfusion.SfGrid.WPF.resx`

**Step 3:** Create culture-specific resource file (e.g., `Syncfusion.SfGrid.WPF.de.resx` for German)

**Step 4:** Add Name/Value pairs with localized strings:

| Name | Value (English) | Value (German) |
|------|-----------------|----------------|
| GridAddNewRecordLabel | Add New Row | Neue Zeile hinzufügen |
| GridFilterBarMessagePlaceHolder | Type here to filter | Zum Filtern hier eingeben |

### Custom Assembly or Namespace

Specify custom resource assembly/namespace:

```csharp
public MainWindow()
{
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo("de-DE");
    Syncfusion.UI.Xaml.Grid.GridResourceWrapper.SetResources("Assembly_name", "namespace_name");
    InitializeComponent();
}
```

---

## Theming

### Built-in Themes

Apply themes using SfSkinManager:

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    
    <syncfusion:SfTreeGrid syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}"/>
</Window>
```

### Available Theme Options

- **MaterialLight**
- **MaterialDark**
- **MaterialDarkBlue**
- **MaterialLightBlue**
- **Office2019Colorful**
- **Office2019Black**
- **Office2019White**
- **Office2019DarkGray**
- **Office2019HighContrast**
- **FluentLight**
- **FluentDark**

### Programmatic Theme Application

```csharp
SfSkinManager.SetTheme(this.treeGrid, new Theme("MaterialLight"));
```

---

## Helper Methods and Utilities

### TreeGridIndexResolver

Resolve indices between row/column and node/visible column:

**Get Node from Row Index:**
```csharp
var node = treeGrid.GetNodeAtRowIndex(3);
```

**Resolve Node Index from Row Index:**
```csharp
int nodeIndex = TreeGridIndexResolver.ResolveToNodeIndex(treeGrid, 5);
```

**Resolve Row Index from Node Index:**
```csharp
int rowIndex = TreeGridIndexResolver.ResolveToRowIndex(treeGrid, 3);
```

**Resolve Row Index from Data Object:**
```csharp
int rowIndex = TreeGridIndexResolver.ResolveToRowIndex(treeGrid, dataObject);
```

**Resolve Row Index from TreeNode:**
```csharp
int rowIndex = TreeGridIndexResolver.ResolveToRowIndex(treeGrid, treeNode);
```

**Grid Visible Column Index:**
```csharp
int gridColumnIndex = TreeGridIndexResolver.ResolveToGridVisibleColumnIndex(treeGrid, 2);
```

**Scroll Column Index:**
```csharp
int scrollColumnIndex = TreeGridIndexResolver.ResolveToScrollColumnIndex(treeGrid, 3);
```

**Start Column Index:**
```csharp
int startColumnIndex = TreeGridIndexResolver.ResolveToStartColumnIndex(treeGrid);
```

**Header Index:**
```csharp
int headerIndex = TreeGridIndexResolver.GetHeaderIndex(treeGrid);
```

### Dispose Method

Release TreeGrid resources:

```csharp
this.treeGrid.Dispose();
```

---

## NodeCheckBox Features

### Enable NodeCheckBox

Display checkboxes in expander cells:

```xml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ShowCheckBox="True"
                       CheckBoxSelectionMode="Default"
                       ItemsSource="{Binding PersonDetails}"/>
```

```csharp
treeGrid.ShowCheckBox = true;
treeGrid.CheckBoxSelectionMode = CheckBoxSelectionMode.Default;
```

### Indeterminate State

Enable tri-state checking:

```xml
<syncfusion:SfTreeGrid Name="treeGrid"
                       AllowTriStateChecking="True"
                       ShowCheckBox="True"/>
```

```csharp
treeGrid.AllowTriStateChecking = true;
```

### Recursive Checking

Enable recursive parent-child checkbox state updates:

```xml
<syncfusion:SfTreeGrid Name="treeGrid"
                       EnableRecursiveChecking="True"
                       ShowCheckBox="True"/>
```

```csharp
treeGrid.EnableRecursiveChecking = true;
```

### CheckBox Mapping

Bind checkbox state to data object property:

```xml
<syncfusion:SfTreeGrid Name="treeGrid"
                       CheckBoxMappingName="IsSelected"
                       ShowCheckBox="True"/>
```

```csharp
treeGrid.CheckBoxMappingName = "IsSelected";
```

### Recursive Checking Mode

Control when recursive checking applies:

```xml
<syncfusion:SfTreeGrid Name="treeGrid"
                       EnableRecursiveChecking="True"
                       RecursiveCheckingMode="OnCheck"
                       ShowCheckBox="True"/>
```

```csharp
treeGrid.RecursiveCheckingMode = RecursiveCheckingMode.OnCheck;
```

### CheckBox Selection Modes

**Default Mode:** Selection and checkbox state are independent
```csharp
treeGrid.CheckBoxSelectionMode = CheckBoxSelectionMode.Default;
```

**SelectOnCheck Mode:** Selection only through checkbox
```csharp
treeGrid.CheckBoxSelectionMode = CheckBoxSelectionMode.SelectOnCheck;
```

**SynchronizeSelection Mode:** Selection and checkbox state synchronized
```csharp
treeGrid.CheckBoxSelectionMode = CheckBoxSelectionMode.SynchronizeSelection;
```

### Programmatic Checkbox Operations

**Set Checked State:**
```csharp
var treeNode = treeGrid.View.Nodes[0];
treeNode.SetCheckedState(true);
```

**Set Checked State Without Recursive Update:**
```csharp
var treeNode = treeGrid.View.Nodes[0];
treeNode.SetCheckedState(true, false, false);
```

**Get Checked Nodes:**
```csharp
var checkedNodes = treeGrid.GetCheckedNodes();
```

**Get All Checked Nodes (Including Not in View):**
```csharp
var allCheckedNodes = treeGrid.GetCheckedNodes(true);
```

### NodeCheckStateChanged Event

Handle checkbox state changes:

```csharp
treeGrid.NodeCheckStateChanged += TreeGrid_NodeCheckStateChanged;

private void TreeGrid_NodeCheckStateChanged(object sender, NodeCheckStateChangedEventArgs e)
{
    var node = e.Node;
    var isChecked = node.IsChecked;
}
```

---

## ToolTip Customization

### Enable Cell ToolTips

Enable tooltips for all cells:

```xml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ShowToolTip="True"
                       ItemsSource="{Binding EmployeeDetails}"/>
```

```csharp
this.treeGrid.ShowToolTip = true;
```

### Enable Column-Specific ToolTips

Enable tooltip for specific column:

```xml
<syncfusion:TreeGridTextColumn HeaderText="First Name" 
                               MappingName="FirstName" 
                               ShowToolTip="True"/>
```

```csharp
this.treeGrid.Columns["FirstName"].ShowToolTip = true;
```

### Header ToolTips

Enable header tooltips:

```xml
<syncfusion:TreeGridTextColumn HeaderText="First Name" 
                               MappingName="FirstName" 
                               ShowHeaderToolTip="True"/>
```

```csharp
this.treeGrid.Columns["FirstName"].ShowHeaderToolTip = true;
```

### ToolTip Style Customization

Customize tooltip appearance:

```xml
<Window.Resources>
    <Style TargetType="ToolTip">
        <Setter Property="BorderThickness" Value="1,1,1,1"/>
        <Setter Property="BorderBrush" Value="Red"/>
        <Setter Property="Background" Value="SkyBlue"/>
    </Style>
</Window.Resources>
```

### ToolTip Template

Custom tooltip template:

```xml
<Window.Resources>
    <DataTemplate x:Key="TemplateToolTip">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Image Height="100" Width="100" Source="{Binding LastName}"/>
            <TextBlock Grid.Row="1" Text="{Binding LastName}" HorizontalAlignment="Center"/>
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:TreeGridTextColumn HeaderText="Last Name" 
                               MappingName="LastName" 
                               ToolTipTemplate="{StaticResource TemplateToolTip}" 
                               ShowToolTip="True"/>
```

### ToolTip Template with CellBoundValue

Use TreeGridDataContextHelper for template binding:

```xml
<syncfusion:TreeGridTextColumn MappingName="Salary"
                               SetCellBoundToolTip="True"
                               ToolTipTemplate="{StaticResource TemplateToolTip}"/>
```

Access properties:
- `Record`: Underlying data object
- `Value`: Cell value

### ToolTip Template Selector

Conditional tooltip templates:

```xml
<Window.Resources>
    <DataTemplate x:Key="ToolTip1">
        <Grid>
            <TextBlock Text="{Binding Record.Id}" FontWeight="Bold" Foreground="Red"/>
        </Grid>
    </DataTemplate>
    <DataTemplate x:Key="ToolTip2">
        <Grid>
            <TextBlock Text="{Binding Record.Id}" FontWeight="Bold" Foreground="Green"/>
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:TreeGridTextColumn MappingName="Id" 
                               SetCellBoundToolTip="True"
                               ShowToolTip="True">
    <syncfusion:TreeGridTextColumn.ToolTipTemplateSelector>
        <local:ToolTipTemplateSelector DefaultTemplate="{StaticResource ToolTip1}"
                                       AlternateTemplate="{StaticResource ToolTip2}"/>
    </syncfusion:TreeGridTextColumn.ToolTipTemplateSelector>
</syncfusion:TreeGridTextColumn>
```

```csharp
public class ToolTipTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate AlternateTemplate { get; set; }
    
    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        var treeGridData = item as TreeGridDataContextHelper;
        if (treeGridData == null)
            return this.DefaultTemplate;
        
        if ((treeGridData.Record as Employee).Id == (int)treeGridData.Value && 
            ((int)treeGridData.Value % 2) == 0)
            return this.AlternateTemplate;
        else
            return this.DefaultTemplate;
    }
}
```

### CellToolTipOpening Event

Handle tooltip opening:

```csharp
this.treeGrid.CellToolTipOpening += TreeGrid_CellToolTipOpening;

private void TreeGrid_CellToolTipOpening(object sender, TreeGridCellToolTipOpeningEventArgs e)
{
    var column = e.Column;
    var node = e.Node;
    var record = e.Record;
    var rowColumnIndex = e.RowColumnIndex;
    var toolTip = e.ToolTip;
}
```

---

## UI Automation and Accessibility

### Enable Coded UI Testing

Enable UI automation support:

```csharp
using Syncfusion.UI.Xaml.Grid;

public MainWindow()
{
    InitializeComponent();
    AutomationPeerHelper.EnableCodedUI = true;
}
```

### Automation Properties

TreeGrid exposes the following automation properties:

**SfTreeGrid:**
- RowCount
- ColumnCount
- SelectionMode
- SelectionUnit
- SelectionIndex
- SelectedItemCount

**TreeGridCell:**
- CellValue
- FormattedValue
- RowIndex
- ColumnIndex
- ColumnName

**TreeGridHeaderCell:**
- MappingName
- SortDirection
- SortNumberVisibility

**TreeGridRowHeaderCell:**
- RowErrorMessage
- RowIndex
- State

### Automation Peer Classes

| Control | Automation Peer | Property Provider |
|---------|----------------|-------------------|
| SfTreeGrid | SfTreeGridAutomationPeer | SfTreeGridPropertyProvider |
| TreeGridCell | TreeGridCellAutomationPeer | SfTreeGridCellPropertyProvider |
| TreeGridHeaderCell | TreeGridHeaderCellAutomationPeer | SfTreeGridHeaderCellPropertyProvider |
| TreeGridRowHeaderCell | TreeGridRowHeaderCellAutomationPeer | SfTreeGridRowHeaderCellPropertyProvider |
| TreeGridStackedHeaderCell | TreeGridStackedHeaderCellAutomationPeer | SfTreeGridStackedHeaderCellPropertyProvider |
| TreeGridExpanderCell | TreeGridExpanderCellAutomationPeer | SfTreeGridExpanderCellPropertyProvider |

---

## Best Practices

### MVVM Pattern

1. **Use NotificationObject or INotifyPropertyChanged** for all ViewModel properties
2. **Leverage ElementName binding** for complex scenarios
3. **Use RelayCommand or DelegateCommand** for command implementations
4. **Bind entire Columns collection** when dynamic column generation is needed
5. **Keep ViewModels testable** by avoiding direct UI references

### Serialization

1. **Always use try-catch** when serializing/deserializing
2. **Validate XML files** before deserialization
3. **Use custom serialization controllers** for custom column types
4. **Store serialization options** in application settings
5. **Version your serialization format** for backward compatibility

### Performance

1. **Dispose TreeGrid** when no longer needed
2. **Use selective serialization** to reduce file size
3. **Implement lazy loading** for large datasets
4. **Use virtualization** for better performance
5. **Cache resolved indices** when frequently accessed

### Localization

1. **Set culture early** in application lifecycle
2. **Test with pseudo-localization** during development
3. **Use resource managers** for dynamic string loading
4. **Provide fallback resources** for missing translations
5. **Consider RTL layouts** for international applications

### Theming

1. **Apply themes consistently** across the application
2. **Use theme-aware colors** in custom templates
3. **Test with multiple themes** during development
4. **Avoid hardcoded colors** that conflict with themes
5. **Leverage SfSkinManager** for centralized theme management

---

## Troubleshooting

### MVVM Binding Issues

**Problem:** SelectedItem binding not updating
**Solution:** Ensure Mode=TwoWay and UpdateSourceTrigger=PropertyChanged

**Problem:** Command not executing
**Solution:** Verify ElementName binding points to TreeGrid and DataContext is correct

**Problem:** ComboBox ItemsSource not populating
**Solution:** Check ElementName binding and ensure collection is initialized

### Serialization Issues

**Problem:** Deserialization throws exception
**Solution:** Verify XML file format and ensure all custom types are registered in KnownTypes

**Problem:** Custom columns not serializing
**Solution:** Implement custom serialization controller and override required methods

**Problem:** Settings not persisting
**Solution:** Check file permissions and ensure file path is valid

### Localization Issues

**Problem:** Localized strings not appearing
**Solution:** Verify resource file naming convention and culture code

**Problem:** Custom assembly localization not working
**Solution:** Use GridResourceWrapper.SetResources with correct assembly and namespace

**Problem:** Missing translations
**Solution:** Ensure all required keys exist in culture-specific resource file

### Theme Issues

**Problem:** Theme not applying
**Solution:** Ensure SfSkinManager assembly is referenced and theme name is correct

**Problem:** Custom styles conflicting with theme
**Solution:** Use theme-aware colors and avoid overriding base styles

### NodeCheckBox Issues

**Problem:** Recursive checking not working
**Solution:** Ensure EnableRecursiveChecking=True and check data structure

**Problem:** CheckBox state not synchronizing
**Solution:** Verify CheckBoxSelectionMode is set to SynchronizeSelection

**Problem:** CheckBox disabled
**Solution:** Check IsCheckBoxEnabled property in style

### ToolTip Issues

**Problem:** ToolTip not appearing
**Solution:** Verify ShowToolTip=True at grid or column level

**Problem:** ToolTip template not applying
**Solution:** Ensure template is defined in resources and referenced correctly

**Problem:** ToolTip showing incorrect data
**Solution:** Check SetCellBoundToolTip property and template binding

### UI Automation Issues

**Problem:** Coded UI not recognizing controls
**Solution:** Ensure AutomationPeerHelper.EnableCodedUI=True before InitializeComponent

**Problem:** Properties not accessible in automation
**Solution:** Verify extension assemblies are installed in correct location

**Problem:** Automation tests failing
**Solution:** Check control names match expected automation peer class names

---

## Additional Resources

### Code Samples

Complete working examples demonstrating advanced features are available in the Syncfusion WPF TreeGrid GitHub repository.

### Performance Tips

- Use `SuspendNotifications()` and `ResumeNotifications()` for bulk updates
- Implement `INotifyPropertyChanged` efficiently
- Use value converters for complex data transformations
- Cache frequently accessed data

### Security Considerations

- Validate serialized XML before deserialization
- Sanitize user input in custom templates
- Implement proper access controls for sensitive data
- Use encrypted storage for serialized settings

### Migration Guide

When upgrading from older versions:

1. Check for breaking changes in release notes
2. Update automation peer implementations
3. Verify serialization compatibility
4. Test themes with new version
5. Update resource files for new localization keys

---

**Document Version:** 1.0  
**Last Updated:** March 2026  
**Platform:** WPF  
**Control:** SfTreeGrid  

For the latest updates and additional documentation, refer to the official Syncfusion WPF TreeGrid documentation and GitHub examples repository.

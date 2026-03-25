# Editing in WPF TreeView (SfTreeView)

## Table of Contents
- [Overview](#overview)
- [Enabling Editing](#enabling-editing)
- [Edit Triggers](#edit-triggers)
- [EditTemplate Configuration](#edittemplate-configuration)
- [Programmatic Editing](#programmatic-editing)
- [Editing Events](#editing-events)
- [Validation](#validation)
- [IEditableObject Support](#ieditableobject-support)

## Overview

The WPF TreeView supports inline editing of tree node content, allowing users to modify node text directly within the tree. Editing can be triggered by keyboard (F2), mouse clicks, or programmatically. The control provides full customization through templates and events for validation and commit/cancel logic.

## Enabling Editing

Enable editing by setting the `AllowEditing` property to `true`:

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       AllowEditing="True"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders">
```

```csharp
treeView.AllowEditing = true;
```

**Important:** For **Bound Mode**, you must define an `EditTemplate`. For **Unbound Mode**, a default TextBox is provided.

## Edit Triggers

Control how users enter edit mode using the `EditTrigger` property:

### None (Default - F2 Key Only)

```xml
<syncfusion:SfTreeView EditTrigger="None"/>
```

**Behavior:**
- Press **F2** key on selected item to edit
- No mouse click editing

**Use Cases:**
- Prevent accidental edits
- Require explicit edit action

### Tap (Single Click)

```xml
<syncfusion:SfTreeView EditTrigger="Tap"/>
```

```csharp
treeView.EditTrigger = TreeViewEditTrigger.Tap;
```

**Behavior:**
- Single click on selected item enters edit mode
- F2 key also works

**Use Cases:**
- Quick editing workflows
- Touch-friendly applications

### DoubleTap (Double Click)

```xml
<syncfusion:SfTreeView EditTrigger="DoubleTap"/>
```

```csharp
treeView.EditTrigger = TreeViewEditTrigger.DoubleTap;
```

**Behavior:**
- Double click on item enters edit mode
- F2 key also works
- Single click for selection

**Use Cases:**
- Traditional desktop behavior
- Reduce accidental edits
- Windows Explorer-like experience

## EditTemplate Configuration

### Basic EditTemplate (Bound Mode)

For data-bound TreeViews, define an `EditTemplate`:

```xml
<syncfusion:SfTreeView x:Name="treeView"
                       AllowEditing="True"
                       EditTrigger="DoubleTap"
                       ItemsSource="{Binding Folders}"
                       ChildPropertyName="SubFolders">
    
    <!-- Display template -->
    <syncfusion:SfTreeView.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding FolderName}" 
                       VerticalAlignment="Center"/>
        </DataTemplate>
    </syncfusion:SfTreeView.ItemTemplate>
    
    <!-- Edit template -->
    <syncfusion:SfTreeView.EditTemplate>
        <DataTemplate>
            <TextBox Text="{Binding FolderName, Mode=TwoWay}" 
                     VerticalContentAlignment="Center"
                     Height="{Binding ItemHeight, ElementName=treeView}"
                     Margin="-4,0,-4,0"/>
        </DataTemplate>
    </syncfusion:SfTreeView.EditTemplate>
</syncfusion:SfTreeView>
```

**Key Points:**
- Bind TextBox to the data property you want to edit
- Use `Mode=TwoWay` binding
- Match height to `ItemHeight` for consistent appearance
- Adjust margin for visual alignment

### EditTemplate with Validation

```xml
<syncfusion:SfTreeView.EditTemplate>
    <DataTemplate>
        <TextBox Text="{Binding FolderName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"
                 VerticalContentAlignment="Center"
                 MaxLength="50">
            <TextBox.Style>
                <Style TargetType="TextBox">
                    <Style.Triggers>
                        <DataTrigger Binding="{Binding Path=(Validation.HasError), RelativeSource={RelativeSource Self}}" Value="True">
                            <Setter Property="BorderBrush" Value="Red"/>
                            <Setter Property="BorderThickness" Value="2"/>
                        </DataTrigger>
                    </Style.Triggers>
                </Style>
            </TextBox.Style>
        </TextBox>
    </DataTemplate>
</syncfusion:SfTreeView.EditTemplate>
```

### EditTemplateSelector for Different Node Types

For scenarios where different nodes require different edit controls:

```csharp
public class FileEditTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderEditTemplate { get; set; }
    public DataTemplate FileEditTemplate { get; set; }

    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        var fileModel = item as FileModel;
        if (fileModel != null)
        {
            return fileModel.IsFolder ? FolderEditTemplate : FileEditTemplate;
        }
        return base.SelectTemplate(item, container);
    }
}
```

```xml
<Window.Resources>
    <local:FileEditTemplateSelector x:Key="editTemplateSelector">
        <local:FileEditTemplateSelector.FolderEditTemplate>
            <DataTemplate>
                <TextBox Text="{Binding FolderName, Mode=TwoWay}"/>
            </DataTemplate>
        </local:FileEditTemplateSelector.FolderEditTemplate>
        <local:FileEditTemplateSelector.FileEditTemplate>
            <DataTemplate>
                <TextBox Text="{Binding FileName, Mode=TwoWay}"/>
            </DataTemplate>
        </local:FileEditTemplateSelector.FileEditTemplate>
    </local:FileEditTemplateSelector>
</Window.Resources>

<syncfusion:SfTreeView EditTemplateSelector="{StaticResource editTemplateSelector}"/>
```

## Programmatic Editing

### Begin Edit

Start editing programmatically using `BeginEdit`:

```csharp
// Edit first node
if (treeView.Nodes.Count > 0)
{
    treeView.BeginEdit(treeView.Nodes[0]);
}

// Edit specific item
var folderToEdit = viewModel.Folders.FirstOrDefault(f => f.FolderName == "Documents");
if (folderToEdit != null)
{
    // Find TreeViewNode for the data item
    var node = treeView.GetNode(folderToEdit);
    if (node != null)
    {
        treeView.BeginEdit(node);
    }
}

// Edit selected item
if (treeView.SelectedItem != null)
{
    var selectedNode = treeView.GetNode(treeView.SelectedItem);
    treeView.BeginEdit(selectedNode);
}
```

**Note:** When `BeginEdit` is called, `CurrentItem` is automatically set to the node being edited.

### End Edit

Commit editing programmatically using `EndEdit`:

```csharp
// End edit for specific node
var nodeBeingEdited = treeView.Nodes[0];
treeView.EndEdit(nodeBeingEdited);

// End current edit
if (treeView.CurrentItem != null)
{
    var currentNode = treeView.GetNode(treeView.CurrentItem);
    treeView.EndEdit(currentNode);
}
```

**Commit Behavior:**
- Changes are committed to the data model
- Edit mode exits
- Item returns to display template

### Cancel Edit Programmatically

Use the `ItemBeginEdit` event to cancel:

```csharp
treeView.ItemBeginEdit += (s, e) =>
{
    // Cancel edit for protected items
    var folder = e.Node.Content as FolderModel;
    if (folder != null && folder.IsProtected)
    {
        e.Cancel = true;
        MessageBox.Show("Cannot edit protected folders");
    }
};
```

## Editing Events

### ItemBeginEdit Event

Fires before editing begins. Can be canceled.

```csharp
treeView.ItemBeginEdit += TreeView_ItemBeginEdit;

private void TreeView_ItemBeginEdit(object sender, TreeViewItemBeginEditEventArgs e)
{
    var node = e.Node;
    var folder = node.Content as FolderModel;
    
    // Cancel edit for system folders
    if (folder != null && folder.IsSystem)
    {
        e.Cancel = true;
        MessageBox.Show("System folders cannot be edited");
        return;
    }
    
    // Log edit start
    Debug.WriteLine($"Editing started: {folder?.FolderName}");
}
```

**Event Arguments:**
- `Node` - TreeViewNode being edited
- `Cancel` - Set to `true` to prevent editing

### ItemEndEdit Event

Fires after editing ends. Cannot be canceled.

```csharp
treeView.ItemEndEdit += TreeView_ItemEndEdit;

private void TreeView_ItemEndEdit(object sender, TreeViewItemEndEditEventArgs e)
{
    var node = e.Node;
    var folder = node.Content as FolderModel;
    
    // Validate edited value
    if (folder != null && string.IsNullOrWhiteSpace(folder.FolderName))
    {
        MessageBox.Show("Folder name cannot be empty");
        folder.FolderName = "Untitled Folder"; // Reset to default
    }
    
    // Save changes to database
    SaveFolder(folder);
    
    // Log edit completion
    Debug.WriteLine($"Editing completed: {folder?.FolderName}");
}
```

**Event Arguments:**
- `Node` - TreeViewNode that was edited

## Validation

### Property-Level Validation with IDataErrorInfo

Implement validation in your data model:

```csharp
public class FolderModel : INotifyPropertyChanged, IDataErrorInfo
{
    private string folderName;

    public string FolderName
    {
        get { return folderName; }
        set
        {
            folderName = value;
            OnPropertyChanged(nameof(FolderName));
        }
    }

    public string Error => null;

    public string this[string columnName]
    {
        get
        {
            if (columnName == nameof(FolderName))
            {
                if (string.IsNullOrWhiteSpace(FolderName))
                    return "Folder name cannot be empty";
                
                if (FolderName.IndexOfAny(Path.GetInvalidFileNameChars()) >= 0)
                    return "Folder name contains invalid characters";
                
                if (FolderName.Length > 255)
                    return "Folder name too long (max 255 characters)";
            }
            return null;
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**EditTemplate with Validation Binding:**

```xml
<syncfusion:SfTreeView.EditTemplate>
    <DataTemplate>
        <TextBox Text="{Binding FolderName, Mode=TwoWay, ValidatesOnDataErrors=True}"
                 VerticalContentAlignment="Center">
            <TextBox.Style>
                <Style TargetType="TextBox">
                    <Setter Property="BorderBrush" Value="Gray"/>
                    <Style.Triggers>
                        <Trigger Property="Validation.HasError" Value="True">
                            <Setter Property="BorderBrush" Value="Red"/>
                            <Setter Property="ToolTip" 
                                    Value="{Binding RelativeSource={RelativeSource Self}, 
                                            Path=(Validation.Errors)[0].ErrorContent}"/>
                        </Trigger>
                    </Style.Triggers>
                </Style>
            </TextBox.Style>
        </TextBox>
    </DataTemplate>
</syncfusion:SfTreeView.EditTemplate>
```

### Event-Based Validation

```csharp
private void TreeView_ItemEndEdit(object sender, TreeViewItemEndEditEventArgs e)
{
    var folder = e.Node.Content as FolderModel;
    if (folder == null) return;

    // Validate and provide feedback
    if (string.IsNullOrWhiteSpace(folder.FolderName))
    {
        MessageBox.Show("Folder name cannot be empty", "Validation Error");
        folder.FolderName = "Untitled Folder";
        return;
    }

    // Check for duplicates
    if (IsDuplicateName(folder.FolderName, folder))
    {
        MessageBox.Show("A folder with this name already exists", "Validation Error");
        folder.FolderName = GetUniqueName(folder.FolderName);
    }
}

private bool IsDuplicateName(string name, FolderModel excludeFolder)
{
    return viewModel.Folders.Any(f => 
        f != excludeFolder && 
        f.FolderName.Equals(name, StringComparison.OrdinalIgnoreCase));
}
```

## IEditableObject Support

Implement `IEditableObject` to enable ESC key rollback:

```csharp
public class FolderModel : INotifyPropertyChanged, IEditableObject
{
    private string folderName;
    private FolderModel backupData;

    public string FolderName
    {
        get { return folderName; }
        set
        {
            folderName = value;
            OnPropertyChanged(nameof(FolderName));
        }
    }

    // IEditableObject implementation
    public void BeginEdit()
    {
        // Take backup of current state
        backupData = new FolderModel
        {
            FolderName = this.FolderName
        };
        Debug.WriteLine($"BeginEdit: {FolderName}");
    }

    public void CancelEdit()
    {
        // Restore from backup when ESC is pressed
        if (backupData != null)
        {
            this.FolderName = backupData.FolderName;
            backupData = null;
        }
        Debug.WriteLine($"CancelEdit: {FolderName}");
    }

    public void EndEdit()
    {
        // Commit changes
        backupData = null;
        Debug.WriteLine($"EndEdit: {FolderName}");
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**Behavior with IEditableObject:**
- **BeginEdit:** Called when edit starts - take backup
- **CancelEdit:** Called when ESC pressed - restore backup
- **EndEdit:** Called when Enter pressed or focus lost - commit changes

**Without IEditableObject:** ESC key does not rollback changes.

## Common Patterns

### Rename on Double-Click

```xml
<syncfusion:SfTreeView AllowEditing="True"
                       EditTrigger="DoubleTap"
                       SelectionMode="Single"/>
```

### Edit Selected Item on Button Click

```xml
<StackPanel>
    <Button Content="Rename Selected" Click="RenameButton_Click"/>
    <syncfusion:SfTreeView x:Name="treeView" AllowEditing="True"/>
</StackPanel>
```

```csharp
private void RenameButton_Click(object sender, RoutedEventArgs e)
{
    if (treeView.SelectedItem != null)
    {
        var node = treeView.GetNode(treeView.SelectedItem);
        if (node != null)
        {
            treeView.BeginEdit(node);
        }
    }
    else
    {
        MessageBox.Show("Please select an item to rename");
    }
}
```

### Prevent Editing Root Nodes

```csharp
treeView.ItemBeginEdit += (s, e) =>
{
    // Only allow editing child nodes
    if (e.Node.Level == 0)
    {
        e.Cancel = true;
        MessageBox.Show("Root nodes cannot be edited");
    }
};
```

### Auto-Select Text on Edit

```xml
<syncfusion:SfTreeView.EditTemplate>
    <DataTemplate>
        <TextBox Text="{Binding FolderName, Mode=TwoWay}"
                 Loaded="EditTextBox_Loaded"/>
    </DataTemplate>
</syncfusion:SfTreeView.EditTemplate>
```

```csharp
private void EditTextBox_Loaded(object sender, RoutedEventArgs e)
{
    var textBox = sender as TextBox;
    if (textBox != null)
    {
        textBox.Focus();
        textBox.SelectAll();
    }
}
```

## Best Practices

1. **Always Define EditTemplate in Bound Mode:** Required for data binding
2. **Use Two-Way Binding:** Ensure changes update the data model
3. **Implement IEditableObject:** Enable ESC key rollback
4. **Validate in ItemEndEdit:** Perform validation after editing completes
5. **Provide Visual Feedback:** Use validation styles and tooltips
6. **Match Heights:** Set EditTemplate height to ItemHeight
7. **Consider Edit Triggers:** Choose appropriate trigger for your UX
8. **Handle Empty Values:** Prevent invalid states

## Troubleshooting

**Editing not working:**
- Verify `AllowEditing="True"`
- Check `EditTemplate` is defined (Bound Mode)
- Ensure proper data binding in EditTemplate

**Changes not saving:**
- Use `Mode=TwoWay` binding
- Check model implements `INotifyPropertyChanged`
- Verify property setter is called

**ESC key not rolling back:**
- Implement `IEditableObject` in data model
- Ensure backup logic in `BeginEdit` and `CancelEdit`

**EditTemplate misaligned:**
- Set `Height="{Binding ItemHeight, ElementName=treeView}"`
- Adjust margins: `Margin="-4,0,-4,0"`
- Use `VerticalContentAlignment="Center"`

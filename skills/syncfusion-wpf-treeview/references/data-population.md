# Data Population in WPF TreeView (SfTreeView)

## Table of Contents
- [Overview](#overview)
- [Bound Mode (Data Binding)](#bound-mode-data-binding)
  - [Create Data Model](#create-data-model)
  - [Simple Hierarchical Binding](#simple-hierarchical-binding)
  - [Complex Hierarchical Binding with HierarchyPropertyDescriptors](#complex-hierarchical-binding-with-hierarchypropertydescriptors)
- [Unbound Mode (Manual Nodes)](#unbound-mode-manual-nodes)
- [Node Population Modes](#node-population-modes)
- [Notification Subscription](#notification-subscription)
- [ItemTemplate Configuration](#itemtemplate-configuration)

## Overview

The WPF TreeView (SfTreeView) supports two primary modes for populating data:

1. **Bound Mode:** Bind to a data source using `ItemsSource` property (recommended for MVVM)
2. **Unbound Mode:** Manually create `TreeViewNode` objects and add them to the `Nodes` collection

Choose bound mode for data-driven applications with MVVM architecture, and unbound mode for scenarios where you need direct control over node creation.

## Bound Mode (Data Binding)

Bound mode is the recommended approach for most applications. It follows the MVVM pattern and automatically reflects data changes in the UI.

### Create Data Model

Define your hierarchical data model with `INotifyPropertyChanged` support:

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class FileManager : INotifyPropertyChanged
{
    private string itemName;
    private ObservableCollection<FileManager> subFiles;

    public string ItemName
    {
        get { return itemName; }
        set
        {
            itemName = value;
            OnPropertyChanged(nameof(ItemName));
        }
    }

    public ObservableCollection<FileManager> SubFiles
    {
        get { return subFiles; }
        set
        {
            subFiles = value;
            OnPropertyChanged(nameof(SubFiles));
        }
    }

    public FileManager()
    {
        SubFiles = new ObservableCollection<FileManager>();
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**Key Requirements:**
- Use `ObservableCollection<T>` for child collections to enable automatic UI updates
- Implement `INotifyPropertyChanged` to notify property changes
- Include a collection property for child items (`SubFiles` in this example)

### Simple Hierarchical Binding

The most common scenario is binding to a self-referencing hierarchical collection where each parent contains a collection of children of the same type.

**Step 1:** Create the ViewModel:

```csharp
public class FileManagerViewModel
{
    public ObservableCollection<FileManager> Folders { get; set; }

    public FileManagerViewModel()
    {
        Folders = new ObservableCollection<FileManager>();
        LoadData();
    }

    private void LoadData()
    {
        // Root folder: Documents
        var documents = new FileManager { ItemName = "Documents" };
        documents.SubFiles.Add(new FileManager { ItemName = "Work" });
        documents.SubFiles.Add(new FileManager { ItemName = "Personal" });
        documents.SubFiles.Add(new FileManager { ItemName = "Projects" });

        // Root folder: Downloads
        var downloads = new FileManager { ItemName = "Downloads" };
        downloads.SubFiles.Add(new FileManager { ItemName = "Software" });
        downloads.SubFiles.Add(new FileManager { ItemName = "Media" });
        
        // Add nested levels
        var projects = documents.SubFiles[2]; // "Projects"
        projects.SubFiles.Add(new FileManager { ItemName = "Client A" });
        projects.SubFiles.Add(new FileManager { ItemName = "Client B" });

        // Root folder: Pictures
        var pictures = new FileManager { ItemName = "Pictures" };
        var vacation = new FileManager { ItemName = "Vacation 2025" };
        vacation.SubFiles.Add(new FileManager { ItemName = "Beach Photos" });
        vacation.SubFiles.Add(new FileManager { ItemName = "Mountain Trip" });
        pictures.SubFiles.Add(vacation);
        pictures.SubFiles.Add(new FileManager { ItemName = "Family" });

        Folders.Add(documents);
        Folders.Add(downloads);
        Folders.Add(pictures);
    }
}
```

**Step 2:** Bind in XAML:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourNamespace">
    
    <Window.DataContext>
        <local:FileManagerViewModel/>
    </Window.DataContext>

    <Grid>
        <syncfusion:SfTreeView x:Name="treeView"
                               ItemsSource="{Binding Folders}"
                               ChildPropertyName="SubFiles">
            <syncfusion:SfTreeView.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding ItemName}" 
                               VerticalAlignment="Center"
                               FontSize="14"/>
                </DataTemplate>
            </syncfusion:SfTreeView.ItemTemplate>
        </syncfusion:SfTreeView>
    </Grid>
</Window>
```

**Critical Properties:**
- `ItemsSource="{Binding Folders}"` - Binds to the root collection
- `ChildPropertyName="SubFiles"` - Specifies which property contains child items
- `ItemTemplate` - Defines how each node is displayed

### Complex Hierarchical Binding with HierarchyPropertyDescriptors

For scenarios where your hierarchy consists of different types at different levels (e.g., Folder → File → SubFile), use `HierarchyPropertyDescriptors`.

**Step 1:** Define multiple model types:

```csharp
// Level 1: Folder
public class Folder : INotifyPropertyChanged
{
    private string folderName;
    private ObservableCollection<File> files;

    public string FolderName
    {
        get { return folderName; }
        set
        {
            folderName = value;
            OnPropertyChanged(nameof(FolderName));
        }
    }

    public ObservableCollection<File> Files
    {
        get { return files; }
        set
        {
            files = value;
            OnPropertyChanged(nameof(Files));
        }
    }

    public Folder()
    {
        Files = new ObservableCollection<File>();
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}

// Level 2: File
public class File : INotifyPropertyChanged
{
    private string fileName;
    private ObservableCollection<SubFile> subFiles;

    public string FileName
    {
        get { return fileName; }
        set
        {
            fileName = value;
            OnPropertyChanged(nameof(FileName));
        }
    }

    public ObservableCollection<SubFile> SubFiles
    {
        get { return subFiles; }
        set
        {
            subFiles = value;
            OnPropertyChanged(nameof(SubFiles));
        }
    }

    public File()
    {
        SubFiles = new ObservableCollection<SubFile>();
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}

// Level 3: SubFile
public class SubFile : INotifyPropertyChanged
{
    private string subFileName;

    public string SubFileName
    {
        get { return subFileName; }
        set
        {
            subFileName = value;
            OnPropertyChanged(nameof(SubFileName));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**Step 2:** Configure HierarchyPropertyDescriptors in XAML:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:treeViewEngine="clr-namespace:Syncfusion.UI.Xaml.TreeView.Engine;assembly=Syncfusion.SfTreeView.WPF"
        xmlns:local="clr-namespace:YourNamespace">
    
    <Window.DataContext>
        <local:HierarchyViewModel/>
    </Window.DataContext>

    <Grid>
        <syncfusion:SfTreeView x:Name="treeView" 
                               ItemsSource="{Binding Folders}">
            <!-- Define hierarchy descriptors for each level -->
            <syncfusion:SfTreeView.HierarchyPropertyDescriptors>
                <!-- Level 1: Folder has Files -->
                <treeViewEngine:HierarchyPropertyDescriptor 
                    TargetType="{x:Type local:Folder}" 
                    ChildPropertyName="Files"/>
                
                <!-- Level 2: File has SubFiles -->
                <treeViewEngine:HierarchyPropertyDescriptor 
                    TargetType="{x:Type local:File}" 
                    ChildPropertyName="SubFiles"/>
                
                <!-- Level 3: SubFile has no children (leaf node) -->
            </syncfusion:SfTreeView.HierarchyPropertyDescriptors>
            
            <!-- Template to display all types -->
            <syncfusion:SfTreeView.ItemTemplate>
                <DataTemplate>
                    <TextBlock VerticalAlignment="Center">
                        <TextBlock.Text>
                            <MultiBinding StringFormat="{}{0}{1}{2}">
                                <Binding Path="FolderName"/>
                                <Binding Path="FileName"/>
                                <Binding Path="SubFileName"/>
                            </MultiBinding>
                        </TextBlock.Text>
                    </TextBlock>
                </DataTemplate>
            </syncfusion:SfTreeView.ItemTemplate>
        </syncfusion:SfTreeView>
    </Grid>
</Window>
```

**Key Concepts:**
- Each `HierarchyPropertyDescriptor` maps a type to its child property
- `TargetType` - The parent type
- `ChildPropertyName` - Property containing children in that type
- TreeView automatically resolves the correct descriptor based on node type

## Unbound Mode (Manual Nodes)

Use unbound mode when you need direct control over node creation or when building a tree structure programmatically without a data source.

### Creating Nodes in XAML

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:Engine="clr-namespace:Syncfusion.UI.Xaml.TreeView.Engine;assembly=Syncfusion.SfTreeView.WPF">
    <Grid>
        <syncfusion:SfTreeView x:Name="treeView">
            <syncfusion:SfTreeView.Nodes>
                <!-- Root Node 1 -->
                <Engine:TreeViewNode Content="Documents" IsExpanded="True">
                    <Engine:TreeViewNode.ChildNodes>
                        <Engine:TreeViewNode Content="Reports"/>
                        <Engine:TreeViewNode Content="Invoices"/>
                        <Engine:TreeViewNode Content="Contracts"/>
                    </Engine:TreeViewNode.ChildNodes>
                </Engine:TreeViewNode>
                
                <!-- Root Node 2 with nested children -->
                <Engine:TreeViewNode Content="Projects" IsExpanded="True">
                    <Engine:TreeViewNode.ChildNodes>
                        <Engine:TreeViewNode Content="Active" IsExpanded="True">
                            <Engine:TreeViewNode.ChildNodes>
                                <Engine:TreeViewNode Content="Project Alpha"/>
                                <Engine:TreeViewNode Content="Project Beta"/>
                            </Engine:TreeViewNode.ChildNodes>
                        </Engine:TreeViewNode>
                        <Engine:TreeViewNode Content="Completed"/>
                        <Engine:TreeViewNode Content="On Hold"/>
                    </Engine:TreeViewNode.ChildNodes>
                </Engine:TreeViewNode>
            </syncfusion:SfTreeView.Nodes>
        </syncfusion:SfTreeView>
    </Grid>
</Window>
```

### Creating Nodes Programmatically

```csharp
using Syncfusion.UI.Xaml.TreeView.Engine;

// Create root nodes
var documentsNode = new TreeViewNode
{
    Content = "Documents",
    IsExpanded = true
};

// Add child nodes
documentsNode.ChildNodes.Add(new TreeViewNode { Content = "Reports" });
documentsNode.ChildNodes.Add(new TreeViewNode { Content = "Invoices" });

var reportsNode = documentsNode.ChildNodes[0];
reportsNode.ChildNodes.Add(new TreeViewNode { Content = "Q1 Report.pdf" });
reportsNode.ChildNodes.Add(new TreeViewNode { Content = "Q2 Report.pdf" });

// Add to TreeView
treeView.Nodes.Add(documentsNode);

var picturesNode = new TreeViewNode { Content = "Pictures" };
picturesNode.ChildNodes.Add(new TreeViewNode { Content = "Vacation" });
picturesNode.ChildNodes.Add(new TreeViewNode { Content = "Family" });

treeView.Nodes.Add(picturesNode);
```

**TreeViewNode Properties:**
- `Content` - Display text or custom object
- `IsExpanded` - Initial expansion state
- `ChildNodes` - Collection of child nodes
- `ParentNode` - Reference to parent (set automatically)
- `Level` - Depth level in hierarchy (0-based)

## Node Population Modes

Control when child nodes are loaded using the `NodePopulationMode` property:

```xml
<syncfusion:SfTreeView NodePopulationMode="OnDemand"
                       ItemsSource="{Binding LargeDataSet}"/>
```

### OnDemand (Default)

Child nodes are populated only when the parent node is expanded:

```csharp
treeView.NodePopulationMode = NodePopulationMode.OnDemand;
```

**Benefits:**
- Faster initial load for large datasets
- Reduces memory consumption
- Ideal for load-on-demand scenarios

**Use When:**
- Working with large hierarchies (1000+ nodes)
- Implementing lazy loading from databases
- Network-based data sources

### Instant

All child nodes are populated immediately when the TreeView is loaded:

```csharp
treeView.NodePopulationMode = NodePopulationMode.Instant;
```

**Benefits:**
- All nodes available immediately
- Simpler for small datasets
- Better for search/filter scenarios

**Use When:**
- Small to medium datasets (< 500 nodes)
- Need to search across all nodes immediately
- Implementing expand-all functionality

## Notification Subscription

Control how TreeView responds to collection and property changes:

```xml
<syncfusion:SfTreeView NotificationSubscriptionMode="CollectionChange"
                       ItemsSource="{Binding DynamicData}"/>
```

### CollectionChange

Updates tree structure when child item collections change:

```csharp
treeView.NotificationSubscriptionMode = NotificationSubscriptionMode.CollectionChange;

// Adding/removing items will automatically update UI
viewModel.Folders[0].SubFiles.Add(new FileManager { ItemName = "New File" });
```

**Use When:**
- Adding or removing nodes dynamically
- Real-time data updates
- Drag-and-drop operations

### PropertyChange

Updates UI when collection properties change:

```csharp
treeView.NotificationSubscriptionMode = NotificationSubscriptionMode.PropertyChange;

// Replacing entire collection will update UI
viewModel.Folders[0].SubFiles = new ObservableCollection<FileManager>(newFiles);
```

**Use When:**
- Replacing entire collections
- Batch updates
- Data refresh scenarios

### None (Default)

No automatic updates. Requires manual refresh:

```csharp
treeView.NotificationSubscriptionMode = NotificationSubscriptionMode.None;

// Manual refresh required
viewModel.Folders.Add(new FileManager { ItemName = "New Folder" });
treeView.RefreshView();
```

**Use When:**
- Static data (no changes after load)
- Performance-critical scenarios
- Manual control over updates

## ItemTemplate Configuration

Customize how each node is displayed using `ItemTemplate`:

### Simple Text Display

```xml
<syncfusion:SfTreeView.ItemTemplate>
    <DataTemplate>
        <TextBlock Text="{Binding ItemName}" 
                   FontSize="14"
                   VerticalAlignment="Center"/>
    </DataTemplate>
</syncfusion:SfTreeView.ItemTemplate>
```

### With Icon and Text

```xml
<syncfusion:SfTreeView.ItemTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal">
            <Image Source="{Binding Icon}" 
                   Width="16" 
                   Height="16" 
                   Margin="0,0,5,0"/>
            <TextBlock Text="{Binding ItemName}" 
                       VerticalAlignment="Center"/>
        </StackPanel>
    </DataTemplate>
</syncfusion:SfTreeView.ItemTemplate>
```

### Conditional Formatting with DataTriggers

```xml
<syncfusion:SfTreeView.ItemTemplate>
    <DataTemplate>
        <TextBlock Text="{Binding ItemName}">
            <TextBlock.Style>
                <Style TargetType="TextBlock">
                    <Setter Property="Foreground" Value="Black"/>
                    <Style.Triggers>
                        <DataTrigger Binding="{Binding IsImportant}" Value="True">
                            <Setter Property="Foreground" Value="Red"/>
                            <Setter Property="FontWeight" Value="Bold"/>
                        </DataTrigger>
                    </Style.Triggers>
                </Style>
            </TextBlock.Style>
        </TextBlock>
    </DataTemplate>
</syncfusion:SfTreeView.ItemTemplate>
```

### Using ItemTemplateSelector

For different templates based on node type or level:

```csharp
public class FileTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        var fileManager = item as FileManager;
        if (fileManager != null)
        {
            return fileManager.SubFiles.Count > 0 ? FolderTemplate : FileTemplate;
        }
        return base.SelectTemplate(item, container);
    }
}
```

```xml
<Window.Resources>
    <local:FileTemplateSelector x:Key="templateSelector">
        <local:FileTemplateSelector.FolderTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding ItemName}" FontWeight="Bold"/>
            </DataTemplate>
        </local:FileTemplateSelector.FolderTemplate>
        <local:FileTemplateSelector.FileTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding ItemName}" FontStyle="Italic"/>
            </DataTemplate>
        </local:FileTemplateSelector.FileTemplate>
    </local:FileTemplateSelector>
</Window.Resources>

<syncfusion:SfTreeView ItemTemplateSelector="{StaticResource templateSelector}"/>
```

## Best Practices

1. **Use Bound Mode for MVVM:** Leverage data binding for cleaner architecture
2. **Choose OnDemand for Large Datasets:** Improve performance with lazy loading
3. **Implement INotifyPropertyChanged:** Enable automatic UI updates
4. **Use ObservableCollection:** For dynamic collections that change
5. **Set NotificationSubscriptionMode:** Match your update pattern (CollectionChange vs PropertyChange)
6. **Optimize ItemTemplate:** Keep templates simple for better performance
7. **Consider HierarchyPropertyDescriptors:** For complex multi-type hierarchies

## Common Scenarios

### File Explorer
Use bound mode with hierarchical data model representing folders and files.

### Organization Chart
Use HierarchyPropertyDescriptors with different types for departments, managers, and employees.

### Category Tree
Use simple hierarchical binding with self-referencing Category objects.

### Static Menu
Use unbound mode for fixed menu structures defined in XAML.

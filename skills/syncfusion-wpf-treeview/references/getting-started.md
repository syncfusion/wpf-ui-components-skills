# Getting Started with WPF TreeView (SfTreeView)

This guide provides a quick overview for getting started with the Syncfusion WPF TreeView (SfTreeView) control. Follow these steps to add the TreeView to your WPF application and display hierarchical data.

## Assembly Deployment

The WPF TreeView control requires the following assembly references:

### Required Assemblies

Add these assemblies to your WPF project:

- **Syncfusion.SfBusyIndicator.WPF** - Core loading indicator support
- **Syncfusion.SfTreeView.WPF** - Main TreeView control assembly
- **Syncfusion.SfGridCommon.WPF** - Common grid utilities

### Installation via NuGet

Install the TreeView package using NuGet Package Manager:

```powershell
Install-Package Syncfusion.SfTreeView.WPF
```

Or through the Package Manager UI in Visual Studio:
1. Right-click on your project → **Manage NuGet Packages**
2. Search for **Syncfusion.SfTreeView.WPF**
3. Click **Install**

The required dependencies will be added automatically.

## Creating Your First TreeView

### Option 1: Adding TreeView by Designer

The TreeView control can be added by dragging it from the Toolbox:

1. Open your WPF window/page in the Visual Studio designer
2. Locate **SfTreeView** in the Toolbox (under Syncfusion Controls)
3. Drag and drop it onto your design surface
4. Assembly references are added automatically

### Option 2: Adding TreeView in XAML

To add the control manually in XAML:

**Step 1:** Import the Syncfusion WPF namespace in your Window/Page:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

For accessing TreeViewNode in XAML (unbound mode):

```xml
xmlns:Engine="clr-namespace:Syncfusion.UI.Xaml.TreeView.Engine;assembly=Syncfusion.SfTreeView.WPF"
```

**Step 2:** Declare the SfTreeView control:

```xml
<Window x:Class="GettingStarted.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="TreeView Example" Height="450" Width="600">
    <Grid>
        <syncfusion:SfTreeView x:Name="treeView" />
    </Grid>
</Window>
```

### Option 3: Adding TreeView in C#

To create the TreeView programmatically:

**Step 1:** Import the namespace in your code-behind:

```csharp
using Syncfusion.UI.Xaml.TreeView;
```

**Step 2:** Create and add the TreeView instance:

```csharp
using Syncfusion.UI.Xaml.TreeView;
using System.Windows;

namespace GettingStarted
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create TreeView instance
            SfTreeView treeView = new SfTreeView
            {
                Width = 300,
                Height = 400
            };
            
            // Add to layout container
            rootGrid.Children.Add(treeView);
        }
    }
}
```

## License Configuration

Syncfusion components require a valid license key. Register it in your application startup:

```csharp
// In App.xaml.cs
using Syncfusion.Licensing;

public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // Register Syncfusion license
        SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY_HERE");
        
        base.OnStartup(e);
    }
}
```

**License Types:**
- **Community License:** Free for qualifying individuals and companies (revenue < $1M USD)
- **Trial License:** 30-day evaluation period
- **Commercial License:** For commercial use

Get your license key from the [Syncfusion website](https://www.syncfusion.com/sales/products).

## Basic TreeView Examples

Now that you've added the control, let's populate it with data. The TreeView supports two modes for data population.

### Example 1: Unbound Mode (Manual Nodes)

Create tree nodes manually using TreeViewNode objects in XAML:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:Engine="clr-namespace:Syncfusion.UI.Xaml.TreeView.Engine;assembly=Syncfusion.SfTreeView.WPF"
        Title="Unbound TreeView" Height="450" Width="400">
    <Grid>
        <syncfusion:SfTreeView Width="300" Height="400">
            <syncfusion:SfTreeView.Nodes>
                <!-- Root Node 1 -->
                <Engine:TreeViewNode Content="Documents" IsExpanded="True">
                    <Engine:TreeViewNode.ChildNodes>
                        <Engine:TreeViewNode Content="Reports"/>
                        <Engine:TreeViewNode Content="Invoices"/>
                        <Engine:TreeViewNode Content="Contracts"/>
                    </Engine:TreeViewNode.ChildNodes>
                </Engine:TreeViewNode>
                
                <!-- Root Node 2 -->
                <Engine:TreeViewNode Content="Pictures" IsExpanded="True">
                    <Engine:TreeViewNode.ChildNodes>
                        <Engine:TreeViewNode Content="Vacation"/>
                        <Engine:TreeViewNode Content="Family"/>
                    </Engine:TreeViewNode.ChildNodes>
                </Engine:TreeViewNode>
                
                <!-- Root Node 3 -->
                <Engine:TreeViewNode Content="Music">
                    <Engine:TreeViewNode.ChildNodes>
                        <Engine:TreeViewNode Content="Rock"/>
                        <Engine:TreeViewNode Content="Jazz"/>
                        <Engine:TreeViewNode Content="Classical"/>
                    </Engine:TreeViewNode.ChildNodes>
                </Engine:TreeViewNode>
            </syncfusion:SfTreeView.Nodes>
        </syncfusion:SfTreeView>
    </Grid>
</Window>
```

**Key Properties:**
- `Content` - Display text for the node
- `IsExpanded` - Whether node is initially expanded
- `ChildNodes` - Collection of child nodes

### Example 2: Bound Mode (Data Binding)

Bind the TreeView to a hierarchical data source. This is the recommended approach for MVVM applications.

**Step 1:** Define your data model:

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class FolderModel : INotifyPropertyChanged
{
    private string folderName;
    private ObservableCollection<FolderModel> subFolders;

    public string FolderName
    {
        get { return folderName; }
        set
        {
            folderName = value;
            OnPropertyChanged(nameof(FolderName));
        }
    }

    public ObservableCollection<FolderModel> SubFolders
    {
        get { return subFolders; }
        set
        {
            subFolders = value;
            OnPropertyChanged(nameof(SubFolders));
        }
    }

    public FolderModel()
    {
        SubFolders = new ObservableCollection<FolderModel>();
    }

    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**Step 2:** Create your ViewModel:

```csharp
using System.Collections.ObjectModel;

public class FileManagerViewModel
{
    public ObservableCollection<FolderModel> Folders { get; set; }

    public FileManagerViewModel()
    {
        Folders = new ObservableCollection<FolderModel>();
        PopulateData();
    }

    private void PopulateData()
    {
        var documents = new FolderModel { FolderName = "Documents" };
        documents.SubFolders.Add(new FolderModel { FolderName = "Work Documents" });
        documents.SubFolders.Add(new FolderModel { FolderName = "Personal Documents" });

        var downloads = new FolderModel { FolderName = "Downloads" };
        downloads.SubFolders.Add(new FolderModel { FolderName = "Software" });
        downloads.SubFolders.Add(new FolderModel { FolderName = "Media" });

        var pictures = new FolderModel { FolderName = "Pictures" };
        pictures.SubFolders.Add(new FolderModel { FolderName = "Vacation 2025" });
        pictures.SubFolders.Add(new FolderModel { FolderName = "Family Photos" });

        Folders.Add(documents);
        Folders.Add(downloads);
        Folders.Add(pictures);
    }
}
```

**Step 3:** Bind in XAML:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourNamespace"
        Title="Bound TreeView" Height="450" Width="400">
    
    <Window.DataContext>
        <local:FileManagerViewModel/>
    </Window.DataContext>

    <Grid>
        <syncfusion:SfTreeView x:Name="treeView"
                               ItemsSource="{Binding Folders}"
                               ChildPropertyName="SubFolders"
                               AutoExpandMode="RootNodes"
                               Width="300" Height="400">
            <syncfusion:SfTreeView.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding FolderName}" 
                               VerticalAlignment="Center"
                               FontSize="14"/>
                </DataTemplate>
            </syncfusion:SfTreeView.ItemTemplate>
        </syncfusion:SfTreeView>
    </Grid>
</Window>
```

**Key Properties for Bound Mode:**
- `ItemsSource` - Bind to your data collection
- `ChildPropertyName` - Property name containing child items (`SubFolders`)
- `ItemTemplate` - DataTemplate to display each node
- `AutoExpandMode` - Control initial expansion (None, RootNodes, AllNodes)

## Theming Support

Apply built-in themes to your TreeView for consistent styling:

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <syncfusion:SfTreeView syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=MaterialLight}"/>
</Window>
```

**Available Themes:**
- MaterialLight, MaterialDark
- FluentLight, FluentDark
- Office2019Colorful, Office2019Black, Office2019White
- And many more

Or programmatically in code:

```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetTheme(this, new Theme("MaterialLight"));
```

## Next Steps

Now that you have a basic TreeView running, explore additional features:

- **Selection:** Enable single or multiple selection modes
- **Checkboxes:** Add checkboxes with recursive state management
- **Drag & Drop:** Reorder nodes with drag-and-drop
- **Editing:** Enable inline editing of node content
- **Load on Demand:** Lazy load child nodes for large hierarchies
- **Customization:** Customize appearance with templates and styles

Refer to the other reference documents for detailed implementation guides on these features.

# Getting Started with DocumentContainer

This guide covers the basics of adding and configuring the Syncfusion WPF DocumentContainer control in your application.

## Assembly Deployment

To use the DocumentContainer control, you need to reference the following assemblies or NuGet packages:

**Required Assemblies:**
- `Syncfusion.Tools.WPF`
- `Syncfusion.Shared.WPF`

**NuGet Package:**
Install the Syncfusion WPF Tools package via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Tools.WPF
```

Or search for "Syncfusion.Tools.WPF" in the Visual Studio NuGet Package Manager UI.

**Additional Resources:**
- [Control Dependencies Documentation](https://help.syncfusion.com/wpf/control-dependencies#documentcontainer)
- [NuGet Package Installation Guide](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages)

## Creating a WPF Project

1. Open Visual Studio
2. Create a new **WPF Application** project
3. Choose .NET Framework 4.5 or later (or .NET Core/.NET 5+)
4. Name your project (e.g., "DocumentContainerDemo")

## Adding Control Through Designer

The easiest way to add DocumentContainer is through the Visual Studio designer:

1. Open the toolbox in Visual Studio
2. Locate the **DocumentContainer** control (under Syncfusion controls)
3. Drag and drop it onto your window designer

**What happens automatically:**
- Required assembly references are added to your project
- Syncfusion namespace is added to XAML
- Control is placed in your window with default settings

## Adding Control Manually in XAML

For more control over the setup, add the DocumentContainer manually in XAML:

### Step 1: Add Assembly References

Right-click your project → Add Reference → Browse to:
- `Syncfusion.Tools.WPF.dll`
- `Syncfusion.Shared.WPF.dll`

Or use NuGet as described above.

### Step 2: Import Syncfusion Namespace

Add the Syncfusion WPF schema to your window's root element:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="DocumentContainerDemo.MainWindow"
        Title="DocumentContainer Sample" Height="600" Width="800">
    <!-- Content here -->
</Window>
```

### Step 3: Declare DocumentContainer

Add the DocumentContainer control inside your layout:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="DocumentContainerDemo.MainWindow"
        Title="DocumentContainer Sample" Height="600" Width="800">
    <Grid>
        <!-- Basic DocumentContainer -->
        <syncfusion:DocumentContainer x:Name="documentContainer" 
                                     Mode="TDI"
                                     VerticalAlignment="Stretch" 
                                     HorizontalAlignment="Stretch"/>
    </Grid>
</Window>
```

## Adding Control Manually in C#

You can also create and configure the DocumentContainer entirely in code:

### Step 1: Add Assembly References

Same as XAML approach - add references to:
- `Syncfusion.Tools.WPF`
- `Syncfusion.Shared.WPF`

### Step 2: Import Namespace

Add the using directive at the top of your code file:

```csharp
using Syncfusion.Windows.Tools.Controls;
```

### Step 3: Create and Add DocumentContainer

```csharp
using System.Windows;
using Syncfusion.Windows.Tools.Controls;

namespace DocumentContainerDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create DocumentContainer instance
            DocumentContainer documentContainer = new DocumentContainer();
            
            // Set properties
            documentContainer.Mode = DocumentContainerMode.TDI;
            
            // Add DocumentContainer as window content
            this.Content = documentContainer;
        }
    }
}
```

## Adding Document Windows

The DocumentContainer can host any WPF framework element (buttons, text blocks, user controls, etc.).

### XAML Approach

```xaml
<syncfusion:DocumentContainer x:Name="documentContainer" Mode="TDI">
    <!-- Document 1: Button -->
    <Button Content="Button Document"/>
    
    <!-- Document 2: TextBox -->
    <TextBox Text="TextBox Document"/>
    
    <!-- Document 3: Custom Control -->
    <Grid>
        <TextBlock Text="Grid Document" 
                   VerticalAlignment="Center" 
                   HorizontalAlignment="Center"/>
    </Grid>
</syncfusion:DocumentContainer>
```

### C# Approach

```csharp
// Create document elements
Button button1 = new Button { Content = "Button Document" };
TextBox textBox1 = new TextBox { Text = "TextBox Document" };
Grid grid1 = new Grid();
grid1.Children.Add(new TextBlock 
{ 
    Text = "Grid Document",
    VerticalAlignment = VerticalAlignment.Center,
    HorizontalAlignment = HorizontalAlignment.Center
});

// Add to DocumentContainer
documentContainer.Items.Add(button1);
documentContainer.Items.Add(textBox1);
documentContainer.Items.Add(grid1);
```

## Setting Headers for Documents

Every document in the container should have a header (tab title). Use the `DocumentContainer.Header` attached property:

### XAML Approach

```xaml
<syncfusion:DocumentContainer x:Name="documentContainer" Mode="TDI">
    <!-- Setting header with attached property -->
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Features Document">
        <FlowDocument TextAlignment="Left">
            <Paragraph TextAlignment="Center">
                <Bold>Document Container Features</Bold>
            </Paragraph>
            <Paragraph>
                This sample exhibits the special features of the Syncfusion 
                Document Container Control for WPF.
            </Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <TextBox syncfusion:DocumentContainer.Header="Editor" 
             TextWrapping="Wrap"
             AcceptsReturn="True"/>
    
    <Button syncfusion:DocumentContainer.Header="Button Tab"
            Content="Click Me"/>
</syncfusion:DocumentContainer>
```

### C# Approach

```csharp
// Create document element
FlowDocumentScrollViewer flowViewer = new FlowDocumentScrollViewer();
FlowDocument flowDoc = new FlowDocument();
flowDoc.Blocks.Add(new Paragraph(new Run("Document content here")));
flowViewer.Document = flowDoc;

// Set header using attached property
DocumentContainer.SetHeader(flowViewer, "Features Document");

// Add to container
documentContainer.Items.Add(flowViewer);

// Setting headers for other elements
TextBox editor = new TextBox();
DocumentContainer.SetHeader(editor, "Editor");
documentContainer.Items.Add(editor);

Button button = new Button { Content = "Click Me" };
DocumentContainer.SetHeader(button, "Button Tab");
documentContainer.Items.Add(button);
```

## Basic TDI Mode Setup

TDI (Tabbed Document Interface) displays documents as tabs:

```xaml
<syncfusion:DocumentContainer Name="documentContainer" Mode="TDI">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 1">
        <FlowDocument>
            <Paragraph>Content for Document 1</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Document 2">
        <FlowDocument>
            <Paragraph>Content for Document 2</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

## Basic MDI Mode Setup

MDI (Multiple Document Interface) displays documents as floating windows:

```xaml
<syncfusion:DocumentContainer Name="documentContainer" 
                              Mode="MDI"
                              CanMDIMinimize="True">
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Window 1">
        <FlowDocument>
            <Paragraph>Content for Window 1</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <FlowDocumentScrollViewer syncfusion:DocumentContainer.Header="Window 2">
        <FlowDocument>
            <Paragraph>Content for Window 2</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

## Complete Working Example

Here's a complete, runnable example demonstrating the DocumentContainer:

**MainWindow.xaml:**
```xaml
<Window x:Class="DocumentContainerDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="DocumentContainer Demo" Height="600" Width="900">
    <Grid>
        <syncfusion:DocumentContainer x:Name="documentContainer" 
                                     Mode="TDI"
                                     SwitchMode="QuickTabs">
            
            <!-- Document 1: Welcome Screen -->
            <Grid syncfusion:DocumentContainer.Header="Welcome">
                <StackPanel Margin="20" VerticalAlignment="Center">
                    <TextBlock Text="Welcome to DocumentContainer" 
                               FontSize="24" 
                               FontWeight="Bold"
                               HorizontalAlignment="Center"/>
                    <TextBlock Text="This demonstrates tabbed document management" 
                               FontSize="14"
                               Margin="0,10,0,0"
                               HorizontalAlignment="Center"/>
                </StackPanel>
            </Grid>
            
            <!-- Document 2: Text Editor -->
            <TextBox syncfusion:DocumentContainer.Header="Text Editor"
                     AcceptsReturn="True"
                     TextWrapping="Wrap"
                     Padding="10"
                     FontFamily="Consolas"
                     FontSize="12"/>
            
            <!-- Document 3: Data Grid -->
            <Grid syncfusion:DocumentContainer.Header="Data View">
                <DataGrid Margin="10" AutoGenerateColumns="True"/>
            </Grid>
            
            <!-- Document 4: Properties -->
            <StackPanel syncfusion:DocumentContainer.Header="Properties" Margin="10">
                <TextBlock Text="Properties Panel" FontWeight="Bold" Margin="0,0,0,10"/>
                <Label Content="Name:"/>
                <TextBox Margin="0,0,0,10"/>
                <Label Content="Description:"/>
                <TextBox Height="60" TextWrapping="Wrap" AcceptsReturn="True"/>
            </StackPanel>
        </syncfusion:DocumentContainer>
    </Grid>
</Window>
```

**MainWindow.xaml.cs:**
```csharp
using System.Windows;
using Syncfusion.Windows.Tools.Controls;

namespace DocumentContainerDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Optional: Add documents programmatically
            AddDocumentDynamically();
        }
        
        private void AddDocumentDynamically()
        {
            // Create a new document
            Button newDocument = new Button
            {
                Content = "This document was added from code",
                Padding = new Thickness(20)
            };
            
            // Set header
            DocumentContainer.SetHeader(newDocument, "Dynamic Doc");
            
            // Add to container
            documentContainer.Items.Add(newDocument);
        }
    }
}
```

## Next Steps

Now that you have the basic DocumentContainer set up:

- **Learn about TDI vs MDI modes:** Read `container-modes.md` to understand when to use each mode
- **Manage windows:** See `window-management.md` for minimizing, maximizing, and resizing
- **Customize tabs:** Check `tab-management.md` for tab groups, pinning, and organization
- **Add navigation:** Explore `window-switchers.md` for CTRL+TAB switching styles
- **Persist state:** Read `state-persistence.md` to save and restore layouts

## Common Issues and Tips

**Issue:** DocumentContainer appears empty or has no visible size.
- **Solution:** Ensure the container has explicit dimensions or is placed in a layout that provides space (Grid, DockPanel, etc.). Set `VerticalAlignment="Stretch"` and `HorizontalAlignment="Stretch"`.

**Issue:** Headers don't appear on tabs.
- **Solution:** Use the `DocumentContainer.Header` attached property on each child element.

**Issue:** Assembly reference errors.
- **Solution:** Verify both `Syncfusion.Tools.WPF` and `Syncfusion.Shared.WPF` are referenced and their versions match.

**Tip:** Start with TDI mode for modern tabbed interfaces, use MDI mode only if you specifically need floating windows.

**Tip:** Use meaningful header names that clearly identify each document's purpose.

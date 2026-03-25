# Customization and Styling

## Table of Contents
- [Overview](#overview)
- [Setting Document Headers](#setting-document-headers)
- [Custom Header Templates](#custom-header-templates)
- [TDI Toolbar Integration](#tdi-toolbar-integration)
- [Theme and Skin Support](#theme-and-skin-support)
- [Visual Customization](#visual-customization)
- [Container Appearance](#container-appearance)
- [Icon Customization](#icon-customization)

## Overview

The DocumentContainer provides extensive customization options to match your application's branding and user experience requirements. You can customize headers, apply themes, add toolbars, and control the visual appearance of both the container and its documents.

## Setting Document Headers

### Basic Header Setting

Every document should have a header that appears in tabs (TDI) or window title bars (MDI).

**XAML Approach:**
```xaml
<syncfusion:DocumentContainer Name="DocContainer" Mode="TDI">
    <!-- Using Header attached property -->
    <FlowDocumentScrollViewer 
        x:Name="flow" 
        syncfusion:DocumentContainer.Header="Features Document">
        <FlowDocument>
            <Paragraph>Document content here</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
    
    <TextBox 
        syncfusion:DocumentContainer.Header="Text Editor"
        AcceptsReturn="True"
        TextWrapping="Wrap"/>
    
    <Button 
        syncfusion:DocumentContainer.Header="Button Tab"
        Content="Click Me"/>
</syncfusion:DocumentContainer>
```

**C# Approach:**
```csharp
// Create document element
FlowDocumentScrollViewer flowViewer = new FlowDocumentScrollViewer();

// Set header using SetHeader method
DocumentContainer.SetHeader(flowViewer, "Features Document");

// Add to container
documentContainer.Items.Add(flowViewer);

// Get header value
string header = DocumentContainer.GetHeader(flowViewer);
Console.WriteLine($"Document header: {header}");
```

### Dynamic Header Updates

```csharp
// Update header based on content changes
private void UpdateHeader(UIElement document, string newHeader)
{
    DocumentContainer.SetHeader(document, newHeader);
}

// Example: Update header when file is modified
private void Document_TextChanged(object sender, TextChangedEventArgs e)
{
    TextBox editor = sender as TextBox;
    string currentHeader = DocumentContainer.GetHeader(editor);
    
    // Add asterisk to indicate unsaved changes
    if (!currentHeader.EndsWith("*"))
    {
        DocumentContainer.SetHeader(editor, currentHeader + "*");
    }
}

// Remove asterisk after save
private void SaveDocument(TextBox editor)
{
    // Save logic here
    
    string header = DocumentContainer.GetHeader(editor);
    if (header.EndsWith("*"))
    {
        DocumentContainer.SetHeader(editor, header.TrimEnd('*'));
    }
}
```

### Header with Path Information

```csharp
private void SetHeaderWithPath(UIElement document, string filePath)
{
    // Show filename in header, full path in tooltip
    string fileName = Path.GetFileName(filePath);
    DocumentContainer.SetHeader(document, fileName);
    
    // Optional: Set tooltip to show full path
    if (document is FrameworkElement element)
    {
        element.ToolTip = filePath;
    }
}
```

## Custom Header Templates

Create rich header templates with icons, close buttons, and custom styling.

### Header with Icon

```xaml
<syncfusion:DocumentContainer Mode="TDI">
    <FlowDocumentScrollViewer>
        <syncfusion:DocumentContainer.Header>
            <StackPanel Orientation="Horizontal">
                <Image Source="/Images/document-icon.png" 
                       Width="16" 
                       Height="16" 
                       Margin="0,0,5,0"/>
                <TextBlock Text="Features" VerticalAlignment="Center"/>
            </StackPanel>
        </syncfusion:DocumentContainer.Header>
        <FlowDocument>
            <Paragraph>Document content</Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</syncfusion:DocumentContainer>
```

### Header with Custom Controls

```xaml
<syncfusion:DocumentContainer Mode="TDI">
    <TextBox AcceptsReturn="True">
        <syncfusion:DocumentContainer.Header>
            <StackPanel Orientation="Horizontal">
                <!-- Icon -->
                <Image Source="/Images/file-icon.png" 
                       Width="16" Height="16" 
                       Margin="0,0,5,0"/>
                
                <!-- Title -->
                <TextBlock Text="Document 1" 
                           VerticalAlignment="Center"
                           Margin="0,0,10,0"/>
                
                <!-- Modified indicator -->
                <Ellipse Width="8" Height="8" 
                         Fill="Orange"
                         VerticalAlignment="Center"
                         ToolTip="Modified"/>
            </StackPanel>
        </syncfusion:DocumentContainer.Header>
    </TextBox>
</syncfusion:DocumentContainer>
```

### Programmatic Custom Header

```csharp
private void AddDocumentWithCustomHeader(string title, bool isModified, UIElement content)
{
    // Create header stack panel
    StackPanel headerPanel = new StackPanel 
    { 
        Orientation = Orientation.Horizontal 
    };
    
    // Add icon
    Image icon = new Image
    {
        Source = new BitmapImage(new Uri("/Images/document-icon.png", UriKind.Relative)),
        Width = 16,
        Height = 16,
        Margin = new Thickness(0, 0, 5, 0)
    };
    headerPanel.Children.Add(icon);
    
    // Add title
    TextBlock titleText = new TextBlock
    {
        Text = title,
        VerticalAlignment = VerticalAlignment.Center,
        Margin = new Thickness(0, 0, 5, 0)
    };
    headerPanel.Children.Add(titleText);
    
    // Add modified indicator if needed
    if (isModified)
    {
        Ellipse modifiedIndicator = new Ellipse
        {
            Width = 8,
            Height = 8,
            Fill = Brushes.Orange,
            VerticalAlignment = VerticalAlignment.Center,
            ToolTip = "Modified"
        };
        headerPanel.Children.Add(modifiedIndicator);
    }
    
    // Set as header
    DocumentContainer.SetHeader(content, headerPanel);
    
    // Add to container
    documentContainer.Items.Add(content);
}

// Usage
AddDocumentWithCustomHeader("Document 1", true, new TextBox());
```

## TDI Toolbar Integration

Add toolbars to the TDI tab header panel for quick access to commands.

### Adding Toolbar to DocumentContainer

```xaml
<syncfusion:DocumentContainer Name="documentContainer" Mode="TDI">
    <!-- Toolbar in header panel -->
    <syncfusion:DocumentContainer.TDIToolBarTray>
        <ToolBarTray>
            <ToolBar>
                <Button Content="New" Click="New_Click">
                    <Button.ToolTip>Create new document</Button.ToolTip>
                </Button>
                <Button Content="Open" Click="Open_Click">
                    <Button.ToolTip>Open document</Button.ToolTip>
                </Button>
                <Button Content="Save" Click="Save_Click">
                    <Button.ToolTip>Save document</Button.ToolTip>
                </Button>
                <Separator/>
                <Button Content="Close" Click="Close_Click">
                    <Button.ToolTip>Close document</Button.ToolTip>
                </Button>
            </ToolBar>
        </ToolBarTray>
    </syncfusion:DocumentContainer.TDIToolBarTray>
    
    <!-- Documents -->
    <Grid syncfusion:DocumentContainer.Header="Tab 1"/>
    <Grid syncfusion:DocumentContainer.Header="Tab 2"/>
</syncfusion:DocumentContainer>
```

### Programmatic Toolbar Addition

```csharp
private void AddToolbar()
{
    // Create toolbar tray
    ToolBarTray toolBarTray = new ToolBarTray();
    
    // Create toolbar
    ToolBar toolBar = new ToolBar();
    
    // Add buttons
    Button newButton = new Button { Content = "New" };
    newButton.Click += New_Click;
    toolBar.Items.Add(newButton);
    
    Button openButton = new Button { Content = "Open" };
    openButton.Click += Open_Click;
    toolBar.Items.Add(openButton);
    
    Button saveButton = new Button { Content = "Save" };
    saveButton.Click += Save_Click;
    toolBar.Items.Add(saveButton);
    
    toolBar.Items.Add(new Separator());
    
    Button closeButton = new Button { Content = "Close" };
    closeButton.Click += Close_Click;
    toolBar.Items.Add(closeButton);
    
    // Add toolbar to tray
    toolBarTray.ToolBars.Add(toolBar);
    
    // Set toolbar tray on DocumentContainer
    documentContainer.TDIToolBarTray = toolBarTray;
}
```

### Advanced Toolbar with Icons

```xaml
<syncfusion:DocumentContainer.TDIToolBarTray>
    <ToolBarTray>
        <ToolBar>
            <!-- New Document -->
            <Button Click="New_Click" ToolTip="New Document">
                <StackPanel Orientation="Horizontal">
                    <Image Source="/Images/new-icon.png" Width="16" Height="16"/>
                    <TextBlock Text="New" Margin="5,0,0,0"/>
                </StackPanel>
            </Button>
            
            <!-- Open Document -->
            <Button Click="Open_Click" ToolTip="Open Document">
                <StackPanel Orientation="Horizontal">
                    <Image Source="/Images/open-icon.png" Width="16" Height="16"/>
                    <TextBlock Text="Open" Margin="5,0,0,0"/>
                </StackPanel>
            </Button>
            
            <!-- Save Document -->
            <Button Click="Save_Click" ToolTip="Save Document">
                <StackPanel Orientation="Horizontal">
                    <Image Source="/Images/save-icon.png" Width="16" Height="16"/>
                    <TextBlock Text="Save" Margin="5,0,0,0"/>
                </StackPanel>
            </Button>
            
            <Separator/>
            
            <!-- Layout Options -->
            <ComboBox Width="120" SelectionChanged="LayoutMode_Changed">
                <ComboBoxItem Content="TDI" IsSelected="True"/>
                <ComboBoxItem Content="MDI"/>
            </ComboBox>
        </ToolBar>
    </ToolBarTray>
</syncfusion:DocumentContainer.TDIToolBarTray>
```

## Theme and Skin Support

The DocumentContainer supports Syncfusion's theme framework for consistent styling.

### Applying Themes

```csharp
using Syncfusion.SfSkinManager;

// Apply theme to DocumentContainer
SfSkinManager.SetTheme(documentContainer, new Theme("MaterialDark"));

// Available themes:
// - MaterialLight
// - MaterialDark
// - Office2019Colorful
// - Office2019Black
// - Office2019White
// - FluentLight
// - FluentDark
// - Windows11Light
// - Windows11Dark
```

### Theme in XAML

```xaml
<Window xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">
    <Grid>
        <syncfusion:DocumentContainer 
            syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialDark}"
            Mode="TDI">
            <!-- Documents -->
        </syncfusion:DocumentContainer>
    </Grid>
</Window>
```

### Custom Theme Colors

```csharp
// Create custom theme
var customTheme = new Theme("CustomTheme");

// Apply to DocumentContainer
SfSkinManager.SetTheme(documentContainer, customTheme);
```

## Visual Customization

### Container Background

```xaml
<syncfusion:DocumentContainer Mode="TDI" Background="#F5F5F5">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

### Tab Styling (TDI)

```xaml
<syncfusion:DocumentContainer Mode="TDI">
    <syncfusion:DocumentContainer.Resources>
        <!-- Custom tab style -->
        <Style TargetType="TabItem">
            <Setter Property="FontSize" Value="14"/>
            <Setter Property="FontWeight" Value="SemiBold"/>
            <Setter Property="Padding" Value="15,5"/>
        </Style>
    </syncfusion:DocumentContainer.Resources>
    
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

### Window Border Styling (MDI)

```xaml
<syncfusion:DocumentContainer Mode="MDI">
    <syncfusion:DocumentContainer.Resources>
        <!-- Custom window border color -->
        <SolidColorBrush x:Key="MDIWindowBorderBrush" Color="#0078D7"/>
    </syncfusion:DocumentContainer.Resources>
    
    <!-- Windows -->
</syncfusion:DocumentContainer>
```

## Container Appearance

### Setting Container Size

```xaml
<syncfusion:DocumentContainer 
    Mode="TDI"
    Width="800"
    Height="600"
    MinWidth="400"
    MinHeight="300">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

### Container Margins and Padding

```xaml
<Grid>
    <syncfusion:DocumentContainer 
        Mode="TDI"
        Margin="10"
        Padding="5">
        <!-- Documents -->
    </syncfusion:DocumentContainer>
</Grid>
```

### Full Screen Container

```xaml
<syncfusion:DocumentContainer 
    Mode="TDI"
    HorizontalAlignment="Stretch"
    VerticalAlignment="Stretch">
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

## Icon Customization

### Setting Container Icon

```xaml
<syncfusion:DocumentContainer Name="DocContainer" Mode="TDI">
    <syncfusion:DocumentContainer.Icon>
        <ImageBrush ImageSource="/Images/document-icon.png"/>
    </syncfusion:DocumentContainer.Icon>
    
    <!-- Documents -->
</syncfusion:DocumentContainer>
```

### Document-Specific Icons

```xaml
<syncfusion:DocumentContainer Mode="TDI">
    <!-- Document with icon -->
    <TextBox>
        <syncfusion:DocumentContainer.Header>
            <StackPanel Orientation="Horizontal">
                <Image Source="/Images/text-icon.png" Width="16" Height="16"/>
                <TextBlock Text="Text Document" Margin="5,0,0,0"/>
            </StackPanel>
        </syncfusion:DocumentContainer.Header>
    </TextBox>
    
    <!-- Another document with different icon -->
    <Grid>
        <syncfusion:DocumentContainer.Header>
            <StackPanel Orientation="Horizontal">
                <Image Source="/Images/grid-icon.png" Width="16" Height="16"/>
                <TextBlock Text="Data Grid" Margin="5,0,0,0"/>
            </StackPanel>
        </syncfusion:DocumentContainer.Header>
    </Grid>
</syncfusion:DocumentContainer>
```

## Best Practices

1. **Consistent headers:** Use clear, descriptive headers that help users identify documents
2. **Visual indicators:** Show modification status, file type, or other state in headers
3. **Toolbar relevance:** Only add toolbar commands that apply to documents
4. **Theme consistency:** Apply the same theme throughout your application
5. **Icon clarity:** Use standard, recognizable icons for file types
6. **Performance:** Avoid complex header templates for many documents
7. **Accessibility:** Ensure custom headers have appropriate tooltips and ARIA labels

## Common Customization Scenarios

### Scenario 1: File Editor with Modification Indicator

```csharp
public class DocumentInfo
{
    public string FilePath { get; set; }
    public bool IsModified { get; set; }
    public UIElement Content { get; set; }
}

private void AddFileDocument(DocumentInfo info)
{
    string fileName = Path.GetFileName(info.FilePath);
    string displayName = info.IsModified ? $"{fileName}*" : fileName;
    
    // Create header with icon
    StackPanel header = new StackPanel { Orientation = Orientation.Horizontal };
    
    // File type icon
    Image icon = GetFileTypeIcon(info.FilePath);
    header.Children.Add(icon);
    
    // File name
    TextBlock title = new TextBlock 
    { 
        Text = displayName,
        VerticalAlignment = VerticalAlignment.Center,
        Margin = new Thickness(5, 0, 0, 0)
    };
    header.Children.Add(title);
    
    DocumentContainer.SetHeader(info.Content, header);
    info.Content.ToolTip = info.FilePath;
    
    documentContainer.Items.Add(info.Content);
}
```

### Scenario 2: IDE-Style Layout with Toolbar

```xaml
<syncfusion:DocumentContainer Mode="TDI" SwitchMode="QuickTabs">
    <syncfusion:DocumentContainer.TDIToolBarTray>
        <ToolBarTray>
            <ToolBar>
                <Button Content="⊕ New" Click="NewFile_Click"/>
                <Button Content="📁 Open" Click="OpenFile_Click"/>
                <Button Content="💾 Save" Click="SaveFile_Click"/>
                <Button Content="💾 Save All" Click="SaveAll_Click"/>
                <Separator/>
                <ComboBox Width="100" ToolTip="Select Encoding">
                    <ComboBoxItem Content="UTF-8" IsSelected="True"/>
                    <ComboBoxItem Content="ASCII"/>
                    <ComboBoxItem Content="Unicode"/>
                </ComboBox>
            </ToolBar>
        </ToolBarTray>
    </syncfusion:DocumentContainer.TDIToolBarTray>
    
    <!-- Code editor documents -->
</syncfusion:DocumentContainer>
```

### Scenario 3: Themed Document Viewer

```csharp
private void ApplyDarkTheme()
{
    // Apply dark theme
    SfSkinManager.SetTheme(documentContainer, new Theme("MaterialDark"));
    
    // Custom colors for documents
    foreach (var item in documentContainer.Items)
    {
        if (item is TextBox textBox)
        {
            textBox.Background = new SolidColorBrush(Color.FromRgb(30, 30, 30));
            textBox.Foreground = Brushes.White;
        }
    }
}

private void ApplyLightTheme()
{
    // Apply light theme
    SfSkinManager.SetTheme(documentContainer, new Theme("MaterialLight"));
    
    // Custom colors for documents
    foreach (var item in documentContainer.Items)
    {
        if (item is TextBox textBox)
        {
            textBox.Background = Brushes.White;
            textBox.Foreground = Brushes.Black;
        }
    }
}
```

## Troubleshooting

**Issue:** Custom headers don't appear
- **Solution:** Ensure the header is set before adding the item to the container

**Issue:** Toolbar doesn't show
- **Solution:** Verify container is in TDI mode; MDI doesn't support TDIToolBarTray

**Issue:** Theme doesn't apply
- **Solution:** Ensure Syncfusion.SfSkinManager assembly is referenced

**Issue:** Icons in headers are blurry
- **Solution:** Use vector images or high-DPI PNG files (24x24 or 32x32)

## Related Documentation

- **Getting Started:** See `getting-started.md` for basic header setup
- **Tab Management:** See `tab-management.md` for tab-specific customization
- **Window Management:** See `window-management.md` for MDI window customization

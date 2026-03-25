# Getting Started with WPF GridSplitter

This guide covers the fundamentals of adding and configuring the Syncfusion WPF GridSplitter (SfGridSplitter) control in your WPF application.

## Assembly Deployment

The SfGridSplitter control requires specific assemblies to be referenced in your project.

### Required Assemblies

Add references to the following assemblies:
- **Syncfusion.SfInput.WPF** - Contains the SfGridSplitter control
- **Syncfusion.SfShared.WPF** - Shared utilities and base classes

### NuGet Package Installation

Install the required package using the NuGet Package Manager:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.SfInput.WPF
```

**Visual Studio Package Manager UI:**
1. Right-click on your project → Manage NuGet Packages
2. Search for "Syncfusion.SfInput.WPF"
3. Click Install

The NuGet package automatically includes all required dependencies.

### Control Dependencies

For more details about control dependencies and NuGet packages:
- Control dependencies documentation: https://help.syncfusion.com/wpf/control-dependencies#sfgridsplitter
- NuGet package installation guide: https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages

## Adding Control in XAML

### Step 1: Add Assembly References

Ensure the following assemblies are referenced in your project:
- Syncfusion.SfInput.WPF
- Syncfusion.SfShared.WPF

### Step 2: Import Syncfusion WPF Schema

Add the Syncfusion WPF namespace to your XAML file:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Step 3: Declare SfGridSplitter Control

Add the SfGridSplitter control within a Grid:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="SfGridSplitterSample.MainWindow"
        Title="SfGridSplitter Sample" Height="350" Width="525">
    <Grid>
        <!-- Adding SfGridSplitter control -->
        <syncfusion:SfGridSplitter x:Name="sfGridSplitter"
                                   HorizontalAlignment="Stretch"/>
    </Grid>
</Window>
```

## Adding Control in C#

### Step 1: Add Assembly References

Add the same assembly references as for XAML (see above).

### Step 2: Import Namespace

Import the SfGridSplitter namespace in your code file:

```csharp
using Syncfusion.Windows.Controls.Input;
```

### Step 3: Create and Add Control

Create an instance of SfGridSplitter and add it to your layout:

```csharp
using Syncfusion.Windows.Controls.Input;

namespace SfGridSplitterSample 
{    
    public partial class MainWindow : Window 
    {
        public MainWindow() 
        {
            InitializeComponent();

            // Creating an instance of SfGridSplitter control
            SfGridSplitter sfGridSplitter = new SfGridSplitter();
            sfGridSplitter.HorizontalAlignment = HorizontalAlignment.Stretch;

            // Adding SfGridSplitter as window content
            this.Content = sfGridSplitter;
        } 
    }
}
```

## Basic Grid Splitter Setup

### Understanding Grid Structure

SfGridSplitter works within a Grid layout and requires proper row or column definitions. The splitter should be placed in its own row (for horizontal splitting) or column (for vertical splitting).

### Basic Horizontal Splitter

Splits space between rows:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <!-- Top Panel -->
        <TextBlock Grid.Row="0" 
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"
                   Text="Panel 1"/>
        
        <!-- Bottom Panel -->
        <TextBlock Grid.Row="2"
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center" 
                   Text="Panel 2"/>

        <!-- Grid Splitter -->
        <syncfusion:SfGridSplitter Name="gridSplitter"
                                   Grid.Row="1"
                                   HorizontalAlignment="Stretch" 
                                   ResizeBehavior="PreviousAndNext"
                                   Height="5"/>
    </Grid>
</Border>
```

**Key Points:**
- **Row Definition:** Middle row has `Height="Auto"` to size to splitter
- **Splitter Placement:** `Grid.Row="1"` places splitter in middle row
- **Alignment:** `HorizontalAlignment="Stretch"` spans full width
- **ResizeBehavior:** `PreviousAndNext` resizes adjacent panels

### Basic Vertical Splitter

Splits space between columns:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <!-- Left Panel -->
        <TextBlock Grid.Column="0" 
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"
                   Text="Panel 1"/>
        
        <!-- Right Panel -->
        <TextBlock Grid.Column="2"
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center" 
                   Text="Panel 2"/>

        <!-- Grid Splitter -->
        <syncfusion:SfGridSplitter Name="gridSplitter"
                                   Grid.Column="1"
                                   VerticalAlignment="Stretch" 
                                   ResizeBehavior="PreviousAndNext"
                                   Width="5"/>
    </Grid>
</Border>
```

**Key Points:**
- **Column Definition:** Middle column has `Width="Auto"` to size to splitter
- **Splitter Placement:** `Grid.Column="1"` places splitter in middle column
- **Alignment:** `VerticalAlignment="Stretch"` spans full height
- **ResizeBehavior:** `PreviousAndNext` resizes adjacent panels

## Initial Configuration

### Essential Properties

When setting up SfGridSplitter, configure these essential properties:

#### 1. Alignment Properties

**For Horizontal Splitters (resizing rows):**
```xml
<syncfusion:SfGridSplitter HorizontalAlignment="Stretch"
                           Height="5"/>
```

**For Vertical Splitters (resizing columns):**
```xml
<syncfusion:SfGridSplitter VerticalAlignment="Stretch"
                           Width="5"/>
```

#### 2. ResizeBehavior Property

Controls which rows or columns are affected by the splitter:

```xml
<syncfusion:SfGridSplitter ResizeBehavior="PreviousAndNext"/>
```

**Options:**
- **PreviousAndNext:** Resizes elements before and after the splitter (most common)
- **PreviousAndCurrent:** Resizes previous element and current element
- **CurrentAndNext:** Resizes current element and next element

#### 3. Size Properties

**Horizontal Splitter:**
```xml
<syncfusion:SfGridSplitter Height="5"/>
```

**Vertical Splitter:**
```xml
<syncfusion:SfGridSplitter Width="5"/>
```

Typical sizes: 3-10 pixels depending on desired prominence.

### Complete Setup Example

Here's a complete example with proper configuration:

```xml
<Window x:Class="GridSplitterDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="GridSplitter Demo" Height="400" Width="600">
    
    <Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*" MinHeight="50"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*" MinHeight="50"/>
            </Grid.RowDefinitions>
            
            <!-- Top Content Area -->
            <Border Grid.Row="0" Background="LightBlue" Padding="10">
                <StackPanel>
                    <TextBlock Text="Top Panel" 
                               FontSize="16" 
                               FontWeight="Bold"
                               Margin="0,0,0,10"/>
                    <TextBlock Text="This panel can be resized using the splitter below."
                               TextWrapping="Wrap"/>
                </StackPanel>
            </Border>
            
            <!-- Grid Splitter -->
            <syncfusion:SfGridSplitter Grid.Row="1"
                                       HorizontalAlignment="Stretch"
                                       Height="5"
                                       Background="DarkGray"
                                       ResizeBehavior="PreviousAndNext"/>
            
            <!-- Bottom Content Area -->
            <Border Grid.Row="2" Background="LightGreen" Padding="10">
                <StackPanel>
                    <TextBlock Text="Bottom Panel" 
                               FontSize="16" 
                               FontWeight="Bold"
                               Margin="0,0,0,10"/>
                    <TextBlock Text="Drag the splitter above to resize this panel."
                               TextWrapping="Wrap"/>
                </StackPanel>
            </Border>
        </Grid>
    </Border>
</Window>
```

## Common Setup Patterns

### Pattern 1: IDE-Style Layout (Vertical Split)

Tree view on left, content on right:

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="200" MinWidth="100"/>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*" MinWidth="200"/>
    </Grid.ColumnDefinitions>
    
    <TreeView Grid.Column="0"/>
    
    <syncfusion:SfGridSplitter Grid.Column="1"
                               VerticalAlignment="Stretch"
                               Width="5"
                               ResizeBehavior="PreviousAndNext"/>
    
    <ContentControl Grid.Column="2"/>
</Grid>
```

### Pattern 2: Master-Detail Layout (Horizontal Split)

List on top, details on bottom:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" MinHeight="100"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="200" MinHeight="100"/>
    </Grid.RowDefinitions>
    
    <DataGrid Grid.Row="0"/>
    
    <syncfusion:SfGridSplitter Grid.Row="1"
                               HorizontalAlignment="Stretch"
                               Height="5"
                               ResizeBehavior="PreviousAndNext"/>
    
    <ContentControl Grid.Row="2"/>
</Grid>
```

## Setup Checklist

Before running your application, verify:

- [ ] **Assemblies referenced:** Syncfusion.SfInput.WPF and Syncfusion.SfShared.WPF
- [ ] **Namespace imported:** xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
- [ ] **Grid structure:** Parent Grid with row/column definitions
- [ ] **Splitter placement:** In separate row (Height="Auto") or column (Width="Auto")
- [ ] **Alignment set:** HorizontalAlignment or VerticalAlignment = Stretch
- [ ] **Size specified:** Height for horizontal, Width for vertical splitters
- [ ] **ResizeBehavior configured:** Typically PreviousAndNext
- [ ] **MinHeight/MinWidth:** Set on resizable rows/columns to prevent collapse

## Troubleshooting Setup Issues

### Splitter Not Visible

**Problem:** Splitter doesn't appear in the layout.

**Solutions:**
- Ensure Height (horizontal) or Width (vertical) is explicitly set
- Check that splitter is in a separate row/column with Auto sizing
- Verify Background property is set to a visible color
- Confirm Grid.Row or Grid.Column is correctly specified

### Splitter Not Resizing

**Problem:** Dragging the splitter has no effect.

**Solutions:**
- Verify HorizontalAlignment="Stretch" for horizontal splitters
- Verify VerticalAlignment="Stretch" for vertical splitters
- Check that ResizeBehavior is set (default may not work as expected)
- Ensure adjacent rows/columns have star (*) sizing, not fixed sizes

### Panels Collapsing to Zero

**Problem:** Panels resize to zero height/width.

**Solutions:**
- Set MinHeight on RowDefinitions for horizontal splitters
- Set MinWidth on ColumnDefinitions for vertical splitters
- Example: `<RowDefinition Height="*" MinHeight="50"/>`

## Next Steps

Now that you have basic GridSplitter setup, explore:
- **Resize and Layout Operations:** Learn about ResizeBehavior options, increments, and programmatic resizing
- **Collapse/Expand Features:** Add interactive collapse buttons
- **Customization:** Style the splitter with custom templates and colors

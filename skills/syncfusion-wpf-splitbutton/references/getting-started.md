# Getting Started with Split Button

This guide covers the installation, setup, and basic configuration of the Syncfusion WPF Split Button control.

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Adding Control via Designer](#adding-control-via-designer)
- [Adding Control Manually in XAML](#adding-control-manually-in-xaml)
- [Adding Control Manually in C#](#adding-control-manually-in-c)
- [Setting Label](#setting-label)
- [Size Modes](#size-modes)
- [Icon Templates](#icon-templates)
- [Icon Template Selector](#icon-template-selector)
- [Setting Images](#setting-images)
- [Icon Width and Height](#icon-width-and-height)
- [IsDefault Mode](#isdefault-mode)
- [Adding Items to Split Button](#adding-items-to-split-button)
- [Applying Themes](#applying-themes)

## Assembly Deployment

The SplitButtonAdv control requires the following assembly reference:

**Required Assembly:**
- `Syncfusion.Shared.WPF`

**Installation Options:**

1. **NuGet Package:** Install via NuGet Package Manager
   ```
   Install-Package Syncfusion.Shared.WPF
   ```

2. **Manual Reference:** Add assembly reference from Syncfusion installation directory

For complete dependency information, refer to the [SplitButtonAdv control dependencies](https://help.syncfusion.com/wpf/control-dependencies#splitbutton) documentation.

## Adding Control via Designer

You can add the SplitButtonAdv control by dragging it from the toolbox:

1. Open the WPF designer view
2. Locate **SplitButtonAdv** in the toolbox
3. Drag and drop it onto the designer surface
4. The required assemblies (`Syncfusion.Shared.WPF`) are automatically added to the project

**Generated XAML:**

```xaml
<syncfusion:SplitButtonAdv x:Name="splitButtonAdv" Label="Split Button"/>
```

**Note:** The `syncfusion` namespace is auto-generated in the XAML.

## Adding Control Manually in XAML

To add the control manually in XAML:

**Step 1:** Add assembly reference
- Add `Syncfusion.Shared.WPF` to your project references

**Step 2:** Import namespace

Import the Syncfusion WPF schema in your XAML:

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

Or import the control namespace:

```xaml
xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Tools.Controls;assembly=Syncfusion.Shared.WPF"
```

**Step 3:** Declare the control

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <syncfusion:SplitButtonAdv Height="44" 
                                    VerticalAlignment="Center" 
                                    HorizontalAlignment="Center" 
                                    Width="162"/>
    </Grid>
</Window>
```

## Adding Control Manually in C#

To add the control programmatically in C#:

**Step 1:** Add assembly reference
- Add `Syncfusion.Shared.WPF` to your project references

**Step 2:** Import namespace

```csharp
using Syncfusion.Windows.Tools.Controls;
```

**Step 3:** Create and add control instance

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        SplitButtonAdv splitButtonAdv = new SplitButtonAdv();
        splitButtonAdv.Height = 44;
        splitButtonAdv.Width = 162;
        splitButtonAdv.VerticalAlignment = VerticalAlignment.Center;
        splitButtonAdv.HorizontalAlignment = HorizontalAlignment.Center;
        
        Root.Children.Add(splitButtonAdv);
    }
}
```

**XAML for root container:**

```xaml
<Grid x:Name="Root">
</Grid>
```

## Setting Label

The label is the text displayed on the button that explains its action. Set it using the `Label` property:

**XAML:**

```xaml
<syncfusion:SplitButtonAdv Label="Colors" />
```

**C#:**

```csharp
SplitButtonAdv button = new SplitButtonAdv();
button.Label = "Colors";
```

## Size Modes

The SplitButtonAdv control supports three predefined size modes based on your application requirements:

### Available Size Modes

The `SizeMode` enumeration contains:
- **Small** - Icon only, no label
- **Normal** - Small icon with text beside it
- **Large** - Large icon with text below it

### Small Mode

When set to Small, only the icon is displayed without the label.

**XAML:**

```xaml
<syncfusion:SplitButtonAdv SizeMode="Small" Label="Colors"/>
```

**C#:**

```csharp
SplitButtonAdv button = new SplitButtonAdv();
button.Label = "Colors";
button.SizeMode = SizeMode.Small;
```

### Normal Mode

In Normal mode, a small icon is displayed with text beside it.

**XAML:**

```xaml
<syncfusion:SplitButtonAdv SizeMode="Normal" Label="Colors"/>
```

**C#:**

```csharp
SplitButtonAdv button = new SplitButtonAdv();
button.Label = "Colors";
button.SizeMode = SizeMode.Normal;
```

### Large Mode

In Large mode, a large icon is displayed with text at the bottom.

**XAML:**

```xaml
<syncfusion:SplitButtonAdv SizeMode="Large" Label="Colors"/>
```

**C#:**

```csharp
SplitButtonAdv button = new SplitButtonAdv();
button.Label = "Colors";
button.SizeMode = SizeMode.Large;
```

## Icon Templates

The `IconTemplate` property provides support for setting any type of image including path data, font icons, and custom templates. The icon automatically resizes the template content according to its size.

**Example with Path Data:**

```xaml
<Window.Resources>
    <DataTemplate x:Key="customIconTemplate">
        <Grid Width="16" Height="16">
            <Path Data="M11.403995,24.319994C12.395995,24.319994 13.199994,25.127996..."
                  Fill="#FF3A3A38"
                  Stretch="Fill" />
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:SplitButtonAdv Label="Login" 
                            SizeMode="Normal"
                            IconTemplate="{StaticResource customIconTemplate}"/>
```

**Different Templates for Different Size Modes:**

```xaml
<Window.Resources>
    <DataTemplate x:Key="smallIconTemplate">
        <Grid Width="12" Height="16">
            <Path Data="..." Fill="#FF3A3A38" Stretch="Fill" />
        </Grid>
    </DataTemplate>

    <DataTemplate x:Key="normalIconTemplate">
        <Grid Width="16" Height="16">
            <Path Data="..." Fill="#FF3A3A38" Stretch="Fill" />
        </Grid>
    </DataTemplate>

    <DataTemplate x:Key="largeIconTemplate">
        <Grid Width="32" Height="32">
            <Path Data="..." Fill="#FF3A3A38" Stretch="Fill" />
        </Grid>
    </DataTemplate>
</Window.Resources>

<StackPanel>
    <syncfusion:SplitButtonAdv SizeMode="Small" 
                               Label="Login"
                               IconTemplate="{StaticResource smallIconTemplate}"/>
    <syncfusion:SplitButtonAdv SizeMode="Normal" 
                               Label="Login"
                               IconTemplate="{StaticResource normalIconTemplate}"/>
    <syncfusion:SplitButtonAdv SizeMode="Large" 
                               Label="Login"
                               IconTemplate="{StaticResource largeIconTemplate}"/>
</StackPanel>
```

**Icon Loading Priority:**

The SplitButtonAdv loads icons in the following priority order:
1. `IconTemplate` (highest priority)
2. `LargeIcon`
3. `SmallIcon` (lowest priority)

## Icon Template Selector

The `IconTemplateSelector` property allows you to specify different data templates based on dynamic conditions.

**DataTemplateSelector Implementation:**

```csharp
public class TemplateSelector : DataTemplateSelector
{
    public DataTemplate NewIcon { get; set; }
    public DataTemplate OpenIcon { get; set; }
    
    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        if (item == null)
        {
            return OpenIcon;
        }
        
        if ((item as Model).IsChecked)
        {
            return NewIcon;
        }
        
        return base.SelectTemplate(item, container);
    }
}
```

**XAML Usage:**

```xaml
<Window.Resources>
    <DataTemplate x:Key="newIcon">
        <Grid Width="12" Height="16">
            <Path Data="M0,0 L5.9999999,0 11,5 11,15 0,15 z"
                  Fill="White" Stretch="Fill" />
        </Grid>
    </DataTemplate>
    
    <DataTemplate x:Key="openIcon">
        <Grid Width="16" Height="16">
            <Path Data="M0,0 L5,0 6,1 12,1 12,3.4999998..."
                  Fill="White" Stretch="Fill" />
        </Grid>
    </DataTemplate>
    
    <local:TemplateSelector x:Key="IconTemp" 
                            NewIcon="{StaticResource newIcon}" 
                            OpenIcon="{StaticResource openIcon}"/>
</Window.Resources>

<syncfusion:SplitButtonAdv Label="IconTemplateSelector" 
                            IconTemplateSelector="{StaticResource IconTemp}"/>
```

**Icon Loading Priority with Template Selector:**

1. `IconTemplateSelector` (highest priority)
2. `IconTemplate`
3. `LargeIcon`
4. `SmallIcon` (lowest priority)

## Setting Images

You can add images to the button using `SmallIcon` or `LargeIcon` properties:

- **SmallIcon** - Used for Normal and Small size modes
- **LargeIcon** - Used for Large size mode

**Small Icon (Small Mode):**

```xaml
<syncfusion:SplitButtonAdv SizeMode="Small" Label="Syncfusion" />
```

```csharp
SplitButtonAdv button = new SplitButtonAdv();
button.Label = "Syncfusion";
button.SizeMode = SizeMode.Small;
```

**Small Icon (Normal Mode):**

```xaml
<syncfusion:SplitButtonAdv SizeMode="Normal" Label="Syncfusion"/>
```

```csharp
SplitButtonAdv button = new SplitButtonAdv();
button.Label = "Syncfusion";
button.SizeMode = SizeMode.Normal;
```

**Large Icon:**

```xaml
<syncfusion:SplitButtonAdv SizeMode="Large" Label="Syncfusion"/>
```

```csharp
SplitButtonAdv button = new SplitButtonAdv();
button.Label = "Syncfusion";
button.SizeMode = SizeMode.Large;
```

## Icon Width and Height

You can customize the icon dimensions using `IconWidth` and `IconHeight` properties.

**Example 1 - 20x20 Icon:**

```xaml
<syncfusion:SplitButtonAdv SizeMode="Normal" 
                            IconHeight="20" 
                            IconWidth="20"  
                            Label="Syncfusion"/>
```

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
splitButton.Label = "Syncfusion";
splitButton.IconWidth = 20;
splitButton.IconHeight = 20;
```

**Example 2 - 30x30 Icon:**

```xaml
<syncfusion:SplitButtonAdv SizeMode="Normal" 
                            IconHeight="30" 
                            IconWidth="30"  
                            Label="Syncfusion"/>
```

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
splitButton.Label = "Syncfusion";
splitButton.IconWidth = 30;
splitButton.IconHeight = 30;
```

## IsDefault Mode

The `IsDefault` property indicates whether the SplitButtonAdv is a default button. When set to `true`, the user can activate the button by pressing the `Enter` key.

**XAML:**

```xaml
<syncfusion:SplitButtonAdv x:Name="defaultButton" 
                            Label="Default" 
                            Click="SplitButtonAdv_Click" 
                            IsDefault="True"/>
```

**Use Case:** This is useful for forms where you want a primary action button that can be activated with the Enter key without needing to click it.

## Adding Items to Split Button

The `DropDownMenuGroup` acts as a container for dropdown menu items. It provides options to add menu items along with features like header names, resizing, and scrollbars.

**Basic Menu Items:**

```xaml
<syncfusion:SplitButtonAdv Label="Country" SizeMode="Normal">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="India"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="France"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Germany"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

**C# Code:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
DropDownMenuGroup menu = new DropDownMenuGroup();

DropDownMenuItem item1 = new DropDownMenuItem() 
{ 
    Header = "India", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item2 = new DropDownMenuItem() 
{ 
    Header = "France", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item3 = new DropDownMenuItem() 
{ 
    Header = "Germany", 
    HorizontalAlignment = HorizontalAlignment.Left
};

menu.Items.Add(item1);
menu.Items.Add(item2);
menu.Items.Add(item3);

splitButton.Content = menu;
splitButton.Label = "Country";
splitButton.SizeMode = SizeMode.Normal;
```

**Note:** For data binding and command actions, refer to the [data-binding.md](data-binding.md) and [command-binding.md](command-binding.md) reference files.

## Applying Themes

SplitButtonAdv supports various built-in themes:

**Option 1: Apply Theme Using SfSkinManager**

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
    <syncfusion:SplitButtonAdv Label="Themed Button"/>
</Window>
```

**Option 2: Create Custom Theme Using ThemeStudio**

Visit the [ThemeStudio documentation](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme) to create custom themes for your application.

For more details on themes, refer to [customization.md](customization.md).

## GitHub Samples

View complete samples on GitHub:
- [Getting Started Sample](https://github.com/SyncfusionExamples/wpf-split-button-examples/blob/master/Samples/Getting-Started)

This sample showcases how to add the split button control with its basic features like image sizing options and size modes.

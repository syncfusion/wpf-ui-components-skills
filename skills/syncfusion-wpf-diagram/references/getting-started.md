# Getting Started with WPF Diagram (SfDiagram)

## Overview

SfDiagram is found in the `Syncfusion.UI.Xaml.Diagram` namespace and added via the `Syncfusion.SfDiagram.WPF` NuGet package. This guide covers project setup through a working flowchart.

## Assembly Deployment

### NuGet Installation (Recommended)
```powershell
Install-Package Syncfusion.SfDiagram.WPF
```

Or install via the Visual Studio NuGet Package Manager UI. See [NuGet package guide](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages) for details.

### License Key Registration
Starting with v16.2.0.x, a license key is required:
```csharp
// In App.xaml.cs, before InitializeComponent()
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```
See [licensing overview](https://help.syncfusion.com/common/essential-studio/licensing/overview).

## Adding SfDiagram to Your Project

### Via XAML

1. Add the assembly reference: `Syncfusion.SfDiagram.WPF`
2. Add the namespace and declare the control:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="MyApp.MainWindow">
    <Grid>
        <syncfusion:SfDiagram x:Name="diagram">
            <syncfusion:SfDiagram.Nodes>
                <syncfusion:NodeCollection/>
            </syncfusion:SfDiagram.Nodes>
            <syncfusion:SfDiagram.Connectors>
                <syncfusion:ConnectorCollection/>
            </syncfusion:SfDiagram.Connectors>
        </syncfusion:SfDiagram>
    </Grid>
</Window>
```

### Via Code-Behind (C#)

```csharp
using Syncfusion.UI.Xaml.Diagram;

SfDiagram diagram = new SfDiagram();
diagram.Nodes = new NodeCollection();
diagram.Connectors = new ConnectorCollection();
RootGrid.Children.Add(diagram);
```

## Merging Built-In Shape Resources

SfDiagram ships with a built-in shapes resource dictionary. Merge it before using shape keys like `"Ellipse"`, `"Rectangle"`, `"PredefinedProcess"`:

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/BasicShapes.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

## Basic Diagram Elements

| Element | Class | Purpose |
|---------|-------|---------|
| Node | `NodeViewModel` | Visual shapes (rectangles, ellipses, custom) |
| Connector | `ConnectorViewModel` | Lines linking nodes or points |
| Annotation | `AnnotationEditorViewModel` | Text labels on nodes/connectors |
| Port | `NodePortViewModel` | Fixed connection points on nodes |

## Complete Minimal Flowchart Example

```xaml
<ResourceDictionary.MergedDictionaries>
    <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/BasicShapes.xaml"/>
</ResourceDictionary.MergedDictionaries>

<!--Shape style-->
<Style x:Key="ShapeStyle" TargetType="Path">
    <Setter Property="Fill" Value="#FF5B9BD5"/>
    <Setter Property="Stretch" Value="Fill"/>
    <Setter Property="Stroke" Value="#FFEDF1F6"/>
</Style>

<!--Connector style-->
<Style TargetType="syncfusion:Connector">
    <Setter Property="TargetDecoratorStyle">
        <Setter.Value>
            <Style TargetType="Path">
                <Setter Property="Fill" Value="Black"/>
                <Setter Property="Stretch" Value="Fill"/>
                <Setter Property="Height" Value="10"/>
                <Setter Property="Width" Value="10"/>
            </Style>
        </Setter.Value>
    </Setter>
</Style>

<syncfusion:SfDiagram x:Name="diagram">
    <syncfusion:SfDiagram.Nodes>
        <syncfusion:NodeCollection>
            <syncfusion:NodeViewModel ID="Start" OffsetX="300" OffsetY="80"
                                      UnitWidth="120" UnitHeight="40"
                                      Shape="{StaticResource Ellipse}"
                                      ShapeStyle="{StaticResource ShapeStyle}">
                <syncfusion:NodeViewModel.Annotations>
                    <syncfusion:AnnotationCollection>
                        <syncfusion:AnnotationEditorViewModel Content="Start"/>
                    </syncfusion:AnnotationCollection>
                </syncfusion:NodeViewModel.Annotations>
            </syncfusion:NodeViewModel>
            <syncfusion:NodeViewModel ID="Process" OffsetX="300" OffsetY="180"
                                      UnitWidth="120" UnitHeight="60"
                                      Shape="{StaticResource PredefinedProcess}"
                                      ShapeStyle="{StaticResource ShapeStyle}">
                <syncfusion:NodeViewModel.Annotations>
                    <syncfusion:AnnotationCollection>
                        <syncfusion:AnnotationEditorViewModel Content="Process"/>
                    </syncfusion:AnnotationCollection>
                </syncfusion:NodeViewModel.Annotations>
            </syncfusion:NodeViewModel>
            <syncfusion:NodeViewModel ID="End" OffsetX="300" OffsetY="290"
                                      UnitWidth="40" UnitHeight="40"
                                      Shape="{StaticResource Ellipse}"
                                      ShapeStyle="{StaticResource ShapeStyle}">
                <syncfusion:NodeViewModel.Annotations>
                    <syncfusion:AnnotationCollection>
                        <syncfusion:AnnotationEditorViewModel Content="End"/>
                    </syncfusion:AnnotationCollection>
                </syncfusion:NodeViewModel.Annotations>
            </syncfusion:NodeViewModel>
        </syncfusion:NodeCollection>
    </syncfusion:SfDiagram.Nodes>
    <syncfusion:SfDiagram.Connectors>
        <syncfusion:ConnectorCollection>
            <syncfusion:ConnectorViewModel SourceNodeID="Start" TargetNodeID="Process"/>
            <syncfusion:ConnectorViewModel SourceNodeID="Process" TargetNodeID="End"/>
        </syncfusion:ConnectorCollection>
    </syncfusion:SfDiagram.Connectors>
</syncfusion:SfDiagram>
```

## Theming

Apply Syncfusion themes using the `SfSkinManager`:
```xaml
<syncfusion:SfDiagram syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=FluentLight}"/>
```

## Common Gotchas

- **Nodes not visible:** Ensure you've merged `BasicShapes.xaml` before referencing shape keys.
- **License warning on startup:** Register the license key before `InitializeComponent()` in `App.xaml.cs`.
- **NodeCollection null:** Always initialize `diagram.Nodes = new NodeCollection()` before adding nodes in code.

## Related

- Adding nodes with shapes → `references/nodes.md`
- Adding connectors → `references/connectors.md`
- Applying automatic layouts → `references/automatic-layouts.md`

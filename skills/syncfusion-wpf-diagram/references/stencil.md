# Stencil (Symbol Palette) in WPF Diagram (SfDiagram)

## Overview

`Stencil` is a gallery of reusable diagram elements that users can drag and drop onto the diagram surface. It lives in the `Syncfusion.UI.Xaml.Diagram.Stencil` namespace.

## Namespace Declaration

```xaml
xmlns:stencil="clr-namespace:Syncfusion.UI.Xaml.Diagram.Stencil;assembly=Syncfusion.SfDiagram.WPF"
```

## Basic Stencil Setup

```xaml
<stencil:Stencil x:Name="stencil"
                 Width="200"
                 ExpandMode="All"
                 BorderBrush="#dfdfdf"
                 BorderThickness="0,0,1,0"
                 GroupMappingName="Key">
    <stencil:Stencil.SymbolSource>
        <syncfusion:SymbolCollection>
            <syncfusion:NodeViewModel Key="Basic Shapes" Name="Rectangle"
                                      UnitWidth="100" UnitHeight="70"
                                      Shape="{StaticResource Rectangle}"/>
            <syncfusion:NodeViewModel Key="Basic Shapes" Name="Ellipse"
                                      UnitWidth="100" UnitHeight="70"
                                      Shape="{StaticResource Ellipse}"/>
            <syncfusion:ConnectorViewModel Key="Connectors"
                                           SourcePoint="0,0" TargetPoint="100,0"/>
        </syncfusion:SymbolCollection>
    </stencil:Stencil.SymbolSource>
    <stencil:Stencil.SymbolGroups>
        <syncfusion:SymbolGroups>
            <syncfusion:SymbolGroupProvider MappingName="Key"/>
        </syncfusion:SymbolGroups>
    </stencil:Stencil.SymbolGroups>
</stencil:Stencil>
```

```csharp
Stencil stencil = new Stencil()
{
    ExpandMode       = ExpandMode.All,
    BorderBrush      = new SolidColorBrush(Colors.LightGray),
    BorderThickness  = new Thickness(0, 0, 1, 0),
    GroupMappingName = "Key",
};

stencil.SymbolSource = new SymbolCollection();

// Add nodes as symbols
(stencil.SymbolSource as SymbolCollection).Add(new NodeViewModel()
{
    Key        = "Basic Shapes",
    Name       = "Rectangle",
    UnitWidth  = 100,
    UnitHeight = 70,
    Shape      = App.Current.Resources["Rectangle"],
});

// Add connectors as symbols
(stencil.SymbolSource as SymbolCollection).Add(new ConnectorViewModel()
{
    Key          = "Connectors",
    SourcePoint  = new Point(0, 0),
    TargetPoint  = new Point(100, 0),
});
```

## Symbol Sources

### 1. Diagram Elements as Symbols

Any `NodeViewModel`, `ConnectorViewModel`, `GroupViewModel`, or `ContainerViewModel` can be a symbol. Add a `Key` property to control grouping.

```csharp
// Group node
GroupViewModel group = new GroupViewModel()
{
    Key = "Groups",
    Nodes = new NodeCollection()
    {
        new NodeViewModel() { ID="n1", OffsetX=0,   OffsetY=0,   UnitWidth=80, UnitHeight=60, Shape=App.Current.Resources["Rectangle"] },
        new NodeViewModel() { ID="n2", OffsetX=100, OffsetY=100, UnitWidth=80, UnitHeight=60, Shape=App.Current.Resources["Rectangle"] },
    },
    Connectors = new ConnectorCollection()
    {
        new ConnectorViewModel() { SourceNodeID="n1", TargetNodeID="n2" },
    }
};
(stencil.SymbolSource as SymbolCollection).Add(group);
```

### 2. SymbolViewModel (Custom Templates)

Use `SymbolViewModel` with a `SymbolTemplate` (DataTemplate) to show any custom content in the palette:

```xaml
<DataTemplate x:Key="CustomSymbol">
    <StackPanel>
        <Image Source="/Images/custom.png" Width="80" Height="60"/>
        <TextBlock Text="Custom" HorizontalAlignment="Center"/>
    </StackPanel>
</DataTemplate>
```

```csharp
SymbolViewModel customSymbol = new SymbolViewModel()
{
    Symbol         = "Custom",
    Key            = "Custom Shapes",
    SymbolTemplate = App.Current.Resources["CustomSymbol"] as DataTemplate,
    SearchTags     = new List<string> { "custom", "special" },
};
(stencil.SymbolSource as SymbolCollection).Add(customSymbol);
```

### 3. Built-in Shape Category Symbols

Use `SymbolCollection` with built-in shapes from the standard ResourceDictionary:

```xaml
<ResourceDictionary.MergedDictionaries>
    <ResourceDictionary Source="/Syncfusion.SfDiagram.Wpf;component/Resources/BasicShapes.xaml"/>
</ResourceDictionary.MergedDictionaries>

<stencil:Stencil.SymbolSource>
    <syncfusion:SymbolCollection>
        <syncfusion:NodeViewModel Key="Basic" Name="Rectangle" Shape="{StaticResource Rectangle}"
                                  UnitWidth="100" UnitHeight="70"/>
        <syncfusion:NodeViewModel Key="Basic" Name="Triangle"  Shape="{StaticResource Triangle}"
                                  UnitWidth="100" UnitHeight="70"/>
        <syncfusion:NodeViewModel Key="Basic" Name="Diamond"   Shape="{StaticResource Diamond}"
                                  UnitWidth="100" UnitHeight="70"/>
    </syncfusion:SymbolCollection>
</stencil:Stencil.SymbolSource>
```

## Symbol Grouping

Set `GroupMappingName` on the stencil to the property name that defines the group. All symbols sharing the same `Key` value are grouped together.

```csharp
stencil.GroupMappingName = "Key"; // groups by the Key property
```

Use `SymbolGroups` with `SymbolGroupProvider`:

```xaml
<stencil:Stencil.SymbolGroups>
    <syncfusion:SymbolGroups>
        <syncfusion:SymbolGroupProvider MappingName="Key"/>
    </syncfusion:SymbolGroups>
</stencil:Stencil.SymbolGroups>
```

### ExpandMode

| Value | Behavior |
|-------|----------|
| `All` | All groups expanded |
| `ZeroOrOne` | At most one group open at a time |
| `ZeroOrMore` | Any number of groups can be open |
| `One` | Always exactly one group open |
| `OneOrMore` | At least one group open |

## Symbol Search

Enable the built-in search box:

```xaml
<stencil:Stencil ShowSearchTextBox="True" ...>
```

Symbols are matched by `Name`. Add aliases via `SearchTags`:

```csharp
new SymbolViewModel()
{
    Name       = "Decision",
    SearchTags = new List<string> { "gateway", "branch", "diamond" },
    ...
}
```

## Symbol Filtering

`SymbolFilters` allow showing/hiding symbol groups based on user selection:

```csharp
stencil.SymbolFilters = new SymbolFilterCollection()
{
    new SymbolFilterProvider()
    {
        Content         = "All",
        SymbolFilter    = FilterAll,
    },
    new SymbolFilterProvider()
    {
        Content         = "Basic Shapes",
        SymbolFilter    = FilterBasic,
    },
};

// Filter delegate — return true to show the symbol
private bool FilterAll(SymbolFilterProvider filter, object symbol)
{
    return true;
}

private bool FilterBasic(SymbolFilterProvider filter, object symbol)
{
    if (symbol is NodeViewModel node)
        return node.Key?.ToString() == "Basic Shapes";
    return false;
}
```

## Drag and Drop from Stencil

When a symbol is dragged from the stencil onto the diagram, SfDiagram creates a clone of the symbol. Listen to the `ItemAddedEvent` to customize the dropped element:

```csharp
diagram.ItemAddedEvent += (s, e) =>
{
    if (e.Item is NodeViewModel node && e.ItemAdded == ItemAdded.Stencil)
    {
        // Customize after drop
        node.UnitWidth  = 120;
        node.UnitHeight = 80;
    }
};
```

Restrict drops with `ItemDropEventArgs`:

```csharp
diagram.ItemDropEvent += (s, e) =>
{
    // Cancel drop if condition not met
    if (/* some condition */)
        e.Cancel = true;
};
```

## Stencil Constraints

```csharp
// Example: disable drag from stencil
stencil.Constraints = StencilConstraints.Default & ~StencilConstraints.AllowDragDrop;
```

## Common Stencil + Diagram Layout

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="200"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>

    <stencil:Stencil Grid.Column="0" x:Name="stencil"
                     GroupMappingName="Key" ExpandMode="All">
        <!--...symbols...-->
    </stencil:Stencil>

    <syncfusion:SfDiagram Grid.Column="1" x:Name="diagram"/>
</Grid>
```

## Common Gotchas

- **Wrong namespace:** Use `stencil:Stencil`, not `syncfusion:Stencil`.
- **Symbols not grouped:** Make sure `GroupMappingName` matches the property name (e.g., `"Key"`) and all symbols have that property set.
- **Shape not found:** Merge the shape ResourceDictionary (e.g., `BasicShapes.xaml`) before referencing shape keys.
- **Search not working:** Set `Name` property on each `NodeViewModel` in the SymbolSource — search matches on `Name`.

## Related
- Built-in shapes → `references/nodes.md` (shape resource dictionaries)
- BPMN palette → `references/bpmn-shapes.md`
- Swimlane palette → `references/swimlane.md`

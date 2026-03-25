---
title: Getting Started with SfHeatMap (WPF)
---

# Getting Started

This reference provides a step-by-step quick start to initialize and populate `SfHeatMap`.

## Table of Contents
- Initialize
- Prepare data
- Populate data
- Map data (TableMapping example)
- Color mapping and legend
- Full example

## Initialize

Add `SfHeatMap` to a XAML page inside `Syncfusion.UI.Xaml.HeatMap` namespace.

Example XAML:

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
  <Grid>
    <syncfusion:SfHeatMap />
  </Grid>
</Window>
```

## Prepare data

Create a POCO and a collection. Example classes shown below.

```csharp
public class Product { public string ProductName { get; set; } public double Y2007 { get; set; } /* ... */ }
public class Products : ObservableCollection<Product> { }
```

## Populate data

Provide a sample collection as a resource with several `Product` items (see main example for full list).

## Map data (TableMapping)

Use `TableMapping` to map header and columns when each object holds multiple numeric properties.

```xaml
<syncfusion:TableMapping x:Key="itemsMapping">
  <syncfusion:TableMapping.HeaderMapping>
    <syncfusion:ColumnMapping PropertyName="ProductName" DisplayName="Product"/>
  </syncfusion:TableMapping.HeaderMapping>
  <syncfusion:TableMapping.ColumnMapping>
    <syncfusion:ColumnMapping PropertyName="Y2007" DisplayName="2007"/>
    <!-- more columns -->
  </syncfusion:TableMapping.ColumnMapping>
</syncfusion:TableMapping>

<syncfusion:SfHeatMap ItemsSource="{StaticResource productsData}" ItemsMapping="{StaticResource itemsMapping}" />
```

## Color mapping and legend

Define `ColorMappingCollection` and pass it to `SfHeatMap` and `SfHeatMapLegend`.

```xaml
<syncfusion:ColorMappingCollection x:Key="colorMapping">
  <syncfusion:ColorMapping Value="0" Color="#8ec8f8" />
  <syncfusion:ColorMapping Value="30" Color="#0d47a1" />
</syncfusion:ColorMappingCollection>

<syncfusion:SfHeatMap ColorMappingCollection="{StaticResource colorMapping}" />
<syncfusion:SfHeatMapLegend ColorMappingCollection="{StaticResource colorMapping}" />
```

## Full example

See the main `docs/Getting-Started.md` for a complete XAML and C# sample showing the `Products` collection, `TableMapping`, and `ColorMappingCollection`.

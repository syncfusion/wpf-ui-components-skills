---
layout: post
title: Virtualization in WPF PropertyGrid control | Syncfusion
description: Learn about Virtualization support in Syncfusion Essential Studio WPF PropertyGrid control, its elements and more.
platform: wpf
control: PropertyGrid
documentation: ug
---

# Virtualization in WPF PropertyGrid

By loading only items that are within viewport, UI virtualization allows `PropertyGrid` to load faster. Virtualization is enabled by default.

```csharp
PropertyGrid propertyGrid = new PropertyGrid();
propertyGrid.IsVirtualizing = true;
propertyGrid.EnableGrouping = true;
propertyGrid.PropertyExpandMode = PropertyExpandModes.NestedMode;
propertyGrid.SelectedObject = new Button();
```

```xaml
<syncfusion:PropertyGrid x:Name="propertyGrid" IsVirtualizing="True" PropertyExpandMode="NestedMode" EnableGrouping="True">
    <syncfusion:PropertyGrid.SelectedObject>
        <Button />
    </syncfusion:PropertyGrid.SelectedObject>
</syncfusion:PropertyGrid>
```

![PropertyGrid in the Virtualization mode](Virtualization-images/Virtualization.png)

N> When Virtualization is enabled, only properties that are in viewport will be in loaded state.  

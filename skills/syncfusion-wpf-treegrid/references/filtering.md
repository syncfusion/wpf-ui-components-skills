# Filtering for WPF TreeGrid

## Table of Contents

- [Overview](#overview)
- [Filter Levels](#filter-levels)
- [View Filtering](#view-filtering)
- [Column Filtering](#column-filtering)
- [UI Filtering](#ui-filtering)
- [Filter Modes](#filter-modes)
- [Programmatic Filtering](#programmatic-filtering)
- [Filter Events](#filter-events)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

WPF TreeGrid supports filtering at different levels: Root, All nodes, or Extended to child nodes.

## Filter Levels

```csharp
// Filter only root nodes
treeGrid.FilterLevel = FilterLevel.Root;

// Filter all nodes
treeGrid.FilterLevel = FilterLevel.All;

// Filter root and show matching children
treeGrid.FilterLevel = FilterLevel.Extended;
```

## View Filtering

```csharp
// Apply filter using View.Filter
treeGrid.View.Filter = item =>
{
    var employee = item as Employee;
    return employee != null && employee.Salary > 50000;
};

treeGrid.View.RefreshFilter();
```

## Column Filtering

```csharp
// Add filter predicates
treeGrid.FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.Equals,
    FilterValue = "Manager",
    MappingName = "Title",
    PredicateType = PredicateType.And
});

// Complex filtering
treeGrid.FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.GreaterThan,
    FilterValue = 50000,
    MappingName = "Salary",
    PredicateType = PredicateType.And
});
```

## UI Filtering

```xaml
<!-- Enable UI filtering -->
<syncfusion:SfTreeGrid AllowFiltering="True"
                       FilterMode="Both"/>

<!-- Column-level control -->
<syncfusion:TreeGridTextColumn MappingName="FirstName" 
                               AllowFiltering="True"/>
```

### Filter Modes

```csharp
// CheckBox filter only
treeGrid.FilterMode = FilterMode.CheckBox;

// Advanced filter only
treeGrid.FilterMode = FilterMode.AdvancedFilter;

// Both modes
treeGrid.FilterMode = FilterMode.Both;
```

## Checkbox Filter

```csharp
// Customize checkbox filter items
treeGrid.FilterItemsPopulating += (s, e) =>
{
    if (e.Column.MappingName == "Department")
    {
        // Add "Select All" option
        var selectAll = e.FilteredItems.FirstOrDefault(item => item.ActualValue == null);
        if (selectAll != null)
        {
            selectAll.DisplayText = "All Departments";
        }
    }
};
```

## Advanced Filter

```csharp
// Set default filter type
var column = treeGrid.Columns["Salary"] as TreeGridNumericColumn;
column.FilterType = FilterType.GreaterThan;
```

## Programmatic Filtering

```csharp
// Add multiple filters
public void ApplyCustomFilter()
{
    treeGrid.FilterPredicates.Clear();
    
    // Filter by department
    treeGrid.FilterPredicates.Add(new FilterPredicate()
    {
        FilterType = FilterType.Equals,
        FilterValue = "Sales",
        MappingName = "Department"
    });
    
    // AND filter by salary
    treeGrid.FilterPredicates.Add(new FilterPredicate()
    {
        FilterType = FilterType.GreaterThanOrEqual,
        FilterValue = 60000,
        MappingName = "Salary",
        PredicateType = PredicateType.And
    });
    
    treeGrid.View.RefreshFilter();
}

// Clear filters
public void ClearFilters()
{
    treeGrid.FilterPredicates.Clear();
    treeGrid.View.RefreshFilter();
}
```

## Filter Events

```csharp
// Before filtering
treeGrid.FilterItemsPopulating += (s, e) =>
{
    // Customize filter items
    Debug.WriteLine($"Populating filter for: {e.Column.MappingName}");
};

// After filtering
treeGrid.FilterItemsPopulated += (s, e) =>
{
    Debug.WriteLine($"Filter populated for: {e.Column.MappingName}");
    Debug.WriteLine($"Total items: {e.FilteredItems.Count}");
};
```

## Instant Filtering

```csharp
// Enable instant filtering as user types
treeGrid.AllowFiltering = true;
treeGrid.FilterMode = FilterMode.AdvancedFilter;

// The grid automatically filters as the user types in the filter box
```

## Custom Filter Logic

```csharp
// Implement custom filter using View.Filter
treeGrid.View.Filter = item =>
{
    var employee = item as Employee;
    if (employee == null) return false;
    
    // Complex business logic
    if (employee.Department == "IT" && employee.YearsOfExperience > 5)
        return true;
        
    if (employee.Salary > 100000)
        return true;
        
    return false;
};

treeGrid.View.RefreshFilter();
```

## Filter with Multiple Conditions

```csharp
public void ApplyComplexFilter()
{
    // Create filter group
    var filterGroup = new FilterPredicate()
    {
        FilterPredicates = new List<FilterPredicate>()
        {
            new FilterPredicate()
            {
                FilterType = FilterType.Equals,
                FilterValue = "Developer",
                MappingName = "Title"
            },
            new FilterPredicate()
            {
                FilterType = FilterType.Equals,
                FilterValue = "Manager",
                MappingName = "Title",
                PredicateType = PredicateType.Or
            }
        }
    };
    
    treeGrid.FilterPredicates.Add(filterGroup);
    treeGrid.View.RefreshFilter();
}
```

## Best Practices

### 1. Choose Appropriate Filter Level

```csharp
// For hierarchical filtering, use Extended
treeGrid.FilterLevel = FilterLevel.Extended;

// For simple root-level filtering, use Root for better performance
treeGrid.FilterLevel = FilterLevel.Root;
```

### 2. Optimize Filter Performance

```csharp
// Avoid complex logic in View.Filter for large datasets
// Use FilterPredicates for better performance
treeGrid.FilterPredicates.Add(new FilterPredicate()
{
    FilterType = FilterType.Contains,
    FilterValue = searchText,
    MappingName = "Name"
});
```

### 3. Provide User Feedback

```csharp
treeGrid.FilterItemsPopulated += (s, e) =>
{
    var filteredCount = treeGrid.View.Records.Count;
    StatusLabel.Text = $"Showing {filteredCount} of {totalCount} records";
};
```

## Troubleshooting

### Issue: Filter Not Working

```csharp
// Verify AllowFiltering is enabled
treeGrid.AllowFiltering = true;

// Check column-level setting
column.AllowFiltering = true;

// Ensure View.RefreshFilter() is called
treeGrid.View.RefreshFilter();
```

### Issue: Child Nodes Not Filtered

```csharp
// Use appropriate FilterLevel
treeGrid.FilterLevel = FilterLevel.Extended; // or FilterLevel.All
```

### Issue: Filter UI Not Showing

```csharp
// Check FilterMode
treeGrid.FilterMode = FilterMode.Both;

// Verify column has AllowFiltering enabled
var column = treeGrid.Columns["FirstName"];
column.AllowFiltering = true;
```

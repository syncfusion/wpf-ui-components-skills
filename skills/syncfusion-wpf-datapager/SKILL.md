---
name: syncfusion-wpf-datapager
description: Implements Syncfusion WPF DataPager (SfDataPager) for paginating large datasets in WPF applications. Use this when implementing pagination controls, page navigation, or splitting large data into manageable chunks. Supports configurable page sizes, navigation buttons, numeric page buttons, and works with DataGrid, ListBox, ListView, and ItemsControl.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing DataPager

Guide for implementing the Syncfusion WPF DataPager (SfDataPager) to paginate large datasets in WPF applications. The DataPager control provides navigation buttons, numeric page buttons, and flexible data‑binding options for dividing data into manageable pages.

## When to Use This Skill

Use this skill when the user needs to:
- Paginate large datasets in WPF applications
- Add page navigation controls (first, last, next, previous buttons)
- Display data in manageable page sizes
- Implement on-demand paging for large datasets
- Bind paginated data to any ItemsControl (DataGrid, ListBox, ListView)
- Customize pagination appearance (colors, display modes, orientation)
- Handle page change events and validation
- Create paginated views with numeric page buttons
- Change page size at runtime
- Navigate programmatically between pages

## Component Overview

**SfDataPager** is a WPF control that divides data collections into pages and provides navigation UI:
- **Navigation buttons**: First, Last, Previous, Next page buttons
- **Numeric buttons**: Direct page access (e.g., 1, 2, 3, 4, 5)
- **Auto ellipsis**: Shows ellipsis button when page count exceeds numeric button count
- **Flexible binding**: Works with any ItemsControl through PagedSource property
- **On-demand paging**: Load data dynamically for current page only
- **Events**: PageIndexChanging and PageIndexChanged for validation and custom logic
- **Appearance**: Customizable colors, display modes, and orientation

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Required assemblies (Syncfusion.SfGrid.WPF, Syncfusion.Data.WPF)
- Control structure and elements
- Creating first SfDataPager application
- Integrating with SfDataGrid
- Adding SfDataPager to toolbox
- Theme support and theming

### Data Binding and Paging
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Source and PagedSource properties
- PageIndex property and events
- Binding to ListBox, ListView, or other ItemsControls
- On-demand paging with UseOnDemandPaging
- LoadDynamicItems method for dynamic data loading
- PageSize configuration
- Runtime PageSize changes with ComboBox

### Appearance and Styling
📄 **Read:** [references/appearance.md](references/appearance.md)
- AutoEllipsisMode (After, Before, Both, None)
- AccentBackground and AccentForeground colors
- NumericButtonStyle customization
- DisplayMode options (12 different modes)
- Orientation (Horizontal, Vertical)

### Page Navigation
📄 **Read:** [references/page-navigation.md](references/page-navigation.md)
- MoveToFirstPage(), MoveToLastPage() methods
- MoveToNextPage(), MoveToPreviousPage() methods
- MoveToPage(int pageIndex) for direct navigation
- PageIndexChanging event for validation
- Interacting with users before page changes

### Styles and Templates
📄 **Read:** [references/styles-templates.md](references/styles-templates.md)
- Editing styles in Expression Blend
- Editing styles in Visual Studio
- Edit a Copy vs Create Empty options
- Custom control templates
- Resource management

## Quick Start Example

Basic SfDataPager with SfDataGrid integration:

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:sfgrid="clr-namespace:Syncfusion.UI.Xaml.Grid;assembly=Syncfusion.SfGrid.WPF"
        xmlns:datapager="clr-namespace:Syncfusion.UI.Xaml.Controls.DataPager;assembly=Syncfusion.SfGrid.WPF">
    
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>
    
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        
        <!-- DataGrid displays paginated data -->
        <sfgrid:SfDataGrid AutoGenerateColumns="True"
                           ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
        
        <!-- DataPager provides navigation -->
        <datapager:SfDataPager x:Name="sfDataPager"
                               Grid.Row="1"
                               NumericButtonCount="10"
                               PageSize="10"
                               Source="{Binding OrdersDetails}"/>
    </Grid>
</Window>
```

Key concepts:
- **Source**: Bind to your data collection (e.g., `OrdersDetails`)
- **PagedSource**: Automatically wrapped PagedCollectionView for ItemsControl
- **PageSize**: Number of items per page
- **NumericButtonCount**: Number of numeric page buttons to display

## Common Patterns

### Pattern 1: Basic Pagination with DataGrid

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <sfgrid:SfDataGrid ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           PageSize="20"
                           Source="{Binding DataCollection}"/>
</Grid>
```

### Pattern 2: Customized Appearance

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       AccentBackground="DodgerBlue"
                       AccentForeground="White"
                       AutoEllipsisMode="Both"
                       NumericButtonCount="5"
                       PageSize="15"
                       Source="{Binding DataCollection}"/>
```

### Pattern 3: Runtime PageSize Change

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*"/>
        <ColumnDefinition Width="Auto"/>
    </Grid.ColumnDefinitions>
    
    <sfgrid:SfDataGrid ItemsSource="{Binding ElementName=sfDataPager, Path=PagedSource}"/>
    
    <ComboBox Name="pageSizeComboBox"
              Grid.Column="1" Grid.Row="1"
              SelectedIndex="0"
              ItemsSource="{Binding PageSizeOptions}"/>
    
    <datapager:SfDataPager x:Name="sfDataPager"
                           Grid.Row="1"
                           PageSize="{Binding SelectedValue, ElementName=pageSizeComboBox}"
                           Source="{Binding DataCollection}"/>
</Grid>
```

### Pattern 4: On-Demand Paging (Large Datasets)

```xml
<datapager:SfDataPager x:Name="sfDataPager"
                       UseOnDemandPaging="True"
                       PageCount="100"
                       PageSize="20"
                       OnDemandLoading="OnDemandDataLoading"/>
```

```csharp
private void OnDemandDataLoading(object sender, OnDemandLoadingEventArgs args)
{
    // Load data for current page only
    var pageData = dataSource.Skip(args.StartIndex).Take(args.PageSize);
    sfDataPager.LoadDynamicItems(args.StartIndex, pageData);
}
```

### Pattern 5: Page Change Validation

```xml
<datapager:SfDataPager PageIndexChanging="sfDataPager_PageIndexChanging"/>
```

```csharp
private void sfDataPager_PageIndexChanging(object sender, PageIndexChangingEventArgs e)
{
    // Validate or prompt user before page change
    if (HasUnsavedChanges())
    {
        var result = MessageBox.Show("Unsaved changes. Continue?", 
                                     "Confirm", 
                                     MessageBoxButton.YesNo);
        if (result == MessageBoxResult.No)
        {
            e.Cancel = true; // Cancel page navigation
        }
    }
}
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Source` | IEnumerable | Data collection for paging |
| `PagedSource` | PagedCollectionView | Paginated data (bind to ItemsSource) |
| `PageSize` | int | Items per page (default: 0 = all items) |
| `PageIndex` | int | Current page index (0-based) |
| `PageCount` | int | Total number of pages |
| `NumericButtonCount` | int | Number of numeric buttons to display |
| `AutoEllipsisMode` | AutoEllipsisMode | Ellipsis button placement (After, Before, Both, None) |
| `AccentBackground` | Brush | Background color for navigation/selected buttons |
| `AccentForeground` | Brush | Foreground color for selected button |
| `DisplayMode` | PageDisplayMode | Which buttons to show (e.g., FirstLastPreviousNextNumeric) |
| `Orientation` | Orientation | Horizontal or Vertical layout |
| `UseOnDemandPaging` | bool | Enable on-demand data loading |

## Key Methods

| Method | Description |
|--------|-------------|
| `MoveToFirstPage()` | Navigate to first page |
| `MoveToLastPage()` | Navigate to last page |
| `MoveToNextPage()` | Navigate to next page |
| `MoveToPreviousPage()` | Navigate to previous page |
| `MoveToPage(int pageIndex)` | Navigate to specific page |
| `LoadDynamicItems(int startIndex, IEnumerable items)` | Load data for on-demand paging |

## Key Events

| Event | Description |
|-------|-------------|
| `PageIndexChanging` | Before page changes (cancelable) |
| `PageIndexChanged` | After page changes |

## Implementation Workflow

When user requests DataPager implementation:

1. **Assess requirements**: Determine data size, page size needs, appearance preferences
2. **Choose paging mode**: 
   - Normal paging (small to medium datasets)
   - On-demand paging (large datasets, millions of records)
3. **Add assemblies**: Syncfusion.SfGrid.WPF, Syncfusion.Data.WPF (if using with DataGrid)
4. **Set up data binding**: Source → PagedSource → ItemsControl.ItemsSource
5. **Configure appearance**: PageSize, NumericButtonCount, AccentColors, DisplayMode
6. **Add event handlers** (if needed): PageIndexChanging for validation
7. **Test navigation**: Verify all navigation buttons work correctly

## Common Use Cases

- **Paginated data grids**: Display large datasets in DataGrid with page navigation
- **Document viewers**: Navigate through document pages
- **Product catalogs**: Browse items across multiple pages
- **Search results**: Display search results with pagination
- **Report viewers**: Navigate multi-page reports
- **Large list displays**: Any scenario with 100+ items that need pagination
- **Lazy loading scenarios**: Load data as user navigates pages

# Card Sorting in WPF Kanban

Sort cards within columns based on priority, date, status, or custom fields. Sorting helps organize cards for better workflow visibility and management.

## Overview

**Purpose:** Control card order within columns  
**Approaches:** Custom field sorting or index-based sorting  
**Dynamic:** Sorting updates during drag-and-drop with manual refresh  
**Flexibility:** Sort by any numeric or string property

## Sorting Properties

| Property | Type | Description |
|----------|------|-------------|
| `SortingMappingPath` | string | Property name to sort by (e.g., "Priority", "Index") |
| `SortingOrder` | enum | Ascending or Descending sort direction |

### KanbanSortingOrder Values
- `Ascending` - Lower values appear first (A→Z, 1→9)
- `Descending` - Higher values appear first (Z→A, 9→1)

**Default:** If `SortingMappingPath` is empty, cards remain in ItemsSource order (no sorting)

## Custom Field Sorting

Sort cards by a property value (priority, date, status, etc.)

### Basic Example

```xml
<kanban:SfKanban x:Name="kanban" 
                 SortingMappingPath="Priority"
                 SortingOrder="Descending"
                 ItemsSource="{Binding Cards}"
                 ColumnMappingPath="Category"
                 CardDragEnd="OnKanbanCardDragEnd">
</kanban:SfKanban>
```

**C#:**
```csharp
kanban.SortingMappingPath = "Priority";
kanban.SortingOrder = KanbanSortingOrder.Descending;
kanban.ItemsSource = viewModel.Cards;
```

### Custom Data Model

```csharp
public class CardDetails
{
    public string Title { get; set; }
    public string Description { get; set; }
    public string Priority { get; set; }  // "Critical", "High", "Medium", "Low"
    public string Category { get; set; }
}
```

### ViewModel

```csharp
public class SortingViewModel
{
    public ObservableCollection<CardDetails> Cards { get; set; }
    public SortingViewModel()
    {
        Cards = new ObservableCollection<CardDetails>()
        {
            new CardDetails { Title = "Task 1", Priority = "Medium", Category = "Open" },
            new CardDetails { Title = "Task 2", Priority = "Critical", Category = "Open" },
            new CardDetails { Title = "Task 3", Priority = "Low", Category = "In Progress" },
            new CardDetails { Title = "Task 4", Priority = "High", Category = "Open" },
        };
    }
}
```

### Refreshing After Drag-Drop

**Important:** Call `RefreshKanbanColumn()` in CardDragEnd event to apply sorting after drag-drop:

```csharp
private void OnKanbanCardDragEnd(object sender, KanbanDragEndEventArgs e)
{
    // Refresh target column to apply sorting
    kanban.RefreshKanbanColumn(e.TargetKey.ToString());
}
```

**Why Needed:** Drag-drop doesn't automatically re-sort. Manual refresh applies SortingMappingPath logic.

### Custom Card Template for Priority

```xml
<kanban:SfKanban.CardTemplate>
    <DataTemplate>
        <Border Background="#F3CFCE"
                BorderThickness="1"
                CornerRadius="8"
                Padding="8">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                
                <!-- Priority Indicator -->
                <StackPanel Grid.Row="0" 
                           Orientation="Horizontal" 
                           Spacing="4">
                    <TextBlock Text="•" 
                              FontSize="14" 
                              FontWeight="Bold" 
                              Foreground="Orange"/>
                    <TextBlock Text="{Binding Priority}" 
                              FontSize="12" 
                              FontWeight="Bold" 
                              Foreground="Orange"/>
                </StackPanel>
                
                <!-- Title -->
                <TextBlock Grid.Row="1" 
                          Text="{Binding Title}" 
                          FontWeight="Bold" 
                          FontSize="14" 
                          Margin="0,8,0,0"/>
                
                <!-- Description -->
                <TextBlock Grid.Row="2" 
                          Text="{Binding Description}" 
                          FontSize="12" 
                          TextWrapping="Wrap" 
                          Margin="0,4,0,0"/>
            </Grid>
        </Border>
    </DataTemplate>
</kanban:SfKanban.CardTemplate>
```

## Index-Based Sorting

Sort by numeric index for precise positioning control.

### Overview

**Purpose:** Maintain exact card order with manual positioning  
**Approach:** Update index values after drag-drop  
**Use Case:** User-defined card order, manual prioritization

### Data Model

```csharp
public class CardDetails
{
    public string Title { get; set; }
    public string Description { get; set; }
    public int Index { get; set; }  // Numeric index for sorting
    public string Category { get; set; }
}
```

### XAML Setup

```xml
<kanban:SfKanban x:Name="kanban" 
                 SortingMappingPath="Index"
                 SortingOrder="Ascending"
                 ItemsSource="{Binding Cards}"
                 ColumnMappingPath="Category"
                 CardDragStart="OnKanbanCardDragStart"
                 CardDragEnd="OnKanbanCardDragEnd">
</kanban:SfKanban>
```

### Event Handlers

```csharp
private KanbanCardItem selectedCard;
private KanbanColumn targetColumn;

// Track dragged card
private void OnKanbanCardDragStart(object sender, KanbanDragStartEventArgs e)
{
    this.selectedCard = e.SelectedCard;
}

// Handle drop with index update
private void OnKanbanCardDragEnd(object sender, KanbanDragEndEventArgs e)
{
    if (this.kanban == null || this.selectedCard == null || e.TargetColumn == null)
    {
        return;
    }

    this.ApplySortingWithoutPositionChange(e);
    this.kanban.RefreshKanbanColumn(e.TargetKey.ToString());   
}

private void ApplySortingWithoutPositionChange(KanbanDragEndEventArgs e)
{
    var cardItems = e.TargetColumn.Items.Cast<KanbanCardItem>().ToList();
    if (this.kanban == null || this.selectedCard == null || this.selectedCard.Content == null || e.TargetColumn == null || e.TargetKey == null || cardItems == null || !cardItems.Any()
        || e.SelectedColumn == null || (e.SelectedColumn == e.TargetColumn && e.SelectedColumnIndex == e.TargetCardIndex))
    {
        return;
    }

    // Retrieve sorting configuration
    var sortMappingPath = this.kanban.SortingMappingPath;
    var sortingOrder = this.kanban.SortingOrder;
    CardDetails cardDetails = this.selectedCard.Content as CardDetails;

    // Proceed only if sorting path is defined
    if (cardDetails == null || string.IsNullOrEmpty(sortMappingPath))
    {
        return;
    }

    // Extract items from the target colum
    var targetColumnItems = cardItems.Where(card => card != null && card.Content is CardDetails).ToList();

    // Sort items based on the sorting order
    if (targetColumnItems.Count > 0)
    {
        Func<KanbanCardItem, object> keySelector = item => this.GetPropertyInfo(item.GetType(), sortMappingPath);
        targetColumnItems = sortingOrder == KanbanSortingOrder.Ascending
            ? targetColumnItems.OrderBy(item => keySelector(item) ?? 0).ToList()
            : targetColumnItems.OrderByDescending(item => keySelector(item) ?? 0).ToList();
    }

    // Determine the index to insert the dragged card.
    int currentCardIndex = e.TargetCardIndex;

    if (e.SelectedColumn != e.TargetColumn)
    {
        cardDetails.Category = e.TargetKey.ToString();
    }

    if (currentCardIndex >= 0 && currentCardIndex <= targetColumnItems.Count)
    {
        targetColumnItems.Insert(currentCardIndex, this.selectedCard);
    }
    else
    {
        targetColumnItems.Add(this.selectedCard);
        currentCardIndex = targetColumnItems.Count - 1;
    }

    // Update index property of all items using smart positioning logic
    this.ApplySmartIndexUpdate(targetColumnItems, currentCardIndex);
}

private void ApplySmartIndexUpdate(List<KanbanCardItem> items, int droppedIndex)
{
    if (this.kanban == null || items == null || items.Count == 0)
    {
        return;
    }

    if (this.kanban.SortingOrder == KanbanSortingOrder.Ascending)
    {
        this.HandleAscendingIndexSorting(items, droppedIndex);
        return;
    }

    this.HandleDescendingIndexSorting(items, droppedIndex);
}

private void HandleAscendingIndexSorting(List<KanbanCardItem> items, int currentCardIndex)
{
    int afterCardIndex = -1;
    int lastItemIndex = -1;

    // Get the index of the card after the insertion point
    if (currentCardIndex < items.Count - 1)
    {
        var afterCard = items[currentCardIndex + 1];
        afterCardIndex = GetCardIndex(afterCard?.Content) ?? -1;
    }

    for (int i = 0; i < items.Count; i++)
    {
        var item = items[i].Content;
        if (item == null)
        {
            continue;
        }

        PropertyInfo propertyInfo = this.GetPropertyInfo(item.GetType(), "Index");
        if (propertyInfo == null)
        {
            continue;
        }

        int itemIndex = Convert.ToInt32(propertyInfo.GetValue(item) ?? 0);
        bool isFirstItem = i == 0;
        if (isFirstItem)
        {
            // If the inserted card is at the beginning, assign a smart index
            if (currentCardIndex == 0)
            {
                lastItemIndex = afterCardIndex > 1 ? afterCardIndex - 1 : 1;
                propertyInfo.SetValue(item, lastItemIndex);
            }
            else
            {
                lastItemIndex = itemIndex;
            }
        }
        else
        {
            // Increment index for subsequent items
            lastItemIndex++;
            propertyInfo.SetValue(item, lastItemIndex);
        }
    }
}

private void HandleDescendingIndexSorting(List<KanbanCardItem> items, int currentCardIndex)
{
    int beforeCardIndex = -1;
    int lastItemIndex = -1;

    // Get the index of the card before the insertion point
    if (currentCardIndex > 0 && currentCardIndex < items.Count)
    {
        var cardBefore = items[currentCardIndex - 1];
        beforeCardIndex = GetCardIndex(cardBefore?.Content) ?? -1;
    }

    for (int i = items.Count - 1; i >= 0; i--)
    {
        var item = items[i].Content;
        if (item == null)
        {
            continue;
        }

        PropertyInfo propertyInfo = this.GetPropertyInfo(item.GetType(), "Index");
        if (propertyInfo == null)
        {
            continue;
        }

        int itemIndex = Convert.ToInt32(propertyInfo.GetValue(item) ?? 0);
        bool isLastItem = i == items.Count - 1;
        if (isLastItem)
        {
            // If the inserted card is at the end, assign a smart index
            if (currentCardIndex == items.Count - 1)
            {
                lastItemIndex = beforeCardIndex > 1 ? beforeCardIndex - 1 : 1;
                propertyInfo.SetValue(item, lastItemIndex);
            }
            else
            {
                lastItemIndex = itemIndex;
            }
        }
        else
        {
            lastItemIndex++;
            propertyInfo.SetValue(item, lastItemIndex);
        }
    }
}

private int? GetCardIndex(object cardDetails)
{
    if (cardDetails == null)
    {
        return null;
    }

    PropertyInfo propertyInfo = this.GetPropertyInfo(cardDetails.GetType(), "Index");
    if (propertyInfo == null)
    {
        return null;
    }

    var indexValue = propertyInfo.GetValue(cardDetails);
    if (indexValue != null)
    {
        return Convert.ToInt32(indexValue);
    }

    return null;
}

private PropertyInfo GetPropertyInfo(Type type, string key)
{
    return this.GetPropertyInfoCustomType(type, key);
}

private PropertyInfo GetPropertyInfoCustomType(Type type, string key)
{
    return type.GetProperty(key);
}
```

### Card Template with Index Display

```xml
<kanban:SfKanban.CardTemplate>
    <DataTemplate>
        <Border Background="#F3EADC"
                BorderThickness="1"
                CornerRadius="8"
                Padding="8">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                
                <!-- Rank Display -->
                <StackPanel Grid.Row="0" 
                           Orientation="Horizontal"
                           HorizontalAlignment="Right">
                    <TextBlock Text="Rank #" 
                              FontSize="12" 
                              FontWeight="Bold" 
                              Foreground="#026B6E"/>
                    <TextBlock Text="{Binding Index}" 
                              FontSize="12" 
                              FontWeight="Bold" 
                              Foreground="#026B6E"/>
                </StackPanel>
                
                <!-- Title -->
                <TextBlock Grid.Row="1" 
                          Text="{Binding Title}" 
                          FontWeight="Bold" 
                          FontSize="14" 
                          Margin="0,8,0,0"/>
                
                <!-- Description -->
                <TextBlock Grid.Row="2" 
                          Text="{Binding Description}" 
                          FontSize="12" 
                          TextWrapping="Wrap" 
                          Margin="0,4,0,0"/>
            </Grid>
        </Border>
    </DataTemplate>
</kanban:SfKanban.CardTemplate>
```

## Sorting by Date

Sort cards by due date or creation date:

### Data Model

```csharp
public class CardDetails
{
    public string Title { get; set; }
    public DateTime DueDate { get; set; }
    public string Category { get; set; }
}
```

### XAML

```xml
<kanban:SfKanban SortingMappingPath="DueDate"
                 SortingOrder="Ascending"
                 ItemsSource="{Binding Cards}"
                 CardDragEnd="OnKanbanCardDragEnd">
</kanban:SfKanban>
```

**Result:** Cards sorted by earliest due date first

## Disabling Sorting

To disable sorting and maintain ItemsSource order:

```csharp
// Don't set SortingMappingPath
kanban.SortingMappingPath = string.Empty;  // Or omit entirely
```

**Effect:** Cards appear in order they exist in ItemsSource collection

## Common Patterns

### Pattern 1: Priority-Based (String)

```csharp
// Priority property: "Critical", "High", "Medium", "Low"
kanban.SortingMappingPath = "Priority";
kanban.SortingOrder = KanbanSortingOrder.Descending;
```

**Sort Order:** Critical → High → Medium → Low (alphabetically descending)

### Pattern 2: Priority-Based (Numeric)

```csharp
// Priority property: 1 (Critical), 2 (High), 3 (Medium), 4 (Low)
kanban.SortingMappingPath = "Priority";
kanban.SortingOrder = KanbanSortingOrder.Ascending;
```

**Sort Order:** 1 → 2 → 3 → 4 (most to least important)

### Pattern 3: Date-Based (Oldest First)

```csharp
kanban.SortingMappingPath = "CreatedDate";
kanban.SortingOrder = KanbanSortingOrder.Ascending;
```

### Pattern 4: Date-Based (Newest First)

```csharp
kanban.SortingMappingPath = "CreatedDate";
kanban.SortingOrder = KanbanSortingOrder.Descending;
```

### Pattern 5: Custom Manual Order

```csharp
kanban.SortingMappingPath = "Index";
kanban.SortingOrder = KanbanSortingOrder.Ascending;
// Implement index update logic in CardDragEnd event
```

## Best Practices

1. **Always Refresh After Drop:** Call RefreshKanbanColumn() in CardDragEnd event
2. **Numeric for Complex Sort:** Use numeric values for multi-level priorities
3. **Index for Manual Order:** Use index-based sorting for user-defined order
4. **Test Sort Logic:** Verify ascending vs descending produces expected order
5. **Property Type Matters:** Ensure sortable type (string, int, DateTime)
6. **Update Model:** Index-based sorting requires updating model in code
7. **Visual Indicators:** Show sort property on cards for clarity

## Troubleshooting

### Sorting Not Applied

**Problem:** Cards not in expected order

**Solutions:**
1. Check SortingMappingPath is set to valid property name
2. Verify SortingOrder is specified (Ascending or Descending)
3. Ensure property exists in data model
4. Check property has values (not all null)

### Sorting Lost After Drag-Drop

**Problem:** Card order reverts after drop

**Solutions:**
1. Add CardDragEnd event handler
2. Call RefreshKanbanColumn() in handler
3. Verify correct column category passed to RefreshKanbanColumn()
4. For index-based, ensure indexes are updated in code

### Cards Not Sorting Correctly

**Problem:** Sort order doesn't match expectation

**Solutions:**
1. Verify Ascending vs Descending setting
2. Check property data type (string vs numeric sorts differently)
3. For string sorting, understand alphabetical ordering
4. For numeric, ensure values are actually numbers, not strings

### Index-Based Sorting Errors

**Problem:** Indexes not updating correctly

**Solutions:**
1. Verify Index property is numeric type (int)
2. Check index update logic handles all scenarios
3. Ensure selectedCard and targetColumn are set correctly
4. Test with simple index assignment first

## Notes

- **Custom Models:** Sorting works with both KanbanModel and custom models
- **Property Type:** Works with IComparable types (string, int, DateTime, etc.)
- **Performance:** Sorting is efficient, even with many cards
- **Persistence:** Sorting is client-side only, not persisted automatically
- **Multiple Properties:** Sort by one property at a time (no multi-level sorting)
- **Refresh Required:** Always call RefreshKanbanColumn() after programmatic changes

## Example Use Cases

### 1. Bug Tracking by Severity
```csharp
SortingMappingPath = "Severity";  // Critical, High, Medium, Low
SortingOrder = Descending;
```

### 2. Tasks by Due Date
```csharp
SortingMappingPath = "DueDate";
SortingOrder = Ascending;  // Earliest first
```

### 3. Features by Priority Score
```csharp
SortingMappingPath = "PriorityScore";  // 1-10
SortingOrder = Descending;  // Highest score first
```

### 4. Support Tickets by Creation Time
```csharp
SortingMappingPath = "CreatedTime";
SortingOrder = Ascending;  // Oldest tickets first
```
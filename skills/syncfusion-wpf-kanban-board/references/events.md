# Kanban Events in WPF

Handle user interactions and drag-drop operations with Kanban events. Respond to card taps, track card movement, and implement custom drag-drop logic.

## Table of Contents
- [Overview](#overview)
- [Available Events](#available-events)
- [CardTapped Event](#cardtapped-event)
- [CardDragStart Event](#carddragstart-event)
- [CardDragOver Event](#carddragover-event)
- [CardDragEnd Event](#carddrop-event)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

**Purpose:** Respond to user interactions and control Kanban behavior  
**Event Categories:** Card tap events, drag-drop lifecycle events  
**Pattern:** MVVM-compatible with command support  
**Use Cases:** Navigation, validation, logging, custom logic

## Available Events

| Event | Description | Event Args |
|-------|-------------|------------|
| `CardTapped` | Fires when user taps/clicks a card | KanbanTappedEventArgs |
| `CardDoubleTapped` | Fires when user double taps/clicks a card | KanbanDoubleTappedEventArgs |
| `CardDragStart` | Fires when drag operation begins | KanbanDragStartEventArgs |
| `CardDragOver` | Fires while dragging over columns | KanbanDragOverEventArgs |
| `CardDragEnd` | Fires when card is dropped | KanbanDragEndEventArgs |
| `CardDragEnter` | Fires when drag operation enter | KanbanDragEnterEventArgs |
| `CardDragLeave` | Fires when drag operation leave | KanbanDragLeaveEventArgs |
| `ColumnGenerated` | Fires when column is generated | KanbanColumnGeneratedEventArgs |
| `ColumnsGenerated` | Fires when columns is generated | KanbanColumnsGeneratedEventArgs |

## CardTapped Event

Respond when user taps or clicks a card.

### Basic Example

```xml
<kanban:SfKanban x:Name="kanban"
                 ItemsSource="{Binding Cards}"
                 CardTapped="OnCardTapped">
</kanban:SfKanban>
```

**C#:**
```csharp
private void OnCardTapped(object sender, KanbanTappedEventArgs e)
{
    // Access the tapped card
    var cardItem = e.SelectedCard;
    var cardData = cardItem.Content as CardDetails;
}
```

## CardDragStart Event

Fires when user begins dragging a card. Use to track the dragged card or prevent dragging.

### XAML

```xml
<kanban:SfKanban x:Name="kanban"
                 ItemsSource="{Binding Cards}"
                 CardDragStart="OnCardDragStarting">
</kanban:SfKanban>
```

### Event Handler

```csharp
private KanbanCardItem selectedCard;

private void OnCardDragStarting(object sender, KanbanDragStartEventArgs e)
{
    // Store reference to dragged card
    selectedCard = e.SelectedCard;

    var cardData = e.SelectedCard.Content as CardDetails;

    // Log drag operation
    System.Diagnostics.Debug.WriteLine($"Dragging card: {cardData.Title}");

    // Optional: Show visual feedback
    ShowDragFeedback(cardData);
}
```

### Event Arguments Properties

```csharp
public class KanbanDragStartEventArgs
{
    // The card being dragged
    public KanbanCardItem Card { get; }
}
```
## CardDragOver Event

Fires while dragging a card over columns. Use to validate drop locations or provide feedback.

### XAML

```xml
<kanban:SfKanban x:Name="kanban"
                 ItemsSource="{Binding Cards}"
                 CardDragOver="OnCardDragOver">
</kanban:SfKanban>
```

### Event Handler

```csharp
 private void OnCardDragOver(object sender, KanbanDragOverEventArgs e)
 {
     // The column currently being hovered.
     var hoveredColumn = e.CurrentColumn;

     // Zero-based indices reported by the event.
     int hoveredColumnIndex = e.CurrentColumnIndex;

     // Example validation: disallow dropping into a specific column (by index or by key).
     bool isDropAllowed = IsDropAllowed(hoveredColumn, hoveredColumnIndex);

     // Provide visual feedback to the user (e.g., highlight allowed/blocked).
     UpdateDragFeedback(hoveredColumn, isDropAllowed);
 }
```
## CardDragEnd Event

Fires when card is dropped into a column. Use to update data, validate moves, or trigger actions.

### XAML

```xml
<kanban:SfKanban x:Name="kanban"
                 ItemsSource="{Binding Cards}"
                 CardDragEnd="OnCardDrop">
</kanban:SfKanban>
```

### Event Handler

```csharp
private void OnCardDrop(object sender, KanbanDragEndEventArgs e)
{
    var sourceColumn = e.SelectedColumn;
    KanbanColumn targetColumn = e.TargetColumn;
    var targetIndex = e.TargetCardIndex;

    // Log the drop
    System.Diagnostics.Debug.WriteLine(
        $"Dropped from {sourceColumn.Title} to {targetColumn.Title} at index {targetIndex}"
    );

    // Update card data
    UpdateCardOnDrop(e);

    // Refresh column if using sorting
    kanban.RefreshKanbanColumn(e.TargetKey.ToString());
}
```

## Complete Event Flow Example

Track entire drag-drop lifecycle:

```xml
<kanban:SfKanban x:Name="kanban"
                 ItemsSource="{Binding Cards}"
                 CardTapped="OnCardTapped"
                 CardDragStart="OnCardDragStarting"
                 CardDragOver="OnCardDragOver"
                 CardDragEnd="OnCardDrop">
</kanban:SfKanban>
```

**Code-Behind:**
```csharp
private KanbanCardItem selectedCard;
private string dragStartTime;

 private void OnCardTapped(object sender, KanbanTappedEventArgs e)
 {
     var card = e.SelectedCard.Content as CardDetails;
     Debug.WriteLine($"Card tapped: {card.Title}");
 }

private void OnCardDragStarting(object sender, KanbanDragStartEventArgs e)
{
    selectedCard = e.SelectedCard;
    dragStartTime = DateTime.Now.ToString("HH:mm:ss");

    var card = e.SelectedCard.Content as CardDetails;
    Debug.WriteLine($"[{dragStartTime}] Drag started: {card.Title}");
}

private void OnCardDragOver(object sender, KanbanDragOverEventArgs e)
{
    Debug.WriteLine($"Dragging over: {e.CurrentColumn.Title}");
}

 private void OnCardDrop(object sender, KanbanDragEndEventArgs e)
 {
     var card = selectedCard?.Content as CardDetails;
     var dropTime = DateTime.Now.ToString("HH:mm:ss");

     Debug.WriteLine($"[{dropTime}] Dropped: {card.Title}");
     Debug.WriteLine($"  From: {e.SelectedColumn.Title}");
     Debug.WriteLine($"  To: {e.TargetColumn.Title}");
     Debug.WriteLine($"  Index: {e.TargetCardIndex}");

     // Update data
     card.Category = e.TargetKey.ToString();

     // Refresh column
     kanban.RefreshKanbanColumn(e.TargetKey.ToString());
 }
```

## Best Practices

1. **Store References:** Save selectedCard in CardDragStart for use in CardDragEnd
2. **Always Refresh:** Call RefreshKanbanColumn() after data changes in CardDragEnd when sorting scenario is enabled.
3. **Validate Moves:** Check business rules before applying drops
4. **Error Handling:** Wrap async operations in try-catch
5. **Use Commands:** Prefer MVVM commands over code-behind events when possible
6. **Log Events:** Track user actions for debugging and analytics
7. **Update Timestamps:** Maintain LastModified or similar audit fields
8. **Server Sync:** Update backend when cards move
9. **Visual Feedback:** Show users what's happening during drag-drop
10. **Null Checks:** Always verify card data exists before accessing

## Troubleshooting

### CardTapped Not Firing

**Problem:** Tapping cards doesn't trigger event

**Solutions:**
1. Verify CardTapped event is wired in XAML
2. Check if CardTemplate intercepts pointer events
3. Ensure kanban control has focus
4. Test with simple handler to isolate issue

### Card Reference Null in CardDragEnd

**Problem:** selectedCard is null in CardDragEnd handler

**Solutions:**
1. Verify CardDragStart event is wired
2. Check selectedCard is assigned in CardDragStart
3. Ensure field is at class level (not local variable)
4. Verify drag operation completes (not cancelled)

### Changes Not Visible After Drop

**Problem:** Card doesn't update position/column

**Solutions:**
1. Call RefreshKanbanColumn() after data changes
2. Verify correct column category passed to RefreshKanbanColumn()
3. Ensure ObservableCollection is used for ItemsSource
4. Check INotifyPropertyChanged is implemented on model

### Commands Not Executing

**Problem:** CardTappedCommand doesn't fire

**Solutions:**
1. Verify command is public property
2. Check binding path in XAML
3. Ensure command accepts KanbanTappedEventArgs parameter
4. Verify ViewModel is set as DataContext
5. Check CanExecute returns true

### Drag-Drop Performance Issues

**Problem:** Lag during drag operations

**Solutions:**
1. Minimize work in CardDragOver (fires frequently)
2. Use async operations in CardDragEnd
3. Batch server updates instead of per-card
4. Optimize card templates
5. Reduce number of cards or use virtualization

## Notes

- **Event Order:** CardDragStart → CardDragOver (multiple) → CardDragEnd
- **CardTappedCommand:** MVVM alternative to CardTapped event
- **Async Operations:** Supported in event handlers (use async void)
- **Thread Safety:** Events fire on UI thread
- **Event Arguments:** All are read-only, provide access to event context
- **RefreshKanbanColumn():** Required for sorting/WIP limits after drops

## Example Use Cases

### 1. Audit Logging
```csharp
private void OnCardDrop(object sender, KanbanDragEndEventArgs e)
{
    var auditEntry = new AuditLog
    {
        Action = "CardMoved",
        Timestamp = DateTime.UtcNow,
        UserId = currentUser.Id,
        Details = $"Moved card to {e.TargetColumn.Title}"
    };

    await SaveAuditLogAsync(auditEntry);
}
```

### 2. Notification Triggers
```csharp
private void OnCardDrop(object sender, KanbanDragEndEventArgs e)
{
    if (e.TargetColumn.Title == "Done")
    {
        SendCompletionNotification(selectedCard.Content);
    }
}
```

### 4. Workflow Automation
```csharp
private void OnCardDrop(object sender, KanbanDragEndEventArgs e)
{
    var card = selectedCard.Content as CardDetails;

    // Auto-assign when moved to "In Progress"
    if (e.TargetColumn.Title == "In Progress" && string.IsNullOrEmpty(card.Assignee))
    {
        card.Assignee = currentUser.Name;
    }

    // Set completion date when moved to "Done"
    if (e.TargetColumn.Title == "Done")
    {
        card.CompletedDate = DateTime.Now;
    }
}
```
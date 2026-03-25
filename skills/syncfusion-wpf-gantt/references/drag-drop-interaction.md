# Drag and Drop Interaction

Enable interactive task manipulation through drag-and-drop operations in the WPF Gantt control.

## Drag to Resize Tasks

Allow users to adjust task dates by dragging task bars.

### Enable Drag Resizing

```csharp
// Enable dragging to change dates
ganttControl.AllowDragToolTip = true;
ganttControl.EnableTaskDrag = true;
```

### Handle Date Changes

```csharp
ganttControl.TaskDragCompleted += (sender, e) =>
{
    var task = e.Node.Content as TaskDetails;
    Console.WriteLine($"Task '{task.TaskName}' dates changed:");
    Console.WriteLine($"New Start: {task.StartDate}");
    Console.WriteLine($"New Finish: {task.FinishDate}");
    
    // Update database or perform validation
    UpdateTaskInDatabase(task);
};
```

## Drag-and-Drop Reordering

Enable reordering tasks within the grid by dragging rows.

### Enable Row Dragging

```csharp
// Enable drag-and-drop row reordering
ganttControl.AllowDragging = true;
```

### Handle Reordering Events

```csharp
ganttControl.ItemsSource = TaskCollection;

// Handle drop event
ganttControl.ItemDropped += (sender, e) =>
{
    var draggedTask = e.DraggedItem as TaskDetails;
    var targetTask = e.TargetItem as TaskDetails;
    int newIndex = e.DropIndex;
    
    Console.WriteLine($"Moved '{draggedTask.TaskName}' to position {newIndex}");
    
    // Update task order in collection
    TaskCollection.Move(TaskCollection.IndexOf(draggedTask), newIndex);
};
```

## Drag Constraints

Control what can be dragged and where tasks can be dropped.

### Restrict Dragging Certain Tasks

```csharp
ganttControl.ItemDragStarting += (sender, e) =>
{
    var task = e.DraggedItem as TaskDetails;
    
    // Prevent dragging milestones
    if (task.IsMilestone)
    {
        e.Cancel = true;
        MessageBox.Show("Milestones cannot be moved.");
    }
    
    // Prevent dragging locked tasks
    if (task.IsLocked)
    {
        e.Cancel = true;
    }
};
```

### Restrict Drop Targets

```csharp
ganttControl.ItemDropping += (sender, e) =>
{
    var draggedTask = e.DraggedItem as TaskDetails;
    var targetTask = e.TargetItem as TaskDetails;
    
    // Prevent dropping on completed tasks
    if (targetTask.Progress == 100)
    {
        e.Cancel = true;
        MessageBox.Show("Cannot drop on completed tasks.");
    }
    
    // Prevent creating circular dependencies
    if (WouldCreateCircularDependency(draggedTask, targetTask))
    {
        e.Cancel = true;
    }
};
```

## Drag Tooltips

Show information during drag operations.

### Custom Drag Tooltip

```csharp
ganttControl.AllowDragToolTip = true;

ganttControl.DragTooltipTemplate = CreateDragTooltipTemplate();

private DataTemplate CreateDragTooltipTemplate()
{
    var template = new DataTemplate();
    var factory = new FrameworkElementFactory(typeof(Border));
    factory.SetValue(Border.BackgroundProperty, Brushes.White);
    factory.SetValue(Border.BorderBrushProperty, Brushes.Gray);
    factory.SetValue(Border.BorderThicknessProperty, new Thickness(1));
    factory.SetValue(Border.PaddingProperty, new Thickness(5));
    
    var stackPanel = new FrameworkElementFactory(typeof(StackPanel));
    
    var nameText = new FrameworkElementFactory(typeof(TextBlock));
    nameText.SetBinding(TextBlock.TextProperty, new Binding("TaskName"));
    stackPanel.AppendChild(nameText);
    
    var dateText = new FrameworkElementFactory(typeof(TextBlock));
    dateText.SetBinding(TextBlock.TextProperty, 
        new Binding("StartDate") { StringFormat = "Start: {0:MMM d}" });
    stackPanel.AppendChild(dateText);
    
    factory.AppendChild(stackPanel);
    template.VisualTree = factory;
    
    return template;
}
```

## Automatic Data Synchronization

Ensure data model stays synchronized with visual changes.

```csharp
public class TaskDetails : INotifyPropertyChanged
{
    private DateTime _startDate;
    private DateTime _finishDate;
    
    public DateTime StartDate
    {
        get => _startDate;
        set
        {
            if (_startDate != value)
            {
                _startDate = value;
                OnPropertyChanged(nameof(StartDate));
                
                // Recalculate duration
                Duration = (FinishDate - StartDate).Days;
                
                // Update dependent tasks
                UpdateDependentTasks();
            }
        }
    }
    
    public DateTime FinishDate
    {
        get => _finishDate;
        set
        {
            if (_finishDate != value)
            {
                _finishDate = value;
                OnPropertyChanged(nameof(FinishDate));
                
                // Recalculate duration
                Duration = (FinishDate - StartDate).Days;
            }
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Complete Example

```csharp
public class MainWindow : Window
{
    private GanttControl ganttControl;
    private ObservableCollection<TaskDetails> TaskCollection;
    
    public MainWindow()
    {
        InitializeComponent();
        SetupGanttInteraction();
    }
    
    private void SetupGanttInteraction()
    {
        // Enable all drag features
        ganttControl.EnableTaskDrag = true;
        ganttControl.AllowDragging = true;
        ganttControl.AllowDragToolTip = true;
        
        // Subscribe to events
        ganttControl.TaskDragStarting += OnTaskDragStarting;
        ganttControl.TaskDragCompleted += OnTaskDragCompleted;
        ganttControl.ItemDragStarting += OnItemDragStarting;
        ganttControl.ItemDropped += OnItemDropped;
    }
    
    private void OnTaskDragStarting(object sender, TaskDragStartingEventArgs e)
    {
        var task = e.Node.Content as TaskDetails;
        Console.WriteLine($"Starting to drag task: {task.TaskName}");
    }
    
    private void OnTaskDragCompleted(object sender, TaskDragCompletedEventArgs e)
    {
        var task = e.Node.Content as TaskDetails;
        
        // Validate new dates
        if (task.StartDate < DateTime.Today)
        {
            MessageBox.Show("Cannot schedule tasks in the past.");
            // Revert changes
            task.StartDate = e.OriginalStartDate;
            task.FinishDate = e.OriginalFinishDate;
        }
        else
        {
            // Save to database
            SaveTaskChanges(task);
        }
    }
    
    private void OnItemDragStarting(object sender, ItemDragStartingEventArgs e)
    {
        var task = e.DraggedItem as TaskDetails;
        
        // Prevent dragging locked tasks
        if (task.IsLocked)
        {
            e.Cancel = true;
        }
    }
    
    private void OnItemDropped(object sender, ItemDroppedEventArgs e)
    {
        var draggedTask = e.DraggedItem as TaskDetails;
        int newIndex = e.DropIndex;
        
        // Update task order
        TaskCollection.Move(TaskCollection.IndexOf(draggedTask), newIndex);
        
        // Save new order
        SaveTaskOrder();
    }
    
    private void SaveTaskChanges(TaskDetails task)
    {
        // Database save logic
    }
    
    private void SaveTaskOrder()
    {
        // Database save logic
    }
}
```

## Best Practices

1. **Always Validate:** Check constraints before accepting drag operations.

2. **Provide Feedback:** Use tooltips to show what's happening during drag.

3. **Handle Undo:** Consider implementing undo/redo for drag operations.

4. **Update Dependencies:** When dragging tasks with dependencies, update related tasks.

5. **Persist Changes:** Save changes to the data source after drag operations complete.

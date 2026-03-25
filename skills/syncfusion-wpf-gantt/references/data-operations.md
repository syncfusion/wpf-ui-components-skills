# Data Operations

Comprehensive guide for filtering, sorting, virtualization, and import/export operations in the WPF Gantt control.

## Table of Contents
- [Filtering Tasks](#filtering-tasks)
- [Sorting Tasks](#sorting-tasks)
- [Data Virtualization](#data-virtualization)
- [XML Import and Export](#xml-import-and-export)
- [Bulk Operations](#bulk-operations)

## Filtering Tasks

Filter tasks based on various criteria.

### Predicate-Based Filtering

```csharp
// Filter to show only incomplete tasks
ganttControl.FilterPredicate = task =>
{
    var taskDetails = task as TaskDetails;
    return taskDetails.Progress < 100;
};

// Refresh view
ganttControl.RefreshGantt();
```

### Date Range Filtering

```csharp
private void FilterByDateRange(DateTime startDate, DateTime endDate)
{
    ganttControl.FilterPredicate = task =>
    {
        var taskDetails = task as TaskDetails;
        return taskDetails.StartDate >= startDate && 
               taskDetails.FinishDate <= endDate;
    };
    
    ganttControl.RefreshGantt();
}
```

### Multi-Criteria Filtering

```csharp
public class TaskFilter
{
    public bool ShowCompleted { get; set; }
    public bool ShowOverdue { get; set; }
    public string ResourceFilter { get; set; }
    public string PriorityFilter { get; set; }
}

private void ApplyFilter(TaskFilter filter)
{
    ganttControl.FilterPredicate = task =>
    {
        var taskDetails = task as TaskDetails;
        
        // Completed filter
        if (!filter.ShowCompleted && taskDetails.Progress == 100)
            return false;
        
        // Overdue filter
        if (filter.ShowOverdue && 
            (taskDetails.FinishDate >= DateTime.Today || taskDetails.Progress == 100))
            return false;
        
        // Resource filter
        if (!string.IsNullOrEmpty(filter.ResourceFilter))
        {
            if (taskDetails.Resources == null || 
                !taskDetails.Resources.Any(r => r.Name.Contains(filter.ResourceFilter)))
                return false;
        }
        
        // Priority filter
        if (!string.IsNullOrEmpty(filter.PriorityFilter) && 
            taskDetails.Priority != filter.PriorityFilter)
            return false;
        
        return true;
    };
    
    ganttControl.RefreshGantt();
}
```

### Clear Filtering

```csharp
private void ClearFilter()
{
    ganttControl.FilterPredicate = null;
    ganttControl.RefreshGantt();
}
```

## Sorting Tasks

Sort tasks by various properties.

### Sort by Property

```csharp
// Sort by start date
var sortedTasks = new ObservableCollection<TaskDetails>(
    TaskCollection.OrderBy(t => t.StartDate)
);
ganttControl.ItemsSource = sortedTasks;
```

### Multi-Level Sorting

```csharp
// Sort by priority, then by start date
var sortedTasks = new ObservableCollection<TaskDetails>(
    TaskCollection
        .OrderBy(t => t.Priority)
        .ThenBy(t => t.StartDate)
);
ganttControl.ItemsSource = sortedTasks;
```

### Custom Sorting with IComparer

```csharp
public class TaskComparer : IComparer<TaskDetails>
{
    public int Compare(TaskDetails x, TaskDetails y)
    {
        // Sort by progress (incomplete first), then by start date
        if (x.Progress == 100 && y.Progress < 100)
            return 1;
        if (x.Progress < 100 && y.Progress == 100)
            return -1;
        
        return x.StartDate.CompareTo(y.StartDate);
    }
}

// Apply custom sorting
var sortedTasks = new ObservableCollection<TaskDetails>(
    TaskCollection.OrderBy(t => t, new TaskComparer())
);
ganttControl.ItemsSource = sortedTasks;
```

## Data Virtualization

Improve performance with large datasets using virtualization.

### Enable Virtualization

```csharp
// Enable row virtualization for better performance
ganttControl.EnableVirtualization = true;
```

### Virtual Mode Example

```csharp
public class LargeProjectViewModel
{
    private ObservableCollection<TaskDetails> _allTasks;
    
    public LargeProjectViewModel()
    {
        // Load large dataset (e.g., 10,000+ tasks)
        _allTasks = LoadTasksFromDatabase();
        
        // Enable virtualization
        GanttControl.EnableVirtualization = true;
        GanttControl.ItemsSource = _allTasks;
    }
    
    private ObservableCollection<TaskDetails> LoadTasksFromDatabase()
    {
        // Load tasks efficiently
        var tasks = new ObservableCollection<TaskDetails>();
        
        using (var context = new ProjectContext())
        {
            // Use deferred execution
            var dbTasks = context.Tasks
                .Include(t => t.Resources)
                .Include(t => t.Dependencies)
                .AsNoTracking()
                .ToList();
            
            foreach (var task in dbTasks)
            {
                tasks.Add(new TaskDetails(task));
            }
        }
        
        return tasks;
    }
}
```

### Performance Best Practices

```csharp
// 1. Disable auto-expand for large hierarchies
ganttControl.AutoExpandMode = AutoExpandMode.None;

// 2. Use lightweight data structures
public class LightweightTaskDetails
{
    // Only essential properties
    public int TaskId { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public DateTime FinishDate { get; set; }
    public double Progress { get; set; }
}

// 3. Load on demand
private void LoadChildrenOnExpand(object sender, ExpandEventArgs e)
{
    var task = e.Node.Content as TaskDetails;
    
    if (!task.ChildrenLoaded)
    {
        task.Child = LoadChildTasks(task.TaskId);
        task.ChildrenLoaded = true;
    }
}
```

## XML Import and Export

Import and export Gantt data to/from XML format.

### Export to XML

```csharp
using System.Xml.Serialization;

private void ExportToXml(string filePath)
{
    var serializer = new XmlSerializer(typeof(ObservableCollection<TaskDetails>));
    
    using (var writer = new StreamWriter(filePath))
    {
        serializer.Serialize(writer, TaskCollection);
    }
    
    MessageBox.Show($"Project exported to {filePath}");
}
```

### Import from XML

```csharp
private void ImportFromXml(string filePath)
{
    var serializer = new XmlSerializer(typeof(ObservableCollection<TaskDetails>));
    
    using (var reader = new StreamReader(filePath))
    {
        var importedTasks = (ObservableCollection<TaskDetails>)serializer.Deserialize(reader);
        
        // Validate and load
        if (ValidateImportedData(importedTasks))
        {
            TaskCollection = importedTasks;
            ganttControl.ItemsSource = TaskCollection;
            MessageBox.Show("Project imported successfully.");
        }
    }
}

private bool ValidateImportedData(ObservableCollection<TaskDetails> tasks)
{
    // Validate task IDs are unique
    var uniqueIds = new HashSet<int>();
    
    foreach (var task in tasks)
    {
        if (uniqueIds.Contains(task.TaskId))
            return false;
        uniqueIds.Add(task.TaskId);
        
        // Validate date ranges
        if (task.StartDate > task.FinishDate)
            return false;
    }
    
    return true;
}
```

### Export to Microsoft Project XML

```csharp
private void ExportToMSProjectXml(string filePath)
{
    // Create MS Project compatible XML
    var xmlDoc = new XmlDocument();
    var projectNode = xmlDoc.CreateElement("Project");
    
    foreach (var task in TaskCollection)
    {
        var taskNode = xmlDoc.CreateElement("Task");
        
        var idNode = xmlDoc.CreateElement("UID");
        idNode.InnerText = task.TaskId.ToString();
        taskNode.AppendChild(idNode);
        
        var nameNode = xmlDoc.CreateElement("Name");
        nameNode.InnerText = task.TaskName;
        taskNode.AppendChild(nameNode);
        
        var startNode = xmlDoc.CreateElement("Start");
        startNode.InnerText = task.StartDate.ToString("yyyy-MM-ddTHH:mm:ss");
        taskNode.AppendChild(startNode);
        
        var finishNode = xmlDoc.CreateElement("Finish");
        finishNode.InnerText = task.FinishDate.ToString("yyyy-MM-ddTHH:mm:ss");
        taskNode.AppendChild(finishNode);
        
        var percentCompleteNode = xmlDoc.CreateElement("PercentComplete");
        percentCompleteNode.InnerText = task.Progress.ToString();
        taskNode.AppendChild(percentCompleteNode);
        
        projectNode.AppendChild(taskNode);
    }
    
    xmlDoc.AppendChild(projectNode);
    xmlDoc.Save(filePath);
}
```

## Bulk Operations

Perform operations on multiple tasks efficiently.

### Bulk Update

```csharp
private void BulkUpdateProgress(IEnumerable<TaskDetails> tasks, double newProgress)
{
    // Suspend notifications for performance
    var collection = TaskCollection as INotifyCollectionChanged;
    
    foreach (var task in tasks)
    {
        task.Progress = newProgress;
    }
    
    // Refresh once
    ganttControl.RefreshGantt();
}
```

### Bulk Delete

```csharp
private void DeleteTasks(IEnumerable<TaskDetails> tasksToDelete)
{
    var taskList = tasksToDelete.ToList();
    
    foreach (var task in taskList)
    {
        // Remove dependencies first
        RemoveTaskDependencies(task);
        
        // Remove from parent
        if (task.ParentTask != null)
        {
            task.ParentTask.Child.Remove(task);
        }
        else
        {
            TaskCollection.Remove(task);
        }
    }
    
    ganttControl.RefreshGantt();
}

private void RemoveTaskDependencies(TaskDetails task)
{
    // Remove all dependencies involving this task
    foreach (var t in TaskCollection)
    {
        if (t.Predecessor != null)
        {
            var deps = t.Predecessor.Where(p => p.GanttTaskIndex != task.TaskId).ToList();
            t.Predecessor = new ObservableCollection<Predecessor>(deps);
        }
    }
}
```

### Search Tasks

```csharp
private IEnumerable<TaskDetails> SearchTasks(string searchText)
{
    return TaskCollection.Where(task =>
        task.TaskName.Contains(searchText, StringComparison.OrdinalIgnoreCase) ||
        (task.Notes != null && task.Notes.Contains(searchText, StringComparison.OrdinalIgnoreCase))
    );
}

private void HighlightSearchResults(string searchText)
{
    var results = SearchTasks(searchText);
    
    foreach (var task in results)
    {
        // Apply highlight style
        task.NodeStyle = new Style(typeof(GanttNode))
        {
            Setters = 
            {
                new Setter(GanttNode.BackgroundProperty, Brushes.Yellow)
            }
        };
    }
    
    ganttControl.RefreshGantt();
}
```

## Best Practices

1. **Enable Virtualization:** Always use virtualization for projects with 100+ tasks.

2. **Filter Before Sort:** Apply filters first to reduce the dataset before sorting.

3. **Batch Updates:** Use bulk operations for updating multiple tasks to improve performance.

4. **Validate Imports:** Always validate XML data before importing to avoid corrupt project states.

5. **Deferred Loading:** Load child tasks on-demand for large hierarchical structures.

6. **Refresh Once:** After bulk operations, call `RefreshGantt()` only once at the end.

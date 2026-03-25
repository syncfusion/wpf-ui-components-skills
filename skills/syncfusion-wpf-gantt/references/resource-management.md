# Resource Management

Manage resources, resource assignments, and resource-based views in the WPF Gantt control.

## Resource Assignment

Assign resources to tasks and track allocations.

### Define Resources

```csharp
public class Resource : INotifyPropertyChanged
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Role { get; set; }
    public double CostPerHour { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
}

// Create resource collection
public ObservableCollection<Resource> Resources { get; set; } = new ObservableCollection<Resource>
{
    new Resource { Id = 1, Name = "John Smith", Role = "Developer", CostPerHour = 50 },
    new Resource { Id = 2, Name = "Jane Doe", Role = "Designer", CostPerHour = 45 },
    new Resource { Id = 3, Name = "Bob Johnson", Role = "Tester", CostPerHour = 40 }
};
```

### Assign Resources to Tasks

```csharp
// Single resource assignment
var task = new TaskDetails
{
    TaskId = 1,
    TaskName = "Design UI",
    StartDate = DateTime.Today,
    FinishDate = DateTime.Today.AddDays(5),
    Resources = new ObservableCollection<Resource>
    {
        Resources.First(r => r.Name == "Jane Doe")
    }
};

// Multiple resource assignment
var developmentTask = new TaskDetails
{
    TaskId = 2,
    TaskName = "Implement Features",
    StartDate = DateTime.Today,
    FinishDate = DateTime.Today.AddDays(10),
    Resources = new ObservableCollection<Resource>
    {
        Resources.First(r => r.Name == "John Smith"),
        Resources.First(r => r.Name == "Bob Johnson")
    }
};
```

### Resource Mapping

```csharp
ganttControl.TaskAttributeMapping = new TaskAttributeMapping
{
    TaskIdMapping = "TaskId",
    TaskNameMapping = "TaskName",
    StartDateMapping = "StartDate",
    ChildMapping = "Child",
    FinishDateMapping = "FinishDate",
    ProgressMapping = "Progress",
    ResourceInfoMapping = "Resources"  // Map resource collection
};
```

## Resource View

Display tasks organized by resources.

### Enable Resource View

```csharp
// Switch to resource view
ganttControl.UseOnDemandSchedule = true;
ganttControl.ResourceCollection = Resources;

// Configure resource view
ganttControl.ShowResourceView = true;
ganttControl.ResourceViewType = ResourceViewType.GroupByResource;
```

### Gantt Inline Items (Resource View)

```csharp
public class ResourceViewModel
{
    public ObservableCollection<Resource> Resources { get; set; }
    public ObservableCollection<TaskDetails> Tasks { get; set; }
    
    public ResourceViewModel()
    {
        Resources = LoadResources();
        Tasks = LoadTasks();
        
        // Group tasks by resource
        var resourceTasks = Tasks
            .SelectMany(t => t.Resources, (task, resource) => new { Task = task, Resource = resource })
            .GroupBy(x => x.Resource.Id);
        
        // Create inline items for each resource
        foreach (var group in resourceTasks)
        {
            var resource = Resources.First(r => r.Id == group.Key);
            resource.AssignedTasks = new ObservableCollection<TaskDetails>(
                group.Select(x => x.Task)
            );
        }
    }
}
```

### Display Resources in Grid

```xml
<syncfusion:GanttControl.TaskAttributeMapping>
    <syncfusion:TaskAttributeMapping
        ResourceInfoMapping="Resources">
    </syncfusion:TaskAttributeMapping>
</syncfusion:GanttControl.TaskAttributeMapping>

<syncfusion:GanttControl.InternalGrid>
    <syncfusion:GanttGrid>
        <syncfusion:GanttGrid.Columns>
            <syncfusion:GanttColumn HeaderText="Task Name" MappingName="TaskName" Width="200"/>
            <syncfusion:GanttColumn HeaderText="Resources" MappingName="Resources" Width="150">
                <syncfusion:GanttColumn.CellTemplate>
                    <DataTemplate>
                        <ItemsControl ItemsSource="{Binding Resources}">
                            <ItemsControl.ItemTemplate>
                                <DataTemplate>
                                    <TextBlock Text="{Binding Name}" Margin="0,0,5,0"/>
                                </DataTemplate>
                            </ItemsControl.ItemTemplate>
                        </ItemsControl>
                    </DataTemplate>
                </syncfusion:GanttColumn.CellTemplate>
            </syncfusion:GanttColumn>
        </syncfusion:GanttGrid.Columns>
    </syncfusion:GanttGrid>
</syncfusion:GanttControl.InternalGrid>
```

## Resource Allocation Tracking

Track resource utilization and workload.

### Calculate Resource Allocation

```csharp
public class ResourceAllocation
{
    public Resource Resource { get; set; }
    public DateTime Date { get; set; }
    public double HoursAllocated { get; set; }
    public double PercentUtilization { get; set; }
}

private List<ResourceAllocation> CalculateResourceAllocations()
{
    var allocations = new List<ResourceAllocation>();
    
    foreach (var resource in Resources)
    {
        // Get all tasks assigned to this resource
        var assignedTasks = Tasks.Where(t => 
            t.Resources != null && t.Resources.Any(r => r.Id == resource.Id)
        );
        
        // Calculate by date
        var dateRange = GetDateRange(Tasks);
        
        foreach (var date in dateRange)
        {
            var tasksOnDate = assignedTasks.Where(t => 
                t.StartDate <= date && t.FinishDate >= date
            );
            
            double hoursAllocated = tasksOnDate.Sum(t => 8.0 / t.Duration.Days); // Assume 8-hour workday
            
            allocations.Add(new ResourceAllocation
            {
                Resource = resource,
                Date = date,
                HoursAllocated = hoursAllocated,
                PercentUtilization = (hoursAllocated / 8.0) * 100
            });
        }
    }
    
    return allocations;
}
```

### Identify Overallocated Resources

```csharp
private List<Resource> GetOverallocatedResources()
{
    var allocations = CalculateResourceAllocations();
    
    // Find resources with >100% utilization on any date
    var overallocated = allocations
        .Where(a => a.PercentUtilization > 100)
        .Select(a => a.Resource)
        .Distinct()
        .ToList();
    
    return overallocated;
}

private void HighlightOverallocatedResources()
{
    var overallocated = GetOverallocatedResources();
    
    foreach (var task in Tasks)
    {
        if (task.Resources != null && 
            task.Resources.Any(r => overallocated.Any(o => o.Id == r.Id)))
        {
            // Highlight task with overallocated resource
            task.NodeStyle = new Style(typeof(GanttNode))
            {
                Setters = 
                {
                    new Setter(GanttNode.BackgroundProperty, Brushes.Orange)
                }
            };
        }
    }
    
    ganttControl.RefreshGantt();
}
```

## Multi-Resource Tasks

Handle tasks requiring multiple resources simultaneously.

### Define Multi-Resource Task

```csharp
var multiResourceTask = new TaskDetails
{
    TaskId = 10,
    TaskName = "Code Review Meeting",
    StartDate = DateTime.Today.AddDays(5),
    FinishDate = DateTime.Today.AddDays(5),
    Duration = new TimeSpan(2, 0, 0), // 2 hours
    Resources = new ObservableCollection<Resource>
    {
        Resources.First(r => r.Name == "John Smith"),
        Resources.First(r => r.Name == "Jane Doe"),
        Resources.First(r => r.Name == "Bob Johnson")
    }
};
```

### Resource Effort Distribution

```csharp
public class ResourceAssignment
{
    public Resource Resource { get; set; }
    public double AllocationPercent { get; set; } // 0-100
    public double Hours { get; set; }
}

public class TaskDetails
{
    public ObservableCollection<ResourceAssignment> ResourceAssignments { get; set; }
    
    // Calculate total effort
    public double TotalEffortHours =>
        ResourceAssignments?.Sum(ra => ra.Hours) ?? 0;
}

// Example: Task requiring partial allocation
var task = new TaskDetails
{
    TaskName = "Review Documentation",
    ResourceAssignments = new ObservableCollection<ResourceAssignment>
    {
        new ResourceAssignment 
        { 
            Resource = Resources[0], 
            AllocationPercent = 50,  // Part-time allocation
            Hours = 20 
        },
        new ResourceAssignment 
        { 
            Resource = Resources[1], 
            AllocationPercent = 25,  // Quarter-time allocation
            Hours = 10 
        }
    }
};
```

## Resource Cost Calculation

Calculate costs based on resource assignments.

```csharp
private double CalculateTaskCost(TaskDetails task)
{
    if (task.Resources == null || !task.Resources.Any())
        return 0;
    
    double totalCost = 0;
    double taskDurationDays = (task.FinishDate - task.StartDate).Days;
    double hoursPerDay = 8.0;
    
    foreach (var resource in task.Resources)
    {
        double hours = taskDurationDays * hoursPerDay;
        totalCost += hours * resource.CostPerHour;
    }
    
    return totalCost;
}

private double CalculateProjectCost()
{
    return Tasks.Sum(task => CalculateTaskCost(task));
}
```

## Complete Resource Management Example

```csharp
public class ResourceManagementViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Resource> Resources { get; set; }
    public ObservableCollection<TaskDetails> Tasks { get; set; }
    
    public ResourceManagementViewModel()
    {
        InitializeResources();
        InitializeTasks();
    }
    
    private void InitializeResources()
    {
        Resources = new ObservableCollection<Resource>
        {
            new Resource { Id = 1, Name = "John Smith", Role = "Developer", CostPerHour = 50 },
            new Resource { Id = 2, Name = "Jane Doe", Role = "Designer", CostPerHour = 45 },
            new Resource { Id = 3, Name = "Bob Johnson", Role = "Tester", CostPerHour = 40 }
        };
    }
    
    private void InitializeTasks()
    {
        Tasks = new ObservableCollection<TaskDetails>
        {
            new TaskDetails
            {
                TaskId = 1,
                TaskName = "Project Planning",
                StartDate = new DateTime(2024, 1, 1),
                FinishDate = new DateTime(2024, 1, 5),
                Resources = new ObservableCollection<Resource> { Resources[0], Resources[1] }
            },
            new TaskDetails
            {
                TaskId = 2,
                TaskName = "Design Phase",
                StartDate = new DateTime(2024, 1, 8),
                FinishDate = new DateTime(2024, 1, 19),
                Resources = new ObservableCollection<Resource> { Resources[1] }
            },
            new TaskDetails
            {
                TaskId = 3,
                TaskName = "Development",
                StartDate = new DateTime(2024, 1, 22),
                FinishDate = new DateTime(2024, 2, 16),
                Resources = new ObservableCollection<Resource> { Resources[0] }
            },
            new TaskDetails
            {
                TaskId = 4,
                TaskName = "Testing",
                StartDate = new DateTime(2024, 2, 19),
                FinishDate = new DateTime(2024, 3, 1),
                Resources = new ObservableCollection<Resource> { Resources[2] }
            }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

## Best Practices

1. **Track Allocations:** Monitor resource utilization to avoid overallocation.

2. **Define Roles:** Use resource roles to ensure appropriate skill matching.

3. **Calculate Costs:** Track resource costs for budget management.

4. **Multi-Resource Tasks:** Use for meetings, reviews, and collaborative work.

5. **Resource Views:** Switch to resource view for workload balancing.

# Data Binding in WPF Gantt Control

## Table of Contents
- [Overview](#overview)
- [TaskDetails Binding](#taskdetails-binding)
- [ViewModel Pattern](#viewmodel-pattern)
- [Task Properties](#task-properties)
- [Creating Task Hierarchies](#creating-task-hierarchies)
- [External Property Binding](#external-property-binding)
- [Task Attribute Mapping](#task-attribute-mapping)
- [Resource Assignment](#resource-assignment)
- [Best Practices](#best-practices)

## Overview

Data binding is the foundation of working with the WPF Gantt control. The control supports two primary binding approaches:

1. **TaskDetails Binding** - Using the built-in `TaskDetails` class that implements `IGanttTask`
2. **External Property Binding** - Binding custom classes with `TaskAttributeMapping`

Both approaches follow MVVM patterns and support `ObservableCollection` for dynamic updates.

## TaskDetails Binding

The simplest approach uses the built-in `TaskDetails` class, which inherits from the `IGanttTask` interface and provides all necessary properties for Gantt visualization.

### Basic TaskDetails Binding

**XAML:**

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ItemsSource="{Binding TaskDetails}">
    <syncfusion:GanttControl.DataContext>
        <local:ViewModel/>
    </syncfusion:GanttControl.DataContext>
</syncfusion:GanttControl>
```

**C#:**

```csharp
ganttControl.ItemsSource = new ViewModel().TaskDetails;
```

### TaskDetails with Attribute Mapping

For more control over property mapping:

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ItemsSource="{Binding TaskDetails}">
    <syncfusion:GanttControl.TaskAttributeMapping>
        <syncfusion:TaskAttributeMapping 
            TaskIdMapping="TaskId"
            TaskNameMapping="TaskName"
            StartDateMapping="StartDate"
            ChildMapping="Child"
            FinishDateMapping="FinishDate"
            DurationMapping="Duration"
            ProgressMapping="Progress"/>
    </syncfusion:GanttControl.TaskAttributeMapping>
    <syncfusion:GanttControl.DataContext>
        <local:ViewModel/>
    </syncfusion:GanttControl.DataContext>
</syncfusion:GanttControl>
```

**C# Code:**

```csharp
TaskAttributeMapping taskAttributeMapping = new TaskAttributeMapping();
taskAttributeMapping.TaskIdMapping = "TaskId";
taskAttributeMapping.TaskNameMapping = "TaskName";
taskAttributeMapping.StartDateMapping = "StartDate";
taskAttributeMapping.ChildMapping = "Child";
taskAttributeMapping.FinishDateMapping = "FinishDate";
taskAttributeMapping.DurationMapping = "Duration";
taskAttributeMapping.ProgressMapping = "Progress";

ganttControl.TaskAttributeMapping = taskAttributeMapping;
ganttControl.ItemsSource = new ViewModel().TaskDetails;
```

## ViewModel Pattern

Implement the ViewModel to manage your task collection with `INotifyPropertyChanged` support:

```csharp
using System;
using System.Collections.ObjectModel;
using System.ComponentModel;
using Syncfusion.Windows.Controls.Gantt;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<TaskDetails> taskDetails;

    public ViewModel()
    {
        this.taskDetails = this.GetTaskDetails();
    }

    /// <summary>
    /// Gets or sets the task collection.
    /// </summary>
    public ObservableCollection<TaskDetails> TaskDetails
    {
        get { return this.taskDetails; }
        set
        {
            this.taskDetails = value;
            OnPropertyChanged("TaskDetails");
        }
    }

    /// <summary>
    /// Creates and returns the task details collection
    /// </summary>
    private ObservableCollection<TaskDetails> GetTaskDetails()
    {
        ObservableCollection<TaskDetails> tasks = new ObservableCollection<TaskDetails>();
        
        // Create parent task
        var scopeTask = new TaskDetails 
        { 
            TaskId = 1, 
            TaskName = "Scope", 
            StartDate = new DateTime(2024, 1, 3), 
            FinishDate = new DateTime(2024, 1, 14), 
            Progress = 40d 
        };
        
        // Add child tasks
        scopeTask.Child.Add(new TaskDetails 
        { 
            TaskId = 2, 
            TaskName = "Determine project scope", 
            StartDate = new DateTime(2024, 1, 3), 
            FinishDate = new DateTime(2024, 1, 5), 
            Progress = 20d 
        });
        
        scopeTask.Child.Add(new TaskDetails 
        { 
            TaskId = 3, 
            TaskName = "Justify project via business model", 
            StartDate = new DateTime(2024, 1, 3), 
            FinishDate = new DateTime(2024, 1, 7), 
            Progress = 20d 
        });
        
        scopeTask.Child.Add(new TaskDetails 
        { 
            TaskId = 4, 
            TaskName = "Secure executive sponsorship", 
            StartDate = new DateTime(2024, 1, 3), 
            FinishDate = new DateTime(2024, 1, 14), 
            Progress = 20d 
        });
        
        tasks.Add(scopeTask);
        
        return tasks;
    }

    public event PropertyChangedEventHandler PropertyChanged;
    
    private void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Task Properties

### Core TaskDetails Properties

| Property | Type | Description |
|----------|------|-------------|
| `TaskId` | int | Unique identifier for the task |
| `TaskName` | string | Display name of the task |
| `StartDate` | DateTime | Task start date and time |
| `FinishDate` | DateTime | Task finish date and time |
| `Duration` | TimeSpan | Duration between start and finish |
| `Progress` | double | Completion percentage (0-100) |
| `Child` | ObservableCollection<TaskDetails> | Collection of child tasks |
| `Predecessor` | ObservableCollection<Predecessor> | Task dependencies |
| `Resources` | ObservableCollection<Resource> | Assigned resources |

### Complete Task Example

```csharp
var task = new TaskDetails
{
    TaskId = 10,
    TaskName = "Development Phase",
    StartDate = new DateTime(2024, 2, 1),
    FinishDate = new DateTime(2024, 2, 20),
    Duration = TimeSpan.FromDays(20),
    Progress = 35.5,
    
    // Optional properties
    BaselineStart = new DateTime(2024, 2, 1),
    BaselineFinish = new DateTime(2024, 2, 18), // Original plan
    IsMilestone = false
};
```

## Creating Task Hierarchies

Task hierarchies use parent-child relationships through the `Child` collection:

```csharp
private ObservableCollection<TaskDetails> CreateProjectStructure()
{
    var tasks = new ObservableCollection<TaskDetails>();
    
    // Root task (parent)
    var projectPhase = new TaskDetails
    {
        TaskId = 1,
        TaskName = "Project Implementation",
        StartDate = new DateTime(2024, 1, 1),
        FinishDate = new DateTime(2024, 3, 31),
        Progress = 40d
    };
    
    // Level 1 children
    var designPhase = new TaskDetails
    {
        TaskId = 2,
        TaskName = "Design Phase",
        StartDate = new DateTime(2024, 1, 1),
        FinishDate = new DateTime(2024, 1, 31),
        Progress = 80d
    };
    
    // Level 2 children (grandchildren)
    designPhase.Child.Add(new TaskDetails
    {
        TaskId = 3,
        TaskName = "UI Design",
        StartDate = new DateTime(2024, 1, 1),
        FinishDate = new DateTime(2024, 1, 15),
        Progress = 100d
    });
    
    designPhase.Child.Add(new TaskDetails
    {
        TaskId = 4,
        TaskName = "Database Design",
        StartDate = new DateTime(2024, 1, 16),
        FinishDate = new DateTime(2024, 1, 31),
        Progress = 60d
    });
    
    // Add child to parent
    projectPhase.Child.Add(designPhase);
    
    // Add development phase
    var devPhase = new TaskDetails
    {
        TaskId = 5,
        TaskName = "Development Phase",
        StartDate = new DateTime(2024, 2, 1),
        FinishDate = new DateTime(2024, 3, 15),
        Progress = 30d
    };
    
    devPhase.Child.Add(new TaskDetails
    {
        TaskId = 6,
        TaskName = "Frontend Development",
        StartDate = new DateTime(2024, 2, 1),
        FinishDate = new DateTime(2024, 2, 28),
        Progress = 40d
    });
    
    devPhase.Child.Add(new TaskDetails
    {
        TaskId = 7,
        TaskName = "Backend Development",
        StartDate = new DateTime(2024, 2, 1),
        FinishDate = new DateTime(2024, 3, 15),
        Progress = 25d
    });
    
    projectPhase.Child.Add(devPhase);
    
    // Add root to collection
    tasks.Add(projectPhase);
    
    return tasks;
}
```

## External Property Binding

For custom data models, use `TaskAttributeMapping` to map your properties to Gantt requirements:

### Custom Task Class

```csharp
using System;
using System.Collections.ObjectModel;
using System.ComponentModel;

public class Task : INotifyPropertyChanged
{
    private DateTime startDate, endDate;
    private TimeSpan duration;
    private double progress;
    private int id;
    private string name;
    private ObservableCollection<Task> childCollection;
    private ObservableCollection<Resource> resource;
    private ObservableCollection<Predecessor> predecessor;

    public Task()
    {
        this.ChildCollection = new ObservableCollection<Task>();
        this.Predecessor = new ObservableCollection<Predecessor>();
        this.Resource = new ObservableCollection<Resource>();
    }

    public int ID
    {
        get { return id; }
        set { id = value; OnPropertyChanged("ID"); }
    }

    public string Name
    {
        get { return name; }
        set { name = value; OnPropertyChanged("Name"); }
    }

    public DateTime StartDate
    {
        get { return startDate; }
        set { startDate = value; OnPropertyChanged("StartDate"); }
    }

    public DateTime EndDate
    {
        get { return endDate; }
        set { endDate = value; OnPropertyChanged("EndDate"); }
    }

    public TimeSpan Duration
    {
        get { return duration; }
        set { duration = value; OnPropertyChanged("Duration"); }
    }

    public double Progress
    {
        get { return progress; }
        set { progress = value; OnPropertyChanged("Progress"); }
    }

    public ObservableCollection<Task> ChildCollection
    {
        get { return childCollection; }
        set { childCollection = value; OnPropertyChanged("ChildCollection"); }
    }

    public ObservableCollection<Resource> Resource
    {
        get { return resource; }
        set { resource = value; OnPropertyChanged("Resource"); }
    }

    public ObservableCollection<Predecessor> Predecessor
    {
        get { return predecessor; }
        set { predecessor = value; OnPropertyChanged("Predecessor"); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    
    private void OnPropertyChanged(string propName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
    }
}
```

### Binding Custom Class

**XAML:**

```xml
<syncfusion:GanttControl x:Name="ganttControl" 
                         ItemsSource="{Binding TaskCollection}">
    <syncfusion:GanttControl.TaskAttributeMapping>
        <syncfusion:TaskAttributeMapping 
            TaskIdMapping="ID"
            TaskNameMapping="Name"
            StartDateMapping="StartDate"
            ChildMapping="ChildCollection"
            FinishDateMapping="EndDate"
            DurationMapping="Duration"
            ProgressMapping="Progress"
            PredecessorMapping="Predecessor"
            ResourceInfoMapping="Resource"/>
    </syncfusion:GanttControl.TaskAttributeMapping>
    <syncfusion:GanttControl.DataContext>
        <local:ViewModel/>
    </syncfusion:GanttControl.DataContext>
</syncfusion:GanttControl>
```

**C#:**

```csharp
TaskAttributeMapping mapping = new TaskAttributeMapping
{
    TaskIdMapping = "ID",
    TaskNameMapping = "Name",
    StartDateMapping = "StartDate",
    ChildMapping = "ChildCollection",
    FinishDateMapping = "EndDate",
    DurationMapping = "Duration",
    ProgressMapping = "Progress",
    PredecessorMapping = "Predecessor",
    ResourceInfoMapping = "Resource"
};

ganttControl.TaskAttributeMapping = mapping;
ganttControl.ItemsSource = new ViewModel().TaskCollection;
```

## Task Attribute Mapping

### Available Mapping Properties

| Mapping Property | Target Property | Description |
|-----------------|----------------|-------------|
| `TaskIdMapping` | Task ID | Unique identifier property name |
| `TaskNameMapping` | Task Name | Task name property name |
| `StartDateMapping` | Start Date | Start date property name |
| `FinishDateMapping` | Finish Date | End date property name |
| `DurationMapping` | Duration | Duration property name |
| `ProgressMapping` | Progress | Progress percentage property name |
| `ChildMapping` | Child Collection | Child tasks collection property name |
| `PredecessorMapping` | Predecessors | Dependencies property name |
| `ResourceInfoMapping` | Resources | Resources property name |
| `BaselineStartMapping` | Baseline Start | Baseline start date property name |
| `BaselineFinishMapping` | Baseline Finish | Baseline finish date property name |

## Resource Assignment

Assign resources (team members, equipment, etc.) to tasks:

```csharp
// Define resources
var resources = new ObservableCollection<Resource>
{
    new Resource { ID = 1, Name = "John Smith" },
    new Resource { ID = 2, Name = "Jane Doe" },
    new Resource { ID = 3, Name = "Peter Johnson" }
};

// Assign to single task
var task1 = new TaskDetails
{
    TaskId = 1,
    TaskName = "Requirements Analysis",
    StartDate = new DateTime(2024, 1, 1),
    FinishDate = new DateTime(2024, 1, 10),
    Resources = new ObservableCollection<Resource> 
    { 
        new Resource { ID = 1, Name = "John Smith" },
        new Resource { ID = 2, Name = "Jane Doe" }
    }
};

// Multi-resource assignment
var task2 = new TaskDetails
{
    TaskId = 2,
    TaskName = "Implementation",
    StartDate = new DateTime(2024, 1, 11),
    FinishDate = new DateTime(2024, 2, 15),
    Resources = resources // All three resources
};
```

### Resource Assignment in Hierarchies

```csharp
var projectTasks = new ObservableCollection<TaskDetails>();

var parentTask = new TaskDetails
{
    TaskId = 1,
    TaskName = "Development",
    StartDate = new DateTime(2024, 1, 1),
    FinishDate = new DateTime(2024, 3, 31),
    Resources = new ObservableCollection<Resource> 
    { 
        new Resource { ID = 1, Name = "Team Lead" } 
    }
};

// Child with different resources
parentTask.Child.Add(new TaskDetails
{
    TaskId = 2,
    TaskName = "Frontend",
    StartDate = new DateTime(2024, 1, 1),
    FinishDate = new DateTime(2024, 2, 15),
    Resources = new ObservableCollection<Resource>
    {
        new Resource { ID = 2, Name = "Frontend Developer 1" },
        new Resource { ID = 3, Name = "Frontend Developer 2" }
    }
});

projectTasks.Add(parentTask);
```

## Best Practices

### 1. Use ObservableCollection

Always use `ObservableCollection<T>` for automatic UI updates:

```csharp
// ✅ Good - Observable
public ObservableCollection<TaskDetails> TaskDetails { get; set; }

// ❌ Bad - Not observable
public List<TaskDetails> TaskDetails { get; set; }
```

### 2. Implement INotifyPropertyChanged

For custom classes, implement property change notifications:

```csharp
public class Task : INotifyPropertyChanged
{
    private string name;
    
    public string Name
    {
        get { return name; }
        set 
        { 
            name = value;
            OnPropertyChanged("Name"); // Notify UI
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    private void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### 3. Set TaskId Consistently

Ensure unique TaskId values for proper dependency tracking:

```csharp
// ✅ Good - Unique IDs
var task1 = new TaskDetails { TaskId = 1, TaskName = "Task 1" };
var task2 = new TaskDetails { TaskId = 2, TaskName = "Task 2" };

// ❌ Bad - Duplicate IDs
var task1 = new TaskDetails { TaskId = 1, TaskName = "Task 1" };
var task2 = new TaskDetails { TaskId = 1, TaskName = "Task 2" }; // Same ID!
```

### 4. Initialize Collections

Initialize child collections in constructors:

```csharp
public Task()
{
    // ✅ Initialize collections
    this.ChildCollection = new ObservableCollection<Task>();
    this.Resources = new ObservableCollection<Resource>();
    this.Predecessor = new ObservableCollection<Predecessor>();
}
```

### 5. Validate Date Ranges

Ensure StartDate is before or equal to FinishDate:

```csharp
public DateTime FinishDate
{
    get { return finishDate; }
    set
    {
        if (value >= StartDate) // Validation
        {
            finishDate = value;
            OnPropertyChanged("FinishDate");
        }
    }
}
```

### 6. Use Appropriate Schedule Types

Match your data granularity with schedule type:

```csharp
// For daily tracking
ganttControl.ScheduleType = ScheduleType.WeekWithDays;

// For hourly tracking
ganttControl.ScheduleType = ScheduleType.DayWithHours;

// For long-term projects
ganttControl.ScheduleType = ScheduleType.YearWithMonths;
```

## Common Issues and Solutions

### Issue: Tasks Not Displaying

**Problem:** ItemsSource is bound but no tasks appear.

**Solutions:**
- Verify TaskDetails collection is not null or empty
- Check TaskAttributeMapping if using custom class
- Ensure StartDate and FinishDate are set
- Verify DataContext is set correctly

### Issue: Hierarchy Not Showing

**Problem:** Parent-child relationships not displaying.

**Solutions:**
- Check ChildMapping is set correctly in TaskAttributeMapping
- Verify Child collection is initialized
- Ensure ChildCollection is ObservableCollection, not List

### Issue: Changes Not Reflecting

**Problem:** Updating task properties doesn't update UI.

**Solutions:**
- Use ObservableCollection instead of List
- Implement INotifyPropertyChanged in custom classes
- Call OnPropertyChanged when properties change

### Issue: Resources Not Appearing

**Problem:** Resources assigned but not visible in Gantt.

**Solutions:**
- Verify ResourceInfoMapping is set in TaskAttributeMapping
- Check Resource collection is initialized
- Ensure Resource class has ID and Name properties

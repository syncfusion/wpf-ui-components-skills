# Task Relationships and Dependencies

## Table of Contents
- [Overview](#overview)
- [Dependency Types](#dependency-types)
- [Creating Dependencies](#creating-dependencies)
- [Auto-Update Hierarchy](#auto-update-hierarchy)
- [Baseline Support](#baseline-support)
- [Milestones](#milestones)

## Overview

Task dependencies define relationships between tasks, determining the order in which tasks must be completed. The Gantt control supports four relationship types and automatically updates parent tasks when child tasks change.

## Dependency Types

### 1. Finish-to-Start (FS)

Cannot start the successor task until the predecessor task finishes.

**Most common dependency type.** Task B cannot begin until Task A completes.

```
Task A: ══════════╗
Task B:            ╚══════════
```

**Example:**
```csharp
task2.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = 1, // Task 1 must finish first
    GanttTaskRelationship = GanttTaskRelationship.FinishToStart 
});
```

### 2. Finish-to-Finish (FF)

Cannot finish the successor task until the predecessor task finishes.

**Both tasks finish together.** Task B cannot complete until Task A completes.

```
Task A: ══════════╗
Task B: ══════════╝
```

**Example:**
```csharp
task2.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = 1,
    GanttTaskRelationship = GanttTaskRelationship.FinishToFinish 
});
```

### 3. Start-to-Start (SS)

Cannot start the successor task until the predecessor task starts.

**Both tasks start together.** Task B cannot begin until Task A begins.

```
Task A: ╔══════════
Task B: ╚══════════
```

**Example:**
```csharp
task2.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = 1,
    GanttTaskRelationship = GanttTaskRelationship.StartToStart 
});
```

### 4. Start-to-Finish (SF)

Cannot finish the successor task until the predecessor task starts.

**Rare dependency.** Task B cannot finish until Task A starts.

```
Task A:     ╔══════════
Task B: ════╝
```

**Example:**
```csharp
task2.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = 1,
    GanttTaskRelationship = GanttTaskRelationship.StartToFinish 
});
```

## Creating Dependencies

### XAML Configuration

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ItemsSource="{Binding TaskCollection}">
    <syncfusion:GanttControl.TaskAttributeMapping>
        <syncfusion:TaskAttributeMapping 
            TaskIdMapping="TaskId"
            TaskNameMapping="TaskName"
            StartDateMapping="StartDate"
            FinishDateMapping="FinishDate"
            PredecessorMapping="Predecessor"/>
    </syncfusion:GanttControl.TaskAttributeMapping>
</syncfusion:GanttControl>
```

### Code-Behind Implementation

```csharp
// Define tasks
var task1 = new TaskDetails
{
    TaskId = 1,
    TaskName = "Requirements Analysis",
    StartDate = new DateTime(2024, 1, 1),
    FinishDate = new DateTime(2024, 1, 10)
};

var task2 = new TaskDetails
{
    TaskId = 2,
    TaskName = "Design",
    StartDate = new DateTime(2024, 1, 11),
    FinishDate = new DateTime(2024, 1, 25)
};

var task3 = new TaskDetails
{
    TaskId = 3,
    TaskName = "Development",
    StartDate = new DateTime(2024, 1, 26),
    FinishDate = new DateTime(2024, 3, 15)
};

var task4 = new TaskDetails
{
    TaskId = 4,
    TaskName = "Testing",
    StartDate = new DateTime(2024, 3, 1),
    FinishDate = new DateTime(2024, 3, 20)
};

// Task 2 starts after Task 1 finishes
task2.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = 1, 
    GanttTaskRelationship = GanttTaskRelationship.FinishToStart 
});

// Task 3 starts after Task 2 finishes
task3.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = 2, 
    GanttTaskRelationship = GanttTaskRelationship.FinishToStart 
});

// Task 4 (testing) runs parallel with development (starts together)
task4.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = 3, 
    GanttTaskRelationship = GanttTaskRelationship.StartToStart 
});

TaskCollection.Add(task1);
TaskCollection.Add(task2);
TaskCollection.Add(task3);
TaskCollection.Add(task4);
```

### Multiple Dependencies

A task can have multiple predecessors:

```csharp
var integrationTask = new TaskDetails
{
    TaskId = 5,
    TaskName = "Integration",
    StartDate = new DateTime(2024, 3, 16),
    FinishDate = new DateTime(2024, 3, 30)
};

// Integration depends on both development AND testing completing
integrationTask.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = 3, // Development
    GanttTaskRelationship = GanttTaskRelationship.FinishToStart 
});

integrationTask.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = 4, // Testing
    GanttTaskRelationship = GanttTaskRelationship.FinishToStart 
});
```

## Auto-Update Hierarchy

The Gantt control automatically updates parent tasks when child tasks change. This feature is enabled by default.

### Enable/Disable Auto-Update

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         UseAutoUpdateHierarchy="True">
</syncfusion:GanttControl>
```

```csharp
// Enable (default)
ganttControl.UseAutoUpdateHierarchy = true;

// Disable (manual updates required)
ganttControl.UseAutoUpdateHierarchy = false;
```

### How Auto-Update Works

When enabled, parent tasks automatically reflect:
- **Start Date:** Earliest start date of all child tasks
- **Finish Date:** Latest finish date of all child tasks
- **Duration:** Calculated from start to finish
- **Progress:** Weighted average of child task progress

### Example with Auto-Update

```csharp
var parentTask = new Task
{
    TaskId = 1,
    TaskName = "Development Phase",
    StartDate = new DateTime(2024, 1, 1),
    FinishDate = new DateTime(2024, 3, 31)
};

// Add child tasks
parentTask.ChildCollection.Add(new Task
{
    TaskId = 2,
    TaskName = "Backend Development",
    StartDate = new DateTime(2024, 1, 1),
    FinishDate = new DateTime(2024, 2, 15),
    Progress = 80
});

parentTask.ChildCollection.Add(new Task
{
    TaskId = 3,
    TaskName = "Frontend Development",
    StartDate = new DateTime(2024, 1, 15),
    FinishDate = new DateTime(2024, 3, 31),
    Progress = 50
});

// Parent task automatically updates:
// - Start: 2024-01-01 (earliest child start)
// - Finish: 2024-03-31 (latest child finish)
// - Progress: ~60% (weighted average)
```

## Baseline Support

Baselines capture the original project plan for comparison with actual progress.

### Baseline Properties

```csharp
var task = new TaskDetails
{
    TaskId = 1,
    TaskName = "Implementation",
    
    // Current (actual) values
    StartDate = new DateTime(2024, 1, 1),
    FinishDate = new DateTime(2024, 2, 10), // Delayed by 5 days
    Progress = 60,
    
    // Baseline (original plan)
    BaselineStart = new DateTime(2024, 1, 1),
    BaselineFinish = new DateTime(2024, 2, 5), // Original target
    BaselineCost = 50000,
    
    // Actual cost
    Cost = 55000 // Over budget
};
```

### Enable Baseline Display

```xml
<syncfusion:GanttControl x:Name="ganttControl"
                         ShowBaseline="True">
    <syncfusion:GanttControl.TaskAttributeMapping>
        <syncfusion:TaskAttributeMapping 
            StartDateMapping="StartDate"
            FinishDateMapping="FinishDate"
            BaselineStartMapping="BaselineStart"
            BaselineFinishMapping="BaselineFinish"
            CostMapping="Cost"
            BaselineCostMapping="BaselineCost"/>
    </syncfusion:GanttControl.TaskAttributeMapping>
</syncfusion:GanttControl>
```

```csharp
ganttControl.ShowBaseline = true;
```

### Variance View

Compare actual vs. baseline values:

```csharp
// Show variance view (differences between actual and baseline)
ganttControl.LoadVarianceTableView();

// Return to default editing view
ganttControl.LoadDefaultTableView();
```

**Variance Calculations:**
- **Start Variance:** Actual Start - Baseline Start
- **Finish Variance:** Actual Finish - Baseline Finish
- **Cost Variance:** Actual Cost - Baseline Cost

## Milestones

Milestones represent significant events or checkpoints with zero duration.

### Creating Milestones

```csharp
var milestone = new TaskDetails
{
    TaskId = 10,
    TaskName = "Project Kickoff",
    StartDate = new DateTime(2024, 1, 1),
    FinishDate = new DateTime(2024, 1, 1), // Same date = milestone
    IsMilestone = true
};

// Or set FinishDate = StartDate automatically
var deliveryMilestone = new TaskDetails
{
    TaskId = 20,
    TaskName = "Product Delivery",
    StartDate = new DateTime(2024, 6, 30),
    FinishDate = new DateTime(2024, 6, 30),
    IsMilestone = true
};
```

### Milestone Mapping

```xml
<syncfusion:GanttControl.TaskAttributeMapping>
    <syncfusion:TaskAttributeMapping 
        MileStoneMapping="IsMilestone"/>
</syncfusion:GanttControl.TaskAttributeMapping>
```

### Milestones in Dependencies

```csharp
var designTask = new TaskDetails
{
    TaskId = 1,
    TaskName = "Design Phase",
    StartDate = new DateTime(2024, 1, 1),
    FinishDate = new DateTime(2024, 1, 31)
};

var designCompleteMilestone = new TaskDetails
{
    TaskId = 2,
    TaskName = "Design Complete",
    StartDate = new DateTime(2024, 1, 31),
    FinishDate = new DateTime(2024, 1, 31),
    IsMilestone = true
};

// Milestone depends on design completion
designCompleteMilestone.Predecessor.Add(new Predecessor
{
    GanttTaskIndex = 1,
    GanttTaskRelationship = GanttTaskRelationship.FinishToStart
});

var developmentTask = new TaskDetails
{
    TaskId = 3,
    TaskName = "Development Phase",
    StartDate = new DateTime(2024, 2, 1),
    FinishDate = new DateTime(2024, 4, 30)
};

// Development starts after milestone
developmentTask.Predecessor.Add(new Predecessor
{
    GanttTaskIndex = 2,
    GanttTaskRelationship = GanttTaskRelationship.FinishToStart
});
```

## Common Patterns

### Pattern 1: Sequential Tasks (Waterfall)

```csharp
// Each phase depends on the previous phase completing
tasks[1].Predecessor.Add(new Predecessor { GanttTaskIndex = 0, GanttTaskRelationship = GanttTaskRelationship.FinishToStart });
tasks[2].Predecessor.Add(new Predecessor { GanttTaskIndex = 1, GanttTaskRelationship = GanttTaskRelationship.FinishToStart });
tasks[3].Predecessor.Add(new Predecessor { GanttTaskIndex = 2, GanttTaskRelationship = GanttTaskRelationship.FinishToStart });
```

### Pattern 2: Parallel Tasks with Convergence

```csharp
// Task 2 and 3 run in parallel after Task 1
tasks[1].Predecessor.Add(new Predecessor { GanttTaskIndex = 0, GanttTaskRelationship = GanttTaskRelationship.FinishToStart });
tasks[2].Predecessor.Add(new Predecessor { GanttTaskIndex = 0, GanttTaskRelationship = GanttTaskRelationship.FinishToStart });

// Task 4 waits for both Task 2 and 3
tasks[3].Predecessor.Add(new Predecessor { GanttTaskIndex = 1, GanttTaskRelationship = GanttTaskRelationship.FinishToStart });
tasks[3].Predecessor.Add(new Predecessor { GanttTaskIndex = 2, GanttTaskRelationship = GanttTaskRelationship.FinishToStart });
```

### Pattern 3: Overlapping Tasks

```csharp
// Testing starts when development is 50% complete (via Start-to-Start)
testingTask.Predecessor.Add(new Predecessor 
{ 
    GanttTaskIndex = developmentTaskIndex, 
    GanttTaskRelationship = GanttTaskRelationship.StartToStart 
});
```

## Best Practices

1. **Use Finish-to-Start for Most Dependencies:** It's the most intuitive and commonly used relationship type.

2. **Avoid Circular Dependencies:** Ensure Task A doesn't depend on Task B while Task B depends on Task A.

3. **Enable Auto-Update Hierarchy:** Keeps parent tasks synchronized automatically, reducing manual updates.

4. **Set Baselines Early:** Capture the original plan before work begins for accurate variance tracking.

5. **Use Milestones for Key Events:** Mark project phases, deliveries, and approval gates with milestones.

6. **Document Complex Dependencies:** Add comments or tooltips explaining why specific relationships exist.

## Troubleshooting

### Issue: Dependencies Not Displaying

**Solution:** Ensure PredecessorMapping is set in TaskAttributeMapping and Predecessor collection is initialized.

### Issue: Parent Tasks Not Updating

**Solution:** Verify `UseAutoUpdateHierarchy=true` and child tasks have valid Start/FinishDate values.

### Issue: Baseline Not Showing

**Solution:** Set `ShowBaseline=true` and map BaselineStartMapping and BaselineFinishMapping properties.

# Swim Lanes in WPF Kanban

Swim lanes are horizontal categorizations on the Kanban board used for grouping cards. They provide transparency and organization by categorizing workflow by projects, teams, users, or any custom criteria.

## Overview

**Purpose:** Group cards horizontally across all columns  
**Default Grouping:** By `Assignee` property in KanbanModel  
**Custom Grouping:** Use `SwimlaneKey` to group by any property

**Visual Structure:**
```
┌─ Swim Lane 1 (e.g., Team A) ──────────┐
│ [To Do] │ [In Progress] │ [Done]      │
│  Card1  │   Card2       │  Card3      │
└────────────────────────────────────────┘
┌─ Swim Lane 2 (e.g., Team B) ──────────┐
│ [To Do] │ [In Progress] │ [Done]      │
│  Card4  │   Card5       │  Card6      │
└────────────────────────────────────────┘
```

## Enabling Swim Lanes

### Default Swim Lane (by Assignee)

For `KanbanModel`, swim lanes automatically group by `Assignee` property:

```csharp
var tasks = new ObservableCollection<KanbanModel>
{
    new KanbanModel { Assignee = "Janet Leverling", /* ... */ },
    new KanbanModel { Assignee = "Andrew Fuller", /* ... */ },
    new KanbanModel { Assignee = "Janet Leverling", /* ... */ },
};
```

**Result:** Two swim lanes - one for "Janet Leverling", one for "Andrew Fuller"

### Custom Swim Lane Property

Group by any property using `SwimlaneKey`:

```xml
<kanban:SfKanban SwimlaneKey="ColorKey"
                 ItemsSource="{Binding TaskDetails}"/>
```

**C#:**
```csharp
kanban.SwimlaneKey = "ColorKey";
```

**Example Data:**
```csharp
new KanbanModel { ColorKey = "High", /* ... */ },
new KanbanModel { ColorKey = "Normal", /* ... */ },
new KanbanModel { ColorKey = "High", /* ... */ },
```

**Result:** Two swim lanes - "High" priority and "Normal" priority

## Complete Example

### XAML

```xml
<kanban:SfKanban x:Name="kanban"
                 SwimlaneKey="Assignee"
                 ItemsSource="{Binding TaskDetails}">
    <kanban:SfKanban.DataContext>
        <local:ViewModel/>
    </kanban:SfKanban.DataContext>
</kanban:SfKanban>
```

### ViewModel

```csharp
using Syncfusion.UI.Xaml.Kanban;

public class ViewModel
{
    public ObservableCollection<KanbanModel> TaskDetails { get; set; }

    public ViewModel()
    {
        TaskDetails = GetTaskDetails();
    }

    private ObservableCollection<KanbanModel> GetTaskDetails()
    {
        var tasks = new ObservableCollection<KanbanModel>();

        // Janet's tasks
        tasks.Add(new KanbanModel
        {
            Title = "Customer meeting",
            Assignee = "Janet Leverling",
            Category = "Open",
            ColorKey = "High"
        });

        tasks.Add(new KanbanModel
        {
            Title = "Edge browser issues",
            Assignee = "Janet Leverling",
            Category = "In Progress",
            ColorKey = "Normal"
        });

        // Andrew's tasks
        tasks.Add(new KanbanModel
        {
            Title = "Application performance",
            Assignee = "Andrew Fuller",
            Category = "Open",
            ColorKey = "Normal"
        });

        tasks.Add(new KanbanModel
        {
            Title = "Analysis",
            Assignee = "Andrew Fuller",
            Category = "In Progress",
            ColorKey = "High"
        });

        return tasks;
    }
}
```

## Customizing Swim Lane Headers

### Default Header

Shows the value of the grouping property (Assignee, ColorKey, etc.)

### Custom Header Template

Full control over swim lane header appearance:

```xml
<kanban:SfKanban SwimlaneKey="Assignee"
                 ItemsSource="{Binding TaskDetails}">
    <kanban:SfKanban.SwimlaneHeaderTemplate>
        <DataTemplate>
            <Border BorderBrush="Black"
                    CornerRadius="5"
                    Width="150"
                    Margin="10,2,10,0"
                    HorizontalAlignment="Left">
                <StackPanel Background="LightGray"
                           Orientation="Horizontal">
                    <!-- Expand/Collapse Icon -->
                    <Grid Background="Transparent"
                          Height="30"
                          Width="30">
                        <Path x:Name="ExpandedPath"
                              Data="M30.587915,0L31.995998,1.4199842 15.949964,17.351 0,1.4979873 1.4099131,0.078979151 15.949964,14.53102z"
                              Stretch="Uniform"
                              Fill="#FF000000"
                              Width="14"
                              Height="14"/>
                    </Grid>
                    
                    <!-- Swim Lane Title -->
                    <TextBlock FontWeight="Medium"
                              FontSize="15"
                              VerticalAlignment="Center"
                              Text="{Binding Title}" />
                </StackPanel>
            </Border>
        </DataTemplate>
    </kanban:SfKanban.SwimlaneHeaderTemplate>
</kanban:SfKanban>
```

**DataContext:** KanbanModel instance (first card in swim lane)

**Available Bindings:**
- `Title` - Card title (use grouping property value as identifier)
- Any property from KanbanModel or custom model

### Advanced Header with Image

```xml
<kanban:SfKanban.SwimlaneHeaderTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal" 
                    Background="#F0F0F0"
                    Padding="10,5">
            <!-- Avatar Image -->
            <Border Width="40" 
                    Height="40" 
                    CornerRadius="20"
                    Margin="0,0,10,0">
                <ContentControl Content="{Binding Image}"/>
            </Border>
            
            <!-- Info -->
            <StackPanel VerticalAlignment="Center">
                <TextBlock Text="{Binding Assignee}"
                          FontWeight="Bold"
                          FontSize="14"/>
                <TextBlock Text="{Binding Title}"
                          FontSize="11"
                          Foreground="Gray"/>
            </StackPanel>
        </StackPanel>
    </DataTemplate>
</kanban:SfKanban.SwimlaneHeaderTemplate>
```

## Use Cases

### 1. Group by Team Members

```csharp
// Set swim lane by assignee
kanban.SwimlaneKey = "Assignee";

// Data with assignees
new KanbanModel { Assignee = "John Doe", /* ... */ },
new KanbanModel { Assignee = "Jane Smith", /* ... */ },
```

**Result:** Each team member gets their own swim lane row

### 2. Group by Priority

```csharp
// Set swim lane by priority
kanban.SwimlaneKey = "ColorKey";

// Data with priorities
new KanbanModel { ColorKey = "High", /* ... */ },
new KanbanModel { ColorKey = "Normal", /* ... */ },
new KanbanModel { ColorKey = "Low", /* ... */ },
```

**Result:** Three swim lanes - High, Normal, Low priority

### 3. Group by Project

```csharp
// Custom model with project
public class TaskDetails
{
    public string Project { get; set; }
    // ... other properties
}

// Set swim lane by project
kanban.SwimlaneKey = "Project";
```

**Result:** Each project gets its own swim lane

### 4. Group by Issue Type

```csharp
// Custom model with issue type
public class Issue
{
    public string Type { get; set; }  // Bug, Feature, Task
    // ... other properties
}

// Set swim lane by type
kanban.SwimlaneKey = "Type";
```

**Result:** Swim lanes for Bugs, Features, Tasks

## Best Practices

1. **Limit Swim Lanes:** 3-7 swim lanes optimal for readability
2. **Consistent Values:** Use standardized values for grouping property
3. **Meaningful Headers:** Customize headers to show relevant information
4. **Visual Hierarchy:** Use colors or icons to differentiate swim lanes
5. **Team Organization:** Group by assignee for team-based workflows
6. **Priority Focus:** Group by priority to highlight urgent work
7. **Project Tracking:** Group by project for multi-project boards

## Troubleshooting

### Swim Lanes Not Appearing

**Problem:** All cards in single row, no swim lanes

**Solutions:**
1. Check SwimlaneKey is set
2. Verify mapped property exists in data model
3. Ensure property has varying values (not all same)
4. For KanbanModel, Assignee is default - check if set

### All Cards in One Swim Lane

**Problem:** Only one swim lane shows despite different values

**Solutions:**
1. Verify grouping property has different values in data
2. Check property name in SwimlaneKey matches model exactly (case-sensitive)
3. Ensure property type is consistent (all strings or all ints, etc.)

### Custom Header Not Showing

**Problem:** SwimlaneHeaderTemplate not applied

**Solutions:**
1. Verify DataTemplate is correctly defined in XAML
2. Check bindings in template reference valid properties
3. Ensure swim lanes are enabled (SwimlaneKey is set)
4. Test with simple template first

### Empty Swim Lanes

**Problem:** Swim lane headers show but no cards

**Solutions:**
1. Check cards have Category values matching column Categories
2. Verify ColumnMappingPath if using custom model
3. Ensure ItemsSource has data
4. Check if cards are filtered out by workflows

## Common Patterns

### Pattern 1: Team-Based Board
```csharp
kanban.SwimlaneKey = "Assignee";
// Cards automatically grouped by team member
```

### Pattern 2: Priority-Based Board
```csharp
kanban.SwimlaneKey = "ColorKey";
// High-priority tasks in top swim lane
```

### Pattern 3: Project-Based Board
```csharp
// Custom model with Project property
kanban.SwimlaneKey = "Project";
// Each project in separate swim lane
```

### Pattern 4: Custom Header with Count
```xml
<kanban:SfKanban.SwimlaneHeaderTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="{Binding Assignee}"/>
            <TextBlock Text=" (" Margin="5,0,0,0"/>
            <TextBlock Text="{Binding ItemsCount}"/>
            <TextBlock Text=" tasks)"/>
        </StackPanel>
    </DataTemplate>
</kanban:SfKanban.SwimlaneHeaderTemplate>
```

## Notes

- **DataContext for Template:** First KanbanModel in swim lane group
- **No Swim Lanes:** Omit SwimlaneKey property for flat board
- **Null Values:** Cards with null grouping property appear in separate "ungrouped" swim lane
- **Dynamic Grouping:** Changing SwimlaneKey at runtime updates swim lanes immediately
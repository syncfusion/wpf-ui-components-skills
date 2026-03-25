# Columns in WPF Kanban

## Table of Contents
- [Column Overview](#column-overview)
- [Auto-Generate vs Manual Columns](#auto-generate-vs-manual-columns)
- [Column Width Configuration](#column-width-configuration)
- [Column Categorization](#column-categorization)
- [Multi-Category Columns](#multi-category-columns)
- [Column Header Customization](#column-header-customization)
- [Expand/Collapse Columns](#expandcollapse-columns)
- [Drag and Drop Configuration](#drag-and-drop-configuration)
- [Drag-Drop Placeholders](#drag-drop-placeholders)
- [Work-in-Progress (WIP) Limits](#work-in-progress-wip-limits)
- [Troubleshooting](#troubleshooting)

## Column Overview

Columns organize the Kanban board into distinct workflow stages (e.g., To-Do, In Progress, Testing, Done). Each column contains cards matching its assigned categories.

**Key Concepts:**
- Columns can be auto-generated or manually defined
- Cards are placed in columns based on Category property matching
- Each column supports drag-drop, collapse/expand, and WIP limits
- Multiple data categories can map to a single column

## Auto-Generate vs Manual Columns

### Auto-Generate Columns

Automatically creates columns from unique Category values in data.

```xml
<kanban:SfKanban AutoGenerateColumns="True"
                 ItemsSource="{Binding TaskDetails}"/>
```

**C#:**
```csharp
kanban.AutoGenerateColumns = true;
kanban.ItemsSource = viewModel.TaskDetails;
```

**Advantages:**
- Quick setup, no column configuration needed
- Dynamically adapts to data changes
- Column headers match Category values

**Disadvantages:**
- Cannot customize column headers
- No control over column order
- Cannot combine multiple categories in one column

**When to Use:** Prototyping, simple boards, dynamic category sets

### Manual Columns

Explicitly define each column with custom headers and category mappings.

```xml
<kanban:SfKanban AutoGenerateColumns="False"
                 ItemsSource="{Binding TaskDetails}">
    <kanban:KanbanColumn Title="To Do" Categories="Open,New" />
    <kanban:KanbanColumn Title="In Progress" Categories="In Progress" />
    <kanban:KanbanColumn Title="Done" Categories="Closed,Done" />
</kanban:SfKanban>
```

**C#:**
```csharp
kanban.AutoGenerateColumns = false;
kanban.Columns.Add(new KanbanColumn() { Title = "To Do", Categories = "Open,New" });
kanban.Columns.Add(new KanbanColumn() { Title = "In Progress", Categories = "In Progress" });
kanban.Columns.Add(new KanbanColumn() { Title = "Done", Categories = "Closed,Done" });
```

**Advantages:**
- Custom column headers
- Control column order
- Multiple categories per column
- Set WIP limits per column
- Configure drag-drop per column

**When to Use:** Production apps, custom workflows, WIP limits needed

## Column Width Configuration

### Minimum and Maximum Width

Set bounds for automatic column sizing:

```xml
<kanban:SfKanban MinColumnWidth="200"
                 MaxColumnWidth="400"
                 ItemsSource="{Binding TaskDetails}"/>
```

**C#:**
```csharp
kanban.MinColumnWidth = 200;
kanban.MaxColumnWidth = 400;
```

**Behavior:**
- Columns resize within bounds based on content
- Maintains readability with minimum width
- Prevents excessive width with maximum

**Use Case:** Responsive layouts, varying card content sizes

### Fixed Column Width

Set exact width for all columns:

```xml
<kanban:SfKanban ColumnWidth="300"
                 ItemsSource="{Binding TaskDetails}"/>
```

**C#:**
```csharp
kanban.ColumnWidth = 300;
```

**Behavior:**
- All columns have identical width
- Overrides minimum/maximum settings
- Provides uniform appearance

**Use Case:** Consistent layout, fixed-size displays

### Column Width Priority

1. **ColumnWidth** (if set) - Highest priority, exact width
2. **MinColumnWidth / MaxColumnWidth** - Bounded auto-sizing
3. **Default** - Smart sizing based on content

## Column Categorization

### Default Category Mapping

By default, columns categorize by the `Category` property in `KanbanModel`:

```csharp
var task = new KanbanModel()
{
    Category = "In Progress",  // Determines column placement
    // ...other properties
};
```

### Custom Property Mapping

Map columns to a different property using `ColumnMappingPath`:

```xml
<kanban:SfKanban ColumnMappingPath="Status"
                 ItemsSource="{Binding Tasks}"/>
```

**Custom Model:**
```csharp
public class Task
{
    public string Title { get; set; }
    public string Status { get; set; }  // Used for column assignment
    public string Description { get; set; }
}

public class TaskViewModel
{
    private ObservableCollection<Task> _tasks;
    public ObservableCollection<Task> Tasks
    {
        get => _tasks;
        set { _tasks = value; }
    }

    public TaskViewModel()
    {
        Tasks = new ObservableCollection<Task>
        {
            new Task 
            { 
                Title = "Fix login bug", 
                Status = "Open",
                Description = "User cannot login with email"
            },
            new Task 
            { 
                Title = "Add dashboard", 
                Status = "In Progress",
                Description = "Create analytics dashboard"
            }
        };
    }
}

```

**Important:** ColumnMappingPath only works with properties of `KanbanModel` when using default model.

### Category Matching Rules

- **Exact Match:** Card Category must exactly match column Categories value
- **Case-Sensitive:** "open" ≠ "Open"
- **Multiple Categories:** Comma-separated values in Categories property
- **Unmatched Cards:** Not displayed if no column matches their category

## Multi-Category Columns

Map multiple data categories to a single column:

```xml
<kanban:KanbanColumn Title="Backlog" 
                     Categories="Open,New,Pending" />
```

**Effect:** Cards with Category of "Open", "New", or "Pending" all appear in "Backlog" column

**Example: Grouping Related States**
```xml
<kanban:SfKanban AutoGenerateColumns="False">
    <kanban:KanbanColumn Title="To Do" 
                         Categories="Open,New,Backlog" />
    <kanban:KanbanColumn Title="Active" 
                         Categories="In Progress,Validated,Review" />
    <kanban:KanbanColumn Title="Complete" 
                         Categories="Done,Closed,Resolved" />
</kanban:SfKanban>
```

**Use Case:** Simplifying complex workflows, grouping similar states

## Column Header Customization

### Custom Header Text

```xml
<kanban:KanbanColumn Title="📋 To Do" Categories="Open" />
<kanban:KanbanColumn Title="⚙️ In Progress" Categories="In Progress" />
<kanban:KanbanColumn Title="✅ Done" Categories="Done" />
```

**Result:** Headers display with emojis or custom text

### Custom Header Template

Full control over header appearance:

```xml
<kanban:SfKanban ItemsSource="{Binding TaskDetails}">
    <kanban:SfKanban.ColumnHeaderTemplate>
        <DataTemplate>
            <Grid Height="50" Background="LightBlue">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                
                <!-- Column Name -->
                <TextBlock Grid.Row="0"
                          Text="{Binding HeaderText}"
                          FontWeight="Bold"
                          FontSize="14"
                          Foreground="DarkBlue"
                          Margin="10,5,0,0"/>
                
                <!-- Card Count -->
                <StackPanel Grid.Row="1" 
                           Orientation="Horizontal"
                           Margin="10,0,0,5">
                    <TextBlock Text="Items: " 
                              FontSize="11"
                              Foreground="Black"/>
                    <TextBlock Text="{Binding CardsCount}" 
                              FontSize="11"
                              FontWeight="Bold"
                              Foreground="Black"/>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </kanban:SfKanban.ColumnHeaderTemplate>
</kanban:SfKanban>
```

**DataContext Properties:**
- `Title` - Column header text
- `Cards.Count` - Number of cards in column

## Expand/Collapse Columns

### Programmatic Control

```xml
<kanban:KanbanColumn Title="Done" 
                     Categories="Done" 
                     IsExpanded="False" />
```

**C#:**
```csharp
var column = new KanbanColumn() 
{ 
    Title = "Done", 
    Categories = "Done", 
    IsExpanded = false 
};
kanban.Columns.Add(column);
```

**Effect:** Column starts collapsed, showing only header

### User Interaction

By default, users can collapse/expand columns by tapping toggle button in column header (top-right corner).

**Collapsed State:**
- Shows only column header
- Hides all cards
- Saves horizontal space
- Toggle button rotates to indicate state

**Expanded State:**
- Shows header and cards
- Full column functionality
- Default state

### Use Cases

- Collapse "Done" column to focus on active work
- Collapse low-priority columns temporarily
- Save screen space on smaller displays

## Drag and Drop Configuration

### Enable/Disable Per Column

**Disable Dragging FROM Column:**
```xml
<kanban:KanbanColumn Title="Done" 
                     Categories="Done" 
                     AllowDrag="False" />
```

### Default Behavior

- `AllowDrag = true` (default) - Cards can be dragged from column

### Common Patterns

**1. Lock Completed Items:**
```xml
<kanban:KanbanColumn Title="Done" 
                     Categories="Done" 
                     AllowDrag="False" />
```

**2. Prevent Drops to Backlog:**
```xml
<kanban:KanbanColumn Title="Backlog" 
                     Categories="Open" />
```

**3. Read-Only Archive:**
```xml
<kanban:KanbanColumn Title="Archive" 
                     Categories="Archived" 
                     AllowDrag="False" />
```

## Drag-Drop Placeholders

Visual indicators showing where card will be placed during drag.

### Customize Placeholder Appearance

Use theme keys to customize:

```xml
<Grid>
    <kanban:SfKanban ItemsSource="{Binding TaskDetails}">
        <kanban:SfKanban.PlaceholderStyle>
            <kanban:PlaceholderStyle FontSize="20"
                        Foreground="DarkGreen"
                        Fill="Fuchsia"
                        Stroke="Blue"
                        StrokeThickness="2"
                        SelectedFontSize="20"
                        SelectedForeground="Maroon"
                        SelectedStroke="Yellow"
                        SelectedStrokeThickness="2"
                        SelectedBackground="LawnGreen"/>
        </kanban:SfKanban.PlaceholderStyle >
    </kanban:SfKanban>
</Grid>
```

**Placeholder Types:**
- **Drag Placeholder:** Shows current drag position
- **Drop Placeholder:** Highlights valid drop location

## Work-in-Progress (WIP) Limits

Set minimum and maximum card limits per column to manage workflow.

### Setting WIP Limits

```xml
<kanban:KanbanColumn Title="In Progress" 
                     Categories="In Progress"
                     MinimumLimit="1"
                     MaximumLimit="5" />
```

**C#:**
```csharp
var column = new KanbanColumn() 
{ 
    Title = "In Progress",
    Categories = "In Progress",
    MinimumLimit = 1,    // Must have at least 1 card
    MaximumLimit = 5     // Cannot exceed 5 cards
};
```

### Visual Indicators

**Error Bar Colors:**
- **Red:** Card count exceeds MaximumLimit
- **Yellow/Orange:** Card count below MinimumLimit
- **Normal:** Card count within limits

**Location:** Appears in column header area

### Example: Development Workflow

```xml
<kanban:SfKanban AutoGenerateColumns="False">
    <!-- Backlog: No limits -->
    <kanban:KanbanColumn Title="Backlog" 
                         Categories="Open" />
    
    <!-- In Progress: Limit active work -->
    <kanban:KanbanColumn Title="In Progress" 
                         Categories="In Progress"
                         MaximumLimit="3" />
    
    <!-- Testing: Limit testing bottleneck -->
    <kanban:KanbanColumn Title="Testing" 
                         Categories="Testing"
                         MinimumLimit="1"
                         MaximumLimit="5" />
    
    <!-- Done: No limits -->
    <kanban:KanbanColumn Title="Done" 
                         Categories="Done" />
</kanban:SfKanban>
```

### Benefits

1. **Prevent Overload:** MaximumLimit stops too many tasks in one stage
2. **Maintain Flow:** MinimumLimit ensures stages aren't empty
3. **Visual Feedback:** Immediate indication of workflow issues
4. **Team Coordination:** Shared understanding of capacity limits

### Best Practices

- Set MaximumLimit on bottleneck columns
- Use MinimumLimit to ensure continuous flow
- Adjust limits based on team capacity
- Review and update limits periodically

## Troubleshooting

### Columns Not Appearing

**Problem:** Kanban is empty, no columns visible

**Solutions:**
1. Check AutoGenerateColumns setting:
   - If true: Verify data has Category property with values
   - If false: Ensure Columns collection has items
2. Verify ItemsSource is bound and contains data
3. Check DataContext is set correctly

### Cards in Wrong Columns

**Problem:** Cards appear in unexpected columns

**Solutions:**
1. Verify Category values match column Categories exactly (case-sensitive)
2. Check for typos in Categories property values
3. If using ColumnMappingPath, ensure property exists and has correct values
4. Review multi-category column assignments

### WIP Limits Not Working

**Problem:** No visual indicators for exceeded limits

**Solutions:**
1. Verify MinimumLimit/MaximumLimit are set on columns
2. Check actual card count vs limits
3. Ensure theme resources are loaded
4. Verify column has correct Categories assignment

### Drag-Drop Not Working

**Problem:** Cannot move cards between columns

**Solutions:**
1. Check AllowDrag=true on source column (default)
2. Review Workflows configuration (may restrict transitions)
3. Ensure Categories are correctly mapped

### Column Width Issues

**Problem:** Columns too narrow or too wide

**Solutions:**
1. Set MinColumnWidth to prevent excessive narrowing
2. Set MaxColumnWidth to prevent excessive widening
3. Use ColumnWidth for uniform sizing
4. Check for conflicting width settings

### Custom Header Not Displaying

**Problem:** ColumnHeaderTemplate not applied

**Solutions:**
1. Verify DataTemplate is correctly defined
2. Check bindings in template (Title, Cards.Count)
3. Ensure template is set on SfKanban, not on KanbanColumn
4. Test with simple template first

## Best Practices

1. **Manual Columns for Production:** Better control and customization
2. **Meaningful Headers:** Clear, concise column names (3-7 words)
3. **Set WIP Limits:** Prevent bottlenecks in workflow
4. **Group Related States:** Use multi-category columns to simplify UI
5. **Responsive Widths:** Set MinColumnWidth for mobile/tablet support
6. **Lock Completed Work:** Set AllowDrag=false on "Done" column
7. **Visual Hierarchy:** Use header customization for priority columns
8. **Test Drag-Drop:** Verify all valid transitions work correctly
9. **Limit Column Count:** 3-7 columns optimal for readability
10. **Consistent Naming:** Use standardized category names across data

## Common Patterns

### Pattern 1: Simple Three-Column Board
```xml
<kanban:SfKanban AutoGenerateColumns="False">
    <kanban:KanbanColumn Title="To Do" Categories="Open" />
    <kanban:KanbanColumn Title="Doing" Categories="In Progress" MaximumLimit="3" />
    <kanban:KanbanColumn Title="Done" Categories="Done" AllowDrag="False" />
</kanban:SfKanban>
```

### Pattern 2: Development Workflow
```xml
<kanban:SfKanban AutoGenerateColumns="False">
    <kanban:KanbanColumn Title="Backlog" Categories="Open,New" />
    <kanban:KanbanColumn Title="Development" Categories="In Progress" MaximumLimit="5" />
    <kanban:KanbanColumn Title="Code Review" Categories="Review" MaximumLimit="3" />
    <kanban:KanbanColumn Title="Testing" Categories="Testing" MaximumLimit="4" />
    <kanban:KanbanColumn Title="Done" Categories="Closed,Done" AllowDrag="False" />
</kanban:SfKanban>
```

### Pattern 3: Support Ticket Board
```xml
<kanban:SfKanban AutoGenerateColumns="False">
    <kanban:KanbanColumn Title="New Tickets" Categories="New" />
    <kanban:KanbanColumn Title="In Progress" Categories="Assigned,Working" MaximumLimit="10" />
    <kanban:KanbanColumn Title="Waiting" Categories="Pending,Blocked" />
    <kanban:KanbanColumn Title="Resolved" Categories="Resolved,Closed" AllowDrag="False" />
</kanban:SfKanban>
```
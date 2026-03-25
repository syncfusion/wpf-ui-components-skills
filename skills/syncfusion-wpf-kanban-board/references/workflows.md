# Workflows in WPF Kanban

Workflows define the allowed flow of cards between columns, enabling restrictions when moving cards from one state to another. This enforces business rules and prevents invalid state transitions.

## Overview

**Purpose:** Control which columns cards can be moved to  
**Implementation:** Define allowed transitions per source category  
**Enforcement:** Drag-and-drop prevented for disallowed transitions  
**Visual Feedback:** Invalid drop targets not highlighted during drag

## Basic Concepts

### Without Workflows
- Cards can move to any column
- No restrictions on state transitions
- User has complete freedom

### With Workflows
- Cards can only move to explicitly allowed columns
- State transitions follow defined rules
- Business logic enforced automatically

## Defining Workflows

### Workflow Structure

Each `KanbanWorkflow` defines:
- **Category:** Source category/state
- **AllowedTransitions:** List of target categories card can move to

```xml
<kanban:SfKanban.Workflows>
    <kanban:KanbanWorkflow Category="Open">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>In Progress</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
</kanban:SfKanban.Workflows>
```

**Effect:** Cards in "Open" category can ONLY be moved to "In Progress"

## Complete Example

### XAML

```xml
<kanban:SfKanban x:Name="kanban"
                 AutoGenerateColumns="False"
                 ItemsSource="{Binding TaskDetails}">
    <kanban:SfKanban.DataContext>
        <local:ViewModel/>
    </kanban:SfKanban.DataContext>
    
    <!-- Define Workflows -->
    <kanban:SfKanban.Workflows>
        <!-- Open can only go to In Progress -->
        <kanban:KanbanWorkflow Category="Open">
            <kanban:KanbanWorkflow.AllowedTransitions>
                <x:String>In Progress</x:String>
            </kanban:KanbanWorkflow.AllowedTransitions>
        </kanban:KanbanWorkflow>
        
        <!-- In Progress can go to Closed or Won't Fix -->
        <kanban:KanbanWorkflow Category="In Progress">
            <kanban:KanbanWorkflow.AllowedTransitions>
                <x:String>Closed</x:String>
                <x:String>Won't Fix</x:String>
            </kanban:KanbanWorkflow.AllowedTransitions>
        </kanban:KanbanWorkflow>
    </kanban:SfKanban.Workflows>
    
    <!-- Define Columns -->
    <kanban:KanbanColumn Title="To Do" 
                         Categories="Open,Postponed" />
    <kanban:KanbanColumn Title="In Progress"
                         Categories="In Progress"/>
    <kanban:KanbanColumn Title="Done"
                         Categories="Closed,Won't Fix"/>
</kanban:SfKanban>
```

### C# Approach

```csharp
using Syncfusion.UI.Xaml.Kanban;

kanban.Workflows = new WorkflowCollection()
{
    // Open -> In Progress
    new KanbanWorkflow() 
    { 
        Category = "Open", 
        AllowedTransitions = { "In Progress" } 
    },
    
    // In Progress -> Closed or Won't Fix
    new KanbanWorkflow() 
    { 
        Category = "In Progress", 
        AllowedTransitions = { "Closed", "Won't Fix" } 
    }
};
```

### Behavior

**Allowed Transitions:**
- Open → In Progress ✓
- In Progress → Closed ✓
- In Progress → Won't Fix ✓

**Blocked Transitions:**
- Open → Closed ✗
- Open → Won't Fix ✗
- In Progress → Open ✗
- Closed → (anywhere) ✗ (no workflow defined)

## Workflow Patterns

### Pattern 1: Linear Workflow

Cards must progress through stages in order:

```xml
<kanban:SfKanban.Workflows>
    <!-- To Do -> In Progress -->
    <kanban:KanbanWorkflow Category="To Do">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>In Progress</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <!-- In Progress -> Testing -->
    <kanban:KanbanWorkflow Category="In Progress">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Testing</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <!-- Testing -> Done -->
    <kanban:KanbanWorkflow Category="Testing">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Done</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
</kanban:SfKanban.Workflows>
```

**Flow:** To Do → In Progress → Testing → Done (no skipping, no going back)

### Pattern 2: Branching Workflow

Cards can take different paths based on outcome:

```xml
<kanban:SfKanban.Workflows>
    <!-- Backlog -> Analysis -->
    <kanban:KanbanWorkflow Category="Backlog">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Analysis</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <!-- Analysis -> Development OR Rejected -->
    <kanban:KanbanWorkflow Category="Analysis">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Development</x:String>
            <x:String>Rejected</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <!-- Development -> Testing -->
    <kanban:KanbanWorkflow Category="Development">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Testing</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <!-- Testing -> Done OR Development (failed testing) -->
    <kanban:KanbanWorkflow Category="Testing">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Done</x:String>
            <x:String>Development</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
</kanban:SfKanban.Workflows>
```

**Paths:**
- Success: Backlog → Analysis → Development → Testing → Done
- Rejection: Backlog → Analysis → Rejected
- Re-work: Testing → Development (loop back)

### Pattern 3: Support Ticket Workflow

```xml
<kanban:SfKanban.Workflows>
    <!-- New -> Assigned OR Rejected -->
    <kanban:KanbanWorkflow Category="New">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Assigned</x:String>
            <x:String>Rejected</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <!-- Assigned -> Working OR Waiting -->
    <kanban:KanbanWorkflow Category="Assigned">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Working</x:String>
            <x:String>Waiting</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <!-- Working -> Resolved OR Waiting -->
    <kanban:KanbanWorkflow Category="Working">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Resolved</x:String>
            <x:String>Waiting</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <!-- Waiting -> Working (customer responded) -->
    <kanban:KanbanWorkflow Category="Waiting">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Working</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
</kanban:SfKanban.Workflows>
```

### Pattern 4: One-Way Progression (No Backwards)

Prevent moving cards backwards in workflow:

```xml
<kanban:SfKanban.Workflows>
    <!-- Each stage can only move forward -->
    <kanban:KanbanWorkflow Category="Backlog">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>To Do</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <kanban:KanbanWorkflow Category="To Do">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>In Progress</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <kanban:KanbanWorkflow Category="In Progress">
        <kanban:KanbanWorkflow.AllowedTransitions>
            <x:String>Done</x:String>
        </kanban:KanbanWorkflow.AllowedTransitions>
    </kanban:KanbanWorkflow>
    
    <!-- Done: No workflow = cannot move anywhere -->
</kanban:SfKanban.Workflows>
```

**Effect:** Cards can only progress forward, never backwards

## Preventing All Drag-Drop from Column

To make a column read-only (no drops from or to):

**Omit workflow for that category:**
```xml
<!-- No workflow for "Done" category -->
<!-- Cards in "Done" cannot be moved anywhere -->
```

**Or set AllowDrag/AllowDrop on column:**
```xml
<kanban:KanbanColumn Title="Done" 
                     Categories="Done" 
                     AllowDrag="False" 
                     AllowDrop="False" />
```

## Workflow Rules

### Rule 1: Exact Category Match
Workflow Category must match card's Category value exactly (case-sensitive)

### Rule 2: Undefined Workflow
If no workflow is defined for a category, cards can move to ANY column

### Rule 3: Empty AllowedTransitions
If AllowedTransitions is empty, cards cannot be moved to ANY column

### Rule 4: Multi-Category Columns
Use exact Category value in workflow, not column's Categories string

**Example:**
```xml
<!-- Column combines multiple categories -->
<kanban:KanbanColumn Title="Done" Categories="Closed,Done" />

<!-- Workflow uses exact category value -->
<kanban:KanbanWorkflow Category="In Progress">
    <kanban:KanbanWorkflow.AllowedTransitions>
        <x:String>Closed</x:String>  <!-- Not "Closed,Done" -->
    </kanban:KanbanWorkflow.AllowedTransitions>
</kanban:KanbanWorkflow>
```

## Use Cases

### 1. Software Development
```
Backlog → To Do → Development → Code Review → Testing → Done
                                    ↓
                                Rejected
```

### 2. Issue Tracking
```
New → Open → Assigned → In Progress → Resolved → Closed
        ↓                    ↓
    Duplicate           Won't Fix
```

### 3. Content Approval
```
Draft → Review → Approved → Published
          ↓
      Rejected → Draft (re-submission)
```

### 4. Sales Pipeline
```
Lead → Qualified → Proposal → Negotiation → Won
         ↓            ↓           ↓
       Lost        Lost        Lost
```

## Best Practices

1. **Document Workflow:** Maintain diagram of allowed transitions
2. **Test Thoroughly:** Verify all valid and invalid transitions
3. **User Feedback:** Provide visual cues for invalid drops
4. **Flexibility:** Allow common backward transitions (rework scenarios)
5. **Terminal States:** Lock final states (Done, Archived, etc.)
6. **Error Handling:** Handle edge cases (null categories, unmatched values)
7. **Business Rules:** Align workflows with actual business processes

## Troubleshooting

### Drag-Drop Not Working

**Problem:** Cannot move cards at all

**Solutions:**
1. Check if workflows are too restrictive
2. Verify AllowedTransitions contain valid category values
3. Ensure category values match exactly (case-sensitive)
4. Test without workflows first to isolate issue

### Cards Moving to Wrong Columns

**Problem:** Cards can move where they shouldn't

**Solutions:**
1. Verify workflow Category matches card's Category exactly
2. Check for missing workflow definitions (undefined = no restrictions)
3. Review column Categories for typos
4. Ensure workflow covers all necessary categories

### Workflow Not Enforced

**Problem:** Restrictions not being applied

**Solutions:**
1. Check workflows are added to SfKanban.Workflows collection
2. Verify Category values in workflows match data exactly
3. Ensure AllowedTransitions list is populated
4. Test with simple workflow first

### Cannot Move Card Backwards

**Problem:** Need to move card to previous stage but blocked

**Solutions:**
1. Add backward transition to workflow
2. Re-evaluate if workflow should be one-way
3. Consider temporary "hold" or "blocked" columns
4. Implement admin override if needed

## Common Mistakes

❌ **Wrong Category Value:**
```xml
<!-- Column has "In Progress" -->
<kanban:KanbanWorkflow Category="InProgress">  <!-- Missing space -->
```

❌ **Using Column Categories String:**
```xml
<kanban:KanbanWorkflow Category="Open,New">  <!-- Wrong -->
```
✓ **Use Individual Categories:**
```xml
<kanban:KanbanWorkflow Category="Open">  <!-- Correct -->
<kanban:KanbanWorkflow Category="New">   <!-- Correct -->
```

❌ **Empty Transitions (unintended lock):**
```xml
<kanban:KanbanWorkflow Category="Open">
    <!-- Empty = cannot move anywhere -->
</kanban:KanbanWorkflow>
```

✓ **Explicitly Define Transitions:**
```xml
<kanban:KanbanWorkflow Category="Open">
    <kanban:KanbanWorkflow.AllowedTransitions>
        <x:String>In Progress</x:String>
    </kanban:KanbanWorkflow.AllowedTransitions>
</kanban:KanbanWorkflow>
```

## Notes

- Workflows are enforced client-side in UI only
- Server-side validation recommended for data integrity
- Workflows do not affect programmatic card updates
- Undefined workflows = no restrictions (default behavior)
- Combine with AllowDrag/AllowDrop for column-level control
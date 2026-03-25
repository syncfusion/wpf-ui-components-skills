# Visual Customization

Comprehensive guide for customizing the appearance of the WPF Gantt control including node styles, tooltips, highlighting, and visual indicators.

##Table of Contents
- [Custom Node Styles](#custom-node-styles)
- [Custom Tooltips](#custom-tooltips)
- [Highlighting Tasks](#highlighting-tasks)
- [Strip Lines](#strip-lines)
- [DateTime Indicators](#datetime-indicators)
- [Themes](#themes)

## Custom Node Styles

Customize the visual appearance of task bars, milestones, and header nodes.

### Basic Node Styling

```xml
<syncfusion:GanttControl.Resources>
    <Style TargetType="gantt:GanttNode">
        <Setter Property="Background" Value="LightBlue"/>
        <Setter Property="BorderBrush" Value="Navy"/>
        <Setter Property="BorderThickness" Value="1"/>
        <Setter Property="Height" Value="25"/>
    </Style>
</syncfusion:GanttControl.Resources>
```

### Conditional Styling Based on Progress

```xml
<Style TargetType="gantt:GanttNode">
    <Setter Property="Background" Value="LightGreen"/>
    <Style.Triggers>
        <!-- Completed tasks (100% progress) -->
        <DataTrigger Binding="{Binding Progress}" Value="100">
            <Setter Property="Background" Value="Green"/>
        </DataTrigger>
        
        <!-- In-progress tasks -->
        <DataTrigger Binding="{Binding Progress, Converter={StaticResource ProgressRangeConverter}}" Value="InProgress">
            <Setter Property="Background" Value="Orange"/>
        </DataTrigger>
        
        <!-- Not started (0% progress) -->
        <DataTrigger Binding="{Binding Progress}" Value="0">
            <Setter Property="Background" Value="LightGray"/>
        </DataTrigger>
    </Style.Triggers>
</Style>
```

### Custom Milestone Style

```xml
<Style TargetType="gantt:MileStone">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="gantt:MileStone">
                <Path Width="20" Height="20"
                      Data="M 10,0 L 20,10 L 10,20 L 0,10 Z"
                      Fill="Red"
                      Stroke="DarkRed"
                      StrokeThickness="2"/>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

## Custom Tooltips

Display rich information when hovering over tasks.

### Simple Tooltip

```csharp
ganttControl.NodeTooltipTemplate = CreateTooltipTemplate();

private DataTemplate CreateTooltipTemplate()
{
    var template = new DataTemplate();
    var factory = new FrameworkElementFactory(typeof(StackPanel));
    
    // Task name
    var nameText = new FrameworkElementFactory(typeof(TextBlock));
    nameText.SetBinding(TextBlock.TextProperty, new Binding("TaskName"));
    nameText.SetValue(TextBlock.FontWeightProperty, FontWeights.Bold);
    factory.AppendChild(nameText);
    
    // Date range
    var dateText = new FrameworkElementFactory(typeof(TextBlock));
    dateText.SetBinding(TextBlock.TextProperty, new Binding { StringFormat = "{0:MMM d} - {1:MMM d}", Path = new PropertyPath(".") });
    factory.AppendChild(dateText);
    
    // Progress
    var progressText = new FrameworkElementFactory(typeof(TextBlock));
    progressText.SetBinding(TextBlock.TextProperty, new Binding("Progress") { StringFormat = "Progress: {0}%" });
    factory.AppendChild(progressText);
    
    template.VisualTree = factory;
    return template;
}
```

### XAML Tooltip Template

```xml
<syncfusion:GanttControl.NodeTooltipTemplate>
    <DataTemplate>
        <Border Background="White" 
                BorderBrush="Gray" 
                BorderThickness="1" 
                Padding="10"
                CornerRadius="5">
            <StackPanel>
                <TextBlock Text="{Binding TaskName}" 
                          FontWeight="Bold" 
                          FontSize="14"/>
                <TextBlock Text="{Binding StartDate, StringFormat='Start: {0:MMM d, yyyy}'}"/>
                <TextBlock Text="{Binding FinishDate, StringFormat='End: {0:MMM d, yyyy}'}"/>
                <TextBlock Text="{Binding Progress, StringFormat='Progress: {0}%'}"/>
                <TextBlock Text="{Binding Duration, StringFormat='Duration: {0} days'}"/>
                
                <!-- Resources -->
                <ItemsControl ItemsSource="{Binding Resources}">
                    <ItemsControl.ItemTemplate>
                        <DataTemplate>
                            <TextBlock Text="{Binding Name}" 
                                      Foreground="Blue"/>
                        </DataTemplate>
                    </ItemsControl.ItemTemplate>
                </ItemsControl>
            </StackPanel>
        </Border>
    </DataTemplate>
</syncfusion:GanttControl.NodeTooltipTemplate>
```

## Highlighting Tasks

Emphasize specific tasks based on criteria.

### Highlight Critical Path

```csharp
private void HighlightCriticalTasks()
{
    foreach (var task in TaskCollection.Where(t => t.IsCritical))
    {
        // Apply custom style to critical tasks
        task.NodeStyle = new Style(typeof(GanttNode))
        {
            Setters = 
            {
                new Setter(GanttNode.BackgroundProperty, Brushes.Red),
                new Setter(GanttNode.HeightProperty, 30.0)
            }
        };
    }
}
```

### Highlight Overdue Tasks

```csharp
private void HighlightOverdueTasks()
{
    DateTime today = DateTime.Today;
    
    foreach (var task in TaskCollection)
    {
        if (task.FinishDate < today && task.Progress < 100)
        {
            task.NodeStyle = new Style(typeof(GanttNode))
            {
                Setters = 
                {
                    new Setter(GanttNode.BackgroundProperty, Brushes.DarkRed),
                    new Setter(GanttNode.ForegroundProperty, Brushes.White)
                }
            };
        }
    }
}
```

## Strip Lines

Add vertical lines to mark important dates or events.

### Adding Strip Lines

```csharp
// Create strip line collection
var stripLines = new ObservableCollection<GanttStripLine>();

// Add milestone strip line
stripLines.Add(new GanttStripLine
{
    StartDate = new DateTime(2024, 3, 15),
    EndDate = new DateTime(2024, 3, 15),
    Background = new SolidColorBrush(Colors.Red),
    Width = 2,
    Content = "Major Milestone",
    ContentTemplate = CreateStripLineTemplate()
});

// Add project phase strip line (range)
stripLines.Add(new GanttStripLine
{
    StartDate = new DateTime(2024, 1, 1),
    EndDate = new DateTime(2024, 2, 28),
    Background = new SolidColorBrush(Color.FromArgb(50, 0, 0, 255)), // Semi-transparent
    Content = "Phase 1"
});

// Current date indicator
stripLines.Add(new GanttStripLine
{
    StartDate = DateTime.Today,
    EndDate = DateTime.Today,
    Background = Brushes.Green,
    Width = 3,
    Content = "Today"
});

ganttControl.StripLines = stripLines;
```

### Strip Line Template

```xml
<DataTemplate x:Key="StripLineTemplate">
    <Grid>
        <TextBlock Text="{Binding Content}" 
                  FontWeight="Bold" 
                  Foreground="White"
                  Background="Red"
                  Padding="5"
                  VerticalAlignment="Top"/>
    </Grid>
</DataTemplate>
```

## DateTime Indicators

Customize the current date/time indicator.

```csharp
// Enable current date indicator
ganttControl.ShowCurrentDateIndicator = true;

// Customize indicator style
ganttControl.CurrentDateIndicatorBrush = Brushes.Green;
ganttControl.CurrentDateIndicatorWidth = 3;
```

## Themes

Apply built-in themes for modern appearance.

```csharp
using Syncfusion.SfSkinManager;

// Apply Material Light theme
SfSkinManager.SetTheme(ganttControl, new Theme("MaterialLight"));

// Apply Fluent Dark theme
SfSkinManager.SetTheme(ganttControl, new Theme("FluentDark"));

// Apply Office 2019 theme
SfSkinManager.SetTheme(ganttControl, new Theme("Office2019Colorful"));
```

**Available Themes:**
- MaterialLight
- MaterialDark
- FluentLight
- FluentDark
- Office2019Colorful
- Office2019Black
- Office2019White

## Common Patterns

### Pattern 1: Priority-Based Coloring

```csharp
private Style GetStyleByPriority(string priority)
{
    var style = new Style(typeof(GanttNode));
    
    switch (priority)
    {
        case "High":
            style.Setters.Add(new Setter(GanttNode.BackgroundProperty, Brushes.Red));
            break;
        case "Medium":
            style.Setters.Add(new Setter(GanttNode.BackgroundProperty, Brushes.Orange));
            break;
        case "Low":
            style.Setters.Add(new Setter(GanttNode.BackgroundProperty, Brushes.Green));
            break;
    }
    
    return style;
}
```

### Pattern 2: Resource-Based Coloring

```csharp
private Dictionary<string, Brush> resourceColors = new Dictionary<string, Brush>
{
    { "Team A", Brushes.Blue },
    { "Team B", Brushes.Green },
    { "Team C", Brushes.Orange }
};

private void ApplyResourceColors()
{
    foreach (var task in TaskCollection)
    {
        if (task.Resources != null && task.Resources.Any())
        {
            var teamName = task.Resources.First().Name;
            if (resourceColors.ContainsKey(teamName))
            {
                task.NodeStyle = new Style(typeof(GanttNode))
                {
                    Setters = { new Setter(GanttNode.BackgroundProperty, resourceColors[teamName]) }
                };
            }
        }
    }
}
```

## Best Practices

1. **Use Themes for Consistency:** Apply built-in themes rather than custom styling everything.

2. **Keep Tooltips Concise:** Show only essential information to avoid overwhelming users.

3. **Use Strip Lines Sparingly:** Too many can clutter the view.

4. **Conditional Styling:** Style based on task state (progress, overdue, priority) for better visualization.

5. **Test Color Accessibility:** Ensure sufficient contrast for colorblind users.

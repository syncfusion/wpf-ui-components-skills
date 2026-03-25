# Events

Comprehensive guide for handling Sunburst Chart events. Events allow you to respond to user interactions and segment creation, enabling custom behaviors and dynamic updates.

## Overview

The Sunburst Chart provides two primary events:

1. **SegmentCreated** – Fires when each segment is created during rendering
2. **SelectionChanged** – Fires when segment selection changes (with SelectionBehavior)

These events enable dynamic customization, data tracking, and interactive features beyond built-in behaviors.

## SegmentCreated Event

The `SegmentCreated` event occurs for each segment as the chart renders. Use this event to customize segment appearance or track segment creation.

### Event Signature

```csharp
public event EventHandler<SunburstSegmentCreatedEventArgs> SegmentCreated
```

### Event Arguments

`SunburstSegmentCreatedEventArgs` provides access to:
- **Segment** – The created segment object with properties:
  - `CurrentLevel` – Hierarchy level (0-based index)
  - `Category` – Segment category name
  - `Value` – Segment numeric value
  - `Interior` – Fill color (can be modified)
  - `Stroke` – Border color
  - `StrokeThickness` – Border width

### Basic Usage

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}"               
                          ValueMemberPath="EmployeesCount" 
                          SegmentCreated="Chart_SegmentCreated">
    
    <sunburst:SfSunburstChart.Levels>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="Country"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobDescription"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobGroup"/>
        <sunburst:SunburstHierarchicalLevel GroupMemberPath="JobRole"/>
    </sunburst:SfSunburstChart.Levels>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
private void Chart_SegmentCreated(object sender, SunburstSegmentCreatedEventArgs e)
{
    // Access segment properties
    int level = e.Segment.CurrentLevel;
    string category = e.Segment.Category;
    double value = e.Segment.Value;
    
    // Custom logic here
}
```

### Example 1: Color by Level

Apply different colors based on hierarchy level:

```csharp
private void Chart_SegmentCreated(object sender, SunburstSegmentCreatedEventArgs e)
{
    byte r = 180, g = 0, b = 96;
    byte r1 = 255, g1 = 153, b1 = 0;
    byte r2 = 102, g2 = 153, b2 = 0;
    byte r3 = 102, g3 = 204, b3 = 255;
    
    if (e.Segment.CurrentLevel == 0)
        e.Segment.Interior = new SolidColorBrush(Color.FromRgb(r3, g3, b3));
    else if (e.Segment.CurrentLevel == 1)
        e.Segment.Interior = new SolidColorBrush(Color.FromRgb(r1, g1, b1));
    else if (e.Segment.CurrentLevel == 2)
        e.Segment.Interior = new SolidColorBrush(Color.FromRgb(r, g, b));
    else if (e.Segment.CurrentLevel == 3)
        e.Segment.Interior = new SolidColorBrush(Color.FromRgb(r2, g2, b2));
}
```

**Result:** Each hierarchy level displays in a distinct color.

### Example 2: Conditional Styling

Apply special styling based on segment value or category:

```csharp
private void Chart_SegmentCreated(object sender, SunburstSegmentCreatedEventArgs e)
{
    // Highlight segments with high values
    if (e.Segment.Value > 100)
    {
        e.Segment.Interior = new SolidColorBrush(Colors.Red);
        e.Segment.Stroke = new SolidColorBrush(Colors.DarkRed);
        e.Segment.StrokeThickness = 3;
    }
    
    // Dim segments with low values
    else if (e.Segment.Value < 20)
    {
        e.Segment.Interior = new SolidColorBrush(Colors.LightGray);
        e.Segment.Opacity = 0.5;
    }
    
    // Special color for specific categories
    else if (e.Segment.Category == "Priority")
    {
        e.Segment.Interior = new SolidColorBrush(Colors.Gold);
    }
}
```

### Example 3: Gradient by Value

Create a gradient effect based on segment values:

```csharp
private void Chart_SegmentCreated(object sender, SunburstSegmentCreatedEventArgs e)
{
    // Calculate intensity based on value (0-255)
    double maxValue = 200; // Your data's max value
    byte intensity = (byte)Math.Min(255, (e.Segment.Value / maxValue) * 255);
    
    // Apply color with varying intensity
    e.Segment.Interior = new SolidColorBrush(Color.FromRgb(intensity, 100, 200));
}
```

### Example 4: Pattern-Based Styling

Apply hatched or patterned fills:

```csharp
private void Chart_SegmentCreated(object sender, SunburstSegmentCreatedEventArgs e)
{
    // Apply pattern to specific level
    if (e.Segment.CurrentLevel == 2)
    {
        // Create a hatched brush
        DrawingBrush hatchBrush = new DrawingBrush()
        {
            Viewport = new Rect(0, 0, 10, 10),
            ViewportUnits = BrushMappingMode.Absolute,
            TileMode = TileMode.Tile
        };
        
        GeometryDrawing drawing = new GeometryDrawing()
        {
            Geometry = new RectangleGeometry(new Rect(0, 0, 10, 10)),
            Pen = new Pen(Brushes.DarkGray, 1)
        };
        
        hatchBrush.Drawing = drawing;
        e.Segment.Interior = hatchBrush;
    }
}
```

## SelectionChanged Event

The `SelectionChanged` event fires when users select or deselect a segment (requires `SunburstSelectionBehavior`).

### Event Signature

```csharp
public event EventHandler<SunburstSelectionChangedEventArgs> SelectionChanged
```

### Event Arguments

`SunburstSelectionChangedEventArgs` provides:
- **SelectedSegment** – The newly selected segment with properties:
  - `Category` – Segment category name
  - `Value` – Segment numeric value
  - `CurrentLevel` – Hierarchy level
  - `SliceIndex` – Index within the level
  - `Interior` – Fill color

### Basic Usage

**XAML:**
```xml
<sunburst:SfSunburstChart ItemsSource="{Binding Data}" 
                          ValueMemberPath="EmployeesCount" 
                          SelectionChanged="Chart_SelectionChanged">
    
    <sunburst:SfSunburstChart.Behaviors>
        <sunburst:SunburstSelectionBehavior EnableSelection="True" 
                                           SelectionCursor="Hand"/>               
    </sunburst:SfSunburstChart.Behaviors>
    
</sunburst:SfSunburstChart>
```

**C#:**
```csharp
private void Chart_SelectionChanged(object sender, SunburstSelectionChangedEventArgs e)
{
    // Access selected segment
    string category = e.SelectedSegment.Category;
    double value = e.SelectedSegment.Value;
    int level = e.SelectedSegment.CurrentLevel;
    int index = e.SelectedSegment.SliceIndex;
    
    // Custom logic here
}
```

### Example 1: Display Selection Info

Show selected segment details in a popup:

**XAML:**
```xml
<Grid>
    <sunburst:SfSunburstChart x:Name="chart"
                              ItemsSource="{Binding Data}" 
                              ValueMemberPath="EmployeesCount" 
                              SelectionChanged="Chart_SelectionChanged">
        
        <sunburst:SfSunburstChart.Behaviors>
            <sunburst:SunburstSelectionBehavior EnableSelection="True" 
                                               SelectionCursor="Hand"/>               
        </sunburst:SfSunburstChart.Behaviors>
        
    </sunburst:SfSunburstChart>
    
    <Popup x:Name="selectionPopup" 
           Placement="MousePoint" 
           Height="180" 
           Width="220">
        <Border Background="White" 
                BorderBrush="Black" 
                BorderThickness="3">
            <Grid Margin="20,5,5,5">
                <Grid.RowDefinitions>
                    <RowDefinition/>
                    <RowDefinition/>
                    <RowDefinition/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                
                <TextBlock Text="Category: " FontWeight="Bold" Grid.Row="0" Grid.Column="0"/>
                <TextBlock x:Name="categoryText" FontWeight="Bold" Grid.Row="0" Grid.Column="1"/>
                
                <TextBlock Text="Value: " FontWeight="Bold" Grid.Row="1" Grid.Column="0"/>
                <TextBlock x:Name="valueText" FontWeight="Bold" Grid.Row="1" Grid.Column="1"/>
                
                <TextBlock Text="Level: " FontWeight="Bold" Grid.Row="2" Grid.Column="0"/>
                <TextBlock x:Name="levelText" FontWeight="Bold" Grid.Row="2" Grid.Column="1"/>
                
                <TextBlock Text="Index: " FontWeight="Bold" Grid.Row="3" Grid.Column="0"/>
                <TextBlock x:Name="indexText" FontWeight="Bold" Grid.Row="3" Grid.Column="1"/>
            </Grid>
        </Border>
    </Popup>
</Grid>
```

**C#:**
```csharp
private void Chart_SelectionChanged(object sender, SunburstSelectionChangedEventArgs e)
{
    selectionPopup.IsOpen = true;
    categoryText.Text = e.SelectedSegment.Category.ToString();
    valueText.Text = e.SelectedSegment.Value.ToString();
    levelText.Text = e.SelectedSegment.CurrentLevel.ToString();
    indexText.Text = e.SelectedSegment.SliceIndex.ToString();
}
```

### Example 2: Update Dashboard

Update other UI elements based on selection:

```csharp
private void Chart_SelectionChanged(object sender, SunburstSelectionChangedEventArgs e)
{
    // Update text blocks
    SelectedCategoryLabel.Text = $"Selected: {e.SelectedSegment.Category}";
    SelectedValueLabel.Text = $"Value: {e.SelectedSegment.Value:N0}";
    
    // Update progress bar
    double maxValue = 200; // Your max value
    double percentage = (e.SelectedSegment.Value / maxValue) * 100;
    ValueProgressBar.Value = percentage;
    
    // Load detailed data for this segment
    LoadDetailedView(e.SelectedSegment.Category);
}

private void LoadDetailedView(string category)
{
    // Load detailed data grid, charts, etc.
    DetailDataGrid.ItemsSource = GetDetailedData(category);
}
```

### Example 3: Track Selection History

Maintain a selection history log:

```csharp
private ObservableCollection<SelectionInfo> _selectionHistory = new ObservableCollection<SelectionInfo>();

private void Chart_SelectionChanged(object sender, SunburstSelectionChangedEventArgs e)
{
    var selectionInfo = new SelectionInfo
    {
        Timestamp = DateTime.Now,
        Category = e.SelectedSegment.Category,
        Value = e.SelectedSegment.Value,
        Level = e.SelectedSegment.CurrentLevel
    };
    
    _selectionHistory.Insert(0, selectionInfo); // Add to top
    
    // Keep only last 10 selections
    if (_selectionHistory.Count > 10)
        _selectionHistory.RemoveAt(10);
    
    // Update history list in UI
    SelectionHistoryList.ItemsSource = _selectionHistory;
}

public class SelectionInfo
{
    public DateTime Timestamp { get; set; }
    public string Category { get; set; }
    public double Value { get; set; }
    public int Level { get; set; }
}
```

### Example 4: Filter Related Data

Filter other views based on selection:

```csharp
private void Chart_SelectionChanged(object sender, SunburstSelectionChangedEventArgs e)
{
    string selectedCategory = e.SelectedSegment.Category;
    
    // Filter data grid
    var filteredData = _allData.Where(item => 
        item.Category == selectedCategory || 
        item.ParentCategory == selectedCategory
    ).ToList();
    
    DataGrid.ItemsSource = filteredData;
    
    // Update related charts
    UpdateRelatedCharts(selectedCategory);
    
    // Log analytics
    LogSelectionAnalytics(selectedCategory, e.SelectedSegment.Value);
}
```

## Combining Events

Use both events together for comprehensive customization:

```csharp
// Customize segments as they're created
private void Chart_SegmentCreated(object sender, SunburstSegmentCreatedEventArgs e)
{
    // Apply level-based colors
    Color[] levelColors = {
        Colors.CornflowerBlue,  // Level 0
        Colors.LightCoral,       // Level 1
        Colors.MediumSeaGreen,  // Level 2
        Colors.Gold              // Level 3
    };
    
    if (e.Segment.CurrentLevel < levelColors.Length)
    {
        e.Segment.Interior = new SolidColorBrush(levelColors[e.Segment.CurrentLevel]);
    }
}

// Respond to user interactions
private void Chart_SelectionChanged(object sender, SunburstSelectionChangedEventArgs e)
{
    // Update status bar
    StatusText.Text = $"Selected {e.SelectedSegment.Category} " +
                      $"(Level {e.SelectedSegment.CurrentLevel + 1}): " +
                      $"{e.SelectedSegment.Value:N0} items";
    
    // Update detail panel
    DetailPanel.DataContext = GetDetailedInfo(e.SelectedSegment.Category);
}
```

## Best Practices

### SegmentCreated Event

1. **Performance:** Keep logic lightweight; this fires for every segment
2. **Consistent styling:** Use level-based or value-based rules for consistency
3. **Color accessibility:** Ensure sufficient contrast for readability
4. **Avoid complex calculations:** Pre-calculate values if possible
5. **Test with large datasets:** Verify performance with realistic data volumes

### SelectionChanged Event

1. **Null checks:** Always verify SelectedSegment is not null
2. **UI updates:** Use Dispatcher for thread-safe UI updates if needed
3. **Debouncing:** Consider debouncing rapid selections (hover mode)
4. **State management:** Track selection state for undo/redo functionality
5. **Analytics:** Log selections for user behavior analysis

### General Guidelines

- Separate concerns: Keep event handlers focused on single responsibilities
- Use MVVM: Consider moving logic to ViewModel with commands
- Error handling: Wrap event logic in try-catch for robustness
- Documentation: Comment complex event logic for maintainability
- Testing: Create unit tests for event handler logic

## Troubleshooting

**SegmentCreated not firing:**
- Verify event is subscribed in XAML or code-behind
- Check that ItemsSource has data
- Ensure chart is rendered (visible in UI)

**SelectionChanged not firing:**
- Verify SelectionBehavior is added
- Check EnableSelection="True"
- Ensure segments are selectable (not disabled)
- Confirm SelectionMode is set correctly

**Event firing multiple times:**
- Normal for SegmentCreated (once per segment)
- For SelectionChanged, check if selection is toggling rapidly
- Consider debouncing for hover-based selection

**Null reference exceptions:**
- Always check e.SelectedSegment for null
- Verify segment properties before accessing
- Use null-conditional operators (?.)

**Performance issues:**
- Simplify SegmentCreated logic
- Avoid heavy operations in event handlers
- Consider async operations for time-consuming tasks
- Profile and optimize hotspots

## Complete Example

Comprehensive event handling:

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.DataContext = new ViewModel();
    }
    
    // Customize segments as created
    private void SunburstChart_SegmentCreated(object sender, SunburstSegmentCreatedEventArgs e)
    {
        // Color intensity based on value
        double normalizedValue = e.Segment.Value / 200.0; // Max value
        byte intensity = (byte)(normalizedValue * 200 + 55);
        
        switch (e.Segment.CurrentLevel)
        {
            case 0: // Root level - Blue shades
                e.Segment.Interior = new SolidColorBrush(Color.FromRgb(0, 0, intensity));
                break;
            case 1: // Second level - Green shades
                e.Segment.Interior = new SolidColorBrush(Color.FromRgb(0, intensity, 0));
                break;
            case 2: // Third level - Red shades
                e.Segment.Interior = new SolidColorBrush(Color.FromRgb(intensity, 0, 0));
                break;
            default:
                // Use default coloring
                break;
        }
        
        // Add border to high-value segments
        if (e.Segment.Value > 100)
        {
            e.Segment.Stroke = new SolidColorBrush(Colors.Black);
            e.Segment.StrokeThickness = 2;
        }
    }
    
    // Handle selection changes
    private void SunburstChart_SelectionChanged(object sender, SunburstSelectionChangedEventArgs e)
    {
        if (e.SelectedSegment != null)
        {
            // Update UI elements
            StatusLabel.Text = $"Selected: {e.SelectedSegment.Category}";
            ValueLabel.Text = $"Value: {e.SelectedSegment.Value:N2}";
            LevelLabel.Text = $"Level: {e.SelectedSegment.CurrentLevel + 1}";
            
            // Load detailed information
            LoadSegmentDetails(e.SelectedSegment);
            
            // Log for analytics
            LogSelection(e.SelectedSegment.Category, e.SelectedSegment.Value);
        }
    }
    
    private void LoadSegmentDetails(SunburstSegment segment)
    {
        // Implementation here
    }
    
    private void LogSelection(string category, double value)
    {
        // Analytics logging
        Debug.WriteLine($"User selected: {category}, Value: {value}");
    }
}
```

This completes the event handling documentation for the Sunburst Chart!

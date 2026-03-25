# Collapse and Expand Features

This guide covers the collapse and expand functionality of SfGridSplitter, allowing users to hide and show panels interactively with built-in collapse buttons.

## EnableCollapseButton Property

The EnableCollapseButton property controls the visibility of collapse/expand buttons on the splitter.

### Basic Collapse Button Setup

Enable collapse buttons to allow users to hide adjacent panels:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <TextBlock Grid.Row="0" 
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center"
                   Text="Panel 1"/>
        
        <TextBlock Grid.Row="2"
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center" 
                   Text="Panel 2"/>

        <!-- Grid Splitter with collapse buttons -->
        <syncfusion:SfGridSplitter EnableCollapseButton="True"
                                   HorizontalAlignment="Stretch"
                                   Height="8"
                                   Grid.Row="1"
                                   ResizeBehavior="PreviousAndNext"/>
    </Grid>
</Border>
```

**Key Points:**
- **EnableCollapseButton:** Set to `True` to display buttons
- **Height/Width:** Increase size (e.g., 8-10 pixels) to accommodate buttons
- **Buttons:** Appear as arrows pointing in collapse directions

### Default Behavior

When EnableCollapseButton is true:

**For Horizontal Splitters:**
- **Up arrow button:** Collapses the panel above the splitter
- **Down arrow button:** Collapses the panel below the splitter
- Click again to expand the collapsed panel

**For Vertical Splitters:**
- **Left arrow button:** Collapses the panel to the left
- **Right arrow button:** Collapses the panel to the right
- Click again to expand the collapsed panel

## Collapsing and Expanding Panels

### Horizontal Splitter Collapse

Collapse rows using horizontal splitter:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" MinHeight="0"/>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" MinHeight="0"/>
        </Grid.RowDefinitions>
        
        <Border Grid.Row="0" Background="LightBlue" Padding="10">
            <TextBlock Text="Top Panel - Can be collapsed"
                       VerticalAlignment="Center"
                       HorizontalAlignment="Center"/>
        </Border>
        
        <Border Grid.Row="2" Background="LightGreen" Padding="10">
            <TextBlock Text="Bottom Panel - Can be collapsed"
                       VerticalAlignment="Center"
                       HorizontalAlignment="Center"/>
        </Border>

        <!-- Horizontal splitter with collapse buttons -->
        <syncfusion:SfGridSplitter EnableCollapseButton="True"
                                   Grid.Row="1"
                                   HorizontalAlignment="Stretch"
                                   Height="8"
                                   Background="DarkGray"
                                   ResizeBehavior="PreviousAndNext"/>
    </Grid>
</Border>
```

**Collapse Behavior:**
- Clicking up arrow: Row 0 collapses to height 0
- Clicking down arrow: Row 2 collapses to height 0
- Button changes to expand indicator when panel is collapsed
- Clicking expand button restores original size

**Important:** Set `MinHeight="0"` on rows to allow full collapse.

### Vertical Splitter Collapse

Collapse columns using vertical splitter:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" MinWidth="0"/>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" MinWidth="0"/>
        </Grid.ColumnDefinitions>
        
        <Border Grid.Column="0" Background="LightCoral" Padding="10">
            <TextBlock Text="Left Panel"
                       VerticalAlignment="Center"
                       HorizontalAlignment="Center"/>
        </Border>
        
        <Border Grid.Column="2" Background="LightYellow" Padding="10">
            <TextBlock Text="Right Panel"
                       VerticalAlignment="Center"
                       HorizontalAlignment="Center"/>
        </Border>

        <!-- Vertical splitter with collapse buttons -->
        <syncfusion:SfGridSplitter EnableCollapseButton="True"
                                   Grid.Column="1"
                                   VerticalAlignment="Stretch"
                                   Width="8"
                                   Background="DarkGray"
                                   ResizeBehavior="PreviousAndNext"/>
    </Grid>
</Border>
```

**Important:** Set `MinWidth="0"` on columns to allow full collapse.

## Custom Collapse Button Templates

Customize the appearance of collapse buttons using template properties.

### Button Template Properties

SfGridSplitter provides four template properties for button customization:

- **UpButtonTemplate:** Button for collapsing/expanding panel above
- **DownButtonTemplate:** Button for collapsing/expanding panel below
- **LeftButtonTemplate:** Button for collapsing/expanding panel to left
- **RightButtonTemplate:** Button for collapsing/expanding panel to right

### Custom Horizontal Splitter Buttons

Customize up and down buttons:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    
    <Border Grid.Row="0" Background="LightBlue"/>
    <Border Grid.Row="2" Background="LightGreen"/>
    
    <syncfusion:SfGridSplitter Grid.Row="1"
                               Height="25"
                               HorizontalAlignment="Stretch"
                               EnableCollapseButton="True"
                               ResizeBehavior="PreviousAndNext"
                               Background="LightGray">
        
        <!-- Custom Up Button -->
        <syncfusion:SfGridSplitter.UpButtonTemplate>
            <DataTemplate>
                <Ellipse Width="20" Height="20" 
                         Fill="Blue"
                         ToolTip="Collapse Top Panel"/>
            </DataTemplate>
        </syncfusion:SfGridSplitter.UpButtonTemplate>

        <!-- Custom Down Button -->
        <syncfusion:SfGridSplitter.DownButtonTemplate>
            <DataTemplate>
                <Ellipse Width="20" Height="20" 
                         Fill="Orange"
                         ToolTip="Collapse Bottom Panel"/>
            </DataTemplate>
        </syncfusion:SfGridSplitter.DownButtonTemplate>
    </syncfusion:SfGridSplitter>
</Grid>
```

### Custom Vertical Splitter Buttons

Customize left and right buttons:

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="*" />
    </Grid.ColumnDefinitions>
    
    <Border Grid.Column="0" Background="LightCoral"/>
    <Border Grid.Column="2" Background="LightYellow"/>
    
    <syncfusion:SfGridSplitter Grid.Column="1"
                               Width="25"
                               VerticalAlignment="Stretch"
                               EnableCollapseButton="True"
                               ResizeBehavior="PreviousAndNext"
                               Background="LightGray">
        
        <!-- Custom Left Button -->
        <syncfusion:SfGridSplitter.LeftButtonTemplate>
            <DataTemplate>
                <Ellipse Width="20" Height="20" 
                         Fill="Red"
                         ToolTip="Collapse Left Panel"/>
            </DataTemplate>
        </syncfusion:SfGridSplitter.LeftButtonTemplate>

        <!-- Custom Right Button -->
        <syncfusion:SfGridSplitter.RightButtonTemplate>
            <DataTemplate>
                <Ellipse Width="20" Height="20" 
                         Fill="Green"
                         ToolTip="Collapse Right Panel"/>
            </DataTemplate>
        </syncfusion:SfGridSplitter.RightButtonTemplate>
    </syncfusion:SfGridSplitter>
</Grid>
```

### Advanced Custom Button Design

Create sophisticated button designs with icons and effects:

```xml
<syncfusion:SfGridSplitter EnableCollapseButton="True"
                           Grid.Row="1"
                           Height="30"
                           HorizontalAlignment="Stretch">
    
    <!-- Styled Up Button with Icon -->
    <syncfusion:SfGridSplitter.UpButtonTemplate>
        <DataTemplate>
            <Border Background="#FF4A90E2"
                    CornerRadius="3"
                    Width="24" Height="24"
                    BorderBrush="White"
                    BorderThickness="1">
                <Path Data="M 8,12 L 12,8 L 16,12" 
                      Stroke="White" 
                      StrokeThickness="2"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center"/>
            </Border>
        </DataTemplate>
    </syncfusion:SfGridSplitter.UpButtonTemplate>

    <!-- Styled Down Button with Icon -->
    <syncfusion:SfGridSplitter.DownButtonTemplate>
        <DataTemplate>
            <Border Background="#FFE24A4A"
                    CornerRadius="3"
                    Width="24" Height="24"
                    BorderBrush="White"
                    BorderThickness="1">
                <Path Data="M 8,8 L 12,12 L 16,8" 
                      Stroke="White" 
                      StrokeThickness="2"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center"/>
            </Border>
        </DataTemplate>
    </syncfusion:SfGridSplitter.DownButtonTemplate>
</syncfusion:SfGridSplitter>
```

## Complete Multi-Splitter Example

Combine horizontal and vertical splitters with custom collapse buttons:

```xml
<Border Margin="10" BorderBrush="DarkGray" BorderThickness="1">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        
        <!-- Top Left Panel -->
        <Border Grid.Row="0" Grid.Column="0" 
                Background="LightBlue" Padding="10">
            <TextBlock Text="Panel 1" 
                       HorizontalAlignment="Center" 
                       VerticalAlignment="Center"/>
        </Border>
        
        <!-- Bottom Left Panel -->
        <Border Grid.Row="2" Grid.Column="0" 
                Background="LightGreen" Padding="10">
            <TextBlock Text="Panel 2" 
                       HorizontalAlignment="Center" 
                       VerticalAlignment="Center"/>
        </Border>
        
        <!-- Right Panel (spans all rows) -->
        <Border Grid.RowSpan="3" Grid.Column="2" 
                Background="LightYellow" Padding="10">
            <TextBlock Text="Panel 3" 
                       HorizontalAlignment="Center" 
                       VerticalAlignment="Center"/>
        </Border>

        <!-- Horizontal Splitter (left side) -->
        <syncfusion:SfGridSplitter Grid.Row="1" Grid.Column="0"
                                   Height="8"
                                   HorizontalAlignment="Stretch"
                                   EnableCollapseButton="True"
                                   ResizeBehavior="PreviousAndNext">
            
            <syncfusion:SfGridSplitter.UpButtonTemplate>
                <DataTemplate>
                    <Ellipse Width="20" Height="20" Fill="Blue"/>
                </DataTemplate>
            </syncfusion:SfGridSplitter.UpButtonTemplate>

            <syncfusion:SfGridSplitter.DownButtonTemplate>
                <DataTemplate>
                    <Ellipse Width="20" Height="20" Fill="Orange"/>
                </DataTemplate>
            </syncfusion:SfGridSplitter.DownButtonTemplate>
        </syncfusion:SfGridSplitter>

        <!-- Vertical Splitter (spans all rows) -->
        <syncfusion:SfGridSplitter Grid.RowSpan="3" Grid.Column="1"
                                   Width="8"
                                   VerticalAlignment="Stretch"
                                   EnableCollapseButton="True"
                                   ResizeBehavior="PreviousAndNext">
            
            <syncfusion:SfGridSplitter.LeftButtonTemplate>
                <DataTemplate>
                    <Ellipse Width="20" Height="20" Fill="Red"/>
                </DataTemplate>
            </syncfusion:SfGridSplitter.LeftButtonTemplate>

            <syncfusion:SfGridSplitter.RightButtonTemplate>
                <DataTemplate>
                    <Ellipse Width="20" Height="20" Fill="Green"/>
                </DataTemplate>
            </syncfusion:SfGridSplitter.RightButtonTemplate>
        </syncfusion:SfGridSplitter>
    </Grid>
</Border>
```

## User Interaction Patterns

### Pattern 1: IDE-Style Collapsible Panels

Emulate IDE layouts with optional side panels:

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="200" MinWidth="0"/> <!-- Toolbox -->
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>                  <!-- Editor -->
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="250" MinWidth="0"/> <!-- Properties -->
    </Grid.ColumnDefinitions>
    
    <!-- Toolbox Panel -->
    <Border Grid.Column="0" Background="#FF2D2D30">
        <TextBlock Text="Toolbox" Foreground="White" Margin="10"/>
    </Border>
    
    <!-- Editor Panel -->
    <Border Grid.Column="2" Background="White">
        <TextBlock Text="Code Editor" Margin="10"/>
    </Border>
    
    <!-- Properties Panel -->
    <Border Grid.Column="4" Background="#FF2D2D30">
        <TextBlock Text="Properties" Foreground="White" Margin="10"/>
    </Border>
    
    <!-- Left Splitter -->
    <syncfusion:SfGridSplitter Grid.Column="1"
                               VerticalAlignment="Stretch"
                               Width="5"
                               EnableCollapseButton="True"
                               Background="#FF007ACC"/>
    
    <!-- Right Splitter -->
    <syncfusion:SfGridSplitter Grid.Column="3"
                               VerticalAlignment="Stretch"
                               Width="5"
                               EnableCollapseButton="True"
                               Background="#FF007ACC"/>
</Grid>
```

**Benefit:** Users can collapse side panels to maximize editor space.

### Pattern 2: Collapsible Navigation Panel

Main content with collapsible navigation:

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="250" MinWidth="0"/>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    
    <!-- Navigation -->
    <TreeView Grid.Column="0"/>
    
    <!-- Content -->
    <ContentControl Grid.Column="2"/>
    
    <!-- Splitter with Left Collapse Only -->
    <syncfusion:SfGridSplitter Grid.Column="1"
                               VerticalAlignment="Stretch"
                               Width="8"
                               EnableCollapseButton="True"
                               ResizeBehavior="PreviousAndNext">
        
        <!-- Hide right button (content shouldn't collapse) -->
        <syncfusion:SfGridSplitter.RightButtonTemplate>
            <DataTemplate>
                <Grid Visibility="Collapsed"/>
            </DataTemplate>
        </syncfusion:SfGridSplitter.RightButtonTemplate>
    </syncfusion:SfGridSplitter>
</Grid>
```

**Benefit:** Only navigation collapses; main content always visible.

## Important Considerations

### MinHeight and MinWidth Requirements

For full collapse functionality, set minimums to zero:

```xml
<!-- Allow full collapse -->
<RowDefinition Height="*" MinHeight="0"/>

<!-- Prevent full collapse (minimum 100px) -->
<RowDefinition Height="*" MinHeight="100"/>
```

### Button Template Visibility

Custom button templates are only visible when:
- `EnableCollapseButton="True"`
- Splitter has sufficient size to display buttons
- Templates are properly defined in DataTemplate

### Button Size Recommendations

Increase splitter size to accommodate collapse buttons:

- **Default splitter:** 3-5 pixels (no buttons)
- **With collapse buttons:** 8-12 pixels (standard buttons)
- **Custom buttons:** 20-30 pixels (larger custom designs)

### Template Application Scope

- **UpButtonTemplate / DownButtonTemplate:** Used for horizontal splitters only
- **LeftButtonTemplate / RightButtonTemplate:** Used for vertical splitters only
- Setting templates for wrong orientation has no effect

## Troubleshooting

### Collapse Buttons Not Showing

**Problem:** EnableCollapseButton is true but buttons don't appear.

**Solutions:**
- Increase Height (horizontal) or Width (vertical) of splitter to 8+ pixels
- Verify EnableCollapseButton is set to `True`
- Check that splitter is properly placed in Grid structure

### Panels Not Collapsing Fully

**Problem:** Clicking collapse button partially hides panel.

**Solution:** Set `MinHeight="0"` or `MinWidth="0"` on row/column definitions:

```xml
<Grid.RowDefinitions>
    <RowDefinition Height="*" MinHeight="0"/>
</Grid.RowDefinitions>
```

### Custom Buttons Not Visible

**Problem:** Custom button templates defined but not showing.

**Solutions:**
- Ensure DataTemplate contains visible elements (not collapsed)
- Verify template properties match splitter orientation
- Check splitter size accommodates button dimensions

### Button Template Not Changing on Collapse

**Problem:** Button appearance doesn't update when panel is collapsed/expanded.

**Note:** Syncfusion manages button state internally. Custom templates apply to both collapsed and expanded states. For state-specific designs, use triggers or binding in the template.

## Best Practices

1. **Size Appropriately:** Make splitters 8-10 pixels when using collapse buttons
2. **Set Minimums:** Use `MinHeight="0"` / `MinWidth="0"` for full collapse
3. **Visual Feedback:** Use tooltips on custom buttons to indicate functionality
4. **Consistent Design:** Match button styles with application theme
5. **Accessibility:** Ensure buttons are large enough for easy clicking (20x20 minimum)
6. **Test Interactions:** Verify collapse/expand works in both directions

## Next Steps

Explore splitter customization:
- **Customization and Styling:** Learn about gripper templates, preview styles, and visual customization

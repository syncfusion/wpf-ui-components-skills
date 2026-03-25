# Navigation Features in WPF Carousel

## Table of Contents
- [Overview](#overview)
- [Keyboard Navigation](#keyboard-navigation)
- [Scroll Bar Navigation](#scroll-bar-navigation)
- [Mouse Wheel Navigation](#mouse-wheel-navigation)
- [Navigation Commands and Methods](#navigation-commands-and-methods)
- [Looping Items in Custom Path](#looping-items-in-custom-path)
- [Navigation Patterns](#navigation-patterns)

## Overview

The Carousel control provides multiple navigation methods to browse through items: keyboard shortcuts, mouse wheel, scroll bars, and programmatic commands. This flexibility ensures users can navigate carousel items in the way most natural to their workflow.

**Available Navigation Methods:**
- **Keyboard**: Arrow keys, Home, End, Page Up/Down
- **Mouse**: Clicking items, mouse wheel scrolling
- **Scroll Bars**: Horizontal and vertical scroll bars
- **Commands**: Built-in commands for programmatic control
- **Methods**: Direct method calls for custom logic

## Keyboard Navigation

Navigate carousel items using keyboard shortcuts. This is the primary navigation method for accessibility and power users.

### Setup for Keyboard Navigation

```csharp
// Model.cs
public class Model
{
    public string Header { get; set; }
}

// ViewModel.cs
using System.Collections.ObjectModel;

public class ViewModel
{
    private ObservableCollection<Model> carouselItem;
    
    public ObservableCollection<Model> CarouselItem
    {
        get { return carouselItem; }
        set { carouselItem = value; }
    }
    
    public ViewModel()
    {
        CarouselItem = new ObservableCollection<Model>();
        for (int i = 1; i <= 10; i++)
        {
            CarouselItem.Add(new Model() { Header = $"Item{i}" });
        }
    }
}
```

### XAML Configuration

```xml
<syncfusion:Carousel Name="carousel"
                     ItemsSource="{Binding CarouselItem}">
    <syncfusion:Carousel.DataContext>
        <local:ViewModel/>
    </syncfusion:Carousel.DataContext>
    
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <Border Height="60"
                    Width="80"
                    BorderBrush="Purple"
                    BorderThickness="3"
                    Background="LightBlue">
                <TextBlock Text="{Binding Header}" 
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"
                           FontSize="14"
                           FontWeight="Bold"/>
            </Border>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

### Keyboard Shortcuts

| Key(s) | Action | Description |
|--------|--------|-------------|
| **Up**, **Left** | Previous Item | Navigate to the previous item from currently selected |
| **Down**, **Right** | Next Item | Navigate to the next item from currently selected |
| **Home**, **Ctrl+Up**, **Ctrl+Left** | First Item | Jump to the first item |
| **End**, **Ctrl+Down**, **Ctrl+Right** | Last Item | Jump to the last item |
| **Page Up** | Previous Page | Navigate to previous page (based on ItemsPerPage) |
| **Page Down** | Next Page | Navigate to next page (based on ItemsPerPage) |

### Navigation Examples

**Navigate to Previous Item:**
- Press `Left Arrow` or `Up Arrow`
- Item selection moves backward by one position

**Navigate to Next Item:**
- Press `Right Arrow` or `Down Arrow`
- Item selection moves forward by one position

**Jump to First Item:**
- Press `Home`, `Ctrl+Up`, or `Ctrl+Left`
- Selection jumps to the first carousel item

**Jump to Last Item:**
- Press `End`, `Ctrl+Down`, or `Ctrl+Right`
- Selection jumps to the last carousel item

**Navigate by Page:**
- Press `Page Up`: Move backward by `ItemsPerPage` items
- Press `Page Down`: Move forward by `ItemsPerPage` items

**Example with ItemsPerPage:**
```xml
<syncfusion:Carousel ItemsPerPage="5"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding CarouselItem}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L100,0"/>
    </syncfusion:Carousel.Path>
</syncfusion:Carousel>
```

When `ItemsPerPage="5"`, pressing `Page Down` navigates 5 items forward.

## Scroll Bar Navigation

Enable scroll bars to navigate through carousel items using mouse dragging or clicking.

### Enable Scroll Bars

By default, scroll bars are collapsed. Enable them using `ScrollViewer` attached properties:

```xml
<syncfusion:Carousel ScrollViewer.VerticalScrollBarVisibility="Visible" 
                     ScrollViewer.HorizontalScrollBarVisibility="Visible"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding CarouselItem}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L100,0" />
    </syncfusion:Carousel.Path>
    
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <Border Height="100"
                    Width="100"
                    BorderBrush="Purple"
                    BorderThickness="3"
                    Background="LightBlue">
                <TextBlock Text="{Binding Header}" 
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"/>
            </Border>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

### Code-Behind Configuration

```csharp
using System.Windows.Controls;

ScrollViewer.SetHorizontalScrollBarVisibility(carousel, ScrollBarVisibility.Visible);
ScrollViewer.SetVerticalScrollBarVisibility(carousel, ScrollBarVisibility.Visible);
```

### ScrollBarVisibility Options

| Value | Description |
|-------|-------------|
| `Visible` | Scroll bar is always visible |
| `Auto` | Scroll bar appears automatically when needed |
| `Hidden` | Scroll bar is hidden but scrolling still works |
| `Disabled` | Scroll bar is hidden and scrolling is disabled |

### When to Use Scroll Bars

- **Custom path mode**: Most useful with `VisualMode="CustomPath"`
- **Linear layouts**: Horizontal or vertical item arrangements
- **Mouse-centric users**: Users who prefer mouse over keyboard
- **Touch interfaces**: Better for touch-enabled devices

**Note:** Scroll bars are most effective in CustomPath mode. In Standard mode (circular), they may be less intuitive.

## Mouse Wheel Navigation

Navigate carousel items by scrolling the mouse wheel forward or backward.

### Mouse Wheel Behavior

- **Scroll Forward**: Navigate to next item (same as Down/Right arrow)
- **Scroll Backward**: Navigate to previous item (same as Up/Left arrow)
- **Smooth Scrolling**: Animated transition between items

### Enable Mouse Wheel Navigation

Mouse wheel navigation is enabled by default. Ensure the Carousel has focus:

```xml
<syncfusion:Carousel Focusable="True"
                     FocusVisualStyle="{x:Null}"
                     ItemsSource="{Binding Items}"/>
```

### Example with Mouse Wheel

```xml
<syncfusion:Carousel Name="carousel"
                     Height="400"
                     Width="600"
                     Focusable="True"
                     ItemsSource="{Binding CarouselItem}">
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <Border Height="80" Width="120" 
                    BorderBrush="Navy" 
                    BorderThickness="2"
                    Background="AliceBlue">
                <TextBlock Text="{Binding Header}" 
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"
                           FontSize="16"/>
            </Border>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

**Best Practices:**
- Ensure carousel has visual focus indication
- Combine with keyboard navigation for accessibility
- Works well with both Standard and CustomPath modes

## Navigation Commands and Methods

The Carousel control provides built-in commands and methods for programmatic navigation.

### Available Commands

| Command | Method Equivalent | Description |
|---------|-------------------|-------------|
| `SelectFirstItemCommand` | `SelectFirstItem()` | Select the first item |
| `SelectLastItemCommand` | `SelectLastItem()` | Select the last item |
| `SelectNextItemCommand` | `SelectNextItem()` | Select the next item |
| `SelectPreviousItemCommand` | `SelectPreviousItem()` | Select the previous item |
| `SelectNextPageCommand` | `SelectNextPage()` | Select next page item |
| `SelectPreviousPageCommand` | `SelectPreviousPage()` | Select previous page item |

### Using Commands in XAML

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <!-- Carousel -->
    <syncfusion:Carousel Name="carousel" 
                         Grid.Row="0"
                         ItemsSource="{Binding CarouselItem}"/>
    
    <!-- Navigation Buttons -->
    <StackPanel Grid.Row="1" 
                Orientation="Horizontal" 
                HorizontalAlignment="Center"
                Margin="10">
        <Button Content="First" 
                Command="{Binding ElementName=carousel, Path=SelectFirstItemCommand}"
                Width="80" Margin="5"/>
        <Button Content="Previous" 
                Command="{Binding ElementName=carousel, Path=SelectPreviousItemCommand}"
                Width="80" Margin="5"/>
        <Button Content="Next" 
                Command="{Binding ElementName=carousel, Path=SelectNextItemCommand}"
                Width="80" Margin="5"/>
        <Button Content="Last" 
                Command="{Binding ElementName=carousel, Path=SelectLastItemCommand}"
                Width="80" Margin="5"/>
    </StackPanel>
</Grid>
```

### Using Methods in Code-Behind

```csharp
// Navigate to first item
carousel.SelectFirstItem();

// Navigate to last item
carousel.SelectLastItem();

// Navigate to previous item
carousel.SelectPreviousItem();

// Navigate to next item
carousel.SelectNextItem();

// Navigate to previous page
carousel.SelectPreviousPage();

// Navigate to next page
carousel.SelectNextPage();

// Direct index selection
carousel.SelectedIndex = 3;
```

### Execute Commands Programmatically

```csharp
// Execute command from code
carousel.SelectFirstItemCommand.Execute(null);
carousel.SelectNextItemCommand.Execute(null);
carousel.SelectPreviousPageCommand.Execute(null);
```

### Custom Navigation Logic

```csharp
private void NavigateToItem(int index)
{
    if (index >= 0 && index < carousel.Items.Count)
    {
        carousel.SelectedIndex = index;
    }
}

private void NavigateRelative(int offset)
{
    int newIndex = carousel.SelectedIndex + offset;
    
    // Wrap around if needed
    if (newIndex < 0)
        newIndex = carousel.Items.Count - 1;
    else if (newIndex >= carousel.Items.Count)
        newIndex = 0;
    
    carousel.SelectedIndex = newIndex;
}

// Navigate forward by 3 items
private void SkipForward()
{
    NavigateRelative(3);
}

// Navigate backward by 2 items
private void SkipBackward()
{
    NavigateRelative(-2);
}
```

## Looping Items in Custom Path

In CustomPath mode, items scroll linearly and stop at the first/last item. Enable looping to create circular navigation that wraps around.

### Enable Looping

```xml
<syncfusion:Carousel EnableLooping="True"
                     VisualMode="CustomPath"
                     ItemsSource="{Binding CarouselItem}">
    <syncfusion:Carousel.Path>
        <Path Data="M0,0 L500,0"/>
    </syncfusion:Carousel.Path>
    
    <syncfusion:Carousel.ItemTemplate>
        <DataTemplate>
            <Border Height="80" Width="100"
                    BorderBrush="Purple"
                    BorderThickness="2"
                    Background="LightBlue">
                <TextBlock Text="{Binding Header}" 
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"/>
            </Border>
        </DataTemplate>
    </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

```csharp
carousel.EnableLooping = true;
carousel.VisualMode = VisualMode.CustomPath;
```

### Looping Behavior

**Without Looping (EnableLooping="False"):**
- Navigate forward from last item: Stops at last item
- Navigate backward from first item: Stops at first item
- Linear navigation with endpoints

**With Looping (EnableLooping="True"):**
- Navigate forward from last item: Wraps to first item
- Navigate backward from first item: Wraps to last item
- Circular navigation without endpoints

### When to Use Looping

**Use looping when:**
- Creating infinite carousel experiences
- Displaying repeating content (e.g., product carousel)
- User should never reach an "end"
- Gallery or slideshow interfaces

**Don't use looping when:**
- Displaying sequential content (e.g., tutorial steps)
- Clear start and end points are needed
- Navigation context is important
- Timeline or chronological displays

**Note:** `EnableLooping` only works in `VisualMode="CustomPath"`. In Standard mode, carousel is naturally circular.

## Navigation Patterns

### Pattern 1: Arrow Button Navigation

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
        <ColumnDefinition Width="Auto"/>
    </Grid.ColumnDefinitions>
    
    <Button Grid.Column="0" 
            Content="◄" 
            FontSize="24"
            Width="50"
            Command="{Binding ElementName=carousel, Path=SelectPreviousItemCommand}"/>
    
    <syncfusion:Carousel Grid.Column="1" 
                         Name="carousel"
                         ItemsSource="{Binding Items}"/>
    
    <Button Grid.Column="2" 
            Content="►" 
            FontSize="24"
            Width="50"
            Command="{Binding ElementName=carousel, Path=SelectNextItemCommand}"/>
</Grid>
```

### Pattern 2: Thumbnail Navigation

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <!-- Main carousel -->
    <syncfusion:Carousel Name="mainCarousel" 
                         Grid.Row="0"
                         ItemsSource="{Binding Images}"/>
    
    <!-- Thumbnail list -->
    <ListBox Grid.Row="1" 
             Height="100"
             ItemsSource="{Binding Images}"
             SelectedIndex="{Binding ElementName=mainCarousel, Path=SelectedIndex, Mode=TwoWay}"
             ScrollViewer.HorizontalScrollBarVisibility="Auto"
             ScrollViewer.VerticalScrollBarVisibility="Disabled">
        <ListBox.ItemsPanel>
            <ItemsPanelTemplate>
                <StackPanel Orientation="Horizontal"/>
            </ItemsPanelTemplate>
        </ListBox.ItemsPanel>
        <ListBox.ItemTemplate>
            <DataTemplate>
                <Image Source="{Binding}" 
                       Width="80" 
                       Height="80" 
                       Margin="5"/>
            </DataTemplate>
        </ListBox.ItemTemplate>
    </ListBox>
</Grid>
```

### Pattern 3: Indicator Dots

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <syncfusion:Carousel Name="carousel" 
                         Grid.Row="0"
                         ItemsSource="{Binding Items}"/>
    
    <!-- Indicator dots -->
    <ItemsControl Grid.Row="1" 
                  ItemsSource="{Binding Items}"
                  HorizontalAlignment="Center"
                  Margin="10">
        <ItemsControl.ItemsPanel>
            <ItemsPanelTemplate>
                <StackPanel Orientation="Horizontal"/>
            </ItemsPanelTemplate>
        </ItemsControl.ItemsPanel>
        <ItemsControl.ItemTemplate>
            <DataTemplate>
                <Ellipse Width="12" Height="12" 
                         Margin="3"
                         Fill="Gray"
                         Cursor="Hand">
                    <Ellipse.Style>
                        <Style TargetType="Ellipse">
                            <Style.Triggers>
                                <DataTrigger Binding="{Binding IsSelected}" Value="True">
                                    <Setter Property="Fill" Value="DodgerBlue"/>
                                </DataTrigger>
                            </Style.Triggers>
                        </Style>
                    </Ellipse.Style>
                </Ellipse>
            </DataTemplate>
        </ItemsControl.ItemTemplate>
    </ItemsControl>
</Grid>
```

### Pattern 4: Timer-Based Auto-Navigation

```csharp
using System.Windows.Threading;

private DispatcherTimer autoNavigateTimer;

private void StartAutoNavigation()
{
    autoNavigateTimer = new DispatcherTimer();
    autoNavigateTimer.Interval = TimeSpan.FromSeconds(3);
    autoNavigateTimer.Tick += AutoNavigateTimer_Tick;
    autoNavigateTimer.Start();
}

private void AutoNavigateTimer_Tick(object sender, EventArgs e)
{
    carousel.SelectNextItem();
    
    // Loop back to first if at end (without EnableLooping)
    if (carousel.SelectedIndex == carousel.Items.Count - 1)
    {
        carousel.SelectedIndex = 0;
    }
}

private void StopAutoNavigation()
{
    autoNavigateTimer?.Stop();
}

// Pause on mouse enter
private void Carousel_MouseEnter(object sender, MouseEventArgs e)
{
    StopAutoNavigation();
}

// Resume on mouse leave
private void Carousel_MouseLeave(object sender, MouseEventArgs e)
{
    StartAutoNavigation();
}
```

### Pattern 5: Gesture-Based Navigation (Touch)

```csharp
using System.Windows.Input;

private Point touchStartPoint;

private void Carousel_TouchDown(object sender, TouchEventArgs e)
{
    touchStartPoint = e.GetTouchPoint(carousel).Position;
}

private void Carousel_TouchUp(object sender, TouchEventArgs e)
{
    Point touchEndPoint = e.GetTouchPoint(carousel).Position;
    double deltaX = touchEndPoint.X - touchStartPoint.X;
    
    // Swipe threshold
    if (Math.Abs(deltaX) > 50)
    {
        if (deltaX > 0)
        {
            // Swipe right - previous item
            carousel.SelectPreviousItem();
        }
        else
        {
            // Swipe left - next item
            carousel.SelectNextItem();
        }
    }
}
```

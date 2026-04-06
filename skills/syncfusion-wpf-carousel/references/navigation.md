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

Define your model and collection as usual; see `getting-started.md` for a full `ViewModel` example used with keyboard navigation. The Carousel binds to a collection and exposes keyboard navigation when focused.

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
                    # Navigation (split)

                    This file was split to keep reference files small.

                    - `navigation-basics.md`: core behaviors (keyboard, scroll, mouse wheel, commands).
                    - `navigation-patterns.md`: patterns and advanced examples (arrow buttons, thumbnails, indicator dots, timers, gestures, looping).

                    ```
```

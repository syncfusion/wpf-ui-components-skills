# Popup Customization in SfMultiColumnDropDownControl

## Table of Contents
- [Overview](#overview)
- [Popup Appearance](#popup-appearance)
- [Popup Sizing](#popup-sizing)
  - [Fixed Size](#fixed-size)
  - [Auto-Sizing](#auto-sizing)
  - [Min/Max Constraints](#minmax-constraints)
  - [Runtime Resizing](#runtime-resizing)
- [Popup Content Customization](#popup-content-customization)
- [Popup Events](#popup-events)
- [Advanced Customization](#advanced-customization)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The SfMultiColumnDropDownControl popup is highly customizable, allowing you to control its appearance, size, behavior, and content. The popup contains the embedded SfDataGrid and can be styled to match your application's theme.

## Popup Appearance

### Background and Border

Customize the visual appearance of the popup:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    PopupBackground="WhiteSmoke"
    PopupBorderBrush="SteelBlue"
    PopupBorderThickness="2"
    PopupDropDownGridBackground="White"/>
```

### Appearance Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **PopupBackground** | Brush | Background of popup container | Gainsboro |
| **PopupBorderBrush** | Brush | Border color of popup | Black |
| **PopupBorderThickness** | Thickness | Border thickness | 1 |
| **PopupDropDownGridBackground** | Brush | Background of the grid inside popup | White |

### Custom Styling Example

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    PopupBackground="#F5F5F5"
    PopupBorderBrush="#2196F3"
    PopupBorderThickness="2,2,2,2"
    PopupDropDownGridBackground="White"
    PopupHeight="300"
    PopupWidth="500"/>
```

### Gradient Background

```xml
<syncfusion:SfMultiColumnDropDownControl x:Name="sfMultiColumn">
    <syncfusion:SfMultiColumnDropDownControl.PopupBackground>
        <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
            <GradientStop Color="White" Offset="0"/>
            <GradientStop Color="LightGray" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:SfMultiColumnDropDownControl.PopupBackground>
</syncfusion:SfMultiColumnDropDownControl>
```

## Popup Sizing

### Fixed Size

Set explicit width and height for the popup:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    PopupHeight="350"
    PopupWidth="600"
    IsAutoPopupSize="False"/>
```

**Code-Behind:**
```csharp
sfMultiColumn.PopupHeight = 350;
sfMultiColumn.PopupWidth = 600;
sfMultiColumn.IsAutoPopupSize = false;
```

### Auto-Sizing

Let the popup size automatically based on grid content:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    IsAutoPopupSize="True"/>
```

**When IsAutoPopupSize is True:**
- Popup width matches the widest row or control width
- Popup height fits all visible rows (up to MaxHeight)
- No manual width/height needed

### Min/Max Constraints

Control popup size boundaries:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    PopupMinHeight="200"
    PopupMaxHeight="500"
    PopupMinWidth="300"
    PopupMaxWidth="800"
    IsAutoPopupSize="False"/>
```

### Size Properties Table

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| **PopupHeight** | double | Fixed height of popup | NaN (auto) |
| **PopupWidth** | double | Fixed width of popup | NaN (auto) |
| **PopupMinHeight** | double | Minimum popup height | DefaultPopupHeight |
| **PopupMaxHeight** | double | Maximum popup height | PositiveInfinity |
| **PopupMinWidth** | double | Minimum popup width | DefaultPopUpWidth |
| **PopupMaxWidth** | double | Maximum popup width | PositiveInfinity |
| **ActualPopupHeight** | double | Current actual height (read-only) | - |
| **ActualPopupWidth** | double | Current actual width (read-only) | - |
| **IsAutoPopupSize** | bool | Auto-size based on content | false |

### Getting Actual Popup Size

```csharp
// Get current popup dimensions
double actualHeight = sfMultiColumn.ActualPopupHeight;
double actualWidth = sfMultiColumn.ActualPopupWidth;

Console.WriteLine($"Popup Size: {actualWidth}x{actualHeight}");
```

### Runtime Resizing

The popup includes a resize thumb in the bottom-right corner that allows users to resize it at runtime.

**Enable Resizing:**
```xml
<!-- Resizing is enabled by default -->
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    PopupMinHeight="150"
    PopupMaxHeight="600"
    PopupMinWidth="250"
    PopupMaxWidth="1000"/>
```

**Resize Constraints:**
- User can only resize between Min and Max values
- Maintains aspect ratio if both dimensions constrained
- Resize thumb appears automatically

### Responsive Sizing Example

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    PopupMinHeight="200"
    PopupMaxHeight="600"
    PopupMinWidth="400"
    PopupMaxWidth="1200"
    PopupHeight="300"
    PopupWidth="600"/>
```

## Popup Content Customization

### PopupContentTemplate

Add custom content to the popup:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName">
    
    <syncfusion:SfMultiColumnDropDownControl.PopupContentTemplate>
        <DataTemplate>
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                
                <!-- Custom Header -->
                <Border Grid.Row="0" Background="LightBlue" Padding="10">
                    <TextBlock Text="Select an Order" FontWeight="Bold" FontSize="16"/>
                </Border>
                
                <!-- Grid (Content) -->
                <ContentPresenter Grid.Row="1" Content="{Binding}"/>
                
                <!-- Custom Footer -->
                <Border Grid.Row="2" Background="LightGray" Padding="10">
                    <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
                        <Button Content="OK" Width="75" Margin="5"/>
                        <Button Content="Cancel" Width="75" Margin="5"/>
                    </StackPanel>
                </Border>
            </Grid>
        </DataTemplate>
    </syncfusion:SfMultiColumnDropDownControl.PopupContentTemplate>
</syncfusion:SfMultiColumnDropDownControl>
```

### HeaderTemplate

Add a custom header above the grid in the popup:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    ItemsSource="{Binding Orders}"
    DisplayMember="CustomerName"
    SelectionMode="Multiple">
    
    <syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
        <DataTemplate>
            <StackPanel Background="WhiteSmoke">
                <!-- Search Box -->
                <TextBox x:Name="SearchBox" 
                         Margin="10,10,10,5"
                         Padding="5"
                         Watermark="Search orders..."
                         TextChanged="SearchBox_TextChanged"/>
                
                <!-- Selection Info -->
                <TextBlock Margin="10,5,10,10" 
                           FontSize="12" 
                           Foreground="Gray">
                    <Run Text="Selected: "/>
                    <Run Text="{Binding RelativeSource={RelativeSource AncestorType=syncfusion:SfMultiColumnDropDownControl}, Path=SelectedItems.Count}"/>
                    <Run Text=" items"/>
                </TextBlock>
                
                <Separator Margin="0"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
</syncfusion:SfMultiColumnDropDownControl>
```

### Advanced Header with Filtering

```xml
<syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
    <DataTemplate>
        <Grid Background="White">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            
            <!-- Title -->
            <TextBlock Grid.Row="0" 
                       Text="Select Orders" 
                       FontWeight="Bold" 
                       FontSize="16"
                       Margin="10"/>
            
            <!-- Filter Options -->
            <StackPanel Grid.Row="1" Orientation="Horizontal" Margin="10,0,10,10">
                <TextBlock Text="Filter by Country:" VerticalAlignment="Center" Margin="0,0,10,0"/>
                <ComboBox x:Name="CountryFilter" Width="150" SelectionChanged="CountryFilter_SelectionChanged">
                    <ComboBoxItem Content="All Countries"/>
                    <ComboBoxItem Content="Germany"/>
                    <ComboBoxItem Content="Mexico"/>
                    <ComboBoxItem Content="UK"/>
                </ComboBox>
            </StackPanel>
        </Grid>
    </DataTemplate>
</syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
```

## Popup Events

### Popup Opening Event

Fires before the popup opens (can be canceled):

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    PopupOpening="SfMultiColumn_PopupOpening"/>
```

```csharp
private void SfMultiColumn_PopupOpening(object sender, PopupOpeningEventArgs e)
{
    // Cancel opening based on condition
    if (!IsDataReady())
    {
        e.Cancel = true;
        MessageBox.Show("Data is not ready yet.");
        return;
    }
    
    // Prepare data before showing
    RefreshData();
    
    // Log event
    Console.WriteLine("Popup is opening...");
}
```

### Popup Opened Event

Fires after the popup has opened:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    PopupOpened="SfMultiColumn_PopupOpened"/>
```

```csharp
private void SfMultiColumn_PopupOpened(object sender, PopupOpenedEventArgs e)
{
    // Set focus to search box in header
    var searchBox = FindChildControl<TextBox>(sfMultiColumn, "SearchBox");
    searchBox?.Focus();
    
    // Track analytics
    TrackPopupOpen();
    
    Console.WriteLine("Popup opened successfully");
}
```

### Popup Closing Event

Fires before the popup closes (can be canceled):

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    PopupClosing="SfMultiColumn_PopupClosing"/>
```

```csharp
private void SfMultiColumn_PopupClosing(object sender, PopupClosingEventArgs e)
{
    // Validate selection before closing
    if (sfMultiColumn.SelectedItem == null)
    {
        e.Cancel = true;
        MessageBox.Show("Please select an item.");
        return;
    }
    
    // Confirm multi-selection changes
    if (sfMultiColumn.SelectionMode == DropDownSelectionMode.Multiple)
    {
        if (sfMultiColumn.SelectedItems.Count == 0)
        {
            var result = MessageBox.Show(
                "No items selected. Continue?", 
                "Confirm", 
                MessageBoxButton.YesNo);
            
            if (result == MessageBoxResult.No)
            {
                e.Cancel = true;
            }
        }
    }
}
```

### Popup Closed Event

Fires after the popup has closed:

```xml
<syncfusion:SfMultiColumnDropDownControl 
    x:Name="sfMultiColumn"
    PopupClosed="SfMultiColumn_PopupClosed"/>
```

```csharp
private void SfMultiColumn_PopupClosed(object sender, PopupClosedEventArgs e)
{
    // Process the selection
    var selectedOrder = sfMultiColumn.SelectedItem as OrderInfo;
    if (selectedOrder != null)
    {
        ProcessOrder(selectedOrder);
    }
    
    // Update dependent UI
    RefreshOrderDetails();
    
    // Clean up resources
    CleanupPopupResources();
}
```

### Event Sequence

1. **PopupOpening** (cancelable) → Popup is about to open
2. **PopupOpened** → Popup is now visible
3. User interacts with popup
4. **PopupClosing** (cancelable) → Popup is about to close
5. **PopupClosed** → Popup is now hidden

## Advanced Customization

### StaysOpen Property

Keep the popup open even when clicking outside:

```csharp
// Access popup from control template
sfMultiColumn.Loaded += (o, e) =>
{
    var popup = sfMultiColumn.Template.FindName("PART_Popup", sfMultiColumn) as Popup;
    if (popup != null)
    {
        popup.StaysOpen = true;
    }
};
```

**Use Cases:**
- Debug scenarios
- Multi-step selection processes
- Custom close logic

### Programmatically Open/Close Popup

```csharp
// Open popup
sfMultiColumn.IsDropDownOpen = true;

// Close popup
sfMultiColumn.IsDropDownOpen = false;

// Toggle popup
sfMultiColumn.IsDropDownOpen = !sfMultiColumn.IsDropDownOpen;
```

### Popup Animation

```xml
<syncfusion:SfMultiColumnDropDownControl>
    <syncfusion:SfMultiColumnDropDownControl.Resources>
        <Storyboard x:Key="PopupOpenAnimation">
            <DoubleAnimation Storyboard.TargetProperty="Opacity"
                             From="0" To="1" Duration="0:0:0.3"/>
        </Storyboard>
    </syncfusion:SfMultiColumnDropDownControl.Resources>
</syncfusion:SfMultiColumnDropDownControl>
```

### Custom Popup Position

```csharp
sfMultiColumn.Loaded += (o, e) =>
{
    var popup = sfMultiColumn.Template.FindName("PART_Popup", sfMultiColumn) as Popup;
    if (popup != null)
    {
        popup.Placement = PlacementMode.Bottom;
        popup.HorizontalOffset = 0;
        popup.VerticalOffset = 5;
    }
};
```

## Best Practices

### 1. Set Reasonable Default Sizes

```xml
<syncfusion:SfMultiColumnDropDownControl 
    PopupHeight="300"
    PopupWidth="500"
    PopupMinHeight="200"
    PopupMaxHeight="600"/>
```

### 2. Use Auto-Sizing for Dynamic Content

```xml
<!-- When item count varies greatly -->
<syncfusion:SfMultiColumnDropDownControl 
    IsAutoPopupSize="True"
    PopupMaxHeight="500"/>  <!-- Prevent excessive height -->
```

### 3. Provide Visual Consistency

```xml
<!-- Match application theme -->
<syncfusion:SfMultiColumnDropDownControl 
    PopupBackground="{StaticResource AppBackgroundBrush}"
    PopupBorderBrush="{StaticResource AppAccentBrush}"
    PopupBorderThickness="1"/>
```

### 4. Add Search in Header for Large Datasets

```xml
<syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
    <DataTemplate>
        <TextBox Watermark="Search..." Margin="10"/>
    </DataTemplate>
</syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
```

### 5. Handle Popup Events for Validation

```csharp
// Validate before closing
private void SfMultiColumn_PopupClosing(object sender, PopupClosingEventArgs e)
{
    if (RequiresValidation() && !IsValid())
    {
        e.Cancel = true;
        ShowValidationMessage();
    }
}
```

## Troubleshooting

### Popup Not Showing Correct Size

**Problem:** Popup appears too small or too large

**Solutions:**
```xml
<!-- Set explicit dimensions -->
<syncfusion:SfMultiColumnDropDownControl 
    PopupHeight="350"
    PopupWidth="600"
    IsAutoPopupSize="False"/>

<!-- Or use auto-sizing with constraints -->
<syncfusion:SfMultiColumnDropDownControl 
    IsAutoPopupSize="True"
    PopupMinHeight="200"
    PopupMaxHeight="500"/>
```

### Popup Styling Not Applied

**Problem:** PopupBackground or borders not visible

**Solutions:**
```xml
<!-- Ensure properties are set -->
<syncfusion:SfMultiColumnDropDownControl 
    PopupBackground="WhiteSmoke"  <!-- Not Background -->
    PopupBorderBrush="Black"      <!-- Not BorderBrush -->
    PopupBorderThickness="2"/>    <!-- Not BorderThickness -->
```

### Custom Template Not Showing

**Problem:** PopupContentTemplate or HeaderTemplate not displaying

**Solutions:**
```xml
<!-- Ensure DataTemplate is properly defined -->
<syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
    <DataTemplate>
        <!-- Must have content -->
        <StackPanel>
            <TextBlock Text="Header Content"/>
        </StackPanel>
    </DataTemplate>
</syncfusion:SfMultiColumnDropDownControl.HeaderTemplate>
```

### Popup Events Not Firing

**Problem:** Event handlers never called

**Solutions:**
```xml
<!-- Verify event names and handlers -->
<syncfusion:SfMultiColumnDropDownControl 
    PopupOpening="SfMultiColumn_PopupOpening"  <!-- Correct event name -->
    PopupClosed="SfMultiColumn_PopupClosed"/>  <!-- Not PopupClose -->
```

```csharp
// Ensure correct signature
private void SfMultiColumn_PopupOpening(object sender, PopupOpeningEventArgs e)
{
    // Handler code
}
```

### Resize Not Working

**Problem:** Cannot resize popup with mouse

**Solutions:**
```xml
<!-- Set min/max constraints -->
<syncfusion:SfMultiColumnDropDownControl 
    PopupMinHeight="150"
    PopupMaxHeight="600"
    PopupMinWidth="250"
    PopupMaxWidth="1000"/>

<!-- Resize thumb appears in bottom-right corner -->
<!-- Ensure popup is large enough to see thumb -->
```

### StaysOpen Not Working

**Problem:** Popup still closes when clicking outside

**Solution:**
```csharp
// Must access through template after control is loaded
sfMultiColumn.Loaded += (o, e) =>
{
    var popup = sfMultiColumn.Template.FindName("PART_Popup", sfMultiColumn) as Popup;
    if (popup != null)
    {
        popup.StaysOpen = true;
    }
};
```

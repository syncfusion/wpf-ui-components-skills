# Restrict User Selection in WPF Rating (SfRating)

The SfRating control supports read-only mode, which prevents users from changing the rating value. This is useful for displaying ratings that should not be modified, such as average ratings, historical data, or locked feedback.

## Table of Contents
- [Overview](#overview)
- [Setting Read-Only Mode](#setting-read-only-mode)
- [Use Cases for Read-Only Mode](#use-cases-for-read-only-mode)
- [Behavior in Read-Only Mode](#behavior-in-read-only-mode)
- [Combining Read-Only with Other Features](#combining-read-only-with-other-features)
- [Dynamic Read-Only State](#dynamic-read-only-state)
- [Programmatic Value Updates](#programmatic-value-updates)
- [Visual Indicators for Read-Only State](#visual-indicators-for-read-only-state)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

The `IsReadOnly` property controls whether users can interact with and change the rating value. When set to `true`, the rating becomes non-editable and displays the current value only.

**Default value:** `false` (interactive)

## Setting Read-Only Mode

### XAML Configuration

**XAML:**
```xml
<syncfusion:SfRating ItemsCount="5" 
                     Value="4"
                     IsReadOnly="True"
                     Width="150"/>
```

### C# Configuration

**C#:**
```csharp
SfRating rating = new SfRating()
{
    ItemsCount = 5,
    Value = 4,
    IsReadOnly = true,
    Width = 150
};
this.Content = rating;
```

## Use Cases for Read-Only Mode

### 1. Displaying Average Ratings

Show calculated average ratings from multiple users without allowing changes:

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="4.3"
                     Precision="Exact"
                     IsReadOnly="True"
                     ShowToolTip="True"
                     AutoToolTipPrecision="1"/>
<TextBlock Text="(based on 247 reviews)" 
           Margin="5,0,0,0"
           VerticalAlignment="Center"/>
```

**Why read-only:** Users should see the aggregated rating but not modify it.

### 2. Historical Rating Display

Display past ratings that are locked and cannot be changed:

```xml
<StackPanel>
    <TextBlock Text="Your rating from 2023:" FontWeight="Bold"/>
    <syncfusion:SfRating ItemsCount="5"
                         Value="3"
                         IsReadOnly="True"
                         Width="150"
                         Margin="0,5,0,0"/>
</StackPanel>
```

**Why read-only:** Historical data should not be retroactively modified.

### 3. Submitted Feedback

After a user submits a rating, display it in read-only mode:

```csharp
private void SubmitRating_Click(object sender, RoutedEventArgs e)
{
    // Save rating to database
    SaveRatingToDatabase(userRating.Value);
    
    // Lock the rating
    userRating.IsReadOnly = true;
    
    // Show confirmation
    MessageBox.Show("Thank you for your feedback!");
}
```

**Why read-only:** Once submitted, the rating should not be changed without proper workflow.

### 4. Permission-Based Ratings

Control who can edit ratings based on user permissions:

```csharp
public void LoadRating(bool userCanEdit)
{
    SfRating rating = new SfRating()
    {
        ItemsCount = 5,
        Value = GetCurrentRating(),
        IsReadOnly = !userCanEdit  // Read-only if user cannot edit
    };
    
    ratingContainer.Content = rating;
}
```

**Why read-only:** Enforce access control for rating modifications.

### 5. Comparison Displays

Show multiple ratings side-by-side for comparison:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <StackPanel Grid.Row="0" Orientation="Horizontal">
        <TextBlock Text="Quality:" Width="100"/>
        <syncfusion:SfRating ItemsCount="5" Value="4.5" 
                             Precision="Half" IsReadOnly="True"/>
    </StackPanel>
    
    <StackPanel Grid.Row="1" Orientation="Horizontal" Margin="0,10,0,0">
        <TextBlock Text="Value:" Width="100"/>
        <syncfusion:SfRating ItemsCount="5" Value="4.0" 
                             Precision="Half" IsReadOnly="True"/>
    </StackPanel>
    
    <StackPanel Grid.Row="2" Orientation="Horizontal" Margin="0,10,0,0">
        <TextBlock Text="Service:" Width="100"/>
        <syncfusion:SfRating ItemsCount="5" Value="3.5" 
                             Precision="Half" IsReadOnly="True"/>
    </StackPanel>
</Grid>
```

**Why read-only:** These are display metrics, not user input fields.

### 6. Dashboard Metrics

Display KPIs or performance ratings in dashboards:

```xml
<Border BorderBrush="LightGray" BorderThickness="1" Padding="15">
    <StackPanel>
        <TextBlock Text="Customer Satisfaction" 
                   FontSize="16" FontWeight="Bold"/>
        <syncfusion:SfRating ItemsCount="5"
                             Value="4.7"
                             Precision="Exact"
                             IsReadOnly="True"
                             ItemSize="24"
                             Margin="0,10,0,0">
            <syncfusion:SfRating.ItemContainerStyle>
                <Style TargetType="syncfusion:SfRatingItem">
                    <Setter Property="RatedFill" Value="#4CAF50"/>
                </Style>
            </syncfusion:SfRating.ItemContainerStyle>
        </syncfusion:SfRating>
    </StackPanel>
</Border>
```

**Why read-only:** Metrics are calculated values, not user inputs.

## Behavior in Read-Only Mode

When `IsReadOnly="True"`:

- **No mouse interaction:** Clicking rating items does not change the value
- **No ValueChanged events:** The ValueChanged event will not fire from user interaction
- **Hover effects still work:** PointerOver styling properties still apply (visual feedback)
- **Tooltips still appear:** ShowToolTip continues to function if enabled
- **Programmatic changes allowed:** You can still update the Value property in code
- **Keyboard input disabled:** Arrow keys and other keyboard input are ignored

## Combining Read-Only with Other Features

### Read-Only + Precise Values

Display exact calculated values:

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="4.37"
                     Precision="Exact"
                     IsReadOnly="True"
                     ShowToolTip="True"
                     AutoToolTipPrecision="2"/>
```

Perfect for showing precise aggregated ratings from analytics.

### Read-Only + Custom Styling

Differentiate read-only ratings visually:

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="4"
                     IsReadOnly="True">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="Gray"/>
            <Setter Property="UnratedFill" Value="LightGray"/>
            <Setter Property="RatedStroke" Value="DarkGray"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

Use muted colors to indicate the rating is not interactive.

### Read-Only + Half Precision

Common for displaying average ratings:

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="4.5"
                     Precision="Half"
                     IsReadOnly="True"
                     ShowToolTip="True"/>
```

Shows half-star averages calculated from user feedback.

## Dynamic Read-Only State

Change read-only state based on application logic:

### Example 1: Toggle Based on User Action

```csharp
private bool _ratingSubmitted = false;

private void SubmitButton_Click(object sender, RoutedEventArgs e)
{
    if (!_ratingSubmitted)
    {
        SaveRating(productRating.Value);
        productRating.IsReadOnly = true;  // Lock after submission
        _ratingSubmitted = true;
        
        submitButton.Content = "Rating Submitted";
        submitButton.IsEnabled = false;
    }
}
```

### Example 2: Conditional Based on User Role

```csharp
public void SetupRating(UserRole role)
{
    bool canEdit = (role == UserRole.Admin || role == UserRole.Moderator);
    
    productRating.IsReadOnly = !canEdit;
    
    if (canEdit)
    {
        statusText.Text = "You can edit this rating";
    }
    else
    {
        statusText.Text = "Rating is view-only";
    }
}
```

### Example 3: Time-Based Locking

```csharp
public void LoadRating(DateTime ratingDate)
{
    // Allow editing only within 24 hours
    TimeSpan elapsed = DateTime.Now - ratingDate;
    bool isEditable = elapsed.TotalHours <= 24;
    
    productRating.IsReadOnly = !isEditable;
    
    if (!isEditable)
    {
        MessageBox.Show("This rating can no longer be edited.");
    }
}
```

## Programmatic Value Updates

Even in read-only mode, you can update the value programmatically:

```csharp
// Rating is read-only, but code can still update it
productRating.IsReadOnly = true;

// This works - updates the display
productRating.Value = 4.5;

// This also works - updates from data binding
// The user just can't interact with the control
```

This is useful for:
- Refreshing displayed averages
- Updating dashboard metrics
- Showing real-time calculated values
- Synchronizing with database changes

## Visual Indicators for Read-Only State

While the control doesn't automatically change appearance in read-only mode, you can add visual indicators:

### Option 1: Muted Colors

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="4"
                     IsReadOnly="True">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="Opacity" Value="0.6"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

### Option 2: Different Color Scheme

```xml
<syncfusion:SfRating ItemsCount="5"
                     Value="3"
                     IsReadOnly="True">
    <syncfusion:SfRating.ItemContainerStyle>
        <Style TargetType="syncfusion:SfRatingItem">
            <Setter Property="RatedFill" Value="#9E9E9E"/>
            <Setter Property="UnratedFill" Value="#E0E0E0"/>
        </Style>
    </syncfusion:SfRating.ItemContainerStyle>
</syncfusion:SfRating>
```

### Option 3: Label Indicator

```xml
<StackPanel Orientation="Horizontal">
    <syncfusion:SfRating ItemsCount="5" 
                         Value="4" 
                         IsReadOnly="True"/>
    <TextBlock Text="(read-only)" 
               FontStyle="Italic" 
               Foreground="Gray"
               Margin="10,0,0,0"
               VerticalAlignment="Center"/>
</StackPanel>
```

## Complete Examples

### Example 1: Product Review Display

```xml
<Border BorderBrush="LightGray" BorderThickness="1" Padding="10">
    <StackPanel>
        <TextBlock Text="Overall Rating" FontWeight="Bold" FontSize="14"/>
        
        <StackPanel Orientation="Horizontal" Margin="0,5,0,0">
            <syncfusion:SfRating ItemsCount="5"
                                 Value="4.3"
                                 Precision="Exact"
                                 IsReadOnly="True"
                                 ShowToolTip="True"
                                 AutoToolTipPrecision="1"
                                 ItemSize="20">
                <syncfusion:SfRating.ItemContainerStyle>
                    <Style TargetType="syncfusion:SfRatingItem">
                        <Setter Property="RatedFill" Value="#FFD700"/>
                    </Style>
                </syncfusion:SfRating.ItemContainerStyle>
            </syncfusion:SfRating>
            
            <TextBlock Text="4.3 out of 5" 
                       Margin="10,0,0,0"
                       VerticalAlignment="Center"/>
        </StackPanel>
        
        <TextBlock Text="Based on 1,247 reviews" 
                   FontSize="11"
                   Foreground="Gray"
                   Margin="0,5,0,0"/>
    </StackPanel>
</Border>
```

### Example 2: Editable vs Read-Only Comparison

```csharp
public partial class RatingExample : Window
{
    public RatingExample()
    {
        InitializeComponent();
        SetupRatings();
    }
    
    private void SetupRatings()
    {
        // User's editable rating
        userRating.ItemsCount = 5;
        userRating.Value = 0;
        userRating.IsReadOnly = false;
        
        // Display average (read-only)
        averageRating.ItemsCount = 5;
        averageRating.Value = 4.3;
        averageRating.Precision = Syncfusion.Windows.Primitives.Precision.Exact;
        averageRating.IsReadOnly = true;
    }
    
    private void SubmitRating_Click(object sender, RoutedEventArgs e)
    {
        // Save user rating
        double newRating = userRating.Value;
        SaveToDatabase(newRating);
        
        // Lock user's rating
        userRating.IsReadOnly = true;
        
        // Update average
        averageRating.Value = RecalculateAverage();
    }
}
```

**XAML:**
```xml
<StackPanel Margin="20">
    <TextBlock Text="Rate this product:" FontWeight="Bold"/>
    <syncfusion:SfRating x:Name="userRating" Margin="0,5,0,20"/>
    
    <TextBlock Text="Average rating:" FontWeight="Bold"/>
    <syncfusion:SfRating x:Name="averageRating" Margin="0,5,0,20"/>
    
    <Button Content="Submit Rating" 
            Click="SubmitRating_Click"
            Width="120"/>
</StackPanel>
```

### Example 3: Multi-Criteria Read-Only Ratings

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    
    <TextBlock Grid.Row="0" Grid.Column="0" Text="Quality:" Margin="0,0,10,10"/>
    <syncfusion:SfRating Grid.Row="0" Grid.Column="1" 
                         ItemsCount="5" Value="4.5" 
                         Precision="Half" IsReadOnly="True" Margin="0,0,0,10"/>
    
    <TextBlock Grid.Row="1" Grid.Column="0" Text="Value:" Margin="0,0,10,10"/>
    <syncfusion:SfRating Grid.Row="1" Grid.Column="1" 
                         ItemsCount="5" Value="4.0" 
                         Precision="Half" IsReadOnly="True" Margin="0,0,0,10"/>
    
    <TextBlock Grid.Row="2" Grid.Column="0" Text="Shipping:" Margin="0,0,10,10"/>
    <syncfusion:SfRating Grid.Row="2" Grid.Column="1" 
                         ItemsCount="5" Value="3.5" 
                         Precision="Half" IsReadOnly="True" Margin="0,0,0,10"/>
    
    <TextBlock Grid.Row="3" Grid.Column="0" Text="Overall:" FontWeight="Bold" Margin="0,0,10,0"/>
    <syncfusion:SfRating Grid.Row="3" Grid.Column="1" 
                         ItemsCount="5" Value="4.0" 
                         Precision="Half" IsReadOnly="True"/>
</Grid>
```

## Best Practices

1. **Use for Display Data:** Set IsReadOnly="True" when showing calculated or aggregated ratings
2. **Lock After Submission:** Change to read-only after users submit ratings to prevent accidental changes
3. **Visual Differentiation:** Consider using different colors or opacity to distinguish read-only ratings
4. **Combine with Tooltips:** Enable tooltips on read-only ratings to show exact numeric values
5. **Accessibility:** Ensure read-only ratings are announced properly by screen readers
6. **Context Labels:** Add labels to clarify why a rating is read-only (e.g., "Average", "Your previous rating")
7. **Precision Choice:** Use Exact or Half precision for read-only calculated values

## Common Patterns

### Pattern 1: View-Only Average
```xml
<syncfusion:SfRating Value="{Binding AverageRating}"
                     Precision="Half"
                     IsReadOnly="True"
                     ShowToolTip="True"/>
```

### Pattern 2: Locked After Submit
```csharp
rating.IsReadOnly = (rating.Value > 0);  // Read-only if already rated
```

### Pattern 3: Admin Override
```csharp
rating.IsReadOnly = (CurrentUser.Role != UserRole.Admin);
```

## Troubleshooting

### Rating Still Interactive

**Problem:** Rating changes even with IsReadOnly="True".

**Solutions:**
- Verify IsReadOnly property is actually set to True
- Check for code that might be resetting it to False
- Ensure ValueChanged events aren't programmatically changing the value
- Rebuild the application

### Visual State Not Clear

**Problem:** Users don't realize rating is read-only.

**Solutions:**
- Add visual indicators (opacity, different colors)
- Include text label explaining the rating is read-only
- Use context (e.g., "Average Rating" label implies read-only)
- Consider disabling hover effects for read-only ratings

### Programmatic Updates Not Working

**Problem:** Cannot update Value in read-only mode.

**Solutions:**
- Read-only mode only blocks user interaction; code updates work fine
- Check that the value is within valid range [0, ItemsCount]
- Verify data binding is configured correctly
- Ensure no conflicting value setters in code

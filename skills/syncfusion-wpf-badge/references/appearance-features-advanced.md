# Appearance — Advanced Examples and Edge Cases

## Dynamic Badge Content from Multiple Properties

```csharp
public class OrderViewModel : INotifyPropertyChanged
{
    private int _itemCount;
    private int _errorCount;
    
    public int ItemCount
    {
        get { return _itemCount; }
        set { SetProperty(ref _itemCount, value); }
    }
    
    public int ErrorCount
    {
        get { return _errorCount; }
        set { SetProperty(ref _errorCount, value); }
    }
    
    public string BadgeContent
    {
        get
        {
            if (ErrorCount > 0)
                return $"{ErrorCount}!";
            return ItemCount > 0 ? ItemCount.ToString() : null;
        }
    }
}
```

## Badge with Custom Animation Behavior

```csharp
public void AnimateBadge(UIElement badge)
{
    // Scale animation
    DoubleAnimation scaleX = new DoubleAnimation
    {
        From = 1.0,
        To = 1.3,
        Duration = new Duration(TimeSpan.FromSeconds(0.2)),
        AutoReverse = true
    };
    
    ScaleTransform transform = new ScaleTransform();
    badge.RenderTransform = transform;
    
    transform.BeginAnimation(ScaleTransform.ScaleXProperty, scaleX);
}
```

## Batch Update Pattern

```csharp
// Update multiple badges efficiently
void UpdateNotifications(List<BadgeUpdate> updates)
{
    foreach (var update in updates)
    {
        var badge = GetBadgeForItem(update.ItemId);
        if (badge != null)
        {
            badge.Content = update.Count;
            if (update.IsUrgent)
                badge.Fill = BadgeFill.Error;
        }
    }
}
```

## Responsive Badge Sizing

```xaml
<Grid>
    <Button Width="100" Height="50" Content="Inbox">
        <notification:SfBadge.Badge>
            <notification:SfBadge 
                Width="{Binding RelativeSource={RelativeSource AncestorType=Button}, Path=ActualWidth, Converter={StaticResource PercentageConverter}, ConverterParameter=0.3}"
                Height="20"
                Content="5"
                x:Name="badge"/>
        </notification:SfBadge.Badge>
    </Button>
</Grid>
```

## Badge with Tooltip

```xaml
<notification:SfBadge 
    Content="5"
    x:Name="badge">
    <ToolTipService.ToolTip>
        <ToolTip>
            <StackPanel>
                <TextBlock Text="5 Notifications" FontWeight="Bold"/>
                <TextBlock Text="Click to view"/>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
</notification:SfBadge>
```

## Edge Case: Very Large Content

```xaml
<!-- Handle long content with smaller font -->
<notification:SfBadge 
    Content="URGENT_UPDATE"
    Width="70"
    Height="30"
    FontSize="9"
    Shape="Rectangle"/>
```

## Edge Case: Content with Special Characters

```csharp
// Handle emoji and special characters
badge.Content = "📧"; // Email indicator
badge.Content = "⚠️"; // Warning indicator
badge.Content = "✓"; // Check mark
```

## Performance: Reusing Badge References

```csharp
// Instead of creating new badges
SfBadge cachedBadge = null;

void UpdateOrCreateBadge(Button button, object content)
{
    if (cachedBadge == null)
    {
        cachedBadge = new SfBadge { Content = content };
        SfBadge.SetBadge(button, cachedBadge);
    }
    else
    {
        cachedBadge.Content = content;
    }
}
```

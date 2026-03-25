# SfBusyIndicator Methods

## Table of Contents
- [Overview](#overview)
- [Dispose()](#dispose)
- [Dispose(Boolean)](#disposeboolean)
- [OnApplyTemplate()](#onapplytemplate)
- [OnAnimationTypeChanged()](#onanimationtypechanged)
- [OnPropertyChanged()](#onpropertychanged)
- [Custom Control Scenarios](#custom-control-scenarios)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The SfBusyIndicator control exposes several methods that can be overridden when creating custom controls or extensions. These methods provide hooks into the control's lifecycle, template application, and property change handling.

This guide documents the methods listed in the API reference with concise descriptions and concrete examples that can be adapted for custom implementations. All example code is written as if implemented inside a control class or derived class.

## Dispose()

The `Dispose()` method explicitly releases managed and unmanaged resources when the control is no longer needed. This implements the IDisposable pattern for proper resource cleanup.

### Purpose
- Release managed resources (event handlers, timers, child controls)
- Release unmanaged resources (file handles, database connections)
- Prevent memory leaks in long-running applications
- Ensure deterministic cleanup of resources

### Usage Pattern

```csharp
public class MyBusyIndicator : SfBusyIndicator, IDisposable
{
    private bool _disposed;
    
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    ~MyBusyIndicator()
    {
        Dispose(false);
    }
    
    protected virtual void Dispose(bool disposing)
    {
        if (_disposed) return;
        
        if (disposing)
        {
            // Free managed resources here
            // - Detach event handlers
            // - Dispose child controls
            // - Stop timers
        }
        
        // Free unmanaged resources here (if any)
        
        _disposed = true;
    }
}
```

### Complete Example with Resources

```csharp
public class ResourceAwareBusyIndicator : SfBusyIndicator, IDisposable
{
    private DispatcherTimer _timer;
    private bool _disposed;
    
    public ResourceAwareBusyIndicator()
    {
        _timer = new DispatcherTimer();
        _timer.Interval = TimeSpan.FromSeconds(1);
        _timer.Tick += OnTimerTick;
    }
    
    private void OnTimerTick(object sender, EventArgs e)
    {
        // Update logic
    }
    
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    protected virtual void Dispose(bool disposing)
    {
        if (_disposed) return;
        
        if (disposing)
        {
            // Stop and dispose timer
            if (_timer != null)
            {
                _timer.Stop();
                _timer.Tick -= OnTimerTick;
                _timer = null;
            }
            
            // Detach other event handlers
            this.Loaded -= OnLoaded;
            this.Unloaded -= OnUnloaded;
        }
        
        _disposed = true;
    }
    
    ~ResourceAwareBusyIndicator()
    {
        Dispose(false);
    }
}
```

## Dispose(Boolean)

The `Dispose(bool disposing)` protected overload implements the actual resource cleanup logic. The `disposing` parameter indicates whether the method is being called from `Dispose()` (true) or from a finalizer (false).

### Parameters
- **disposing** (bool): True if called from Dispose(), False if called from finalizer

### Implementation Guidelines

When `disposing` is **true**:
- Release managed objects (event handlers, timers, child controls)
- Call Dispose() on disposable member objects
- Safe to access other managed objects

When `disposing` is **false**:
- Release ONLY unmanaged resources
- Do NOT access other managed objects (they may already be garbage collected)
- Called from finalizer during garbage collection

### Example

```csharp
protected virtual void Dispose(bool disposing)
{
    if (_disposed) return;
    
    if (disposing)
    {
        // Managed resource cleanup
        if (_storyboard != null)
        {
            _storyboard.Stop();
            _storyboard = null;
        }
        
        if (_subscription != null)
        {
            _subscription.Dispose();
            _subscription = null;
        }
    }
    
    // Unmanaged resource cleanup (rare in WPF)
    // For example: Close native window handles, COM objects
    if (_nativeHandle != IntPtr.Zero)
    {
        NativeMethods.CloseHandle(_nativeHandle);
        _nativeHandle = IntPtr.Zero;
    }
    
    _disposed = true;
}
```

## OnApplyTemplate()

Override `OnApplyTemplate()` to locate template parts, initialize visual elements, and cache references to elements defined in the control template. This method is called when the template is applied to the control.

### Purpose
- Access template parts (elements with x:Name in template)
- Initialize visual elements
- Cache references for later use
- Set up bindings or event handlers on template elements

### Basic Override

```csharp
public override void OnApplyTemplate()
{
    base.OnApplyTemplate();
    
    // Access template part
    var animationHost = GetTemplateChild("PART_AnimationHost") as FrameworkElement;
    if (animationHost != null)
    {
        // Work with template part
    }
}
```

### Example: Accessing Storyboard Resources

```csharp
public class CustomBusyIndicator : SfBusyIndicator
{
    private Storyboard _animationStoryboard;
    private FrameworkElement _animationHost;
    
    public override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        
        // Find the animation host part
        _animationHost = GetTemplateChild("PART_AnimationHost") as FrameworkElement;
        
        if (_animationHost != null)
        {
            // Access storyboard from template resources
            _animationStoryboard = _animationHost.Resources["AnimationStoryboard"] as Storyboard;
            
            if (_animationStoryboard != null)
            {
                // Start animation if IsBusy is true
                if (this.IsBusy)
                {
                    _animationStoryboard.Begin(_animationHost, true);
                }
            }
        }
    }
}
```

### Example: Caching Multiple Template Parts

```csharp
public class AdvancedBusyIndicator : SfBusyIndicator
{
    private FrameworkElement _animationContainer;
    private TextBlock _headerTextBlock;
    private Border _backgroundBorder;
    
    public override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        
        // Cache all template parts
        _animationContainer = GetTemplateChild("PART_AnimationContainer") as FrameworkElement;
        _headerTextBlock = GetTemplateChild("PART_HeaderText") as TextBlock;
        _backgroundBorder = GetTemplateChild("PART_Background") as Border;
        
        // Initialize template parts
        if (_headerTextBlock != null)
        {
            _headerTextBlock.FontSize = 14;
        }
        
        if (_backgroundBorder != null)
        {
            _backgroundBorder.Opacity = 0.9;
        }
        
        // Attach event handlers to template elements
        if (_animationContainer != null)
        {
            _animationContainer.MouseEnter += OnAnimationMouseEnter;
            _animationContainer.MouseLeave += OnAnimationMouseLeave;
        }
    }
    
    private void OnAnimationMouseEnter(object sender, MouseEventArgs e)
    {
        // Pause animation on hover
    }
    
    private void OnAnimationMouseLeave(object sender, MouseEventArgs e)
    {
        // Resume animation
    }
}
```

### Template Part Contract

Define expected template parts using attributes:

```csharp
[TemplatePart(Name = "PART_AnimationHost", Type = typeof(FrameworkElement))]
[TemplatePart(Name = "PART_HeaderText", Type = typeof(TextBlock))]
public class DocumentedBusyIndicator : SfBusyIndicator
{
    public override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        
        var animationHost = GetTemplateChild("PART_AnimationHost") as FrameworkElement;
        var headerText = GetTemplateChild("PART_HeaderText") as TextBlock;
        
        // Initialize parts
    }
}
```

## OnAnimationTypeChanged()

Override this method to respond when the `AnimationType` property changes. Use it to swap templates, reload storyboard resources, or restart animations.

### Purpose
- React to animation type changes
- Load different animation resources
- Restart or reconfigure animations
- Update visual elements based on new animation type

### Basic Override

```csharp
protected override void OnAnimationTypeChanged(DependencyPropertyChangedEventArgs e)
{
    base.OnAnimationTypeChanged(e);
    
    // Handle animation type change
    var oldType = (AnimationTypes)e.OldValue;
    var newType = (AnimationTypes)e.NewValue;
    
    Debug.WriteLine($"Animation changed from {oldType} to {newType}");
}
```

### Example: Restarting Animation

```csharp
public class AutoRestartBusyIndicator : SfBusyIndicator
{
    private Storyboard _animationStoryboard;
    
    public override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        
        var host = GetTemplateChild("PART_AnimationHost") as FrameworkElement;
        if (host != null)
        {
            _animationStoryboard = host.Resources["AnimationStoryboard"] as Storyboard;
        }
    }
    
    protected override void OnAnimationTypeChanged(DependencyPropertyChangedEventArgs e)
    {
        base.OnAnimationTypeChanged(e);
        
        // Restart animation with new type
        RestartAnimation();
    }
    
    private void RestartAnimation()
    {
        if (_animationStoryboard == null || !this.IsBusy)
            return;
        
        // Stop current animation
        _animationStoryboard.Stop(this);
        
        // Restart with new animation type
        _animationStoryboard.Begin(this, true);
    }
}
```

### Example: Loading Different Resources

```csharp
public class DynamicResourceBusyIndicator : SfBusyIndicator
{
    protected override void OnAnimationTypeChanged(DependencyPropertyChangedEventArgs e)
    {
        base.OnAnimationTypeChanged(e);
        
        var newType = (AnimationTypes)e.NewValue;
        
        // Load animation-specific resources
        switch (newType)
        {
            case AnimationTypes.Flight:
                LoadFlightAnimationResources();
                break;
            case AnimationTypes.DoubleCircle:
                LoadCircleAnimationResources();
                break;
            case AnimationTypes.Ball:
                LoadBallAnimationResources();
                break;
            default:
                LoadDefaultAnimationResources();
                break;
        }
    }
    
    private void LoadFlightAnimationResources()
    {
        // Load flight-specific brushes, paths, etc.
        var resourceDict = new ResourceDictionary();
        resourceDict.Source = new Uri("/Resources/FlightAnimation.xaml", UriKind.Relative);
        this.Resources.MergedDictionaries.Add(resourceDict);
    }
    
    // Additional load methods...
}
```

### Important Notes

- Ensure the template provides appropriate resources for each animation type
- The `PART_AnimationHost` element should exist in the template
- Animation resources should be properly defined in the template

## OnPropertyChanged()

Override this method to react to dependency property changes such as `IsBusy`, enabling you to start/stop animations or update visual states.

### Purpose
- Monitor specific property changes
- Update animations based on property values
- Synchronize related properties
- Trigger visual state changes

### Basic Override

```csharp
protected override void OnPropertyChanged(DependencyPropertyChangedEventArgs e)
{
    base.OnPropertyChanged(e);
    
    // Check which property changed
    if (e.Property == SfBusyIndicator.IsBusyProperty)
    {
        // Handle IsBusy change
        var isBusy = (bool)e.NewValue;
        Debug.WriteLine($"IsBusy changed to: {isBusy}");
    }
}
```

### Example: Animation Control

```csharp
public class AnimationControlledBusyIndicator : SfBusyIndicator
{
    private Storyboard _animationStoryboard;
    
    public override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        
        var host = GetTemplateChild("PART_AnimationHost") as FrameworkElement;
        if (host != null)
        {
            _animationStoryboard = host.Resources["AnimationStoryboard"] as Storyboard;
        }
    }
    
    protected override void OnPropertyChanged(DependencyPropertyChangedEventArgs e)
    {
        base.OnPropertyChanged(e);
        
        if (e.Property == SfBusyIndicator.IsBusyProperty)
        {
            var isBusy = (bool)e.NewValue;
            
            if (isBusy)
            {
                // Start animation
                _animationStoryboard?.Begin(this, true);
            }
            else
            {
                // Stop animation
                _animationStoryboard?.Stop(this);
            }
        }
    }
}
```

### Example: Multiple Property Monitoring

```csharp
public class MultiPropertyBusyIndicator : SfBusyIndicator
{
    protected override void OnPropertyChanged(DependencyPropertyChangedEventArgs e)
    {
        base.OnPropertyChanged(e);
        
        if (e.Property == SfBusyIndicator.IsBusyProperty)
        {
            OnIsBusyChanged((bool)e.OldValue, (bool)e.NewValue);
        }
        else if (e.Property == SfBusyIndicator.AnimationTypeProperty)
        {
            OnAnimationTypePropertyChanged();
        }
        else if (e.Property == SfBusyIndicator.HeaderProperty)
        {
            OnHeaderChanged(e.OldValue, e.NewValue);
        }
    }
    
    private void OnIsBusyChanged(bool oldValue, bool newValue)
    {
        Debug.WriteLine($"IsBusy: {oldValue} → {newValue}");
        
        if (newValue)
        {
            StartAnimation();
            RaiseAnimationStartedEvent();
        }
        else
        {
            StopAnimation();
            RaiseAnimationStoppedEvent();
        }
    }
    
    private void OnAnimationTypePropertyChanged()
    {
        // Reload animation resources
    }
    
    private void OnHeaderChanged(object oldHeader, object newHeader)
    {
        // Update header display
    }
    
    // Custom events
    public event EventHandler AnimationStarted;
    public event EventHandler AnimationStopped;
    
    private void RaiseAnimationStartedEvent()
    {
        AnimationStarted?.Invoke(this, EventArgs.Empty);
    }
    
    private void RaiseAnimationStoppedEvent()
    {
        AnimationStopped?.Invoke(this, EventArgs.Empty);
    }
}
```

### Example: Visual State Management

```csharp
public class VisualStateBusyIndicator : SfBusyIndicator
{
    protected override void OnPropertyChanged(DependencyPropertyChangedEventArgs e)
    {
        base.OnPropertyChanged(e);
        
        if (e.Property == SfBusyIndicator.IsBusyProperty)
        {
            UpdateVisualState((bool)e.NewValue);
        }
    }
    
    private void UpdateVisualState(bool isBusy)
    {
        if (isBusy)
        {
            VisualStateManager.GoToState(this, "BusyState", true);
        }
        else
        {
            VisualStateManager.GoToState(this, "IdleState", true);
        }
    }
}
```

## Custom Control Scenarios

### Scenario 1: Timed Auto-Hide

Create a busy indicator that automatically hides after a timeout:

```csharp
public class TimedBusyIndicator : SfBusyIndicator
{
    private DispatcherTimer _autoHideTimer;
    
    public static readonly DependencyProperty AutoHideDelayProperty =
        DependencyProperty.Register("AutoHideDelay", typeof(TimeSpan), typeof(TimedBusyIndicator),
            new PropertyMetadata(TimeSpan.FromSeconds(30)));
    
    public TimeSpan AutoHideDelay
    {
        get => (TimeSpan)GetValue(AutoHideDelayProperty);
        set => SetValue(AutoHideDelayProperty, value);
    }
    
    public TimedBusyIndicator()
    {
        _autoHideTimer = new DispatcherTimer();
        _autoHideTimer.Tick += OnAutoHideTimerTick;
    }
    
    protected override void OnPropertyChanged(DependencyPropertyChangedEventArgs e)
    {
        base.OnPropertyChanged(e);
        
        if (e.Property == IsBusyProperty)
        {
            var isBusy = (bool)e.NewValue;
            
            if (isBusy)
            {
                // Start timer when busy
                _autoHideTimer.Interval = AutoHideDelay;
                _autoHideTimer.Start();
            }
            else
            {
                // Stop timer when not busy
                _autoHideTimer.Stop();
            }
        }
    }
    
    private void OnAutoHideTimerTick(object sender, EventArgs e)
    {
        _autoHideTimer.Stop();
        this.IsBusy = false;
    }
    
    protected override void Dispose(bool disposing)
    {
        if (disposing)
        {
            if (_autoHideTimer != null)
            {
                _autoHideTimer.Stop();
                _autoHideTimer.Tick -= OnAutoHideTimerTick;
                _autoHideTimer = null;
            }
        }
        base.Dispose(disposing);
    }
}
```

### Scenario 2: Progress-Aware Indicator

Combine busy indicator with progress tracking:

```csharp
public class ProgressBusyIndicator : SfBusyIndicator
{
    public static readonly DependencyProperty ProgressPercentageProperty =
        DependencyProperty.Register("ProgressPercentage", typeof(double), typeof(ProgressBusyIndicator),
            new PropertyMetadata(0.0, OnProgressChanged));
    
    public double ProgressPercentage
    {
        get => (double)GetValue(ProgressPercentageProperty);
        set => SetValue(ProgressPercentageProperty, value);
    }
    
    private static void OnProgressChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        var indicator = d as ProgressBusyIndicator;
        indicator?.UpdateProgressHeader();
    }
    
    private void UpdateProgressHeader()
    {
        if (ProgressPercentage >= 100)
        {
            this.Header = "Complete!";
            this.IsBusy = false;
        }
        else
        {
            this.Header = $"Progress: {ProgressPercentage:F0}%";
        }
    }
}
```

## Best Practices

1. **Always call base methods** - Call `base.OnApplyTemplate()`, `base.OnPropertyChanged()`, etc.
2. **Null-check template parts** - Template parts may not exist in custom templates
3. **Dispose resources properly** - Implement IDisposable pattern correctly
4. **Use try-finally in Dispose** - Ensure cleanup even if exceptions occur
5. **Cache template parts** - Store references to frequently accessed elements
6. **Don't assume template structure** - Your control may be re-templated
7. **Detach event handlers** - Prevent memory leaks by removing handlers in Dispose
8. **Test with custom templates** - Verify your overrides work with different templates

## Troubleshooting

**Issue:** GetTemplateChild returns null
- **Solution:** Ensure template part name matches exactly and OnApplyTemplate has been called

**Issue:** Animation doesn't restart after animation type change
- **Solution:** Implement OnAnimationTypeChanged to restart animation properly

**Issue:** Memory leaks in custom control
- **Solution:** Ensure Dispose properly detaches all event handlers and releases resources

**Issue:** Property changes not detected
- **Solution:** Verify you're checking the correct DependencyProperty in OnPropertyChanged

**Issue:** Storyboard not found in OnApplyTemplate
- **Solution:** Check that storyboard exists in template resources with correct key

## Related Topics

- **Getting Started** - Basic control setup before extending
- **IsBusy Property** - Primary property monitored in OnPropertyChanged
- **Animation Types** - Types managed through OnAnimationTypeChanged
- **Header Customization** - Can be managed through OnPropertyChanged

# Segments and Custom Content

## Table of Contents
- [Segments Overview](#segments-overview)
- [SegmentCount](#segmentcount)
- [Custom Content Overview](#custom-content-overview)
- [Binding Progress Text to Center](#binding-progress-text-to-center)
- [Play/Pause Button Pattern](#playpause-button-pattern)
- [Combining ProgressContent with Radius](#combining-progresscontent-with-radius)

---

## Segments Overview

Segmentation splits the circular progress ring into discrete visual sections separated by gaps. Use it to represent step-based workflows, multi-phase tasks, or visually divided progress (e.g., 4 quarters, 10 steps).

---

## SegmentCount

Set `SegmentCount` to divide the ring:

```xaml
<Syncfusion:SfCircularProgressBar
    Progress="70"
    SegmentCount="4"
    Width="150" Height="150" />
```

```csharp
SfCircularProgressBar circular = new SfCircularProgressBar();
circular.Progress = 70;
circular.SegmentCount = 4;
grid.Children.Add(circular);
```

- `SegmentCount="4"` — divides the ring into 4 equal segments with gaps between them
- `SegmentCount="0"` — no segmentation (default, continuous ring)
- Progress fills segments sequentially as `Progress` value increases

**Use cases:**
- Multi-step wizard (SegmentCount = number of steps)
- Battery level indicator (SegmentCount = 4 bars)
- Quiz progress (SegmentCount = number of questions)

---

## Custom Content Overview

The `ProgressContent` property places any WPF UI element at the center of the circular progress ring. Use it for:
- Custom text (percentage, label, status)
- Action buttons (play, pause, stop)
- Images or icons representing the task
- Any composite UI panel

**Important:** Set `ShowProgressValue="False"` when using `ProgressContent` to avoid overlapping with the built-in percentage text.

---

## Binding Progress Text to Center

Display a custom percentage label in the center, bound to the control's own `Progress` property using `x:Reference`:

```xaml
<Syncfusion:SfCircularProgressBar
    x:Name="CustomContentBar"
    Progress="70"
    ShowProgressValue="False"
    Width="150" Height="150">
    <Syncfusion:SfCircularProgressBar.ProgressContent>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <TextBlock
                Text="{Binding Progress, StringFormat={}{0}%, ElementName=CustomContentBar}"
                TextAlignment="Center"
                FontSize="16"
                FontWeight="Bold" />
            <TextBlock
                Text="Used"
                TextAlignment="Center"
                FontSize="12" />
        </StackPanel>
    </Syncfusion:SfCircularProgressBar.ProgressContent>
</Syncfusion:SfCircularProgressBar>
```

**Alternative — DataContext binding:**
```xaml
<Syncfusion:SfCircularProgressBar
    x:Name="CustomContentProgressBarLevel"
    Progress="70"
    ShowProgressValue="False">
    <Syncfusion:SfCircularProgressBar.ProgressContent>
        <TextBlock
            Text="{Binding Progress, StringFormat={}{0}%}"
            DataContext="{x:Reference CustomContentProgressBarLevel}"
            TextAlignment="Center" />
    </Syncfusion:SfCircularProgressBar.ProgressContent>
</Syncfusion:SfCircularProgressBar>
```

---

## Play/Pause Button Pattern

Place interactive buttons in the center to control progress. Uses `DispatcherTimer` in the ViewModel to drive progress automatically:

**ViewModel:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    public DispatcherTimer PlayPauseTimer { get; }
    private int playPauseProgress;

    public int PlayPauseProgress
    {
        get => playPauseProgress;
        set { playPauseProgress = value; RaisePropertyChanged(nameof(PlayPauseProgress)); }
    }

    public ViewModel()
    {
        PlayPauseTimer = new DispatcherTimer
        {
            Interval = TimeSpan.FromMilliseconds(30)
        };
        PlayPauseTimer.Tick += (s, e) =>
        {
            PlayPauseProgress = PlayPauseProgress < 100 ? PlayPauseProgress + 1 : 0;
        };
        PlayPauseTimer.Start();
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void RaisePropertyChanged(string name)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**XAML:**
```xaml
<Window.DataContext>
    <local:ViewModel />
</Window.DataContext>

<Syncfusion:SfCircularProgressBar
    Progress="{Binding PlayPauseProgress, Mode=TwoWay}"
    TrackInnerRadius="0"
    IndicatorOuterRadius="0.7"
    IndicatorInnerRadius="0.65"
    ShowProgressValue="False"
    Width="150" Height="150">
    <Syncfusion:SfCircularProgressBar.ProgressContent>
        <Grid>
            <Button x:Name="PlayButton"
                    Visibility="Collapsed"
                    Click="PlayButton_Click"
                    Background="Transparent"
                    Width="30" Height="30">
                <TextBlock Text="▶" FontSize="16" />
            </Button>
            <Button x:Name="PauseButton"
                    Click="PauseButton_Click"
                    Background="Transparent"
                    Width="30" Height="30">
                <TextBlock Text="⏸" FontSize="16" />
            </Button>
        </Grid>
    </Syncfusion:SfCircularProgressBar.ProgressContent>
</Syncfusion:SfCircularProgressBar>
```

**Code-behind:**
```csharp
private void PlayButton_Click(object sender, RoutedEventArgs e)
{
    PlayButton.Visibility = Visibility.Collapsed;
    PauseButton.Visibility = Visibility.Visible;
    (DataContext as ViewModel)?.PlayPauseTimer.Start();
}

private void PauseButton_Click(object sender, RoutedEventArgs e)
{
    PauseButton.Visibility = Visibility.Collapsed;
    PlayButton.Visibility = Visibility.Visible;
    (DataContext as ViewModel)?.PlayPauseTimer.Stop();
}
```

---

## Combining ProgressContent with Radius

For a ring style with a centered text display:

```xaml
<Syncfusion:SfCircularProgressBar
    x:Name="RingBar"
    Progress="80"
    IndicatorOuterRadius="0.85"
    IndicatorInnerRadius="0.7"
    TrackOuterRadius="0.85"
    TrackInnerRadius="0.7"
    ShowProgressValue="False"
    Width="160" Height="160">
    <Syncfusion:SfCircularProgressBar.ProgressContent>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <TextBlock
                Text="{Binding Progress, StringFormat={}{0}%, ElementName=RingBar}"
                FontSize="18" FontWeight="Bold"
                TextAlignment="Center" />
            <TextBlock Text="Complete" FontSize="11" TextAlignment="Center" />
        </StackPanel>
    </Syncfusion:SfCircularProgressBar.ProgressContent>
</Syncfusion:SfCircularProgressBar>
```

---

## Gotchas

- **`ShowProgressValue` must be `False`** when using `ProgressContent` — otherwise both the built-in percentage text and custom content render simultaneously and overlap.
- **`x:Reference` binding** — using `DataContext="{x:Reference ControlName}"` or `ElementName=ControlName` is the cleanest way to bind `ProgressContent` text to the progress value without a separate ViewModel property.
- **`SegmentCount` gaps are visual only** — the `Progress` value still operates on the 0–100 scale; segments are purely cosmetic divisions.
- **`DispatcherTimer` runs on the UI thread** — it is safe to update bound properties directly without `Dispatcher.Invoke`.

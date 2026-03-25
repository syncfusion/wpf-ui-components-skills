# Selected Item & Status

## SelectedIndex

`SelectedIndex` sets which step is the last active (selected) step. All steps before it become `Active`; the step at the index takes the `SelectedItemStatus` value; steps after it become `Inactive`.

```xaml
<Syncfusion:SfStepProgressBar SelectedIndex="2">
    <Syncfusion:StepViewItem Content="Ordered" />   <!-- Active -->
    <Syncfusion:StepViewItem Content="Packed" />    <!-- Active -->
    <Syncfusion:StepViewItem Content="Shipped" />   <!-- SelectedItemStatus (default Inactive) -->
    <Syncfusion:StepViewItem Content="Delivered" /> <!-- Inactive -->
</Syncfusion:SfStepProgressBar>
```

```csharp
stepProgressBar.SelectedIndex = 2;
```

---

## SelectedItemStatus

Controls the visual status of the step at `SelectedIndex`. Default: `Inactive`.

| Value | Meaning | Use When |
|---|---|---|
| `Active` | Completed — filled marker | Step is fully done |
| `Inactive` | Not started — empty marker | Step not yet reached |
| `Indeterminate` | In progress — animated/pulsing | Step is currently in progress |

```xaml
<Syncfusion:SfStepProgressBar SelectedIndex="2" SelectedItemStatus="Indeterminate">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

```csharp
stepProgressBar.SelectedIndex = 2;
stepProgressBar.SelectedItemStatus = StepStatus.Indeterminate;
```

**Decision guide:**
- Order placed and confirmed → `Active`
- Order currently being processed/shipped → `Indeterminate`
- Order not yet reached that stage → `Inactive`

---

## SelectedItemProgress

Sets the partial fill percentage (0–100) on the selected step. Only visually meaningful when `SelectedItemStatus="Indeterminate"`. Default: `100`.

```xaml
<Syncfusion:SfStepProgressBar
    SelectedIndex="2"
    SelectedItemStatus="Indeterminate"
    SelectedItemProgress="50">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

```csharp
stepProgressBar.SelectedIndex = 2;
stepProgressBar.SelectedItemStatus = StepStatus.Indeterminate;
stepProgressBar.SelectedItemProgress = 50;
```

Use `SelectedItemProgress` to show how far along the current step is (e.g., file upload at 50%).

---

## AnimationDuration

Controls how long the step transition animation takes. Default: `500ms`.

```xaml
<Syncfusion:SfStepProgressBar
    SelectedIndex="3"
    AnimationDuration="0:0:1"
    Orientation="Vertical">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

```csharp
stepProgressBar.AnimationDuration = new TimeSpan(0, 0, 1); // 1 second
```

- Set to `0:0:0` to disable animation entirely
- Slower animations (1–2s) work well for onboarding/wizard UX to draw attention

---

## MVVM Pattern — Dynamic Step Advancement

Bind `SelectedIndex` and `SelectedItemStatus` to a ViewModel for programmatic step control:

```csharp
// ViewModel.cs
public class OrderViewModel : INotifyPropertyChanged
{
    private int _selectedIndex;
    private StepStatus _selectedItemStatus = StepStatus.Indeterminate;

    public int SelectedIndex
    {
        get => _selectedIndex;
        set { _selectedIndex = value; OnPropertyChanged(nameof(SelectedIndex)); }
    }

    public StepStatus SelectedItemStatus
    {
        get => _selectedItemStatus;
        set { _selectedItemStatus = value; OnPropertyChanged(nameof(SelectedItemStatus)); }
    }

    public ICommand NextStepCommand => new RelayCommand(() =>
    {
        SelectedItemStatus = StepStatus.Active; // complete current
        SelectedIndex++;
        SelectedItemStatus = StepStatus.Indeterminate; // mark new as in-progress
    });

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

```xaml
<Syncfusion:SfStepProgressBar
    SelectedIndex="{Binding SelectedIndex}"
    SelectedItemStatus="{Binding SelectedItemStatus}">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

---

## Gotchas

- **`SelectedItemProgress` without `Indeterminate`** — setting it when `SelectedItemStatus` is `Active` or `Inactive` has no visible effect; always pair with `Indeterminate`
- **`SelectedIndex` out of range** — setting `SelectedIndex` beyond item count is silently ignored; validate bounds in MVVM before binding
- **Advancing steps** — to move a step from Indeterminate → Active, set `SelectedItemStatus = Active` first, then increment `SelectedIndex` and set `Indeterminate` again for the new current step

# Getting Started

## Assembly Setup

**NuGet package:** `Syncfusion.SfRadialMenu.WPF`

Or add assembly references manually:
- `Syncfusion.SfRadialMenu.WPF.dll`
- `Syncfusion.SfShared.WPF.dll`

**Namespace:**
```csharp
using Syncfusion.Windows.Controls.Navigation;
```

**XAML namespace:**
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

---

## Add via Designer

Drag `SfRadialSlider` from the Toolbox onto the designer. The following assemblies are added automatically:
- `Syncfusion.SfRadialMenu.WPF`
- `Syncfusion.SfShared.WPF`

---

## Add via XAML

```xaml
<Window x:Class="RadialSliderSample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="SfRadialSlider Sample" Height="450" Width="800">
    <Grid>
        <syncfusion:SfRadialSlider Name="radialSlider"
                                   Height="200"
                                   Width="200"/>
    </Grid>
</Window>
```

---

## Add via C\#

```csharp
using Syncfusion.Windows.Controls.Navigation;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        SfRadialSlider radialSlider = new SfRadialSlider();
        radialSlider.Height = 200;
        radialSlider.Width  = 200;
        this.Content = radialSlider;
    }
}
```

---

## Select a Value

The user selects a value by **dragging the pointer** along the circular track or **clicking** any tick on the track. Read the selected value via the `Value` property (default: `0`).

### Programmatic Value Assignment

```xaml
<syncfusion:SfRadialSlider Value="34" Name="radialSlider"/>
```

```csharp
radialSlider.Value = 34;
```

---

## Display Selected Value in the Center

Bind `Content` and `Value` to the same ViewModel property so the inner rim shows the current value:

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private double _selectedValue;
    public double SelectedValue
    {
        get => _selectedValue;
        set { _selectedValue = value; OnPropertyChanged(nameof(SelectedValue)); }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

```xaml
<syncfusion:SfRadialSlider Content="{Binding SelectedValue, Mode=TwoWay}"
                           Value="{Binding SelectedValue, Mode=TwoWay}"
                           Height="200" Width="200">
    <syncfusion:SfRadialSlider.DataContext>
        <local:ViewModel/>
    </syncfusion:SfRadialSlider.DataContext>
</syncfusion:SfRadialSlider>
```

---

## ValueChanged Event

```xaml
<syncfusion:SfRadialSlider ValueChanged="RadialSlider_ValueChanged"
                           Name="radialSlider"/>
```

```csharp
private void RadialSlider_ValueChanged(object sender,
    RoutedPropertyChangedEventArgs<double> e)
{
    double oldValue = e.OldValue;
    double newValue = e.NewValue;
    // React to value change
}
```

---

## Themes with SfSkinManager

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<syncfusion:SfRadialSlider
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentLight}"
    Height="200" Width="200"/>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `SfRadialSlider` not found in XAML | Missing assembly references | Add both `Syncfusion.SfRadialMenu.WPF` and `Syncfusion.SfShared.WPF` |
| Control appears tiny | No `Height`/`Width` set | Set explicit `Height` and `Width` (e.g., 200×200) |
| Center content blank | `Content` not bound to `Value` | Bind `Content="{Binding SelectedValue}"` alongside `Value="{Binding SelectedValue}"` |
| Value jumps to unexpected steps | `SmallChange` default is `0.1` | Set `SmallChange="1"` or desired step |

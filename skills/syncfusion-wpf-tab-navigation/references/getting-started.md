# Getting Started with TabNavigationControl

## Assembly References

Add both assemblies to your WPF project:

```
Syncfusion.Tools.WPF
Syncfusion.Shared.WPF
```

**In `.csproj`:**
```xml
<Reference Include="Syncfusion.Tools.WPF">
    <HintPath>path\to\Syncfusion.Tools.WPF.dll</HintPath>
</Reference>
<Reference Include="Syncfusion.Shared.WPF">
    <HintPath>path\to\Syncfusion.Shared.WPF.dll</HintPath>
</Reference>
```

**Or via NuGet:**
```
Install-Package Syncfusion.Tools.WPF
```

> **Note:** Both assemblies are required. Missing `Syncfusion.Shared.WPF` will cause runtime errors even if XAML compiles.

---

## XAML Namespace

Use the Syncfusion WPF schema (covers all Syncfusion WPF controls):

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

---

## Adding Control in XAML

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <syncfusion:TabNavigationControl x:Name="tabNavigation"
                                          TransitionEffect="Slide"
                                          Width="400" Height="300">
            <syncfusion:TabNavigationItem Header="1">
                <syncfusion:TabNavigationItem.Content>
                    <Grid>
                        <TextBlock Text="Item 1" HorizontalAlignment="Center"
                                   VerticalAlignment="Center" FontSize="20"/>
                    </Grid>
                </syncfusion:TabNavigationItem.Content>
            </syncfusion:TabNavigationItem>
            <syncfusion:TabNavigationItem Header="2">
                <syncfusion:TabNavigationItem.Content>
                    <Grid>
                        <TextBlock Text="Item 2" HorizontalAlignment="Center"
                                   VerticalAlignment="Center" FontSize="20"/>
                    </Grid>
                </syncfusion:TabNavigationItem.Content>
            </syncfusion:TabNavigationItem>
            <syncfusion:TabNavigationItem Header="3">
                <syncfusion:TabNavigationItem.Content>
                    <Grid>
                        <TextBlock Text="Item 3" HorizontalAlignment="Center"
                                   VerticalAlignment="Center" FontSize="20"/>
                    </Grid>
                </syncfusion:TabNavigationItem.Content>
            </syncfusion:TabNavigationItem>
        </syncfusion:TabNavigationControl>
    </Grid>
</Window>
```

---

## Adding Control in C#

```csharp
using Syncfusion.Windows.Tools.Controls;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        TabNavigationControl tabNavigation = new TabNavigationControl();
        tabNavigation.Width  = 400;
        tabNavigation.Height = 300;
        tabNavigation.TransitionEffect = TransitionEffect.Slide;

        TabNavigationItem item1 = new TabNavigationItem();
        item1.Header  = "1";
        item1.Content = "Item 1";

        TabNavigationItem item2 = new TabNavigationItem();
        item2.Header  = "2";
        item2.Content = "Item 2";

        TabNavigationItem item3 = new TabNavigationItem();
        item3.Header  = "3";
        item3.Content = "Item 3";

        TabNavigationItem item4 = new TabNavigationItem();
        item4.Header  = "4";
        item4.Content = "Item 4";

        tabNavigation.Items.Add(item1);
        tabNavigation.Items.Add(item2);
        tabNavigation.Items.Add(item3);
        tabNavigation.Items.Add(item4);

        this.Content = tabNavigation;
    }
}
```

---

## Applying Themes

Use `SfSkinManager` to apply a consistent Syncfusion visual style:

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
syncfusion:SfSkinManager.VisualStyle="FluentLight"
```

```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetVisualStyle(tabNavigation, VisualStyles.FluentLight);
```

**Supported styles:** `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Office2019Colorful`, and more.

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| XAML design-time error | Wrong XAML namespace | Use `http://schemas.syncfusion.com/wpf` |
| Runtime assembly exception | Missing `Syncfusion.Shared.WPF` | Add both assemblies as references |
| Items show no transition | `TransitionEffect` not set | Add `TransitionEffect="Slide"` (or other value) |
| Content not visible | `TabStripVisibility` set to `Collapsed` without alternate navigation | Keep tab strip visible or provide custom navigation |

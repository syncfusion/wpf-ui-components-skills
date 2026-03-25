# Getting Started with SfTreeNavigator

## Assembly Reference

Add the following assembly to your WPF project:

```
Syncfusion.SfTreeNavigator.WPF
```

**In `.csproj`:**
```xml
<Reference Include="Syncfusion.SfTreeNavigator.WPF">
    <HintPath>path\to\Syncfusion.SfTreeNavigator.WPF.dll</HintPath>
</Reference>
```

**Or via NuGet:**
```
Install-Package Syncfusion.SfTreeNavigator.WPF
```

---

## XAML Namespace

```xaml
xmlns:navigation="clr-namespace:Syncfusion.Windows.Controls.Navigation;assembly=Syncfusion.SfTreeNavigator.WPF"
```

---

## Declarative XAML Example

Use `SfTreeNavigator` as the container and nest `SfTreeNavigatorItem` elements at any depth.

```xaml
<Window xmlns:navigation="clr-namespace:Syncfusion.Windows.Controls.Navigation;assembly=Syncfusion.SfTreeNavigator.WPF">
    <Grid>
        <navigation:SfTreeNavigator Header="Syncfusion"
                                     Width="300" Height="400"
                                     VerticalAlignment="Top"
                                     HorizontalAlignment="Left">
            <navigation:SfTreeNavigatorItem Header="WinRT (XAML)">
                <navigation:SfTreeNavigatorItem Header="Grid"/>
                <navigation:SfTreeNavigatorItem Header="Chart"/>
                <navigation:SfTreeNavigatorItem Header="Tools">
                    <navigation:SfTreeNavigatorItem Header="TabControl"/>
                    <navigation:SfTreeNavigatorItem Header="Docking Manager"/>
                </navigation:SfTreeNavigatorItem>
            </navigation:SfTreeNavigatorItem>
            <navigation:SfTreeNavigatorItem Header="Metro Studio"/>
            <navigation:SfTreeNavigatorItem Header="Help Desk"/>
        </navigation:SfTreeNavigator>
    </Grid>
</Window>
```

**How it works:**
- The top-level text "Syncfusion" is the `Header` on `SfTreeNavigator` — always shown at root
- Clicking an item with children drills down in-place
- A back button appears at top to return to the previous level

---

## C# Code-Behind Creation

```csharp
using Syncfusion.Windows.Controls.Navigation;

SfTreeNavigator navigator = new SfTreeNavigator();
navigator.Header = "Syncfusion";
navigator.Width  = 300;
navigator.Height = 400;

SfTreeNavigatorItem winrt = new SfTreeNavigatorItem { Header = "WinRT (XAML)" };
SfTreeNavigatorItem grid  = new SfTreeNavigatorItem { Header = "Grid" };
SfTreeNavigatorItem chart = new SfTreeNavigatorItem { Header = "Chart" };
winrt.Items.Add(grid);
winrt.Items.Add(chart);

SfTreeNavigatorItem metro = new SfTreeNavigatorItem { Header = "Metro Studio" };

navigator.Items.Add(winrt);
navigator.Items.Add(metro);

this.Content = navigator;
```

---

## Applying Themes

Use `SfSkinManager` to apply a consistent Syncfusion theme:

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
syncfusion:SfSkinManager.VisualStyle="FluentLight"
```

```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetVisualStyle(treeNavigator, VisualStyles.FluentLight);
```

**Supported styles:** `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Office2019Colorful`, etc.

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Items not visible | Assembly reference missing | Add `Syncfusion.SfTreeNavigator.WPF` |
| XAML namespace error | Wrong assembly name in xmlns | Use `assembly=Syncfusion.SfTreeNavigator.WPF` |
| No back button visible | No nested children exist | Back button only appears when drilled into a child level |

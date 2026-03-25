# Getting Started

## Assembly Setup

**NuGet package:** `Syncfusion.SfAccordion.WPF`

Or add assembly reference manually:
- `Syncfusion.SfAccordion.WPF.dll`

**Namespace:**
```csharp
using Syncfusion.Windows.Controls.Layout;
```

**XAML namespace:**
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

---

## Add via Designer

Drag `SfAccordion` from the Toolbox onto the designer surface. The `Syncfusion.SfAccordion.WPF` assembly reference is added automatically.

---

## Add via XAML

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        x:Class="AccordionSample.MainWindow"
        Title="SfAccordion Sample" Height="350" Width="525">
    <Grid>
        <syncfusion:SfAccordion x:Name="accordion"
                                HorizontalAlignment="Center"
                                VerticalAlignment="Center"
                                Width="300" Height="250">
            <syncfusion:SfAccordionItem Header="WPF"
                                        Content="Essential Studio for WPF"/>
            <syncfusion:SfAccordionItem Header="Silverlight"
                                        Content="Essential Studio for Silverlight"/>
            <syncfusion:SfAccordionItem Header="WinRT"
                                        Content="Essential Studio for WinRT"/>
            <syncfusion:SfAccordionItem Header="Windows Phone"
                                        Content="Essential Studio for Windows Phone"/>
        </syncfusion:SfAccordion>
    </Grid>
</Window>
```

---

## Add via C\#

```csharp
using Syncfusion.Windows.Controls.Layout;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        SfAccordion accordion = new SfAccordion();
        accordion.Width  = 300;
        accordion.Height = 250;

        accordion.Items.Add(new SfAccordionItem { Header = "WPF",          Content = "Essential Studio for WPF" });
        accordion.Items.Add(new SfAccordionItem { Header = "Silverlight",   Content = "Essential Studio for Silverlight" });
        accordion.Items.Add(new SfAccordionItem { Header = "WinRT",         Content = "Essential Studio for WinRT" });
        accordion.Items.Add(new SfAccordionItem { Header = "Windows Phone", Content = "Essential Studio for Windows Phone" });

        this.Content = accordion;
    }
}
```

---

## Expand an Item by Default

Set `IsSelected="True"` on the desired `SfAccordionItem`:

```xaml
<syncfusion:SfAccordionItem Header="WPF"
                             Content="Essential Studio for WPF"
                             IsSelected="True"/>
```

```csharp
accordion.Items.Add(new SfAccordionItem
{
    Header     = "WPF",
    Content    = "Essential Studio for WPF",
    IsSelected = true
});
```

> **Default behavior:** In `SelectionMode="One"` (default), the first item is expanded automatically if no item has `IsSelected=true`.

---

## Themes with SfSkinManager

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<syncfusion:SfAccordion
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentLight}"
    Width="300" Height="250">
    <syncfusion:SfAccordionItem Header="WPF" Content="Essential Studio for WPF"/>
</syncfusion:SfAccordion>
```

Apply to the whole window:
```xaml
<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `SfAccordion` not found in XAML | Missing assembly reference | Add `Syncfusion.SfAccordion.WPF` reference |
| No items visible | `Width`/`Height` not set | Set explicit dimensions or use `HorizontalAlignment="Stretch"` |
| First item won't collapse | Default `SelectionMode="One"` locks one item | Switch to `ZeroOrOne` or `ZeroOrMore` if collapsing all is needed |

# Getting Started with WPF ButtonAdv

## Table of Contents
- [Assembly Setup](#assembly-setup)
- [Adding via Designer](#adding-via-designer)
- [Adding via XAML](#adding-via-xaml)
- [Adding via C#](#adding-via-c)
- [Setting the Label](#setting-the-label)
- [Setting Corner Radius](#setting-corner-radius)
- [IsDefault and IsCancel Modes](#isdefault-and-iscancel-modes)
- [Icon Priority Order](#icon-priority-order)

---

## Assembly Setup

**Required assembly:** `Syncfusion.Shared.WPF`

Install via NuGet:
```
Install-Package Syncfusion.Tools.WPF
```

Or add project reference to `Syncfusion.Shared.WPF.dll` directly.

**XAML namespace declaration:**
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

Or use the control namespace directly:
```xaml
xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Tools.Controls;assembly=Syncfusion.Shared.WPF"
```

---

## Adding via Designer

Drag **ButtonAdv** from the Visual Studio Toolbox and drop it into the designer canvas.

- The assembly `Syncfusion.Shared.WPF` is added automatically.
- The following XAML is generated:

```xaml
<syncfusion:ButtonAdv x:Name="buttonAdv" Label="ButtonAdv"/>
```

> **Note:** `syncfusion` is an auto-generated namespace alias in the XAML.

---

## Adding via XAML

Add the assembly reference, import the namespace, then declare the control:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <syncfusion:ButtonAdv Height="44"
                              Width="162"
                              VerticalAlignment="Center"
                              HorizontalAlignment="Center"/>
    </Grid>
</Window>
```

---

## Adding via C#

Import the namespace, create the instance, and add to a panel:

```csharp
using Syncfusion.Windows.Tools.Controls;
using System.Windows.Media.Imaging;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        ButtonAdv button = new ButtonAdv();
        button.Height = 44;
        button.Width = 162;
        Root.Children.Add(button);  // Root is a named Grid or Panel in XAML
    }
}
```

---

## Setting the Label

The `Label` property sets the display text on the button. Use it instead of the standard WPF `Content` property.

**XAML:**
```xaml
<syncfusion:ButtonAdv Label="Log in"
                      SmallIcon="Images/user.png"
                      SizeMode="Normal"/>
```

**C#:**
```csharp
ButtonAdv button = new ButtonAdv();
button.Label = "Log in";
button.SmallIcon = new BitmapImage(new Uri("Images/user.png", UriKind.RelativeOrAbsolute));
```

> **Why Label instead of Content?** ButtonAdv uses `Label` to support its layout system across
> size modes (icon-only in Small, icon+text-side in Normal, icon+text-below in Large). Using the
> base WPF `Content` property bypasses this layout.

---

## Setting Corner Radius

Use `CornerRadius` to give the button rounded corners. The default value is `3`.

**XAML:**
```xaml
<syncfusion:ButtonAdv Label="Submit"
                      SmallIcon="Images/submit.png"
                      CornerRadius="15"
                      SizeMode="Normal"
                      IconHeight="25"
                      IconWidth="30"/>
```

**C#:**
```csharp
ButtonAdv button = new ButtonAdv();
button.Label = "Submit";
button.CornerRadius = new CornerRadius(15);
button.IconWidth = 30;
button.IconHeight = 25;
button.SmallIcon = new BitmapImage(new Uri("Images/submit.png", UriKind.RelativeOrAbsolute));
```

Set individual corners using the full constructor:
```csharp
button.CornerRadius = new CornerRadius(topLeft: 10, topRight: 10, bottomRight: 0, bottomLeft: 0);
```

---

## IsDefault and IsCancel Modes

These modes let users trigger buttons via keyboard shortcuts — useful for dialog boxes.

### IsDefault — Enter Key

When `IsDefault="True"`, the button is activated when the user presses **Enter**, even if the button does not have focus.

```xaml
<syncfusion:ButtonAdv x:Name="okButton"
                      Label="OK"
                      IsDefault="True"
                      Click="OkButton_Click"/>
```

### IsCancel — Escape Key

When `IsCancel="True"`, the button is activated when the user presses **Escape**.

```xaml
<syncfusion:ButtonAdv x:Name="cancelButton"
                      Label="Cancel"
                      IsCancel="True"
                      Click="CancelButton_Click"/>
```

### Dialog Pattern

```xaml
<StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
    <syncfusion:ButtonAdv Label="OK"     IsDefault="True"  Click="Ok_Click"     Margin="0,0,8,0"/>
    <syncfusion:ButtonAdv Label="Cancel" IsCancel="True"   Click="Cancel_Click"/>
</StackPanel>
```

---

## Icon Priority Order

ButtonAdv resolves which icon to display based on this priority (highest to lowest):

1. `IconTemplateSelector` — dynamic template selector
2. `IconTemplate` — DataTemplate with vector/font icon
3. `LargeIcon` — image for Large size mode
4. `SmallIcon` — image for Small/Normal size modes

If a higher-priority source is set, lower-priority sources are ignored. This means if you set
`IconTemplate`, the `SmallIcon` or `LargeIcon` will not be shown.

```xaml
<!-- IconTemplate takes priority over SmallIcon here -->
<syncfusion:ButtonAdv Label="User" SizeMode="Normal" SmallIcon="Images/user.png">
    <syncfusion:ButtonAdv.IconTemplate>
        <DataTemplate>
            <Grid Width="16" Height="16">
                <Path Data="..." Fill="#FF3A3A38" Stretch="Fill"/>
            </Grid>
        </DataTemplate>
    </syncfusion:ButtonAdv.IconTemplate>
</syncfusion:ButtonAdv>
```

> For `IconTemplate` and `IconTemplateSelector` details, see
> [size-modes-and-icons.md](size-modes-and-icons.md).

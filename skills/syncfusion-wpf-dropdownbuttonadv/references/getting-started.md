# Getting Started with DropDownButtonAdv

## Assembly Setup

Add reference to:
- `Syncfusion.Shared.WPF`

**NuGet:** `Syncfusion.Tools.WPF` package (includes Shared.WPF)

Import namespace in XAML:
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

Import namespace in C#:
```csharp
using Syncfusion.Windows.Tools.Controls;
```

---

## Adding the Control

### Via Designer (Toolbox)

Drag **DropDownButtonAdv** from the toolbox onto the designer surface. The assembly reference is added automatically and the following XAML is generated:

```xaml
<syncfusion:DropDownButtonAdv x:Name="dropdownButtonAdv" Label="Drop Down Button"/>
```

### Via XAML (Manual)

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <syncfusion:DropDownButtonAdv Height="44"
                                      Width="162"
                                      Label="Country"
                                      VerticalAlignment="Center"
                                      HorizontalAlignment="Center" />
    </Grid>
</Window>
```

### Via C# (Code-Behind)

```csharp
using Syncfusion.Windows.Tools.Controls;

DropDownButtonAdv dropdownButtonAdv = new DropDownButtonAdv();
dropdownButtonAdv.Height = 44;
dropdownButtonAdv.Width = 162;
dropdownButtonAdv.Label = "Country";
Root.Children.Add(dropdownButtonAdv);
```

---

## Setting the Label

The `Label` property sets the button's visible text:

```xaml
<syncfusion:DropDownButtonAdv Label="Country" SmallIcon="Images/flag.png"/>
```

```csharp
button.Label = "Country";
```

---

## Size Modes

Use `SizeMode` to control the button's visual size. The `SizeMode` enum has three values:

| SizeMode | Icon | Label | Icon Property |
|---|---|---|---|
| `Small` | ✓ (only) | Hidden | `SmallIcon` |
| `Normal` | ✓ (small, left) | ✓ | `SmallIcon` |
| `Large` | ✓ (large, top) | ✓ (bottom) | `LargeIcon` |

```xaml
<!-- Small: icon only, no label -->
<syncfusion:DropDownButtonAdv SizeMode="Small" SmallIcon="Images/flag.png" Label="Country"/>

<!-- Normal: small icon with label -->
<syncfusion:DropDownButtonAdv SizeMode="Normal" SmallIcon="Images/flag.png" Label="Country"/>

<!-- Large: large icon above label -->
<syncfusion:DropDownButtonAdv SizeMode="Large" LargeIcon="Images/flaglarge.png" Label="Country"/>
```

---

## Setting Icons

### Image-Based Icons

```xaml
<!-- SmallIcon for Small/Normal modes -->
<syncfusion:DropDownButtonAdv SizeMode="Normal" SmallIcon="Images/flag.png" Label="Country"/>

<!-- LargeIcon for Large mode -->
<syncfusion:DropDownButtonAdv SizeMode="Large" LargeIcon="Images/flaglarge.png" Label="Country"/>
```

```csharp
button.SmallIcon = new BitmapImage(new Uri("Images/flag.png", UriKind.RelativeOrAbsolute));
button.LargeIcon = new BitmapImage(new Uri("Images/flaglarge.png", UriKind.RelativeOrAbsolute));
```

**Icon priority order:** `IconTemplateSelector` → `IconTemplate` → `LargeIcon` → `SmallIcon`

### Custom Icon Size

```xaml
<syncfusion:DropDownButtonAdv SizeMode="Normal"
                               SmallIcon="Images/flag.png"
                               IconWidth="24"
                               IconHeight="24"
                               Label="Country"/>
```

### Vector/Path Icon Template

Use `IconTemplate` to display vector graphics or font icons instead of image files. The template auto-scales to fit:

```xaml
<Window.Resources>
    <DataTemplate x:Key="countryIconTemplate">
        <Grid Width="16" Height="16">
            <Path Data="M0,0 L16,8 0,16 Z" Fill="#FF3A3A38" Stretch="Fill"/>
        </Grid>
    </DataTemplate>
</Window.Resources>

<syncfusion:DropDownButtonAdv SizeMode="Normal"
                               Label="Country"
                               IconTemplate="{StaticResource countryIconTemplate}"/>
```

### Dynamic Icon Template Selector

When icon should change based on data state:

```csharp
public class MyTemplateSelector : DataTemplateSelector
{
    public DataTemplate IconA { get; set; }
    public DataTemplate IconB { get; set; }

    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        // Return IconA or IconB based on item state
        return (item as MyModel)?.IsActive == true ? IconA : IconB;
    }
}
```

```xaml
<local:MyTemplateSelector x:Key="mySelector"
    IconA="{StaticResource iconATemplate}"
    IconB="{StaticResource iconBTemplate}"/>

<syncfusion:DropDownButtonAdv IconTemplateSelector="{StaticResource mySelector}"
                               Content="{Binding IsActive}"/>
```

---

## Applying Themes

Apply built-in Syncfusion themes via `SfSkinManager`:

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<syncfusion:DropDownButtonAdv syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentLight}"
                               Label="Country"/>
```

**Available themes:** `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Office2019Colorful`, `Office2019Black`, `Windows11Light`, `Windows11Dark`, and more.

For custom themes, use ThemeStudio: [Create a custom theme](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Icon doesn't appear in Large mode | Using `SmallIcon` instead of `LargeIcon` | Use `LargeIcon` when `SizeMode="Large"` |
| Label hidden in Small mode | By design — Small mode is icon-only | Use `Normal` or `Large` for label visibility |
| `IconTemplate` ignored | Both `IconTemplate` and `SmallIcon` set | `IconTemplate` takes priority; remove `SmallIcon` if not wanted |
| Assembly not found at runtime | Only nuget package added, not reference | Ensure `Syncfusion.Shared.WPF` is referenced |

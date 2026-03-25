# Size Modes & Icons in WPF ButtonAdv

## Table of Contents
- [SizeMode Overview](#sizemode-overview)
- [Small Mode](#small-mode)
- [Normal Mode](#normal-mode)
- [Large Mode](#large-mode)
- [SmallIcon vs LargeIcon](#smallicon-vs-largeicon)
- [Icon Width and Height](#icon-width-and-height)
- [IconTemplate (Vector / Font Icons)](#icontemplate-vector--font-icons)
- [IconTemplateSelector (Dynamic Icons)](#icontemplateselector-dynamic-icons)
- [Multiline Text (IsMultiLine)](#multiline-text-ismultiline)

---

## SizeMode Overview

`SizeMode` controls the visual layout of the button — specifically how the icon and label are
arranged. It uses the `SizeMode` enum with three values.

| SizeMode | Icon Shown | Label Shown | Layout |
|---|---|---|---|
| `Small` | SmallIcon (16×16) | ❌ No | Icon only |
| `Normal` | SmallIcon | ✅ Yes | Icon left, label right |
| `Large` | LargeIcon | ✅ Yes | Icon top, label below |

Default is `Normal`.

---

## Small Mode

Displays the icon only — the label is hidden. Useful for compact toolbars.

```xaml
<syncfusion:ButtonAdv SizeMode="Small"
                      Label="Log in"
                      SmallIcon="Images/user.png"/>
```

```csharp
ButtonAdv button = new ButtonAdv();
button.Label = "Log in";
button.SizeMode = SizeMode.Small;
button.SmallIcon = new BitmapImage(new Uri("Images/user.png", UriKind.RelativeOrAbsolute));
```

> Even though `Label` is set, it won't be visible in Small mode. Keep it for accessibility/tooltips.

---

## Normal Mode

Displays a small icon on the left with the label text on the right. This is the default layout.

```xaml
<syncfusion:ButtonAdv SizeMode="Normal"
                      Label="Log in"
                      SmallIcon="Images/user.png"/>
```

```csharp
ButtonAdv button = new ButtonAdv();
button.Label = "Log in";
button.SizeMode = SizeMode.Normal;
button.SmallIcon = new BitmapImage(new Uri("Images/user.png", UriKind.RelativeOrAbsolute));
```

---

## Large Mode

Displays a large icon on top with the label text below. Commonly used in Ribbon-style UIs.

```xaml
<syncfusion:ButtonAdv SizeMode="Large"
                      Label="Log in"
                      LargeIcon="Images/user-large.png"/>
```

```csharp
ButtonAdv button = new ButtonAdv();
button.Label = "Log in";
button.SizeMode = SizeMode.Large;
button.LargeIcon = new BitmapImage(new Uri("Images/user-large.png", UriKind.RelativeOrAbsolute));
```

---

## SmallIcon vs LargeIcon

- **`SmallIcon`** — used when `SizeMode` is `Small` or `Normal`
- **`LargeIcon`** — used when `SizeMode` is `Large`

You can set both so the button looks correct regardless of which mode is active:

```xaml
<syncfusion:ButtonAdv Label="Save"
                      SmallIcon="Images/save-small.png"
                      LargeIcon="Images/save-large.png"
                      SizeMode="Large"/>
```

```csharp
button.SmallIcon = new BitmapImage(new Uri("Images/save-small.png", UriKind.RelativeOrAbsolute));
button.LargeIcon = new BitmapImage(new Uri("Images/save-large.png", UriKind.RelativeOrAbsolute));
```

> **Gotcha:** If you use `SizeMode="Large"` but only set `SmallIcon`, the button may appear without
> an icon. Always set `LargeIcon` when using Large mode.

---

## Icon Width and Height

Use `IconWidth` and `IconHeight` to control the rendered size of the icon independently of the
image file's actual dimensions.

```xaml
<!-- Smaller icon -->
<syncfusion:ButtonAdv SizeMode="Normal"
                      Label="Syncfusion"
                      SmallIcon="Images/logo.png"
                      IconWidth="20"
                      IconHeight="20"/>

<!-- Larger icon -->
<syncfusion:ButtonAdv SizeMode="Normal"
                      Label="Syncfusion"
                      SmallIcon="Images/logo.png"
                      IconWidth="30"
                      IconHeight="30"/>
```

```csharp
button.IconWidth = 24;
button.IconHeight = 24;
```

---

## IconTemplate (Vector / Font Icons)

`IconTemplate` accepts a `DataTemplate` that can contain any WPF visual element — path geometry,
font glyphs, shapes, etc. The icon automatically resizes to fit the template's defined dimensions.

This is the preferred approach for crisp, scalable icons that don't rely on bitmap image files.

```xaml
<Window.Resources>
    <DataTemplate x:Key="UserIconTemplate">
        <Grid Width="16" Height="16">
            <Path Data="M21.576999,13.473151C26.414003,15.496185 30.259996,20.071221
                        31.999999,25.86432 15.448002,32.143386 0,25.86432 0,25.86432
                        1.7140042,20.158227 5.4690005,15.632174 10.202001,13.564156
                        11.338002,15.514191 13.444005,16.827195 15.862003,16.827195
                        18.317996,16.827195 20.455997,15.474182 21.576999,13.473151z
                        M16.000003,0C19.617999,0 22.550998,2.933049 22.550998,6.551072
                        22.550998,10.170134 19.617999,13.102144 16.000003,13.102144
                        12.381993,13.102144 9.4489957,10.170134 9.4489957,6.551072
                        9.4489957,2.933049 12.381993,0 16.000003,0z"
                  Fill="#FF3A3A38"
                  Stretch="Fill"/>
        </Grid>
    </DataTemplate>
</Window.Resources>

<!-- Apply to each size mode -->
<syncfusion:ButtonAdv Label="Login" SizeMode="Small"
                      IconTemplate="{StaticResource UserIconTemplate}"/>
<syncfusion:ButtonAdv Label="Login" SizeMode="Normal"
                      IconTemplate="{StaticResource UserIconTemplate}"/>
<syncfusion:ButtonAdv Label="Login" SizeMode="Large"
                      IconTemplate="{StaticResource UserIconTemplate}"/>
```

> **Note:** When `IconTemplate` is set, `SmallIcon` and `LargeIcon` are ignored. The template
> content scales to fit the button layout automatically.

---

## IconTemplateSelector (Dynamic Icons)

`IconTemplateSelector` lets you switch the icon template dynamically based on bound data. Inherit
from `DataTemplateSelector` and override `SelectTemplate`.

**ViewModel:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    private bool _isChecked;
    public bool IsChecked
    {
        get => _isChecked;
        set { _isChecked = value; OnPropertyChanged(nameof(IsChecked)); }
    }
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**Template Selector:**
```csharp
public class IconTemplateSelector : DataTemplateSelector
{
    public DataTemplate CheckedIcon   { get; set; }
    public DataTemplate UnCheckedIcon { get; set; }

    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        if (item is bool isChecked)
            return isChecked ? CheckedIcon : UnCheckedIcon;
        return base.SelectTemplate(item, container);
    }
}
```

**XAML:**
```xaml
<Window.Resources>
    <DataTemplate x:Key="CheckedIcon">
        <Grid Width="12" Height="16">
            <!-- checked icon path data -->
        </Grid>
    </DataTemplate>
    <DataTemplate x:Key="UnCheckedIcon">
        <Grid Width="16" Height="16">
            <!-- unchecked icon path data -->
        </Grid>
    </DataTemplate>
    <local:IconTemplateSelector x:Key="MyIconSelector"
        CheckedIcon="{StaticResource CheckedIcon}"
        UnCheckedIcon="{StaticResource UnCheckedIcon}"/>
</Window.Resources>

<CheckBox Name="Toggle" IsChecked="{Binding IsChecked, Mode=TwoWay}" Content="Toggle Icon"/>
<syncfusion:ButtonAdv Label="Dynamic"
                      IconTemplateSelector="{StaticResource MyIconSelector}"
                      DataContext="{Binding IsChecked}"/>
```

> **Priority:** `IconTemplateSelector` overrides both `IconTemplate` and image-based icons.

---

## Multiline Text (IsMultiLine)

`IsMultiLine` allows the button label to wrap across multiple lines. This property only applies
when `SizeMode="Large"`.

```xaml
<syncfusion:ButtonAdv SizeMode="Large"
                      IsMultiLine="True"
                      LargeIcon="Images/account.png"
                      Label="Sign in with your Syncfusion Account"/>
```

```csharp
ButtonAdv button = new ButtonAdv();
button.SizeMode = SizeMode.Large;
button.IsMultiLine = true;
button.LargeIcon = new BitmapImage(new Uri("Images/account.png", UriKind.RelativeOrAbsolute));
button.Label = "Sign in with your Syncfusion Account";
```

> **Gotcha:** `IsMultiLine` has no effect in `Small` or `Normal` size modes. If the text isn't
> wrapping, verify that `SizeMode` is set to `Large`.

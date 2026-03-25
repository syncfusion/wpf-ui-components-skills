# Getting Started with MenuAdv

## Assembly Reference

Add the following assembly to your WPF project:

```
Syncfusion.Shared.WPF
```

**In `.csproj`:**
```xml
<Reference Include="Syncfusion.Shared.WPF">
    <HintPath>path\to\Syncfusion.Shared.WPF.dll</HintPath>
</Reference>
```

**Or via NuGet:**
```
Install-Package Syncfusion.Shared.WPF
```

---

## XAML Namespace

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

Or using the CLR namespace directly:
```xaml
xmlns:shared="clr-namespace:Syncfusion.Windows.Shared;assembly=Syncfusion.Shared.WPF"
```

---

## Adding Control in XAML

Nest `MenuItemAdv` elements inside `MenuAdv`. Each `MenuItemAdv` can contain further `MenuItemAdv` children to create submenus:

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <DockPanel>
        <syncfusion:MenuAdv Height="25" DockPanel.Dock="Top">
            <syncfusion:MenuItemAdv Header="Products"/>
            <syncfusion:MenuItemAdv Header="Business Intelligence"/>
            <syncfusion:MenuItemAdv Header="User Interface">
                <syncfusion:MenuItemAdv Header="WPF">
                    <syncfusion:MenuItemAdv Header="Tools"/>
                    <syncfusion:MenuItemAdv Header="Chart"/>
                    <syncfusion:MenuItemAdv Header="Grid"/>
                    <syncfusion:MenuItemAdv Header="Diagram"/>
                </syncfusion:MenuItemAdv>
                <syncfusion:MenuItemAdv Header="Silverlight"/>
                <syncfusion:MenuItemAdv Header="Reporting"/>
            </syncfusion:MenuItemAdv>
        </syncfusion:MenuAdv>
    </DockPanel>
</Window>
```

---

## Adding Control in C#

```csharp
using Syncfusion.Windows.Shared;

MenuAdv mAdv = new MenuAdv();

MenuItemAdv product  = new MenuItemAdv { Header = "Products" };
MenuItemAdv bi       = new MenuItemAdv { Header = "Business Intelligence" };
MenuItemAdv ui       = new MenuItemAdv { Header = "User Interface" };
MenuItemAdv wpf      = new MenuItemAdv { Header = "WPF" };
MenuItemAdv tools    = new MenuItemAdv { Header = "Tools" };
MenuItemAdv chart    = new MenuItemAdv { Header = "Chart" };
MenuItemAdv grid     = new MenuItemAdv { Header = "Grid" };
MenuItemAdv diagram  = new MenuItemAdv { Header = "Diagram" };
MenuItemAdv sl       = new MenuItemAdv { Header = "Silverlight" };
MenuItemAdv reporting = new MenuItemAdv { Header = "Reporting" };

wpf.Items.Add(tools);
wpf.Items.Add(chart);
wpf.Items.Add(grid);
wpf.Items.Add(diagram);

ui.Items.Add(wpf);
ui.Items.Add(sl);

product.Items.Add(bi);
product.Items.Add(ui);
product.Items.Add(reporting);

mAdv.Items.Add(product);

this.Content = mAdv;
```

---

## Setting Icons on Menu Items

Set the `Icon` property of `MenuItemAdv` to an `Image` element to show an image on the left:

```xaml
<syncfusion:MenuAdv Height="25" VerticalAlignment="Top">
    <syncfusion:MenuItemAdv Header="File">
        <syncfusion:MenuItemAdv Header="New">
            <syncfusion:MenuItemAdv.Icon>
                <Image Source="Assets/NewIcon.png" Width="15" Height="15"/>
            </syncfusion:MenuItemAdv.Icon>
        </syncfusion:MenuItemAdv>
        <syncfusion:MenuItemAdv Header="Open">
            <syncfusion:MenuItemAdv.Icon>
                <Image Source="Assets/OpenIcon.png" Width="15" Height="15"/>
            </syncfusion:MenuItemAdv.Icon>
        </syncfusion:MenuItemAdv>
        <syncfusion:MenuItemAdv Header="Save">
            <syncfusion:MenuItemAdv.Icon>
                <Image Source="Assets/SaveIcon.png" Width="15" Height="15"/>
            </syncfusion:MenuItemAdv.Icon>
        </syncfusion:MenuItemAdv>
    </syncfusion:MenuItemAdv>
</syncfusion:MenuAdv>
```

```csharp
MenuItemAdv newItem = new MenuItemAdv { Header = "New" };
newItem.Icon = new Image
{
    Source = new BitmapImage(new Uri("Assets/NewIcon.png", UriKind.Relative)),
    Width  = 15,
    Height = 15
};
```

---

## Applying Themes

```xaml
syncfusion:SfSkinManager.VisualStyle="FluentLight"
```

```csharp
using Syncfusion.SfSkinManager;
SfSkinManager.SetVisualStyle(menuAdv, VisualStyles.FluentLight);
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `MenuAdv` not found in XAML | Wrong/missing namespace | Add `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"` |
| Nested items not appearing | Items added to wrong parent | Verify parent `MenuItemAdv.Items.Add(child)` |
| Icon not displaying | Image path incorrect or file not copied | Set image `Build Action = Resource` or use absolute URI |
| Menu covers content below | Not docked to top | Wrap in `DockPanel` and set `DockPanel.Dock="Top"` |

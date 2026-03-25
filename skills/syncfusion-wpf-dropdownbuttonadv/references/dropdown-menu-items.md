# Dropdown Menu Items

## Table of Contents
- [DropDownMenuGroup Container](#dropdownmenugroup-container)
- [Declarative Menu Items](#declarative-menu-items)
- [Icon Bar](#icon-bar)
- [Scrollable Lists](#scrollable-lists)
- [Resizable Popup](#resizable-popup)
- [Checkable Items](#checkable-items)
- [Custom Items with MoreItems](#custom-items-with-moreitems)
- [Gotchas](#gotchas)

---

## DropDownMenuGroup Container

`DropDownMenuGroup` is the required container for all menu items inside `DropDownButtonAdv`. It handles layout, scrolling, resizing, and custom items.

```xaml
<syncfusion:DropDownButtonAdv Label="Options">
    <syncfusion:DropDownMenuGroup>
        <!-- Items go here -->
    </syncfusion:DropDownMenuGroup>
</syncfusion:DropDownButtonAdv>
```

The group is set as the `Content` of `DropDownButtonAdv`. Only one `DropDownMenuGroup` per button.

---

## Declarative Menu Items

Add `DropDownMenuItem` elements directly inside the group. Each item supports a `Header` (text) and an optional `Icon`:

```xaml
<syncfusion:DropDownButtonAdv Label="Country" SizeMode="Normal" SmallIcon="Images/flag.png">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="India">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="Images/india.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="France">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="Images/france.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Germany"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:DropDownButtonAdv>
```

```csharp
DropDownButtonAdv button = new DropDownButtonAdv();
DropDownMenuGroup menu = new DropDownMenuGroup();
DropDownMenuItem item1 = new DropDownMenuItem
{
    Header = "India",
    Icon = new BitmapImage(new Uri("Images/india.png", UriKind.RelativeOrAbsolute)),
    HorizontalAlignment = HorizontalAlignment.Left
};
menu.Items.Add(item1);
button.Content = menu;
```

> **Always set `HorizontalAlignment="Left"`** on `DropDownMenuItem` for proper alignment in the popup list.

---

## Icon Bar

The `IconBarEnabled` property on `DropDownMenuGroup` shows a vertical colored strip to the left of item icons:

```xaml
<syncfusion:DropDownMenuGroup IconBarEnabled="True">
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="India">
        <syncfusion:DropDownMenuItem.Icon>
            <Image Source="Images/india.png"/>
        </syncfusion:DropDownMenuItem.Icon>
    </syncfusion:DropDownMenuItem>
</syncfusion:DropDownMenuGroup>
```

> Default value: `false`. Enable it when items have icons and you want a consistent visual alignment bar.

---

## Scrollable Lists

For large item lists, constrain the popup height and enable scrolling:

```xaml
<syncfusion:DropDownMenuGroup MaxHeight="150" ScrollBarVisibility="Visible">
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="India"/>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="France"/>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Germany"/>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Canada"/>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="China"/>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Japan"/>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Spain"/>
</syncfusion:DropDownMenuGroup>
```

```csharp
menu.MaxHeight = 150;
menu.ScrollBarVisibility = ScrollBarVisibility.Visible;
```

---

## Resizable Popup

Allow users to drag-resize the popup height/width using a gripper at the bottom:

```xaml
<syncfusion:DropDownMenuGroup IsResizable="True">
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="India"/>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="France"/>
</syncfusion:DropDownMenuGroup>
```

```csharp
menu.IsResizable = true;
```

> Combine with `ScrollBarVisibility="Visible"` when the item list may exceed the resized height.

---

## Checkable Items

Items can be toggled checked/unchecked when `IsCheckable="True"`:

```xaml
<syncfusion:DropDownMenuGroup>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left"
                                  Header="India"
                                  IsCheckable="True"
                                  IsChecked="True"/>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left"
                                  Header="France"
                                  IsCheckable="True"/>
    <syncfusion:DropDownMenuItem HorizontalAlignment="Left"
                                  Header="Germany"/>
</syncfusion:DropDownMenuGroup>
```

```csharp
DropDownMenuItem item = new DropDownMenuItem
{
    Header = "India",
    IsCheckable = true,
    IsChecked = true,
    HorizontalAlignment = HorizontalAlignment.Left
};
```

To respond to check state changes, use the `IsCheckedChanged` event (see `events-and-appearance.md`).

---

## Custom Items with MoreItems

`MoreItems` appends custom `UIElement` children at the bottom of the group. Accepts `ObservableCollection<UIElement>`:

```xaml
<Window.DataContext>
    <local:ColorViewModel/>
</Window.DataContext>

<syncfusion:DropDownButtonAdv Label="Colors" SmallIcon="Images/colors.png">
    <syncfusion:DropDownMenuGroup MoreItems="{Binding CustomItems}"
                                   IconBarEnabled="True"
                                   IsMoreItemsIconTrayEnabled="False">
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Black"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Red"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:DropDownButtonAdv>
```

```csharp
// ViewModel
public ObservableCollection<UIElement> CustomItems { get; set; }

public ColorViewModel()
{
    CustomItems = new ObservableCollection<UIElement>();
    CustomItems.Add(new Label { Content = "More Colors..." });
}
```

**`IsMoreItemsIconTrayEnabled`:** When `true`, custom items also show the icon bar column. When `false` (default), custom items are rendered without the icon tray.

To use `DropDownMenuItem` as a custom item (with icon support in tray):

```csharp
CustomItems.Add(new DropDownMenuItem
{
    Header = "Sky Blue",
    Icon = new Image { Source = new BitmapImage(new Uri("/Images/skyblue.png", UriKind.RelativeOrAbsolute)) }
});
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Items misaligned in popup | Missing `HorizontalAlignment="Left"` | Always set on each `DropDownMenuItem` |
| `MoreItems` not updating | `List<T>` used instead of `ObservableCollection<T>` | Use `ObservableCollection<UIElement>` |
| Icon bar shows for regular items but not `MoreItems` | `IsMoreItemsIconTrayEnabled` is `false` | Set `IsMoreItemsIconTrayEnabled="True"` |
| Scrollbar not visible | `MaxHeight` not set on group | Set `MaxHeight` to constrain popup height |
| Resize gripper not visible | `IsResizable` not set | Set `IsResizable="True"` on `DropDownMenuGroup` |

# GroupView and Content

## GroupView as Item Container

`GroupView` is the list container placed inside a `GroupBarItem`. It hosts `GroupViewItem` elements — clickable navigation links with text and icons.

```xaml
<syncfusion:GroupBarItem Header="Mailbox">
    <syncfusion:GroupView>
        <syncfusion:GroupViewItem Text="Inbox"         ImageSource="Images/inbox.png"/>
        <syncfusion:GroupViewItem Text="Sent Items"    ImageSource="Images/sent.png"/>
        <syncfusion:GroupViewItem Text="Deleted Items" ImageSource="Images/trash.png"/>
    </syncfusion:GroupView>
</syncfusion:GroupBarItem>
```

```csharp
GroupBarItem item = new GroupBarItem { Header = "Mailbox" };
GroupView view = new GroupView();

GroupViewItem inbox = new GroupViewItem { Text = "Inbox" };
GroupViewItem sent  = new GroupViewItem { Text = "Sent Items" };
view.Items.Add(inbox);
view.Items.Add(sent);

item.Content = view;
groupBar.Items.Add(item);
```

---

## GroupViewItem Properties

| Property | Type | Description |
|---|---|---|
| `Text` | `string` | Display label for the item |
| `ImageSource` | `ImageSource` | Icon image shown next to the text |
| `ToolTip` | `object` | Tooltip on hover |
| `IsSelected` | `bool` | Currently selected state |

---

## ListView Mode vs Icon Mode

`IsListViewMode` on `GroupView` switches between list-style (text + small icon) and large-icon mode:

```xaml
<!-- List view: text beside small icons (default for navigation) -->
<syncfusion:GroupView IsListViewMode="True">
    <syncfusion:GroupViewItem Text="List View"/>
    <syncfusion:GroupViewItem Text="Show ContextMenu"/>
    <syncfusion:GroupViewItem Text="Show ToolTip"/>
</syncfusion:GroupView>
```

```xaml
<!-- Icon mode: large icons (default when IsListViewMode not set) -->
<syncfusion:GroupView>
    <syncfusion:GroupViewItem Text="Inbox" ImageSource="Images/inbox.png"/>
</syncfusion:GroupView>
```

```csharp
groupView.IsListViewMode = true;
```

> Use `IsListViewMode="True"` for Outlook-style navigation where items show as a list with small icons.

---

## Default Image for GroupViewItems

When `GroupViewItem` has no `ImageSource`, a default image is shown. Configure the default image on `GroupView`:

```xaml
<syncfusion:GroupView DefaultGroupViewItemImage="Images/default.png">
    <syncfusion:GroupViewItem Text="No Icon Item"/>
    <syncfusion:GroupViewItem Text="Another Item"/>
</syncfusion:GroupView>
```

---

## Adding Custom Panel Content

Use any WPF panel instead of `GroupView` when you need form-like or custom content:

```xaml
<syncfusion:GroupBarItem Header="Orientation Settings">
    <StackPanel Orientation="Vertical" Margin="4">
        <TextBlock Text="GroupBar Orientation" Margin="0,4,0,2"/>
        <RadioButton IsChecked="True">Horizontal</RadioButton>
        <RadioButton>Vertical</RadioButton>
        <Separator Margin="0,4"/>
        <TextBlock Text="GroupView Orientation" Margin="0,4,0,2"/>
        <RadioButton>Horizontal</RadioButton>
        <RadioButton IsChecked="True">Vertical</RadioButton>
    </StackPanel>
</syncfusion:GroupBarItem>
```

Any `UIElement` works — `Grid`, `StackPanel`, `UserControl`, embedded data grids, etc.

---

## Tooltip for GroupViewItems

Set a tooltip that appears when hovering over a `GroupViewItem`:

```xaml
<syncfusion:GroupView>
    <syncfusion:GroupViewItem Text="Inbox" ToolTip="View incoming messages"/>
    <syncfusion:GroupViewItem Text="Sent" ToolTip="View sent messages"/>
</syncfusion:GroupView>
```

For more tooltip configuration, see [behavior-and-features.md](behavior-and-features.md).

---

## Adding GroupView in ListView Mode (Full Example)

```xaml
<syncfusion:GroupBar Height="300" Width="230" Name="groupBar">
    <syncfusion:GroupBarItem Name="generalItem" HeaderImageSource="Images/label.png" Header="General">
        <syncfusion:GroupView IsListViewMode="True">
            <syncfusion:GroupViewItem Text="List View"/>
            <syncfusion:GroupViewItem Text="Show ContextMenu"/>
            <syncfusion:GroupViewItem Text="Show ToolTip"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
    <syncfusion:GroupBarItem Name="stateItem" HeaderImageSource="Images/notes.png" Header="State Persistence">
        <syncfusion:GroupView IsListViewMode="True">
            <syncfusion:GroupViewItem Text="Save State"/>
            <syncfusion:GroupViewItem Text="Load State"/>
            <syncfusion:GroupViewItem Text="Reset State"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
</syncfusion:GroupBar>
```

---

## Rotating Item Content

The content inside a `GroupBarItem` can be rotated. See [orientation-and-layout.md](orientation-and-layout.md) for rotation and orientation details.

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| GroupViewItems show as large icons | `IsListViewMode` not set | Set `IsListViewMode="True"` on `GroupView` |
| No default image shown | `DefaultGroupViewItemImage` not set | Set the property on `GroupView` to provide a fallback image |
| Custom panel content overflows | No `ScrollViewer` wrapping it | Wrap in `ScrollViewer` if content may exceed available height |
| `GroupViewItem` click not firing | No event handler subscribed | Subscribe to `GroupViewItem.Click` or use `IsSelected` binding |

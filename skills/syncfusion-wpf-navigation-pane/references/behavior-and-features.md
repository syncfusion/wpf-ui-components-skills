# Behavior and Features

## Table of Contents
- [Drag-and-Drop Reordering](#drag-and-drop-reordering)
- [Built-in Context Menu](#built-in-context-menu)
- [Toolbars in GroupBar](#toolbars-in-groupbar)
- [Tooltips for GroupViewItems](#tooltips-for-groupviewitems)
- [Animation for Expand/Collapse](#animation-for-expandcollapse)
- [Collapsing and Closing in StackMode](#collapsing-and-closing-in-stackmode)
- [State Persistence](#state-persistence)
- [Localization](#localization)
- [Gotchas](#gotchas)

---

## Drag-and-Drop Reordering

Enable drag-and-drop reordering of `GroupBarItem` elements by setting `DragItemVisibility`:

```xaml
<syncfusion:GroupBar DragItemVisibility="True" Height="300" Width="230" Name="groupBar">
    <syncfusion:GroupBarItem Header="Mailbox"/>
    <syncfusion:GroupBarItem Header="Contacts"/>
    <syncfusion:GroupBarItem Header="Tasks"/>
</syncfusion:GroupBar>
```

```csharp
groupBar.DragItemVisibility = true;
```

### Drag Marker Color

Customize the color of the drag insertion indicator:

```xaml
<syncfusion:GroupBar DragItemVisibility="True" DragMarkerBrush="Red"
                     Height="200" Width="230" Name="groupBar">
    <syncfusion:GroupBarItem Header="Item 1"/>
    <syncfusion:GroupBarItem Header="Item 2"/>
</syncfusion:GroupBar>
```

```csharp
groupBar.DragMarkerBrush = Brushes.Red;
```

### Monitor Drag State

`DraggingItemInProgress` indicates when a drag operation is in progress:

```csharp
groupBar.DraggingItemInProgress = true; // or read its current value
```

---

## Built-in Context Menu

`GroupBar` includes a built-in context menu (similar to VS 2005 Toolbox) that allows users to:
- **Add** new GroupBarItem
- **Remove** a GroupBarItem
- **Rename** a GroupBarItem header
- **Move** item up or down
- **Sort** items alphabetically (ascending/descending)
- **Show/Hide** items in StackMode

The context menu is enabled by default. To disable it, override or hide it via the `ContextMenu` property:

```xaml
<!-- Disable context menu -->
<syncfusion:GroupBar ContextMenu="{x:Null}" Height="300" Width="230"/>
```

---

## Toolbars in GroupBar

Custom toolbars can be embedded within GroupBar items. Place toolbar controls as content within a `GroupBarItem`:

```xaml
<syncfusion:GroupBarItem Header="Toolbar Section">
    <ToolBar>
        <Button Content="New"  ToolTip="New Item"/>
        <Button Content="Open" ToolTip="Open Item"/>
        <Separator/>
        <Button Content="Save" ToolTip="Save Item"/>
    </ToolBar>
</syncfusion:GroupBarItem>
```

For full `ToolBarAdv` integration, see the ToolBar skill.

---

## Tooltips for GroupViewItems

Add tooltips to individual `GroupViewItem` elements:

```xaml
<syncfusion:GroupView IsListViewMode="True">
    <syncfusion:GroupViewItem Text="Inbox"      ToolTip="View your inbox messages"/>
    <syncfusion:GroupViewItem Text="Sent Items" ToolTip="View sent messages"/>
    <syncfusion:GroupViewItem Text="Deleted"    ToolTip="View deleted items"/>
</syncfusion:GroupView>
```

For more complex tooltips, use the `ToolTip` property with any UIElement:

```xaml
<syncfusion:GroupViewItem Text="Inbox">
    <syncfusion:GroupViewItem.ToolTip>
        <StackPanel>
            <TextBlock FontWeight="Bold" Text="Inbox"/>
            <TextBlock Text="3 unread messages"/>
        </StackPanel>
    </syncfusion:GroupViewItem.ToolTip>
</syncfusion:GroupViewItem>
```

---

## Animation for Expand/Collapse

GroupBar supports animation when items expand or collapse. Configure animation speed or style via `AnimationSpeed` (if available in the theme) or through the control's animation properties:

```xaml
<syncfusion:GroupBar AnimationSpeed="300" Height="300" Width="230" VisualMode="Default">
    <syncfusion:GroupBarItem Header="Mailbox">
        <syncfusion:GroupView>
            <syncfusion:GroupViewItem Text="Inbox"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
</syncfusion:GroupBar>
```

> Animation behavior may vary between classic and ThemeStudio themes.

---

## Collapsing and Closing in StackMode

### AllowCollapse

Enable a toggle button that collapses the entire GroupBar into a compact strip:

```xaml
<syncfusion:GroupBar VisualMode="StackMode" AllowCollapse="True" Height="300" Width="230"/>
```

```csharp
groupBar.AllowCollapse = true;
```

### Collapse Button Customization

Customize the collapse button template (target type is `ToggleButton`):

```xaml
<syncfusion:GroupBar AllowCollapse="True" VisualMode="StackMode" Height="300" Width="230">
    <syncfusion:GroupBar.CollapseButtonTemplate>
        <ControlTemplate TargetType="{x:Type ToggleButton}">
            <Border Height="16" Width="16">
                <Path Name="arrow" Data="M0,0 L3.5,4 7,0z"
                      Fill="Black"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center"/>
            </Border>
            <ControlTemplate.Triggers>
                <Trigger Property="IsChecked" Value="True">
                    <Setter TargetName="arrow" Property="Data" Value="M3,1 L7,5 3,9z"/>
                </Trigger>
            </ControlTemplate.Triggers>
        </ControlTemplate>
    </syncfusion:GroupBar.CollapseButtonTemplate>
    <syncfusion:GroupBarItem Header="Mailbox"/>
</syncfusion:GroupBar>
```

### Collapse Button Colors

```xaml
<syncfusion:GroupBar AllowCollapse="True"
                     CollapseButtonBackground="AliceBlue"
                     CollapseButtonMouseOverBackground="LightSteelBlue"
                     VisualMode="StackMode"/>
```

```csharp
groupBar.CollapseButtonBackground          = Brushes.AliceBlue;
groupBar.CollapseButtonMouseOverBackground = Brushes.LightSteelBlue;
```

---

## State Persistence

Save and restore the GroupBar layout (item order, visibility, selected item) using the built-in state methods.

### Save State

```csharp
myGroupBar.SaveBarState();
```

### Load State

```csharp
myGroupBar.LoadBarState();
RegisterEvents(myGroupBar); // Re-subscribe events after load
```

### Reset State

```csharp
myGroupBar.ResetBarState();
RegisterEvents(myGroupBar);
```

### Re-subscribe Events After State Restore

After loading/resetting state, re-attach event handlers to items:

```csharp
public void RegisterEvents(GroupBar groupBar)
{
    if (groupBar == null) return;

    foreach (GroupBarItem tab in groupBar.Items)
    {
        // Subscribe to tab events
        tab.IsSelectedChanged += OnTabSelectionChanged;

        if (tab.Content is GroupView view)
        {
            foreach (GroupViewItem item in view.Items)
            {
                item.Click += OnViewItemClicked;
            }
        }
    }
}
```

### Auto-Save on Load

```xaml
<syncfusion:GroupBar SaveOriginalState="True" Height="300" Width="230"/>
```

```csharp
groupBar.SaveOriginalState = true;
```

---

## Localization

GroupBar supports localization of its built-in strings (context menu labels, etc.) by setting the application culture:

```csharp
System.Threading.Thread.CurrentThread.CurrentUICulture =
    new System.Globalization.CultureInfo("fr-FR");
```

For custom string overrides, provide a localized resource file:
- File: `Syncfusion.Tools.Wpf.{culture}.resx`
- Place in the application resources folder
- Override the relevant string keys

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Drag reordering not working | `DragItemVisibility` not set | Set `DragItemVisibility="True"` on `GroupBar` |
| Events lost after `LoadBarState()` | State restore replaces item instances | Always call `RegisterEvents()` after `LoadBarState()` or `ResetBarState()` |
| `CollapseButtonTemplate` not visible | `AllowCollapse` is `false` | Set `AllowCollapse="True"` to show the collapse button |
| Context menu rename breaks bindings | Renaming via context menu updates `HeaderText` but not bound model | Handle `GroupBarItemAdded`/Renamed events to sync back to ViewModel |
| `SaveBarState()` saves nothing | Called before items are loaded | Call after all items are added and the GroupBar is fully initialized |

# Visual Modes

`VisualMode` is the most important configuration for `GroupBar`. It controls how items expand, collapse, and display.

## Visual Mode Comparison

| Mode | Behavior | Use When |
|---|---|---|
| `Default` | One item expanded at a time; clicking another collapses the current | Standard accordion-style navigation |
| `MultipleExpansion` | Any number of items can be expanded simultaneously (tree-view style) | Users need to see multiple sections at once |
| `StackMode` | Outlook-style bar; expanded item's content shown in popup at bottom | Outlook-like sidebar navigation |

---

## Default Mode

Only one `GroupBarItem` can be expanded at a time. Expanding a new item collapses the previously expanded one.

```xaml
<syncfusion:GroupBar Height="300" Width="230" VisualMode="Default" Name="groupBar">
    <syncfusion:GroupBarItem Header="General" HeaderImageSource="Images/label.png">
        <syncfusion:GroupView>
            <syncfusion:GroupViewItem Text="List View"/>
            <syncfusion:GroupViewItem Text="Show ContextMenu"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
    <syncfusion:GroupBarItem Header="Visual Mode" HeaderImageSource="Images/tasks.png">
        <syncfusion:GroupView>
            <syncfusion:GroupViewItem Text="Default"/>
            <syncfusion:GroupViewItem Text="Multiple Expansion"/>
            <syncfusion:GroupViewItem Text="StackMode"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
</syncfusion:GroupBar>
```

```csharp
groupBar.VisualMode = VisualMode.Default;
```

---

## MultipleExpansion Mode

All items can be expanded or collapsed independently, similar to a tree view.

```xaml
<syncfusion:GroupBar Height="300" Width="230" VisualMode="MultipleExpansion" Name="groupBar">
    <syncfusion:GroupBarItem Header="Mailbox">
        <syncfusion:GroupView>
            <syncfusion:GroupViewItem Text="Inbox"/>
            <syncfusion:GroupViewItem Text="Sent Items"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
    <syncfusion:GroupBarItem Header="Contacts">
        <syncfusion:GroupView>
            <syncfusion:GroupViewItem Text="All Contacts"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
</syncfusion:GroupBar>
```

```csharp
groupBar.VisualMode = VisualMode.MultipleExpansion;
```

---

## StackMode

Outlook-style navigation. Items are shown as a stack of headers; the selected item's content appears above the stack (or as a popup at the bottom). A popup menu at the bottom allows access to hidden items.

```xaml
<syncfusion:GroupBar Height="300" Width="230" VisualMode="StackMode" Name="groupBar">
    <syncfusion:GroupBarItem Header="General" HeaderImageSource="Images/label.png">
        <syncfusion:GroupView IsListViewMode="True">
            <syncfusion:GroupViewItem Text="List View"/>
            <syncfusion:GroupViewItem Text="Show ContextMenu"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
    <syncfusion:GroupBarItem Header="State Persistence" HeaderImageSource="Images/notes.png">
        <syncfusion:GroupView IsListViewMode="True">
            <syncfusion:GroupViewItem Text="Save State"/>
            <syncfusion:GroupViewItem Text="Load State"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
</syncfusion:GroupBar>
```

```csharp
groupBar.VisualMode = VisualMode.StackMode;
```

### AllowCollapse in StackMode

Allow the entire GroupBar to be collapsed (hidden) via a toggle button:

```xaml
<syncfusion:GroupBar VisualMode="StackMode" AllowCollapse="True" Height="300" Width="230"/>
```

```csharp
groupBar.AllowCollapse = true;
```

### ShowGripper in StackMode

A gripper allows the user to resize the visible items area in StackMode. Default is `true`.

```xaml
<syncfusion:GroupBar VisualMode="StackMode" ShowGripper="True" Height="300" Width="230"/>
```

```csharp
groupBar.ShowGripper = true;
```

> `ShowGripper` only works when `VisualMode="StackMode"`. It has no effect in Default or MultipleExpansion modes.

---

## StackMode Layout: ThemeStudio vs Classic Themes

> ⚠️ **Important visual difference** depending on the applied theme:

| Theme Type | StackMode Item Arrangement |
|---|---|
| ThemeStudio themes (Windows11, Fluent, Material, Office2019, System) | Items arranged **horizontally** |
| Classic themes (Office2003, Office2007, Blend, Default) | Items arranged **vertically** |

Always test StackMode appearance when switching between theme families.

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `ShowGripper` has no effect | `VisualMode` is not `StackMode` | Only works in StackMode |
| `AllowCollapse` toggle button not visible | `AllowCollapse` not set | Set `AllowCollapse="True"` on `GroupBar` |
| All items collapsed, nothing shows | No item has `IsSelected="True"` | Set `IsSelected="True"` on at least one `GroupBarItem` |
| StackMode layout looks different after theme change | ThemeStudio vs classic theme behavior | Expected — ThemeStudio themes use horizontal layout in StackMode |
| `MultipleExpansion` mode items all collapsed | Default collapsed state | Set `IsSelected="True"` on items you want initially expanded |

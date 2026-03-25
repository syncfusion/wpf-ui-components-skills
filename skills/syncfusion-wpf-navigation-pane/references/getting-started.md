# Getting Started with GroupBar

## Assembly Setup

Add references to both:
- `Syncfusion.Tools.WPF`
- `Syncfusion.Shared.WPF`

**NuGet:** Install the `Syncfusion.Tools.WPF` NuGet package (includes both assemblies).

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

Drag **GroupBar** from the toolbox onto the designer surface. Both `Syncfusion.Tools.WPF` and `Syncfusion.Shared.WPF` are added automatically.

### Via XAML (Manual)

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <syncfusion:GroupBar Name="groupBar"/>
    </Grid>
</Window>
```

### Via C# (Code-Behind)

```csharp
using Syncfusion.Windows.Tools.Controls;

GroupBar groupBar = new GroupBar();
this.Content = groupBar;
```

---

## Adding GroupBarItems

`GroupBarItem` elements are the top-level expandable sections. Add them directly as children of `GroupBar`:

```xaml
<syncfusion:GroupBar Height="300" Width="230" Name="groupBar">
    <syncfusion:GroupBarItem Header="Mailbox"/>
    <syncfusion:GroupBarItem Header="Favorite Folders"/>
    <syncfusion:GroupBarItem Header="Contacts"/>
    <syncfusion:GroupBarItem Header="Task"/>
    <syncfusion:GroupBarItem Header="Notes"/>
</syncfusion:GroupBar>
```

```csharp
GroupBar groupBar = new GroupBar();

GroupBarItem item1 = new GroupBarItem { HeaderText = "Mailbox" };
GroupBarItem item2 = new GroupBarItem { HeaderText = "Favorite Folders" };
GroupBarItem item3 = new GroupBarItem { HeaderText = "Contacts" };

groupBar.Items.Add(item1);
groupBar.Items.Add(item2);
groupBar.Items.Add(item3);

this.Content = groupBar;
```

> **Note:** Use `Header` in XAML and `HeaderText` in C#. Both set the display text.

---

## Adding Content via GroupView

`GroupView` with `GroupViewItem` elements provides a list of clickable navigation items inside a `GroupBarItem`:

```xaml
<syncfusion:GroupBar Height="300" Width="230" Name="groupBar" VisualMode="MultipleExpansion">
    <syncfusion:GroupBarItem Header="Mailbox">
        <syncfusion:GroupView>
            <syncfusion:GroupViewItem Text="Inbox"        ImageSource="Images/inbox.png"/>
            <syncfusion:GroupViewItem Text="Sent Items"   ImageSource="Images/sent.png"/>
            <syncfusion:GroupViewItem Text="Notes"        ImageSource="Images/notes.png"/>
            <syncfusion:GroupViewItem Text="Deleted Items" ImageSource="Images/trash.png"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
</syncfusion:GroupBar>
```

```csharp
GroupBar groupBar = new GroupBar();
GroupBarItem item = new GroupBarItem { Header = "Mailbox" };
GroupView view = new GroupView();

view.Items.Add(new GroupViewItem { Text = "Inbox" });
view.Items.Add(new GroupViewItem { Text = "Sent Items" });
view.Items.Add(new GroupViewItem { Text = "Notes" });

item.Content = view;
groupBar.Items.Add(item);
this.Content = groupBar;
```

---

## Adding Content via Panel

Any WPF panel or UIElement can be used as `GroupBarItem.Content`:

```xaml
<syncfusion:GroupBarItem Header="Settings">
    <StackPanel Orientation="Vertical">
        <TextBlock Text="GroupBar Orientation" Margin="4,4,2,2"/>
        <RadioButton IsChecked="True" Margin="4,2,2,2">Horizontal</RadioButton>
        <RadioButton Margin="4,2,2,2">Vertical</RadioButton>
    </StackPanel>
</syncfusion:GroupBarItem>
```

> Use `GroupView` when you need a list of navigation links. Use any `Panel` when you need rich custom content (forms, options, embedded controls).

---

## Visual Mode Quick Reference

Set `VisualMode` to control how items expand and collapse:

```xaml
<!-- One item expanded at a time (default) -->
<syncfusion:GroupBar VisualMode="Default" .../>

<!-- Multiple items can be expanded simultaneously -->
<syncfusion:GroupBar VisualMode="MultipleExpansion" .../>

<!-- Outlook-style stack bar -->
<syncfusion:GroupBar VisualMode="StackMode" .../>
```

See [visual-modes.md](visual-modes.md) for full details.

---

## Applying Themes

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<syncfusion:GroupBar syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentLight}"
                     VisualMode="StackMode"/>
```

Apply to the entire window to theme all controls at once:
```xaml
<Window syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=MaterialLight}">
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Items don't expand | `VisualMode` not set | Set `VisualMode="Default"` or `"StackMode"` explicitly |
| `Header` vs `HeaderText` confusion | Both work but in different contexts | Use `Header` in XAML, `HeaderText` in C# |
| Content not visible | `GroupBarItem` has no content set | Assign `GroupView` or `Panel` to item's `Content` property |
| Assembly not found | Only one assembly added | Add **both** `Syncfusion.Tools.WPF` and `Syncfusion.Shared.WPF` |

# GroupBar Items

## GroupBarItem Properties

`GroupBarItem` is the top-level expandable section in `GroupBar`. Key properties:

| Property | Type | Description |
|---|---|---|
| `Header` | `object` | Display text in XAML |
| `HeaderText` | `string` | Display text in C# |
| `HeaderImageSource` | `ImageSource` | Icon shown in item header |
| `IsSelected` | `bool` | Initially expand this item |
| `ShowInGroupBar` | `bool` | Whether item is visible in the bar |
| `GroupBarItemCornerRadius` | `CornerRadius` | Rounded corners on header |
| `GroupBarItemCursorType` | `ItemCursorType` | Cursor when hovering (Default/Hand) |
| `Content` | `object` | The content shown when expanded (GroupView or Panel) |

---

## Setting Header with Image

```xaml
<syncfusion:GroupBar Height="200" Width="230" Name="groupBar">
    <syncfusion:GroupBarItem Header="Mailbox" HeaderImageSource="Images/mail.png"/>
    <syncfusion:GroupBarItem Header="Contacts" HeaderImageSource="Images/contacts.png"/>
</syncfusion:GroupBar>
```

```csharp
GroupBarItem item = new GroupBarItem();
item.HeaderText = "Mailbox";
item.HeaderImageSource = new BitmapImage(new Uri("Images/mail.png", UriKind.RelativeOrAbsolute));
```

---

## Setting Initially Selected Item

Use `IsSelected="True"` to expand an item when the GroupBar loads:

```xaml
<syncfusion:GroupBarItem Header="Mailbox" IsSelected="True">
    <syncfusion:GroupView>
        <syncfusion:GroupViewItem Text="Inbox"/>
    </syncfusion:GroupView>
</syncfusion:GroupBarItem>
```

```csharp
groupBarItem.IsSelected = true;
```

---

## Corner Radius

Apply rounded corners to the `GroupBarItem` header:

```xaml
<syncfusion:GroupBarItem Header="Mailbox" GroupBarItemCornerRadius="20" ShowInGroupBar="True"/>
```

```csharp
GroupBarItem item = new GroupBarItem();
item.HeaderText = "Mailbox";
item.GroupBarItemCornerRadius = new CornerRadius(20d);
item.ShowInGroupBar = true;
groupBar.Items.Add(item);
```

---

## Cursor Type

Change the mouse cursor when hovering over the item header:

```xaml
<syncfusion:GroupBar GroupBarItemCursorType="Hand" Height="200" Width="230" Name="groupBar">
    <syncfusion:GroupBarItem Header="Mailbox"/>
</syncfusion:GroupBar>
```

```csharp
groupBar.GroupBarItemCursorType = ItemCursorType.Hand;
```

**Values:** `Default` (standard arrow), `Hand` (pointer cursor)

---

## Content Length

`ItemContentLength` sets the height (vertical layout) or width (horizontal layout) of the expanded item's content area:

```xaml
<syncfusion:GroupBar Height="200" Width="230" ItemContentLength="100" Name="groupBar">
    <syncfusion:GroupBarItem Header="Settings" IsSelected="True">
        <StackPanel>
            <TextBlock Text="Option A"/>
            <TextBlock Text="Option B"/>
        </StackPanel>
    </syncfusion:GroupBarItem>
</syncfusion:GroupBar>
```

```csharp
groupBar.ItemContentLength = 100;
```

---

## Dragging State

`DraggingItemInProgress` is a read-write property indicating drag state. Primarily used to monitor or control drag behavior:

```xaml
<syncfusion:GroupBar DraggingItemInProgress="True" Height="200" Width="230" Name="groupBar">
    <syncfusion:GroupBarItem Header="Item 1"/>
    <syncfusion:GroupBarItem Header="Item 2"/>
</syncfusion:GroupBar>
```

```csharp
groupBar.DraggingItemInProgress = true;
```

> For drag-and-drop reordering of items, see [behavior-and-features.md](behavior-and-features.md).

---

## Hidden Items Count

Get the number of hidden GroupBarItems (items not visible due to StackMode constraints):

```csharp
int hiddenCount = groupBar.HiddenItemsCount;
```

This is a read-only dependency property. Use it to display a count or decide whether to offer a "show more items" UI.

---

## GroupBarItem Events

### GroupBarItemAdded

Fires when a `GroupBarItem` is added — either programmatically or via the built-in context menu:

```csharp
groupBar.GroupBarItemAdded += (sender, e) =>
{
    // e.Item is the newly added GroupBarItem
};
```

### GroupBarItemRemoved

Fires when a `GroupBarItem` is removed — programmatically or via context menu:

```csharp
groupBar.GroupBarItemRemoved += (sender, e) =>
{
    // e.Item is the removed GroupBarItem
};
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Item header text not showing | Using `HeaderText` in XAML | Use `Header` in XAML; `HeaderText` is the C# property |
| Item not expanding | `IsSelected` not set on any item | Set `IsSelected="True"` on at least one item in Default/MultipleExpansion modes |
| CornerRadius has no effect | StackMode + ThemeStudio theme | Corner radius styling may be overridden by ThemeStudio themes |
| `HiddenItemsCount` always 0 | Not in StackMode | This property only reflects hidden items in StackMode |

# Items and Panel Structure

The TabSplitter is composed of `TabSplitterItem` objects (the tabs), each containing two panel collections: `TopPanelItems` and `BottomPanelItems`. Each panel holds `SplitterPage` instances that render content.

## Table of Contents
- [TabSplitterItem Structure](#tabsplitteritem-structure)
- [SplitterPage: Header and Content](#splitterpage-header-and-content)
- [Selected Page](#selected-page)
- [Collapsing the Bottom Panel](#collapsing-the-bottom-panel)
- [Setting Bottom Panel Height](#setting-bottom-panel-height)
- [Hiding the Header on Single Child](#hiding-the-header-on-single-child)
- [Multiple TabSplitterItems](#multiple-tabsplitteritems)

---

## TabSplitterItem Structure

A `TabSplitterItem` represents one tab in the `TabSplitter`. It has two panel collections:

| Collection | Position | Description |
|------------|----------|-------------|
| `TopPanelItems` | Top (or left if Vertical) | Primary panel pages |
| `BottomPanelItems` | Bottom (or right if Vertical) | Secondary panel pages |

```xml
<syncfusion:TabSplitter>
    <syncfusion:TabSplitterItem Header="Window1.xml">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="XAML" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Design" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>
</syncfusion:TabSplitter>
```

**C#:**
```csharp
TabSplitterItem item = new TabSplitterItem() { Header = "Window1.xml" };

SplitterPage top = new SplitterPage() { Header = "XAML" };
SplitterPage bottom = new SplitterPage() { Header = "Design" };

item.TopPanelItems.Add(top);
item.BottomPanelItems.Add(bottom);

tabSplitter.Items.Add(item);
```

---

## SplitterPage: Header and Content

`SplitterPage` is a `ContentControl` — it displays a tab header and hosts any WPF content.

```xml
<syncfusion:SplitterPage Header="XAML">
    <!-- Any WPF content here -->
    <Grid>
        <TextBox AcceptsReturn="True" />
    </Grid>
</syncfusion:SplitterPage>
```

**Key `SplitterPage` properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Header` | `object` | The tab label shown in the panel header strip |
| `Content` | `object` | The visual content of the page (standard `ContentControl.Content`) |
| `IsSelectedPage` | `bool` | Sets this page as the active/selected tab |

---

## Selected Page

Use `IsSelectedPage="True"` to pre-select a specific `SplitterPage` on load.

**XAML:**
```xml
<syncfusion:TabSplitterItem Header="Window1.xml">
    <syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:SplitterPage Header="XAML" IsSelectedPage="True" />
    </syncfusion:TabSplitterItem.TopPanelItems>
    <syncfusion:TabSplitterItem.BottomPanelItems>
        <syncfusion:SplitterPage Header="Design" />
    </syncfusion:TabSplitterItem.BottomPanelItems>
</syncfusion:TabSplitterItem>
```

**C#:**
```csharp
splitterPage.IsSelectedPage = true;
```

**Default behavior:** If no page has `IsSelectedPage="True"`, the first page in each panel is selected by default.

---

## Collapsing the Bottom Panel

Set `IsCollapsedBottomPanel="True"` on a `TabSplitterItem` to collapse its bottom panel. The user can still expand it interactively.

**XAML:**
```xml
<syncfusion:TabSplitterItem Header="Window1.xml" IsCollapsedBottomPanel="True">
    <syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:SplitterPage Header="XAML" />
    </syncfusion:TabSplitterItem.TopPanelItems>
    <syncfusion:TabSplitterItem.BottomPanelItems>
        <syncfusion:SplitterPage Header="Design" />
    </syncfusion:TabSplitterItem.BottomPanelItems>
</syncfusion:TabSplitterItem>
```

**C#:**
```csharp
tabSplitterItem.IsCollapsedBottomPanel = true;
```

**Default:** `false` — both panels are visible.

**Use when:** You want the control to start in a "maximized top panel" state, with the bottom panel available on demand.

---

## Setting Bottom Panel Height

Control the initial height of the bottom panel across all items using `BottomPanelHeight` on the `TabSplitter`.

**XAML:**
```xml
<syncfusion:TabSplitter BottomPanelHeight="150">
    <syncfusion:TabSplitterItem Header="MainWindow.xml">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="Design" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="XAML" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>
</syncfusion:TabSplitter>
```

**C#:**
```csharp
tabSplitter.BottomPanelHeight = 150;
```

**Note:** `BottomPanelHeight` applies globally to all items in the `TabSplitter`. It sets the initial rendered height; the user can drag the splitter to resize.

---

## Hiding the Header on Single Child

When the `TabSplitter` contains only **one** `TabSplitterItem`, you can hide its tab header strip by enabling `HideHeaderOnSingleChild`.

**XAML:**
```xml
<syncfusion:TabSplitter HideHeaderOnSingleChild="True">
    <syncfusion:TabSplitterItem Header="Window1.xml">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="XAML" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Design" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>
</syncfusion:TabSplitter>
```

**C#:**
```csharp
tabSplitter.HideHeaderOnSingleChild = true;
```

**Constraint:** Only works when there is exactly **one** `TabSplitterItem`. With two or more items, the header always shows so the user can switch tabs.

---

## Multiple TabSplitterItems

Add multiple `TabSplitterItem` objects to create a multi-tab split view — each tab independently has its own top and bottom panels:

```xml
<syncfusion:TabSplitter>
    <syncfusion:TabSplitterItem Header="Window1.xaml">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="XAML" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Design" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>

    <syncfusion:TabSplitterItem Header="Window2.xaml">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="XAML" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Design" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>
</syncfusion:TabSplitter>
```

**C#:**
```csharp
TabSplitterItem item1 = new TabSplitterItem() { Header = "Window1.xaml" };
item1.TopPanelItems.Add(new SplitterPage() { Header = "XAML" });
item1.BottomPanelItems.Add(new SplitterPage() { Header = "Design" });

TabSplitterItem item2 = new TabSplitterItem() { Header = "Window2.xaml" };
item2.TopPanelItems.Add(new SplitterPage() { Header = "XAML" });
item2.BottomPanelItems.Add(new SplitterPage() { Header = "Design" });

tabSplitter.Items.Add(item1);
tabSplitter.Items.Add(item2);
```

---

## Related References

- [Getting Started](getting-started.md) — Initial setup
- [Orientation and Layout](orientation-and-layout.md) — Horizontal vs Vertical
- [Appearance](appearance.md) — Selected tab colors and themes

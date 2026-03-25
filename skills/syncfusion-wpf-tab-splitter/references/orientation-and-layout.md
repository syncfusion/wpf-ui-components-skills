# Orientation and Layout

The `TabSplitterItem` supports horizontal and vertical split orientations. Additional layout behaviors include swapping tab groups and collapsing/expanding panels.

---

## Orientation

Each `TabSplitterItem` has an `Orientation` property that controls how `TopPanelItems` and `BottomPanelItems` are arranged:

| Value | Layout |
|-------|--------|
| `Horizontal` (default) | Top panel above, bottom panel below — horizontal divider |
| `Vertical` | Left panel (top) beside right panel (bottom) — vertical divider |

**XAML — Vertical split:**
```xml
<syncfusion:TabSplitter>
    <syncfusion:TabSplitterItem Header="Window1.xml" Orientation="Vertical">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="XAML">
                <TextBlock Text="Left Panel" />
            </syncfusion:SplitterPage>
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Design">
                <TextBlock Text="Right Panel" />
            </syncfusion:SplitterPage>
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>
</syncfusion:TabSplitter>
```

**C#:**
```csharp
tabSplitterItem.Orientation = Orientation.Vertical;
```

**Default — Horizontal split (top/bottom):**
```xml
<syncfusion:TabSplitterItem Header="Window1.xml" Orientation="Horizontal">
    <!-- TopPanelItems renders above, BottomPanelItems below -->
</syncfusion:TabSplitterItem>
```

**When to use Vertical:** Use when content benefits from side-by-side comparison (e.g., source code on the left, preview on the right) rather than stacked panels.

---

## Swap

The TabSplitter provides built-in support for **swapping** the positions of the two tab groups. This is a UI-level interaction feature — the user can swap via the control's built-in swap button.

No additional configuration is required to enable swapping; it is part of the default TabSplitter UI behavior.

---

## Collapse and Expand

Each `TabSplitterItem` supports collapsing its bottom panel:

- **Collapse programmatically:** Set `IsCollapsedBottomPanel="True"` on the `TabSplitterItem`
- **Expand programmatically:** Set `IsCollapsedBottomPanel="False"`
- **User interaction:** The control renders a collapse/expand button at the splitter divider

See [items-and-panels.md](items-and-panels.md#collapsing-the-bottom-panel) for full usage details.

---

## Mixed Orientations

Each `TabSplitterItem` can have its own orientation. This allows different tabs to use different split directions:

```xml
<syncfusion:TabSplitter>
    <!-- Tab 1: horizontal split (top/bottom) -->
    <syncfusion:TabSplitterItem Header="MainWindow.xaml" Orientation="Horizontal">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="XAML" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Design" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>

    <!-- Tab 2: vertical split (left/right) -->
    <syncfusion:TabSplitterItem Header="App.xaml" Orientation="Vertical">
        <syncfusion:TabSplitterItem.TopPanelItems>
            <syncfusion:SplitterPage Header="Source" />
        </syncfusion:TabSplitterItem.TopPanelItems>
        <syncfusion:TabSplitterItem.BottomPanelItems>
            <syncfusion:SplitterPage Header="Preview" />
        </syncfusion:TabSplitterItem.BottomPanelItems>
    </syncfusion:TabSplitterItem>
</syncfusion:TabSplitter>
```

---

## Related References

- [Getting Started](getting-started.md) — Initial setup
- [Items and Panels](items-and-panels.md) — IsCollapsedBottomPanel, BottomPanelHeight
- [Appearance](appearance.md) — Visual styling

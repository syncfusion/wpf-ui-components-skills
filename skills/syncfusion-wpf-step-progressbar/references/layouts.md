# Layouts

## ItemsStretch

Controls how step items use available space. Default: `None`.

| Value | Behavior | Use When |
|---|---|---|
| `None` | Items retain natural size; use `ItemSpacing` for gaps | Fixed-size step bars, or when you control spacing manually |
| `Fill` | Items expand to fill all available space equally | Full-width step bars that should always span the container |
| `Auto` | Item size is driven by content and `SecondaryContent` size | Vertical step bars with variable-length content blocks |

---

## None Mode — ItemSpacing

When `ItemsStretch="None"` (default), control the gap between adjacent steps with `ItemSpacing`. Default: `80`.

```xaml
<Syncfusion:SfStepProgressBar SelectedIndex="2" ItemSpacing="150">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

```csharp
stepProgressBar.ItemSpacing = 150;
```

> **Note:** `ItemSpacing` is the distance from each step to its *previous* step. The single step size is calculated from `ItemSpacing` + `MarkerWidth` (horizontal) or `MarkerHeight` (vertical).

---

## Fill Mode — Stretch to Container

Use `ItemsStretch="Fill"` to make items expand and fill the available container width (horizontal) or height (vertical):

```xaml
<Syncfusion:SfStepProgressBar SelectedIndex="2" ItemsStretch="Fill">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

```csharp
stepProgressBar.ItemsStretch = ItemsStretch.Fill;
```

---

## Fill Mode — MinimumItemSpacing

When `ItemsStretch="Fill"`, set a minimum spacing to prevent items from collapsing too close together. Default: `40`.

```xaml
<Syncfusion:SfStepProgressBar
    ItemsStretch="Fill"
    MinimumItemSpacing="220"
    SelectedIndex="2"
    SelectedItemStatus="Indeterminate">
    <Syncfusion:StepViewItem Content="Ordered" />
    <Syncfusion:StepViewItem Content="Packed" />
    <Syncfusion:StepViewItem Content="Shipped" />
    <Syncfusion:StepViewItem Content="Delivered" />
</Syncfusion:SfStepProgressBar>
```

Wrap in a `ScrollViewer` if `MinimumItemSpacing` causes the bar to exceed the container:

```xaml
<ScrollViewer HorizontalScrollBarVisibility="Auto">
    <Syncfusion:SfStepProgressBar ItemsStretch="Fill" MinimumItemSpacing="220" SelectedIndex="2">
        ...
    </Syncfusion:SfStepProgressBar>
</ScrollViewer>
```

---

## Auto Mode — Content-Driven Sizing (Vertical Only)

`ItemsStretch="Auto"` sizes each step item based on its `Content` and `SecondaryContent` height. Only applicable in `Orientation="Vertical"`. Falls back to `MinimumItemSpacing` when content is smaller than `MarkerHeight`.

```xaml
<Syncfusion:SfStepProgressBar
    SelectedIndex="2"
    Orientation="Vertical"
    ItemsStretch="Auto"
    Margin="20">
    <Syncfusion:StepViewItem
        Content="Ordered"
        SecondaryContentTemplate="{StaticResource LongDescriptionTemplate}" />
    <Syncfusion:StepViewItem
        ContentTemplate="{StaticResource StepTwoTemplate}"
        SecondaryContentTemplate="{StaticResource StepTwoSecondaryTemplate}" />
    <Syncfusion:StepViewItem
        Content="Packed"
        SecondaryContentTemplate="{StaticResource ShortDescriptionTemplate}" />
    <Syncfusion:StepViewItem
        ContentTemplate="{StaticResource StepFourTemplate}"
        SecondaryContentTemplate="{StaticResource StepFourSecondaryTemplate}" />
</Syncfusion:SfStepProgressBar>
```

**When to use Auto:**
- Vertical wizard steps where each step has a different amount of descriptive text
- Onboarding flows where step descriptions vary significantly in length

---

## Decision Guide

```
Need the bar to span full container width/height?
  → YES: ItemsStretch="Fill"
         Set MinimumItemSpacing to prevent overcrowding
  → NO:  ItemsStretch="None" (default)
         Adjust ItemSpacing for desired gap between steps

Vertical orientation with variable content heights?
  → Use ItemsStretch="Auto"
     Falls back to MinimumItemSpacing for small content items
```

---

## Gotchas

- **`Auto` is vertical-only** — setting `ItemsStretch="Auto"` on a horizontal step bar behaves like `Fill`
- **`ItemSpacing` ignored in Fill/Auto** — `ItemSpacing` only applies in `None` mode
- **`MinimumItemSpacing` default is 40** — in Fill mode with many items and a narrow container, items may still look cramped; increase `MinimumItemSpacing` and add a `ScrollViewer`
- **Step bar doesn't auto-scroll** — wrap in `ScrollViewer` if content may overflow the container

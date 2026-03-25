# Selection Mode

`SfAccordion.SelectionMode` controls how many items can be expanded simultaneously and whether collapsing all items is allowed.

**Enum:** `AccordionSelectionMode`  
**Default:** `One`

---

## The Four Modes

| Mode | Max expanded | Min expanded | Can collapse all? |
|---|---|---|---|
| `One` | 1 | 1 | ❌ No |
| `OneOrMore` | Unlimited | 1 | ❌ No |
| `ZeroOrOne` | 1 | 0 | ✅ Yes |
| `ZeroOrMore` | Unlimited | 0 | ✅ Yes |

---

## One (Default)

Exactly one item is always expanded. The expanded item is locked — it cannot be collapsed by clicking; selecting another item collapses the previous one.

```xaml
<syncfusion:SfAccordion SelectionMode="One">
    <syncfusion:SfAccordionItem Header="WPF"          Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="Silverlight"  Content="Essential Studio for Silverlight"/>
    <syncfusion:SfAccordionItem Header="WinRT"        Content="Essential Studio for WinRT"/>
</syncfusion:SfAccordion>
```

**Use when:** Navigation panels, settings panels, and anywhere only one section should be open at a time.

---

## OneOrMore

Multiple items can be expanded. At least one must always remain expanded.

```xaml
<syncfusion:SfAccordion SelectionMode="OneOrMore">
    <syncfusion:SfAccordionItem Header="WPF"         Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="Silverlight" Content="Essential Studio for Silverlight"/>
    <syncfusion:SfAccordionItem Header="WinRT"       Content="Essential Studio for WinRT"/>
</syncfusion:SfAccordion>
```

```csharp
accordion.SelectionMode = AccordionSelectionMode.OneOrMore;
```

**Use when:** Multi-category filter panels or checklists where at least one selection is mandatory.

---

## ZeroOrOne

At most one item can be expanded, but all can be collapsed.

```xaml
<syncfusion:SfAccordion SelectionMode="ZeroOrOne">
    <syncfusion:SfAccordionItem Header="WPF"         Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="Silverlight" Content="Essential Studio for Silverlight"/>
</syncfusion:SfAccordion>
```

**Use when:** Optional collapsible panels where the user may want everything hidden, but only one at a time when expanded.

---

## ZeroOrMore

Multiple items can be expanded, and all can be collapsed.

```xaml
<syncfusion:SfAccordion SelectionMode="ZeroOrMore">
    <syncfusion:SfAccordionItem Header="WPF"          Content="Essential Studio for WPF" IsSelected="True"/>
    <syncfusion:SfAccordionItem Header="Silverlight"  Content="Essential Studio for Silverlight"/>
    <syncfusion:SfAccordionItem Header="WinRT"        Content="Essential Studio for WinRT" IsSelected="True"/>
</syncfusion:SfAccordion>
```

**Use when:** FAQ lists, feature comparison panels, or any scenario where multiple independent sections need to be open simultaneously.

---

## Decision Guide

```
User can only ever see one section at a time?
├── Yes → One required open?
│         ├── Yes → SelectionMode="One"       (default, navigation panels)
│         └── No  → SelectionMode="ZeroOrOne" (optional collapsible sidebar)
└── No  → At least one must stay open?
          ├── Yes → SelectionMode="OneOrMore"  (required multi-select filter)
          └── No  → SelectionMode="ZeroOrMore" (FAQ, independent sections)
```

---

## SelectAll / UnselectAll Behavior by Mode

| Method | `One` | `OneOrMore` | `ZeroOrOne` | `ZeroOrMore` |
|---|---|---|---|---|
| `SelectAll()` | Last item only | All items | Last item only | All items |
| `UnselectAll()` | No effect | Highest-index item stays | All collapsed | All collapsed |

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Item won't collapse when clicked | `SelectionMode="One"` — the item is locked | Change to `ZeroOrOne` if collapsing all is desired |
| `UnselectAll()` leaves one item expanded | `OneOrMore` requires at least one | Expected — highest-index item stays |
| Can't open two items at once | `SelectionMode` is `One` or `ZeroOrOne` | Switch to `OneOrMore` or `ZeroOrMore` |

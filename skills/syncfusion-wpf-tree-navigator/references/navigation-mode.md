# Navigation Mode

SfTreeNavigator supports two navigation modes controlled by the `NavigationMode` property:
`Default` and `Extended`.

---

## Default Mode

**`NavigationMode="Default"`** — Shows a single header row at the top with a back button when drilled into a child level.

```xaml
<navigation:SfTreeNavigator Header="Products"
                             NavigationMode="Default"
                             Width="300" Height="400"
                             ItemsSource="{Binding Models}">
    ...
</navigation:SfTreeNavigator>
```

**Behavior:**
- At root: shows `Header` text only (e.g., "Products")
- After drilling into an item: shows that item's `Header` + a back (`<`) button
- Only the current level's name is visible at top
- Clicking back returns one level up

**When to use:**  
Deep hierarchies (3+ levels). Keeps header area minimal — only the current context label is shown. Good for mobile-style navigation panels.

---

## Extended Mode

**`NavigationMode="Extended"`** — Shows a stacked breadcrumb of ALL ancestor headers from root to current level. Each ancestor header is clickable.

```xaml
<navigation:SfTreeNavigator Header="Products"
                             NavigationMode="Extended"
                             Width="300" Height="400"
                             ItemsSource="{Binding Models}">
    ...
</navigation:SfTreeNavigator>
```

**Behavior:**
- At root: shows root `Header` ("Products")
- After drilling two levels: shows root header → level 1 header → level 2 header stacked vertically
- Each header in the stack is a clickable `TreeNavigatorHeaderItem` — clicking navigates directly to that level
- Breadcrumb collapses back as user navigates up

**When to use:**  
Shallow hierarchies (2 levels) where showing the full path is helpful for context. Good for site-map style navigation where breadcrumb location matters.

---

## Decision Guide

| Factor | Use Default | Use Extended |
|---|---|---|
| Hierarchy depth | 3+ levels deep | 1-2 levels |
| Header space | Limited | Can spare vertical space |
| Navigation style | Back-button drill-down | Breadcrumb / direct jump |
| User expectation | Mobile-style | Site-map / explorer-style |

---

## Styling TreeNavigatorHeaderItem (Extended Mode)

In `Extended` mode, each stacked ancestor header renders as a `TreeNavigatorHeaderItem`. Style it to customize appearance:

```xaml
<navigation:SfTreeNavigator NavigationMode="Extended" ...>
    <navigation:SfTreeNavigator.Resources>
        <Style TargetType="navigation:TreeNavigatorHeaderItem">
            <Setter Property="FontWeight" Value="Bold"/>
            <Setter Property="FontSize" Value="13"/>
            <Setter Property="Foreground" Value="#1565C0"/>
            <Setter Property="Padding" Value="8,6"/>
        </Style>
    </navigation:SfTreeNavigator.Resources>
</navigation:SfTreeNavigator>
```

---

## Full Example — Both Modes

```xaml
<!-- Default mode -->
<navigation:SfTreeNavigator x:Name="defaultNav"
                             Header="Enterprise Suite"
                             NavigationMode="Default"
                             Width="280" Height="380"
                             Margin="0,0,20,0"
                             ItemsSource="{Binding Models}">
    <navigation:SfTreeNavigator.ItemTemplate>
        <HierarchicalDataTemplate ItemsSource="{Binding Models}">
            <TextBlock Text="{Binding Header}"/>
        </HierarchicalDataTemplate>
    </navigation:SfTreeNavigator.ItemTemplate>
</navigation:SfTreeNavigator>

<!-- Extended mode -->
<navigation:SfTreeNavigator x:Name="extendedNav"
                             Header="Enterprise Suite"
                             NavigationMode="Extended"
                             Width="280" Height="380"
                             ItemsSource="{Binding Models}">
    <navigation:SfTreeNavigator.ItemTemplate>
        <HierarchicalDataTemplate ItemsSource="{Binding Models}">
            <TextBlock Text="{Binding Header}"/>
        </HierarchicalDataTemplate>
    </navigation:SfTreeNavigator.ItemTemplate>
</navigation:SfTreeNavigator>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| No breadcrumb visible | `NavigationMode` not set to `Extended` | Set `NavigationMode="Extended"` explicitly |
| Ancestors not clickable | `Default` mode active | Switch to `Extended` mode |
| Stacked headers overflow | Too many levels in Extended | Prefer `Default` for deep trees |

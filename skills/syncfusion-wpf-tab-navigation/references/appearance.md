# Appearance Customization

TabNavigationControl exposes three visibility properties to control which UI chrome elements are shown.

---

## HeaderVisibility

Controls the visibility of the **header tab strip** — the row of tab buttons at the top.

```xaml
<!-- Hide the header -->
<syncfusion:TabNavigationControl x:Name="TabNavigation"
                                  HeaderVisibility="Collapsed">
    <syncfusion:TabNavigationItem Header="TabItem1" Content="TabNavigationItem 1"/>
    <syncfusion:TabNavigationItem Header="TabItem2" Content="TabNavigationItem 2"/>
    <syncfusion:TabNavigationItem Header="TabItem3" Content="TabNavigationItem 3"/>
</syncfusion:TabNavigationControl>
```

```csharp
// Hide the header
tabNavigation.HeaderVisibility = Visibility.Collapsed;

// Show the header
tabNavigation.HeaderVisibility = Visibility.Visible;
```

**When to use:**  
Hide when the navigation is driven programmatically or by external controls — e.g., a banner/rotator that auto-advances without user tab interaction.

---

## NavigationButtonVisibility

Controls the visibility of the **prev/next arrow navigation buttons**.

```xaml
<!-- Hide the navigation buttons -->
<syncfusion:TabNavigationControl x:Name="TabNavigation"
                                  TabStripVisibility="Visible"
                                  NavigationButtonVisibility="Collapsed">
    <syncfusion:TabNavigationItem Header="TabItem1" Content="TabNavigationItem 1"/>
    <syncfusion:TabNavigationItem Header="TabItem2" Content="TabNavigationItem 2"/>
</syncfusion:TabNavigationControl>
```

```csharp
// Hide navigation buttons
tabNavigation.NavigationButtonVisibility = Visibility.Collapsed;

// Show navigation buttons
tabNavigation.NavigationButtonVisibility = Visibility.Visible;
```

**When to use:**  
Hide when you want users to navigate only via the tab headers (not arrow buttons), or when there are only 2-3 items and arrow navigation is redundant.

---

## TabStripVisibility

Controls the visibility of the **tab strip panel** below the content area.

```xaml
<!-- Show the tab strip -->
<syncfusion:TabNavigationControl x:Name="TabNavigation"
                                  TabStripVisibility="Visible">
    <syncfusion:TabNavigationItem Header="TabItem1" Content="TabNavigationItem 1"/>
    <syncfusion:TabNavigationItem Header="TabItem2" Content="TabNavigationItem 2"/>
</syncfusion:TabNavigationControl>
```

```csharp
// Show tab strip
tabNavigation.TabStripVisibility = Visibility.Visible;

// Hide tab strip
tabNavigation.TabStripVisibility = Visibility.Collapsed;
```

**Default:** `Collapsed`. Set to `Visible` when you want the bottom tab strip panel displayed.

---

## Full Chrome Control Example

```xaml
<!-- Fully minimal — no header, no nav buttons, no tab strip -->
<syncfusion:TabNavigationControl TransitionEffect="Fade"
                                  HeaderVisibility="Collapsed"
                                  NavigationButtonVisibility="Collapsed"
                                  TabStripVisibility="Collapsed"
                                  ItemsSource="{Binding BannerItems}"/>
```

```xaml
<!-- Full chrome — all elements visible -->
<syncfusion:TabNavigationControl TransitionEffect="Slide"
                                  HeaderVisibility="Visible"
                                  NavigationButtonVisibility="Visible"
                                  TabStripVisibility="Visible"
                                  ItemsSource="{Binding MyCollection}"/>
```

---

## Visibility Properties Summary

| Property | Controls | Default |
|---|---|---|
| `HeaderVisibility` | Header tab strip (top row of tab buttons) | `Visible` |
| `NavigationButtonVisibility` | Prev/Next arrow buttons | `Visible` |
| `TabStripVisibility` | Bottom tab strip panel | `Collapsed` |

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| No way to navigate after hiding header | `HeaderVisibility="Collapsed"` with no alternative | Keep at least `NavigationButtonVisibility="Visible"` or navigate programmatically |
| Tab strip not appearing | Default is `Collapsed` | Explicitly set `TabStripVisibility="Visible"` |
| `NavigationButtonVisibility` has no effect | `TabStripVisibility="Collapsed"` hides the region | Set `TabStripVisibility="Visible"` first |

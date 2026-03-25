# Orientation and Layout

## GroupBar Orientation

GroupBar supports both vertical and horizontal layouts. By default, GroupBar is vertical (items stack top-to-bottom).

```xaml
<!-- Horizontal: items placed left-to-right -->
<syncfusion:GroupBar Orientation="Horizontal" Height="200" Width="500" Name="groupBar">
    <syncfusion:GroupBarItem Header="Mailbox"/>
    <syncfusion:GroupBarItem Header="Contacts"/>
</syncfusion:GroupBar>
```

```csharp
groupBar.Orientation = Orientation.Horizontal;
```

When horizontal, `ItemContentLength` sets the **width** of the expanded content area (instead of height).

---

## GroupView and GroupViewItem Orientation

`GroupView` items can also be oriented horizontally or vertically:

```xaml
<syncfusion:GroupBarItem Header="Options">
    <syncfusion:GroupView>
        <!-- Horizontal GroupViewItems -->
        <syncfusion:GroupViewItem Text="Item 1"/>
        <syncfusion:GroupViewItem Text="Item 2"/>
    </syncfusion:GroupView>
</syncfusion:GroupBarItem>
```

Use the `Orientation` property on `GroupView` to control layout direction:

```xaml
<syncfusion:GroupView Orientation="Horizontal">
    <syncfusion:GroupViewItem Text="A"/>
    <syncfusion:GroupViewItem Text="B"/>
</syncfusion:GroupView>
```

---

## Adjusting GroupBar Width

Set `Width` on the `GroupBar` control directly. In StackMode, the Navigation Pane popup can be resized by the user if `PopupResizeDirection` is set:

```xaml
<syncfusion:GroupBar Width="230" Height="400" VisualMode="StackMode"/>
```

For minimum/maximum width constraints, use standard WPF `MinWidth`/`MaxWidth`.

---

## Changing Header Height

Control the height of `GroupBarItem` headers via the `GroupBarItemHeight` property on `GroupBar`, or by styling the item header template:

```xaml
<syncfusion:GroupBar GroupBarItemHeight="32" Height="300" Width="230">
    <syncfusion:GroupBarItem Header="Mailbox"/>
</syncfusion:GroupBar>
```

To use a custom style for the header border (background, corner radius, height):

```xaml
<Window.Resources>
    <Style x:Key="headerStyle" TargetType="{x:Type Border}">
        <Setter Property="CornerRadius" Value="7"/>
        <Setter Property="Background"   Value="LightSteelBlue"/>
        <Setter Property="Height"       Value="30"/>
    </Style>
</Window.Resources>

<syncfusion:GroupBar GroupBarHeaderStyle="{StaticResource headerStyle}"/>
```

---

## Rotating the GroupBar

The entire GroupBar can be rotated using a `LayoutTransform`:

```xaml
<syncfusion:GroupBar Height="230" Width="300" Name="groupBar">
    <syncfusion:GroupBar.LayoutTransform>
        <RotateTransform Angle="90"/>
    </syncfusion:GroupBar.LayoutTransform>
    <syncfusion:GroupBarItem Header="Mailbox"/>
    <syncfusion:GroupBarItem Header="Contacts"/>
</syncfusion:GroupBar>
```

---

## FlowDirection (RTL Support)

Apply right-to-left layout for Arabic, Hebrew, or other RTL languages:

```xaml
<syncfusion:GroupBar FlowDirection="RightToLeft" Height="300" Width="230" VisualMode="StackMode">
    <syncfusion:GroupBarItem Header="البريد"/>
    <syncfusion:GroupBarItem Header="جهات الاتصال"/>
</syncfusion:GroupBar>
```

```csharp
groupBar.FlowDirection = FlowDirection.RightToLeft;
```

---

## Navigation Pane: PopupResizeDirection

In StackMode, the Navigation Pane popup can be configured to resize in specific directions:

```xaml
<syncfusion:GroupBar Height="200" Width="230"
                     VisualMode="StackMode"
                     PopupResizeDirection="Both"
                     Name="groupBar">
    <syncfusion:GroupBarItem Header="Settings" IsSelected="True">
        <StackPanel>
            <TextBlock Text="Option A"/>
        </StackPanel>
    </syncfusion:GroupBarItem>
</syncfusion:GroupBar>
```

```csharp
groupBar.PopupResizeDirection = PopupResizeDirection.Both;
```

**PopupResizeDirection values:**

| Value | Behavior |
|---|---|
| `Both` | Resize in both horizontal and vertical directions |
| `Horizontal` | Resize horizontally only |
| `Vertical` | Resize vertically only |
| `None` | No resizing allowed |

---

## Navigation Pane: ShowGripper

The gripper lets users drag to resize the item area in StackMode:

```xaml
<syncfusion:GroupBar VisualMode="StackMode" ShowGripper="True" Height="300" Width="230"/>
```

```csharp
groupBar.ShowGripper = true;
```

> `ShowGripper` default is `true`. Only applicable in `StackMode`.

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Horizontal orientation content cut off | `ItemContentLength` not set | Set `ItemContentLength` to desired width for content area |
| `PopupResizeDirection` has no effect | Not in StackMode | Only applies to StackMode's navigation pane popup |
| `GroupBarHeaderStyle` not applied | Wrong `TargetType` in style | Ensure `TargetType="{x:Type Border}"` in the header style |
| RTL text alignment broken | Individual item `FlowDirection` overrides parent | Set `FlowDirection` on the root `GroupBar`, not individual items |

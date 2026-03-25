# Visual Styles in SfAvatarView (WPF)

## Table of Contents
- [AvatarShape Overview](#avatarshape-overview)
- [Circle Shape](#circle-shape)
- [Square Shape](#square-shape)
- [Custom Shape](#custom-shape)
- [AvatarSize Values](#avatarsize-values)
- [Reusing Styles with ResourceDictionary](#reusing-styles-with-resourcedictionary)

## AvatarShape Overview

Use the `AvatarShape` property to control the visual outline of the avatar:

| AvatarShape | Description |
|---|---|
| `Circle` | Circular avatar (default) |
| `Square` | Square avatar with no rounding |
| `Custom` | Manual control of `Height`, `Width`, `CornerRadius` |

Default shape is `Circle` if not specified.

---

## Circle Shape

Use `AvatarShape="Circle"` with `AvatarSize` to pick from 5 pre-defined sizes:

```xml
<syncfusion:SfAvatarView AvatarShape="Circle"
                         AvatarSize="Large"
                         ContentType="AvatarCharacter"
                         AvatarCharacter="Avatar5" />
```

**Displaying all circle sizes:**

```xml
<StackPanel Orientation="Horizontal">
    <syncfusion:SfAvatarView AvatarShape="Circle" AvatarSize="ExtraSmall"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
    <syncfusion:SfAvatarView AvatarShape="Circle" AvatarSize="Small"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
    <syncfusion:SfAvatarView AvatarShape="Circle" AvatarSize="Medium"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
    <syncfusion:SfAvatarView AvatarShape="Circle" AvatarSize="Large"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
    <syncfusion:SfAvatarView AvatarShape="Circle" AvatarSize="ExtraLarge"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
</StackPanel>
```

---

## Square Shape

Use `AvatarShape="Square"` with `AvatarSize` for square avatars at the same 5 pre-defined sizes:

```xml
<syncfusion:SfAvatarView AvatarShape="Square"
                         AvatarSize="Large"
                         ContentType="AvatarCharacter"
                         AvatarCharacter="Avatar5" />
```

**Displaying all square sizes:**

```xml
<StackPanel Orientation="Horizontal">
    <syncfusion:SfAvatarView AvatarShape="Square" AvatarSize="ExtraSmall"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
    <syncfusion:SfAvatarView AvatarShape="Square" AvatarSize="Small"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
    <syncfusion:SfAvatarView AvatarShape="Square" AvatarSize="Medium"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
    <syncfusion:SfAvatarView AvatarShape="Square" AvatarSize="Large"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
    <syncfusion:SfAvatarView AvatarShape="Square" AvatarSize="ExtraLarge"
                             ContentType="AvatarCharacter" AvatarCharacter="Avatar13"/>
</StackPanel>
```

---

## Custom Shape

Use `AvatarShape="Custom"` to fully control dimensions via `Height`, `Width`, `CornerRadius`, and `FontSize`. The `AvatarSize` property is ignored in Custom mode.

```xml
<syncfusion:SfAvatarView AvatarShape="Custom"
                         Height="60"
                         Width="75"
                         AvatarName="Alex"
                         FontSize="30"
                         CornerRadius="0,20,0,20" />
```

**Code-behind:**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    AvatarShape = AvatarShape.Custom,
    Height = 60,
    Width = 75,
    AvatarName = "Alex",
    FontSize = 30,
    CornerRadius = new CornerRadius(0, 20, 0, 20)
};
```

**When to use Custom:**
- Non-square aspect ratios (e.g., 60×75)
- Asymmetric corner rounding (e.g., pill or diamond variants)
- Matching specific UI design dimensions

---

## AvatarSize Values

`AvatarSize` applies to `Circle` and `Square` shapes only:

| AvatarSize | Typical Use |
|---|---|
| `ExtraSmall` | Compact lists, inline name chips |
| `Small` | Comment threads, chat lists |
| `Medium` | Standard list items |
| `Large` | Profile cards, contact details |
| `ExtraLarge` | Profile pages, hero sections |

```xml
<!-- Standard profile card avatar -->
<syncfusion:SfAvatarView AvatarShape="Circle"
                         AvatarSize="ExtraLarge"
                         ContentType="Initials"
                         AvatarName="Maria Garcia"
                         InitialsType="DoubleCharacter" />
```

---

## Reusing Styles with ResourceDictionary

Define a shared style to avoid repeating `ContentType` and `AvatarCharacter` across multiple avatars:

```xml
<Window.Resources>
    <Style x:Key="TeamAvatarStyle" TargetType="syncfusion:SfAvatarView">
        <Setter Property="ContentType" Value="AvatarCharacter"/>
        <Setter Property="AvatarCharacter" Value="Avatar13"/>
        <Setter Property="AvatarShape" Value="Circle"/>
    </Style>
</Window.Resources>

<!-- Apply at different sizes -->
<syncfusion:SfAvatarView AvatarSize="Small"  Style="{StaticResource TeamAvatarStyle}"/>
<syncfusion:SfAvatarView AvatarSize="Medium" Style="{StaticResource TeamAvatarStyle}"/>
<syncfusion:SfAvatarView AvatarSize="Large"  Style="{StaticResource TeamAvatarStyle}"/>
```

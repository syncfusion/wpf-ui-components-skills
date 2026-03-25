# Customization in SfAvatarView (WPF)

## Table of Contents
- [Border](#border)
- [Background](#background)
- [Gradient Background](#gradient-background)
- [Font](#font)

## Border

### BorderBrush

Control the border color using `BorderBrush`:

```xml
<syncfusion:SfAvatarView ContentType="AvatarCharacter"
                         AvatarCharacter="Avatar16"
                         AvatarSize="ExtraLarge"
                         BorderBrush="Red" />
```

**Code-behind:**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    ContentType = AvatarContentType.AvatarCharacter,
    AvatarSize = AvatarSize.ExtraLarge,
    AvatarCharacter = AvatarCharacter.Avatar16,
    BorderBrush = new SolidColorBrush(Colors.Red),
    BorderThickness = new Thickness(2)
};
```

### BorderThickness

Control the border width using `BorderThickness`:

```xml
<syncfusion:SfAvatarView AvatarShape="Custom"
                         AvatarSize="ExtraLarge"
                         BorderBrush="Black"
                         BorderThickness="4" />
```

**Code-behind:**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.ExtraLarge,
    BorderThickness = new Thickness(4),
    BorderBrush = new SolidColorBrush(Colors.Black)
};
```

**Tip:** Always set both `BorderBrush` and `BorderThickness` together — a border only becomes visible when both have non-default values.

---

## Background

Set the avatar background to any solid color using the `Background` property. Commonly used with `Initials` content type:

```xml
<syncfusion:SfAvatarView ContentType="Initials"
                         AvatarSize="ExtraLarge"
                         AvatarName="Alex"
                         InitialsType="DoubleCharacter"
                         Background="Bisque" />
```

**Code-behind:**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    ContentType = AvatarContentType.Initials,
    AvatarSize = AvatarSize.ExtraLarge,
    AvatarName = "Alex",
    InitialsType = AvatarInitialsType.DoubleCharacter,
    Background = new SolidColorBrush(Colors.Bisque)
};
```

---

## Gradient Background

Use `LinearGradientBrush` or `RadialGradientBrush` as the `Background` for gradient fills:

**Linear gradient (XAML):**

```xml
<syncfusion:SfAvatarView ContentType="Initials"
                         AvatarName="Alex"
                         AvatarSize="ExtraLarge"
                         InitialsType="DoubleCharacter">
    <syncfusion:SfAvatarView.Background>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,0">
            <GradientStop Color="#2F9BDF" Offset="0"/>
            <GradientStop Color="#51F1F2" Offset="1"/>
        </LinearGradientBrush>
    </syncfusion:SfAvatarView.Background>
</syncfusion:SfAvatarView>
```

**Linear gradient (code-behind):**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    ContentType = AvatarContentType.Initials,
    AvatarSize = AvatarSize.ExtraLarge,
    InitialsType = AvatarInitialsType.DoubleCharacter,
    AvatarName = "Alex",
    Background = new LinearGradientBrush
    {
        StartPoint = new Point(0, 0),
        EndPoint = new Point(1, 0),
        GradientStops = new GradientStopCollection
        {
            new GradientStop { Color = Color.FromArgb(255, 47, 155, 223), Offset = 0 },
            new GradientStop { Color = Color.FromArgb(255, 81, 241, 242), Offset = 1 }
        }
    }
};
```

**Vertical gradient example:**

```xml
<syncfusion:SfAvatarView.Background>
    <LinearGradientBrush StartPoint="0,0" EndPoint="0,1">
        <GradientStop Color="#FF6B6B" Offset="0"/>
        <GradientStop Color="#FF8E53" Offset="1"/>
    </LinearGradientBrush>
</syncfusion:SfAvatarView.Background>
```

---

## Font

Customize the initials text appearance using `FontFamily`, `FontSize`, and `Foreground`:

```xml
<syncfusion:SfAvatarView ContentType="Initials"
                         AvatarName="Alex"
                         AvatarSize="ExtraLarge"
                         FontFamily="Segoe UI Variable Static Display"
                         Foreground="#FF69531C"
                         Background="#FFF2E9C8" />
```

**Code-behind:**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    ContentType = AvatarContentType.Initials,
    AvatarName = "Alex",
    AvatarSize = AvatarSize.ExtraLarge,
    FontFamily = new FontFamily("Segoe UI Variable Static Display"),
    Foreground = new SolidColorBrush(Color.FromArgb(0xFF, 0x69, 0x53, 0x1C)),
    Background = new SolidColorBrush(Color.FromArgb(0xFF, 0xF2, 0xE9, 0xC8))
};
```

**Font properties apply to:**
- `Initials` content type (initials text rendering)
- `ContentTemplate` content (if the template contains text)

**Note:** `FontFamily` and `Foreground` have no visible effect on `AvatarCharacter` or `CustomImage` content types.

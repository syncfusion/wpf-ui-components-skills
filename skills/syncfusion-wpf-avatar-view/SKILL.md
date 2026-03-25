---
name: syncfusion-wpf-avatar-view
description: Implement Syncfusion WPF SfAvatarView for displaying user profile avatars and initials. Use this when working with avatar controls, user profile displays, group avatars, or badge integration in WPF. Covers all content types (Default, Initials, CustomImage, AvatarCharacter, Group, ContentTemplate), visual styles (Circle, Square, custom shape/size), appearance customization, and SfBadge integration.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing SfAvatarView (WPF)

## When to Use This Skill

Use this skill when the user needs to:
- Display a user profile picture, initials, or avatar character
- Show text initials (single or double character) with a colored background
- Use pre-built avatar characters (Avatar1–Avatar25)
- Build a group avatar view from a data collection
- Customize avatar shape (Circle, Square, Custom) and size
- Apply borders, backgrounds, gradients, or font styling to an avatar
- Attach a badge (status indicator) to an avatar

**Component:** `SfAvatarView`
**Assembly:** `Syncfusion.Shared.WPF`
**XAML Namespace:** `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"`

---

## Component Overview

`SfAvatarView` displays a user avatar in one of five content modes:

| ContentType | What it Shows |
|---|---|
| `Default` | Built-in Avatar1 character, no setup needed |
| `Initials` | Text initials extracted from `AvatarName` |
| `CustomImage` | User-supplied image via `ImageSource` |
| `AvatarCharacter` | One of 25 pre-built character images |
| `Group` | Multiple avatars from a bound collection |

Shape is controlled by `AvatarShape` (Circle / Square / Custom). Size by `AvatarSize` (ExtraSmall → ExtraLarge).

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package and assembly setup
- XAML namespace declaration
- Minimal working example (Default type)
- ImageSource quick start
- Required CSS/theme imports

### Content Types
📄 **Read:** [references/content-types.md](references/content-types.md)
- `Default` — zero-config avatar character
- `Initials` — `AvatarName` + `InitialsType` (Single/Double)
- `CustomImage` — `ImageSource` with pack:// URI
- `AvatarCharacter` — Avatar1–Avatar25 enum
- `Group` view — `GroupSource` + member path properties + ViewModel pattern
- `ContentTemplate` — fully custom DataTemplate rendering

### Visual Styles
📄 **Read:** [references/visual-styles.md](references/visual-styles.md)
- `AvatarShape`: Circle (default), Square, Custom
- `AvatarSize`: ExtraSmall, Small, Medium, Large, ExtraLarge
- Custom shape — explicit Height, Width, CornerRadius, FontSize
- Style reuse via WPF ResourceDictionary

### Customization
📄 **Read:** [references/customization.md](references/customization.md)
- `BorderBrush` and `BorderThickness`
- `Background` — solid color
- Gradient background — `LinearGradientBrush` / `RadialGradientBrush`
- `FontFamily`, `FontSize`, `Foreground` for initials text

### Badge Integration
📄 **Read:** [references/badge-integration.md](references/badge-integration.md)
- Attaching `SfBadge` to `SfAvatarView`
- Positioning badge with `HorizontalPosition` / `VerticalPosition`
- Status indicator pattern (online/offline dot)

---

## Quick Start Example

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

**Display initials:**
```xml
<syncfusion:SfAvatarView ContentType="Initials"
                         AvatarName="Alex"
                         InitialsType="DoubleCharacter"
                         AvatarShape="Circle"
                         AvatarSize="Large" />
```

**Display a custom image:**
```xml
<syncfusion:SfAvatarView ContentType="CustomImage"
                         AvatarShape="Circle"
                         AvatarSize="Large"
                         ImageSource="pack://application:,,,/Assets/profile.png" />
```

**Display a pre-built character:**
```xml
<syncfusion:SfAvatarView ContentType="AvatarCharacter"
                         AvatarCharacter="Avatar5"
                         AvatarShape="Circle"
                         AvatarSize="Medium" />
```

---

## Common Patterns

**Pattern 1 — Pick content type based on available data:**
- Have a profile image URL → `ContentType="CustomImage"` + `ImageSource`
- Have a name string → `ContentType="Initials"` + `AvatarName`
- Need a placeholder → `ContentType="AvatarCharacter"` + choose Avatar1–Avatar25
- Need fully custom UI inside the avatar → `ContentTemplate` with `DataTemplate`

**Pattern 2 — Group avatar from a list:**
- Bind a collection → `ContentType="Group"` + `GroupSource`
- Map member paths: `InitialsMemberPath`, `ImageSourceMemberPath`, `BackgroundColorMemberPath`, `InitialsColorMemberPath`

**Pattern 3 — Shape + size selection:**
- Profile card with circular avatar → `AvatarShape="Circle"` + `AvatarSize` enum
- Square tile avatar → `AvatarShape="Square"` + `AvatarSize` enum
- Fully custom dimensions → `AvatarShape="Custom"` + explicit `Height`/`Width`/`CornerRadius`

---

## Key Props Reference

| Property | Type | Purpose |
|---|---|---|
| `ContentType` | `AvatarContentType` | Selects rendering mode |
| `AvatarName` | `string` | Source for initials extraction |
| `InitialsType` | `AvatarInitialsType` | Single or double character initials |
| `ImageSource` | `ImageSource` | Image URI for CustomImage mode |
| `AvatarCharacter` | `AvatarCharacter` | Pre-built character enum (Avatar1–Avatar25) |
| `AvatarShape` | `AvatarShape` | Circle / Square / Custom |
| `AvatarSize` | `AvatarSize` | ExtraSmall / Small / Medium / Large / ExtraLarge |
| `GroupSource` | `IEnumerable` | Data collection for Group mode |
| `ContentTemplate` | `DataTemplate` | Custom template for avatar content |
| `BorderBrush` | `Brush` | Border color |
| `BorderThickness` | `Thickness` | Border width |
| `Background` | `Brush` | Background fill (solid or gradient) |
| `CornerRadius` | `CornerRadius` | Rounding for Custom shape |

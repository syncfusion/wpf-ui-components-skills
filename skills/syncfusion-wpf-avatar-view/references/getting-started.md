# Getting Started with SfAvatarView (WPF)

## Installation

Install via NuGet Package Manager:

```
Syncfusion.Shared.WPF
```

Or via Package Manager Console:

```powershell
Install-Package Syncfusion.Shared.WPF
```

## Assembly Reference

Add the following assembly to your project:

```
Syncfusion.Shared.WPF.dll
```

## XAML Namespace

Declare the Syncfusion namespace in your Window or UserControl:

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

## Minimal Working Example

The simplest avatar — uses `Default` content type with no configuration needed:

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:SfAvatarView />
    </Grid>
</Window>
```

This renders a pre-built Avatar1 character in Circle shape at Small size.

## Quick Start: Display a Profile Image

Use `ContentType="CustomImage"` with a local asset:

```xml
<syncfusion:SfAvatarView ContentType="CustomImage"
                         AvatarShape="Circle"
                         AvatarSize="Large"
                         ImageSource="pack://application:,,,/Assets/profile.png" />
```

**Code-behind equivalent:**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    ContentType = AvatarContentType.CustomImage,
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large,
    ImageSource = new BitmapImage(new Uri("pack://application:,,,/Assets/profile.png"))
};
grid.Children.Add(avatarView);
```

## Quick Start: Display Initials

When no image is available, show initials from a name:

```xml
<syncfusion:SfAvatarView ContentType="Initials"
                         AvatarName="Alex Johnson"
                         InitialsType="DoubleCharacter"
                         AvatarShape="Circle"
                         AvatarSize="Large" />
```

Renders "AJ" inside the avatar circle.

## Required Namespaces (Code-Behind)

```csharp
using Syncfusion.Windows.Shared;
```

## Common Setup Mistakes

- **Missing XAML namespace** — ensure `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"` is declared
- **Image not showing** — verify the `pack://application:,,,/` URI is correct and image Build Action is `Resource`
- **Default avatar renders as blank** — `Default` type always shows Avatar1; if blank, check assembly reference

# Content Types in SfAvatarView (WPF)

## Table of Contents
- [Overview](#overview)
- [Default](#default)
- [Initials](#initials)
- [CustomImage](#customimage)
- [AvatarCharacter](#avatarcharacter)
- [Group](#group)
- [ContentTemplate](#contenttemplate)

## Overview

The `ContentType` property (type: `AvatarContentType`) controls what the avatar renders. Choose based on available data:

| ContentType | When to Use |
|---|---|
| `Default` | Placeholder with no data required |
| `Initials` | You have a name string |
| `CustomImage` | You have an image path or URI |
| `AvatarCharacter` | You want a pre-built character |
| `Group` | You have a collection of users |

---

## Default

Zero-configuration avatar. Renders Avatar1 character in Circle/Small by default.

```xml
<syncfusion:SfAvatarView />
```

Use as a fallback when no user data is available.

---

## Initials

Displays text initials extracted from `AvatarName`. Combine with `InitialsType` to control single or double character display.

**Single character** (first letter of name):

```xml
<syncfusion:SfAvatarView ContentType="Initials"
                         AvatarName="Alex"
                         InitialsType="SingleCharacter"
                         AvatarShape="Circle"
                         AvatarSize="Large" />
```

Renders: "A"

**Double character** (first letters of first + last name):

```xml
<syncfusion:SfAvatarView ContentType="Initials"
                         AvatarName="Alex Johnson"
                         InitialsType="DoubleCharacter"
                         AvatarShape="Circle"
                         AvatarSize="Large" />
```

Renders: "AJ"

**Code-behind:**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    ContentType = AvatarContentType.Initials,
    AvatarName = "Alex Johnson",
    InitialsType = AvatarInitialsType.DoubleCharacter,
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large
};
```

**Key properties:**
- `AvatarName` — full name string; initials are auto-extracted
- `InitialsType` — `AvatarInitialsType.SingleCharacter` or `AvatarInitialsType.DoubleCharacter`

---

## CustomImage

Renders a user-supplied image. Set `ImageSource` to a local resource or URI.

```xml
<syncfusion:SfAvatarView ContentType="CustomImage"
                         AvatarShape="Circle"
                         AvatarSize="ExtraLarge"
                         ImageSource="pack://application:,,,/Assets/user.png" />
```

**Code-behind:**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    ContentType = AvatarContentType.CustomImage,
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.ExtraLarge,
    ImageSource = new BitmapImage(new Uri("pack://application:,,,/Assets/user.png"))
};
```

**Note:** Ensure the image file has Build Action set to `Resource` in Visual Studio.

---

## AvatarCharacter

Renders one of 25 pre-built avatar character illustrations. Use `AvatarCharacter` enum to select.

```xml
<syncfusion:SfAvatarView ContentType="AvatarCharacter"
                         AvatarCharacter="Avatar7"
                         AvatarShape="Circle"
                         AvatarSize="Large" />
```

**Available values:** `Avatar1` through `Avatar25`

**Code-behind:**

```csharp
SfAvatarView avatarView = new SfAvatarView
{
    ContentType = AvatarContentType.AvatarCharacter,
    AvatarCharacter = AvatarCharacter.Avatar7,
    AvatarShape = AvatarShape.Circle,
    AvatarSize = AvatarSize.Large
};
```

Use when you want a consistent illustrated avatar style without requiring user images.

---

## Group

Displays multiple avatars from a data collection in a single stacked view. Requires `GroupSource` binding and member path properties.

**ViewModel:**

```csharp
public class Employee
{
    public string Name { get; set; }
    public string ImagePath { get; set; }
    public Color BackgroundColor { get; set; }
    public Color ForegroundColor { get; set; }
}

public class EmployeeViewModel
{
    public ObservableCollection<Employee> Employees { get; set; }

    public EmployeeViewModel()
    {
        Employees = new ObservableCollection<Employee>
        {
            new Employee { Name = "Alex", BackgroundColor = Colors.LightBlue, ForegroundColor = Colors.White },
            new Employee { Name = "Maria", ImagePath = "pack://application:,,,/Assets/maria.png" },
            new Employee { Name = "John", BackgroundColor = Colors.LightGreen, ForegroundColor = Colors.White }
        };
    }
}
```

**XAML:**

```xml
<Window.DataContext>
    <local:EmployeeViewModel />
</Window.DataContext>

<syncfusion:SfAvatarView ContentType="Group"
                         GroupSource="{Binding Employees}"
                         InitialsMemberPath="Name"
                         ImageSourceMemberPath="ImagePath"
                         BackgroundColorMemberPath="BackgroundColor"
                         InitialsColorMemberPath="ForegroundColor"
                         AvatarShape="Circle"
                         AvatarSize="Large" />
```

**Group member path properties:**

| Property | Maps To |
|---|---|
| `InitialsMemberPath` | Property on model containing the name for initials |
| `ImageSourceMemberPath` | Property containing the image path |
| `BackgroundColorMemberPath` | Property containing the background `Color` |
| `InitialsColorMemberPath` | Property containing the initials text `Color` |

**Note:** If an item has an `ImageSourceMemberPath` value, it renders the image; otherwise it falls back to initials.

---

## ContentTemplate

Use `ContentTemplate` when you need fully custom avatar content rendered via a `DataTemplate`. Bind to `Content` inside the template.

```xml
<syncfusion:SfAvatarView AvatarShape="Circle"
                         AvatarSize="ExtraLarge">
    <syncfusion:SfAvatarView.ContentTemplate>
        <DataTemplate>
            <Grid>
                <Image Source="pack://application:,,,/Assets/custom.png"
                       Stretch="UniformToFill" />
                <TextBlock Text="VP"
                           Foreground="White"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Bottom"
                           FontSize="10" />
            </Grid>
        </DataTemplate>
    </syncfusion:SfAvatarView.ContentTemplate>
</syncfusion:SfAvatarView>
```

Use `ContentTemplate` when:
- You need to overlay elements inside the avatar
- The avatar content is not a single image or initials
- You need complex data-bound custom rendering

# Badge Integration with SfAvatarView (WPF)

## Overview

`SfAvatarView` supports `SfBadge` as an attached property to display status indicators — such as online/offline, notification counts, or activity states.

**Assembly required:** `Syncfusion.Tools.WPF` (for `SfBadge`)

---

## Adding a Badge

Use `SfBadge.Badge` as an attached property on `SfAvatarView`:

```xml
<Grid HorizontalAlignment="Center" VerticalAlignment="Center">
    <syncfusion:SfAvatarView ContentType="AvatarCharacter"
                             AvatarCharacter="Avatar1"
                             AvatarShape="Circle"
                             AvatarSize="Large">
        <syncfusion:SfBadge.Badge>
            <syncfusion:SfBadge Shape="None"
                                HorizontalPosition="0.83"
                                VerticalPosition="0.85">
                <syncfusion:SfBadge.Content>
                    <Viewbox>
                        <Grid Height="13" Width="13">
                            <Ellipse Fill="LimeGreen"
                                     Stroke="White"
                                     StrokeThickness="1"/>
                        </Grid>
                    </Viewbox>
                </syncfusion:SfBadge.Content>
            </syncfusion:SfBadge>
        </syncfusion:SfBadge.Badge>
    </syncfusion:SfAvatarView>
</Grid>
```

---

## Badge Positioning

Control badge placement using `HorizontalPosition` and `VerticalPosition` (0.0 = left/top, 1.0 = right/bottom):

| Position | HorizontalPosition | VerticalPosition |
|---|---|---|
| Bottom-right (status dot) | `0.83` | `0.85` |
| Top-right (notification) | `0.83` | `0.15` |
| Bottom-left | `0.17` | `0.85` |
| Center | `0.5` | `0.5` |

---

## Status Indicator Pattern

The most common use — a colored dot indicating online/offline/busy status:

```xml
<syncfusion:SfAvatarView ContentType="AvatarCharacter"
                         AvatarCharacter="Avatar1"
                         AvatarShape="Circle"
                         AvatarSize="Large">
    <syncfusion:SfBadge.Badge>
        <syncfusion:SfBadge Shape="None"
                            HorizontalPosition="0.83"
                            VerticalPosition="0.85">
            <syncfusion:SfBadge.Content>
                <Viewbox>
                    <Grid Height="13" Width="13">
                        <!-- Green = online, Gray = offline, Orange = busy -->
                        <Ellipse Fill="LimeGreen" Stroke="White" StrokeThickness="1"/>
                        <TextBlock FontFamily="Segoe MDL2 Assets"
                                   Text="&#xE930;"
                                   Foreground="White"
                                   HorizontalAlignment="Center"
                                   VerticalAlignment="Center"/>
                    </Grid>
                </Viewbox>
            </syncfusion:SfBadge.Content>
        </syncfusion:SfBadge>
    </syncfusion:SfBadge.Badge>
</syncfusion:SfAvatarView>
```

---

## Notes

- `Shape="None"` on `SfBadge` hides the default badge shape so your custom content fills the area
- Wrap badge content in a `Viewbox` to ensure it scales correctly with the avatar size
- The `Stroke` on the `Ellipse` creates the white ring border between badge and avatar
- To bind status dynamically, bind `Fill` on the `Ellipse` to a color property in your ViewModel

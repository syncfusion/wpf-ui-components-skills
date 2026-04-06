# Layout Features

## Overview

This guide covers layout-related properties and features of the CheckListBox control, including checkbox alignment, orientation, dimensions, and scrolling behavior.

# Layout Features (summary)

This file contained a number of layout-focused examples that duplicated content already covered in `appearance-customization.md` and `virtualization.md`.

To reduce redundancy, detailed examples for checkbox placement, flow direction, sizing, padding/margin, borders, and full layout templates have been consolidated into `appearance-customization.md` and `virtualization.md`.

Keep this short summary as the single entry point for layout guidance; refer to the linked files for authoritative examples:

- Checkbox placement and RTL: [appearance-customization.md](appearance-customization.md)
- Item/Scroll/Virtualization guidance: [virtualization.md](virtualization.md)
- Selection and spacing quick tips: [checking-and-selection.md](checking-and-selection.md)

If you need to restore a specific code example, copy it from the linked file or request it be re-added here.
                                     ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                
                <!-- Item styling with spacing -->
                <syncfusion:CheckListBox.ItemContainerStyle>
                    <Style TargetType="syncfusion:CheckListBoxItem">
                        <Setter Property="Margin" Value="0,6" />
                        <Setter Property="Padding" Value="8" />
                        <Setter Property="FontSize" Value="14" />
                    </Style>
                </syncfusion:CheckListBox.ItemContainerStyle>
                
                <!-- Items -->
                <syncfusion:CheckListBoxItem Content="Task 1" />
                <syncfusion:CheckListBoxItem Content="Task 2" />
                <syncfusion:CheckListBoxItem Content="Task 3" />
                <syncfusion:CheckListBoxItem Content="Task 4" />
                <syncfusion:CheckListBoxItem Content="Task 5" />
            </syncfusion:CheckListBox>
        </Border>
    </Grid>
</Window>
```

## Troubleshooting

**Issue: Control not visible**
- Check if Width/Height are set to 0 or very small values
- Verify parent container has space allocated
- Check Visibility property isn't Collapsed

**Issue: Scrollbar not appearing**
- Set appropriate Height to constrain the control
- Verify ScrollViewer.VerticalScrollBarVisibility is not Disabled
- Ensure items exceed visible area

**Issue: Items cut off**
- Increase Width if using fixed width
- Set HorizontalScrollBarVisibility to Auto
- Check Padding and Margin values

**Issue: Too much spacing**
- Adjust Margin in ItemContainerStyle
- Reduce Padding on control
- Check for inherited margins from parent containers

## Next Steps

- **Customize Appearance:** [appearance-customization.md](appearance-customization.md)
- **Optimize Performance:** [virtualization.md](virtualization.md)
- **Handle Selection:** [checking-and-selection.md](checking-and-selection.md)

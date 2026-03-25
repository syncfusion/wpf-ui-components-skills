# Accessibility

This guide covers accessibility features in TreeMap, including keyboard navigation and WCAG compliance.

## Overview

TreeMap is designed to be accessible, supporting keyboard navigation and providing features for users who rely on assistive technologies.

## Keyboard Navigation

TreeMap supports keyboard shortcuts for navigation and interaction.

### Navigation Keys

| Key | Action |
|-----|--------|
| **Tab** | Move selection to the next item on the right |
| **Shift + Tab** | Move selection to the previous item on the left |

### Behavior with SelectionModes

#### Default SelectionMode

When `SelectionModes` is set to `Default`:
- Each key press **clears the previous selection**
- Only one item is selected at a time
- Tab moves forward, Shift+Tab moves backward
- Previously focused item loses selection when new item is selected

**Usage:**
```xaml
<syncfusion:SfTreeMap SelectionModes="Default"
                      HighlightOnSelection="True"
                      HighlightBorderBrush="Blue"
                      HighlightBorderThickness="2">
</syncfusion:SfTreeMap>
```

#### Multiple SelectionMode

When `SelectionModes` is set to `Multiple`:
- Previous selections are **preserved**
- Each key press adds to the selection
- Multiple items can be selected simultaneously
- Use Ctrl+Click to toggle individual items

**Usage:**
```xaml
<syncfusion:SfTreeMap SelectionModes="Multiple"
                      HighlightOnSelection="True"
                      HighlightBorderBrush="Blue"
                      HighlightBorderThickness="2">
</syncfusion:SfTreeMap>
```

### Keyboard Navigation Example

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                      WeightValuePath="Population"
                      ColorValuePath="Growth"
                      SelectionModes="Default"
                      HighlightOnSelection="True"
                      HighlightBorderBrush="#3498DB"
                      HighlightBorderThickness="3"
                      HighlightHoverBrush="#2ECC71">
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:RangeBrushColorMapping>
            <syncfusion:RangeBrushColorMapping.Brushes>
                <syncfusion:RangeBrush From="0" To="2" Color="#27AE60"/>
                <syncfusion:RangeBrush From="2" To="4" Color="#F39C12"/>
            </syncfusion:RangeBrushColorMapping.Brushes>
        </syncfusion:RangeBrushColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" GroupGap="10"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## WCAG Compliance

TreeMap is designed with Web Content Accessibility Guidelines (WCAG) in mind.

### Key Features for WCAG Compliance

1. **Keyboard Accessibility** - Full keyboard navigation without requiring mouse
2. **Visual Indicators** - Clear focus and selection indicators
3. **Color Contrast** - Configurable colors for sufficient contrast
4. **Predictable Behavior** - Consistent navigation patterns

### Ensuring Compliance

**Use High Contrast Colors:**
```xaml
<syncfusion:SfTreeMap HighlightBorderBrush="Black"
                      HighlightBorderThickness="3">
    <syncfusion:SfTreeMap.LeafItemSettings>
        <syncfusion:LeafItemSettings LeafBorderBrush="White"
                                     LeafBorderThickness="2"/>
    </syncfusion:SfTreeMap.LeafItemSettings>
</syncfusion:SfTreeMap>
```

**Provide Text Alternatives:**
```xaml
<syncfusion:SfTreeMap ShowToolTip="True">
    <syncfusion:SfTreeMap.LeafItemSettings>
        <syncfusion:LeafItemSettings LeafLabelPath="Name"/>
    </syncfusion:SfTreeMap.LeafItemSettings>
    <syncfusion:SfTreeMap.ToolTipTemplate>
        <DataTemplate>
            <StackPanel Background="White" Padding="10">
                <TextBlock Text="{Binding Data.Name}" FontWeight="Bold"/>
                <TextBlock Text="{Binding Data.Value, StringFormat='Value: {0}'}"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfTreeMap.ToolTipTemplate>
</syncfusion:SfTreeMap>
```

## Screen Reader Support

While TreeMap is a visual control, you can enhance screen reader compatibility:

### Adding Automation Properties

```xaml
<syncfusion:SfTreeMap AutomationProperties.Name="Sales Data TreeMap"
                      AutomationProperties.HelpText="TreeMap showing sales distribution by region"
                      ItemsSource="{Binding SalesData}">
</syncfusion:SfTreeMap>
```

### Item-Level Automation

```csharp
// In code-behind or ViewModel
private void SetAutomationProperties(TreeMapItem item, object dataItem)
{
    AutomationProperties.SetName(item, $"{dataItem.Name}: {dataItem.Value}");
    AutomationProperties.SetHelpText(item, $"Category: {dataItem.Category}");
}
```

## Best Practices for Accessibility

### 1. Enable Keyboard Navigation
Always enable selection highlighting to show keyboard focus:
```xaml
<syncfusion:SfTreeMap HighlightOnSelection="True"
                      HighlightBorderBrush="Black"
                      HighlightBorderThickness="3">
</syncfusion:SfTreeMap>
```

### 2. Use Sufficient Color Contrast
Ensure colors meet WCAG contrast ratios (4.5:1 for text):
```xaml
<syncfusion:SfTreeMap.LeafColorMapping>
    <syncfusion:RangeBrushColorMapping>
        <syncfusion:RangeBrushColorMapping.Brushes>
            <!-- Use colors with good contrast -->
            <syncfusion:RangeBrush From="0" To="50" Color="#1A5490"/>
            <syncfusion:RangeBrush From="50" To="100" Color="#B8312F"/>
        </syncfusion:RangeBrushColorMapping.Brushes>
    </syncfusion:RangeBrushColorMapping>
</syncfusion:SfTreeMap.LeafColorMapping>
```

### 3. Provide Text Labels
Always include text labels for important data:
```xaml
<syncfusion:SfTreeMap.LeafItemSettings>
    <syncfusion:LeafItemSettings LeafLabelPath="Name"
                                 LeafBorderBrush="Black"
                                 LeafBorderThickness="1"/>
</syncfusion:SfTreeMap.LeafItemSettings>
```

### 4. Use Clear Focus Indicators
Make selection state obvious:
```xaml
<syncfusion:SfTreeMap HighlightBorderBrush="#000000"
                      HighlightBorderThickness="4"
                      HighlightHoverBrush="#FFFF00">
</syncfusion:SfTreeMap>
```

### 5. Add Tooltips for Details
Provide additional information via tooltips:
```xaml
<syncfusion:SfTreeMap ShowToolTip="True">
    <syncfusion:SfTreeMap.ToolTipTemplate>
        <DataTemplate>
            <Border Background="White" 
                    BorderBrush="Black" 
                    BorderThickness="2" 
                    Padding="8">
                <StackPanel>
                    <TextBlock Text="{Binding Data.Name}" 
                              FontWeight="Bold" 
                              FontSize="14"/>
                    <TextBlock Text="{Binding Data.Details}" 
                              FontSize="12"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeMap.ToolTipTemplate>
</syncfusion:SfTreeMap>
```

## Complete Accessible Example

```xaml
<syncfusion:SfTreeMap ItemsSource="{Binding PopulationDetails}"
                      WeightValuePath="Population"
                      ColorValuePath="Growth"
                      SelectionModes="Default"
                      HighlightOnSelection="True"
                      HighlightBorderBrush="Black"
                      HighlightBorderThickness="4"
                      ShowToolTip="True"
                      AutomationProperties.Name="Population TreeMap"
                      AutomationProperties.HelpText="TreeMap showing world population by continent and country">
    
    <!-- High contrast color mapping -->
    <syncfusion:SfTreeMap.LeafColorMapping>
        <syncfusion:RangeBrushColorMapping>
            <syncfusion:RangeBrushColorMapping.Brushes>
                <syncfusion:RangeBrush From="0" To="2" Color="#2E7D32"/>
                <syncfusion:RangeBrush From="2" To="4" Color="#D84315"/>
            </syncfusion:RangeBrushColorMapping.Brushes>
        </syncfusion:RangeBrushColorMapping>
    </syncfusion:SfTreeMap.LeafColorMapping>
    
    <!-- Visible labels -->
    <syncfusion:SfTreeMap.LeafItemSettings>
        <syncfusion:LeafItemSettings LeafLabelPath="Country"
                                     LeafBorderBrush="White"
                                     LeafBorderThickness="2"/>
    </syncfusion:SfTreeMap.LeafItemSettings>
    
    <!-- Accessible tooltips -->
    <syncfusion:SfTreeMap.ToolTipTemplate>
        <DataTemplate>
            <Border Background="White" 
                    BorderBrush="Black" 
                    BorderThickness="2" 
                    Padding="10">
                <StackPanel>
                    <TextBlock Text="{Binding Data.Country}" 
                              FontWeight="Bold" 
                              FontSize="14"/>
                    <TextBlock Text="{Binding Data.Population, StringFormat='Population: {0:N0}'}" 
                              FontSize="12"/>
                    <TextBlock Text="{Binding Data.Growth, StringFormat='Growth: {0}%'}" 
                              FontSize="12"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </syncfusion:SfTreeMap.ToolTipTemplate>
    
    <!-- Levels -->
    <syncfusion:SfTreeMap.Levels>
        <syncfusion:TreeMapFlatLevel GroupPath="Continent" 
                                     GroupGap="10" 
                                     HeaderHeight="30"/>
    </syncfusion:SfTreeMap.Levels>
</syncfusion:SfTreeMap>
```

## Testing Accessibility

### Keyboard Navigation Test
1. Tab to the TreeMap control
2. Use Tab/Shift+Tab to navigate items
3. Verify visual focus indicator is clear
4. Ensure all items are reachable via keyboard

### Screen Reader Test
1. Enable screen reader (NVDA, JAWS, Narrator)
2. Navigate to TreeMap
3. Verify control announces its purpose
4. Check that item information is spoken clearly

### Color Contrast Test
1. Use contrast checking tools
2. Verify text has 4.5:1 contrast ratio
3. Check selection indicators are visible
4. Test in high contrast mode

## Troubleshooting

**Keyboard navigation not working:**
- Ensure TreeMap has focus (click it first)
- Verify HighlightOnSelection is True
- Check SelectionModes is set appropriately

**Focus indicator not visible:**
- Increase HighlightBorderThickness (minimum 2-3 pixels)
- Use high contrast color for HighlightBorderBrush
- Ensure border color contrasts with leaf colors

**Screen reader not announcing items:**
- Add AutomationProperties.Name to TreeMap
- Set AutomationProperties.HelpText for context
- Consider custom automation properties on items

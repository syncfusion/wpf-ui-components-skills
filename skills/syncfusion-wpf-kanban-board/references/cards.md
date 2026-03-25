# Cards in WPF Kanban

## Table of Contents
- [Card Overview](#card-overview)
- [Default Card Properties](#default-card-properties)
- [Indicator Colors](#indicator-colors)
- [Custom Card Template](#custom-card-template)
- [Conditional Card Templates](#conditional-card-templates)
- [Card Tooltips](#card-tooltips)
- [Troubleshooting](#troubleshooting)

## Card Overview

Kanban cards visually represent tasks and their progression through workflow stages. Each card is created from a data item in the `ItemsSource` collection.

**Two Approaches:**
1. **Default KanbanModel** - Built-in properties with automatic card UI
2. **Custom Model** - Your data model with custom CardTemplate (required)

## Default Card Properties

When using `KanbanModel`, these properties control the default card appearance:

### Core Properties

| Property | Type | Description | Display Location |
|----------|------|-------------|------------------|
| `Title` | string | Card heading | Top, bold text |
| `ID` | object | Unique identifier | Not displayed in default UI |
| `Description` | string | Detailed text | Below title |
| `Category` | object | Column assignment | Determines column placement |
| `Assignee` | string | Assigned person | Used for swim lane grouping |
| `ColorKey` | object | Priority/status key | Color bar on card |
| `Tags` | string[] | Labels/categories | Bottom of card |
| `ImageURL` | Uri | Avatar/icon | Right side of card |

### Example: Complete KanbanModel

```csharp
using Syncfusion.UI.Xaml.Kanban;

var task = new KanbanModel()
{
    Title = "UWP Issue",
    ID = "651",
    Description = "Crosshair label template not visible in UWP platform",
    Category = "Open",
    Assignee = "Janet Leverling",
    ColorKey = "High",
    Tags = new string[] { "Bug Fixing", "Critical" },
};
```

**Result:** Card displays with:
- Bold title "UWP Issue"
- Description text below
- High-priority color indicator (red if mapped)
- Tags "Bug Fixing" and "Critical" at bottom
- Avatar image on right
- Appears in "Open" column

### Property Details

#### Title
**Purpose:** Main card identifier  
**Displayed:** Top of card, bold font  
**Recommended:** Keep under 50 characters  

#### Id
**Purpose:** Unique identifier for tracking  
**Displayed:** Not shown in default UI (internal use)  
**Type:** Usually string or integer  

#### Description
**Purpose:** Detailed task information  
**Displayed:** Below title, smaller font, word-wrapped  
**Recommended:** 1-3 sentences  

#### Category
**Purpose:** Determines which column contains the card  
**Matching:** Must match a column's `Categories` property  
**Example:** If Category = "In Progress", card appears in column with Categories = "In Progress"

#### Assignee
**Purpose:** Person assigned to task  
**Default Use:** Swim lane grouping (SwimlaneKey defaults to "Assignee")  
**Displayed:** Not shown in default card UI (used for grouping)

#### ColorKey
**Purpose:** Visual priority/status indicator  
**Displayed:** Colored bar on right edge of card  
**Requires:** Mapping in `IndicatorColorPalette` property  

#### Tags
**Purpose:** Labels or categories  
**Type:** string[]
**Displayed:** Bottom of card as comma-separated values  
**Example:** ["Bug Fixing", "High Priority"] → "Bug Fixing, High Priority"

#### ImageURL
**Purpose:** Avatar or icon representation  
**Type:** Image control with Uri set  
**Displayed:** Right side of default card  
**Common Use:** Team member avatars, issue type icons

## Indicator Colors

Map `IndicatorColorKey` values to actual colors using `IndicatorColorPalette`.

### Defining Color Palette

**XAML:**
```xml
<kanban:SfKanban x:Name="kanban"
             ItemsSource="{Binding TaskDetails}">
    <kanban:SfKanban.IndicatorColorPalette>
        <kanban:ColorMapping Key="Low" Color="Blue"/>
        <kanban:ColorMapping Key="Normal" Color="Green"/>
        <kanban:ColorMapping Key="High" Color="Orange"/>
        <kanban:ColorMapping Key="Critical" Color="Red"/>
    </kanban:SfKanban.IndicatorColorPalette>
</kanban:SfKanban>
```

**C#:**
```csharp
IndicatorColorPalette indicatorColorPalette = new IndicatorColorPalette();
indicatorColorPalette.Add(new ColorMapping() { Key = "Low", Color = Colors.Blue });
indicatorColorPalette.Add(new ColorMapping() { Key = "Normal", Color = Colors.Green });
indicatorColorPalette.Add(new ColorMapping() { Key = "High", Color = Colors.Orange });
indicatorColorPalette.Add(new ColorMapping() { Key = "Critical", Color = Colors.Red });
kanban.IndicatorColorPalette = indicatorColorPalette;
```

### Using Indicator Colors

```csharp
// In your data
var task = new KanbanModel()
{
    ColorKey = "High",  // Matches "High" key in palette
                                 // ...other properties
};
```

**Result:** Card displays with orange indicator bar (as defined in palette)

### Best Practices

1. **Consistent Keys:** Use same key names across all cards (High, Normal, Low)
2. **Color Meaning:** Choose colors that convey meaning (red = urgent, green = normal)
3. **Accessibility:** Ensure sufficient contrast with card background
4. **Limit Palette:** 3-5 priority levels is optimal

## Custom Card Template

Replace default card UI with your own design using `CardTemplate`.

### When to Use

- Using custom data model (not KanbanModel)
- Need different visual design
- Want additional information displayed
- Require custom layout or styling

**Important:** CardTemplate DataContext is the bound data item (KanbanModel or your custom model)

### Basic Custom Template

```xml
<kanban:SfKanban x:Name="kanban"
                 ItemsSource="{Binding TaskDetails}">
    <kanban:SfKanban.CardTemplate>
        <DataTemplate>
            <Border BorderBrush="LightGray" 
                    BorderThickness="1" 
                    CornerRadius="8" 
                    Background="#F3F3F2"
                    Padding="12">
                <StackPanel>
                    <TextBlock Text="{Binding Title}" 
                               FontWeight="Bold" 
                               FontSize="14" 
                               Foreground="#333333"/>
                    <TextBlock Text="{Binding Description}" 
                               FontSize="12" 
                               TextWrapping="Wrap" 
                               Margin="0,8,0,0"
                               Foreground="#666666"/>
                </StackPanel>
            </Border>
        </DataTemplate>
    </kanban:SfKanban.CardTemplate>
</kanban:SfKanban>
```

### Advanced Custom Template with Priority

```xml
<kanban:SfKanban.CardTemplate>
    <DataTemplate>
        <Border Background="#F3CFCE" 
                BorderBrush="Black" 
                BorderThickness="1" 
                CornerRadius="8" 
                Padding="10">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                
                <!-- Priority indicator -->
                <StackPanel Grid.Row="0" 
                           Orientation="Horizontal" 
                           Spacing="4">
                    <TextBlock Text="•" 
                              FontSize="14" 
                              FontWeight="Bold" 
                              Foreground="Orange"/>
                    <TextBlock Text="{Binding IndicatorColorKey}" 
                              FontSize="12" 
                              FontWeight="Bold" 
                              Foreground="Orange"/>
                </StackPanel>
                
                <!-- Title -->
                <TextBlock Grid.Row="1" 
                          Text="{Binding Title}" 
                          FontWeight="Bold" 
                          FontSize="14" 
                          Margin="0,8,0,0"/>
                
                <!-- Description -->
                <TextBlock Grid.Row="2" 
                          Text="{Binding Description}" 
                          FontSize="12" 
                          TextWrapping="Wrap" 
                          Margin="0,4,0,0"/>
            </Grid>
        </Border>
    </DataTemplate>
</kanban:SfKanban.CardTemplate>
```

### Custom Template with Image and Tags

```xml
<kanban:SfKanban.CardTemplate>
    <DataTemplate>
        <Border BorderBrush="Gray" 
                BorderThickness="1" 
                CornerRadius="4" 
                Background="White"
                Padding="10">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                
                <!-- Header with image -->
                <Grid Grid.Row="0">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="*"/>
                        <ColumnDefinition Width="Auto"/>
                    </Grid.ColumnDefinitions>
                    
                    <TextBlock Text="{Binding Title}" 
                              FontWeight="Bold" 
                              VerticalAlignment="Center"/>
                    
                    <Border Grid.Column="1" 
                           Width="30" 
                           Height="30" 
                           CornerRadius="15">
                        <ContentControl Content="{Binding Image}"/>
                    </Border>
                </Grid>
                
                <!-- Description -->
                <TextBlock Grid.Row="1" 
                          Text="{Binding Description}" 
                          TextWrapping="Wrap" 
                          Margin="0,8,0,0"/>
                
                <!-- Tags -->
                <ItemsControl Grid.Row="2" 
                             ItemsSource="{Binding Tags}"
                             Margin="0,8,0,0">
                    <ItemsControl.ItemsPanel>
                        <ItemsPanelTemplate>
                            <StackPanel Orientation="Horizontal" />
                        </ItemsPanelTemplate>
                    </ItemsControl.ItemsPanel>
                    <ItemsControl.ItemTemplate>
                        <DataTemplate>
                            <Border Background="LightBlue" 
                                   CornerRadius="3" 
                                   Padding="4,2">
                                <TextBlock Text="{Binding}" FontSize="10"/>
                            </Border>
                        </DataTemplate>
                    </ItemsControl.ItemTemplate>
                </ItemsControl>
            </Grid>
        </Border>
    </DataTemplate>
</kanban:SfKanban.CardTemplate>
```

## Conditional Card Templates

Use `CardTemplateSelector` to apply different templates based on card data.

### When to Use

- Different card styles for different priorities
- Special appearance for specific card types
- Conditional formatting based on data values
- Highlighting urgent or overdue tasks

### Step 1: Create Template Selector Class

```csharp
using Syncfusion.UI.Xaml.Kanban;

public class KanbanCardTemplateSelector : DataTemplateSelector
{
    public DataTemplate HighPriorityTemplate { get; set; }
    public DataTemplate NormalPriorityTemplate { get; set; }
    public DataTemplate LowPriorityTemplate { get; set; }

   public override DataTemplate SelectTemplate(object item, DependencyObject container)
   {
    var task = item as KanbanModel;
    if (task != null)
    {
        if (task.ColorKey?.ToString() == "High")
            return HighPriorityTemplate;
        else if (task.ColorKey?.ToString() == "Low")
            return LowPriorityTemplate;
    }

    return NormalPriorityTemplate;
  }
}
```

### Step 2: Define Templates in Resources

```xml
<Grid>
    <Grid.Resources>
        <!-- High Priority Template -->
        <DataTemplate x:Key="highPriorityTemplate">
            <Border BorderBrush="Red" 
                    BorderThickness="2" 
                    CornerRadius="4" 
                    Background="#FFEBEE">
                <StackPanel Margin="10">
                    <TextBlock Text="🔥" FontSize="16"/>
                    <TextBlock Text="{Binding Title}" 
                              FontWeight="Bold" 
                              FontSize="14" 
                              Foreground="Red"/>
                    <TextBlock Text="{Binding Description}" 
                              FontSize="12" 
                              TextWrapping="Wrap" 
                              Margin="0,5,0,0"/>
                </StackPanel>
            </Border>
        </DataTemplate>
        
        <!-- Normal Priority Template -->
        <DataTemplate x:Key="normalTemplate">
            <Border BorderBrush="Gray" 
                    BorderThickness="1" 
                    CornerRadius="4" 
                    Background="#FAFAFA">
                <StackPanel Margin="10">
                    <TextBlock Text="{Binding Title}" 
                              FontWeight="Bold" 
                              FontSize="14"/>
                    <TextBlock Text="{Binding Description}" 
                              FontSize="12" 
                              TextWrapping="Wrap" 
                              Margin="0,5,0,0"/>
                </StackPanel>
            </Border>
        </DataTemplate>
        
        <!-- Low Priority Template -->
        <DataTemplate x:Key="lowPriorityTemplate">
            <Border BorderBrush="Green" 
                    BorderThickness="1" 
                    CornerRadius="4" 
                    Background="#F1F8E9">
                <StackPanel Margin="10">
                    <TextBlock Text="{Binding Title}" 
                              FontWeight="Normal" 
                              FontSize="13"/>
                    <TextBlock Text="{Binding Description}" 
                              FontSize="11" 
                              TextWrapping="Wrap" 
                              Margin="0,5,0,0"
                              Foreground="Gray"/>
                </StackPanel>
            </Border>
        </DataTemplate>
        
        <!-- Template Selector -->
        <local:KanbanCardTemplateSelector x:Key="cardTemplateSelector"
                                          HighPriorityTemplate="{StaticResource highPriorityTemplate}"
                                          NormalPriorityTemplate="{StaticResource normalTemplate}"
                                          LowPriorityTemplate="{StaticResource lowPriorityTemplate}"/>
    </Grid.Resources>

    <kanban:SfKanban x:Name="kanban" 
                     CardTemplateSelector="{StaticResource cardTemplateSelector}"
                     ItemsSource="{Binding TaskDetails}"/>
</Grid>
```

**Result:** 
- High priority cards have red border and background with fire emoji
- Normal priority cards have standard gray appearance
- Low priority cards have green tint with lighter text

## Card Tooltips

Display additional card information on hover.

### Enable Tooltips

```xml
<kanban:SfKanban x:Name="kanban"
                 IsToolTipEnabled="True" 
                 ItemsSource="{Binding TaskDetails}"/>
```

**C#:**
```csharp
kanban.IsToolTipEnabled = true;
```

**Default Tooltip:** Shows Title, Category, and Description from KanbanModel

### Custom Tooltip Template

```xml
<kanban:SfKanban x:Name="kanban"
                 IsToolTipEnabled="True"
                 ItemsSource="{Binding TaskDetails}">
    <kanban:SfKanban.ToolTipTemplate>
        <DataTemplate>
            <Border Background="Black" 
                    CornerRadius="4" 
                    Padding="8">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto"/>
                        <ColumnDefinition Width="*"/>
                    </Grid.ColumnDefinitions>
                    
                    <!-- Title -->
                    <TextBlock Grid.Row="0" Grid.Column="0"
                              Text="Title:" 
                              FontSize="11" 
                              Foreground="White"
                              Margin="0,0,8,4"/>
                    <TextBlock Grid.Row="0" Grid.Column="1"
                              Text="{Binding Title}" 
                              FontSize="11" 
                              Foreground="White"
                              Margin="0,0,0,4"/>
                    
                    <!-- Status -->
                    <TextBlock Grid.Row="1" Grid.Column="0"
                              Text="Status:" 
                              FontSize="11" 
                              Foreground="White"
                              Margin="0,0,8,4"/>
                    <TextBlock Grid.Row="1" Grid.Column="1"
                              Text="{Binding Category}" 
                              FontSize="11" 
                              Foreground="White"
                              Margin="0,0,0,4"/>
                    
                    <!-- Priority -->
                    <TextBlock Grid.Row="2" Grid.Column="0"
                              Text="Priority:" 
                              FontSize="11" 
                              Foreground="White"
                              Margin="0,0,8,4"/>
                    <TextBlock Grid.Row="2" Grid.Column="1"
                              Text="{Binding IndicatorColorKey}" 
                              FontSize="11" 
                              Foreground="White"
                              Margin="0,0,0,4"/>
                    
                    <!-- Description -->
                    <TextBlock Grid.Row="3" Grid.Column="0"
                              Text="Description:" 
                              FontSize="11" 
                              Foreground="White"
                              Margin="0,0,8,0"/>
                    <TextBlock Grid.Row="3" Grid.Column="1"
                              Text="{Binding Description}" 
                              FontSize="11" 
                              Foreground="White"
                              TextWrapping="Wrap"
                              MaxWidth="200"/>
                </Grid>
            </Border>
        </DataTemplate>
    </kanban:SfKanban.ToolTipTemplate>
</kanban:SfKanban>
```

**DataContext:** KanbanModel or your custom model instance

**Note:** `ToolTipTemplate` only applies when `IsToolTipEnabled="True"`

## Troubleshooting

### Cards Not Displaying

**Problem:** Columns show but no cards visible

**Solutions:**
1. Check ItemsSource is bound and contains data
2. Verify Category values match column Categories property
3. If using custom model, ensure CardTemplate is defined
4. Check ColumnMappingPath matches your model property

### Indicator Colors Not Showing

**Problem:** Color indicator missing from cards

**Solutions:**
1. Verify IndicatorColorPalette is defined
2. Check IndicatorColorKey values match palette Key values (case-sensitive)
3. Ensure KanbanColorMapping entries are added to palette
4. Verify cards have IndicatorColorKey property set

### Images Not Displaying

**Problem:** Card images/avatars not visible

**Solutions:**
1. Check image URI is correct
2. Verify image files are added to project with "Content" build action
3. Ensure Image property is set correctly on KanbanModel
4. Check image path case (WPF is case-sensitive)

### Custom Template Not Applied

**Problem:** CardTemplate not taking effect

**Solutions:**
1. Verify CardTemplate is defined in XAML
2. Check DataTemplate bindings match model properties
3. Ensure x:Key is not set on DataTemplate (CardTemplate doesn't use key)
4. Test with simple template first to isolate binding issues

### Tags Not Displaying

**Problem:** Tags list not showing on cards

**Solutions:**
1. Verify Tags property is string[] type
2. Check Tags collection contains values
3. Ensure default card UI is being used (not custom template without tags)
4. Verify Tags binding in custom template if using CardTemplate

### Tooltip Not Showing

**Problem:** Hover doesn't display tooltip

**Solutions:**
1. Check IsToolTipEnabled="True" is set
2. Verify mouse/pointer input is working
3. Ensure ToolTipTemplate DataContext matches model
4. Check if custom template has binding errors

## Best Practices

1. **Use KanbanModel for Quick Start:** Default card UI is ready to use
2. **Keep Titles Concise:** 3-7 words optimal for readability
3. **Limit Tags:** 2-4 tags per card prevents clutter
4. **Meaningful Colors:** Use indicator colors that convey priority/status
5. **Responsive Templates:** Design cards that work at different widths
6. **Test Custom Templates:** Verify binding for all properties
7. **Enable Tooltips:** Provide additional context without cluttering card
8. **Consistent Styling:** Use same visual patterns across all cards
9. **Image Size:** Keep avatar images small (30x30 to 50x50 pixels)
10. **Accessibility:** Ensure sufficient color contrast and font sizes
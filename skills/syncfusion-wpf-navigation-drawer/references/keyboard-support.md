# Keyboard Support in WPF Navigation Drawer

Complete guide for keyboard navigation and accessibility in the Navigation Drawer.

## Overview

The Navigation Drawer provides full keyboard support for accessibility, enabling users to navigate and interact without a mouse.

### Supported Keys

| Key | Action |
|-----|--------|
| **Tab** | Navigate to the next item in the drawer |
| **Shift + Tab** | Navigate to the previous item in the drawer |
| **Up Arrow** | Move focus to the previous item |
| **Down Arrow** | Move focus to the next item |
| **Enter** | Select the focused item |
| **Space** | Select the focused item |
| **Left Arrow** | Collapse the expanded sub-items of the focused parent item |
| **Right Arrow** | Expand the sub-items of the focused parent item |

## Tab Navigation

### Basic Tab Order

Users can press **Tab** to move focus through navigation items sequentially.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M42.128046,6.7269681..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Drafts">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M6.9999996,48.353..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Spam">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M13.999999,3.9500002..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
</syncfusion:SfNavigationDrawer>
```

**Tab Navigation Flow:**
1. Press **Tab** → Focus moves to "Inbox"
2. Press **Tab** → Focus moves to "Sent mail"
3. Press **Tab** → Focus moves to "Drafts"
4. Press **Tab** → Focus moves to "Spam"
5. Press **Shift + Tab** → Focus moves back to "Drafts"

### Tab Navigation with Custom TabIndex

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <syncfusion:NavigationItem Header="Dashboard" TabIndex="1">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Reports" TabIndex="3">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M42.128046,6.7269681..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Analytics" TabIndex="2">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M6.9999996,48.353..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
</syncfusion:SfNavigationDrawer>
```

**Navigation Order:** Dashboard → Analytics → Reports

## Arrow Key Navigation

### Up/Down Arrow Keys

Navigate between items without following tab order.

```csharp
// No special code required - built-in behavior
// Users can press Up/Down arrows to navigate through visible items
```

**Behavior:**
- **Down Arrow:** Move focus to the next visible item
- **Up Arrow:** Move focus to the previous visible item
- Works in all display modes (Default, Compact, Expanded)
- Skips collapsed sub-items automatically

### Left/Right Arrow Keys for Hierarchy

Expand and collapse parent items with arrow keys.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <syncfusion:NavigationItem Header="Email">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
        
        <!-- Sub-items -->
        <syncfusion:NavigationItem.Items>
            <syncfusion:NavigationItem Header="Inbox"/>
            <syncfusion:NavigationItem Header="Sent mail"/>
            <syncfusion:NavigationItem Header="Drafts"/>
        </syncfusion:NavigationItem.Items>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Calendar">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M42.128046,6.7269681..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
        
        <syncfusion:NavigationItem.Items>
            <syncfusion:NavigationItem Header="Today"/>
            <syncfusion:NavigationItem Header="This Week"/>
            <syncfusion:NavigationItem Header="This Month"/>
        </syncfusion:NavigationItem.Items>
    </syncfusion:NavigationItem>
</syncfusion:SfNavigationDrawer>
```

**Arrow Key Behavior:**
- Focus on "Email" parent → Press **Right Arrow** → Expands sub-items
- Sub-items expanded → Press **Left Arrow** → Collapses sub-items
- Focus on sub-item → Press **Left Arrow** → Moves focus to parent and collapses

## Enter and Space Key Selection

### Selecting Items

Both **Enter** and **Space** keys select the focused item.

```xaml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:SfNavigationDrawer DisplayMode="Expanded"
                               ItemsSource="{Binding Items}"
                               SelectedItem="{Binding SelectedItem, Mode=TwoWay}"
                               DisplayMemberPath="Title"
                               IconMemberPath="Icon"
                               ItemClicked="NavigationDrawer_ItemClicked"/>
```

**Code-behind:**
```csharp
private void NavigationDrawer_ItemClicked(object sender, NavigationItemClickedEventArgs e)
{
    var item = e.Item as NavigationModel;
    if (item != null)
    {
        MessageBox.Show($"Selected via keyboard: {item.Title}");
    }
}
```

**Behavior:**
1. Use **Tab** or **Arrow** keys to focus on an item
2. Press **Enter** or **Space** to select the item
3. `ItemClicked` event fires
4. `SelectedItem` property updates
5. Item receives selection background

## Focus Management

### Programmatically Setting Focus

```csharp
// Set focus to the navigation drawer
navigationDrawer.Focus();

// Set focus to a specific item
NavigationItem targetItem = navigationDrawer.Items[0] as NavigationItem;
if (targetItem != null)
{
    // Use keyboard to navigate to specific item
    targetItem.Focus();
}
```

### Focus Visual Styles

Customize the focus indicator appearance:

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded">
    <syncfusion:SfNavigationDrawer.Resources>
        <Style TargetType="syncfusion:NavigationItem">
            <Setter Property="FocusVisualStyle">
                <Setter.Value>
                    <Style>
                        <Setter Property="Control.Template">
                            <Setter.Value>
                                <ControlTemplate>
                                    <Rectangle Stroke="#FF6200EE" 
                                             StrokeThickness="2"
                                             StrokeDashArray="1 2"
                                             SnapsToDevicePixels="True"/>
                                </ControlTemplate>
                            </Setter.Value>
                        </Setter>
                    </Style>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfNavigationDrawer.Resources>
    
    <syncfusion:NavigationItem Header="Dashboard">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Reports">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M42.128046,6.7269681..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
</syncfusion:SfNavigationDrawer>
```

### Removing Focus Visual

```xaml
<Style TargetType="syncfusion:NavigationItem">
    <Setter Property="FocusVisualStyle" Value="{x:Null}"/>
</Style>
```

## Accessibility Features

### ARIA Support

Navigation Drawer implements proper ARIA attributes automatically:

- **AutomationProperties.Name:** Set to item Header value
- **AutomationProperties.Role:** Set to "MenuItem" or "TreeItem"
- **AutomationProperties.ExpandedState:** Reflects sub-item expansion state

### Enhancing Accessibility

```xaml
<syncfusion:NavigationItem Header="Inbox"
                           AutomationProperties.Name="Inbox - 5 unread messages"
                           AutomationProperties.HelpText="View your inbox messages">
    <syncfusion:NavigationItem.Icon>
        <Path Data="M32.032381,16.445429..." Fill="White"/>
    </syncfusion:NavigationItem.Icon>
</syncfusion:NavigationItem>

<syncfusion:NavigationItem Header="Settings"
                           AutomationProperties.Name="Application Settings"
                           AutomationProperties.HelpText="Configure application preferences">
    <syncfusion:NavigationItem.Icon>
        <Path Data="M13.999999,3.9500002..." Fill="White"/>
    </syncfusion:NavigationItem.Icon>
</syncfusion:NavigationItem>
```

### Screen Reader Support

```csharp
// Set AutomationProperties in code-behind
NavigationItem item = new NavigationItem()
{
    Header = "Dashboard"
};

AutomationProperties.SetName(item, "Dashboard - Main overview");
AutomationProperties.SetHelpText(item, "Navigate to the main dashboard");
AutomationProperties.SetLabeledBy(item, labelElement);

navigationDrawer.Items.Add(item);
```

## Keyboard Shortcuts in Content

### Handling Keyboard Events

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer"
                               DisplayMode="Expanded"
                               PreviewKeyDown="NavigationDrawer_PreviewKeyDown">
    <syncfusion:NavigationItem Header="Dashboard">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M32.032381,16.445429..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid Background="White">
            <TextBlock Text="Press Ctrl+D for Dashboard" 
                       HorizontalAlignment="Center"
                       VerticalAlignment="Center"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void NavigationDrawer_PreviewKeyDown(object sender, KeyEventArgs e)
{
    // Custom keyboard shortcuts
    if (Keyboard.Modifiers == ModifierKeys.Control)
    {
        switch (e.Key)
        {
            case Key.D:
                // Navigate to Dashboard
                navigationDrawer.SelectedItem = navigationDrawer.Items[0];
                e.Handled = true;
                break;
                
            case Key.R:
                // Navigate to Reports
                navigationDrawer.SelectedItem = navigationDrawer.Items[1];
                e.Handled = true;
                break;
                
            case Key.S:
                // Navigate to Settings
                navigationDrawer.SelectedItem = navigationDrawer.Items[2];
                e.Handled = true;
                break;
        }
    }
    
    // Toggle drawer with Ctrl+B
    if (Keyboard.Modifiers == ModifierKeys.Control && e.Key == Key.B)
    {
        navigationDrawer.ToggleDrawer();
        e.Handled = true;
    }
}
```

## Complete Keyboard Navigation Example

```xaml
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>

<Grid>
    <syncfusion:SfNavigationDrawer x:Name="navigationDrawer"
                                   DisplayMode="Expanded"
                                   ItemsSource="{Binding NavigationItems}"
                                   SelectedItem="{Binding SelectedItem, Mode=TwoWay}"
                                   DisplayMemberPath="Title"
                                   IconMemberPath="Icon"
                                   PreviewKeyDown="NavigationDrawer_PreviewKeyDown">
        <syncfusion:SfNavigationDrawer.ContentView>
            <Grid Background="White">
                <StackPanel VerticalAlignment="Center" 
                           HorizontalAlignment="Center">
                    <TextBlock Text="{Binding SelectedItem.Title}" 
                              FontSize="24" 
                              FontWeight="Bold"/>
                    <TextBlock Text="Keyboard Shortcuts:" 
                              Margin="0,20,0,10" 
                              FontWeight="SemiBold"/>
                    <TextBlock Text="Tab - Next item"/>
                    <TextBlock Text="Shift+Tab - Previous item"/>
                    <TextBlock Text="Up/Down - Navigate items"/>
                    <TextBlock Text="Left/Right - Collapse/Expand"/>
                    <TextBlock Text="Enter/Space - Select item"/>
                    <TextBlock Text="Ctrl+B - Toggle drawer"/>
                    <TextBlock Text="Ctrl+1-5 - Quick navigation"/>
                </StackPanel>
            </Grid>
        </syncfusion:SfNavigationDrawer.ContentView>
    </syncfusion:SfNavigationDrawer>
</Grid>
```

**Code-behind:**
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        // Set initial focus
        this.Loaded += (s, e) => navigationDrawer.Focus();
    }
    
    private void NavigationDrawer_PreviewKeyDown(object sender, KeyEventArgs e)
    {
        var viewModel = this.DataContext as MainViewModel;
        if (viewModel == null) return;
        
        // Quick navigation with Ctrl+Number
        if (Keyboard.Modifiers == ModifierKeys.Control)
        {
            int index = -1;
            
            switch (e.Key)
            {
                case Key.D1: index = 0; break;
                case Key.D2: index = 1; break;
                case Key.D3: index = 2; break;
                case Key.D4: index = 3; break;
                case Key.D5: index = 4; break;
                case Key.B:
                    navigationDrawer.ToggleDrawer();
                    e.Handled = true;
                    return;
            }
            
            if (index >= 0 && index < viewModel.NavigationItems.Count)
            {
                viewModel.SelectedItem = viewModel.NavigationItems[index];
                e.Handled = true;
            }
        }
    }
}
```

## Best Practices

1. **Always Test with Keyboard:** Verify all functionality is accessible without a mouse
2. **Visible Focus Indicators:** Ensure focus is clearly visible for accessibility
3. **Logical Tab Order:** Maintain intuitive navigation flow
4. **Custom Shortcuts:** Document keyboard shortcuts in your UI
5. **Screen Reader Testing:** Test with NVDA or Windows Narrator
6. **AutomationProperties:** Set meaningful names and help text for screen readers
7. **Don't Override Standard Keys:** Avoid breaking expected Tab/Arrow behavior

## Accessibility Compliance

The Navigation Drawer keyboard support helps meet:
- **WCAG 2.1 Level AA:** Keyboard accessibility requirements
- **Section 508:** Federal accessibility standards
- **ADA Compliance:** Americans with Disabilities Act guidelines

## Sample Code

View complete sample on GitHub: [Keyboard Support Sample](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Keyboard_Support)

---
name: syncfusion-wpf-navigation-drawer
description: Implements the Syncfusion WPF SfNavigationDrawer for slide-out navigation panels and sidebars. Use when building collapsible side panels, hierarchical navigation menus, or responsive drawer display modes (compact/expanded).
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF Navigation Drawer
The WPF Navigation Drawer control is a sidebar navigation component used to create intuitive and efficient navigation menus within applications. It supports compact and expanded display modes, allowing users to switch between them dynamically based on the available screen size. Additionally, the control offers a default mode that enables developers to define a fully custom pane view, providing greater flexibility in designing navigation experiences

## When to Use This Skill

Use this skill when user need to:
- Create sidebar navigation menus in WPF applications
- Implement collapsible drawer navigation with hamburger menu
- Build responsive navigation with compact and expanded modes
- Add hierarchical navigation menu items with icons
- Implement swipeable side panels
- Create custom navigation views with headers and footers
- Handle navigation events and selection
- Support keyboard navigation in drawer menus
- Build modern UI navigation patterns in WPF

## Component Overview

The **Syncfusion WPF Navigation Drawer (SfNavigationDrawer)** is a sidebar navigation control that provides:

- **Three Display Modes:** Default (collapsible), Compact (icon-only), Expanded (full width)
- **Flexible Positioning:** Left, Right, Top, or Bottom placement
- **Built-in Navigation Items:** Tab, Button, Header, Separator types with hierarchical support
- **Custom Views:** Fully customizable header, content, and footer sections
- **Data Binding:** Support for ItemsSource with hierarchical data binding
- **Rich Events:** Opening, Opened, Closing, Closed, ItemClicked, ItemExpanded, ItemCollapsed
- **Keyboard Support:** Full keyboard navigation (Tab, Arrow keys, Enter, Space)
- **Smooth Animations:** SlideOnTop, Push, and Reveal transition effects
- **Responsive Design:** Auto-switching between display modes based on available width

**Use Cases:**
- Email client navigation (Inbox, Sent, Drafts with nested categories)
- E-commerce app menus (Products, Cart, Orders with subcategories)
- Banking app navigation (Accounts, Transfers, Payments)
- Admin dashboards with multi-level navigation
- Mobile-style navigation in desktop WPF apps

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
**Use when:** First-time implementation, setting up the control, understanding basic structure
- Installation and assembly deployment
- Creating basic NavigationDrawer in XAML and C#
- Adding content view and sidebar menu items
- Setting up navigation items with icons
- Minimal working example

### Display Modes
📄 **Read:** [references/display-modes.md](references/display-modes.md)
**Use when:** Choosing between Default, Compact, or Expanded modes, implementing responsive navigation
- Default mode (collapsible drawer with custom views)
- Compact mode (narrow icon bar that expands on toggle)
- Expanded mode (full-width sidebar)
- Auto display mode switching based on window width
- CompactModeWidth and ExpandedModeWidth configuration
- ExpandedModeThresholdWidth for responsive behavior

### Populating Data
📄 **Read:** [references/populating-data.md](references/populating-data.md)
**Use when:** Adding menu items, binding data sources, creating hierarchical navigation
- Built-in NavigationItem properties (Header, Icon, Items)
- ItemsSource data binding
- Hierarchical data binding with nested items
- IconTemplate and ItemTemplate customization
- Different item types: Tab, Button, Header, Separator
- DisplayMemberPath and IconMemberPath
- IndentationWidth for sub-items
- Popup support for sub-items

### Custom Views and Customization
📄 **Read:** [references/custom-views.md](references/custom-views.md)
**Use when:** Creating custom drawer layouts, positioning drawer, adding animations
- DrawerHeaderView, DrawerContentView, DrawerFooterView
- DrawerWidth and DrawerHeight customization
- Position property (Left, Right, Top, Bottom)
- Swipe gestures and TouchThreshold
- Transition animations (SlideOnTop, Push, Reveal)
- AnimationDuration configuration
- DrawerBackground customization
- IsOpen property for programmatic control
- ToggleDrawer() method

### Header and Footer Customization
📄 **Read:** [references/header-and-footer.md](references/header-and-footer.md)
**Use when:** Customizing toggle button, header, or footer sections
- ToggleButtonContentTemplate and ToggleButtonIconTemplate
- IsToggleButtonVisible property
- FooterItems collection
- HeaderHeight and FooterHeight properties
- Custom header and footer views in Default mode

### Commands and Events
📄 **Read:** [references/commands-and-events.md](references/commands-and-events.md)
**Use when:** Handling drawer state changes, item clicks, navigation events
- Opening and Opened events (with Cancel support)
- Closing and Closed events (with Cancel support)
- ItemClicked event (Compact/Expanded modes only)
- ItemExpanded and ItemCollapsed events
- Command binding with Command and CommandParameter
- DelegateCommand implementation example

### Selection Handling
📄 **Read:** [references/handling-selection.md](references/handling-selection.md)
**Use when:** Managing selected items, tracking navigation state
- SelectedItem property usage
- Selection with NavigationItem
- Selection with custom data objects
- SelectionBackground customization

### Keyboard Support
📄 **Read:** [references/keyboard-support.md](references/keyboard-support.md)
**Use when:** Implementing keyboard navigation, ensuring accessibility
- Tab key navigation between elements
- Up/Down arrow keys for item navigation
- Enter and Space for item selection
- Focus management

## Quick Start Example

### Basic Navigation Drawer with Items

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <syncfusion:SfNavigationDrawer x:Name="navigationDrawer" 
                                   DisplayMode="Expanded">
        <!-- Navigation Items -->
        <syncfusion:NavigationItem Header="Inbox" IsSelected="True">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M32.032381,16.445429 L25.410999..." Fill="White"/>
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
        
        <!-- Main Content -->
        <syncfusion:SfNavigationDrawer.ContentView>
            <Grid>
                <Label Content="Content View" 
                       HorizontalAlignment="Center"
                       VerticalAlignment="Center"/>
            </Grid>
        </syncfusion:SfNavigationDrawer.ContentView>
    </syncfusion:SfNavigationDrawer>
</Window>
```

### Code-behind (C#)

```csharp
using Syncfusion.UI.Xaml.NavigationDrawer;

// Create navigation drawer
SfNavigationDrawer navigationDrawer = new SfNavigationDrawer();
navigationDrawer.DisplayMode = DisplayMode.Expanded;

// Add items
navigationDrawer.Items.Add(new NavigationItem()
{
    Header = "Inbox",
    Icon = new Path() { Data = Geometry.Parse("M32.032381...") },
    IsSelected = true
});

navigationDrawer.Items.Add(new NavigationItem()
{
    Header = "Sent mail",
    Icon = new Path() { Data = Geometry.Parse("M42.128046...") }
});

// Set content
Label label = new Label { Content = "Content View" };
navigationDrawer.ContentView = label;
```

## Common Patterns

### Pattern 1: Responsive Navigation (Auto Mode Switching)

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Expanded"
                               AutoChangeDisplayMode="True"
                               ExpandedModeThresholdWidth="700">
    <!-- Items -->
</syncfusion:SfNavigationDrawer>
```
**When to use:** Desktop apps that need to adapt to window resizing

### Pattern 2: Hierarchical Navigation with Sub-items

```xaml
<syncfusion:NavigationItem Header="Inbox" IsExpanded="True">
    <syncfusion:NavigationItem.Icon>
        <Path Data="..." Fill="White"/>
    </syncfusion:NavigationItem.Icon>
    <!-- Sub-items -->
    <syncfusion:NavigationItem Header="Primary">
        <syncfusion:NavigationItem.Icon>
            <Path Data="..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    <syncfusion:NavigationItem Header="Social">
        <syncfusion:NavigationItem.Icon>
            <Path Data="..." Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
</syncfusion:NavigationItem>
```
**When to use:** Multi-level navigation menus (e.g., email categories, product categories)

### Pattern 3: Custom Drawer with Header and Footer

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Default" DrawerWidth="300">
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9">
            <Label Content="User Profile" />
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
    
    <syncfusion:SfNavigationDrawer.DrawerContentView>
        <ListBox ItemsSource="{Binding MenuItems}"/>
    </syncfusion:SfNavigationDrawer.DrawerContentView>
    
    <syncfusion:SfNavigationDrawer.DrawerFooterView>
        <Button Content="Logout" />
    </syncfusion:SfNavigationDrawer.DrawerFooterView>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid><!-- Main content --></Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```
**When to use:** Apps needing custom drawer layouts beyond built-in items

### Pattern 4: Data Binding with ItemsSource

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               ItemsSource="{Binding NavigationItems}"
                               DisplayMemberPath="Title"
                               IconMemberPath="Icon">
</syncfusion:SfNavigationDrawer>
```
**When to use:** Dynamic navigation menus from database or API

## Key Properties

### Essential Properties

| Property | Type | Description |
|----------|------|-------------|
| `DisplayMode` | DisplayMode | Default, Compact, or Expanded |
| `Items` | NavigationItemsCollection | Collection of NavigationItem objects |
| `ContentView` | object | Main content area |
| `IsOpen` | bool | Gets/sets drawer open state |
| `SelectedItem` | object | Currently selected navigation item |
| `Position` | Position | Left, Right, Top, Bottom (Default mode) |

### Display Mode Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AutoChangeDisplayMode` | bool | false | Auto-switch modes based on width |
| `ExpandedModeThresholdWidth` | double | 1007 | Width threshold for auto mode switching |
| `CompactModeWidth` | double | 48 | Width in compact mode |
| `ExpandedModeWidth` | double | 320 | Width in expanded mode |

### Customization Properties

| Property | Type | Description |
|----------|------|-------------|
| `DrawerWidth` | double | Drawer width (Default mode, Left/Right) |
| `DrawerHeight` | double | Drawer height (Default mode, Top/Bottom) |
| `DrawerBackground` | Brush | Background color of drawer |
| `HeaderHeight` | double | Height of header section |
| `FooterHeight` | double | Height of footer section |
| `IndentationWidth` | double | Horizontal spacing for sub-items |

### Animation Properties

| Property | Type | Options |
|----------|------|---------|
| `Transition` | Transition | SlideOnTop, Push, Reveal |
| `AnimationDuration` | TimeSpan | Duration of open/close animation |
| `EnableSwipeGesture` | bool | Enable swipe to open/close |
| `TouchThreshold` | double | Swipe sensitivity (pixels) |

### Template Properties

| Property | Type | Description |
|----------|------|-------------|
| `ToggleButtonContentTemplate` | DataTemplate | Customize toggle button content |
| `ToggleButtonIconTemplate` | DataTemplate | Customize toggle button icon |
| `ItemTemplate` | DataTemplate | Template for navigation items |
| `ItemTemplateSelector` | DataTemplateSelector | Dynamic template selection |

## Common Use Cases

### Use Case 1: Email Client Navigation
**Scenario:** Multi-level inbox with Primary, Social, Promotions
**Solution:** Use hierarchical NavigationItem with IsExpanded, Compact/Expanded modes
**Reference:** [references/populating-data.md](references/populating-data.md)

### Use Case 2: Admin Dashboard
**Scenario:** Top-level menu (Dashboard, Users, Settings) with sub-sections
**Solution:** Data binding with ItemsSource, hierarchical data structure
**Reference:** [references/populating-data.md](references/populating-data.md)

### Use Case 3: Mobile-Style App
**Scenario:** Hamburger menu that slides over content
**Solution:** Default mode with DrawerHeaderView, swipe gestures enabled
**Reference:** [references/custom-views.md](references/custom-views.md)

### Use Case 4: Responsive Desktop App
**Scenario:** Auto-collapse to compact mode on narrow windows
**Solution:** Expanded mode with AutoChangeDisplayMode=true
**Reference:** [references/display-modes.md](references/display-modes.md)

### Use Case 5: Banking App
**Scenario:** Accounts, Transfers, Payments with event handling
**Solution:** ItemClicked event, Command binding for navigation
**Reference:** [references/commands-and-events.md](references/commands-and-events.md)

## Implementation Tips

1. **Choose the Right Display Mode:**
   - Use **Default** for mobile-style overlays with custom views
   - Use **Compact** for icon-only sidebar that expands to full width
   - Use **Expanded** for always-visible full sidebar

2. **Use Built-in Items vs Custom Views:**
   - Use **NavigationItem** (Compact/Expanded) for standard navigation menus
   - Use **Custom Views** (Default mode) for unique layouts

3. **Optimize Performance:**
   - Use data binding with ItemsSource for large datasets
   - Implement ItemTemplateSelector for heterogeneous items
   - Keep hierarchies shallow (2-3 levels max)

4. **Handle State Properly:**
   - Use Opening/Closing events to cancel state changes
   - Store SelectedItem for navigation state restoration
   - Handle ItemClicked for page navigation

5. **Ensure Accessibility:**
   - Implement keyboard navigation (already built-in)
   - Provide meaningful Header text for screen readers
   - Use sufficient color contrast for SelectionBackground


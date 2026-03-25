# Dropdown Menu Items Configuration

This guide covers the configuration and customization of dropdown menu items in the Split Button control.

## Table of Contents
- [Setting Icons for Dropdown Menu Items](#setting-icons-for-dropdown-menu-items)
- [Icon Bar Visibility](#icon-bar-visibility)
- [Scrollbar Visibility](#scrollbar-visibility)
- [Checkable Dropdown Menu Items](#checkable-dropdown-menu-items)
- [Resizing Dropdown Menu](#resizing-dropdown-menu)
- [Adding Custom Dropdown Menu Items](#adding-custom-dropdown-menu-items)
- [Icon Bar Visibility for Custom Items](#icon-bar-visibility-for-custom-items)

## Setting Icons for Dropdown Menu Items

The `Icon` property provides a pictorial representation for dropdown menu items. You can set the icon by assigning an image source.

**XAML Example:**

```xaml
<syncfusion:SplitButtonAdv Label="Country" 
                            x:Name="splitButton" 
                            DropDirection="BottomRight" 
                            SizeMode="Normal">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="India">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/India.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="France">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/France.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="Germany">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/Germany.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

**C# Example:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
DropDownMenuGroup menu = new DropDownMenuGroup();

DropDownMenuItem item1 = new DropDownMenuItem() 
{ 
    Header = "India", 
    HorizontalAlignment = HorizontalAlignment.Left
};
item1.Icon = new Image() { Source = new BitmapImage(new Uri("/Images/India.png", UriKind.Relative)) };

DropDownMenuItem item2 = new DropDownMenuItem() 
{ 
    Header = "France", 
    HorizontalAlignment = HorizontalAlignment.Left
};
item2.Icon = new Image() { Source = new BitmapImage(new Uri("/Images/France.png", UriKind.Relative)) };

DropDownMenuItem item3 = new DropDownMenuItem() 
{ 
    Header = "Germany", 
    HorizontalAlignment = HorizontalAlignment.Left
};
item3.Icon = new Image() { Source = new BitmapImage(new Uri("/Images/Germany.png", UriKind.Relative)) };

menu.Items.Add(item1);
menu.Items.Add(item2);
menu.Items.Add(item3);

splitButton.Content = menu;
splitButton.Label = "Country";
splitButton.DropDirection = DropDirection.BottomRight;
splitButton.SizeMode = SizeMode.Normal;
```

## Icon Bar Visibility

The icon bar is a vertical bar displayed next to the dropdown menu item icons. You can enable or disable it using the `IconBarEnabled` property.

**Default Value:** `false`

**XAML Example:**

```xaml
<syncfusion:SplitButtonAdv Label="Country" 
                            DropDirection="BottomRight" 
                            x:Name="splitButton" 
                            SizeMode="Normal">
    <syncfusion:DropDownMenuGroup IconBarEnabled="True">
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="India">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/India.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="France">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/France.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="Germany">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/Germany.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

**C# Example:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
DropDownMenuGroup menu = new DropDownMenuGroup();

DropDownMenuItem item1 = new DropDownMenuItem() 
{ 
    Header = "India", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item2 = new DropDownMenuItem() 
{ 
    Header = "France", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item3 = new DropDownMenuItem() 
{ 
    Header = "Germany", 
    HorizontalAlignment = HorizontalAlignment.Left
};

menu.Items.Add(item1);
menu.Items.Add(item2);
menu.Items.Add(item3);
menu.IconBarEnabled = true;

splitButton.Content = menu;
splitButton.Label = "Country";
splitButton.SizeMode = SizeMode.Normal;
splitButton.DropDirection = DropDirection.BottomRight;
```

## Scrollbar Visibility

The dropdown menu group supports a built-in scrollbar to display large numbers of menu items in a compact view. Enable it by setting the `ScrollBarVisibility` property to `Visible`.

**XAML Example:**

```xaml
<syncfusion:SplitButtonAdv Label="Country" 
                            DropDirection="BottomRight" 
                            x:Name="splitButton" 
                            SizeMode="Normal">
    <syncfusion:DropDownMenuGroup MaxHeight="111" 
                                    ScrollBarVisibility="Visible">
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="India">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/India.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="France">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/France.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="Germany">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/Germany.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Canada"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="China"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="United States"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Italy"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Japan"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Spain"/>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" Header="Pakistan"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

**C# Example:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
DropDownMenuGroup menu = new DropDownMenuGroup();

DropDownMenuItem item1 = new DropDownMenuItem() 
{ 
    Header = "India", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item2 = new DropDownMenuItem() 
{ 
    Header = "France", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item3 = new DropDownMenuItem() 
{ 
    Header = "Germany", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item4 = new DropDownMenuItem() 
{ 
    Header = "Canada", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item5 = new DropDownMenuItem() 
{ 
    Header = "China", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item6 = new DropDownMenuItem() 
{ 
    Header = "United States", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item7 = new DropDownMenuItem() 
{ 
    Header = "Italy", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item8 = new DropDownMenuItem() 
{ 
    Header = "Japan", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item9 = new DropDownMenuItem() 
{ 
    Header = "Spain", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item10 = new DropDownMenuItem() 
{ 
    Header = "Pakistan", 
    HorizontalAlignment = HorizontalAlignment.Left
};

menu.Items.Add(item1);
menu.Items.Add(item2);
menu.Items.Add(item3);
menu.Items.Add(item4);
menu.Items.Add(item5);
menu.Items.Add(item6);
menu.Items.Add(item7);
menu.Items.Add(item8);
menu.Items.Add(item9);
menu.Items.Add(item10);

menu.MaxHeight = 111;
menu.ScrollBarVisibility = ScrollBarVisibility.Visible;

splitButton.Content = menu;
splitButton.Label = "Country";
splitButton.SizeMode = SizeMode.Normal;
splitButton.DropDirection = DropDirection.BottomRight;
```

**Best Practice:** Set `MaxHeight` to control when the scrollbar appears. Without a maximum height, the dropdown will expand to fit all items.

## Checkable Dropdown Menu Items

The checkable option allows users to check/uncheck dropdown menu items on selection. Enable it by setting the `IsCheckable` property to `true`.

**XAML Example:**

```xaml
<syncfusion:SplitButtonAdv Label="Country" 
                            DropDirection="BottomRight" 
                            x:Name="splitButton" 
                            SizeMode="Normal">
    <syncfusion:DropDownMenuGroup>
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="India" 
                                     IsChecked="True" 
                                     IsCheckable="True"/>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="France" 
                                     IsChecked="True" 
                                     IsCheckable="True"/>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="Germany"/>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

**C# Example:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
DropDownMenuGroup menu = new DropDownMenuGroup();

DropDownMenuItem item1 = new DropDownMenuItem() 
{ 
    Header = "India", 
    IsChecked = true, 
    IsCheckable = true, 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item2 = new DropDownMenuItem() 
{ 
    Header = "France", 
    IsChecked = true, 
    IsCheckable = true, 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item3 = new DropDownMenuItem() 
{ 
    Header = "Germany", 
    HorizontalAlignment = HorizontalAlignment.Left
};

menu.Items.Add(item1);
menu.Items.Add(item2);
menu.Items.Add(item3);

splitButton.Content = menu;
splitButton.DropDirection = DropDirection.BottomRight;
splitButton.Label = "Country";
splitButton.SizeMode = SizeMode.Normal;
```

**Use Case:** Checkable items are ideal for multi-selection scenarios like filtering options, view settings, or feature toggles.

**Event Handling:** Use the `IsCheckedChanged` event to respond to check state changes. See [dropdown-configuration.md](dropdown-configuration.md) for event details.

## Resizing Dropdown Menu

The dropdown menu group popup can be resized using a resizing gripper. Enable this feature by setting the `IsResizable` property to `true`.

**XAML Example:**

```xaml
<syncfusion:SplitButtonAdv Label="Country" x:Name="splitButton">
    <syncfusion:DropDownMenuGroup IsResizable="True">
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="India">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/India.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="France">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/France.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
        
        <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                     Header="Germany">
            <syncfusion:DropDownMenuItem.Icon>
                <Image Source="/Images/Germany.png"/>
            </syncfusion:DropDownMenuItem.Icon>
        </syncfusion:DropDownMenuItem>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

**C# Example:**

```csharp
SplitButtonAdv splitButton = new SplitButtonAdv();
DropDownMenuGroup menu = new DropDownMenuGroup();

DropDownMenuItem item1 = new DropDownMenuItem() 
{ 
    Header = "India", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item2 = new DropDownMenuItem() 
{ 
    Header = "France", 
    HorizontalAlignment = HorizontalAlignment.Left
};
DropDownMenuItem item3 = new DropDownMenuItem() 
{ 
    Header = "Germany", 
    HorizontalAlignment = HorizontalAlignment.Left
};

menu.Items.Add(item1);
menu.Items.Add(item2);
menu.Items.Add(item3);
menu.IsResizable = true;

splitButton.Content = menu;
splitButton.Label = "Country";
```

**Behavior:** When enabled, a gripper appears at the bottom-right corner of the dropdown popup, allowing users to drag and resize the popup height and width.

## Adding Custom Dropdown Menu Items

The dropdown menu group supports loading custom items beyond standard dropdown menu items. Use the `MoreItems` property to add custom content.

**Return Type:** `ObservableCollection<UIElement>`

This means you can add any UIElement as child items, including:
- Labels
- TextBlocks
- Buttons
- Custom controls
- Any WPF UIElement

**ViewModel Example:**

```csharp
using Syncfusion.Windows.Shared;
using System.Collections.ObjectModel;
using System.Windows;
using System.Windows.Controls;

public class ColorViewModel : NotificationObject
{
    public ObservableCollection<UIElement> items = new ObservableCollection<UIElement>();

    public ObservableCollection<UIElement> Items
    {
        get { return items; }
        set 
        { 
            items = value; 
            RaisePropertyChanged("Items"); 
        }
    }

    public ColorViewModel()
    {
        Items.Add(new Label() { Content = "More Items" });
        Items.Add(new TextBlock() { Text = "Custom Content", Margin = new Thickness(5) });
        Items.Add(new Button() { Content = "Action Button", Margin = new Thickness(5) });
    }
}
```

**XAML Example:**

```xaml
<Window x:Class="SplitButton_Custom_Items.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:SplitButton_Custom_Items"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Window.DataContext>
        <local:ColorViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:SplitButtonAdv Label="Colors" SizeMode="Normal">
            <syncfusion:DropDownMenuGroup IconBarEnabled="True" 
                                           MoreItems="{Binding Items}" 
                                           IsMoreItemsIconTrayEnabled="False">
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="Black">
                    <syncfusion:DropDownMenuItem.Icon>
                        <Image Source="/Images/Black.png"/>
                    </syncfusion:DropDownMenuItem.Icon>
                </syncfusion:DropDownMenuItem>
                
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="Orange">
                    <syncfusion:DropDownMenuItem.Icon>
                        <Image Source="/Images/Orange.png"/>
                    </syncfusion:DropDownMenuItem.Icon>
                </syncfusion:DropDownMenuItem>
                
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="Red">
                    <syncfusion:DropDownMenuItem.Icon>
                        <Image Source="/Images/Red.png"/>
                    </syncfusion:DropDownMenuItem.Icon>
                </syncfusion:DropDownMenuItem>
            </syncfusion:DropDownMenuGroup>
        </syncfusion:SplitButtonAdv>
    </Grid>
</Window>
```

**Display Order:** Custom items in `MoreItems` appear at the bottom of the dropdown menu, after all standard `DropDownMenuItem` items.

## Icon Bar Visibility for Custom Items

The icon bar visibility for custom dropdown menu items can be controlled using the `IsMoreItemsIconTrayEnabled` property.

**Default Value:** `false`

**XAML Example:**

```xaml
<Window x:Class="SplitButton_Custom_Items.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:SplitButton_Custom_Items"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Window.DataContext>
        <local:ColorViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:SplitButtonAdv Label="Colors" 
                                    x:Name="splitButton" 
                                    SizeMode="Normal">
            <syncfusion:DropDownMenuGroup IconBarEnabled="True" 
                                           MoreItems="{Binding Colors}" 
                                           IsMoreItemsIconTrayEnabled="True">
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="Black">
                    <syncfusion:DropDownMenuItem.Icon>
                        <Image Source="/Images/Black.png"/>
                    </syncfusion:DropDownMenuItem.Icon>
                </syncfusion:DropDownMenuItem>
                
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="Orange">
                    <syncfusion:DropDownMenuItem.Icon>
                        <Image Source="/Images/Orange.png"/>
                    </syncfusion:DropDownMenuItem.Icon>
                </syncfusion:DropDownMenuItem>
                
                <syncfusion:DropDownMenuItem HorizontalAlignment="Left" 
                                             Header="Red">
                    <syncfusion:DropDownMenuItem.Icon>
                        <Image Source="/Images/Red.png"/>
                    </syncfusion:DropDownMenuItem.Icon>
                </syncfusion:DropDownMenuItem>
            </syncfusion:DropDownMenuGroup>
        </syncfusion:SplitButtonAdv>
    </Grid>
</Window>
```

**Enhanced ViewModel with DropDownMenuItem:**

```csharp
using Syncfusion.Windows.Shared;
using Syncfusion.Windows.Tools.Controls;
using System.Collections.ObjectModel;
using System.Windows;
using System.Windows.Media.Imaging;

public class ColorViewModel : NotificationObject
{
    public ObservableCollection<UIElement> color = new ObservableCollection<UIElement>();

    public ObservableCollection<UIElement> Colors
    {
        get { return color; }
        set 
        { 
            color = value; 
            RaisePropertyChanged("Colors"); 
        }
    }

    public ColorViewModel()
    {
        Colors.Add(new DropDownMenuItem() 
        { 
            Header = "More Items",
            Icon = new Image() 
            { 
                Source = new BitmapImage(new Uri("/Images/More.png", UriKind.RelativeOrAbsolute)) 
            }
        });
    }
}
```

**Visual Difference:**
- `IsMoreItemsIconTrayEnabled="False"` - No icon bar space for custom items
- `IsMoreItemsIconTrayEnabled="True"` - Icon bar space extended to custom items section

## GitHub Samples

View complete samples on GitHub:
- [Customize Menu Items Sample](https://github.com/SyncfusionExamples/wpf-split-button-examples/tree/master/Samples/Customize-Menu-Items) - Demonstrates icon, icon bar, scrollbar, and checkable support
- [Add Custom Items Sample](https://github.com/SyncfusionExamples/wpf-split-button-examples/tree/master/Samples/Add-Custom-Items) - Demonstrates custom items and icon bar visibility

## Summary

This guide covered:
- Setting icons for menu items with `Icon` property
- Controlling icon bar visibility with `IconBarEnabled`
- Enabling scrollbars for large menus with `ScrollBarVisibility`
- Creating checkable items with `IsCheckable` and `IsChecked`
- Enabling resizable dropdowns with `IsResizable`
- Adding custom items via `MoreItems` property
- Managing custom item icon bars with `IsMoreItemsIconTrayEnabled`

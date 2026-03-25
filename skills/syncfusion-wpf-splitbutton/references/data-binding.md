# Data Binding in Split Button

This guide covers data binding patterns for populating dropdown menu items using MVVM principles with the Split Button control.

## Table of Contents
- [Overview](#overview)
- [Creating Model Classes](#creating-model-classes)
- [Creating View Models](#creating-view-models)
- [Binding Data from View Model](#binding-data-from-view-model)
- [Setting DataContext](#setting-datacontext)
- [Binding Commands from View Model](#binding-commands-from-view-model)
- [Complete MVVM Example](#complete-mvvm-example)
- [Best Practices](#best-practices)

## Overview

Data binding provides an easier way to assign, visualize, and interact with collections of predefined data. The data binding in SplitButtonAdv is achieved by populating the `DropDownMenuGroup.ItemsSource` property with a collection.

**Key Benefits:**
- Separation of concerns (MVVM pattern)
- Dynamic menu item generation from data
- Automatic UI updates when data changes
- Reusable view models
- Command pattern integration

## Creating Model Classes

Create a class that holds the properties representing your menu item data. The model should contain all information needed to display and identify each menu item.

**Example: Country Model**

```csharp
public class Country
{
    private string name;
    private BitmapImage flag;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public BitmapImage Flag
    {
        get { return flag; }
        set { flag = value; }
    }
}
```

**Auto-Property Version:**

```csharp
public class Country
{
    public string Name { get; set; }
    public BitmapImage Flag { get; set; }
}
```

**Complex Model Example:**

```csharp
public class MenuItem
{
    public string Name { get; set; }
    public string Description { get; set; }
    public BitmapImage Icon { get; set; }
    public bool IsEnabled { get; set; }
    public bool IsChecked { get; set; }
    public string Category { get; set; }
}
```

## Creating View Models

Create a view model class that populates a list of model objects representing dropdown menu items. The view model should inherit from `NotificationObject` (or implement `INotifyPropertyChanged`) to support property change notifications.

**Basic View Model:**

```csharp
public class CountryViewModel
{
    private List<Country> dropDownItems;

    public List<Country> DropDownItems
    {
        get { return dropDownItems; }
        set { dropDownItems = value; }
    }

    public CountryViewModel()
    {
        DropDownItems = new List<Country>();
        
        DropDownItems.Add(new Country()
        {
            Name = "India",
            Flag = new BitmapImage(new Uri("/Images/India.png", UriKind.Relative))
        });

        DropDownItems.Add(new Country()
        {
            Name = "France",
            Flag = new BitmapImage(new Uri("/Images/France.png", UriKind.Relative))
        });
        
        DropDownItems.Add(new Country()
        {
            Name = "Germany",
            Flag = new BitmapImage(new Uri("/Images/Germany.png", UriKind.Relative))
        });
    }
}
```

**Observable Collection Version:**

Using `ObservableCollection<T>` enables automatic UI updates when items are added or removed:

```csharp
using System.Collections.ObjectModel;

public class CountryViewModel : NotificationObject
{
    private ObservableCollection<Country> dropDownItems;

    public ObservableCollection<Country> DropDownItems
    {
        get { return dropDownItems; }
        set 
        { 
            dropDownItems = value;
            RaisePropertyChanged(nameof(DropDownItems));
        }
    }

    public CountryViewModel()
    {
        DropDownItems = new ObservableCollection<Country>
        {
            new Country { Name = "India", Flag = LoadImage("/Images/India.png") },
            new Country { Name = "France", Flag = LoadImage("/Images/France.png") },
            new Country { Name = "Germany", Flag = LoadImage("/Images/Germany.png") },
            new Country { Name = "Canada", Flag = LoadImage("/Images/Canada.png") },
            new Country { Name = "China", Flag = LoadImage("/Images/China.png") }
        };
    }

    private BitmapImage LoadImage(string path)
    {
        return new BitmapImage(new Uri(path, UriKind.Relative));
    }
}
```

## Binding Data from View Model

Bind the collection to the `ItemsSource` property of `DropDownMenuGroup` and define how each item should be displayed using `ItemTemplate`.

**XAML Example:**

```xaml
<syncfusion:SplitButtonAdv Label="Country" SizeMode="Normal">
    <syncfusion:DropDownMenuGroup ItemsSource="{Binding DropDownItems}">
        <syncfusion:DropDownMenuGroup.ItemTemplate>
            <DataTemplate>
                <syncfusion:DropDownMenuItem Header="{Binding Name}" 
                                             HorizontalAlignment="Left">
                    <syncfusion:DropDownMenuItem.Icon>
                        <Image Source="{Binding Flag}"/>
                    </syncfusion:DropDownMenuItem.Icon>
                </syncfusion:DropDownMenuItem>
            </DataTemplate>
        </syncfusion:DropDownMenuGroup.ItemTemplate>
    </syncfusion:DropDownMenuGroup>
</syncfusion:SplitButtonAdv>
```

**Key Points:**
- `ItemsSource` binds to the collection in your view model
- `ItemTemplate` defines the visual structure for each item
- `Header` binds to the display property (e.g., `Name`)
- `Icon` can bind to image properties

**Advanced ItemTemplate:**

```xaml
<syncfusion:DropDownMenuGroup ItemsSource="{Binding MenuItems}">
    <syncfusion:DropDownMenuGroup.ItemTemplate>
        <DataTemplate>
            <syncfusion:DropDownMenuItem Header="{Binding Name}" 
                                         IsCheckable="{Binding IsCheckable}"
                                         IsChecked="{Binding IsChecked}"
                                         IsEnabled="{Binding IsEnabled}"
                                         HorizontalAlignment="Left">
                <syncfusion:DropDownMenuItem.Icon>
                    <Image Source="{Binding Icon}" Width="16" Height="16"/>
                </syncfusion:DropDownMenuItem.Icon>
            </syncfusion:DropDownMenuItem>
        </DataTemplate>
    </syncfusion:DropDownMenuGroup.ItemTemplate>
</syncfusion:DropDownMenuGroup>
```

## Setting DataContext

The `DataContext` connects the view (XAML) to the view model. Set it in the Window constructor or in XAML.

**Option 1: Code-Behind**

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.DataContext = new CountryViewModel();
    }
}
```

**Option 2: XAML**

```xaml
<Window x:Class="SplitButton_DataBinding.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:SplitButton_DataBinding"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Window.DataContext>
        <local:CountryViewModel/>
    </Window.DataContext>
    
    <Grid>
        <syncfusion:SplitButtonAdv Label="Country">
            <!-- Binding content here -->
        </syncfusion:SplitButtonAdv>
    </Grid>
</Window>
```

**Option 3: Design-Time DataContext**

```xaml
<Window xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        d:DataContext="{d:DesignInstance Type=local:CountryViewModel, IsDesignTimeCreatable=True}">
    <!-- Content -->
</Window>
```

## Binding Commands from View Model

Bind commands to handle user interactions with menu items. This requires implementing the `ICommand` interface, typically using a `DelegateCommand` pattern.

**DelegateCommand Implementation:**

```csharp
using System;
using System.Windows.Input;

public class DelegateCommand<T> : ICommand
{
    private Predicate<T> _canExecute;
    private Action<T> _method;
    private bool _canExecuteCache = true;

    public DelegateCommand(Action<T> method) : this(method, null)
    {
    }

    public DelegateCommand(Action<T> method, Predicate<T> canExecute)
    {
        _method = method;
        _canExecute = canExecute;
    }

    public bool CanExecute(object parameter)
    {
        if (_canExecute != null)
        {
            bool tempCanExecute = _canExecute((T)parameter);

            if (_canExecuteCache != tempCanExecute)
            {
                _canExecuteCache = tempCanExecute;
                RaiseCanExecuteChanged();
            }
        }

        return _canExecuteCache;
    }

    public void RaiseCanExecuteChanged()
    {
        CanExecuteChanged?.Invoke(this, new EventArgs());
    }

    public void Execute(object parameter)
    {
        _method?.Invoke((T)parameter);
    }

    public event EventHandler CanExecuteChanged;
}
```

**View Model with Command:**

```csharp
using Syncfusion.Windows.Shared;
using System.Collections.Generic;
using System.Windows;

public class CountryViewModel : NotificationObject
{
    private List<Country> dropDownItems;
    private bool _canPerformAction = true;

    public List<Country> DropDownItems
    {
        get { return dropDownItems; }
        set { dropDownItems = value; }
    }

    public bool CanPerformAction
    {
        get { return _canPerformAction; }
        set
        {
            _canPerformAction = value;
            ClickCommand.RaiseCanExecuteChanged();
            RaisePropertyChanged(nameof(CanPerformAction));
        }
    }

    public DelegateCommand<object> ClickCommand { get; set; }

    public CountryViewModel()
    {
        DropDownItems = new List<Country>
        {
            new Country { Name = "India" },
            new Country { Name = "France" },
            new Country { Name = "Germany" }
        };

        ClickCommand = new DelegateCommand<object>(
            ClickAction, 
            CanPerformClickAction);
    }

    private bool CanPerformClickAction(object parameter)
    {
        return CanPerformAction;
    }

    private void ClickAction(object parameter)
    {
        Country country = parameter as Country;
        if (country != null)
        {
            MessageBox.Show($"{country.Name} has been clicked");
        }
    }
}
```

**XAML Command Binding:**

```xaml
<Window x:Class="Split_Button_Data_Binding.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:Split_Button_Data_Binding"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Window.DataContext>
        <local:CountryViewModel/>
    </Window.DataContext>
    
    <Grid VerticalAlignment="Center">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="270"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        
        <CheckBox IsChecked="{Binding CanPerformAction}" 
                  Grid.Column="0" 
                  Content="Can perform action in dropdown menu items"/>
        
        <syncfusion:SplitButtonAdv x:Name="splitButton" 
                                    Label="Country" 
                                    Grid.Column="1">
            <syncfusion:DropDownMenuGroup ItemsSource="{Binding DropDownItems}">
                <syncfusion:DropDownMenuGroup.ItemTemplate>
                    <DataTemplate>
                        <syncfusion:DropDownMenuItem 
                            Header="{Binding Name}"
                            Command="{Binding DataContext.ClickCommand, 
                                     Source={x:Reference splitButton}}"
                            CommandParameter="{Binding}">
                            <syncfusion:DropDownMenuItem.Icon>
                                <Image Source="{Binding Flag}"/>
                            </syncfusion:DropDownMenuItem.Icon>
                        </syncfusion:DropDownMenuItem>
                    </DataTemplate>
                </syncfusion:DropDownMenuGroup.ItemTemplate>
            </syncfusion:DropDownMenuGroup>
        </syncfusion:SplitButtonAdv>
    </Grid>
</Window>
```

**Important:** Use `Source={x:Reference splitButton}` or `RelativeSource` to bind to the window's `DataContext` from within the `DataTemplate`.

**Alternative Binding Approaches:**

```xaml
<!-- Using RelativeSource -->
<syncfusion:DropDownMenuItem 
    Command="{Binding DataContext.ClickCommand, 
             RelativeSource={RelativeSource AncestorType=syncfusion:SplitButtonAdv}}"
    CommandParameter="{Binding}"/>

<!-- Using ElementName (requires named element) -->
<syncfusion:DropDownMenuItem 
    Command="{Binding DataContext.ClickCommand, ElementName=splitButton}"
    CommandParameter="{Binding}"/>
```

## Complete MVVM Example

**Complete Window XAML:**

```xaml
<Window x:Class="Split_Button_MVVM.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:Split_Button_MVVM"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Split Button MVVM Example" Height="450" Width="800">
    <Window.DataContext>
        <local:CountryViewModel/>
    </Window.DataContext>
    
    <Grid Margin="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <TextBlock Text="Select a Country:" 
                   FontSize="16" 
                   Margin="0,0,0,10"/>
        
        <CheckBox Grid.Row="1" 
                  IsChecked="{Binding CanPerformAction}" 
                  Content="Enable country selection"
                  Margin="0,0,0,10"/>
        
        <syncfusion:SplitButtonAdv x:Name="splitButton"
                                    Grid.Row="2"
                                    Label="Country" 
                                    SizeMode="Normal"
                                    HorizontalAlignment="Left"
                                    VerticalAlignment="Top">
            <syncfusion:DropDownMenuGroup ItemsSource="{Binding DropDownItems}"
                                           IconBarEnabled="True">
                <syncfusion:DropDownMenuGroup.ItemTemplate>
                    <DataTemplate>
                        <syncfusion:DropDownMenuItem 
                            Header="{Binding Name}"
                            Command="{Binding DataContext.SelectCountryCommand, 
                                     Source={x:Reference splitButton}}"
                            CommandParameter="{Binding}">
                            <syncfusion:DropDownMenuItem.Icon>
                                <Image Source="{Binding Flag}" 
                                       Width="16" 
                                       Height="16"/>
                            </syncfusion:DropDownMenuItem.Icon>
                        </syncfusion:DropDownMenuItem>
                    </DataTemplate>
                </syncfusion:DropDownMenuGroup.ItemTemplate>
            </syncfusion:DropDownMenuGroup>
        </syncfusion:SplitButtonAdv>
    </Grid>
</Window>
```

**Complete View Model:**

```csharp
using Syncfusion.Windows.Shared;
using System;
using System.Collections.ObjectModel;
using System.Windows;
using System.Windows.Media.Imaging;

namespace Split_Button_MVVM
{
    public class CountryViewModel : NotificationObject
    {
        private ObservableCollection<Country> dropDownItems;
        private bool canPerformAction = true;

        public ObservableCollection<Country> DropDownItems
        {
            get { return dropDownItems; }
            set
            {
                dropDownItems = value;
                RaisePropertyChanged(nameof(DropDownItems));
            }
        }

        public bool CanPerformAction
        {
            get { return canPerformAction; }
            set
            {
                canPerformAction = value;
                SelectCountryCommand.RaiseCanExecuteChanged();
                RaisePropertyChanged(nameof(CanPerformAction));
            }
        }

        public DelegateCommand<object> SelectCountryCommand { get; set; }

        public CountryViewModel()
        {
            InitializeData();
            SelectCountryCommand = new DelegateCommand<object>(
                ExecuteSelectCountry,
                CanExecuteSelectCountry);
        }

        private void InitializeData()
        {
            DropDownItems = new ObservableCollection<Country>
            {
                new Country 
                { 
                    Name = "India", 
                    Flag = LoadImage("/Images/India.png") 
                },
                new Country 
                { 
                    Name = "France", 
                    Flag = LoadImage("/Images/France.png") 
                },
                new Country 
                { 
                    Name = "Germany", 
                    Flag = LoadImage("/Images/Germany.png") 
                },
                new Country 
                { 
                    Name = "Canada", 
                    Flag = LoadImage("/Images/Canada.png") 
                },
                new Country 
                { 
                    Name = "China", 
                    Flag = LoadImage("/Images/China.png") 
                }
            };
        }

        private BitmapImage LoadImage(string path)
        {
            try
            {
                return new BitmapImage(new Uri(path, UriKind.Relative));
            }
            catch
            {
                return null;
            }
        }

        private bool CanExecuteSelectCountry(object parameter)
        {
            return CanPerformAction;
        }

        private void ExecuteSelectCountry(object parameter)
        {
            Country country = parameter as Country;
            if (country != null)
            {
                MessageBox.Show(
                    $"You selected: {country.Name}",
                    "Country Selected",
                    MessageBoxButton.OK,
                    MessageBoxImage.Information);
            }
        }
    }
}
```

## Best Practices

### 1. Use ObservableCollection for Dynamic Data

```csharp
// Good: UI updates automatically when items change
public ObservableCollection<Country> Countries { get; set; }

// Avoid: UI doesn't update when items change
public List<Country> Countries { get; set; }
```

### 2. Implement INotifyPropertyChanged

```csharp
// Good: Inherit from NotificationObject or implement INotifyPropertyChanged
public class ViewModel : NotificationObject
{
    private string _selectedItem;
    public string SelectedItem
    {
        get { return _selectedItem; }
        set
        {
            _selectedItem = value;
            RaisePropertyChanged(nameof(SelectedItem));
        }
    }
}
```

### 3. Use Proper Command Binding

```csharp
// Good: Command with CanExecute logic
ClickCommand = new DelegateCommand<object>(
    ExecuteAction,
    CanExecuteAction);

// Avoid: Direct event handlers in MVVM
```

### 4. Handle Null Values

```csharp
private void ExecuteAction(object parameter)
{
    var item = parameter as Country;
    if (item != null)
    {
        // Process item
    }
}
```

### 5. Use Design-Time Data

```xaml
d:DataContext="{d:DesignInstance Type=local:CountryViewModel, IsDesignTimeCreatable=True}"
```

## Troubleshooting

**Problem:** Items not displaying  
**Solution:** Verify `ItemsSource` binding and `DataContext` are set correctly

**Problem:** Commands not executing  
**Solution:** Check command binding path and ensure `CanExecute` returns true

**Problem:** Icons not showing  
**Solution:** Verify image paths are correct and Build Action is set to "Resource"

**Problem:** UI not updating when data changes  
**Solution:** Use `ObservableCollection` and raise `PropertyChanged` events

## GitHub Sample

View the complete sample on GitHub:
- [Data Binding Sample](https://github.com/SyncfusionExamples/wpf-split-button-examples/blob/master/Samples/Data-Binding)

## Related Topics

- [Command Binding](command-binding.md) - Detailed command implementation patterns
- [Dropdown Menu Items](dropdown-menu-items.md) - Menu item configuration options

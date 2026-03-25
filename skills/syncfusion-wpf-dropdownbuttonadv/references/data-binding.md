# Data Binding for DropDownButtonAdv

## Table of Contents
- [When to Use Data Binding](#when-to-use-data-binding)
- [Step 1: Create the Model](#step-1-create-the-model)
- [Step 2: Create the ViewModel](#step-2-create-the-viewmodel)
- [Step 3: Bind ItemsSource in XAML](#step-3-bind-itemssource-in-xaml)
- [Step 4: Bind Commands from ViewModel](#step-4-bind-commands-from-viewmodel)
- [Full MVVM Example](#full-mvvm-example)
- [Gotchas](#gotchas)

---

## When to Use Data Binding

| Use data binding when... | Use declarative items when... |
|---|---|
| Items come from a database or service | Items are static and known at design time |
| Items change at runtime | Small fixed list (3-5 items) |
| MVVM pattern is required | No commands or business logic needed |
| Items need command support per item | Simple click-only scenarios |

---

## Step 1: Create the Model

```csharp
public class Country
{
    public string Name { get; set; }
    public BitmapImage Flag { get; set; }
}
```

---

## Step 2: Create the ViewModel

```csharp
using Syncfusion.Windows.Shared; // For NotificationObject

public class CountryViewModel : NotificationObject
{
    public List<Country> DropDownItems { get; set; }

    public CountryViewModel()
    {
        DropDownItems = new List<Country>
        {
            new Country
            {
                Name = "India",
                Flag = new BitmapImage(new Uri("Images/india.png", UriKind.RelativeOrAbsolute))
            },
            new Country
            {
                Name = "France",
                Flag = new BitmapImage(new Uri("Images/france.png", UriKind.RelativeOrAbsolute))
            },
            new Country
            {
                Name = "Germany",
                Flag = new BitmapImage(new Uri("Images/germany.png", UriKind.RelativeOrAbsolute))
            }
        };
    }
}
```

---

## Step 3: Bind ItemsSource in XAML

Set `DataContext` on the window and bind `DropDownMenuGroup.ItemsSource`. Use `ItemTemplate` to define how each item renders:

```xaml
<Window.DataContext>
    <local:CountryViewModel/>
</Window.DataContext>

<syncfusion:DropDownButtonAdv Label="Country" SmallIcon="Images/flag.png">
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
</syncfusion:DropDownButtonAdv>
```

Code-behind to set DataContext manually:

```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new CountryViewModel();
}
```

---

## Step 4: Bind Commands from ViewModel

When binding commands inside a `DataTemplate`, use `x:Reference` to reach the parent `DropDownButtonAdv`'s `DataContext`:

```xaml
<syncfusion:DropDownButtonAdv x:Name="dropdownButton"
                               Label="Country"
                               SmallIcon="Images/flag.png">
    <syncfusion:DropDownMenuGroup ItemsSource="{Binding DropDownItems}">
        <syncfusion:DropDownMenuGroup.ItemTemplate>
            <DataTemplate>
                <syncfusion:DropDownMenuItem
                    Header="{Binding Name}"
                    HorizontalAlignment="Left"
                    Command="{Binding DataContext.ClickCommand, Source={x:Reference dropdownButton}}"
                    CommandParameter="{Binding .}">
                    <syncfusion:DropDownMenuItem.Icon>
                        <Image Source="{Binding Flag}"/>
                    </syncfusion:DropDownMenuItem.Icon>
                </syncfusion:DropDownMenuItem>
            </DataTemplate>
        </syncfusion:DropDownMenuGroup.ItemTemplate>
    </syncfusion:DropDownMenuGroup>
</syncfusion:DropDownButtonAdv>
```

> **Why `x:Reference`?** Inside a `DataTemplate`, `DataContext` is the item model object. Use `Source={x:Reference dropdownButton}` to navigate back up to the button's DataContext (ViewModel) to find `ClickCommand`.

---

## Full MVVM Example

**Model:**
```csharp
public class Country
{
    public string Name { get; set; }
    public BitmapImage Flag { get; set; }
}
```

**ViewModel with DelegateCommand:**
```csharp
public class CountryViewModel : NotificationObject
{
    public List<Country> DropDownItems { get; set; }
    public DelegateCommand<object> ClickCommand { get; set; }

    private bool _canPerformAction = true;
    public bool CanPerformAction
    {
        get => _canPerformAction;
        set
        {
            _canPerformAction = value;
            ClickCommand.RaiseCanExecuteChanged();
            RaisePropertyChanged(nameof(CanPerformAction));
        }
    }

    public CountryViewModel()
    {
        ClickCommand = new DelegateCommand<object>(OnItemClicked, CanClickItem);
        DropDownItems = new List<Country>
        {
            new Country { Name = "India", Flag = new BitmapImage(new Uri("Images/india.png", UriKind.RelativeOrAbsolute)) },
            new Country { Name = "France", Flag = new BitmapImage(new Uri("Images/france.png", UriKind.RelativeOrAbsolute)) }
        };
    }

    private bool CanClickItem(object parameter) => CanPerformAction;

    private void OnItemClicked(object parameter)
    {
        var country = parameter as Country;
        MessageBox.Show($"{country?.Name} selected");
    }
}
```

**XAML:**
```xaml
<Window.DataContext>
    <local:CountryViewModel/>
</Window.DataContext>
<Grid>
    <CheckBox IsChecked="{Binding CanPerformAction}" Content="Enable actions"/>
    <syncfusion:DropDownButtonAdv x:Name="dropdownButton" Label="Country" SmallIcon="Images/flag.png">
        <syncfusion:DropDownMenuGroup ItemsSource="{Binding DropDownItems}">
            <syncfusion:DropDownMenuGroup.ItemTemplate>
                <DataTemplate>
                    <syncfusion:DropDownMenuItem
                        Header="{Binding Name}"
                        HorizontalAlignment="Left"
                        Command="{Binding DataContext.ClickCommand, Source={x:Reference dropdownButton}}"
                        CommandParameter="{Binding .}">
                        <syncfusion:DropDownMenuItem.Icon>
                            <Image Source="{Binding Flag}"/>
                        </syncfusion:DropDownMenuItem.Icon>
                    </syncfusion:DropDownMenuItem>
                </DataTemplate>
            </syncfusion:DropDownMenuGroup.ItemTemplate>
        </syncfusion:DropDownMenuGroup>
    </syncfusion:DropDownButtonAdv>
</Grid>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `ClickCommand` not found in DataTemplate | `DataContext` is item, not ViewModel | Use `Source={x:Reference dropdownButton}` to reach ViewModel |
| Items don't update when collection changes | Using `List<T>` | Use `ObservableCollection<T>` for dynamic updates |
| `Command.CanExecute` not re-evaluated | Missing `RaiseCanExecuteChanged()` | Call it after changing the `CanPerformAction` property |
| `NotificationObject` not found | Missing `Syncfusion.Windows.Shared` reference | Add `using Syncfusion.Windows.Shared;` |

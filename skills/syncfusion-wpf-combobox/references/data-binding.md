# Data Binding Guide

## Table of Contents
- [Overview](#overview)
- [Data Binding Fundamentals](#data-binding-fundamentals)
- [Creating Model Classes](#creating-model-classes)
- [Creating ViewModels](#creating-viewmodels)
- [Binding ItemsSource](#binding-itemssource)
- [DisplayMemberPath vs ItemTemplate](#displaymemberpath-vs-itemtemplate)
- [Two-Way Binding](#two-way-binding)
- [ObservableCollection Pattern](#observablecollection-pattern)
- [INotifyPropertyChanged Implementation](#inotifypropertychanged-implementation)
- [Advanced Binding Scenarios](#advanced-binding-scenarios)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Data binding connects the ComboBoxAdv control to your application's data layer using the MVVM (Model-View-ViewModel) pattern. This approach provides clean separation of concerns, testability, and responsive UI updates.

**Key Concepts:**
- **Model**: Data object (e.g., Country, Product)
- **ViewModel**: Exposes data collections and handles logic
- **View**: XAML with bindings to ViewModel
- **ObservableCollection**: Notifies UI of collection changes
- **INotifyPropertyChanged**: Notifies UI of property changes

## Data Binding Fundamentals

### Why Data Binding?

1. **Separation of Concerns**: UI logic separate from business logic
2. **Automatic Updates**: UI reflects data changes automatically
3. **Testability**: ViewModels can be unit tested
4. **Reusability**: Models and ViewModels can be shared
5. **Maintainability**: Changes in one layer don't break others

### Basic Binding Flow

```
ViewModel.Collection → ItemsSource → ComboBoxAdv → DisplayMemberPath → Display
                                                  → SelectedItem → ViewModel.SelectedProperty
```

## Creating Model Classes

Models represent your data entities.

### Simple Model

```csharp
public class Country
{
    public string Name { get; set; }
    public string Code { get; set; }
}
```

### Complex Model

```csharp
public class PopulationInfo
{
    public string Continent { get; set; }
    public string Country { get; set; }
    public double Population { get; set; }
    public double Growth { get; set; }
    public string FlagUrl { get; set; }
}
```

### Model with Validation

```csharp
using System.ComponentModel.DataAnnotations;

public class Product
{
    public int Id { get; set; }
    
    [Required]
    [StringLength(100)]
    public string ProductName { get; set; }
    
    [Range(0, 10000)]
    public decimal Price { get; set; }
    
    public string Category { get; set; }
}
```

## Creating ViewModels

ViewModels manage data collections and handle selection logic.

### Basic ViewModel

```csharp
using System.Collections.ObjectModel;

public class CountryViewModel
{
    public ObservableCollection<Country> Countries { get; set; }

    public CountryViewModel()
    {
        Countries = new ObservableCollection<Country>
        {
            new Country { Name = "Denmark", Code = "DK" },
            new Country { Name = "New Zealand", Code = "NZ" },
            new Country { Name = "Canada", Code = "CA" },
            new Country { Name = "Russia", Code = "RU" },
            new Country { Name = "Japan", Code = "JP" }
        };
    }
}
```

### ViewModel with INotifyPropertyChanged

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class PopulationViewModel : INotifyPropertyChanged
{
    private ObservableCollection<PopulationInfo> populationDetails;
    
    public ObservableCollection<PopulationInfo> PopulationDetails
    {
        get { return populationDetails; }
        set
        {
            populationDetails = value;
            OnPropertyChanged(nameof(PopulationDetails));
        }
    }

    public PopulationViewModel()
    {
        PopulationDetails = new ObservableCollection<PopulationInfo>
        {
            new PopulationInfo { Continent = "Asia", Country = "Indonesia", Growth = 3, Population = 237641326 },
            new PopulationInfo { Continent = "Asia", Country = "Russia", Growth = 2, Population = 152518015 },
            new PopulationInfo { Continent = "Asia", Country = "Malaysia", Growth = 1, Population = 29672000 },
            new PopulationInfo { Continent = "North America", Country = "United States", Growth = 4, Population = 315645000 },
            new PopulationInfo { Continent = "North America", Country = "Mexico", Growth = 2, Population = 112336538 },
            new PopulationInfo { Continent = "North America", Country = "Canada", Growth = 1, Population = 35056064 },
            new PopulationInfo { Continent = "South America", Country = "Colombia", Growth = 1, Population = 47000000 },
            new PopulationInfo { Continent = "South America", Country = "Brazil", Growth = 3, Population = 193946886 },
            new PopulationInfo { Continent = "Africa", Country = "Nigeria", Growth = 2, Population = 170901000 },
            new PopulationInfo { Continent = "Africa", Country = "Egypt", Growth = 1, Population = 83661000 },
            new PopulationInfo { Continent = "Europe", Country = "Germany", Growth = 1, Population = 81993000 },
            new PopulationInfo { Continent = "Europe", Country = "France", Growth = 1, Population = 65605000 },
            new PopulationInfo { Continent = "Europe", Country = "UK", Growth = 1, Population = 63181775 }
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### ViewModel with Selection Tracking

```csharp
public class ProductViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Product> products;
    private Product selectedProduct;
    
    public ObservableCollection<Product> Products
    {
        get { return products; }
        set
        {
            products = value;
            OnPropertyChanged(nameof(Products));
        }
    }
    
    public Product SelectedProduct
    {
        get { return selectedProduct; }
        set
        {
            selectedProduct = value;
            OnPropertyChanged(nameof(SelectedProduct));
            // Additional logic when selection changes
            OnSelectionChanged();
        }
    }

    public ProductViewModel()
    {
        LoadProducts();
    }

    private void LoadProducts()
    {
        Products = new ObservableCollection<Product>
        {
            new Product { Id = 1, ProductName = "Laptop", Price = 1200, Category = "Electronics" },
            new Product { Id = 2, ProductName = "Mouse", Price = 25, Category = "Electronics" },
            new Product { Id = 3, ProductName = "Keyboard", Price = 75, Category = "Electronics" }
        };
    }

    private void OnSelectionChanged()
    {
        // Handle selection change
        if (SelectedProduct != null)
        {
            // Log, update other properties, etc.
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Binding ItemsSource

### Setting DataContext

**In XAML:**
```xml
<Window xmlns:local="clr-namespace:YourNamespace">
    <Window.DataContext>
        <local:CountryViewModel/>
    </Window.DataContext>
    
    <syncfusion:ComboBoxAdv ItemsSource="{Binding Countries}"/>
</Window>
```

**In Code-Behind:**
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

### Basic ItemsSource Binding

```xml
<syncfusion:ComboBoxAdv x:Name="comboBoxAdv" 
                        Height="30" Width="200"
                        ItemsSource="{Binding PopulationDetails}"/>
```

### With DisplayMemberPath

```xml
<syncfusion:ComboBoxAdv ItemsSource="{Binding PopulationDetails}"
                        DisplayMemberPath="Country"
                        Height="30" Width="200"/>
```

### With SelectedItem Binding

```xml
<syncfusion:ComboBoxAdv ItemsSource="{Binding Products}"
                        DisplayMemberPath="ProductName"
                        SelectedItem="{Binding SelectedProduct, Mode=TwoWay}"/>
```

## DisplayMemberPath vs ItemTemplate

### DisplayMemberPath (Simple Display)

Use for displaying a single property:

```xml
<syncfusion:ComboBoxAdv ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"/>
```

**Result:** Shows country names

### ItemTemplate (Custom Display)

Use for complex layouts with multiple properties:

```xml
<syncfusion:ComboBoxAdv ItemsSource="{Binding PopulationDetails}">
    <syncfusion:ComboBoxAdv.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="{Binding Country}" FontWeight="Bold" Width="120"/>
                <TextBlock Text=" - "/>
                <TextBlock Text="{Binding Continent}" Foreground="Gray"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:ComboBoxAdv.ItemTemplate>
</syncfusion:ComboBoxAdv>
```

**Result:** Shows "Indonesia - Asia"

### Advanced ItemTemplate with Images

```xml
<syncfusion:ComboBoxAdv ItemsSource="{Binding Countries}">
    <syncfusion:ComboBoxAdv.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <Image Source="{Binding FlagUrl}" Width="24" Height="16" Margin="0,0,8,0"/>
                <TextBlock Text="{Binding Name}" VerticalAlignment="Center"/>
                <TextBlock Text=" (" VerticalAlignment="Center" Foreground="Gray"/>
                <TextBlock Text="{Binding Code}" VerticalAlignment="Center" Foreground="Gray"/>
                <TextBlock Text=")" VerticalAlignment="Center" Foreground="Gray"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:ComboBoxAdv.ItemTemplate>
</syncfusion:ComboBoxAdv>
```

## Two-Way Binding

Two-way binding updates both UI and ViewModel automatically.

### SelectedItem Two-Way Binding

```xml
<syncfusion:ComboBoxAdv ItemsSource="{Binding Products}"
                        DisplayMemberPath="ProductName"
                        SelectedItem="{Binding SelectedProduct, Mode=TwoWay}"/>

<TextBlock Text="{Binding SelectedProduct.Price, StringFormat='Price: {0:C}'}"/>
```

**Behavior:**
- Selecting item updates `ViewModel.SelectedProduct`
- Changing `SelectedProduct` in ViewModel updates ComboBox

### SelectedItems Two-Way Binding (MultiSelect)

```xml
<syncfusion:ComboBoxAdv AllowMultiSelect="True"
                        ItemsSource="{Binding Countries}"
                        DisplayMemberPath="Name"
                        SelectedItems="{Binding SelectedCountries, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
```

```csharp
private ObservableCollection<object> selectedCountries;

public ObservableCollection<object> SelectedCountries
{
    get { return selectedCountries; }
    set
    {
        selectedCountries = value;
        OnPropertyChanged(nameof(SelectedCountries));
    }
}
```

### UpdateSourceTrigger

Control when binding updates occur:

```xml
<!-- Update on each keystroke (editable mode) -->
<syncfusion:ComboBoxAdv Text="{Binding SearchText, UpdateSourceTrigger=PropertyChanged}"/>

<!-- Update when control loses focus (default) -->
<syncfusion:ComboBoxAdv Text="{Binding SearchText, UpdateSourceTrigger=LostFocus}"/>
```

## ObservableCollection Pattern

### Why ObservableCollection?

`ObservableCollection<T>` notifies the UI when items are added, removed, or the list is refreshed.

```csharp
// ✅ Correct - UI updates automatically
public ObservableCollection<Country> Countries { get; set; }

// ❌ Incorrect - UI doesn't update
public List<Country> Countries { get; set; }
```

### Adding Items Dynamically

```csharp
public class ViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Country> Countries { get; set; }
    public ICommand AddCountryCommand { get; set; }

    public ViewModel()
    {
        Countries = new ObservableCollection<Country>();
        AddCountryCommand = new RelayCommand(AddCountry);
        LoadInitialData();
    }

    private void LoadInitialData()
    {
        Countries.Add(new Country { Name = "Denmark", Code = "DK" });
        Countries.Add(new Country { Name = "Canada", Code = "CA" });
    }

    private void AddCountry()
    {
        Countries.Add(new Country { Name = "New Country", Code = "NC" });
        // UI updates automatically
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

### Removing Items

```csharp
private void RemoveCountry(Country country)
{
    Countries.Remove(country);
    // UI updates automatically
}
```

### Clearing Items

```csharp
private void ClearAll()
{
    Countries.Clear();
    // UI updates automatically
}
```

## INotifyPropertyChanged Implementation

### Full Implementation

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class BaseViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
    
    protected bool SetProperty<T>(ref T field, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(field, value)) return false;
        field = value;
        OnPropertyChanged(propertyName);
        return true;
    }
}

public class ProductViewModel : BaseViewModel
{
    private string searchText;
    private Product selectedProduct;
    
    public string SearchText
    {
        get => searchText;
        set => SetProperty(ref searchText, value);
    }
    
    public Product SelectedProduct
    {
        get => selectedProduct;
        set => SetProperty(ref selectedProduct, value);
    }
    
    public ObservableCollection<Product> Products { get; set; }
}
```

## Advanced Binding Scenarios

### Hierarchical Data Binding

```csharp
public class Category
{
    public string Name { get; set; }
    public ObservableCollection<Product> Products { get; set; }
}

public class ViewModel
{
    public ObservableCollection<Category> Categories { get; set; }
    public ObservableCollection<Product> AllProducts { get; set; }
    
    public ViewModel()
    {
        // Flatten for ComboBoxAdv
        AllProducts = new ObservableCollection<Product>();
        foreach (var category in Categories)
        {
            foreach (var product in category.Products)
            {
                AllProducts.Add(product);
            }
        }
    }
}
```

### Filtered Binding

```csharp
using System.Collections.Generic;
using System.Linq;

public class FilteredViewModel : INotifyPropertyChanged
{
    private string filterText;
    private ObservableCollection<Country> allCountries;
    private ObservableCollection<Country> filteredCountries;
    
    public ObservableCollection<Country> FilteredCountries
    {
        get => filteredCountries;
        set { filteredCountries = value; OnPropertyChanged(nameof(FilteredCountries)); }
    }
    
    public string FilterText
    {
        get => filterText;
        set
        {
            filterText = value;
            OnPropertyChanged(nameof(FilterText));
            ApplyFilter();
        }
    }
    
    private void ApplyFilter()
    {
        if (string.IsNullOrWhiteSpace(FilterText))
        {
            FilteredCountries = new ObservableCollection<Country>(allCountries);
        }
        else
        {
            var filtered = allCountries.Where(c => 
                c.Name.Contains(FilterText, StringComparison.OrdinalIgnoreCase));
            FilteredCountries = new ObservableCollection<Country>(filtered);
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

### Async Data Loading

```csharp
public class AsyncViewModel : INotifyPropertyChanged
{
    private bool isLoading;
    
    public ObservableCollection<Product> Products { get; set; }
    
    public bool IsLoading
    {
        get => isLoading;
        set { isLoading = value; OnPropertyChanged(nameof(IsLoading)); }
    }
    
    public AsyncViewModel()
    {
        Products = new ObservableCollection<Product>();
        LoadDataAsync();
    }
    
    private async Task LoadDataAsync()
    {
        IsLoading = true;
        
        // Simulate API call
        var data = await FetchDataFromApiAsync();
        
        foreach (var item in data)
        {
            Products.Add(item);
        }
        
        IsLoading = false;
    }
    
    private async Task<List<Product>> FetchDataFromApiAsync()
    {
        await Task.Delay(1000); // Simulate delay
        return new List<Product> { /* data */ };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

## Best Practices

### 1. Always Use ObservableCollection for ItemsSource

```csharp
// ✅ Correct
public ObservableCollection<Country> Countries { get; set; }

// ❌ Incorrect
public List<Country> Countries { get; set; }
```

### 2. Implement INotifyPropertyChanged for Two-Way Binding

```csharp
// ✅ Required for SelectedItem binding to work
private Country selectedCountry;
public Country SelectedCountry
{
    get => selectedCountry;
    set { selectedCountry = value; OnPropertyChanged(nameof(SelectedCountry)); }
}
```

### 3. Use Mode=TwoWay for Selection Properties

```xml
<syncfusion:ComboBoxAdv SelectedItem="{Binding SelectedProduct, Mode=TwoWay}"/>
```

### 4. Set DataContext at Appropriate Level

```xml
<!-- Window level for global access -->
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>

<!-- Or Grid level for scoped access -->
<Grid>
    <Grid.DataContext>
        <local:ProductViewModel/>
    </Grid.DataContext>
</Grid>
```

### 5. Use DisplayMemberPath for Simple Displays

```xml
<!-- ✅ Simple and efficient -->
<syncfusion:ComboBoxAdv DisplayMemberPath="Name"/>

<!-- ❌ Overkill for single property -->
<syncfusion:ComboBoxAdv>
    <syncfusion:ComboBoxAdv.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}"/>
        </DataTemplate>
    </syncfusion:ComboBoxAdv.ItemTemplate>
</syncfusion:ComboBoxAdv>
```

## Troubleshooting

### Issue: Items Not Displaying

**Cause:** DataContext not set or ItemsSource binding incorrect

**Solution:**
1. Verify DataContext is set
2. Check property name matches ViewModel exactly (case-sensitive)
3. Use Output window to see binding errors

### Issue: SelectedItem Not Updating ViewModel

**Cause:** Missing `Mode=TwoWay` or `INotifyPropertyChanged` not implemented

**Solution:**
```xml
<syncfusion:ComboBoxAdv SelectedItem="{Binding SelectedItem, Mode=TwoWay}"/>
```

And in ViewModel:
```csharp
public Product SelectedItem
{
    get => selectedItem;
    set { selectedItem = value; OnPropertyChanged(nameof(SelectedItem)); }
}
```

### Issue: Adding Items Doesn't Update UI

**Cause:** Using `List<T>` instead of `ObservableCollection<T>`

**Solution:**
```csharp
// Change from List to ObservableCollection
public ObservableCollection<Product> Products { get; set; }
```

### Issue: Binding Errors in Output Window

**Example:** `System.Windows.Data Error: 40 : BindingExpression path error`

**Solution:** Check:
- Property name spelling and case
- Property is public
- DataContext is set before binding

## See Also

- [Getting Started](getting-started.md) - Basic setup
- [Selection and MultiSelection](selection-multiselection.md) - Selection binding
- [Editing and AutoComplete](editing-autocomplete.md) - Text binding
- [MVVM Pattern Documentation](https://learn.microsoft.com/en-us/dotnet/architecture/maui/mvvm)

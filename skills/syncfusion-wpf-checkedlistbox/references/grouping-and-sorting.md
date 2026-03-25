# Grouping and Sorting Items

## Table of Contents
- [Overview](#overview)
- [Grouping Items](#grouping-items)
- [Sorting Items](#sorting-items)
- [Combining Grouping and Sorting](#combining-grouping-and-sorting)
- [Custom Grouping Logic](#custom-grouping-logic)

## Overview

The CheckListBox provides powerful organization features through grouping and sorting using WPF's CollectionView infrastructure. These features help users navigate large datasets by categorizing items logically and arranging them in meaningful order.

**When to use:**
- Large item collections needing categorization
- Datasets with natural hierarchical structure
- Items need alphabetical or numerical ordering
- Multi-level organization required
- Improve user navigation and findability

**Key concepts:**
- `CollectionView` - WPF's data organization layer
- `GroupDescriptions` - Defines how to group items
- `SortDescriptions` - Defines sort order and direction
- Group headers have checked state based on children
- Expand/collapse groups independently

## Grouping Items

Grouping organizes CheckListBox items into collapsible categories based on property values. By default, items display in a flat list; grouping adds hierarchical structure.

### Basic Grouping Setup

**Step 1: Create model with groupable properties**

```csharp
public class Vegetable
{
    public string Name { get; set; }
    public string Category { get; set; }
    public int Price { get; set; }
}
```

**Step 2: Create ViewModel with LoadedCommand**

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Windows.Data;
using System.Windows.Input;

public class ViewModel
{
    public ObservableCollection<Vegetable> Vegetables { get; set; }
    public ICommand LoadedCommand { get; set; }
    
    public void OnLoaded(object param)
    {
        // Get the default CollectionView for the Vegetables collection
        CollectionView view = (CollectionView)CollectionViewSource.GetDefaultView(Vegetables);
        
        // Add group description - groups by Price property
        view.GroupDescriptions.Add(new PropertyGroupDescription("Price"));
    }
    
    public ViewModel()
    {
        Vegetables = new ObservableCollection<Vegetable>
        {
            new Vegetable { Price = 10, Name = "Yarrow", Category = "Leafy and Salad" },
            new Vegetable { Price = 20, Name = "Cabbage", Category = "Leafy and Salad" },
            new Vegetable { Price = 30, Name = "Horse gram", Category = "Beans" },
            new Vegetable { Price = 20, Name = "Green bean", Category = "Beans" },
            new Vegetable { Price = 10, Name = "Onion", Category = "Bulb and Stem" },
            new Vegetable { Price = 30, Name = "Nopal", Category = "Bulb and Stem" }
        };
        
        // Initialize command using a command framework (e.g., Prism's DelegateCommand)
        LoadedCommand = new DelegateCommand<object>(OnLoaded);
    }
}
```

**Step 3: Bind in XAML with Loaded trigger**

```xml
<Window xmlns:i="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:local="clr-namespace:YourNamespace">
    
    <Grid>
        <syncfusion:CheckListBox ItemsSource="{Binding Vegetables}"
                                 DisplayMemberPath="Name">
            <syncfusion:CheckListBox.DataContext>
                <local:ViewModel />
            </syncfusion:CheckListBox.DataContext>
            
            <!-- Trigger grouping when control loads -->
            <i:Interaction.Triggers>
                <i:EventTrigger EventName="Loaded">
                    <i:InvokeCommandAction Command="{Binding LoadedCommand}" />
                </i:EventTrigger>
            </i:Interaction.Triggers>
        </syncfusion:CheckListBox>
    </Grid>
</Window>
```

**Required:** Install `Microsoft.Xaml.Behaviors.Wpf` NuGet package for `i:Interaction.Triggers`.

**Result:** Items grouped by Price (10, 20, 30 groups).

### Alternative: Group in Constructor

You can also apply grouping directly in the constructor without using Loaded event:

```csharp
public ViewModel()
{
    Vegetables = new ObservableCollection<Vegetable>
    {
        // ... populate items
    };
    
    // Apply grouping immediately
    var view = CollectionViewSource.GetDefaultView(Vegetables);
    view.GroupDescriptions.Add(new PropertyGroupDescription("Category"));
}
```

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Vegetables}"
                         DisplayMemberPath="Name">
    <syncfusion:CheckListBox.DataContext>
        <local:ViewModel />
    </syncfusion:CheckListBox.DataContext>
</syncfusion:CheckListBox>
```

### Group Header Behavior

**Checked State:**
- **Unchecked** - No child items are checked
- **Intermediate** - Some (but not all) children checked
- **Checked** - All child items are checked

**User interactions:**
- Click group header checkbox → Check/uncheck all children
- Check/uncheck children → Group header state updates automatically
- Space key on focused group header → Toggle all children

**Expand/Collapse:**
- Click group header text to expand/collapse
- Groups remember expanded state
- All groups collapsed by default (can be configured)

### Nested Grouping (Multi-Level)

Create hierarchical groups by adding multiple PropertyGroupDescription objects. The order determines nesting levels.

```csharp
public void OnLoaded(object param)
{
    CollectionView view = (CollectionView)CollectionViewSource.GetDefaultView(Vegetables);
    
    // First level: Group by Category
    view.GroupDescriptions.Add(new PropertyGroupDescription("Category"));
    
    // Second level: Within each category, group by Price
    view.GroupDescriptions.Add(new PropertyGroupDescription("Price"));
}
```

**Sample data for nested grouping:**

```csharp
public ViewModel()
{
    Vegetables = new ObservableCollection<Vegetable>
    {
        // Leafy and Salad
        new Vegetable { Price = 10, Name = "Yarrow", Category = "Leafy and Salad" },
        new Vegetable { Price = 20, Name = "Pumpkins", Category = "Leafy and Salad" },
        new Vegetable { Price = 30, Name = "Cabbage", Category = "Leafy and Salad" },
        new Vegetable { Price = 10, Name = "Spinach", Category = "Leafy and Salad" },
        new Vegetable { Price = 20, Name = "Wheat Grass", Category = "Leafy and Salad" },
        
        // Beans
        new Vegetable { Price = 10, Name = "Horse gram", Category = "Beans" },
        new Vegetable { Price = 20, Name = "Chickpea", Category = "Beans" },
        new Vegetable { Price = 30, Name = "Green bean", Category = "Beans" },
        
        // Bulb and Stem
        new Vegetable { Price = 10, Name = "Garlic", Category = "Bulb and Stem" },
        new Vegetable { Price = 20, Name = "Onion", Category = "Bulb and Stem" },
        new Vegetable { Price = 30, Name = "Nopal", Category = "Bulb and Stem" }
    };
    
    LoadedCommand = new DelegateCommand<object>(OnLoaded);
}
```

**Structure:**
```
▼ Leafy and Salad
  ▼ 10
    ☐ Yarrow
    ☐ Spinach
  ▼ 20
    ☐ Pumpkins
    ☐ Wheat Grass
  ▼ 30
    ☐ Cabbage
▼ Beans
  ▼ 10
    ☐ Horse gram
  ...
```

## Sorting Items

Sorting arranges items in ascending or descending order based on property values. Items are sorted alphabetically, numerically, or by date depending on the property type.

### Basic Sorting Setup

```csharp
public class ViewModel
{
    public ObservableCollection<Vegetable> Vegetables { get; set; }
    public ICommand LoadedCommand { get; set; }
    
    public void OnLoaded(object param)
    {
        CollectionView view = (CollectionView)CollectionViewSource.GetDefaultView(Vegetables);
        
        // Add sort description
        view.SortDescriptions.Add(new SortDescription
        {
            PropertyName = "Name",
            Direction = ListSortDirection.Ascending
        });
    }
    
    public ViewModel()
    {
        Vegetables = new ObservableCollection<Vegetable>
        {
            new Vegetable { Price = 10, Name = "Yarrow", Category = "Leafy and Salad" },
            new Vegetable { Price = 20, Name = "Cabbage", Category = "Leafy and Salad" },
            new Vegetable { Price = 30, Name = "Horse gram", Category = "Beans" },
            new Vegetable { Price = 20, Name = "Green bean", Category = "Beans" },
            new Vegetable { Price = 10, Name = "Onion", Category = "Bulb and Stem" },
            new Vegetable { Price = 30, Name = "Nopal", Category = "Bulb and Stem" }
        };
        
        LoadedCommand = new DelegateCommand<object>(OnLoaded);
    }
}
```

**XAML:**

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Vegetables}"
                         DisplayMemberPath="Name">
    <syncfusion:CheckListBox.DataContext>
        <local:ViewModel />
    </syncfusion:CheckListBox.DataContext>
    
    <i:Interaction.Triggers>
        <i:EventTrigger EventName="Loaded">
            <i:InvokeCommandAction Command="{Binding LoadedCommand}" />
        </i:EventTrigger>
    </i:Interaction.Triggers>
</syncfusion:CheckListBox>
```

**Result:** Items sorted alphabetically by Name (A→Z).

### Sort Directions

```csharp
// Ascending (A→Z, 0→9, oldest→newest)
view.SortDescriptions.Add(new SortDescription
{
    PropertyName = "Name",
    Direction = ListSortDirection.Ascending
});

// Descending (Z→A, 9→0, newest→oldest)
view.SortDescriptions.Add(new SortDescription
{
    PropertyName = "Price",
    Direction = ListSortDirection.Descending
});
```

### Multiple Sort Criteria

Add multiple SortDescription objects to create multi-level sorting. Items are sorted by first criterion, then by second for equal values, etc.

```csharp
public void OnLoaded(object param)
{
    CollectionView view = (CollectionView)CollectionViewSource.GetDefaultView(Vegetables);
    
    // Primary sort: Category (A→Z)
    view.SortDescriptions.Add(new SortDescription
    {
        PropertyName = "Category",
        Direction = ListSortDirection.Ascending
    });
    
    // Secondary sort: Price (Low→High within each category)
    view.SortDescriptions.Add(new SortDescription
    {
        PropertyName = "Price",
        Direction = ListSortDirection.Ascending
    });
    
    // Tertiary sort: Name (A→Z within same category and price)
    view.SortDescriptions.Add(new SortDescription
    {
        PropertyName = "Name",
        Direction = ListSortDirection.Ascending
    });
}
```

**Result:**
```
Beans
  Chickpea (20)
  Green bean (30)
  Horse gram (10)
Bulb and Stem
  Garlic (10)
  Nopal (30)
  Onion (20)
...
```

## Combining Grouping and Sorting

Apply both grouping and sorting to create organized, sorted groups. Sorting applies within each group.

### Sort Within Groups

```csharp
public void OnLoaded(object param)
{
    CollectionView view = (CollectionView)CollectionViewSource.GetDefaultView(Vegetables);
    
    // Group by Category
    view.GroupDescriptions.Add(new PropertyGroupDescription("Category"));
    
    // Sort by Name within each group
    view.SortDescriptions.Add(new SortDescription
    {
        PropertyName = "Name",
        Direction = ListSortDirection.Ascending
    });
}
```

**Result:**
```
▼ Beans (alphabetically sorted)
  ☐ Chickpea
  ☐ Green bean
  ☐ Horse gram
▼ Bulb and Stem (alphabetically sorted)
  ☐ Garlic
  ☐ Nopal
  ☐ Onion
...
```

### Sort Groups and Items

```csharp
public void OnLoaded(object param)
{
    CollectionView view = (CollectionView)CollectionViewSource.GetDefaultView(Vegetables);
    
    // Group by Category
    view.GroupDescriptions.Add(new PropertyGroupDescription("Category"));
    
    // Sort groups by Category name
    view.SortDescriptions.Add(new SortDescription
    {
        PropertyName = "Category",
        Direction = ListSortDirection.Ascending
    });
    
    // Within groups, sort by Price then Name
    view.SortDescriptions.Add(new SortDescription
    {
        PropertyName = "Price",
        Direction = ListSortDirection.Ascending
    });
    
    view.SortDescriptions.Add(new SortDescription
    {
        PropertyName = "Name",
        Direction = ListSortDirection.Ascending
    });
}
```

### Complete Example with Nested Groups and Sorting

```csharp
public void OnLoaded(object param)
{
    CollectionView view = (CollectionView)CollectionViewSource.GetDefaultView(Vegetables);
    
    // Level 1: Group by Category
    view.GroupDescriptions.Add(new PropertyGroupDescription("Category"));
    
    // Level 2: Group by Price within Category
    view.GroupDescriptions.Add(new PropertyGroupDescription("Price"));
    
    // Sort: Category (A→Z), then Price (Low→High), then Name (A→Z)
    view.SortDescriptions.Add(new SortDescription("Category", ListSortDirection.Ascending));
    view.SortDescriptions.Add(new SortDescription("Price", ListSortDirection.Ascending));
    view.SortDescriptions.Add(new SortDescription("Name", ListSortDirection.Ascending));
}
```

## Custom Grouping Logic

Use `IValueConverter` to create custom grouping logic when PropertyGroupDescription doesn't meet your needs.

### Scenario: Group by Date Ranges

Group employees by birth year ranges and quarters.

**Step 1: Create model**

```csharp
public class Employee
{
    public string Name { get; set; }
    public int Age { get; set; }
    public DateTime DOB { get; set; }
}

public class EmployeesCollection : ObservableCollection<Employee>
{
}
```

**Step 2: Create custom converter**

```csharp
using System;
using System.Globalization;
using System.Windows.Data;

public class DateConverter : IValueConverter
{
    public string Category { get; set; }
    
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null) return "Others";
        
        DateTime date;
        if (!DateTime.TryParse(value.ToString(), out date))
            return "Others";
        
        if (Category == "Year")
        {
            // Group by decade
            if (date >= new DateTime(1980, 1, 1) && date <= new DateTime(1990, 12, 31))
                return "Birth Year (1980-1990)";
            else if (date >= new DateTime(1991, 1, 1) && date <= new DateTime(2000, 12, 31))
                return "Birth Year (1991-2000)";
            else
                return "Others";
        }
        else if (Category == "Month")
        {
            // Sub-group by quarter
            if (date.Month >= 1 && date.Month <= 3)
                return "Quarter-1 (Jan-Mar)";
            else if (date.Month >= 4 && date.Month <= 6)
                return "Quarter-2 (Apr-Jun)";
            else if (date.Month >= 7 && date.Month <= 9)
                return "Quarter-3 (Jul-Sep)";
            else
                return "Quarter-4 (Oct-Dec)";
        }
        
        return "Others";
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

**Step 3: Create ViewModel**

```csharp
public class ViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    private ObservableCollection<GroupDescription> _groupDescriptions;
    private EmployeesCollection _employees;
    
    public ObservableCollection<GroupDescription> GroupDescriptions
    {
        get => _groupDescriptions;
        set
        {
            _groupDescriptions = value;
            OnPropertyChanged(nameof(GroupDescriptions));
        }
    }
    
    public EmployeesCollection Employees
    {
        get => _employees;
        set
        {
            _employees = value;
            OnPropertyChanged(nameof(Employees));
        }
    }
    
    public ViewModel()
    {
        Employees = new EmployeesCollection();
        GroupDescriptions = new ObservableCollection<GroupDescription>();
    }
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**Step 4: Use in XAML**

```xml
<Window.Resources>
    <!-- Define employee data -->
    <local:EmployeesCollection x:Key="employees">
        <local:Employee Name="Daniel" DOB="02/15/1983" Age="37" />
        <local:Employee Name="James" DOB="06/29/1988" Age="32" />
        <local:Employee Name="Michael" DOB="06/10/1987" Age="33" />
        <local:Employee Name="John" DOB="12/22/1990" Age="30" />
        <local:Employee Name="Paul" DOB="04/08/1997" Age="23" />
        <local:Employee Name="Mark" DOB="08/05/1994" Age="26" />
        <local:Employee Name="George" DOB="10/01/1998" Age="22" />
    </local:EmployeesCollection>
    
    <!-- Define converters -->
    <local:DateConverter Category="Year" x:Key="yearConverter" />
    <local:DateConverter Category="Month" x:Key="monthConverter" />
    
    <!-- Define ViewModel with custom group descriptions -->
    <local:ViewModel x:Key="viewModel">
        <local:ViewModel.GroupDescriptions>
            <!-- Level 1: Group by birth year range -->
            <PropertyGroupDescription PropertyName="DOB" 
                                      Converter="{StaticResource yearConverter}" />
            <!-- Level 2: Group by quarter within year range -->
            <PropertyGroupDescription PropertyName="DOB"
                                      Converter="{StaticResource monthConverter}" />
        </local:ViewModel.GroupDescriptions>
    </local:ViewModel>
</Window.Resources>

<Grid>
    <syncfusion:CheckListBox ItemsSource="{StaticResource employees}"
                             GroupDescriptions="{Binding GroupDescriptions}"
                             DataContext="{StaticResource viewModel}"
                             DisplayMemberPath="Name"
                             Margin="20" />
</Grid>
```

**Result:**
```
▼ Birth Year (1980-1990)
  ▼ Quarter-1 (Jan-Mar)
    ☐ Daniel
  ▼ Quarter-2 (Apr-Jun)
    ☐ James
    ☐ Michael
  ▼ Quarter-4 (Oct-Dec)
    ☐ John
▼ Birth Year (1991-2000)
  ▼ Quarter-2 (Apr-Jun)
    ☐ Paul
  ▼ Quarter-3 (Jul-Sep)
    ☐ Mark
  ▼ Quarter-4 (Oct-Dec)
    ☐ George
```

### Other Custom Grouping Scenarios

**Price ranges:**
```csharp
if (price < 10) return "Budget ($0-$9)";
else if (price < 20) return "Standard ($10-$19)";
else return "Premium ($20+)";
```

**Alphabetical sections:**
```csharp
char firstLetter = name[0];
if (firstLetter >= 'A' && firstLetter <= 'M') return "A-M";
else return "N-Z";
```

**Status categories:**
```csharp
if (status == "Active" || status == "Pending") return "Current";
else return "Archived";
```

## Troubleshooting

**Issue: Grouping not appearing**
- Verify `CollectionView.GroupDescriptions.Add()` is called
- Check PropertyName matches model property exactly (case-sensitive)
- Ensure Loaded event fires (add breakpoint in OnLoaded)
- Verify Microsoft.Xaml.Behaviors.Wpf package is installed

**Issue: Sorting not working**
- Check PropertyName spelling
- Ensure property has public getter
- Verify CollectionView is correctly retrieved
- Try removing grouping to isolate issue

**Issue: Custom grouping returns "Others" for all items**
- Debug converter Convert method
- Check value parameter type
- Verify Category property is set on converter instance
- Ensure date parsing succeeds

**Issue: Group header state not updating**
- This is automatic - check if children are actually checking
- Verify ItemContainerStyle IsSelected binding (if using data binding)
- Check for virtualization conflicts

## Next Steps

- **Customize Appearance:** [appearance-customization.md](appearance-customization.md)
- **Optimize Performance:** [virtualization.md](virtualization.md) (important for grouped large datasets)
- **Check Management:** [checking-and-selection.md](checking-and-selection.md)

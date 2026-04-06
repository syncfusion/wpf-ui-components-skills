# Data Model and Binding

## Table of Contents
- [Overview](#overview)
- [Creating the Data Model](#creating-the-data-model)
- [Creating the ViewModel](#creating-the-viewmodel)
- [Binding Data to the Grid](#binding-data-to-the-grid)
- [Column Configuration](#column-configuration)
- [CurrentUser Configuration](#currentuser-configuration)
- [Complete Example](#complete-example)

## Overview

The SfSmartDataGrid is a data-bound control. Before binding data to the control, you must create a data model that represents your data objects. This guide covers:
- Creating data object classes with property change notifications
- Building a ViewModel with ObservableCollection
- Binding the ItemsSource to your data
- Configuring columns manually or automatically
- Setting up CurrentUser for AssistView message differentiation

## Creating the Data Model

A data model is a class that represents the structure of your data. For the Smart DataGrid, it's recommended to implement `INotifyPropertyChanged` to enable automatic UI updates when property values change.

### Step 1: Define the Data Object Class

Create a class that represents a single record in your dataset. For example, an `OrderInfo` class:

```csharp
using System;
using System.ComponentModel;
using System.Runtime.CompilerServices;

public partial class OrderInfo : INotifyPropertyChanged
{ 
    private int _orderID;
    private string _customerName = string.Empty;
    private string _productName = string.Empty;
    private DateTime _orderDate;
    private int _quantity;
    private double _freight;
    private string _shipCountry = string.Empty;
    private string _shipCity = string.Empty;
    private string _paymentStatus = string.Empty;

    public event PropertyChangedEventHandler PropertyChanged;

    protected bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Equals(storage, value))
            return false;
        storage = value;
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        return true;
    }

    public int OrderID
    {
        get => _orderID;
        set => SetProperty(ref _orderID, value);
    }

    public string CustomerName
    {
        get => _customerName;
        set => SetProperty(ref _customerName, value);
    }

    public string ProductName
    {
        get => _productName;
        set => SetProperty(ref _productName, value);
    }

    public DateTime OrderDate
    {
        get => _orderDate;
        set => SetProperty(ref _orderDate, value);
    }

    public int Quantity
    {
        get => _quantity;
        set => SetProperty(ref _quantity, value);
    }

    public double Freight
    {
        get => _freight;
        set => SetProperty(ref _freight, value);
    }

    public string ShipCountry
    {
        get => _shipCountry;
        set => SetProperty(ref _shipCountry, value);
    }

    public string ShipCity
    {
        get => _shipCity;
        set => SetProperty(ref _shipCity, value);
    }

    public string PaymentStatus
    {
        get => _paymentStatus;
        set => SetProperty(ref _paymentStatus, value);
    }

    public OrderInfo() { }

    public OrderInfo(int orderId,
                     string customerName,
                     string productName,
                     DateTime orderDate,
                     int quantity,
                     double freight,
                     string shipCountry,
                     string shipCity,
                     string paymentStatus)
    {
        _orderID = orderId;
        _customerName = customerName;
        _productName = productName;
        _orderDate = orderDate;
        _quantity = quantity;
        _freight = freight;
        _shipCountry = shipCountry;
        _shipCity = shipCity;
        _paymentStatus = paymentStatus;
    }
}
```

### Key Elements of the Data Model

1. **INotifyPropertyChanged Interface:**
   - Enables automatic UI updates when property values change
   - Required for two-way data binding and real-time updates

2. **SetProperty Helper Method:**
   - Simplifies property setters
   - Automatically raises PropertyChanged event
   - Prevents unnecessary notifications when value hasn't changed

3. **Private Backing Fields:**
   - Store the actual property values
   - Convention: prefix with underscore (e.g., `_orderID`)

4. **Public Properties:**
   - Exposed to the UI and grid
   - Use `SetProperty` in setters to trigger notifications

5. **Constructors:**
   - Default constructor for object initialization
   - Parameterized constructor for easy object creation with values

### Property Types

The data model can include various property types that correspond to different column types:

| Property Type | Grid Column Type | Example |
|---------------|------------------|---------|
| `string` | GridTextColumn | CustomerName, ProductName |
| `int`, `double`, `decimal` | GridNumericColumn | OrderID, Quantity, Freight |
| `DateTime` | GridDateTimeColumn | OrderDate |
| `bool` | GridCheckBoxColumn | IsActive |

## Creating the ViewModel

A ViewModel holds the data collection and provides it to the view. It also implements `INotifyPropertyChanged` for dynamic property updates.

### Step 2: Define the ViewModel Class

```csharp
using System;
using System.Collections.ObjectModel;
using System.ComponentModel;

public class ViewModel : INotifyPropertyChanged
{ 
    public ObservableCollection<OrderInfo> OrderInfoCollection { get; set; }

    private Author currentUser;

    public Author CurrentUser
    {
        get
        {
            return this.currentUser;
        }
        set
        {
            this.currentUser = value;
            RaisePropertyChanged("CurrentUser");
        }
    }

    public ObservableCollection<string> AiSuggestions { get; } = new ObservableCollection<string>
    {
        "Which orders have a payment status of Not Paid?",
        "What are the top 10 orders with the highest freight cost?",
        "Which customers have placed the most orders?",
        "What are the orders shipped to Brazil?",
        "What is the total quantity of products ordered across all orders?",
    };

    public event PropertyChangedEventHandler PropertyChanged;
    
    public void RaisePropertyChanged(string propName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
    }

    public ViewModel()
    {
        this.CurrentUser = new Author { Name = "John" };
        OrderInfoCollection = new ObservableCollection<OrderInfo>();
        GenerateOrders();
    }

    private Random random = new Random();
    
    private static readonly string[] Names = new[]
    {
        "Emma", "Olivia", "Charlotte", "Amelia", "Sophia",
        "William", "Isabella", "Emily", "George", "Grace",
        "Lily","Ava","Chloe","Ella","Mia","James","Sophie",
        "Isla","Henry","Lucy"
    };

    private static readonly string[] Countries = new[]
    {
        "Spain","Brazil","Switzerland","France","Germany","UK","Canada","Mexico","USA","Italy"
    };

    public string[] ElectronicsProducts = new string[]
    {
        "Keyboard", "Mouse", "Trackpad", "Stylus", "Scanner", 
        "Webcam", "Microphone", "Monitor", "Speakers", "Headphones", 
        "Printers", "Projectors", "External Drive", "UPS"
    };

    private static readonly string[] Cities = new[]
    {
        "Madrid","Rio","Bern","Paris","Berlin","London","Toronto","Mexico D.F.","Seattle","Rome"
    };

    private static readonly string[] PaymentStatuses = new[] { "Paid", "Not Paid" };

    private void GenerateOrders()
    {
        var rnd = new Random(42);
        int baseOrderId = 20251001;

        for (int i = 0; i < 100; i++)
        {
            int orderId = baseOrderId + i;
            var name = Names[i % Names.Length];
            string productName = ElectronicsProducts[random.Next(ElectronicsProducts.Length)];
            DateTime orderDate = DateTime.Today.AddDays(-rnd.Next(1, 90));

            // Quantity realistic
            int quantity = rnd.Next(1, 20);

            // Freight based on product type
            int freight;
            if (productName == "Mouse" || productName == "Stylus" || productName == "Trackpad")
                freight = rnd.Next(50, 150);
            else if (productName == "Keyboard" || productName == "Webcam" || productName == "Microphone")
                freight = rnd.Next(150, 350);
            else
                freight = rnd.Next(350, 750);

            // Country and city mapping
            var country = Countries[i % Countries.Length];
            var city = Cities[i % Cities.Length];

            // Payment status with realistic ratio
            var status = rnd.NextDouble() < 0.7 ? "Paid" : "Not Paid";

            OrderInfoCollection.Add(new OrderInfo(
                orderId: orderId,
                customerName: name,
                productName: productName,
                orderDate: orderDate,
                quantity: quantity,
                freight: freight,
                shipCountry: country,
                shipCity: city,
                paymentStatus: status));
        }
    }
}
```

### Key Elements of the ViewModel

1. **ObservableCollection:**
   - Use `ObservableCollection<T>` instead of `List<T>`
   - Automatically notifies the UI when items are added/removed
   - Essential for dynamic data updates

2. **CurrentUser Property:**
   - Required for AssistView message differentiation
   - Identifies outgoing requests (from user) vs incoming responses (from AI)
   - Type: `Author` with `Name` property

3. **AiSuggestions Property:**
   - Provides predefined prompts for users
   - Shown in the AssistView as quick-select options
   - Helps guide users toward common operations

4. **Data Generation Method:**
   - Populates the collection with sample data
   - Can be replaced with database queries, API calls, etc.

## Binding Data to the Grid

### Step 3: Set DataContext and Bind ItemsSource

**In XAML:**

```xml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="WpfApplication1.MainWindow"
        xmlns:local="clr-namespace:WpfApplication1"
        Title="MainWindow" Height="600" Width="900">
		
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>

    <Grid x:Name="Root_Grid">
        <syncfusion:SfSmartDataGrid x:Name="SmartGrid" 
                                    ItemsSource="{Binding OrderInfoCollection}" 
                                    CurrentUser="{Binding CurrentUser}" 
                                    Suggestions="{Binding AiSuggestions}"/>
    </Grid>
</Window>
```

**In Code-Behind:**

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        ViewModel viewModel = new ViewModel();
        this.DataContext = viewModel;
        
        // Or set ItemsSource directly
        SmartGrid.ItemsSource = viewModel.OrderInfoCollection;
        SmartGrid.CurrentUser = viewModel.CurrentUser;
    }
}
```

### Binding Essentials

1. **Set DataContext:** Establishes the data source for bindings
2. **Bind ItemsSource:** Connects the grid to the data collection
3. **Bind CurrentUser:** Required for AssistView functionality
4. **Bind Suggestions (Optional):** Provides predefined prompts

## Column Configuration

The Smart DataGrid supports two approaches for defining columns:

### Option 1: AutoGenerateColumns

Let the grid automatically create columns based on data model properties:

```xml
<syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}" 
                            AutoGenerateColumns="True"/>
```

**Pros:**
- Quick setup, no manual column definitions needed
- Automatically reflects data model changes

**Cons:**
- Less control over column appearance and formatting
- All public properties are shown as columns

### Option 2: Manual Column Definitions

Define columns explicitly for full control:

```xml
<syncfusion:SfSmartDataGrid x:Name="SmartGrid" 
                            ItemsSource="{Binding OrderInfoCollection}" 
                            AutoGenerateColumns="False" 
                            CurrentUser="{Binding CurrentUser}" 
                            Suggestions="{Binding AiSuggestions}" 
                            ColumnSizer="Star">
    <syncfusion:SfSmartDataGrid.Columns>
        <syncfusion:GridNumericColumn MappingName="OrderID" 
                                      HeaderText="Order ID" 
                                      NumberDecimalDigits="0" 
                                      TextAlignment="Center"/>
        
        <syncfusion:GridTextColumn MappingName="CustomerName" 
                                   HeaderText="Customer Name" 
                                   TextAlignment="Center"/>

        <syncfusion:GridTextColumn MappingName="ProductName" 
                                   HeaderText="Product Name" 
                                   TextAlignment="Center"/>

        <syncfusion:GridDateTimeColumn MappingName="OrderDate" 
                                       HeaderText="Order Date" 
                                       TextAlignment="Center"/>

        <syncfusion:GridNumericColumn MappingName="Quantity" 
                                      NumberDecimalDigits="0" 
                                      TextAlignment="Center"/>

        <syncfusion:GridNumericColumn MappingName="Freight" 
                                      TextAlignment="Center"/>

        <syncfusion:GridTextColumn MappingName="ShipCity" 
                                   HeaderText="Ship City" 
                                   TextAlignment="Center"/>

        <syncfusion:GridTextColumn MappingName="ShipCountry" 
                                   HeaderText="Ship Country" 
                                   TextAlignment="Center"/>

        <syncfusion:GridTextColumn MappingName="PaymentStatus" 
                                   HeaderText="Payment Status" 
                                   TextAlignment="Center"/>
    </syncfusion:SfSmartDataGrid.Columns>
</syncfusion:SfSmartDataGrid>
```

### Column Types and Properties

| Column Type | Purpose | Key Properties |
|-------------|---------|----------------|
| **GridTextColumn** | Display text data | MappingName, HeaderText, TextAlignment |
| **GridNumericColumn** | Display numbers | MappingName, HeaderText, NumberDecimalDigits |
| **GridDateTimeColumn** | Display dates/times | MappingName, HeaderText, Format |
| **GridCheckBoxColumn** | Display boolean values | MappingName, HeaderText |

**Common Properties:**
- `MappingName` – Property name from data model (required)
- `HeaderText` – Column header display text
- `TextAlignment` – Cell content alignment (Left, Center, Right)
- `Width` – Column width (absolute or Auto)

## CurrentUser Configuration

**IMPORTANT:** The `CurrentUser` property is required for AssistView to differentiate between outgoing requests (from the user) and incoming responses (from AI).

### Why CurrentUser is Required

- Without `CurrentUser`, all messages appear with the same alignment and style
- The control cannot distinguish between user messages and AI responses
- AssistView layout depends on this property for proper message display

### Setting CurrentUser

**In ViewModel:**
```csharp
public Author CurrentUser { get; set; }

public ViewModel()
{
    this.CurrentUser = new Author { Name = "John" };
}
```

**In XAML Binding:**
```xml
<syncfusion:SfSmartDataGrid CurrentUser="{Binding CurrentUser}"/>
```

**In Code-Behind:**
```csharp
SmartGrid.CurrentUser = new Author { Name = "John" };
```

## Complete Example

Here's a complete example combining all elements:

**OrderInfo.cs:**
```csharp
public partial class OrderInfo : INotifyPropertyChanged
{
    // (Full implementation shown in Creating the Data Model section)
}
```

**ViewModel.cs:**
```csharp
public class ViewModel : INotifyPropertyChanged
{
    public ObservableCollection<OrderInfo> OrderInfoCollection { get; set; }
    public Author CurrentUser { get; set; }
    public ObservableCollection<string> AiSuggestions { get; }
    
    public ViewModel()
    {
        CurrentUser = new Author { Name = "John" };
        OrderInfoCollection = new ObservableCollection<OrderInfo>();
        AiSuggestions = new ObservableCollection<string>
        {
            "Which orders have a payment status of Not Paid?",
            "What are the top 10 orders with the highest freight cost?"
        };
        GenerateOrders();
    }
    
    private void GenerateOrders()
    {
        // Generate sample data
    }
}
```

**MainWindow.xaml:**
```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        xmlns:local="clr-namespace:WpfApplication1">
    <Window.DataContext>
        <local:ViewModel/>
    </Window.DataContext>

    <syncfusion:SfSmartDataGrid ItemsSource="{Binding OrderInfoCollection}" 
                                CurrentUser="{Binding CurrentUser}" 
                                Suggestions="{Binding AiSuggestions}"
                                AutoGenerateColumns="False"
                                EnableSmartActions="True">
        <syncfusion:SfSmartDataGrid.Columns>
            <!-- Column definitions -->
        </syncfusion:SfSmartDataGrid.Columns>
    </syncfusion:SfSmartDataGrid>
</Window>
```

## Best Practices

1. **Always implement INotifyPropertyChanged** for data models used with Smart DataGrid
2. **Use ObservableCollection** instead of List for dynamic collections
3. **Set CurrentUser** to enable proper AssistView message differentiation
4. **Define manual columns** when you need control over formatting and display
5. **Use appropriate column types** for different data types (Numeric, DateTime, Text)
6. **Provide meaningful HeaderText** for better user experience
7. **Add AiSuggestions** to guide users toward common operations

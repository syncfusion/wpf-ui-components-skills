# Data Source Configuration

## Table of Contents
- [OLAP Data Source Overview](#olap-data-source-overview)
- [Connection Strings](#connection-strings)
- [OlapDataManager](#olapdatamanager)
- [OlapReport Structure](#olapreport-structure)
- [Dimension Configuration](#dimension-configuration)
- [Measure Configuration](#measure-configuration)
- [Complete Configuration Examples](#complete-configuration-examples)

## OLAP Data Source Overview

The WPF OLAP Grid connects to OLAP data sources through the `OlapDataManager` class, which manages the connection and data retrieval. The grid supports multiple OLAP providers:

- **Microsoft SQL Server Analysis Services (SSAS)** - Local or remote servers
- **Offline Cubes** - .cub files stored locally
- **Mondrian Server** - Open-source OLAP server via XML/A
- **ActivePivot Server** - Via XML/A protocol

The data displayed in the grid is defined by an `OlapReport`, which specifies:
- Which cube to query
- Which dimensions to display (rows and columns)
- Which measures to show
- How to organize the data

## Connection Strings

Connection strings define how to connect to your OLAP data source. The format varies based on the provider.

### Offline Cube Connection

Use when you have a local .cub file containing OLAP data.

**Pattern:**
```csharp
string connectionString = @"DataSource = <path_to_cube_file>; Provider = MSOLAP;";
```

**Example:**
```csharp
string connectionString = @"DataSource = C:\OfflineCube\Adventure_Works_Ext.cub; Provider = MSOLAP;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

**When to use:**
- Working with offline data
- Testing without server access
- Distributing pre-built cubes with applications
- Performance optimization for static data

### Local SQL Server (SSAS) Connection

Use when connecting to SQL Server Analysis Services on your local machine.

**Pattern:**
```csharp
string connectionString = "Data Source=<server>; Initial Catalog=<database>;";
```

**Example:**
```csharp
string connectionString = "Data Source=localhost; Initial Catalog=Adventure Works DW;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

**With Credentials:**
```csharp
string connectionString = "Data Source=localhost; Initial Catalog=Adventure Works DW; " +
                         "User ID=username; Password=password;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

**When to use:**
- Development and testing on local SSAS
- Single-machine deployments
- Internal network servers (use machine name instead of localhost)

### Remote SQL Server (SSAS) via XML/A

Use when connecting to SSAS hosted on a remote server accessible via HTTP/HTTPS.

**Pattern:**
```csharp
string connectionString = "Data Source=<http_url>; Initial Catalog=<database>;";
```

**Example:**
```csharp
string connectionString = "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " +
                         "Initial Catalog=Adventure Works DW 2008 SE;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

**With Credentials:**
```csharp
string connectionString = "Data Source=http://bi.company.com/olap/msmdpump.dll; " +
                         "Initial Catalog=Sales_DW; " +
                         "User ID=analyst; Password=secure123;";
```

**When to use:**
- Cloud-hosted OLAP servers
- Remote data centers
- Cross-network OLAP access
- Internet-based BI solutions

### Mondrian Server Connection

Use when connecting to Mondrian (open-source OLAP server) via XML/A.

**Pattern:**
```csharp
string connectionString = @"Data Source = <mondrian_url>; Initial Catalog = <catalog>;";
```

**Example:**
```csharp
string connectionString = @"Data Source = http://localhost:8080/mondrian/xmla; Initial Catalog = FoodMart;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
// IMPORTANT: Set provider name for Mondrian
dataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.Mondrian;
```

**With Credentials:**
```csharp
string connectionString = @"Data Source = http://server:8080/mondrian/xmla; " +
                          @"Initial Catalog = FoodMart; " +
                          @"User ID = admin; Password = admin;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
dataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.Mondrian;
```

**Critical:** Always set `ProviderName` to `Mondrian` when using Mondrian servers.

### ActivePivot Server Connection

Use when connecting to ActivePivot server via XML/A.

**Pattern:**
```csharp
string connectionString = @"Data Source = <activepivot_url>; Initial Catalog = <catalog>;";
```

**Example:**
```csharp
string connectionString = @"Data Source = http://localhost:8080/cva_s/xmla; Initial Catalog = CVAS;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
// IMPORTANT: Set provider name for ActivePivot
dataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.ActivePivot;
```

**With Credentials:**
```csharp
string connectionString = @"Data Source = http://server:8080/cva_s/xmla; " +
                          @"Initial Catalog = CVAS; " +
                          @"User ID = user; Password = pass;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
dataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.ActivePivot;
```

**Critical:** Always set `ProviderName` to `ActivePivot` when using ActivePivot servers.

## OlapDataManager

The `OlapDataManager` is the central class that manages OLAP connections and data operations.

### Initialization

```csharp
// Basic initialization
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

### Setting Reports

```csharp
// Create and set the report
OlapReport report = CreateMyReport();
dataManager.SetCurrentReport(report);
```

### Binding to Grid

```csharp
// Connect to OlapGrid
olapGrid1.OlapDataManager = dataManager;
olapGrid1.DataBind();
```

### Complete Workflow

```csharp
public void InitializeOlapGrid()
{
    // Step 1: Create connection
    string connectionString = "Data Source=localhost; Initial Catalog=AdventureWorksDW;";
    OlapDataManager dataManager = new OlapDataManager(connectionString);
    
    // Step 2: Create and set report
    OlapReport report = BuildReport();
    dataManager.SetCurrentReport(report);
    
    // Step 3: Bind to grid
    olapGrid1.OlapDataManager = dataManager;
    olapGrid1.DataBind();
}
```

## OlapReport Structure

The `OlapReport` defines what data to display and how to organize it.

### Basic Structure

```csharp
OlapReport report = new OlapReport();

// Set which cube to query
report.CurrentCubeName = "Adventure Works";

// Add dimensions and measures
// CategoricalElements = Column axis (dimensions + measures)
// SeriesElements = Row axis (dimensions only)
```

### Cube Selection

```csharp
// Set the cube name (must match exactly)
report.CurrentCubeName = "Adventure Works";
// or
report.CurrentCubeName = "Sales";
```

**Note:** Cube names are case-sensitive and must match the cube name in your OLAP source exactly.

### Axes Explained

**CategoricalElements (Column Axis):**
- Dimensions and measures that appear as columns
- Can contain both dimensions and measures
- Measures must be added to this axis

**SeriesElements (Row Axis):**
- Dimensions that appear as rows
- Contains only dimensions (no measures)
- Rows expand vertically

**Typical Pattern:**
```csharp
// Columns: Geography + Product + Measures
report.CategoricalElements.Add(geographyDimension);
report.CategoricalElements.Add(productDimension);
report.CategoricalElements.Add(measures);

// Rows: Time dimension
report.SeriesElements.Add(timeDimension);
```

## Dimension Configuration

Dimensions represent categorical data that can be drilled into hierarchically.

### Creating a Dimension

```csharp
DimensionElement dimension = new DimensionElement();
dimension.Name = "Customer"; // Dimension name from cube
dimension.AddLevel("Customer Geography", "Country"); // Hierarchy, Level
```

### AddLevel Method

The `AddLevel` method specifies which hierarchy and level to display initially.

**Signature:**
```csharp
void AddLevel(string hierarchyName, string levelName)
```

**Parameters:**
- `hierarchyName`: The hierarchy within the dimension (e.g., "Customer Geography", "Fiscal")
- `levelName`: The level to display (e.g., "Country", "State", "City" or "Year", "Quarter", "Month")

### Common Dimension Patterns

**Geography Dimension:**
```csharp
DimensionElement geography = new DimensionElement();
geography.Name = "Geography";
geography.AddLevel("Geography", "Country");
// Users can drill down to State, City levels
```

**Time Dimension:**
```csharp
DimensionElement date = new DimensionElement();
date.Name = "Date";
date.AddLevel("Fiscal", "Fiscal Year");
// Users can drill down to Quarter, Month, Day
```

**Product Dimension:**
```csharp
DimensionElement product = new DimensionElement();
product.Name = "Product";
product.AddLevel("Product Categories", "Category");
// Users can drill down to Subcategory, Product Name
```

**Customer Dimension:**
```csharp
DimensionElement customer = new DimensionElement();
customer.Name = "Customer";
customer.AddLevel("Customer Geography", "Country");
```

### Multiple Hierarchies

Some dimensions have multiple hierarchies. Choose the appropriate one:

```csharp
// Date dimension with Calendar hierarchy
DimensionElement calendarDate = new DimensionElement();
calendarDate.Name = "Date";
calendarDate.AddLevel("Calendar", "Calendar Year");

// Date dimension with Fiscal hierarchy
DimensionElement fiscalDate = new DimensionElement();
fiscalDate.Name = "Date";
fiscalDate.AddLevel("Fiscal", "Fiscal Year");
```

## Measure Configuration

Measures represent quantitative data (numbers to analyze).

### Creating Measures

```csharp
MeasureElements measures = new MeasureElements();
measures.Elements.Add(new MeasureElement { Name = "Internet Sales Amount" });
measures.Elements.Add(new MeasureElement { Name = "Reseller Sales Amount" });
measures.Elements.Add(new MeasureElement { Name = "Total Sales Amount" });
```

### Single Measure

```csharp
MeasureElements measures = new MeasureElements();
measures.Elements.Add(new MeasureElement { Name = "Sales Amount" });
```

### Multiple Measures

```csharp
MeasureElements measures = new MeasureElements();

// Add all relevant measures
measures.Elements.Add(new MeasureElement { Name = "Sales Amount" });
measures.Elements.Add(new MeasureElement { Name = "Order Quantity" });
measures.Elements.Add(new MeasureElement { Name = "Discount Amount" });
measures.Elements.Add(new MeasureElement { Name = "Tax Amount" });
measures.Elements.Add(new MeasureElement { Name = "Freight" });
```

**Note:** Measure names must match exactly as defined in the cube (case-sensitive).

## Complete Configuration Examples

### Example 1: Simple Sales Report

```csharp
private OlapReport CreateSalesReport()
{
    OlapReport report = new OlapReport();
    report.CurrentCubeName = "Adventure Works";
    
    // Column: Customer Geography
    DimensionElement customer = new DimensionElement();
    customer.Name = "Customer";
    customer.AddLevel("Customer Geography", "Country");
    
    // Column: Sales Amount measure
    MeasureElements measures = new MeasureElements();
    measures.Elements.Add(new MeasureElement { Name = "Internet Sales Amount" });
    
    // Row: Fiscal Year
    DimensionElement date = new DimensionElement();
    date.Name = "Date";
    date.AddLevel("Fiscal", "Fiscal Year");
    
    // Assemble report
    report.CategoricalElements.Add(customer);
    report.CategoricalElements.Add(measures);
    report.SeriesElements.Add(date);
    
    return report;
}
```

**Result:** Grid showing Internet Sales by Country (columns) and Fiscal Year (rows).

### Example 2: Multi-Dimensional Product Analysis

```csharp
private OlapReport CreateProductAnalysisReport()
{
    OlapReport report = new OlapReport();
    report.CurrentCubeName = "Adventure Works";
    
    // Column: Product Categories
    DimensionElement product = new DimensionElement();
    product.Name = "Product";
    product.AddLevel("Product Categories", "Category");
    
    // Column: Geography
    DimensionElement geography = new DimensionElement();
    geography.Name = "Sales Territory";
    geography.AddLevel("Sales Territory", "Country");
    
    // Column: Multiple measures
    MeasureElements measures = new MeasureElements();
    measures.Elements.Add(new MeasureElement { Name = "Sales Amount" });
    measures.Elements.Add(new MeasureElement { Name = "Order Quantity" });
    measures.Elements.Add(new MeasureElement { Name = "Discount Amount" });
    
    // Row: Time
    DimensionElement date = new DimensionElement();
    date.Name = "Date";
    date.AddLevel("Calendar", "Calendar Year");
    
    // Assemble report
    report.CategoricalElements.Add(product);
    report.CategoricalElements.Add(geography);
    report.CategoricalElements.Add(measures);
    report.SeriesElements.Add(date);
    
    return report;
}
```

**Result:** Grid showing Sales Amount, Order Quantity, and Discount Amount by Product Category and Country (columns) across Calendar Years (rows).

### Example 3: Time-Based Comparison

```csharp
private OlapReport CreateTimeComparisonReport()
{
    OlapReport report = new OlapReport();
    report.CurrentCubeName = "Adventure Works";
    
    // Column: Time period
    DimensionElement date = new DimensionElement();
    date.Name = "Date";
    date.AddLevel("Fiscal", "Fiscal Year");
    
    // Column: Measures
    MeasureElements measures = new MeasureElements();
    measures.Elements.Add(new MeasureElement { Name = "Internet Sales Amount" });
    measures.Elements.Add(new MeasureElement { Name = "Reseller Sales Amount" });
    
    // Row: Product categories
    DimensionElement product = new DimensionElement();
    product.Name = "Product";
    product.AddLevel("Product Categories", "Category");
    
    // Row: Geography
    DimensionElement customer = new DimensionElement();
    customer.Name = "Customer";
    customer.AddLevel("Customer Geography", "Country");
    
    // Assemble report
    report.CategoricalElements.Add(date);
    report.CategoricalElements.Add(measures);
    report.SeriesElements.Add(product);
    report.SeriesElements.Add(customer);
    
    return report;
}
```

**Result:** Grid comparing Internet and Reseller Sales across years (columns) by Product Category and Country (rows).

### Example 4: Using VB.NET

```vbnet
Private Function CreateReport() As OlapReport
    Dim report As OlapReport = New OlapReport()
    report.CurrentCubeName = "Adventure Works"
    
    ' Column dimension
    Dim customer As DimensionElement = New DimensionElement()
    customer.Name = "Customer"
    customer.AddLevel("Customer Geography", "Country")
    
    ' Measures
    Dim measures As MeasureElements = New MeasureElements()
    measures.Elements.Add(New MeasureElement With {.Name = "Sales Amount"})
    
    ' Row dimension
    Dim dateElement As DimensionElement = New DimensionElement()
    dateElement.Name = "Date"
    dateElement.AddLevel("Fiscal", "Fiscal Year")
    
    ' Build report
    report.CategoricalElements.Add(customer)
    report.CategoricalElements.Add(measures)
    report.SeriesElements.Add(dateElement)
    
    Return report
End Function
```

## Best Practices

### Connection Strings
- Store connection strings in configuration files (App.config or appsettings.json)
- Use environment variables for sensitive credentials
- Test connections before building complex reports
- Handle connection failures gracefully

### Report Design
- Start with simple reports (1-2 dimensions, 1 measure)
- Add complexity incrementally
- Choose the most important dimension for rows (better vertical space)
- Limit initial hierarchy levels (users can drill down)
- Keep measure count reasonable (3-5 measures max for readability)

### Performance
- Offline cubes perform better for read-only scenarios
- Limit initial data load with appropriate hierarchy levels
- Consider paging for very large datasets
- Cache OlapDataManager instances when possible

### Error Handling
- Validate cube names before use
- Catch connection exceptions
- Verify dimension and measure names match cube schema
- Provide user-friendly error messages

## Troubleshooting

**"Cannot connect to data source":**
- Verify connection string format
- Check server accessibility (ping, telnet)
- Confirm credentials are correct
- Ensure firewall allows connections

**"Cube not found":**
- Verify cube name matches exactly (case-sensitive)
- Check catalog/database name in connection string
- Ensure cube is deployed and processed

**"Dimension/Measure not found":**
- Verify names match cube schema exactly
- Check hierarchy and level names
- Use design-time query builder to browse available items

**Data appears empty:**
- Verify cube contains data for selected dimensions
- Check if filters or slicers exclude all data
- Ensure cube is processed with current data

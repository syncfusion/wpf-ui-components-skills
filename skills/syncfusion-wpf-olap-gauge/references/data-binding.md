# Data Binding in OLAP Gauge

Comprehensive guide for connecting the OLAP Gauge to various OLAP data sources including offline cubes, SQL Server Analysis Services, Mondrian, and ActivePivot servers.

## Table of Contents

- [Overview](#overview)
- [Connection String Fundamentals](#connection-string-fundamentals)
- [Binding to Offline Cube](#binding-to-offline-cube)
- [Binding to Cube in Local SQL Server](#binding-to-cube-in-local-sql-server)
- [Binding to Cube in Online SQL Server](#binding-to-cube-in-online-sql-server)
- [Binding to Mondrian Server](#binding-to-mondrian-server)
- [Binding to ActivePivot Server](#binding-to-activepivot-server)
- [Authentication and Credentials](#authentication-and-credentials)
- [Data Provider Configuration](#data-provider-configuration)
- [Connection Best Practices](#connection-best-practices)
- [Troubleshooting Connections](#troubleshooting-connections)

## Overview

The OLAP Gauge supports multiple OLAP data sources through the `OlapDataManager` class. The connection method is determined by the connection string format and optional provider configuration.

### Supported Data Sources

1. **Offline Cube Files** (.cub files) - Local cube files on disk
2. **SQL Server Analysis Services (SSAS)** - Microsoft's OLAP solution (local or remote)
3. **XML/A Endpoints** - Any OLAP server supporting XML for Analysis protocol
4. **Mondrian Server** - Open-source OLAP server
5. **ActivePivot Server** - Real-time OLAP server

### OlapDataManager Basics

```csharp
// Create data manager with connection string
OlapDataManager dataManager = new OlapDataManager(connectionString);

// Set the report
dataManager.SetCurrentReport(olapReport);

// Bind to gauge
olapGauge.OlapDataManager = dataManager;
```

## Connection String Fundamentals

Connection strings specify how to connect to the OLAP data source.

### Basic Structure

```
DataSource=<source>; Initial Catalog=<database>; [Provider=<provider>;]
```

### Key Components

- **DataSource** (or **Data Source**): Server URL, file path, or endpoint
- **Initial Catalog**: Database/catalog name
- **Provider**: OLAP provider (optional, defaults to MSOLAP)
- **User ID / Password**: Credentials (optional)

### Format Variations

Both formats are valid:

```csharp
// With spaces
"Data Source=localhost; Initial Catalog=Adventure Works DW;"

// Without spaces (camelCase)
"DataSource=localhost; InitialCatalog=AdventureWorksDW;"
```

## Binding to Offline Cube

Connect to a local cube file (.cub) stored on the file system.

### Connection String Format

```csharp
string connectionString = @"DataSource = <file_path>; Provider = MSOLAP;";
```

### Complete Example

```csharp
using Syncfusion.Olap.Manager;

// Specify physical path to cube file
string connectionString = @"DataSource = C:\Cubes\Adventure_Works_Ext.cub; Provider = MSOLAP;";
OlapDataManager dataManager = new OlapDataManager(connectionString);

// Configure report and bind
dataManager.SetCurrentReport(CreateOlapReport());
this.OlapGauge1.OlapDataManager = dataManager;
```

### Path Considerations

**Absolute paths:**

```csharp
// Windows path
@"DataSource = C:\Data\Cubes\MyCube.cub; Provider = MSOLAP;"

// Network path
@"DataSource = \\Server\Share\Cubes\MyCube.cub; Provider = MSOLAP;"
```

**Relative paths** (relative to application directory):

```csharp
@"DataSource = .\Cubes\MyCube.cub; Provider = MSOLAP;"
@"DataSource = ..\..\Data\MyCube.cub; Provider = MSOLAP;"
```

### Use Cases

- **Development and testing** without server dependency
- **Disconnected scenarios** where server access is unavailable
- **Distribution of pre-processed cubes** with applications
- **Demos and samples** with embedded data

### Limitations

- No real-time data updates
- Manual cube refreshes required
- Larger file sizes for complex cubes
- Limited to Microsoft Analysis Services cube format

## Binding to Cube in Local SQL Server

Connect to SQL Server Analysis Services (SSAS) running on localhost or local network machine.

### Connection String Format

```csharp
"Data source=<server_name>; Initial Catalog=<database_name>;"
```

### Localhost Connection

```csharp
using Syncfusion.Olap.Manager;

string connectionString = "Data source=localhost; Initial Catalog=Adventure Works DW;";
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

VB.NET:

```vb
Imports Syncfusion.Olap.Manager

Dim connectionString As String = "Data source=localhost; Initial Catalog=Adventure Works DW;"
Dim dataManager As New OlapDataManager(connectionString)
```

### Named Instance Connection

For SQL Server named instances:

```csharp
string connectionString = "Data source=localhost\\SQLEXPRESS; Initial Catalog=Adventure Works DW;";
```

### Network Server Connection

```csharp
// By server name
string connectionString = "Data source=MYSERVER; Initial Catalog=Adventure Works DW;";

// By IP address
string connectionString = "Data source=192.168.1.100; Initial Catalog=Adventure Works DW;";

// With port number
string connectionString = "Data source=MYSERVER:2383; Initial Catalog=Adventure Works DW;";
```

### With Windows Authentication

Windows Authentication is used by default. Ensure the application runs under an account with SSAS access:

```csharp
string connectionString = "Data source=localhost; Initial Catalog=Adventure Works DW; Integrated Security=SSPI;";
```

### With SQL Authentication

```csharp
string connectionString = "Data source=localhost; Initial Catalog=Adventure Works DW; " +
                          "User ID=ssas_user; Password=SecurePassword123;";
```

### Complete Example

```csharp
using Syncfusion.Olap.Manager;
using Syncfusion.Olap.Reports;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        ConnectToLocalSSAS();
    }
    
    private void ConnectToLocalSSAS()
    {
        // Connection string for local SSAS
        string connectionString = "Data source=localhost; Initial Catalog=Adventure Works DW;";
        
        // Create data manager
        OlapDataManager dataManager = new OlapDataManager(connectionString);
        
        // Create and set report
        OlapReport report = new OlapReport();
        report.CurrentCubeName = "Adventure Works";
        // ... configure report
        
        dataManager.SetCurrentReport(report);
        this.OlapGauge1.OlapDataManager = dataManager;
    }
}
```

## Binding to Cube in Online SQL Server

Connect to SQL Server Analysis Services via XML/A (XML for Analysis) protocol over HTTP/HTTPS.

### Connection String Format

```csharp
"Data Source=<xmla_endpoint>; Initial Catalog=<database_name>;"
```

### XML/A Endpoint Format

Typical SSAS XML/A endpoint:

```
http(s)://<server>:<port>/olap/msmdpump.dll
```

### HTTP Connection

```csharp
using Syncfusion.Olap.Manager;

string connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " + "Initial Catalog=Adventure Works DW 2008 SE;"
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

VB.NET:

```vb
Imports Syncfusion.Olap.Manager

Dim connectionString As String = "YOUR_END_POINT"; // E.g - "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " & _"Initial Catalog=Adventure Works DW 2008 SE;"
Dim dataManager As New OlapDataManager(connectionString)
```

### HTTPS Connection (Secure)

```csharp
string connectionString = "YOUR_END_POINT"; // E.g - "Data Source=https://secure-server.com/olap/msmdpump.dll; " + "Initial Catalog=Adventure Works DW;"
OlapDataManager dataManager = new OlapDataManager(connectionString);
```

### With Authentication

```csharp
string connectionString = "YOUR_END_POINT"; // E.g - "Data Source=http://server.com/olap/msmdpump.dll; " + "Initial Catalog=Adventure Works DW; " + "User ID=domain\\username; Password=SecurePass123;"
```

### Custom Port

```csharp
string connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=http://server.com:8080/olap/msmdpump.dll; " + "Initial Catalog=Adventure Works DW;"
```

### Complete Example with Error Handling

```csharp
private void ConnectToOnlineSSAS()
{
    try
    {
        string connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " +  "Initial Catalog=Adventure Works DW 2008 SE;"
        
        OlapDataManager dataManager = new OlapDataManager(connectionString);
        dataManager.SetCurrentReport(CreateOlapReport());
        
        this.OlapGauge1.OlapDataManager = dataManager;
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Connection failed: {ex.Message}", "Error", 
                        MessageBoxButton.OK, MessageBoxImage.Error);
    }
}
```

### IIS Configuration Notes

For XML/A access to SSAS through IIS, ensure:
1. **msmdpump.dll** is configured as ISAPI extension
2. Anonymous authentication is disabled if using credentials
3. Windows Authentication or Basic Authentication is enabled
4. Application pool has permissions to SSAS

## Binding to Mondrian Server

Connect to Mondrian OLAP server using XML/A protocol.

### Connection String Format

```csharp
"Data Source = <mondrian_xmla_endpoint>; Initial Catalog = <catalog_name>;"
```

### Provider Configuration

**Important:** Must explicitly set provider to Mondrian:

```csharp
dataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.Mondrian;
```

### Complete Example

```csharp
using Syncfusion.Olap.Manager;
using Syncfusion.Olap.DataProvider;

string connectionString = "YOUR_END_POINT"; // For e.g - @"Data Source = http://localhost:8080/mondrian/xmla; Initial Catalog = FoodMart;"

OlapDataManager dataManager = new OlapDataManager(connectionString);

// Set provider to Mondrian
dataManager.DataProvider.ProviderName = Providers.Mondrian;

// Configure report and bind
dataManager.SetCurrentReport(CreateOlapReport());
this.OlapGauge1.OlapDataManager = dataManager;
```

VB.NET:

```vb
Imports Syncfusion.Olap.Manager
Imports Syncfusion.Olap.DataProvider

Dim connectionString As String = "YOUR_END_POINT"; // For e.g - "Data Source = http://localhost:8080/mondrian/xmla; Initial Catalog = FoodMart;"
Dim dataManager As New OlapDataManager(connectionString)

' Set provider to Mondrian
dataManager.DataProvider.ProviderName = Providers.Mondrian

dataManager.SetCurrentReport(CreateOlapReport())
Me.OlapGauge1.OlapDataManager = dataManager
```

### With Authentication

```csharp
string connectionString = "YOUR_END_POINT"; // For e.g - @"Data Source = http://localhost:8080/mondrian/xmla; " + @"Initial Catalog = FoodMart; " + @"User ID = admin; Password = admin123;"
```

### Custom Port

```csharp
string connectionString = "YOUR_END_POINT"; // For e.g - @"Data Source = http://mondrian-server:9090/mondrian/xmla; " + @"Initial Catalog = SalesCube;"
```

### Mondrian-Specific Considerations

- Schema files must be properly configured on Mondrian server
- Catalog name corresponds to Mondrian schema name
- KPI support may be limited compared to SSAS
- Dimension and measure names are case-sensitive

## Binding to ActivePivot Server

Connect to ActivePivot real-time OLAP server using XML/A protocol.

### Connection String Format

```csharp
"Data Source = <activepivot_xmla_endpoint>; Initial Catalog = <catalog_name>;"
```

### Provider Configuration

**Important:** Must explicitly set provider to ActivePivot:

```csharp
dataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.ActivePivot;
```

### Complete Example

```csharp
using Syncfusion.Olap.Manager;
using Syncfusion.Olap.DataProvider;

string connectionString = "YOUR_END_POINT"; // For e.g - @"Data Source = http://localhost:8080/cva_s/xmla; Initial Catalog = CVAS;";

OlapDataManager dataManager = new OlapDataManager(connectionString);

// Set provider to ActivePivot
dataManager.DataProvider.ProviderName = Providers.ActivePivot;

// Configure and bind
dataManager.SetCurrentReport(CreateOlapReport());
this.OlapGauge1.OlapDataManager = dataManager;
```

VB.NET:

```vb
Imports Syncfusion.Olap.Manager
Imports Syncfusion.Olap.DataProvider

Dim connectionString As String = "YOUR_END_POINT"; // For e.g - "Data Source = http://localhost:8080/cva_s/xmla; Initial Catalog = CVAS;"
Dim dataManager As New OlapDataManager(connectionString)

' Set provider to ActivePivot
dataManager.DataProvider.ProviderName = Providers.ActivePivot

dataManager.SetCurrentReport(CreateOlapReport())
Me.OlapGauge1.OlapDataManager = dataManager
```

### With Authentication

```csharp
string connectionString = "YOUR_END_POINT"; // For e.g - @"Data Source = http://activepivot.company.com:8080/cva_s/xmla; " + @"Initial Catalog = CVAS; " + @"User ID = pivot_user; Password = SecurePass123;"
```

### ActivePivot-Specific Considerations

- Real-time data updates available
- Custom aggregation functions supported
- May require specific ActivePivot version compatibility
- Connection endpoint varies by ActivePivot deployment configuration

## Authentication and Credentials

### Windows Authentication (SSAS)

Default for local SSAS connections:

```csharp
// Implicit Windows Authentication
"Data source=localhost; Initial Catalog=Adventure Works DW;"

// Explicit Windows Authentication
"Data source=localhost; Initial Catalog=Adventure Works DW; Integrated Security=SSPI;"
```

### SQL Server Authentication

```csharp
string connectionString = "YOUR_END_POINT"; // For e.g - "Data source=localhost; Initial Catalog=Adventure Works DW; " + "User ID=ssas_user; Password=SecurePassword123;"
```

### Basic Authentication (HTTP/HTTPS)

For XML/A over HTTP with Basic Authentication:

```csharp
string connectionString = "YOUR_END_POINT"; // For e.g - "Data Source=http://server.com/olap/msmdpump.dll; " + "Initial Catalog=Adventure Works DW; " + "User ID=domain\\username; Password=Pass123;"
```

### Domain Accounts

```csharp
// Domain\Username format
"User ID=CONTOSO\\john.doe; Password=SecurePass123;"

// User Principal Name format
"User ID=john.doe@contoso.com; Password=SecurePass123;"
```

### Credential Storage Best Practices

**Don't hardcode credentials:**

```csharp
// ❌ Bad - credentials in code
string connectionString = "Data source=localhost; User ID=admin; Password=admin123;";
```

**Use configuration files:**

```xml
<!-- App.config -->
<configuration>
  <connectionStrings>
    <add name="OlapConnection" 
         connectionString="Data source=localhost; Initial Catalog=Adventure Works DW; User ID=ssas_user; Password=encrypted_password"/>
  </connectionStrings>
</configuration>
```

```csharp
// ✓ Good - credentials from config
string connectionString = ConfigurationManager.ConnectionStrings["OlapConnection"].ConnectionString;
```

**Use secure storage:**

```csharp
// Use Windows Credential Manager, Azure Key Vault, or similar
string password = GetPasswordFromSecureStorage();
string connectionString = "YOUR_END_POINT"; // For e.g - $"Data source=localhost; Initial Catalog=Adventure Works DW; " + $"User ID=ssas_user; Password={password};"
```

## Data Provider Configuration

### Available Providers

```csharp
using Syncfusion.Olap.DataProvider;

// Microsoft SQL Server Analysis Services (default)
dataManager.DataProvider.ProviderName = Providers.SSAS;

// Mondrian OLAP Server
dataManager.DataProvider.ProviderName = Providers.Mondrian;

// ActivePivot Server
dataManager.DataProvider.ProviderName = Providers.ActivePivot;
```

### When to Set Provider

**SSAS:** Provider setting optional (default)

```csharp
// Both work the same for SSAS
OlapDataManager dataManager = new OlapDataManager(connectionString);
// No need to set provider
```

**Mondrian:** Provider setting **required**

```csharp
OlapDataManager dataManager = new OlapDataManager(connectionString);
dataManager.DataProvider.ProviderName = Providers.Mondrian;  // Must set
```

**ActivePivot:** Provider setting **required**

```csharp
OlapDataManager dataManager = new OlapDataManager(connectionString);
dataManager.DataProvider.ProviderName = Providers.ActivePivot;  // Must set
```

## Connection Best Practices

### 1. Connection Reuse

Reuse `OlapDataManager` instances when possible:

```csharp
// Create once
private OlapDataManager _dataManager;

public MainWindow()
{
    InitializeComponent();
    _dataManager = new OlapDataManager(connectionString);
}

// Reuse for multiple gauges or report changes
private void UpdateReport()
{
    _dataManager.SetCurrentReport(CreateNewReport());
}
```

### 2. Async Connection (for remote servers)

```csharp
private async Task ConnectToOlapAsync()
{
    await Task.Run(() =>
    {
        OlapDataManager dataManager = new OlapDataManager(connectionString);
        dataManager.SetCurrentReport(CreateOlapReport());
        
        Dispatcher.Invoke(() =>
        {
            this.OlapGauge1.OlapDataManager = dataManager;
        });
    });
}
```

### 3. Connection Validation

```csharp
private bool ValidateConnection(string connectionString)
{
    try
    {
        OlapDataManager testManager = new OlapDataManager(connectionString);
        testManager.SetCurrentReport(CreateSimpleReport());
        return true;
    }
    catch
    {
        return false;
    }
}
```

### 4. Timeout Configuration

For slow connections, consider implementing timeout handling:

```csharp
private Task<OlapDataManager> ConnectWithTimeoutAsync(string connectionString, int timeoutSeconds)
{
    return Task.Run(() =>
    {
        var tokenSource = new CancellationTokenSource(TimeSpan.FromSeconds(timeoutSeconds));
        // Connection logic with cancellation token
    });
}
```

## Troubleshooting Connections

### Connection Failed

**Symptoms:** Exception on OlapDataManager creation or SetCurrentReport

**Solutions:**
1. Verify server is accessible (ping, telnet to port)
2. Check connection string syntax
3. Confirm catalog/database name exists
4. Verify credentials if authentication required
5. Check firewall settings

### HTTP 401 Unauthorized

**Solutions:**
1. Add User ID and Password to connection string
2. Verify account has SSAS access permissions
3. Check IIS authentication settings for XML/A endpoint
4. Ensure Windows Authentication or Basic Authentication is enabled

### "Cannot connect to server" (offline cube)

**Solutions:**
1. Verify file path is correct and accessible
2. Check file exists at specified location
3. Ensure application has read permissions
4. Use absolute path instead of relative path

### Slow Performance

**Solutions:**
1. Use local SSAS instead of remote XML/A when possible
2. Optimize MDX queries in report definition
3. Implement caching for frequently accessed data
4. Consider using offline cubes for better performance

### Provider Not Supported

**Solutions:**
1. Set DataProvider.ProviderName for Mondrian/ActivePivot
2. Ensure correct Syncfusion.Olap.DataProvider namespace
3. Verify provider enum value: `Providers.Mondrian` or `Providers.ActivePivot`

### Certificate/SSL Errors (HTTPS)

**Solutions:**
1. Install valid SSL certificate on server
2. Add certificate to trusted root authorities
3. Use `http://` instead of `https://` for testing (not production)
4. Implement custom certificate validation (with caution)

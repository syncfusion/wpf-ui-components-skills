# Data Binding in WPF OLAP Client

## Table of Contents
- [Overview](#overview)
- [Binding to Offline Cube](#binding-to-offline-cube)
- [Binding to Local SQL Server](#binding-to-local-sql-server)
- [Binding to Online SQL Server](#binding-to-online-sql-server)
- [Binding to Mondrian Server](#binding-to-mondrian-server)
- [Binding to ActivePivot Server](#binding-to-activepivot-server)
- [Connection String Patterns](#connection-string-patterns)
- [Authentication and Credentials](#authentication-and-credentials)
- [Troubleshooting Connection Issues](#troubleshooting-connection-issues)

## Overview

The OLAP Client supports connecting to multiple types of OLAP data sources through different connection methods. This guide covers all supported connection types and their configuration patterns.

## Binding to Offline Cube

Connect to an OLAP cube file (.cub) available on the local machine by setting the physical path in the connection string.

### Configuration

```csharp
// Physical path to offline cube file
string connectionString = @"Datasource=C:\OfflineCube\Adventure_Works_Ext.cub; Provider=msolap;";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

**VB.NET:**

```vbnet
' Physical path to offline cube file
Dim connectionString As String = "Datasource=C:\OfflineCube\Adventure_Works_Ext.cub; Provider=msolap;"

Dim olapDataManager As OlapDataManager = New OlapDataManager(connectionString)
Me.olapClient1.OlapDataManager = olapDataManager
Me.olapClient1.DataBind()
```

### Use Cases

- **Disconnected analysis**: Work without server connection
- **Development/testing**: Use sample cubes without server setup
- **Performance**: Fast access to local cube data
- **Offline reporting**: Generate reports in environments without network access

### Path Considerations

- Use absolute paths: `C:\OfflineCube\Adventure_Works_Ext.cub`
- Use relative paths: `..\Data\MyCube.cub` (relative to application directory)
- Ensure read permissions for the cube file location
- File extension must be `.cub`

## Binding to Local SQL Server

Connect to OLAP cubes in SQL Server Analysis Services (SSAS) running on the local machine or network server.

### Configuration

```csharp
// Connection to local SQL Server Analysis Services
string connectionString = "Data source=localhost; Initial Catalog=Adventure Works DW";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

**VB.NET:**

```vbnet
Dim connectionString As String = "Data source=localhost; Initial Catalog=Adventure Works DW"

Dim olapDataManager As OlapDataManager = New OlapDataManager(connectionString)
Me.olapClient1.OlapDataManager = olapDataManager
Me.olapClient1.DataBind()
```

### Connection String Components

- **Data source**: Server name or IP address (`localhost`, `SERVER01`, `192.168.1.100`)
- **Initial Catalog**: Database/cube catalog name

### With Authentication

```csharp
// With SQL Server authentication
string connectionString = "Data source=localhost; " +
                          "Initial Catalog=Adventure Works DW; " +
                          "User ID=myUsername; " +
                          "Password=myPassword;";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

### With Windows Authentication

```csharp
// Use Windows integrated authentication (default)
string connectionString = "Data source=localhost; " +
                          "Initial Catalog=Adventure Works DW; " +
                          "Integrated Security=SSPI;";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

## Binding to Online SQL Server

Connect to SQL Server Analysis Services hosted on a remote server through XML/A (XML for Analysis) protocol.

### Configuration

```csharp
// Connection via XML/A to online server
string connectionString = "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " +
                          "Initial Catalog=Adventure Works DW 2008 SE;";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

**VB.NET:**

```vbnet
Dim connectionString As String = "Data Source=http://bi.syncfusion.com/olap/msmdpump.dll; " & _
                                  "Initial Catalog=Adventure Works DW 2008 SE;"

Dim olapDataManager As OlapDataManager = New OlapDataManager(connectionString)
Me.olapClient1.OlapDataManager = olapDataManager
Me.olapClient1.DataBind()
```

### XML/A URL Format

The Data Source URL typically follows this pattern:
- `http://servername/olap/msmdpump.dll` (HTTP)
- `https://servername/olap/msmdpump.dll` (HTTPS - recommended for production)

### With Credentials

```csharp
// With authentication credentials
string connectionString = "Data Source=http://myserver.com/olap/msmdpump.dll; " +
                          "Initial Catalog=Adventure Works DW; " +
                          "User ID=myUsername; " +
                          "Password=myPassword;";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

### Use Cases

- **Cloud-hosted SSAS**: Connect to Azure Analysis Services or cloud servers
- **Remote data centers**: Access enterprise cubes over the internet
- **Centralized BI**: Multiple clients connecting to shared OLAP server
- **Cross-network access**: Connect from different network segments

## Binding to Mondrian Server

Connect to Mondrian OLAP server (open-source OLAP engine) through XML/A protocol.

### Configuration

```csharp
// Connection to Mondrian server
string connectionString = @"Data Source=http://localhost:8080/mondrian/xmla; " +
                           "Initial Catalog=FoodMart;";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);

// CRITICAL: Set provider to Mondrian
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.Mondrian;

this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

**VB.NET:**

```vbnet
Dim connectionString As String = "Data Source=http://localhost:8080/mondrian/xmla; " & _
                                  "Initial Catalog=FoodMart;"

Dim olapDataManager As OlapDataManager = New OlapDataManager(connectionString)

' CRITICAL: Set provider to Mondrian
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.Mondrian

Me.olapClient1.OlapDataManager = olapDataManager
Me.olapClient1.DataBind()
```

### Important Notes

⚠️ **Must set ProviderName**: Unlike SQL Server connections, Mondrian requires explicitly setting the provider:

```csharp
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.Mondrian;
```

### Mondrian URL Pattern

- Default: `http://servername:8080/mondrian/xmla`
- Port may vary based on your Tomcat/server configuration
- Path typically ends with `/xmla`

### With Authentication

```csharp
string connectionString = @"Data Source=http://localhost:8080/mondrian/xmla; " +
                           "Initial Catalog=FoodMart; " +
                           "User ID=admin; " +
                           "Password=admin;";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.Mondrian;
this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

## Binding to ActivePivot Server

Connect to ActivePivot OLAP server (real-time OLAP platform) through XML/A protocol.

### Configuration

```csharp
// Connection to ActivePivot server
string connectionString = @"Data Source=http://localhost:8080/cva_s/xmla; " +
                           "Initial Catalog=CVAS;";

OlapDataManager olapDataManager = new OlapDataManager(connectionString);

// CRITICAL: Set provider to ActivePivot
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.ActivePivot;

this.olapClient1.OlapDataManager = olapDataManager;
this.olapClient1.DataBind();
```

**VB.NET:**

```vbnet
Dim connectionString As String = "Data Source=http://localhost:8080/cva_s/xmla; " & _
                                  "Initial Catalog=CVAS;"

Dim olapDataManager As OlapDataManager = New OlapDataManager(connectionString)

' CRITICAL: Set provider to ActivePivot
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.ActivePivot

Me.olapClient1.OlapDataManager = olapDataManager
Me.olapClient1.DataBind()
```

### Important Notes

⚠️ **Must set ProviderName**: ActivePivot requires explicitly setting the provider:

```csharp
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.ActivePivot;
```

### ActivePivot URL Pattern

- Default: `http://servername:8080/cva_s/xmla`
- Port and path may vary based on your ActivePivot configuration

## Connection String Patterns

### Quick Reference Table

| Data Source | Connection Pattern | Provider Setting |
|-------------|-------------------|------------------|
| **Offline Cube** | `Datasource=C:\path\file.cub; Provider=msolap;` | Not required |
| **Local SSAS** | `Data source=localhost; Initial Catalog=DBName;` | Not required |
| **Online SSAS** | `Data Source=http://server/olap/msmdpump.dll; Initial Catalog=DBName;` | Not required |
| **Mondrian** | `Data Source=http://server:8080/mondrian/xmla; Initial Catalog=DBName;` | **Required**: `Providers.Mondrian` |
| **ActivePivot** | `Data Source=http://server:8080/cva_s/xmla; Initial Catalog=DBName;` | **Required**: `Providers.ActivePivot` |

### Common Connection String Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `Data source` / `Datasource` | Server name or cube file path | `localhost`, `C:\cube.cub` |
| `Data Source` (URL) | XML/A endpoint URL | `http://server/olap/msmdpump.dll` |
| `Initial Catalog` | Database/catalog name | `Adventure Works DW` |
| `User ID` | Username for authentication | `myUsername` |
| `Password` | Password for authentication | `myPassword` |
| `Integrated Security` | Use Windows auth | `SSPI` |
| `Provider` | OLEDB provider (offline cubes) | `msolap` |

## Authentication and Credentials

### No Authentication (Default)

```csharp
// For servers that don't require authentication
string connectionString = "Data source=localhost; Initial Catalog=Adventure Works DW";
```

### SQL Server Authentication

```csharp
string connectionString = "Data source=localhost; " +
                          "Initial Catalog=Adventure Works DW; " +
                          "User ID=myUsername; " +
                          "Password=myPassword;";
```

### Windows Integrated Authentication

```csharp
string connectionString = "Data source=localhost; " +
                          "Initial Catalog=Adventure Works DW; " +
                          "Integrated Security=SSPI;";
```

### Storing Credentials Securely

⚠️ **Security Best Practice**: Never hardcode passwords in source code. Use:

```csharp
// Read from configuration
string connectionString = ConfigurationManager.ConnectionStrings["OlapConnection"].ConnectionString;

// Or read from secure storage
string password = GetSecurePassword(); // Implement secure retrieval
string connectionString = $"Data source=localhost; Initial Catalog=Adventure Works DW; " +
                          $"User ID=myUsername; Password={password};";
```

## Troubleshooting Connection Issues

### Connection Failed

**Symptoms**: Cannot connect to data source  
**Solutions**:
- Verify server is running and accessible
- Check firewall settings allow connections
- Confirm connection string syntax is correct
- Test connection with other tools (SQL Server Management Studio)

### Invalid Catalog

**Symptoms**: "Catalog not found" or similar errors  
**Solutions**:
- Verify catalog/database name is spelled correctly
- Confirm catalog exists on the server
- Check permissions to access the catalog

### Authentication Failed

**Symptoms**: Access denied or authentication errors  
**Solutions**:
- Verify username and password are correct
- Check user has permissions to access cube
- For Windows auth, ensure current user has access rights
- Confirm SQL Server authentication is enabled (if using SQL auth)

### Provider Not Set for Mondrian/ActivePivot

**Symptoms**: Connection fails or returns incorrect data  
**Solutions**:
```csharp
// MUST set provider for Mondrian
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.Mondrian;

// MUST set provider for ActivePivot
olapDataManager.DataProvider.ProviderName = Syncfusion.Olap.DataProvider.Providers.ActivePivot;
```

### XML/A Endpoint Not Found

**Symptoms**: HTTP 404 or endpoint not found  
**Solutions**:
- Verify XML/A is configured on the server
- Check URL path (typically ends with `/msmdpump.dll` for SSAS)
- Confirm server is configured to accept HTTP/HTTPS requests
- Check IIS configuration for SSAS HTTP access

### Timeout Issues

**Symptoms**: Connection or query timeouts  
**Solutions**:
```csharp
// Increase timeout in connection string
string connectionString = "Data source=localhost; " +
                          "Initial Catalog=Adventure Works DW; " +
                          "Connect Timeout=60;"; // 60 seconds
```

### Offline Cube File Not Found

**Symptoms**: File not found or access denied  
**Solutions**:
- Verify cube file path is correct and absolute
- Check file exists at specified location
- Ensure application has read permissions for the file
- Verify file extension is `.cub`

## Next Steps

After successfully connecting to your data source:

1. Learn about OLAP Client UI elements - see [olap-client-elements.md](olap-client-elements.md)
2. Explore filtering capabilities - see [filtering.md](filtering.md)
3. Learn report management - see [report-management.md](report-management.md)

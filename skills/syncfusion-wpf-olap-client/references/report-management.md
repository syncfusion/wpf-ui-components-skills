# Report Management and Serialization in WPF OLAP Client

## Table of Contents
- [Overview](#overview)
- [Report Manipulation Operations](#report-manipulation-operations)
  - [New Report](#new-report)
  - [Add Report](#add-report)
  - [Remove Report](#remove-report)
  - [Rename Report](#rename-report)
  - [Save Report](#save-report)
  - [Load Report](#load-report)
  - [Report List Navigation](#report-list-navigation)
- [Report Serialization](#report-serialization)
  - [Saving Reports as XML Files](#saving-reports-as-xml-files)
  - [Loading Reports from XML Files](#loading-reports-from-xml-files)
- [Storing and Retrieving Reports as Streams](#storing-and-retrieving-reports-as-streams)
  - [Getting Report as Stream](#getting-report-as-stream)
  - [Loading Report from Stream](#loading-report-from-stream)
  - [Database Storage Example](#database-storage-example)
- [Use Cases](#use-cases)
- [Best Practices](#best-practices)

## Overview

The OLAP Client provides comprehensive report management capabilities, allowing users to create, edit, save, and load multiple reports. Reports can be persisted as XML files or as streams, enabling flexible storage options including databases.

## Report Manipulation Operations

All report manipulation operations are accessible through the OLAP Client toolbar.

### New Report

Creates a new report collection with a single empty report, **clearing the existing report collection**. Use this to start completely fresh.

#### Steps

1. Click the **New Report** icon on the toolbar
2. The New Report dialog opens, prompting for a report name
3. Enter a name for the report
4. Click **OK** to create (or **Cancel** to abort)
5. The report collection is replaced with the new empty report

#### When to Use

- Starting a completely new analysis session
- Clearing all existing reports
- Resetting to blank slate
- After completing a project and beginning a new one

#### Important Note

⚠️ **This operation clears the entire report collection**. If you want to keep existing reports, use **Add Report** instead.

### Add Report

Adds a new report to the **existing report collection** without clearing other reports. Use this to build multiple analytical perspectives.

#### Steps

1. Click the **Add Report** icon on the toolbar
2. The Add Report dialog opens
3. Enter a name for the new report
4. Click **OK** to add (or **Cancel** to abort)
5. The new report is added to the collection

#### When to Use

- Building multiple views in one session
- Creating comparative analyses
- Developing a set of related reports
- Adding alternative perspectives

### Remove Report

Removes the **current/active report** from the report collection.

#### Rules

- **Requires multiple reports**: Only works if the collection has more than one report
- **Cannot remove last report**: You cannot remove the only remaining report
- **Removes active report**: Deletes whichever report is currently selected

#### Steps

1. Select the report you want to remove (via Report List)
2. Click the **Remove Report** icon
3. Report is immediately removed from collection
4. Another report in the collection becomes active

#### When to Use

- Cleaning up unwanted reports
- Removing experimental analyses
- Simplifying report collections
- Deleting outdated perspectives

### Rename Report

Changes the name of the **current/active report**.

#### Steps

1. Click the **Rename Report** icon on the toolbar
2. The Rename Report dialog opens
3. Enter the new name
4. Click **OK** to apply (or **Cancel** to abort)
5. The active report is refreshed with the new name

#### When to Use

- Making report names more descriptive
- Fixing typos in names
- Updating names to reflect content changes
- Standardizing naming conventions

### Save Report

Saves the current report collection to a file on the local system. All reports in the collection are saved together.

#### Steps

1. Click the **Save Report** icon on the toolbar
2. The SaveAs file dialog opens
3. Navigate to the desired save location
4. Enter a file name
5. Click **Save**
6. The report collection is stored as an XML file

#### File Format

Reports are saved as XML files containing:
- Report structure and configuration
- Axis configurations (categorical, series, slicer)
- Filter settings
- Sort settings
- Calculated members
- Report names and metadata

#### When to Use

- Preserving work for future sessions
- Sharing reports with colleagues
- Creating reusable analysis templates
- Backing up important analyses

### Load Report

Loads a previously saved report collection from the local system, **replacing the current collection**.

#### Steps

1. Click the **Load Report** icon on the toolbar
2. The Open file dialog appears
3. Navigate to the saved report file
4. Select the file
5. Click **Open**
6. The report collection is loaded, replacing the current collection

#### When to Use

- Resuming previous analysis sessions
- Opening shared report templates
- Loading standardized reports
- Restoring backed-up analyses

⚠️ **Important**: Loading a report replaces the current collection. Save your current work first if needed.

### Report List Navigation

The Report List drop-down provides quick access to all reports in the current collection.

#### Features

- **Display All Reports**: Shows names of all reports in collection
- **Quick Switching**: Select any report to make it active
- **Current Indicator**: Highlights the currently active report
- **Immediate Load**: Selected report loads instantly

#### Steps

1. Click the **Report List** drop-down
2. View all available reports
3. Click on the desired report
4. That report becomes active and displays immediately

#### When to Use

- Switching between multiple analytical views
- Comparing different perspectives quickly
- Navigating complex report collections
- Reviewing all available analyses

## Report Serialization

Report serialization allows saving and loading reports as XML files for long-term storage and sharing.

### Saving Reports as XML Files

The **Save Report** toolbar button serializes the report collection to XML:

**What Gets Saved:**
- All reports in the collection
- Report names and metadata
- Axis configurations
- Dimension and measure selections
- Filter criteria
- Sort settings
- Calculated members
- Visual preferences

**File Location:**
- User-selected location on local file system
- Typically has `.xml` extension
- Human-readable format (can be viewed in text editor)

### Loading Reports from XML Files

The **Load Report** toolbar button deserializes XML files back into report collections:

**What Gets Loaded:**
- Complete report collection structure
- All saved configurations and settings
- Filters, sorts, and calculations
- Ready to use immediately (if cube is accessible)

**Requirements:**
- Access to the same cube or compatible cube structure
- Same measures and dimensions must exist
- Proper permissions on the cube

## Storing and Retrieving Reports as Streams

For advanced scenarios, reports can be saved and loaded as streams, enabling storage in databases or other non-file-system locations.

### Getting Report as Stream

Retrieve the current session as a stream object:

```csharp
// Get the current report as a stream
Stream stream = this.olapClient1.GetReportStream();

if (stream != null)
{
    // Process the stream (save to database, send over network, etc.)
    ProcessReportStream(stream);
}
```

**VB.NET:**

```vbnet
' Get the current report as a stream
Dim stream As Stream = Me.olapClient1.GetReportStream()

If stream IsNot Nothing Then
    ' Process the stream
    ProcessReportStream(stream)
End If
```

### Loading Report from Stream

Load a report from a stream object:

```csharp
// Load report from a stream
this.olapClient1.LoadReportStream(reportStream);
```

**VB.NET:**

```vbnet
' Load report from a stream
Me.olapClient1.LoadReportStream(reportStream)
```

### Database Storage Example

Store and retrieve reports from a database using streams.

#### Storing Report as Stream in Database

```csharp
// Get report as stream
Stream stream = this.olapClient1.GetReportStream();

if (stream != null)
{
    MemoryStream reportStream = stream as MemoryStream;
    
    // Prompt user for report name
    ReportNameWindow saveReport = new ReportNameWindow(this.ReportNames);
    saveReport.WindowStartupLocation = WindowStartupLocation.CenterOwner;
    saveReport.Owner = App.Current.Windows[0];
    
    if (saveReport.ShowDialog() == true)
    {
        string reportName = saveReport.reportName;
        
        // Save to database
        con.Open();
        SqlCeCommand sqlCmd = new SqlCeCommand(
            "INSERT INTO ReportsTable VALUES(@ReportName, @Report)", con);
        sqlCmd.Parameters.Add("@ReportName", reportName);
        sqlCmd.Parameters.Add("@Report", reportStream.ToArray());
        sqlCmd.ExecuteNonQuery();
        con.Close();
        
        this.ReportNames.Add(saveReport.reportName);
    }
}
```

**VB.NET:**

```vbnet
Dim stream As Stream = Me.olapClient1.GetReportStream()

If stream IsNot Nothing Then
    Dim reportStream As MemoryStream = TryCast(stream, MemoryStream)
    Dim saveReport As New ReportNameWindow(Me.reportNames)
    
    If saveReport.ShowDialog() = True Then
        Dim reportName As String = "SalesReport"
        
        con.Open()
        Dim cmd1 As SqlCeCommand = New SqlCeCommand(
            "INSERT INTO ReportsTable VALUES(@ReportName, @Report)", con)
        cmd1.Parameters.Add("@ReportName", reportName)
        cmd1.Parameters.Add("@Report", reportStream.ToArray())
        cmd1.ExecuteNonQuery()
        con.Close()
    End If
End If
```

#### Loading Report as Stream from Database

```csharp
string reportName = "RevenueReport";
Stream reportStream = null;

// Retrieve from database
con.Open();
SqlCeDataAdapter da = new SqlCeDataAdapter("SELECT * FROM ReportsTable", con);
DataSet dSet = new DataSet();
da.Fill(dSet);
DataTable table = dSet.Tables[0];

foreach (DataRow row in table.Rows)
{
    if ((row.ItemArray[0] as string).Equals(reportName))
    {
        reportStream = new MemoryStream(row.ItemArray[1] as byte[]);
        break;
    }
}

// Load the stream into OLAP Client
this.olapClient1.LoadReportStream(reportStream);
con.Close();
```

**VB.NET:**

```vbnet
Dim reportName As String = "RevenueReport"
Dim reportStream As Stream = Nothing

con.Open()
Dim da As SqlCeDataAdapter = New SqlCeDataAdapter("SELECT * FROM ReportsTable", con)
Dim dSet As DataSet = New DataSet()
da.Fill(dSet)
Dim table As DataTable = dSet.Tables(0)

For Each row As DataRow In table.Rows
    If (TryCast(row.ItemArray(0), String)).Equals(reportName) Then
        reportStream = New MemoryStream(TryCast(row.ItemArray(1), Byte()))
        Exit For
    End If
Next row

Me.olapClient1.LoadReportStream(reportStream)
con.Close()
```

## Use Cases

### OLAP Report Creator

**Scenario:** Power users create standardized reports for team

**Workflow:**
1. Design multiple reports covering common analyses
2. Save report collection to file
3. Distribute to team members
4. Team loads reports and uses immediately

### OLAP Report Viewer

**Scenario:** Business users view pre-built reports without modification

**Workflow:**
1. Load standardized report collection
2. Switch between reports via Report List
3. Export visualizations and data
4. No need to understand report creation

### Central Report Repository

**Scenario:** Organization maintains library of OLAP reports

**Solution:**
1. Store reports as streams in database
2. Users browse available reports
3. Select and load desired reports
4. Central management and version control

### Scheduled Report Generation

**Scenario:** Automated nightly report generation

**Solution:**
1. Load report template from database
2. Refresh data from cube
3. Export to PDF/Excel
4. Email to stakeholders
5. Store updated report back to database

### Multi-Environment Deployment

**Scenario:** Develop reports in test, deploy to production

**Workflow:**
1. Create and test reports in development environment
2. Save reports to files
3. Transport files to production
4. Load reports in production environment
5. Reports connect to production cubes

## Best Practices

### File-Based vs Stream-Based Storage

**Use File-Based (XML) When:**
- Simple deployment scenarios
- User manages own reports
- Sharing via email or file shares
- No centralized report management needed

**Use Stream-Based (Database) When:**
- Centralized report repository required
- Version control and audit trail needed
- Integration with application database
- Multi-user report sharing platform
- Metadata tracking (author, creation date, usage stats)

### Report Naming Conventions

**Good Report Names:**
- Descriptive: "Q4 Sales by Region"
- Purpose-driven: "Executive Dashboard - Weekly"
- Time-stamped: "Inventory Analysis 2025-03"
- Audience-specific: "CFO Monthly Financial Review"

**Naming Guidelines:**
- Include purpose or content description
- Add time period if relevant
- Specify audience if applicable
- Avoid generic names like "Report1", "Test", "Temp"

### Report Collection Organization

**Strategy 1: Thematic Collections**
- One file per business area (Sales, Finance, Operations)
- Multiple related reports in each collection
- Easy to share entire business area analyses

**Strategy 2: Single Report Files**
- One report per file
- More granular sharing
- Easier version control
- Users assemble own collections

**Strategy 3: Template Libraries**
- Standard templates saved separately
- Users load template, customize, save as new file
- Maintains consistency across organization

### Version Control

**Manual Versioning:**
- Include version in filename: `SalesReport_v1.0.xml`
- Date stamps: `SalesReport_2025-03-24.xml`
- Maintain change log document

**Database Versioning:**
```sql
CREATE TABLE ReportsTable (
    ReportID INT PRIMARY KEY,
    ReportName NVARCHAR(255),
    ReportData VARBINARY(MAX),
    Version INT,
    CreatedBy NVARCHAR(100),
    CreatedDate DATETIME,
    ModifiedDate DATETIME
)
```

### Security and Permissions

**File-Based Security:**
- Use file system permissions
- Encrypt sensitive reports
- Secure network shares

**Database Security:**
- Implement row-level security
- Track user access and modifications
- Audit report usage
- Encrypt sensitive data columns

### Error Handling

**Robust Save Operation:**

```csharp
try
{
    Stream stream = this.olapClient1.GetReportStream();
    if (stream != null)
    {
        MemoryStream reportStream = stream as MemoryStream;
        // Database save logic
        con.Open();
        // ... save operations ...
        con.Close();
        
        MessageBox.Show("Report saved successfully!", "Success");
    }
    else
    {
        MessageBox.Show("Unable to create report stream.", "Error");
    }
}
catch (SqlException ex)
{
    MessageBox.Show($"Database error: {ex.Message}", "Error");
}
catch (Exception ex)
{
    MessageBox.Show($"Error saving report: {ex.Message}", "Error");
}
finally
{
    if (con.State == ConnectionState.Open)
        con.Close();
}
```

**Robust Load Operation:**

```csharp
try
{
    if (reportStream != null)
    {
        this.olapClient1.LoadReportStream(reportStream);
        MessageBox.Show("Report loaded successfully!", "Success");
    }
    else
    {
        MessageBox.Show("Report not found in database.", "Error");
    }
}
catch (Exception ex)
{
    MessageBox.Show($"Error loading report: {ex.Message}", "Error");
}
```

## Sample Demonstrations

Complete samples demonstrating report serialization are available at:

**Location:**  
`{system drive}:\Users\<User Name>\AppData\Local\Syncfusion\EssentialStudio\<Version Number>\WPF\OlapClient.WPF\Samples\Serialization\Report Serialization`

## Troubleshooting

### Save Fails

**Possible Causes:**
- No write permissions to destination
- Disk full
- File already open in another application

**Solutions:**
- Check folder permissions
- Verify disk space
- Close any applications using the file

### Load Fails

**Possible Causes:**
- Cube not accessible
- Measures/dimensions removed from cube
- Corrupted XML file
- Incompatible cube structure

**Solutions:**
- Verify cube connection
- Check cube structure matches saved report
- Validate XML file integrity
- Use compatible cube version

### Database Storage Fails

**Possible Causes:**
- Connection string invalid
- Insufficient database permissions
- Table doesn't exist
- BLOB size exceeds column capacity

**Solutions:**
- Verify connection string
- Check database permissions
- Create required table schema
- Increase VARBINARY column size if needed

### Report Collection Confusion

**Problem:** Users accidentally clear reports with New Report

**Solution:**
- Train users on New vs Add Report difference
- Implement confirmation dialog for New Report
- Auto-save before creating new report

## Next Steps

After mastering report management:

1. Explore exporting reports - see [exporting.md](exporting.md)
2. Learn about filtering for focused reports - see [filtering.md](filtering.md)
3. Understand calculated members for custom metrics - see [calculated-members.md](calculated-members.md)

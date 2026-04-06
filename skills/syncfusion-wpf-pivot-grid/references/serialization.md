# Serialization and State Persistence in WPF PivotGrid

This guide covers serialization, deserialization, and state persistence capabilities for saving and loading PivotGrid configurations in the Syncfusion WPF PivotGrid control.

## Serialization Overview

Serialization allows you to save the PivotGrid structure and configuration to an XML file, which can be loaded at any time. This is useful for applications that need to persist their data and structure after closing.

## Serializable Properties

The following PivotGrid properties can be serialized:

| Property Name | Type | Description |
|--------------|------|-------------|
| AllowResizeColumns | bool | Allow column resizing |
| AllowResizeRows | bool | Allow row resizing |
| AllowSelection | bool | Enable cell selection |
| AllowSelectionWithHeaders | bool | Include headers in selection |
| AutoSizeColumnCount | int | Number of columns to auto-size |
| AutoSizeOption | GridAutoSizeOption | Auto-size configuration |
| AutoSizeRowCount | int | Number of rows to auto-size |
| DeferLayoutUpdate | bool | Defer layout updates |
| Filters | ObservableCollection&lt;FilterExpression&gt; | Filter expressions |
| FreezeHeaders | bool | Freeze row/column headers |
| IsDynamicData | bool | Dynamic data flag |
| PivotCalculations | ObservableCollection&lt;PivotComputationInfo&gt; | Calculation definitions |
| PivotColumns | ObservableCollection&lt;PivotItem&gt; | Column pivot items |
| PivotFields | ObservableCollection&lt;PivotItem&gt; | Field definitions |
| PivotRows | ObservableCollection&lt;PivotItem&gt; | Row pivot items |
| ShowCalculationsAsColumns | bool | Display calculations as columns |
| ShowFieldList | bool | Show field list |
| ShowGrandTotals | bool | Display grand totals |
| ShowGroupingBar | bool | Display grouping bar |
| GroupingBar.AllowFiltering | bool | Allow filtering in grouping bar |
| GroupingBar.AllowSorting | bool | Allow sorting in grouping bar |
| EnableColumnHeader | bool | Enable column headers |
| EnableRowHeader | bool | Enable row headers |
| AllowMultiFunctionalSortFilter | bool | Multi-functional sort/filter |
| VisibleRecords | int | Number of visible records |

## Serialization Methods

### Available Methods

| Method | Description | Parameters | Return Type |
|--------|-------------|------------|-------------|
| Serialize() | Serializes to XML using save file dialog | - | void |
| Deserialize() | Deserializes from XML using open file dialog | - | void |
| Serialize(string fileName) | Serializes to specified XML file | string fileName | void |
| Deserialize(string fileName) | Deserializes from specified XML file | string fileName | void |
| SerializedXmlString() | Serializes properties to XML string | - | string |
| DeserializeXmlString(string xmlString) | Deserializes from XML string | string xmlString | void |

## Basic Serialization

### Serialize with File Dialog

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        this.grid1.Children.Add(pivotGrid);
        
        // Configure PivotGrid...
    }

    void button1_Click(object sender, RoutedEventArgs e)
    {
        // Serialize the PivotGrid using file dialog
        pivotGrid.Serialize();
    }
}
```

### Serialize to Specific File

```csharp
void SaveConfiguration_Click(object sender, RoutedEventArgs e)
{
    try
    {
        // Serialize to specific location
        pivotGrid.Serialize(@"C:\PivotGrid\config.xml");
        
        MessageBox.Show("Configuration saved successfully!", 
            "Save", MessageBoxButton.OK, 
            MessageBoxImage.Information);
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Save failed: {ex.Message}", 
            "Error", MessageBoxButton.OK, 
            MessageBoxImage.Error);
    }
}
```

### Serialize to XML String

```csharp
void SaveConfigurationString_Click(object sender, RoutedEventArgs e)
{
    try
    {
        // Serialize to XML string
        string xmlConfig = pivotGrid.SerializedXmlString();
        
        // Save to database, settings, or other storage
        SaveToDatabase(xmlConfig);
        
        MessageBox.Show("Configuration saved!", 
            "Save", MessageBoxButton.OK, 
            MessageBoxImage.Information);
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Save failed: {ex.Message}", 
            "Error", MessageBoxButton.OK, 
            MessageBoxImage.Error);
    }
}

private void SaveToDatabase(string xmlConfig)
{
    // Your database save logic
}
```

## Basic Deserialization

### Deserialize with File Dialog

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        this.grid1.Children.Add(pivotGrid);
    }

    void button1_Click(object sender, RoutedEventArgs e)
    {
        // Deserialize the PivotGrid using file dialog
        pivotGrid.Deserialize();
    }
}
```

### Deserialize from Specific File

```csharp
void LoadConfiguration_Click(object sender, RoutedEventArgs e)
{
    try
    {
        // Deserialize from specific location
        pivotGrid.Deserialize(@"C:\PivotGrid\config.xml");
        
        MessageBox.Show("Configuration loaded successfully!", 
            "Load", MessageBoxButton.OK, 
            MessageBoxImage.Information);
    }
    catch (FileNotFoundException)
    {
        MessageBox.Show("Configuration file not found!", 
            "Error", MessageBoxButton.OK, 
            MessageBoxImage.Error);
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Load failed: {ex.Message}", 
            "Error", MessageBoxButton.OK, 
            MessageBoxImage.Error);
    }
}
```

### Deserialize from XML String

```csharp
void LoadConfigurationString_Click(object sender, RoutedEventArgs e)
{
    try
    {
        // Load XML string from database or settings
        string xmlConfig = LoadFromDatabase();
        
        // Deserialize from XML string
        pivotGrid.DeserializeXmlString(xmlConfig);
        
        MessageBox.Show("Configuration loaded!", 
            "Load", MessageBoxButton.OK, 
            MessageBoxImage.Information);
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Load failed: {ex.Message}", 
            "Error", MessageBoxButton.OK, 
            MessageBoxImage.Error);
    }
}

private string LoadFromDatabase()
{
    // Your database load logic
    return string.Empty;
}
```

## Expand/Collapse State Management

### IgnoreExpandCollapseOnSerialization Property

By default, expand and collapse states of PivotGrid cells are maintained during serialization. The item source during deserialization should match the source used during serialization.

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        this.grid1.Children.Add(pivotGrid);
        
        // Ignore expand/collapse state during serialization
        pivotGrid.IgnoreExpandCollapseOnSerialization = true;
    }
}
```

## State Persistence

State persistence allows you to maintain the collapsed or expanded state when schema items are changed.

### Enable State Persistence

**XAML Configuration**:

```xml
<Grid>
    <syncfusion:PivotGridControl 
        HorizontalAlignment="Left" 
        Name="pivotGrid" 
        VerticalAlignment="Top" 
        StatePersistenceEnabled="True" 
        ItemSource="{Binding Source={StaticResource data}}">

        <syncfusion:PivotGridControl.PivotRows>
            <syncfusion:PivotItem 
                FieldHeader="Product" 
                FieldMappingName="Product" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="Date" 
                FieldMappingName="Date" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotRows>

        <syncfusion:PivotGridControl.PivotColumns>
            <syncfusion:PivotItem 
                FieldHeader="Country" 
                FieldMappingName="Country" 
                TotalHeader="Total" />
            <syncfusion:PivotItem 
                FieldHeader="State" 
                FieldMappingName="State" 
                TotalHeader="Total" />
        </syncfusion:PivotGridControl.PivotColumns>

        <syncfusion:PivotGridControl.PivotCalculations>
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Amount" 
                Format="C" 
                SummaryType="DoubleTotalSum" />
            <syncfusion:PivotComputationInfo 
                CalculationName="Total" 
                FieldName="Quantity" 
                SummaryType="Count" />
        </syncfusion:PivotGridControl.PivotCalculations>

    </syncfusion:PivotGridControl>
</Grid>
```

**Code-Behind**:

```csharp
public partial class MainWindow : Window
{
    PivotGridControl pivotGrid = new PivotGridControl();
    
    public MainWindow()
    {
        InitializeComponent();
        grid1.Children.Add(pivotGrid);
        pivotGrid.ItemSource = ProductSales.GetSalesData();
        
        // Configure PivotRows, PivotColumns, and PivotCalculations...
        
        // Enable state persistence
        pivotGrid.StatePersistenceEnabled = true;
    }
}
```

## Advanced Serialization Patterns

### Configuration Manager

```csharp
public class PivotGridConfigurationManager
{
    private readonly PivotGridControl pivotGrid;
    private readonly string configDirectory;
    
    public PivotGridConfigurationManager(PivotGridControl grid, string directory)
    {
        pivotGrid = grid;
        configDirectory = directory;
        
        if (!Directory.Exists(configDirectory))
            Directory.CreateDirectory(configDirectory);
    }
    
    public bool SaveConfiguration(string configName)
    {
        try
        {
            string filePath = Path.Combine(configDirectory, $"{configName}.xml");
            pivotGrid.Serialize(filePath);
            return true;
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Save failed: {ex.Message}");
            return false;
        }
    }
    
    public bool LoadConfiguration(string configName)
    {
        try
        {
            string filePath = Path.Combine(configDirectory, $"{configName}.xml");
            if (File.Exists(filePath))
            {
                pivotGrid.Deserialize(filePath);
                return true;
            }
            return false;
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Load failed: {ex.Message}");
            return false;
        }
    }
    
    public List<string> GetSavedConfigurations()
    {
        var configs = new List<string>();
        
        if (Directory.Exists(configDirectory))
        {
            var files = Directory.GetFiles(configDirectory, "*.xml");
            foreach (var file in files)
            {
                configs.Add(Path.GetFileNameWithoutExtension(file));
            }
        }
        
        return configs;
    }
    
    public bool DeleteConfiguration(string configName)
    {
        try
        {
            string filePath = Path.Combine(configDirectory, $"{configName}.xml");
            if (File.Exists(filePath))
            {
                File.Delete(filePath);
                return true;
            }
            return false;
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Delete failed: {ex.Message}");
            return false;
        }
    }
}
```

### User Preference Manager

```csharp
public class UserPreferenceManager
{
    private readonly PivotGridControl pivotGrid;
    private readonly string userName;
    
    public UserPreferenceManager(PivotGridControl grid, string user)
    {
        pivotGrid = grid;
        userName = user;
    }
    
    public void SaveUserPreferences()
    {
        string xmlConfig = pivotGrid.SerializedXmlString();
        
        // Save to user settings
        Properties.Settings.Default[$"PivotConfig_{userName}"] = xmlConfig;
        Properties.Settings.Default.Save();
    }
    
    public void LoadUserPreferences()
    {
        string xmlConfig = Properties.Settings.Default[$"PivotConfig_{userName}"] as string;
        
        if (!string.IsNullOrEmpty(xmlConfig))
        {
            pivotGrid.DeserializeXmlString(xmlConfig);
        }
    }
    
    public void ClearUserPreferences()
    {
        Properties.Settings.Default[$"PivotConfig_{userName}"] = string.Empty;
        Properties.Settings.Default.Save();
    }
}
```

### Auto-Save Configuration

```csharp
public class AutoSaveManager
{
    private readonly PivotGridControl pivotGrid;
    private readonly DispatcherTimer autoSaveTimer;
    private readonly string autoSaveFile;
    
    public AutoSaveManager(PivotGridControl grid, string saveFile, TimeSpan interval)
    {
        pivotGrid = grid;
        autoSaveFile = saveFile;
        
        autoSaveTimer = new DispatcherTimer
        {
            Interval = interval
        };
        autoSaveTimer.Tick += AutoSaveTimer_Tick;
    }
    
    public void StartAutoSave()
    {
        autoSaveTimer.Start();
    }
    
    public void StopAutoSave()
    {
        autoSaveTimer.Stop();
    }
    
    private void AutoSaveTimer_Tick(object sender, EventArgs e)
    {
        try
        {
            pivotGrid.Serialize(autoSaveFile);
            Console.WriteLine($"Auto-saved at {DateTime.Now}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Auto-save failed: {ex.Message}");
        }
    }
    
    public void RestoreFromAutoSave()
    {
        if (File.Exists(autoSaveFile))
        {
            try
            {
                pivotGrid.Deserialize(autoSaveFile);
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Restore failed: {ex.Message}");
            }
        }
    }
}
```

### Version Control

```csharp
public class ConfigurationVersionControl
{
    private readonly string baseDirectory;
    
    public ConfigurationVersionControl(string directory)
    {
        baseDirectory = directory;
        
        if (!Directory.Exists(baseDirectory))
            Directory.CreateDirectory(baseDirectory);
    }
    
    public void SaveVersion(PivotGridControl grid, string versionName)
    {
        string fileName = $"{versionName}_{DateTime.Now:yyyyMMddHHmmss}.xml";
        string filePath = Path.Combine(baseDirectory, fileName);
        grid.Serialize(filePath);
    }
    
    public List<ConfigVersion> GetVersions()
    {
        var versions = new List<ConfigVersion>();
        
        if (Directory.Exists(baseDirectory))
        {
            var files = Directory.GetFiles(baseDirectory, "*.xml");
            foreach (var file in files)
            {
                var info = new FileInfo(file);
                versions.Add(new ConfigVersion
                {
                    FileName = info.Name,
                    FilePath = info.FullName,
                    Created = info.CreationTime,
                    Size = info.Length
                });
            }
        }
        
        return versions.OrderByDescending(v => v.Created).ToList();
    }
    
    public void LoadVersion(PivotGridControl grid, string filePath)
    {
        if (File.Exists(filePath))
        {
            grid.Deserialize(filePath);
        }
    }
}

public class ConfigVersion
{
    public string FileName { get; set; }
    public string FilePath { get; set; }
    public DateTime Created { get; set; }
    public long Size { get; set; }
}
```

### Database Serialization

```csharp
public class DatabaseConfigurationManager
{
    private readonly PivotGridControl pivotGrid;
    private readonly string connectionString;
    
    public DatabaseConfigurationManager(PivotGridControl grid, string connString)
    {
        pivotGrid = grid;
        connectionString = connString;
    }
    
    public async Task SaveToDatabase(int userId, string configName)
    {
        string xmlConfig = pivotGrid.SerializedXmlString();
        
        using (var connection = new SqlConnection(connectionString))
        {
            await connection.OpenAsync();
            
            string query = @"
                INSERT INTO PivotGridConfigurations 
                (UserId, ConfigName, XmlConfig, CreatedDate) 
                VALUES (@UserId, @ConfigName, @XmlConfig, @CreatedDate)";
            
            using (var command = new SqlCommand(query, connection))
            {
                command.Parameters.AddWithValue("@UserId", userId);
                command.Parameters.AddWithValue("@ConfigName", configName);
                command.Parameters.AddWithValue("@XmlConfig", xmlConfig);
                command.Parameters.AddWithValue("@CreatedDate", DateTime.Now);
                
                await command.ExecuteNonQueryAsync();
            }
        }
    }
    
    public async Task LoadFromDatabase(int userId, string configName)
    {
        using (var connection = new SqlConnection(connectionString))
        {
            await connection.OpenAsync();
            
            string query = @"
                SELECT XmlConfig 
                FROM PivotGridConfigurations 
                WHERE UserId = @UserId AND ConfigName = @ConfigName";
            
            using (var command = new SqlCommand(query, connection))
            {
                command.Parameters.AddWithValue("@UserId", userId);
                command.Parameters.AddWithValue("@ConfigName", configName);
                
                var result = await command.ExecuteScalarAsync();
                
                if (result != null)
                {
                    string xmlConfig = result.ToString();
                    pivotGrid.DeserializeXmlString(xmlConfig);
                }
            }
        }
    }
}
```

## Best Practices

### Serialization Strategy

1. **Save Location**: Use application data folder for configuration files
2. **File Naming**: Use descriptive names with timestamps
3. **Compression**: Consider compressing XML for large configurations
4. **Validation**: Validate XML before deserialization

### Error Handling

```csharp
public bool SafeSerialize(string filePath)
{
    try
    {
        // Backup existing file
        if (File.Exists(filePath))
        {
            string backupPath = filePath + ".bak";
            File.Copy(filePath, backupPath, true);
        }
        
        // Serialize
        pivotGrid.Serialize(filePath);
        return true;
    }
    catch (UnauthorizedAccessException)
    {
        MessageBox.Show("Access denied to configuration file");
        return false;
    }
    catch (IOException ex)
    {
        MessageBox.Show($"IO Error: {ex.Message}");
        return false;
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Serialization failed: {ex.Message}");
        return false;
    }
}
```

### State Persistence Guidelines

1. **Enable for User Experience**: Maintain expand/collapse states
2. **Consistent Data Source**: Use same data source for serialize/deserialize
3. **Version Compatibility**: Handle configuration version changes
4. **Default Configuration**: Provide fallback if deserialization fails

### Performance Considerations

```csharp
public async Task SerializeAsync(string filePath)
{
    await Task.Run(() =>
    {
        pivotGrid.Serialize(filePath);
    });
}

public async Task DeserializeAsync(string filePath)
{
    await Task.Run(() =>
    {
        pivotGrid.Deserialize(filePath);
    });
}
```

### Configuration Validation

```csharp
public bool ValidateConfiguration(string xmlConfig)
{
    try
    {
        var doc = XDocument.Parse(xmlConfig);
        
        // Check for required elements
        if (doc.Root == null)
            return false;
        
        // Validate structure
        // Add your validation logic
        
        return true;
    }
    catch
    {
        return false;
    }
}
```

## Summary

Serialization and state persistence in the WPF PivotGrid provides:

- **XML Serialization**: Save/load PivotGrid configuration to XML files
- **String Serialization**: Serialize to XML strings for database or settings storage
- **Expand/Collapse States**: Maintain cell states across sessions
- **State Persistence**: Preserve expanded/collapsed state during schema changes
- **Configuration Management**: Advanced patterns for managing multiple configurations
- **Version Control**: Track configuration changes over time
- **Database Integration**: Store configurations in databases
- **Best Practices**: Error handling, validation, and performance optimization

Use serialization and state persistence to provide a seamless user experience, allowing users to save and restore their PivotGrid configurations across sessions.

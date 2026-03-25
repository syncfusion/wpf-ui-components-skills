# Advanced Features in WPF DataGrid

## Table of Contents
- [Localization](#localization)
- [Serialization and Deserialization](#serialization-and-deserialization)

## Localization

### Setting Culture

Change application culture before InitializeComponent to localize DataGrid:

```csharp
public MainWindow()
{
    System.Threading.Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("de-DE");
    InitializeComponent();
}
```

### Creating Resource Files

1. Create **Resources** folder in application
2. Add default resource file: **Syncfusion.SfGrid.WPF.resx**
3. Create culture-specific file: **Syncfusion.SfGrid.WPF.de.resx** (for German)
4. Add Name/Value pairs for localized strings

**Resource File Structure:**

| Name | Value (English) | Value (German) |
|------|----------------|----------------|
| AddNewRow | Add New Row | Neue Zeile hinzufügen |
| FilterDateTimePopup_Today | Today | Heute |
| FilterDateTimePopup_Yesterday | Yesterday | Gestern |
| Ok | OK | OK |
| Cancel | Cancel | Abbrechen |
| SortAscending | Sort Ascending | Aufsteigend sortieren |
| SortDescending | Sort Descending | Absteigend sortieren |
| ClearSorting | Clear Sorting | Sortierung löschen |
| GroupColumn | Group this Column | Diese Spalte gruppieren |
| ShowGroupDropArea | Show Group Drop Area | Gruppenablegebereich anzeigen |

### Custom Assembly or Namespace

If resource file is in different assembly or namespace:

```csharp
public MainWindow()
{
    System.Threading.Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("de-DE");
    Syncfusion.UI.Xaml.Grid.GridResourceWrapper.SetResources("Assembly_name", "namespace_name");
    InitializeComponent();
}
```

### Editing Default Resource

Modify default resource file to change English strings:

```csharp
// Add Syncfusion.SfGrid.WPF.resx to Resources folder
// Modify Name/Value pairs in Resource Designer
```

## Serialization and Deserialization

### Serialize to File

Save DataGrid settings to XML file:

```csharp
using (var file = File.Create("DataGrid.xml"))
{
    dataGrid.Serialize(file);
}
```

### Serialize to Stream

```csharp
FileStream stream = new FileStream("DataGrid", FileMode.Create);
this.dataGrid.Serialize(stream);
```

### Serialization Options

Control what gets serialized:

```csharp
using (var file = File.Create("DataGrid.xml"))
{
    SerializationOptions options = new SerializationOptions
    {
        SerializeSorting = false,
        SerializeGrouping = false,
        SerializeFiltering = false,
        SerializeColumns = false,
        SerializeCaptionSummary = false,
        SerializeGroupSummaries = false,
        SerializeTableSummaries = false,
        SerializeStackedHeaders = false,
        SerializeDetailsViewDefinition = false,
        SerializeUnBoundRows = false
    };
    dataGrid.Serialize(file, options);
}
```

### Deserialize from File

Load DataGrid settings from XML file:

```csharp
using (var file = File.Open("DataGrid.xml", FileMode.Open))
{
    dataGrid.Deserialize(file);
}
```

### Deserialize from Stream

```csharp
FileStream fileStream = new FileStream("DataGrid", FileMode.Open);
this.dataGrid.Deserialize(fileStream);
```

### Deserialization Options

Control what gets deserialized:

```csharp
using (var file = File.Open("DataGrid.xml", FileMode.Open))
{
    DeserializationOptions options = new DeserializationOptions
    {
        DeserializeSorting = false,
        DeserializeGrouping = false,
        DeserializeFiltering = false,
        DeserializeColumns = false,
        DeserializeCaptionSummary = false,
        DeserializeGroupSummaries = false,
        DeserializeTableSummaries = false,
        DeserializeStackedHeaders = false,
        DeserializeDetailsViewDefinition = false,
        DeserializeUnBoundRows = false
    };
    dataGrid.Deserialize(file, options);
}
```

### Custom Column Serialization

Create custom column that can be serialized:

```csharp
// 1. Define serializable custom column
[DataContract(Name = "SerializableCustomGridColumn")]
public class SerializableCustomGridColumn : SerializableGridColumn
{
    [DataMember]
    public string DateMappingName { get; set; }
}

// 2. Create custom SerializationController
dataGrid.SerializationController = new SerializationControllerExt(dataGrid);

public class SerializationControllerExt : SerializationController
{
    public SerializationControllerExt(SfDataGrid dataGrid) : base(dataGrid) { }
    
    protected override SerializableGridColumn GetSerializableGridColumn(GridColumn column)
    {
        if (column.MappingName == "OrderDate")
            return new SerializableCustomGridColumn();
        return base.GetSerializableGridColumn(column);
    }
    
    protected override void StoreGridColumnProperties(GridColumn column, SerializableGridColumn serializableColumn)
    {
        base.StoreGridColumnProperties(column, serializableColumn);
        if (column is DatePickerColumn)
            (serializableColumn as SerializableCustomGridColumn).DateMappingName = (column as DatePickerColumn).DateMappingName;
    }
    
    public override Type[] KnownTypes()
    {
        var types = base.KnownTypes();
        var list = types.Cast<Type>().ToList();
        list.Add(typeof(SerializableCustomGridColumn));
        return list.ToArray();
    }
    
    protected override GridColumn GetGridColumn(SerializableGridColumn serializableColumn)
    {
        if (serializableColumn is SerializableCustomGridColumn)
            return new DatePickerColumn();
        return base.GetGridColumn(serializableColumn);
    }
    
    protected override void RestoreColumnProperties(SerializableGridColumn serializableColumn, GridColumn column)
    {
        base.RestoreColumnProperties(serializableColumn, column);
        if (column is DatePickerColumn)
            (column as DatePickerColumn).DateMappingName = (serializableColumn as SerializableCustomGridColumn).DateMappingName;
    }
}
```

### Serializing Template Columns

Restore template content during deserialization:

```xaml
<Application.Resources>
    <DataTemplate x:Key="cellTemplate">
        <Button Content="{Binding Value}" />
    </DataTemplate>
</Application.Resources>

<syncfusion:SfDataGrid x:Name="dataGrid" AutoGenerateColumns="False" ItemsSource="{Binding Orders}">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTemplateColumn CellTemplate="{StaticResource cellTemplate}"
                                       HeaderText="Order ID"
                                       MappingName="OrderID"
                                       SetCellBoundValue="True" />
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>
```

```csharp
this.dataGrid.SerializationController = new SerializationControllerExt(this.dataGrid);

public class SerializationControllerExt : SerializationController
{
    public SerializationControllerExt(SfDataGrid grid) : base(grid) { }
    
    protected override void RestoreColumnProperties(SerializableGridColumn serializableColumn, GridColumn column)
    {
        base.RestoreColumnProperties(serializableColumn, column);
        if (column is GridTemplateColumn && column.MappingName == "OrderID")
        {
            column.CellTemplate = App.Current.Resources["cellTemplate"] as DataTemplate;
        }
    }
}
```

# Localization and Globalization

Configure localization, flow direction, and culture-specific formatting for the WPF Gantt control.

## Flow Direction

Support right-to-left (RTL) languages like Arabic and Hebrew.

### Enable RTL Flow

```csharp
// Set flow direction for entire control
ganttControl.FlowDirection = FlowDirection.RightToLeft;
```

### XAML Configuration

```xml
<syncfusion:GanttControl FlowDirection="RightToLeft"
                         ItemsSource="{Binding Tasks}">
</syncfusion:GanttControl>
```

### Conditional Flow Direction

```csharp
private void SetFlowDirectionByCulture(string cultureName)
{
    var culture = new CultureInfo(cultureName);
    
    // RTL cultures
    if (culture.TextInfo.IsRightToLeft)
    {
        ganttControl.FlowDirection = FlowDirection.RightToLeft;
    }
    else
    {
        ganttControl.FlowDirection = FlowDirection.LeftToRight;
    }
}

// Usage
SetFlowDirectionByCulture("ar-SA"); // Arabic (Saudi Arabia)
SetFlowDirectionByCulture("he-IL"); // Hebrew (Israel)
```

## Localization

Localize UI text, labels, and messages for different languages.

### Using Resource Files

**Create resource files:**
- `Resources.resx` (default/English)
- `Resources.ar.resx` (Arabic)
- `Resources.fr.resx` (French)
- `Resources.de.resx` (German)

**Resources.resx:**
```
TaskName = Task Name
StartDate = Start Date
EndDate = End Date
Duration = Duration
Progress = Progress
Resources = Resources
```

**Resources.ar.resx:**
```
TaskName = اسم المهمة
StartDate = تاريخ البدء
EndDate = تاريخ الانتهاء
Duration = المدة
Progress = التقدم
Resources = الموارد
```

### Apply Localization

```csharp
using System.Globalization;
using System.Threading;

private void SetCulture(string cultureName)
{
    var culture = new CultureInfo(cultureName);
    
    // Set thread culture
    Thread.CurrentThread.CurrentCulture = culture;
    Thread.CurrentThread.CurrentUICulture = culture;
    
    // Update column headers
    UpdateColumnHeaders();
}

private void UpdateColumnHeaders()
{
    ganttControl.InternalGrid.Columns[0].HeaderText = Resources.TaskName;
    ganttControl.InternalGrid.Columns[1].HeaderText = Resources.StartDate;
    ganttControl.InternalGrid.Columns[2].HeaderText = Resources.EndDate;
    ganttControl.InternalGrid.Columns[3].HeaderText = Resources.Duration;
    ganttControl.InternalGrid.Columns[4].HeaderText = Resources.Progress;
}
```

### Syncfusion Localization Support

```csharp
using Syncfusion.Windows.Shared;

// Load Syncfusion localization assembly
LocalizationManager.SetResources(typeof(MainWindow).Assembly, "YourNamespace");
```

## Culture-Specific Formatting

Format dates, numbers, and currency based on culture.

### Date Formatting

```csharp
private void ApplyDateFormatting(string cultureName)
{
    var culture = new CultureInfo(cultureName);
    
    // Configure date format
    ganttControl.ScheduleHeaderDateFormat = culture.DateTimeFormat.ShortDatePattern;
    
    // Examples:
    // en-US: "M/d/yyyy"
    // fr-FR: "dd/MM/yyyy"
    // de-DE: "dd.MM.yyyy"
    // ja-JP: "yyyy/MM/dd"
}
```

### Number Formatting

```csharp
// Format progress percentage
private string FormatProgress(double progress, CultureInfo culture)
{
    return progress.ToString("P0", culture); // e.g., "75%" or "75 %"
}

// Format duration
private string FormatDuration(double days, CultureInfo culture)
{
    return string.Format(culture, Resources.DurationDays, days);
    // en-US: "5 days"
    // fr-FR: "5 jours"
    // de-DE: "5 Tage"
}
```

### Currency Formatting

```csharp
private string FormatCost(double cost, CultureInfo culture)
{
    return cost.ToString("C", culture);
    // en-US: "$1,234.56"
    // en-GB: "£1,234.56"
    // fr-FR: "1 234,56 €"
    // de-DE: "1.234,56 €"
}
```

## Custom Column Formatting by Culture

```xml
<syncfusion:GanttGrid.Columns>
    <syncfusion:GanttColumn HeaderText="Start Date" MappingName="StartDate" Width="120">
        <syncfusion:GanttColumn.CellTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding StartDate, StringFormat={}{0:d}}" />
                <!-- Uses current culture's short date pattern -->
            </DataTemplate>
        </syncfusion:GanttColumn.CellTemplate>
    </syncfusion:GanttColumn>
    
    <syncfusion:GanttColumn HeaderText="Cost" MappingName="Cost" Width="100">
        <syncfusion:GanttColumn.CellTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding Cost, StringFormat={}{0:C}}" />
                <!-- Uses current culture's currency format -->
            </DataTemplate>
        </syncfusion:GanttColumn.CellTemplate>
    </syncfusion:GanttColumn>
</syncfusion:GanttGrid.Columns>
```

## Complete Localization Example

```csharp
public class LocalizationViewModel : INotifyPropertyChanged
{
    private GanttControl ganttControl;
    
    public ObservableCollection<CultureInfo> AvailableCultures { get; set; }
    public CultureInfo SelectedCulture { get; set; }
    
    public LocalizationViewModel(GanttControl gantt)
    {
        ganttControl = gantt;
        InitializeAvailableCultures();
    }
    
    private void InitializeAvailableCultures()
    {
        AvailableCultures = new ObservableCollection<CultureInfo>
        {
            new CultureInfo("en-US"), // English (United States)
            new CultureInfo("fr-FR"), // French (France)
            new CultureInfo("de-DE"), // German (Germany)
            new CultureInfo("es-ES"), // Spanish (Spain)
            new CultureInfo("ar-SA"), // Arabic (Saudi Arabia)
            new CultureInfo("zh-CN"), // Chinese (Simplified)
            new CultureInfo("ja-JP"), // Japanese (Japan)
        };
        
        SelectedCulture = CultureInfo.CurrentCulture;
    }
    
    public void ApplyCulture(CultureInfo culture)
    {
        // Set thread culture
        Thread.CurrentThread.CurrentCulture = culture;
        Thread.CurrentThread.CurrentUICulture = culture;
        
        // Set flow direction
        ganttControl.FlowDirection = culture.TextInfo.IsRightToLeft
            ? FlowDirection.RightToLeft
            : FlowDirection.LeftToRight;
        
        // Update UI text
        UpdateLocalizedText();
        
        // Refresh display
        ganttControl.RefreshGantt();
    }
    
    private void UpdateLocalizedText()
    {
        // Update column headers
        ganttControl.InternalGrid.Columns[0].HeaderText = GetLocalizedString("TaskName");
        ganttControl.InternalGrid.Columns[1].HeaderText = GetLocalizedString("StartDate");
        ganttControl.InternalGrid.Columns[2].HeaderText = GetLocalizedString("EndDate");
        ganttControl.InternalGrid.Columns[3].HeaderText = GetLocalizedString("Duration");
        ganttControl.InternalGrid.Columns[4].HeaderText = GetLocalizedString("Progress");
        
        // Update schedule type format
        ganttControl.ScheduleType = ScheduleType.Day;
        ganttControl.ScheduleHeaderDateFormat = Thread.CurrentThread.CurrentCulture.DateTimeFormat.ShortDatePattern;
    }
    
    private string GetLocalizedString(string key)
    {
        // Access resource manager
        return Resources.ResourceManager.GetString(key, Thread.CurrentThread.CurrentUICulture);
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

## Week Start Customization

Configure week start day based on culture.

```csharp
private void SetWeekStartByCulture(CultureInfo culture)
{
    // Get culture's first day of week
    DayOfWeek firstDay = culture.DateTimeFormat.FirstDayOfWeek;
    
    ganttControl.StartWeekDay = firstDay;
    
    // Examples:
    // en-US: Sunday
    // en-GB: Monday
    // ar-SA: Saturday
}
```

## Fiscal Year Configuration

Support different fiscal year calendars.

```csharp
private void SetFiscalYear(int fiscalYearStartMonth)
{
    ganttControl.FiscalYearBegin = fiscalYearStartMonth;
    
    // Examples:
    // US Government: October (10)
    // UK: April (4)
    // Japan: April (4)
    // Australia: July (7)
}
```

## Time Zone Support

Handle tasks across different time zones.

```csharp
public class TaskDetails
{
    public DateTime StartDate { get; set; }
    public DateTime FinishDate { get; set; }
    public TimeZoneInfo TimeZone { get; set; } = TimeZoneInfo.Local;
    
    // Convert to specific time zone
    public DateTime GetStartDateInTimeZone(TimeZoneInfo targetTimeZone)
    {
        return TimeZoneInfo.ConvertTime(StartDate, TimeZone, targetTimeZone);
    }
}

// Display in user's local time zone
private void DisplayInLocalTimeZone()
{
    var userTimeZone = TimeZoneInfo.Local;
    
    foreach (var task in TaskCollection)
    {
        var localStart = task.GetStartDateInTimeZone(userTimeZone);
        var localFinish = task.GetFinishDateInTimeZone(userTimeZone);
        
        // Update display
        task.DisplayStartDate = localStart;
        task.DisplayFinishDate = localFinish;
    }
}
```

## Best Practices

1. **Test RTL Layout:** Thoroughly test RTL cultures to ensure proper layout.

2. **Use Resource Files:** Centralize all user-facing strings in resource files.

3. **Format by Culture:** Always use culture-specific formatting for dates, numbers, and currency.

4. **Default to Current Culture:** Use `CultureInfo.CurrentCulture` as the default.

5. **Week Start:** Respect culture's first day of week for calendar operations.

6. **Test Multiple Cultures:** Validate with at least 3 different language families (Latin, Arabic/Hebrew RTL, Asian CJK).

7. **Time Zones:** Consider time zone handling for global teams.

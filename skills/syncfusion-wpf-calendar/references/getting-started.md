# Getting Started with CalendarEdit

## Installation and Assembly Setup

The `CalendarEdit` control requires the **Syncfusion.Shared.WPF** assembly. Follow the steps below based on your installation method.

### Via NuGet Package Manager

1. Open the Package Manager Console in Visual Studio
2. Run the following command:
   ```
   Install-Package Syncfusion.Shared.WPF
   ```
3. The assembly reference will be added automatically

### Via Visual Studio References

1. Right-click on **References** in your WPF project
2. Click **Add Reference**
3. Select **Syncfusion.Shared.WPF**
4. Click **OK**

For more information, refer to the [Control Dependencies documentation](https://help.syncfusion.com/wpf/control-dependencies#calendaredit) and [NuGet packages guide](https://help.syncfusion.com/wpf/visual-studio-integration/nuget-packages).

---

## Adding CalendarEdit via Designer

The simplest method for XAML-based development:

1. Open your WPF window or user control in the designer
2. Locate the **Toolbox** panel
3. Find and expand the **Syncfusion Components** section
4. **Drag** `CalendarEdit` onto your design surface
5. The `Syncfusion.Shared.WPF` assembly is automatically added to your project
6. Use the **SmartTag** feature to configure properties in the designer

---

## Adding CalendarEdit via XAML

### Step 1: Create a New WPF Project

Create a new WPF Application in Visual Studio.

### Step 2: Add Assembly Reference

Add the following assembly to your project references:
- `Syncfusion.Shared.WPF`

### Step 3: Import Syncfusion Namespace

In your XAML file, add the Syncfusion namespace:

```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

### Step 4: Declare CalendarEdit

Add the `CalendarEdit` control to your XAML:

```xaml
<Window x:Class="CalendarEdit_sample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:CalendarEdit_sample"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">

    <Grid Name="grid">
        <!-- CalendarEdit control -->
        <syncfusion:CalendarEdit Name="calendarEdit"
                                 Height="200"
                                 Width="200"/>
    </Grid>
</Window>
```

---

## Adding CalendarEdit via C#

### Step 1: Create a New WPF Application

Create a new WPF Application in Visual Studio.

### Step 2: Add Assembly Reference

Add the following assembly to your project references:
- `Syncfusion.Shared.WPF`

### Step 3: Include Required Namespace

In your code-behind file, add the namespace:

```csharp
using Syncfusion.Windows.Shared;
```

### Step 4: Create CalendarEdit Instance

In your Window class (e.g., `MainWindow.xaml.cs`), add code to create and configure the calendar:

```csharp
using System.Windows;
using Syncfusion.Windows.Shared;

namespace CalendarEdit_sample
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            // Create an instance of CalendarEdit
            CalendarEdit calendarEdit = new CalendarEdit();
            
            // Set height and width
            calendarEdit.Height = 200;
            calendarEdit.Width = 200;
            
            // Add to the window's grid
            grid.Children.Add(calendarEdit);
        }
    }
}
```

---

## Basic Control Structure

The `CalendarEdit` control has the following key structure:

```
CalendarEdit
├── Header (Month/Year navigation)
├── Calendar Grid (Date cells)
├── Previous/Next Buttons
└── Days of Week Labels
```

### Understanding the Layout

- **Header:** Shows current month/year and allows navigation to year/decade modes
- **Calendar Grid:** Displays dates for the current month
- **Navigation Buttons:** Previous (<) and Next (>) for month navigation
- **Day Labels:** Shows abbreviated day names (Sun, Mon, etc.)

---

## Setting Initial Date

### In XAML

```xaml
<syncfusion:CalendarEdit Date="03/15/2024"
                         Name="calendarEdit" />
```

### In C#

```csharp
calendarEdit.Date = new DateTime(2024, 3, 15);
```

---

## Getting the Selected Date

### In C# Code-Behind

```csharp
// Get the selected date
DateTime selectedDate = calendarEdit.Date;

// Display the date
MessageBox.Show($"Selected Date: {selectedDate:yyyy-MM-dd}");
```

### With Event Binding (XAML)

```xaml
<syncfusion:CalendarEdit Name="calendarEdit" />
<TextBlock Text="{Binding ElementName=calendarEdit, Path=Date}" 
           Margin="10"/>
```

---

## Common Setup Patterns

### Pattern 1: Basic Calendar with Current Date

```xaml
<syncfusion:CalendarEdit Date="{Binding Source={x:Static sys:DateTime.Now}}"
                         Name="calendarEdit" />
```

### Pattern 2: Programmatic Setup

```csharp
public MainWindow()
{
    InitializeComponent();
    
    // Configure calendar after initialization
    calendarEdit.Date = DateTime.Now;
    calendarEdit.Height = 250;
    calendarEdit.Width = 300;
    
    // Optional: Subscribe to date selection changes
    calendarEdit.SelectedDateChanged += (s, e) => 
    {
        Debug.WriteLine($"Selected: {calendarEdit.Date}");
    };
}
```

### Pattern 3: Designer Configuration

When adding CalendarEdit via the designer:
1. Drag control to design surface
2. Right-click and select **Properties**
3. Set `Height` to 250, `Width` to 300
4. Set `Date` property to desired default date
5. Compile and run

---

## Troubleshooting

### Issue: CalendarEdit not appearing

**Solution:** Verify that the `Syncfusion.Shared.WPF` assembly is added to project references and the namespace is imported correctly.

### Issue: Date not displaying

**Solution:** Ensure the `Date` property is set to a valid `DateTime` value. Use `DateTime.Now` as default if needed.

### Issue: Assembly not found error

**Solution:** 
1. Check that the NuGet package is installed
2. Rebuild the project
3. Clear the NuGet cache if necessary

---

## Next Steps

- **Date Selection:** Read [date-selection.md](date-selection.md) to learn about single and multiple date selection
- **Navigation:** Read [navigation.md](navigation.md) to customize calendar navigation modes
- **Restrictions:** Read [restrict-dates.md](restrict-dates.md) to set date range constraints
- **Styling:** Read [appearance.md](appearance.md) to customize colors and appearance

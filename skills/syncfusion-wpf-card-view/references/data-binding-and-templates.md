# Data Binding & Templates — CardView (WPF)

## Table of Contents
- [Model and ViewModel Setup](#model-and-viewmodel-setup)
- [HeaderTemplate](#headertemplate)
- [ItemTemplate](#itemtemplate)
- [EditItemTemplate](#edititemtemplate)
- [ItemContainerStyle](#itemcontainerstyle)
- [ItemContainerStyleSelector](#itemcontainerstyleselector)
- [FlowDirection (RTL)](#flowdirection-rtl)
- [SfSkinManager Themes](#sfskinmanager-themes)

---

## Model and ViewModel Setup

CardView works best with an `ObservableCollection<T>` so the UI updates automatically when items are added or removed.

```csharp
// Model.cs
public class Employee
{
    public string FirstName { get; set; }
    public string LastName  { get; set; }
    public int    Age       { get; set; }
    public string Department { get; set; }
}

// ViewModel.cs
public class ViewModel : NotificationObject
{
    private ObservableCollection<Employee> _employees;
    public ObservableCollection<Employee> Employees
    {
        get => _employees;
        set { _employees = value; RaisePropertyChanged(nameof(Employees)); }
    }

    public ViewModel()
    {
        Employees = new ObservableCollection<Employee>
        {
            new Employee { FirstName="Alice", LastName="Smith",  Age=30, Department="Engineering" },
            new Employee { FirstName="Bob",   LastName="Jones",  Age=25, Department="Sales"       },
            new Employee { FirstName="Carol", LastName="Smith",  Age=28, Department="Engineering" },
            new Employee { FirstName="Dave",  LastName="Brown",  Age=35, Department="HR"          },
        };
    }
}
```

```xml
<!-- Bind ViewModel as DataContext -->
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:CardView ItemsSource="{Binding Employees}" Name="cardView"/>
```

---

## HeaderTemplate

Defines the content displayed in the **card header strip** (the colored bar at the top of each card). Also used as the label for fields in the grouping/sorting/filtering header.

```xml
<syncfusion:CardView ItemsSource="{Binding Employees}" Name="cardView">
    <syncfusion:CardView.HeaderTemplate>
        <DataTemplate>
            <!-- DataContext = one Employee item -->
            <TextBlock Text="{Binding FirstName}" FontWeight="Bold"/>
        </DataTemplate>
    </syncfusion:CardView.HeaderTemplate>
</syncfusion:CardView>
```

**Multi-field header:**
```xml
<syncfusion:CardView.HeaderTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="{Binding FirstName}"/>
            <TextBlock Text=" "/>
            <TextBlock Text="{Binding LastName}"/>
        </StackPanel>
    </DataTemplate>
</syncfusion:CardView.HeaderTemplate>
```

---

## ItemTemplate

Defines the **card body in view mode** — shown when the card is not being edited.

```xml
<syncfusion:CardView.ItemTemplate>
    <DataTemplate>
        <ListBox ScrollViewer.HorizontalScrollBarVisibility="Disabled">
            <ListBoxItem Padding="1">
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="90"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Text="First Name:" FontWeight="SemiBold"/>
                    <TextBlock Grid.Column="1"
                               Text="{Binding FirstName, UpdateSourceTrigger=PropertyChanged}"/>
                </Grid>
            </ListBoxItem>
            <ListBoxItem Padding="1">
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="90"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Text="Department:" FontWeight="SemiBold"/>
                    <TextBlock Grid.Column="1"
                               Text="{Binding Department, UpdateSourceTrigger=PropertyChanged}"/>
                </Grid>
            </ListBoxItem>
            <ListBoxItem Padding="1">
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="90"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Text="Age:" FontWeight="SemiBold"/>
                    <TextBlock Grid.Column="1"
                               Text="{Binding Age, UpdateSourceTrigger=PropertyChanged}"/>
                </Grid>
            </ListBoxItem>
        </ListBox>
    </DataTemplate>
</syncfusion:CardView.ItemTemplate>
```

> **Why ListBox?** It provides consistent vertical stacking of field rows within the card without requiring custom panel layout.

---

## EditItemTemplate

Defines the **card body in edit mode** — shown when the user presses F2 or double-clicks a card (requires `CanEdit="True"`). Replace `TextBlock` with `TextBox` to allow edits.

```xml
<syncfusion:CardView CanEdit="True" ItemsSource="{Binding Employees}" Name="cardView">
    <syncfusion:CardView.EditItemTemplate>
        <DataTemplate>
            <ListBox ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="90"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="First Name:"/>
                        <TextBox Grid.Column="1"
                                 Text="{Binding FirstName, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="90"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="Department:"/>
                        <TextBox Grid.Column="1"
                                 Text="{Binding Department, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </DataTemplate>
    </syncfusion:CardView.EditItemTemplate>
    <!-- Also include ItemTemplate and HeaderTemplate -->
</syncfusion:CardView>
```

> **Pattern:** `ItemTemplate` = read-only `TextBlock`s; `EditItemTemplate` = editable `TextBox`es with the same bindings.

---

## ItemContainerStyle

Apply a uniform `Style` to every card container (`CardViewItem`):

```xml
<syncfusion:CardView ItemsSource="{Binding Employees}" Name="cardView">
    <syncfusion:CardView.ItemContainerStyle>
        <Style TargetType="{x:Type syncfusion:CardViewItem}">
            <Setter Property="Width"  Value="200"/>
            <Setter Property="Height" Value="150"/>
            <Setter Property="Margin" Value="5"/>
        </Style>
    </syncfusion:CardView.ItemContainerStyle>
</syncfusion:CardView>
```

---

## ItemContainerStyleSelector

Apply **different styles** to cards based on data values:

```csharp
// StyleSelector.cs
public class DepartmentStyleSelector : StyleSelector
{
    public Style EngineeringStyle { get; set; }
    public Style DefaultStyle     { get; set; }

    public override Style SelectStyle(object item, DependencyObject container)
    {
        if (item is Employee emp && emp.Department == "Engineering")
            return EngineeringStyle;
        return DefaultStyle;
    }
}
```

```xml
<Window.Resources>
    <local:DepartmentStyleSelector x:Key="deptSelector">
        <local:DepartmentStyleSelector.EngineeringStyle>
            <Style TargetType="{x:Type syncfusion:CardViewItem}">
                <Setter Property="Background" Value="#E8F5E9"/>
            </Style>
        </local:DepartmentStyleSelector.EngineeringStyle>
        <local:DepartmentStyleSelector.DefaultStyle>
            <Style TargetType="{x:Type syncfusion:CardViewItem}">
                <Setter Property="Background" Value="White"/>
            </Style>
        </local:DepartmentStyleSelector.DefaultStyle>
    </local:DepartmentStyleSelector>
</Window.Resources>

<syncfusion:CardView ItemContainerStyleSelector="{StaticResource deptSelector}"
                     ItemsSource="{Binding Employees}"
                     Name="cardView"/>
```

---

## FlowDirection (RTL)

For right-to-left languages (Arabic, Hebrew, etc.):

```xml
<syncfusion:CardView FlowDirection="RightToLeft"
                     ItemsSource="{Binding Employees}"
                     Name="cardView"/>
```

---

## SfSkinManager Themes

Apply a visual theme in code-behind:

```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetVisualStyle(cardView, VisualStyles.FluentLight);
```

**Available Visual Styles:**

| Style Name | Appearance |
|---|---|
| `FluentLight` | Microsoft Fluent light |
| `FluentDark` | Microsoft Fluent dark |
| `MaterialLight` | Google Material light |
| `MaterialDark` | Google Material dark |
| `MaterialLightBlue` | Material with blue accent |
| `Windows11Light` | Windows 11 light |
| `Windows11Dark` | Windows 11 dark |
| `Office2019Colorful` | Office 2019 colorful |
| `Office2019Black` | Office 2019 black |
| `Office2019White` | Office 2019 white |
| `SystemTheme` | Follows OS theme |

> Apply themes **after** `InitializeComponent()`. Themes affect the entire control including the grouping/sorting header.

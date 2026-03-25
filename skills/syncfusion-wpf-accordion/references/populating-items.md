# Populating Items

## Table of Contents
- [Declarative Items](#declarative-items)
- [Data Binding via ItemsSource](#data-binding-via-itemssource)
- [DisplayMemberPath](#displaymemberpath)
- [HeaderTemplate and ContentTemplate](#headertemplate-and-contenttemplate)
- [Template Selectors](#template-selectors)
- [Gotchas](#gotchas)

---

## Declarative Items

Add `SfAccordionItem` children directly in XAML or C#.

### XAML

```xaml
<syncfusion:SfAccordion Width="300" Height="250">
    <syncfusion:SfAccordionItem Header="WPF"
                                Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="Silverlight"
                                Content="Essential Studio for Silverlight"/>
    <syncfusion:SfAccordionItem Header="WinRT"
                                Content="Essential Studio for WinRT"/>
    <syncfusion:SfAccordionItem Header="Windows Phone"
                                Content="Essential Studio for Windows Phone"/>
    <syncfusion:SfAccordionItem Header="Universal"
                                Content="Essential Studio for Universal"/>
</syncfusion:SfAccordion>
```

### C\#

```csharp
SfAccordion accordion = new SfAccordion();
accordion.Items.Add(new SfAccordionItem { Header = "WPF",          Content = "Essential Studio for WPF" });
accordion.Items.Add(new SfAccordionItem { Header = "Silverlight",   Content = "Essential Studio for Silverlight" });
accordion.Items.Add(new SfAccordionItem { Header = "WinRT",         Content = "Essential Studio for WinRT" });
accordion.Items.Add(new SfAccordionItem { Header = "Windows Phone", Content = "Essential Studio for Windows Phone" });
accordion.Items.Add(new SfAccordionItem { Header = "Universal",     Content = "Essential Studio for Universal" });
this.Content = accordion;
```

> **Note:** `Header` is always visible. `Content` is visible only when the item is expanded (`IsSelected=true`).

---

## Data Binding via ItemsSource

Bind any business object collection to `ItemsSource`.

### Model

```csharp
public class Employee
{
    public string Name        { get; set; }
    public string Description { get; set; }
}
```

### ViewModel

```csharp
using System.Collections.ObjectModel;

public class EmployeeViewModel
{
    public ObservableCollection<Employee> Employees { get; set; }

    public EmployeeViewModel()
    {
        Employees = new ObservableCollection<Employee>
        {
            new Employee { Name = "James", Description = "Description about James" },
            new Employee { Name = "Linda", Description = "Description about Linda" },
            new Employee { Name = "Carl",  Description = "Description about Carl"  },
            new Employee { Name = "Niko",  Description = "Description about Niko"  },
        };
    }
}
```

### XAML Binding

```xaml
<Window.DataContext>
    <local:EmployeeViewModel/>
</Window.DataContext>

<syncfusion:SfAccordion ItemsSource="{Binding Employees}"
                        DisplayMemberPath="Name"
                        Width="300" Height="250">
    <syncfusion:SfAccordion.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Description}" TextWrapping="Wrap" Margin="8"/>
        </DataTemplate>
    </syncfusion:SfAccordion.ContentTemplate>
</syncfusion:SfAccordion>
```

---

## DisplayMemberPath

Use `DisplayMemberPath` as the quickest way to show a property as the item header (no `DataTemplate` needed):

```xaml
<syncfusion:SfAccordion ItemsSource="{Binding Employees}"
                        DisplayMemberPath="Name"/>
```

> When `HeaderTemplate` is also set, `HeaderTemplate` takes precedence over `DisplayMemberPath`.

---

## HeaderTemplate and ContentTemplate

Use `HeaderTemplate` and `ContentTemplate` for rich layouts:

```xaml
<syncfusion:SfAccordion ItemsSource="{Binding Employees}" Width="400" Height="300">
    <syncfusion:SfAccordion.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}"
                       FontSize="16"
                       FontWeight="SemiBold"
                       Foreground="DarkSlateBlue"
                       VerticalAlignment="Center"
                       Margin="8,0"/>
        </DataTemplate>
    </syncfusion:SfAccordion.HeaderTemplate>
    <syncfusion:SfAccordion.ContentTemplate>
        <DataTemplate>
            <StackPanel Margin="12,8">
                <TextBlock Text="{Binding Name}"       FontWeight="Bold"/>
                <TextBlock Text="{Binding Description}" TextWrapping="Wrap" Opacity="0.8"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfAccordion.ContentTemplate>
</syncfusion:SfAccordion>
```

Applied to a non-data-bound accordion (items added directly):

```xaml
<syncfusion:SfAccordion Width="300" Height="200">
    <syncfusion:SfAccordion.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}"
                       Foreground="Red"
                       FontFamily="Calibri"
                       FontWeight="Bold"
                       FontSize="14"/>
        </DataTemplate>
    </syncfusion:SfAccordion.HeaderTemplate>
    <syncfusion:SfAccordionItem Header="WindowsForms"/>
    <syncfusion:SfAccordionItem Header="WPF"/>
    <syncfusion:SfAccordionItem Header="UWP"/>
</syncfusion:SfAccordion>
```

> When items are added directly (not via `ItemsSource`), `{Binding}` in `HeaderTemplate` binds to the `Header` value itself.

---

## Template Selectors

Apply different header or content templates based on data:

### HeaderTemplateSelector

```csharp
public class EmployeeHeaderSelector : DataTemplateSelector
{
    public DataTemplate ManagerTemplate  { get; set; }
    public DataTemplate EmployeeTemplate { get; set; }

    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        var emp = item as Employee;
        return emp?.IsManager == true ? ManagerTemplate : EmployeeTemplate;
    }
}
```

```xaml
<local:EmployeeHeaderSelector x:Key="headerSelector"
    ManagerTemplate="{StaticResource ManagerHeaderTemplate}"
    EmployeeTemplate="{StaticResource EmployeeHeaderTemplate}"/>

<syncfusion:SfAccordion ItemsSource="{Binding Employees}"
                        HeaderTemplateSelector="{StaticResource headerSelector}"/>
```

### ContentTemplateSelector

Same pattern — set `ContentTemplateSelector` instead:

```xaml
<syncfusion:SfAccordion ItemsSource="{Binding Employees}"
                        ContentTemplateSelector="{StaticResource contentSelector}"/>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Headers blank when using `ItemsSource` | No `DisplayMemberPath` or `HeaderTemplate` | Add `DisplayMemberPath="PropertyName"` or a `HeaderTemplate` |
| Content not showing | `Content` is only visible when item is expanded | Click the item header to expand, or set `IsSelected=true` |
| `HeaderTemplate` binding is `null` | Bound to wrong property name | Use `{Binding}` (no path) when item is added directly; use `{Binding PropertyName}` with `ItemsSource` |
| `ContentTemplate` ignored | `ContentTemplateSelector` is also set | Selector takes priority — remove one or the other |

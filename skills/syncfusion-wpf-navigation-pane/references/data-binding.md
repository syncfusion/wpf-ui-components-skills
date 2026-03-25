# Data Binding for GroupBar

## Table of Contents
- [ItemsSource with ItemContainerStyle](#itemssource-with-itemcontainerstyle)
- [Model and ViewModel](#model-and-viewmodel)
- [Full MVVM Example](#full-mvvm-example)
- [DataContext Binding](#datacontext-binding)
- [Data Binding with XML](#data-binding-with-xml)
- [HeaderTemplate and ContentTemplate](#headertemplate-and-contenttemplate)
- [ItemContainerStyleSelector](#itemcontainerstyle-selector)
- [Gotchas](#gotchas)

---

## ItemsSource with ItemContainerStyle

Bind a collection to `GroupBar.ItemsSource` and map model properties to `GroupBarItem` properties using `ItemContainerStyle`:

```xaml
<syncfusion:GroupBar Name="groupBar" ItemsSource="{Binding GroupData}" VisualMode="StackMode">
    <syncfusion:GroupBar.ItemContainerStyle>
        <Style TargetType="{x:Type syncfusion:GroupBarItem}">
            <Setter Property="HeaderText" Value="{Binding Header}"/>
            <Setter Property="Content"    Value="{Binding Content}"/>
            <Setter Property="IsSelected" Value="{Binding IsSelected}"/>
        </Style>
    </syncfusion:GroupBar.ItemContainerStyle>
</syncfusion:GroupBar>
```

---

## Model and ViewModel

**Model:**
```csharp
public class GroupBarModel
{
    public string Header     { get; set; }
    public string Content    { get; set; }
    public bool   IsSelected { get; set; }
}
```

**ViewModel:**
```csharp
using System.Collections.ObjectModel;

public class GroupBarViewModel
{
    public ObservableCollection<GroupBarModel> GroupData { get; set; }

    public GroupBarViewModel()
    {
        GroupData = new ObservableCollection<GroupBarModel>
        {
            new GroupBarModel { Header = "Mailbox",          Content = "Mailbox content",  IsSelected = true  },
            new GroupBarModel { Header = "Favorite Folders", Content = "Folders content",  IsSelected = false },
            new GroupBarModel { Header = "Contacts",         Content = "Contacts content", IsSelected = false },
            new GroupBarModel { Header = "Tasks",            Content = "Tasks content",    IsSelected = false }
        };
    }
}
```

---

## Full MVVM Example

**XAML:**
```xaml
<Window.DataContext>
    <local:GroupBarViewModel/>
</Window.DataContext>

<syncfusion:GroupBar Name="groupBar"
                     ItemsSource="{Binding GroupData}"
                     VisualMode="StackMode"
                     Height="300" Width="230">
    <syncfusion:GroupBar.ItemContainerStyle>
        <Style TargetType="{x:Type syncfusion:GroupBarItem}">
            <Setter Property="HeaderText" Value="{Binding Header}"/>
            <Setter Property="Content"    Value="{Binding Content}"/>
            <Setter Property="IsSelected" Value="{Binding IsSelected}"/>
        </Style>
    </syncfusion:GroupBar.ItemContainerStyle>
</syncfusion:GroupBar>
```

**Code-behind (alternative):**
```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new GroupBarViewModel();
}
```

---

## DataContext Binding

Bind individual `GroupBarItem` properties to a custom object via `DataContext`:

```xaml
<Window.Resources>
    <local:GroupData x:Key="groupData"/>
</Window.Resources>

<syncfusion:GroupBar DataContext="{StaticResource groupData}"
                     VisualMode="Default"
                     AllowCollapse="True"
                     Height="200" Width="230">
    <syncfusion:GroupBarItem Header="{Binding Header}">
        <syncfusion:GroupView IsListViewMode="True">
            <syncfusion:GroupViewItem Text="{Binding ListView}"/>
            <syncfusion:GroupViewItem Text="Show ContextMenu"/>
        </syncfusion:GroupView>
    </syncfusion:GroupBarItem>
</syncfusion:GroupBar>
```

```csharp
public class GroupData
{
    public string Header   { get; set; } = "GroupBarItem1";
    public string ListView { get; set; } = "ListViewItem";
}
```

---

## Data Binding with XML

Bind to an XML file using `XmlDataProvider`:

**Data.xml:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<Books>
    <Book Name="Programming C#"  Description="Learn C# fundamentals"  ImagePath="cs.png"/>
    <Book Name="Programming WPF" Description="XAML and WPF tutorial"  ImagePath="wpf.png"/>
    <Book Name="Essential WPF"   Description="Visuals and media"      ImagePath="ewpf.png"/>
</Books>
```

**XAML:**
```xaml
<Window.Resources>
    <XmlDataProvider Source="Data.xml" x:Key="xmlSource" XPath="Books"/>
</Window.Resources>

<syncfusion:GroupBar Name="groupBar"
                     ItemsSource="{Binding Source={StaticResource xmlSource}, XPath=Book}"
                     VisualMode="MultipleExpansion">
    <syncfusion:GroupBar.ItemContainerStyle>
        <Style TargetType="{x:Type syncfusion:GroupBarItem}">
            <Setter Property="HeaderTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <TextBlock Text="{Binding XPath=@Name}"/>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
            <Setter Property="ContentTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="4*"/>
                                <ColumnDefinition Width="6*"/>
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding XPath=@ImagePath}"/>
                            <TextBlock Text="{Binding XPath=@Description}"
                                       TextWrapping="Wrap"
                                       Grid.Column="1"/>
                        </Grid>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:GroupBar.ItemContainerStyle>
</syncfusion:GroupBar>
```

---

## HeaderTemplate and ContentTemplate

Use `HeaderTemplate` and `ContentTemplate` in `ItemContainerStyle` for richer visuals:

```xaml
<Style TargetType="{x:Type syncfusion:GroupBarItem}" x:Key="RichItemStyle">
    <Setter Property="HeaderTemplate">
        <Setter.Value>
            <DataTemplate>
                <StackPanel Orientation="Horizontal">
                    <Image Source="{Binding XPath=@ImagePath}" Width="20" Height="20" Margin="3"/>
                    <TextBlock Text="{Binding XPath=@Name}" FontWeight="Bold" Foreground="DarkBlue"/>
                </StackPanel>
            </DataTemplate>
        </Setter.Value>
    </Setter>
    <Setter Property="ContentTemplate">
        <Setter.Value>
            <DataTemplate>
                <TextBlock Text="{Binding XPath=@Description}" TextWrapping="Wrap"/>
            </DataTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

---

## ItemContainerStyleSelector

Apply different styles to different items based on data:

```csharp
public class GroupBarStyleSelector : StyleSelector
{
    public Style StyleA { get; set; }
    public Style StyleB { get; set; }

    public override Style SelectStyle(object item, DependencyObject container)
    {
        var element = item as System.Xml.XmlElement;
        string name = element?.GetAttribute("Name").ToLower() ?? "";
        return name.Contains("wpf") ? StyleA : StyleB;
    }
}
```

```xaml
<local:GroupBarStyleSelector x:Key="styleSelector"
    StyleA="{StaticResource WpfItemStyle}"
    StyleB="{StaticResource DefaultItemStyle}"/>

<syncfusion:GroupBar ItemsSource="{Binding Source={StaticResource xmlSource}, XPath=Book}"
                     ItemContainerStyleSelector="{StaticResource styleSelector}"
                     VisualMode="StackMode"/>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `HeaderText` binding not working | Using `Header` property in setter instead of `HeaderText` | Use `Property="HeaderText"` in `ItemContainerStyle` setters |
| Items not updating at runtime | Using `List<T>` in ViewModel | Use `ObservableCollection<T>` for live update support |
| `DataContext` inherited unexpectedly | Parent window DataContext propagates | Explicitly set `DataContext` on `GroupBar` if needed |
| XML binding shows no items | Wrong `XPath` on `XmlDataProvider` | Ensure `XPath="Books"` on provider, `XPath=Book` on `ItemsSource` |
| `ContentTemplate` ignored | `Content` setter also defined | `ContentTemplate` requires `Content` to be bound to the data item directly |

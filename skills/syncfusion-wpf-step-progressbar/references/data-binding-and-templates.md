# Data Binding & Templates

## Table of Contents
- [Data Binding to Objects](#data-binding-to-objects)
- [Data Binding with XML](#data-binding-with-xml)
- [ItemTemplate](#itemtemplate)
- [ItemTemplateSelector](#itemtemplateselector)
- [MarkerTemplateSelector](#markertemplateselector)
- [SecondaryContentTemplate](#secondarycontenttemplate)
- [SecondaryContentTemplateSelector](#secondarycontenttemplateselector)

---

## Data Binding to Objects

Bind `ItemsSource` to an `ObservableCollection<T>` for MVVM-driven step bars.

**Model and ViewModel:**

```csharp
// Model.cs
public class StepItem
{
    public string ModelText { get; set; }
    public double TitleSpace { get; set; }
}

// ViewModel.cs
public class ViewModel : INotifyPropertyChanged
{
    public ObservableCollection<StepItem> StepViewItems { get; set; }

    public ViewModel()
    {
        StepViewItems = new ObservableCollection<StepItem>
        {
            new StepItem { ModelText = "Ordered",   TitleSpace = 8 },
            new StepItem { ModelText = "Shipped",   TitleSpace = 8 },
            new StepItem { ModelText = "Packed",    TitleSpace = 8 },
            new StepItem { ModelText = "Delivered", TitleSpace = 8 }
        };
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

**XAML — ItemsSource + ItemContainerStyle:**

```xaml
<Syncfusion:SfStepProgressBar
    x:Name="stepperControlName"
    Margin="40"
    ItemsSource="{Binding StepViewItems}"
    Orientation="Horizontal"
    SelectedIndex="2">
    <Syncfusion:SfStepProgressBar.ItemContainerStyle>
        <Style TargetType="Syncfusion:StepViewItem">
            <Setter Property="Content" Value="{Binding ModelText}" />
            <Setter Property="TextSpacing" Value="{Binding TitleSpace}" />
        </Style>
    </Syncfusion:SfStepProgressBar.ItemContainerStyle>
    <Syncfusion:SfStepProgressBar.DataContext>
        <local:ViewModel />
    </Syncfusion:SfStepProgressBar.DataContext>
</Syncfusion:SfStepProgressBar>
```

---

## Data Binding with XML

Use an XML file as the `ItemsSource`:

**Data.xml:**
```xml
<?xml version="1.0" encoding="utf-8" ?>
<StepItems SelectedIndex="3">
    <Step Name="Ordered" />
    <Step Name="Shipped" />
    <Step Name="Packed" />
    <Step Name="Delivered" />
</StepItems>
```

**XAML:**
```xaml
<Window.Resources>
    <XmlDataProvider x:Key="xmlSource" Source="Data.xml" XPath="StepItems" />
</Window.Resources>

<Syncfusion:SfStepProgressBar
    x:Name="stepperControlName"
    ItemsSource="{Binding Source={StaticResource xmlSource}, XPath=Step}"
    SelectedIndex="{Binding Source={StaticResource xmlSource}, XPath=@SelectedIndex}">
    <Syncfusion:SfStepProgressBar.ItemContainerStyle>
        <Style TargetType="Syncfusion:StepViewItem">
            <Setter Property="Content" Value="{Binding XPath=@Name}" />
        </Style>
    </Syncfusion:SfStepProgressBar.ItemContainerStyle>
</Syncfusion:SfStepProgressBar>
```

---

## ItemTemplate

Customize the content displayed inside the step label area with a `DataTemplate`:

```xaml
<Syncfusion:SfStepProgressBar ItemsSource="{Binding StepViewItems}" SelectedIndex="2">
    <Syncfusion:SfStepProgressBar.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Path
                    Width="25" Height="25"
                    HorizontalAlignment="Center" VerticalAlignment="Center"
                    Data="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z"
                    Fill="#2B579A" Stroke="#2B579A" />
            </Grid>
        </DataTemplate>
    </Syncfusion:SfStepProgressBar.ItemTemplate>
</Syncfusion:SfStepProgressBar>
```

- `ItemTemplate` replaces the default text label for every step
- All steps share the same template; use `ItemTemplateSelector` for per-status templates

---

## ItemTemplateSelector

Use different content templates depending on each step's `StepStatus`:

**Template Selector class:**
```csharp
public class StepViewItemTemplateSelector : DataTemplateSelector
{
    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        StepViewItem stepViewItem = item as StepViewItem;
        if (stepViewItem == null) return null;

        if (stepViewItem.Status == StepStatus.Indeterminate)
            return stepViewItem.FindResource("IndeterminateContentTemplate") as DataTemplate;
        else if (stepViewItem.Status == StepStatus.Active)
            return stepViewItem.FindResource("ActiveContentTemplate") as DataTemplate;
        else
            return stepViewItem.FindResource("InactiveContentTemplate") as DataTemplate;
    }
}
```

**XAML:**
```xaml
<Window.Resources>
    <DataTemplate x:Key="ActiveContentTemplate">
        <Path Width="25" Height="25" HorizontalAlignment="Center" VerticalAlignment="Center"
              Data="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z"
              Fill="#2B579A" Stroke="#2B579A" />
    </DataTemplate>
    <DataTemplate x:Key="IndeterminateContentTemplate">
        <Path Width="25" Height="25" HorizontalAlignment="Center" VerticalAlignment="Center"
              Data="M12 4V1L8 5l4 4V6c3.31 0 6 2.69 6 6 0 1.01-.25 1.97-.7 2.8l1.46 1.46C19.54 15.03 20 13.57 20 12c0-4.42-3.58-8-8-8zm0 14c-3.31 0-6-2.69-6-6 0-1.01.25-1.97.7-2.8L5.24 7.74C4.46 8.97 4 10.43 4 12c0 4.42 3.58 8 8 8v3l4-4-4-4v3z"
              Fill="#2B579A" Stroke="#2B579A" />
    </DataTemplate>
    <DataTemplate x:Key="InactiveContentTemplate">
        <Path Width="25" Height="25" HorizontalAlignment="Center" VerticalAlignment="Center"
              Data="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"
              Fill="#D2D2D2" Stroke="#D2D2D2" />
    </DataTemplate>
    <local:StepViewItemTemplateSelector x:Key="stepViewItemTemplateSelector" />
</Window.Resources>

<Syncfusion:SfStepProgressBar
    x:Name="stepperControlName"
    ItemsSource="{Binding StepViewItems}"
    SelectedIndex="2"
    ItemTemplateSelector="{StaticResource stepViewItemTemplateSelector}">
</Syncfusion:SfStepProgressBar>
```

---

## MarkerTemplateSelector

Replace the entire step marker (the circle/square node) with a fully custom shape per status:

**Template Selector class:**
```csharp
public class StepViewItemMarkerTemplateSelector : DataTemplateSelector
{
    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        StepViewItem stepViewItem = item as StepViewItem;
        if (stepViewItem == null) return null;

        if (stepViewItem.Status == StepStatus.Indeterminate)
            return stepViewItem.FindResource("IndeterminateTemplate") as DataTemplate;
        else if (stepViewItem.Status == StepStatus.Active)
            return stepViewItem.FindResource("ActiveTemplate") as DataTemplate;
        else
            return stepViewItem.FindResource("InactiveTemplate") as DataTemplate;
    }
}
```

**XAML:**
```xaml
<Window.Resources>
    <DataTemplate x:Key="IndeterminateTemplate">
        <Grid>
            <Ellipse Width="35" Height="35" Fill="#00ccb1" Stroke="#00ccb1" />
            <Path Width="12" Height="12" HorizontalAlignment="Center" VerticalAlignment="Center"
                  Data="M8,0 C12.411011,0 16,3.5890198 16,8 16,12.411011 12.411011,16 8,16 3.5889893,16 0,12.411011 0,8 0,3.5890198 3.5889893,0 8,0 z"
                  Fill="White" Stretch="Fill" />
        </Grid>
    </DataTemplate>
    <DataTemplate x:Key="ActiveTemplate">
        <Grid>
            <Ellipse Width="35" Height="35" Fill="#00ccb1" Stroke="#00ccb1" />
            <Path Width="16" Height="11" HorizontalAlignment="Center" VerticalAlignment="Center"
                  Data="M15.288992,0 L15.997,0.70702839 5.7260096,11.000999 0,5.8859658 0.66601563,5.1399964 5.6870084,9.6239898 z"
                  Fill="White" Stretch="Fill" Stroke="White" />
        </Grid>
    </DataTemplate>
    <DataTemplate x:Key="InactiveTemplate">
        <Grid>
            <Ellipse Width="35" Height="35" Fill="#FFFFFF" Stroke="#D2D2D2" />
        </Grid>
    </DataTemplate>
    <local:StepViewItemMarkerTemplateSelector x:Key="stepViewItemMarkerTemplateSelector" />
</Window.Resources>

<Syncfusion:SfStepProgressBar
    x:Name="stepperControlName"
    Margin="40"
    ItemsSource="{Binding StepViewItems}"
    MarkerTemplateSelector="{StaticResource stepViewItemMarkerTemplateSelector}"
    SelectedItemStatus="Indeterminate"
    ActiveConnectorColor="#00ccb1"
    SelectedIndex="2">
    <Syncfusion:SfStepProgressBar.ItemContainerStyle>
        <Style TargetType="Syncfusion:StepViewItem">
            <Setter Property="MarkerWidth" Value="35" />
            <Setter Property="MarkerHeight" Value="35" />
        </Style>
    </Syncfusion:SfStepProgressBar.ItemContainerStyle>
</Syncfusion:SfStepProgressBar>
```

> **Tip:** When using `MarkerTemplateSelector`, set `MarkerWidth` and `MarkerHeight` in `ItemContainerStyle` to match your custom marker size.

---

## SecondaryContentTemplate

Display additional content (dates, descriptions, step numbers) above/beside each step marker using a shared template:

```xaml
<Syncfusion:SfStepProgressBar
    ItemsSource="{Binding StepViewItems}"
    Orientation="Horizontal"
    SelectedIndex="2">
    <Syncfusion:SfStepProgressBar.ItemContainerStyle>
        <Style TargetType="Syncfusion:StepViewItem">
            <Setter Property="Content" Value="{Binding Text}" />
        </Style>
    </Syncfusion:SfStepProgressBar.ItemContainerStyle>
    <Syncfusion:SfStepProgressBar.SecondaryContentTemplate>
        <DataTemplate>
            <TextBlock
                HorizontalAlignment="Center" VerticalAlignment="Center"
                Text="{Binding SecondaryText}"
                TextWrapping="Wrap" />
        </DataTemplate>
    </Syncfusion:SfStepProgressBar.SecondaryContentTemplate>
    <Syncfusion:SfStepProgressBar.DataContext>
        <local:ViewModel />
    </Syncfusion:SfStepProgressBar.DataContext>
</Syncfusion:SfStepProgressBar>
```

For per-item secondary content, use `SecondaryContentTemplate` directly on each `StepViewItem`:

```xaml
<Syncfusion:SfStepProgressBar SelectedIndex="2" SelectedItemStatus="Indeterminate">
    <Syncfusion:StepViewItem Content="Ordered">
        <Syncfusion:StepViewItem.SecondaryContentTemplate>
            <DataTemplate>
                <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="Step 1" />
            </DataTemplate>
        </Syncfusion:StepViewItem.SecondaryContentTemplate>
    </Syncfusion:StepViewItem>
    <Syncfusion:StepViewItem Content="Packed">
        <Syncfusion:StepViewItem.SecondaryContentTemplate>
            <DataTemplate>
                <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="Step 2" />
            </DataTemplate>
        </Syncfusion:StepViewItem.SecondaryContentTemplate>
    </Syncfusion:StepViewItem>
</Syncfusion:SfStepProgressBar>
```

---

## SecondaryContentTemplateSelector

Use different secondary content templates per step status:

```csharp
public class StepViewItemTemplateSelector : DataTemplateSelector
{
    public override DataTemplate SelectTemplate(object item, DependencyObject container)
    {
        StepViewItem stepViewItem = item as StepViewItem;
        if (stepViewItem == null) return null;

        if (stepViewItem.Status == StepStatus.Indeterminate)
            return stepViewItem.FindResource("IndeterminateContentTemplate") as DataTemplate;
        else
            return stepViewItem.FindResource("ActiveContentTemplate") as DataTemplate;
    }
}
```

```xaml
<Syncfusion:SfStepProgressBar
    ItemsSource="{Binding Source={StaticResource xmlSource}, XPath=Step}"
    SecondaryContentTemplateSelector="{StaticResource stepViewItemTemplateSelector}"
    SelectedIndex="{Binding Source={StaticResource xmlSource}, XPath=@SelectedIndex}"
    SelectedItemStatus="Indeterminate">
    <Syncfusion:SfStepProgressBar.ItemContainerStyle>
        <Style TargetType="Syncfusion:StepViewItem">
            <Setter Property="Content" Value="{Binding XPath=@Name}" />
        </Style>
    </Syncfusion:SfStepProgressBar.ItemContainerStyle>
</Syncfusion:SfStepProgressBar>
```

---

## Gotchas

- **Template selector uses `StepStatus` enum** — always cast `item` to `StepViewItem` and check `.Status` property; do not cast to the data model
- **`FindResource` in selector** — call `(item as StepViewItem).FindResource(key)` not `Application.Current.FindResource(key)` to resolve templates in the correct resource scope
- **`ItemTemplate` vs `MarkerTemplateSelector`** — `ItemTemplate` customizes the *label content* below/beside the marker; `MarkerTemplateSelector` replaces the *marker node itself*
- **`SecondaryContentTemplate` on the container vs item** — `SfStepProgressBar.SecondaryContentTemplate` is a shared template for all items; `StepViewItem.SecondaryContentTemplate` overrides per-item
- **Binding in data-bound mode** — when using `ItemsSource`, always set `Content` and other properties via `ItemContainerStyle` setters, not by adding child `StepViewItem` elements directly

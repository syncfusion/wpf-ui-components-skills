# Getting Started — CardView (WPF)

Setting up the Syncfusion WPF CardView control from scratch.

---

## Required Assemblies

Add these references to your WPF project:

| Assembly | Purpose |
|---|---|
| `Syncfusion.Shared.WPF` | Core Syncfusion WPF infrastructure |
| `Syncfusion.Tools.WPF` | CardView control implementation |

Install via NuGet:
```
Install-Package Syncfusion.Tools.WPF
```

---

## XAML Namespace

```xml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

---

## Add CardView via Designer

1. Open **Toolbox** → find `CardView` under Syncfusion Tools
2. Drag onto the design surface
3. Namespace is added to the window automatically

---

## Add CardView via XAML

```xml
<Window x:Class="MyApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:CardView Name="cardView" Width="400" Height="300"/>
    </Grid>
</Window>
```

---

## Add CardView via C#

```csharp
using Syncfusion.Windows.Tools.Controls;

CardView cardView = new CardView();
cardView.Width = 400;
cardView.Height = 300;
rootGrid.Children.Add(cardView);
```

---

## Populate with Declarative CardViewItems

Use `CardViewItem` directly in XAML for static/simple content. 
> **Limitation:** Grouping, sorting, filtering, and editing require `ItemsSource` — they do **not** work with declarative items.

```xml
<syncfusion:CardView Name="cardView">
    <syncfusion:CardViewItem Header="Card 1">
        <TextBlock Text="Content for Card 1" Margin="5"/>
    </syncfusion:CardViewItem>
    <syncfusion:CardViewItem Header="Card 2">
        <TextBlock Text="Content for Card 2" Margin="5"/>
    </syncfusion:CardViewItem>
    <syncfusion:CardViewItem Header="Card 3">
        <TextBlock Text="Content for Card 3" Margin="5"/>
    </syncfusion:CardViewItem>
</syncfusion:CardView>
```

---

## Populate via ItemsSource (Recommended for MVVM)

Use `ItemsSource` with `HeaderTemplate` + `ItemTemplate` to bind data. This approach also enables grouping, sorting, filtering, and editing.

```csharp
// Model.cs
public class Person
{
    public string FirstName { get; set; }
    public string LastName  { get; set; }
    public int    Age       { get; set; }
}

// ViewModel.cs
public class ViewModel : NotificationObject
{
    private ObservableCollection<Person> _items;
    public ObservableCollection<Person> CardViewItems
    {
        get => _items;
        set { _items = value; RaisePropertyChanged(nameof(CardViewItems)); }
    }
    public ViewModel()
    {
        CardViewItems = new ObservableCollection<Person>
        {
            new Person { FirstName = "Alice", LastName = "Smith", Age = 30 },
            new Person { FirstName = "Bob",   LastName = "Jones", Age = 25 },
            new Person { FirstName = "Carol", LastName = "Smith", Age = 28 },
        };
    }
}
```

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:CardView ItemsSource="{Binding CardViewItems}" Name="cardView">
    <syncfusion:CardView.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding FirstName}"/>
        </DataTemplate>
    </syncfusion:CardView.HeaderTemplate>
    <syncfusion:CardView.ItemTemplate>
        <DataTemplate>
            <ListBox ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                <ListBoxItem>
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="75"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="First Name:"/>
                        <TextBlock Grid.Column="1" Text="{Binding FirstName}"/>
                    </Grid>
                </ListBoxItem>
                <ListBoxItem>
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="75"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="Last Name:"/>
                        <TextBlock Grid.Column="1" Text="{Binding LastName}"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </DataTemplate>
    </syncfusion:CardView.ItemTemplate>
</syncfusion:CardView>
```

---

## Selection

**Bind SelectedItem in XAML:**
```xml
<syncfusion:CardView SelectedItem="{Binding SelectedPerson}"
                     ItemsSource="{Binding CardViewItems}"
                     Name="cardView"/>
```

**Mark item as pre-selected (declarative):**
```xml
<syncfusion:CardViewItem Header="Card 2" IsSelected="True">
    <TextBlock Text="This card is selected by default"/>
</syncfusion:CardViewItem>
```

**React to selection changes:**
```xml
<syncfusion:CardView SelectedItemChanged="CardView_SelectedItemChanged" .../>
```
```csharp
private void CardView_SelectedItemChanged(object sender, SelectedItemChangedEventArgs e)
{
    var selectedItem = e.NewValue as Person;
    // handle selection
}
```

---

## Orientation

Control the layout direction of cards:

```xml
<!-- Default: Horizontal (cards wrap left-to-right) -->
<syncfusion:CardView Orientation="Horizontal" .../>

<!-- Vertical: cards stack top-to-bottom -->
<syncfusion:CardView Orientation="Vertical" .../>
```

---

## Apply Theme

```csharp
using Syncfusion.SfSkinManager;

// In constructor after InitializeComponent()
SfSkinManager.SetVisualStyle(cardView, VisualStyles.FluentLight);
```

Available styles: `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Windows11Light`, `Windows11Dark`, `Office2019Colorful`, and more.

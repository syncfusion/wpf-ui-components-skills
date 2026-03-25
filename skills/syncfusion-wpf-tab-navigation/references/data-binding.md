# Data Binding in TabNavigationControl

## Table of Contents
- [Binding ObservableCollection](#binding-observablecollection)
- [Binding from XML Data Source](#binding-from-xml-data-source)
- [MVVM Pattern](#mvvm-pattern)
- [Gotchas](#gotchas)

---

## Binding ObservableCollection

The simplest data binding approach — build a collection of `TabNavigationItem` objects and bind it to `ItemsSource`.

### ViewModel / Code-behind

```csharp
using System.Collections.ObjectModel;
using Syncfusion.Windows.Tools.Controls;

public partial class MainWindow : Window
{
    public ObservableCollection<TabNavigationItem> MyCollection
    {
        get { return (ObservableCollection<TabNavigationItem>)GetValue(MyCollectionProperty); }
        set { SetValue(MyCollectionProperty, value); }
    }

    public static readonly DependencyProperty MyCollectionProperty =
        DependencyProperty.Register(
            "MyCollection",
            typeof(ObservableCollection<TabNavigationItem>),
            typeof(MainWindow),
            new PropertyMetadata(null));

    public MainWindow()
    {
        InitializeComponent();

        MyCollection = new ObservableCollection<TabNavigationItem>();
        for (int i = 0; i < 10; i++)
        {
            var item = new TabNavigationItem();
            item.Header  = i.ToString();
            item.Content = "Item " + i.ToString();
            MyCollection.Add(item);
        }

        this.DataContext = this;
    }
}
```

### XAML

```xaml
<syncfusion:TabNavigationControl TransitionEffect="Slide"
                                  ItemsSource="{Binding MyCollection}"/>
```

---

## Binding from XML Data Source

Load XML data into a typed collection, then bind to `ItemsSource`.

### XML File (`Assets/Books.xml`)

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Books>
  <book>
    <title>XML Developer's Guide</title>
    <description>An in-depth look at creating applications with XML.</description>
  </book>
  <book>
    <title>Midnight Rain</title>
    <description>A former architect battles corporate zombies to become queen of the world.</description>
  </book>
  <book>
    <title>Oberon's Legacy</title>
    <description>The mysterious agent Oberon helps rebuild life in post-apocalypse London.</description>
  </book>
</Books>
```

### Model

```csharp
public class BookModel
{
    public string BookName    { get; set; }
    public string Description { get; set; }
}
```

### ViewModel

```csharp
using System.Xml.Linq;
using System.Collections.ObjectModel;

public class ViewModel
{
    public ObservableCollection<BookModel> BookModelItems { get; set; }

    public ViewModel()
    {
        BookModelItems = new ObservableCollection<BookModel>();

        XDocument xDoc = XDocument.Load(@"Assets\Books.xml");
        foreach (XElement el in xDoc.Descendants("book"))
        {
            BookModelItems.Add(new BookModel
            {
                BookName    = el.Element("title").Value,
                Description = el.Element("description").Value
            });
        }
    }
}
```

### MainWindow Code-Behind

Convert the `BookModel` collection into `TabNavigationItem` objects for binding:

```csharp
using Syncfusion.Windows.Tools.Controls;
using System.Collections.ObjectModel;

public partial class MainWindow : Window
{
    public ObservableCollection<TabNavigationItem> BookCollection
    {
        get { return (ObservableCollection<TabNavigationItem>)GetValue(BookCollectionProperty); }
        set { SetValue(BookCollectionProperty, value); }
    }

    public static readonly DependencyProperty BookCollectionProperty =
        DependencyProperty.Register(
            "BookCollection",
            typeof(ObservableCollection<TabNavigationItem>),
            typeof(MainWindow),
            new PropertyMetadata(null));

    public MainWindow()
    {
        InitializeComponent();

        ViewModel view = new ViewModel();
        BookCollection = new ObservableCollection<TabNavigationItem>();

        foreach (var book in view.BookModelItems)
        {
            var tab = new TabNavigationItem();
            tab.Header  = book.BookName;
            tab.Content = book.Description;
            BookCollection.Add(tab);
        }

        this.DataContext = this;
    }
}
```

### XAML

```xaml
<syncfusion:TabNavigationControl x:Name="TabNavigation"
                                  TransitionEffect="Slide"
                                  ItemsSource="{Binding BookCollection}"/>
```

---

## MVVM Pattern

For clean MVVM separation, expose the collection from a dedicated ViewModel and set it as the DataContext:

```xaml
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>

<syncfusion:TabNavigationControl ItemsSource="{Binding TabItems}"
                                  TransitionEffect="Fade"/>
```

```csharp
public class MainViewModel
{
    public ObservableCollection<TabNavigationItem> TabItems { get; set; }

    public MainViewModel()
    {
        TabItems = new ObservableCollection<TabNavigationItem>();
        for (int i = 1; i <= 5; i++)
        {
            TabItems.Add(new TabNavigationItem
            {
                Header  = $"Tab {i}",
                Content = $"Content for Tab {i}"
            });
        }
    }
}
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Items not displaying | `DataContext` not set | Set `this.DataContext = this` or use `Window.DataContext` in XAML |
| Binding not updating | Using `List<T>` instead of `ObservableCollection<T>` | Switch to `ObservableCollection<TabNavigationItem>` |
| XML load failure | Incorrect file path in `XDocument.Load()` | Use absolute path or correct relative path from output directory |
| Empty tab headers | `Header` not assigned when building collection | Assign `tab.Header` before adding to collection |

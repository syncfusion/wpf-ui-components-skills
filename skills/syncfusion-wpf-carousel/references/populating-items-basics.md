````markdown
# Populating Items — Basics

Core patterns for adding content to the Carousel: direct `CarouselItem` elements, collection binding with `ItemsSource`, and `ItemTemplate` usage for custom visuals.

## Direct `CarouselItem` (static)

Use for small, static collections. Example:

```xml
<syncfusion:Carousel>
  <syncfusion:CarouselItem>
    <Viewbox Width="100" Height="100"><Image Source="Images/1.png"/></Viewbox>
  </syncfusion:CarouselItem>
</syncfusion:Carousel>
```

## Collection Binding (dynamic)

Bind to an `ObservableCollection<T>` via `ItemsSource` and provide an `ItemTemplate`.

```xml
<syncfusion:Carousel ItemsSource="{Binding HeaderCollection}">
  <syncfusion:Carousel.ItemTemplate>
    <DataTemplate><TextBlock Text="{Binding Header}"/></DataTemplate>
  </syncfusion:Carousel.ItemTemplate>
</syncfusion:Carousel>
```

For a complete `ViewModel` example, see `getting-started.md`.

````

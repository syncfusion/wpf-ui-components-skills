# Sorting & Filtering — CardView (WPF)

## Table of Contents
- [Sorting Overview](#sorting-overview)
- [Enable / Disable Sorting](#enable--disable-sorting)
- [Sort by Clicking Field Name](#sort-by-clicking-field-name)
- [Sort Grouped Items](#sort-grouped-items)
- [Filtering Overview](#filtering-overview)
- [Enable / Disable Filtering](#enable--disable-filtering)
- [Filter by Field Values](#filter-by-field-values)
- [Clear a Filter](#clear-a-filter)
- [Hide the Sorting / Filtering Header](#hide-the-sorting--filtering-header)
- [Combined Example](#combined-example)

---

## Sorting Overview

CardView allows users to sort cards by clicking a field name in the header. Each click cycles through: **default → ascending → descending → default**.

> **Prerequisite:** Sorting requires `ItemsSource` data binding. It does **not** work with declarative `CardViewItem` children.

---

## Enable / Disable Sorting

```xml
<!-- Enable (default is true) -->
<syncfusion:CardView CanSort="True" ItemsSource="{Binding Employees}" Name="cardView"/>

<!-- Disable -->
<syncfusion:CardView CanSort="False" ItemsSource="{Binding Employees}" Name="cardView"/>
```

```csharp
cardView.CanSort = true;   // enable
cardView.CanSort = false;  // disable
```

---

## Sort by Clicking Field Name

When `CanSort="True"`, the header area shows field names from the bound data. The user clicks a field name to sort:

- **1st click** → Ascending order
- **2nd click** → Descending order
- **3rd click** → Default order (unsorted)

```xml
<syncfusion:CardView CanSort="True"
                     ItemsSource="{Binding Employees}"
                     Name="cardView">
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
                <ListBoxItem>
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="75"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="Age:"/>
                        <TextBlock Grid.Column="1" Text="{Binding Age}"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </DataTemplate>
    </syncfusion:CardView.ItemTemplate>
</syncfusion:CardView>
```

---

## Sort Grouped Items

When `CanGroup="True"` and `CanSort="True"` are both set, the user can click field names in the **grouping drop region** to sort grouped items:

```xml
<syncfusion:CardView CanGroup="True"
                     CanSort="True"
                     ItemsSource="{Binding Employees}"
                     Name="cardView"/>
```

```csharp
cardView.CanGroup = true;
cardView.CanSort  = true;
```

---

## Filtering Overview

CardView allows users to filter cards by selecting specific field values from a popup. Cards that don't match the selected values are hidden.

> **Prerequisite:** Filtering requires `ItemsSource` data binding.

---

## Enable / Disable Filtering

Filtering is enabled by default when `ShowHeader="True"` (default). The filter icon appears on each field column in the header area. There is no explicit `CanFilter` property — filtering is available whenever the header is visible with `ItemsSource`.

To disable filtering, hide the header:
```xml
<syncfusion:CardView ShowHeader="False" ItemsSource="{Binding Employees}" Name="cardView"/>
```

---

## Filter by Field Values

1. Ensure `ShowHeader="True"` (default) and `ItemsSource` is bound
2. Click the **filter icon** next to a field name in the header
3. A popup appears listing all unique values for that field
4. Check/uncheck values to show only matching cards

```xml
<syncfusion:CardView ItemsSource="{Binding Employees}"
                     Name="cardView">
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
                        <TextBlock Text="Department:"/>
                        <TextBlock Grid.Column="1" Text="{Binding Department}"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </DataTemplate>
    </syncfusion:CardView.ItemTemplate>
</syncfusion:CardView>
```

---

## Clear a Filter

Inside the filter popup, a **Clear Filter** button appears at the bottom. Clicking it removes all filter conditions for that field and shows all cards again.

> There is no programmatic API to clear filters — it must be done via the UI popup.

---

## Hide the Sorting / Filtering Header

Use `ShowHeader="False"` to hide the entire header bar (including both the sorting field list and the filtering icons). This removes interactive sorting and filtering from the UI.

```xml
<syncfusion:CardView ShowHeader="False"
                     ItemsSource="{Binding Employees}"
                     Name="cardView"/>
```

```csharp
cardView.ShowHeader = false;
```

---

## Combined Example

Enable grouping, sorting, and filtering together:

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:CardView CanGroup="True"
                     CanSort="True"
                     ItemsSource="{Binding Employees}"
                     Name="cardView">
    <syncfusion:CardView.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding FirstName}"/>
        </DataTemplate>
    </syncfusion:CardView.HeaderTemplate>
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
                        <TextBlock Grid.Column="1" Text="{Binding FirstName, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="90"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="Last Name:" FontWeight="SemiBold"/>
                        <TextBlock Grid.Column="1" Text="{Binding LastName, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="90"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="Age:" FontWeight="SemiBold"/>
                        <TextBlock Grid.Column="1" Text="{Binding Age, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </DataTemplate>
    </syncfusion:CardView.ItemTemplate>
</syncfusion:CardView>
```

**Behavior summary:**
- User drags a field (e.g., `Department`) into the drop region → cards are **grouped**
- User clicks a field name (e.g., `Age`) in the header → cards are **sorted**
- User clicks the filter icon on a field → popup opens for **filtering**

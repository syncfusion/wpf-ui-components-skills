# Grouping — CardView (WPF)

Group card items by field values. Cards with the same value appear together under a shared group header.

> **Prerequisite:** Grouping requires `ItemsSource` data binding. It does **not** work with declarative `CardViewItem` children.

---

## Enable Grouping

Set `CanGroup="True"` to allow the user to drag fields into the grouping header:

```xml
<syncfusion:CardView CanGroup="True"
                     ItemsSource="{Binding Employees}"
                     Name="cardView"/>
```

```csharp
cardView.CanGroup = true;
```

When enabled, a **drop zone** appears at the top of the control. The user drags a field name from the header into this drop zone to group cards by that field.

---

## Drag-Drop Grouping

1. Set `CanGroup="True"` and `ShowHeader="True"` (default)
2. The header region shows available field names (from bound properties)
3. User drags a field name into the grouping area at the top
4. Cards are immediately grouped by that field's values

---

## Programmatic Grouping

Use `GroupCards(string fieldName)` to group cards by a specific field without user interaction:

```csharp
// Group by the "Department" property
cardView.GroupCards("Department");
```

```xml
<!-- Example: button triggers programmatic grouping -->
<Button Content="Group by Department" Click="GroupByDept_Click"/>

<syncfusion:CardView CanGroup="True"
                     ItemsSource="{Binding Employees}"
                     Name="cardView"/>
```

```csharp
private void GroupByDept_Click(object sender, RoutedEventArgs e)
{
    cardView.GroupCards("Department");
}
```

> **Field name is case-sensitive** and must exactly match the bound property name on the model class.

---

## Multi-Level (Nested) Grouping

Group by multiple fields simultaneously. Drag additional fields into the grouping header, or call `GroupCards` multiple times:

```xml
<syncfusion:CardView CanGroup="True"
                     ItemsSource="{Binding Employees}"
                     Name="cardView"/>
```

```csharp
// Primary group: Department, secondary group: LastName
cardView.GroupCards("Department");
cardView.GroupCards("LastName");
```

Cards are grouped first by `Department`, then sub-grouped by `LastName` within each department.

---

## Hide the Grouping Header

Use `ShowHeader="False"` to hide the entire header area (grouping drop zone + field list). This also hides the sorting and filtering header.

```xml
<syncfusion:CardView ShowHeader="False"
                     ItemsSource="{Binding Employees}"
                     Name="cardView"/>
```

```csharp
cardView.ShowHeader = false;
```

> **When to use:** Hide the header when you want to control grouping entirely programmatically via `GroupCards()`, or when the UI should not expose interactive grouping to the end user.

---

## Complete Grouping Example

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <StackPanel Orientation="Horizontal" Margin="5">
        <Button Content="Group by Department" Click="GroupDept_Click" Margin="0,0,5,0"/>
        <Button Content="Group by Last Name"  Click="GroupLast_Click" Margin="0,0,5,0"/>
        <Button Content="Clear Groups"        Click="ClearGroups_Click"/>
    </StackPanel>
    <syncfusion:CardView Grid.Row="1"
                         CanGroup="True"
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
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="Dept: " FontWeight="Bold"/>
                            <TextBlock Text="{Binding Department}"/>
                        </StackPanel>
                    </ListBoxItem>
                    <ListBoxItem>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="Age: " FontWeight="Bold"/>
                            <TextBlock Text="{Binding Age}"/>
                        </StackPanel>
                    </ListBoxItem>
                </ListBox>
            </DataTemplate>
        </syncfusion:CardView.ItemTemplate>
    </syncfusion:CardView>
</Grid>
```

```csharp
private void GroupDept_Click(object sender, RoutedEventArgs e)
    => cardView.GroupCards("Department");

private void GroupLast_Click(object sender, RoutedEventArgs e)
    => cardView.GroupCards("LastName");

private void ClearGroups_Click(object sender, RoutedEventArgs e)
{
    // Reset to ungrouped state by re-binding or clearing group descriptor
    cardView.CanGroup = false;
    cardView.CanGroup = true;
}
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `GroupCards()` has no effect | Using declarative `CardViewItem` children | Switch to `ItemsSource` data binding |
| Field not appearing in header | Property name mismatch | Ensure exact case match with model property name |
| Header not visible | `ShowHeader="False"` set | Set `ShowHeader="True"` (default) |
| Groups not collapsing | Expected behavior — CardView groups are flat | Use a different control (e.g., GroupBox) for collapsible groups |

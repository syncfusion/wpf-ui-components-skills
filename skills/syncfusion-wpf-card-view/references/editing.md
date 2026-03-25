# Editing — CardView (WPF)

## Table of Contents
- [Enable Editing](#enable-editing)
- [EditItemTemplate Requirement](#edititemtemplate-requirement)
- [Keyboard and Mouse Interaction](#keyboard-and-mouse-interaction)
- [Programmatic Edit Control](#programmatic-edit-control)
- [Custom Edit UI Styling](#custom-edit-ui-styling)
- [Full Editing Example](#full-editing-example)
- [Gotchas](#gotchas)

---

## Enable Editing

Set `CanEdit="True"` on the `CardView`. The default is `false` — editing is **off** by default.

```xml
<syncfusion:CardView CanEdit="True"
                     ItemsSource="{Binding Employees}"
                     Name="cardView"/>
```

```csharp
cardView.CanEdit = true;
```

> **Prerequisite:** Editing requires `ItemsSource` data binding. It does **not** work with declarative `CardViewItem` children.

---

## EditItemTemplate Requirement

You **must** define `EditItemTemplate` to enable actual editing. Without it, `CanEdit="True"` has no visible effect.

- **`ItemTemplate`** — displayed in **view mode** (read-only `TextBlock`s)
- **`EditItemTemplate`** — displayed in **edit mode** (editable `TextBox`es)

The `DataContext` for both templates is the bound data item (e.g., `Employee`).

```xml
<syncfusion:CardView CanEdit="True" ItemsSource="{Binding Employees}" Name="cardView">

    <!-- View mode: TextBlocks -->
    <syncfusion:CardView.ItemTemplate>
        <DataTemplate>
            <ListBox ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="75"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="First Name:"/>
                        <TextBlock Grid.Column="1"
                                   Text="{Binding FirstName, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </DataTemplate>
    </syncfusion:CardView.ItemTemplate>

    <!-- Edit mode: TextBoxes with same bindings -->
    <syncfusion:CardView.EditItemTemplate>
        <DataTemplate>
            <ListBox ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="75"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="First Name:"/>
                        <TextBox Grid.Column="1"
                                 Text="{Binding FirstName, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </DataTemplate>
    </syncfusion:CardView.EditItemTemplate>

</syncfusion:CardView>
```

---

## Keyboard and Mouse Interaction

| Action | Result |
|---|---|
| **Double-click** a card | Enter edit mode |
| **F2** with card selected | Enter edit mode |
| **Enter** while editing | Commit and exit edit mode |
| **Esc** while editing | Cancel and exit edit mode |

---

## Programmatic Edit Control

Use `BeginEdit()` and `EndEdit()` to start/stop editing the currently selected card from code:

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <StackPanel Orientation="Horizontal" Margin="5">
        <Button Content="Begin Edit" Click="BeginEdit_Click" Margin="0,0,5,0"/>
        <Button Content="End Edit"   Click="EndEdit_Click"/>
    </StackPanel>
    <syncfusion:CardView Grid.Row="1"
                         CanEdit="True"
                         ItemsSource="{Binding Employees}"
                         Name="cardView">
        <syncfusion:CardView.EditItemTemplate>
            <DataTemplate>
                <ListBox ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                    <ListBoxItem Padding="1">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="75"/>
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
                                <ColumnDefinition Width="75"/>
                                <ColumnDefinition/>
                            </Grid.ColumnDefinitions>
                            <TextBlock Text="Last Name:"/>
                            <TextBox Grid.Column="1"
                                     Text="{Binding LastName, UpdateSourceTrigger=PropertyChanged}"/>
                        </Grid>
                    </ListBoxItem>
                </ListBox>
            </DataTemplate>
        </syncfusion:CardView.EditItemTemplate>
    </syncfusion:CardView>
</Grid>
```

```csharp
private void BeginEdit_Click(object sender, RoutedEventArgs e)
{
    // Starts editing the currently selected card
    cardView.BeginEdit();
}

private void EndEdit_Click(object sender, RoutedEventArgs e)
{
    // Commits changes and exits edit mode
    cardView.EndEdit();
}
```

> **Note:** `BeginEdit()` only works on the currently **selected** card. Ensure a card is selected before calling it.
> `CanEdit` must be `true` for both methods to function.

---

## Custom Edit UI Styling

Apply custom styling to `TextBox` controls inside `EditItemTemplate` to visually distinguish edit mode:

```xml
<syncfusion:CardView.EditItemTemplate>
    <DataTemplate>
        <ListBox ScrollViewer.HorizontalScrollBarVisibility="Disabled">
            <ListBoxItem Padding="1" HorizontalContentAlignment="Stretch">
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="75"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Text="First Name:"/>
                    <TextBox Grid.Column="1"
                             Background="#1A1A1A"
                             Foreground="White"
                             BorderBrush="#0078D4"
                             Text="{Binding FirstName, UpdateSourceTrigger=PropertyChanged}"/>
                </Grid>
            </ListBoxItem>
            <ListBoxItem Padding="1" HorizontalContentAlignment="Stretch">
                <Grid>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="75"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <TextBlock Text="Last Name:"/>
                    <TextBox Grid.Column="1"
                             Background="#E8F5E9"
                             Foreground="#1B5E20"
                             Text="{Binding LastName, UpdateSourceTrigger=PropertyChanged}"/>
                </Grid>
            </ListBoxItem>
        </ListBox>
    </DataTemplate>
</syncfusion:CardView.EditItemTemplate>
```

---

## Full Editing Example

Complete setup with header, view template, and edit template:

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:CardView CanEdit="True"
                     ItemsSource="{Binding Employees}"
                     Name="cardView">
    <syncfusion:CardView.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding FirstName}" FontWeight="Bold"/>
        </DataTemplate>
    </syncfusion:CardView.HeaderTemplate>

    <syncfusion:CardView.ItemTemplate>
        <DataTemplate>
            <ListBox ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="80"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="First Name:"/>
                        <TextBlock Grid.Column="1" Text="{Binding FirstName, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="80"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="Last Name:"/>
                        <TextBlock Grid.Column="1" Text="{Binding LastName, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="80"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="Age:"/>
                        <TextBlock Grid.Column="1" Text="{Binding Age, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </DataTemplate>
    </syncfusion:CardView.ItemTemplate>

    <syncfusion:CardView.EditItemTemplate>
        <DataTemplate>
            <ListBox ScrollViewer.HorizontalScrollBarVisibility="Disabled">
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="80"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="First Name:"/>
                        <TextBox Grid.Column="1" Text="{Binding FirstName, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="80"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="Last Name:"/>
                        <TextBox Grid.Column="1" Text="{Binding LastName, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
                <ListBoxItem Padding="1">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="80"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Text="Age:"/>
                        <TextBox Grid.Column="1" Text="{Binding Age, UpdateSourceTrigger=PropertyChanged}"/>
                    </Grid>
                </ListBoxItem>
            </ListBox>
        </DataTemplate>
    </syncfusion:CardView.EditItemTemplate>
</syncfusion:CardView>
```

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Editing appears enabled but nothing changes | `EditItemTemplate` not defined | Add `EditItemTemplate` with `TextBox` controls |
| `BeginEdit()` has no effect | `CanEdit="False"` or no card selected | Set `CanEdit="True"` and select a card first |
| Changes not saved to model | Binding mode or `UpdateSourceTrigger` missing | Use `UpdateSourceTrigger=PropertyChanged` in `TextBox` bindings |
| Edit mode not triggering with F2 | Card not focused | Click card to select/focus first, then press F2 |
| Declarative `CardViewItem` editing fails | Editing not supported with declarative items | Switch to `ItemsSource` data binding |

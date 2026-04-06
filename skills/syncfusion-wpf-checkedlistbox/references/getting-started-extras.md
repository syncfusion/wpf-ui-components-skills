````markdown
# Getting Started — Full Examples & Extras

This file contains the longer walkthroughs, full XAML/C# examples, and localization instructions moved out of the main getting-started summary.

## NuGet Installation

```powershell
Install-Package Syncfusion.Tools.WPF
```

## Full MainWindow.xaml Example

```xml
<Window x:Class="CheckListBoxDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="CheckListBox Getting Started" Height="400" Width="500">
    <Grid Margin="20">
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        
        <syncfusion:CheckListBox x:Name="checkListBox" Grid.Row="0" ItemChecked="CheckListBox_ItemChecked">
            <syncfusion:CheckListBoxItem Content="Austria" />
            <syncfusion:CheckListBoxItem Content="Australia" />
            <syncfusion:CheckListBoxItem Content="Canada" IsSelected="True" />
            <syncfusion:CheckListBoxItem Content="Finland" />
            <syncfusion:CheckListBoxItem Content="New Zealand" />
        </syncfusion:CheckListBox>
        
        <Button Grid.Row="1" Content="Show Checked Items" Click="ShowCheckedItems_Click" Padding="10,5" />
    </Grid>
</Window>
```

## Code-behind handlers

```csharp
private void CheckListBox_ItemChecked(object sender, ItemCheckedEventArgs e)
{
    var item = e.Item as CheckListBoxItem;
    this.Title = $"{item.Content} was {(e.IsChecked ? "checked" : "unchecked")}";
}

private void ShowCheckedItems_Click(object sender, RoutedEventArgs e)
{
    var sb = new StringBuilder();
    foreach (var item in checkListBox.SelectedItems)
    {
        var checkListItem = item as CheckListBoxItem;
        sb.AppendLine($"- {checkListItem.Content}");
    }
    MessageBox.Show(sb.ToString());
}
```

## Localization

See Syncfusion docs for localizing built-in strings; create `.resx` files and set `CurrentUICulture` appropriately.

````

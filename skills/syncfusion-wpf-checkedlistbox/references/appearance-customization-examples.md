````markdown
# Appearance and Customization — Extended Examples

This companion file contains the more verbose examples, templates, and troubleshooting content moved out of `appearance-customization.md` to reduce duplication and file length.

## Color and Background Examples

### Foreground and Background

```xml
<syncfusion:CheckListBox Foreground="#1976D2" Background="White">
    <syncfusion:CheckListBoxItem Content="Mexico" />
    <syncfusion:CheckListBoxItem Content="Canada" />
    <syncfusion:CheckListBoxItem Content="Bermuda" />
</syncfusion:CheckListBox>
```

```csharp
checkListBox.Foreground = Brushes.DodgerBlue;
checkListBox.Background = Brushes.White;
```

### Gradient Background

```xml
<syncfusion:CheckListBox>
  <syncfusion:CheckListBox.Background>
    <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
      <GradientStop Color="#E3F2FD" Offset="0" />
      <GradientStop Color="#BBDEFB" Offset="1" />
    </LinearGradientBrush>
  </syncfusion:CheckListBox.Background>
</syncfusion:CheckListBox>
```

## Item Templates and Container Styles

### ItemTemplate with icon and subtitle

```xml
<syncfusion:CheckListBox ItemsSource="{Binding Countries}">
  <syncfusion:CheckListBox.ItemTemplate>
    <DataTemplate>
      <StackPanel Orientation="Horizontal">
        <Image Source="{Binding FlagImage}" Width="24" Height="16" Margin="0,0,8,0" />
        <StackPanel>
          <TextBlock Text="{Binding Name}" FontWeight="Bold" />
          <TextBlock Text="{Binding Region}" FontSize="10" Foreground="Gray" />
        </StackPanel>
      </StackPanel>
    </DataTemplate>
  </syncfusion:CheckListBox.ItemTemplate>
</syncfusion:CheckListBox>
```

### ItemContainerStyle: selection binding and hover

```xml
<syncfusion:CheckListBox>
  <syncfusion:CheckListBox.ItemContainerStyle>
    <Style TargetType="syncfusion:CheckListBoxItem">
      <Setter Property="IsSelected" Value="{Binding IsChecked, Mode=TwoWay}" />
      <Style.Triggers>
        <Trigger Property="IsMouseOver" Value="True">
          <Setter Property="Background" Value="#F5F5F5" />
        </Trigger>
      </Style.Triggers>
    </Style>
  </syncfusion:CheckListBox.ItemContainerStyle>
</syncfusion:CheckListBox>
```

## ControlTemplate Example

```xml
<syncfusion:CheckListBox>
  <syncfusion:CheckListBox.Template>
    <ControlTemplate TargetType="syncfusion:CheckListBox">
      <Border Background="{TemplateBinding Background}" BorderBrush="{TemplateBinding BorderBrush}" BorderThickness="{TemplateBinding BorderThickness}" CornerRadius="4">
        <ScrollViewer Padding="{TemplateBinding Padding}">
          <ItemsPresenter />
        </ScrollViewer>
      </Border>
    </ControlTemplate>
  </syncfusion:CheckListBox.Template>
</syncfusion:CheckListBox>
```

## Troubleshooting (concise)

- If colors don't apply, verify theme packages and that implicit styles aren't overriding local setters.
- If `CheckBoxPlacement` doesn't seem to work, check whether your Syncfusion version exposes `CheckBoxAlignment` instead; prefer the property that matches your package version.
- When ItemTemplate isn't visible, remove `DisplayMemberPath` or ensure the `ItemsSource` binding matches your ViewModel.

````

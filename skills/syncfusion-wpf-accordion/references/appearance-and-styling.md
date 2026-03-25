# Appearance and Styling

## Table of Contents
- [AccentBrush](#accentbrush)
- [HeaderTemplate](#headertemplate)
- [ContentTemplate](#contenttemplate)
- [AccordionButtonStyle](#accordionbuttonstyle)
- [ItemContainerStyle](#itemcontainerstyle)
- [Animation](#animation)
- [Themes with SfSkinManager](#themes-with-sfskinmanager)
- [Gotchas](#gotchas)

---

## AccentBrush

Highlights the hot spots of the accordion control (active item indicator, focus states):

```xaml
<syncfusion:SfAccordion AccentBrush="DodgerBlue"
                        Width="300" Height="200">
    <syncfusion:SfAccordionItem Header="WPF"        Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="Silverlight" Content="Essential Studio for Silverlight"/>
</syncfusion:SfAccordion>
```

```csharp
accordion.AccentBrush = new SolidColorBrush(Colors.DodgerBlue);
```

---

## HeaderTemplate

Apply a shared `DataTemplate` to all item headers:

```xaml
<syncfusion:SfAccordion Width="300" Height="250">
    <syncfusion:SfAccordion.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding}"
                       Foreground="DarkSlateBlue"
                       FontFamily="Calibri"
                       FontWeight="Bold"
                       FontSize="16"/>
        </DataTemplate>
    </syncfusion:SfAccordion.HeaderTemplate>
    <syncfusion:SfAccordionItem Header="WindowsForms"/>
    <syncfusion:SfAccordionItem Header="WPF"/>
    <syncfusion:SfAccordionItem Header="UWP"/>
    <syncfusion:SfAccordionItem Header="WinRT"/>
</syncfusion:SfAccordion>
```

> When items are added directly (not via `ItemsSource`), `{Binding}` binds to the `Header` value.

Customize header height via `HeaderTemplate`:

```xaml
<syncfusion:SfAccordion.HeaderTemplate>
    <DataTemplate>
        <TextBlock Text="{Binding Name}" Height="50" FontSize="20" VerticalAlignment="Center"/>
    </DataTemplate>
</syncfusion:SfAccordion.HeaderTemplate>
```

---

## ContentTemplate

Apply a shared `DataTemplate` to all item content areas:

```xaml
<syncfusion:SfAccordion ItemsSource="{Binding Employees}"
                        DisplayMemberPath="Name">
    <syncfusion:SfAccordion.ContentTemplate>
        <DataTemplate>
            <StackPanel Margin="8">
                <TextBlock Text="{Binding Description}"
                           TextWrapping="Wrap"
                           Height="60"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfAccordion.ContentTemplate>
</syncfusion:SfAccordion>
```

---

## AccordionButtonStyle

Customize the expander toggle button (`TargetType="syncfusion:AccordionButton"`). Apply via `ItemContainerStyle`:

```xaml
<Window.Resources>
    <!-- Custom expander button style -->
    <Style x:Key="CustomExpanderStyle" TargetType="syncfusion:AccordionButton">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="syncfusion:AccordionButton">
                    <Border x:Name="background"
                            Background="{TemplateBinding Background}"
                            CornerRadius="4">
                        <VisualStateManager.VisualStateGroups>
                            <VisualStateGroup x:Name="ExpansionStates">
                                <VisualState x:Name="Collapsed">
                                    <Storyboard>
                                        <DoubleAnimation
                                            Storyboard.TargetName="arrow"
                                            Storyboard.TargetProperty="(UIElement.RenderTransform).(TransformGroup.Children)[2].(RotateTransform.Angle)"
                                            To="0" Duration="0:0:0.3" BeginTime="0"/>
                                    </Storyboard>
                                </VisualState>
                                <VisualState x:Name="Expanded">
                                    <Storyboard>
                                        <DoubleAnimation
                                            Storyboard.TargetName="arrow"
                                            Storyboard.TargetProperty="(UIElement.RenderTransform).(TransformGroup.Children)[2].(RotateTransform.Angle)"
                                            To="90" Duration="0:0:0.3" BeginTime="0"/>
                                    </Storyboard>
                                </VisualState>
                            </VisualStateGroup>
                        </VisualStateManager.VisualStateGroups>
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="32"/>
                                <ColumnDefinition Width="*"/>
                            </Grid.ColumnDefinitions>
                            <Grid x:Name="arrow"
                                  Width="16" Height="16"
                                  HorizontalAlignment="Center"
                                  VerticalAlignment="Center"
                                  RenderTransformOrigin="0.5,0.5">
                                <Grid.RenderTransform>
                                    <TransformGroup>
                                        <ScaleTransform/>
                                        <SkewTransform/>
                                        <RotateTransform Angle="-90"/>
                                        <TranslateTransform/>
                                    </TransformGroup>
                                </Grid.RenderTransform>
                                <Path HorizontalAlignment="Center"
                                      VerticalAlignment="Center"
                                      Stroke="DarkBlue"
                                      StrokeThickness="2"
                                      Stretch="Uniform"
                                      Data="M0,0 L8,6 16,0"/>
                            </Grid>
                            <ContentPresenter x:Name="header"
                                              Grid.Column="1"
                                              Content="{TemplateBinding Content}"
                                              ContentTemplate="{TemplateBinding ContentTemplate}"
                                              VerticalAlignment="Center"/>
                        </Grid>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<syncfusion:SfAccordion Width="300" Height="250">
    <syncfusion:SfAccordion.ItemContainerStyle>
        <Style TargetType="syncfusion:SfAccordionItem">
            <Setter Property="AccordionButtonStyle" Value="{StaticResource CustomExpanderStyle}"/>
        </Style>
    </syncfusion:SfAccordion.ItemContainerStyle>
    <syncfusion:SfAccordionItem Header="WPF"        Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="Silverlight" Content="Essential Studio for Silverlight"/>
</syncfusion:SfAccordion>
```

---

## ItemContainerStyle

Apply a consistent style to all `SfAccordionItem` elements:

```xaml
<syncfusion:SfAccordion Width="300" Height="250">
    <syncfusion:SfAccordion.ItemContainerStyle>
        <Style TargetType="syncfusion:SfAccordionItem">
            <Setter Property="Foreground"   Value="DarkGreen"/>
            <Setter Property="FontWeight"   Value="SemiBold"/>
            <Setter Property="BorderBrush"  Value="LightGray"/>
            <Setter Property="BorderThickness" Value="0,0,0,1"/>
        </Style>
    </syncfusion:SfAccordion.ItemContainerStyle>
    <syncfusion:SfAccordionItem Header="WPF"        Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="Silverlight" Content="Essential Studio for Silverlight"/>
</syncfusion:SfAccordion>
```

---

## Animation

Control the expand/collapse animation speed and behavior via `ExpandableContentControl.Percentage` in a custom `SfAccordionItem` style:

```xaml
<Window.Resources>
    <!-- Slow animation style -->
    <Style x:Key="SlowAnimationStyle" TargetType="syncfusion:SfAccordionItem">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="syncfusion:SfAccordionItem">
                    <Border Padding="{TemplateBinding Padding}"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}">
                        <VisualStateManager.VisualStateGroups>
                            <VisualStateGroup x:Name="ExpansionStates">
                                <VisualStateGroup.Transitions>
                                    <VisualTransition GeneratedDuration="0"/>
                                </VisualStateGroup.Transitions>
                                <VisualState x:Name="Collapsed">
                                    <Storyboard>
                                        <DoubleAnimationUsingKeyFrames
                                            BeginTime="0:0:0"
                                            Duration="0:0:1"
                                            Storyboard.TargetName="ExpandSite"
                                            Storyboard.TargetProperty="(layout:ExpandableContentControl.Percentage)">
                                            <SplineDoubleKeyFrame KeySpline="0.2,0,0,1" KeyTime="0:0:1" Value="0"/>
                                        </DoubleAnimationUsingKeyFrames>
                                    </Storyboard>
                                </VisualState>
                                <VisualState x:Name="Expanded">
                                    <Storyboard>
                                        <DoubleAnimationUsingKeyFrames
                                            BeginTime="0:0:0"
                                            Duration="0:0:1"
                                            Storyboard.TargetName="ExpandSite"
                                            Storyboard.TargetProperty="(layout:ExpandableContentControl.Percentage)">
                                            <SplineDoubleKeyFrame KeySpline="0.2,0,0,1" KeyTime="0:0:1" Value="1"/>
                                        </DoubleAnimationUsingKeyFrames>
                                    </Storyboard>
                                </VisualState>
                            </VisualStateGroup>
                        </VisualStateManager.VisualStateGroups>
                        <Grid>
                            <Grid.RowDefinitions>
                                <RowDefinition Height="Auto"/>
                                <RowDefinition Height="Auto"/>
                            </Grid.RowDefinitions>
                            <syncfusion:AccordionButton
                                Name="ExpanderButton"
                                Style="{TemplateBinding AccordionButtonStyle}"
                                Content="{TemplateBinding Header}"
                                ContentTemplate="{TemplateBinding HeaderTemplate}"
                                IsChecked="{TemplateBinding IsSelected}"
                                Grid.Row="0"/>
                            <syncfusion:ExpandableContentControl
                                Name="ExpandSite"
                                Grid.Row="1"
                                Percentage="0"
                                Content="{TemplateBinding Content}"
                                ContentTemplate="{TemplateBinding ContentTemplate}"/>
                        </Grid>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</Window.Resources>

<!-- Apply to accordion -->
<syncfusion:SfAccordion ItemContainerStyle="{StaticResource SlowAnimationStyle}"
                        Width="300" Height="250">
    <syncfusion:SfAccordionItem Header="WPF"        Content="Essential Studio for WPF"/>
    <syncfusion:SfAccordionItem Header="Silverlight" Content="Essential Studio for Silverlight"/>
</syncfusion:SfAccordion>
```

> To **disable** animation, set `Duration="0:0:0"` on both `Collapsed` and `Expanded` storyboards.

---

## Themes with SfSkinManager

```xaml
xmlns:syncfusionskin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"

<syncfusion:SfAccordion
    syncfusionskin:SfSkinManager.Theme="{syncfusionskin:SkinManagerExtension ThemeName=FluentLight}"
    Width="300" Height="250">
    <syncfusion:SfAccordionItem Header="WPF" Content="Essential Studio for WPF"/>
</syncfusion:SfAccordion>
```

**Available themes:**

| Theme Name | Style |
|---|---|
| `FluentLight` | Windows 11 Fluent light |
| `FluentDark` | Windows 11 Fluent dark |
| `MaterialLight` | Material Design light |
| `MaterialDark` | Material Design dark |
| `Office2019Colorful` | Office 2019 colorful |
| `Office2019Black` | Office 2019 black |
| `Windows11Light` | Windows 11 light |
| `Windows11Dark` | Windows 11 dark |
| `SystemTheme` | Matches OS system theme |

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| `AccordionButtonStyle` has no effect | Style applied to wrong element | Use `ItemContainerStyle` → set `AccordionButtonStyle` setter on `SfAccordionItem` |
| Animation too fast/slow | Default `KeyTime` in `ControlTemplate` | Override `ItemContainerStyle` with custom `Duration` |
| `AccentBrush` not visible | Theme overrides default colors | Apply `AccentBrush` after setting `SfSkinManager.Theme` |
| `HeaderTemplate` binding is empty | Using `{Binding Name}` but items added directly | Use `{Binding}` (no path) for direct items; use `{Binding Name}` with `ItemsSource` |
| Custom `ControlTemplate` breaks expand | `ExpandSite` or `ExpanderButton` element name incorrect | Ensure `x:Name="ExpandSite"` on `ExpandableContentControl` and `x:Name="ExpanderButton"` on `AccordionButton` |

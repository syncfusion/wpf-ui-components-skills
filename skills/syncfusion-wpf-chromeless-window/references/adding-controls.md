---
layout: post
title: Adding controls in the title bar | Syncfusion
description: How to place and align custom controls in the ChromelessWindow title bar
platform: wpf
control: ChromelessWindow
documentation: ug
---
# Adding controls in the title bar

You can add controls into the TitleBar by supplying a custom `TitleBarTemplate` which includes your controls.

{% highlight XAML %}
<ControlTemplate x:Key="TitleBarTemplateKey" TargetType="syncfusion:TitleBar">
    <Grid Height="30">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <StackPanel Orientation="Horizontal" Grid.Column="0">
            <Image Source="AppIcon.png" Width="16" Height="16" />
        </StackPanel>

        <ContentPresenter Grid.Column="1" HorizontalAlignment="Center" VerticalAlignment="Center" />

        <StackPanel Orientation="Horizontal" Grid.Column="2" HorizontalAlignment="Right">
            <Button Content="Help" />
            <Button Content="Settings" />
        </StackPanel>
    </Grid>
</ControlTemplate>
{% endhighlight %}

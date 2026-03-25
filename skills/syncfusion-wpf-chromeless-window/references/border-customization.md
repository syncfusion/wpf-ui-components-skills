---
layout: post
title: Customizing Border of the ChromelessWindow | Syncfusion
description: Learn about customizing the border support in SynclessWindow control.
platform: wpf
control: ChromelessWindow
documentation: ug
---
# Customizing Border of the ChromelessWindow

ChromelessWindow exposes properties that allow you to change the border appearance.

## CornerRadius and BorderThickness

Use the `CornerRadius` and `BorderThickness` properties on the window template or outer border to change the radius and thickness of the window edge.

{% highlight XAML %}
<syncfusion:ChromelessWindow Title="Border Sample" Height="350" Width="525">
    <syncfusion:ChromelessWindow.Template>
        <ControlTemplate TargetType="syncfusion:ChromelessWindow">
            <Border Background="White" BorderBrush="Gray" BorderThickness="2" CornerRadius="8">
                <Grid>
                    <syncfusion:TitleBar x:Name="PART_TitleBar" />
                    <ContentPresenter />
                </Grid>
            </Border>
        </ControlTemplate>
    </syncfusion:ChromelessWindow.Template>
</syncfusion:ChromelessWindow>
{% endhighlight %}

## Programmatic changes

You can also set these values in code-behind:

{% highlight c# %}
this.BorderThickness = new Thickness(3);
this.CornerRadius = new CornerRadius(10);
{% endhighlight %}

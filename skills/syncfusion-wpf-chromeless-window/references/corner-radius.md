---
layout: post
title: Corner Radius in ChromelessWindow | Syncfusion
description: Learn about Corner Radius support in ChromelessWindow control
platform: wpf
control: ChromelessWindow
documentation: ug
---
# Corner Radius

The `CornerRadius` allows rounded corners for the ChromelessWindow. You can bind or set this on the window template to adjust the roundness.

{% highlight XAML %}
<syncfusion:ChromelessWindow Title="Corner Radius" Height="300" Width="500">
    <syncfusion:ChromelessWindow.Template>
        <ControlTemplate TargetType="syncfusion:ChromelessWindow">
            <Border CornerRadius="12" Background="White" BorderBrush="Black" BorderThickness="1">
                <Grid>
                    <syncfusion:TitleBar x:Name="PART_TitleBar" />
                    <ContentPresenter />
                </Grid>
            </Border>
        </ControlTemplate>
    </syncfusion:ChromelessWindow.Template>
</syncfusion:ChromelessWindow>
{% endhighlight %}

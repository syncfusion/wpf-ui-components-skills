---
layout: post
title: Title bar custom buttons | Syncfusion
description: Customize maximize, minimize, restore and close buttons in the ChromelessWindow title bar.
platform: wpf
control: ChromelessWindow
documentation: ug
---
# Title bar custom buttons

ChromelessWindow exposes properties to override the templates for the title bar buttons: `MaximizeButtonTemplate`, `MinimizeButtonTemplate`, `RestoreButtonTemplate` and `CloseButtonTemplate`.

## Example: custom close button

{% highlight XAML %}
<ControlTemplate x:Key="CloseButtonTemplate" TargetType="syncfusion:TitleButton">
    <Border Background="Transparent" Width="18" Height="18">
        <Path Data="M2,2 L16,16 M16,2 L2,16" Stroke="Red" StrokeThickness="2" />
    </Border>
</ControlTemplate>

<syncfusion:ChromelessWindow CloseButtonTemplate="{StaticResource CloseButtonTemplate}" Title="Custom Buttons Sample" Height="300" Width="500">
</syncfusion:ChromelessWindow>
{% endhighlight %}

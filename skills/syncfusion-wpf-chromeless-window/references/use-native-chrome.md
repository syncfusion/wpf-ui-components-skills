---
layout: post
title: Use Native Chrome with ChromelessWindow | Syncfusion
description: How to switch between native and chromeless window chrome
platform: wpf
control: ChromelessWindow
documentation: ug
---
# Use Native Chrome

ChromelessWindow supports opting into native OS chrome in scenarios where you prefer the system window frame. Use the `UseNativeChrome` property to toggle this behavior.

{% highlight XAML %}
<syncfusion:ChromelessWindow UseNativeChrome="True" Title="Native Chrome" Height="300" Width="500">
    <Grid />
</syncfusion:ChromelessWindow>
{% endhighlight %}

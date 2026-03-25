---
layout: post
title: Set Visual Styles for ChromelessWindow | Syncfusion
description: How to set visual styles (themes) for ChromelessWindow control
platform: wpf
control: ChromelessWindow
documentation: ug
---
# Set Visual Styles

ChromelessWindow integrates with Syncfusion skinning. Use `SkinStorage.VisualStyle` or `SfSkinManager` to apply visual themes.

{% highlight XAML %}
<syncfusion:ChromelessWindow SkinStorage.VisualStyle="Metro" Title="Themed Window" Height="350" Width="525">
    <Grid />
</syncfusion:ChromelessWindow>
{% endhighlight %}

---
layout: post
title: Getting Started with WPF Double TextBox control | Syncfusion®
description: Learn here about getting started with Syncfusion® WPF Double TextBox control, its elements and more details.
platform: WPF
control: DoubleTextBox
documentation: ug
---

# Getting Started with WPF DoubleTextBox

This section explains how to create a WPF `DoubleTextBox` control and its features.

## Assembly deployment

Refer to the control dependencies section to get the list of assemblies or NuGet package that needs to be added as a reference to use the control in any application.

## Adding WPF DoubleTextBox via designer

You can add the `DoubleTextBox` control to an application by dragging it from the toolbox to a view of the designer. The following dependent assembly will be added automatically:

* Syncfusion.Shared.WPF

## Adding WPF DoubleTextBox via XAML

To add the DoubleTextBox control manually in XAML, follow these steps:
1. Create a new WPF project in Visual Studio.
2. Add the `Syncfusion.Shared.WPF` assembly references to the project.
3. Import Syncfusion WPF schema and declare the `DoubleTextBox` control in XAML page.

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf" 
        x:Class="DoubleTextBoxSample.MainWindow"
        Title="DoubleTextBox Sample" Height="350" Width="525">
    <Grid>
        <!--Adding DoubleTextBox control -->
        <syncfusion:DoubleTextBox x:Name="doubleTextBox" Width="100" Height="25" VerticalAlignment="Center" HorizontalAlignment="Center"/>
    </Grid>
</Window>
```

## Adding WPF DoubleTextBox via C#

Include the required namespace and create an instance of `DoubleTextBox`:

```csharp
using Syncfusion.Windows.Shared;

DoubleTextBox doubleTextBox = new DoubleTextBox();
doubleTextBox.Height = 25;
doubleTextBox.Width = 100;
this.Content = doubleTextBox;
```

## Setting Value

Set the `Value` property rather than `Text`.

```xaml
<syncfusion:DoubleTextBox x:Name="doubleTextBox" Width="100" Height="23" Value="100"/>
```

## Binding Value

Bind `Value` to ViewModel properties with `UpdateSourceTrigger=PropertyChanged` for live updates.

## Value Changed Notification

Handle the `ValueChanged` event to receive old and new values.

## Min Max Value Restriction

Use `MinValue` and `MaxValue` to limit allowed values; set `MinValidation`/`MaxValidation` to control when validation occurs.

## Step Interval to increase or decrease the value

Use `ScrollInterval` and related properties to control increment/decrement behavior.

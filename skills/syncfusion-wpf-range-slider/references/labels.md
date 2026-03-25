# Label Support in WPF Range Slider

Complete reference guide for implementing and customizing labels in Syncfusion WPF Range Slider (SfRangeSlider).

## Table of Contents

1. [Overview](#overview)
2. [Properties Reference](#properties-reference)
3. [ShowValueLabels Property](#showvaluelabels-property)
4. [ShowCustomLabels Property](#showcustomlabels-property)
5. [CustomLabels Collection](#customlabels-collection)
   - [Creating Custom Label Models](#creating-custom-label-models)
   - [ViewModel Implementation](#viewmodel-implementation)
   - [Binding Custom Labels](#binding-custom-labels)
6. [LabelPlacement Property](#labelplacement-property)
   - [BottomRight Placement](#bottomright-placement)
   - [TopLeft Placement](#topleft-placement)
7. [ValuePlacement Property](#valueplacement-property)
   - [TopLeft Value Placement](#topleft-value-placement)
   - [BottomRight Value Placement](#bottomright-value-placement)
8. [LabelOrientation Property](#labelorientation-property)
   - [Horizontal Orientation](#horizontal-orientation)
   - [Vertical Orientation](#vertical-orientation)
9. [LabelLoaded Event](#labelloaded-event)
10. [Complete Working Examples](#complete-working-examples)
    - [Basic Label Example](#basic-label-example)
    - [Custom Labels with MVVM](#custom-labels-with-mvvm)
    - [Advanced Label Customization](#advanced-label-customization)
11. [Best Practices](#best-practices)
12. [Common Scenarios](#common-scenarios)

## Overview

The Label Support feature in WPF Range Slider allows you to display labels for both tick values and custom values. You can customize label positioning, orientation, and content to create intuitive user interfaces.

### Key Features:
- Display value labels for all tick marks
- Create custom labels for specific values
- Control label placement and orientation
- Support for data binding with MVVM pattern
- Event-driven label customization

## Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ShowValueLabels` | bool | false | Displays labels for all tick values |
| `ShowCustomLabels` | bool | false | Displays custom labels from CustomLabels collection |
| `CustomLabels` | ObservableCollection | null | Collection of custom label items with Label and Value properties |
| `LabelPlacement` | LabelPlacement | BottomRight | Position of custom labels (TopLeft/BottomRight) |
| `ValuePlacement` | ValuePlacement | BottomRight | Position of value labels (TopLeft/BottomRight) |
| `LabelOrientation` | Orientation | Horizontal | Orientation of labels (Horizontal/Vertical) |

## ShowValueLabels Property

The `ShowValueLabels` property enables automatic label generation for all tick marks on the slider. When set to true, labels appear at each tick frequency interval.

### XAML Example

```xml
<Window x:Class="RangeSliderDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf">
    <Grid>
        <editors:SfRangeSlider
            Width="300"
            Height="50"
            Minimum="0"
            Maximum="100"
            Value="40"
            TickFrequency="20"
            TickPlacement="BottomRight"
            ShowValueLabels="True" />
    </Grid>
</Window>
```

### C# Code-Behind Example

```csharp
using Syncfusion.Windows.Controls.Input;
using System.Windows;
using System.Windows.Controls;

namespace RangeSliderDemo
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            Grid parentGrid = new Grid();
            
            SfRangeSlider rangeSlider = new SfRangeSlider()
            {
                Width = 300,
                Height = 50,
                Minimum = 0,
                Maximum = 100,
                Value = 40,
                TickFrequency = 20,
                TickPlacement = Syncfusion.Windows.Controls.Input.TickPlacement.BottomRight,
                ShowValueLabels = true
            };
            
            parentGrid.Children.Add(rangeSlider);
            this.Content = parentGrid;
        }
    }
}
```

## ShowCustomLabels Property

The `ShowCustomLabels` property enables the display of custom labels for specific values defined in the `CustomLabels` collection. This is useful for displaying meaningful text instead of numeric values.

### Basic XAML Example

```xml
<editors:SfRangeSlider
    Width="300"
    Height="50"
    Minimum="100"
    Maximum="200"
    CustomLabels="{Binding CustomCollection}"
    ShowCustomLabels="True"
    TickFrequency="20"
    TickPlacement="Outside"
    ThumbToolTipPlacement="BottomRight"/>
```

### C# Example

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Height = 50,
    Minimum = 100,
    Maximum = 200,
    ShowCustomLabels = true,
    TickFrequency = 20,
    TickPlacement = Syncfusion.Windows.Controls.Input.TickPlacement.Outside,
    ThumbToolTipPlacement = ThumbToolTipPlacement.BottomRight
};

Binding binding = new Binding("CustomCollection");
binding.Source = viewModel;
rangeSlider.SetBinding(SfRangeSlider.CustomLabelsProperty, binding);
```

## CustomLabels Collection

The `CustomLabels` property accepts an `ObservableCollection` of items containing `Label` and `Value` properties. This allows you to display custom text at specific positions on the slider.

### Creating Custom Label Models

```csharp
using System.ComponentModel;

namespace RangeSliderDemo.Models
{
    public class CustomLabelItem : INotifyPropertyChanged
    {
        private string _label;
        private double _value;

        public string label
        {
            get { return _label; }
            set
            {
                _label = value;
                OnPropertyChanged(nameof(label));
            }
        }

        public double value
        {
            get { return _value; }
            set
            {
                _value = value;
                OnPropertyChanged(nameof(value));
            }
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

### ViewModel Implementation

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

namespace RangeSliderDemo.ViewModels
{
    public class RangeSliderViewModel : INotifyPropertyChanged
    {
        private ObservableCollection<CustomLabelItem> _customCollection;

        public ObservableCollection<CustomLabelItem> CustomCollection
        {
            get { return _customCollection; }
            set
            {
                _customCollection = value;
                OnPropertyChanged(nameof(CustomCollection));
            }
        }

        public RangeSliderViewModel()
        {
            InitializeCustomLabels();
        }

        private void InitializeCustomLabels()
        {
            CustomCollection = new ObservableCollection<CustomLabelItem>
            {
                new CustomLabelItem { label = "Min", value = 100 },
                new CustomLabelItem { label = "Max", value = 200 }
            };
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

### Binding Custom Labels

#### XAML with DataContext

```xml
<Window x:Class="RangeSliderDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        xmlns:local="clr-namespace:RangeSliderDemo.ViewModels">
    <Window.DataContext>
        <local:RangeSliderViewModel/>
    </Window.DataContext>
    
    <Grid>
        <editors:SfRangeSlider
            Width="300"
            Height="50"
            Minimum="100"
            Maximum="200"
            CustomLabels="{Binding CustomCollection}"
            ShowCustomLabels="True"
            TickFrequency="20"
            TickPlacement="Outside"/>
    </Grid>
</Window>
```

## LabelPlacement Property

The `LabelPlacement` property controls where custom labels appear relative to the slider track. Available options are `TopLeft` and `BottomRight`.

### BottomRight Placement

```xml
<editors:SfRangeSlider
    Width="300"
    Height="50"
    Minimum="100"
    Maximum="200"
    CustomLabels="{Binding CustomCollection}"
    ShowCustomLabels="True"
    TickFrequency="20"
    TickPlacement="Outside"
    LabelPlacement="BottomRight"/>
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Height = 50,
    Minimum = 100,
    Maximum = 200,
    ShowCustomLabels = true,
    TickFrequency = 20,
    TickPlacement = Syncfusion.Windows.Controls.Input.TickPlacement.Outside,
    LabelPlacement = LabelPlacement.BottomRight
};
```

### TopLeft Placement

```xml
<editors:SfRangeSlider
    Width="300"
    Height="50"
    Minimum="100"
    Maximum="200"
    CustomLabels="{Binding CustomCollection}"
    ShowCustomLabels="True"
    TickFrequency="20"
    TickPlacement="Outside"
    LabelPlacement="TopLeft"/>
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Height = 50,
    Minimum = 100,
    Maximum = 200,
    ShowCustomLabels = true,
    TickFrequency = 20,
    TickPlacement = Syncfusion.Windows.Controls.Input.TickPlacement.Outside,
    LabelPlacement = LabelPlacement.TopLeft
};
```

## ValuePlacement Property

The `ValuePlacement` property determines the position of value labels (tick labels) relative to the slider. It works in conjunction with `ShowValueLabels`.

### TopLeft Value Placement

```xml
<editors:SfRangeSlider
    Width="300"
    Height="50"
    Minimum="0"
    Maximum="100"
    Value="40"
    ShowValueLabels="True"
    TickFrequency="20"
    TickPlacement="Outside"
    ValuePlacement="TopLeft"/>
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Height = 50,
    Minimum = 0,
    Maximum = 100,
    Value = 40,
    ShowValueLabels = true,
    TickFrequency = 20,
    TickPlacement = Syncfusion.Windows.Controls.Input.TickPlacement.Outside,
    ValuePlacement = ValuePlacement.TopLeft
};
```

### BottomRight Value Placement

```xml
<editors:SfRangeSlider
    Width="300"
    Height="50"
    Minimum="0"
    Maximum="100"
    Value="40"
    ShowValueLabels="True"
    TickFrequency="20"
    TickPlacement="Outside"
    ValuePlacement="BottomRight"/>
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Height = 50,
    Minimum = 0,
    Maximum = 100,
    Value = 40,
    ShowValueLabels = true,
    TickFrequency = 20,
    TickPlacement = Syncfusion.Windows.Controls.Input.TickPlacement.Outside,
    ValuePlacement = ValuePlacement.BottomRight
};
```

## LabelOrientation Property

The `LabelOrientation` property controls whether labels are displayed horizontally or vertically. This applies to both custom labels and value labels.

### Horizontal Orientation

```xml
<editors:SfRangeSlider
    Width="300"
    Height="50"
    Minimum="100"
    Maximum="200"
    Value="140"
    ShowValueLabels="True"
    TickFrequency="20"
    TickPlacement="Outside"
    ValuePlacement="BottomRight"
    LabelOrientation="Horizontal"/>
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Height = 50,
    Minimum = 100,
    Maximum = 200,
    Value = 140,
    ShowValueLabels = true,
    TickFrequency = 20,
    TickPlacement = Syncfusion.Windows.Controls.Input.TickPlacement.Outside,
    ValuePlacement = ValuePlacement.BottomRight,
    LabelOrientation = Orientation.Horizontal
};
```

### Vertical Orientation

```xml
<editors:SfRangeSlider
    Width="300"
    Height="50"
    Minimum="100"
    Maximum="200"
    Value="140"
    ShowValueLabels="True"
    TickFrequency="20"
    TickPlacement="Outside"
    ValuePlacement="BottomRight"
    LabelOrientation="Vertical"/>
```

```csharp
SfRangeSlider rangeSlider = new SfRangeSlider()
{
    Width = 300,
    Height = 50,
    Minimum = 100,
    Maximum = 200,
    Value = 140,
    ShowValueLabels = true,
    TickFrequency = 20,
    TickPlacement = Syncfusion.Windows.Controls.Input.TickPlacement.Outside,
    ValuePlacement = ValuePlacement.BottomRight,
    LabelOrientation = Orientation.Vertical
};
```

## LabelLoaded Event

The `LabelLoaded` event fires when labels are rendered, allowing dynamic customization based on label values or positions.

### Event Handler Example

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        
        SfRangeSlider rangeSlider = new SfRangeSlider()
        {
            Width = 300,
            Height = 50,
            Minimum = 0,
            Maximum = 100,
            ShowValueLabels = true,
            TickFrequency = 10
        };
        
        rangeSlider.LabelLoaded += RangeSlider_LabelLoaded;
    }
    
    private void RangeSlider_LabelLoaded(object sender, LabelLoadedEventArgs e)
    {
        // Customize label appearance based on value
        if (e.Value >= 80)
        {
            e.Label.Foreground = new SolidColorBrush(Colors.Red);
            e.Label.FontWeight = FontWeights.Bold;
        }
        else if (e.Value >= 50)
        {
            e.Label.Foreground = new SolidColorBrush(Colors.Orange);
        }
        else
        {
            e.Label.Foreground = new SolidColorBrush(Colors.Green);
        }
    }
}
```

## Complete Working Examples

### Basic Label Example

```xml
<Window x:Class="RangeSliderDemo.BasicLabelWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        Title="Basic Label Example" Height="200" Width="400">
    <StackPanel Margin="20">
        <TextBlock Text="Basic Value Labels" FontWeight="Bold" Margin="0,0,0,10"/>
        
        <editors:SfRangeSlider
            Width="300"
            Height="50"
            Minimum="0"
            Maximum="100"
            Value="50"
            TickFrequency="10"
            TickPlacement="BottomRight"
            ShowValueLabels="True"
            ValuePlacement="BottomRight"
            LabelOrientation="Horizontal"/>
    </StackPanel>
</Window>
```

### Custom Labels with MVVM

**ViewModel (TemperatureViewModel.cs):**

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using RangeSliderDemo.Models;

namespace RangeSliderDemo.ViewModels
{
    public class TemperatureViewModel : INotifyPropertyChanged
    {
        private ObservableCollection<CustomLabelItem> _temperatureLabels;
        private double _rangeStart;
        private double _rangeEnd;

        public ObservableCollection<CustomLabelItem> TemperatureLabels
        {
            get { return _temperatureLabels; }
            set
            {
                _temperatureLabels = value;
                OnPropertyChanged(nameof(TemperatureLabels));
            }
        }

        public double RangeStart
        {
            get { return _rangeStart; }
            set
            {
                _rangeStart = value;
                OnPropertyChanged(nameof(RangeStart));
            }
        }

        public double RangeEnd
        {
            get { return _rangeEnd; }
            set
            {
                _rangeEnd = value;
                OnPropertyChanged(nameof(RangeEnd));
            }
        }

        public TemperatureViewModel()
        {
            InitializeTemperatureLabels();
            RangeStart = 20;
            RangeEnd = 30;
        }

        private void InitializeTemperatureLabels()
        {
            TemperatureLabels = new ObservableCollection<CustomLabelItem>
            {
                new CustomLabelItem { label = "Freezing", value = 0 },
                new CustomLabelItem { label = "Cold", value = 10 },
                new CustomLabelItem { label = "Cool", value = 20 },
                new CustomLabelItem { label = "Warm", value = 30 },
                new CustomLabelItem { label = "Hot", value = 40 }
            };
        }

        public event PropertyChangedEventHandler PropertyChanged;

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

**XAML View:**

```xml
<Window x:Class="RangeSliderDemo.TemperatureWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:editors="clr-namespace:Syncfusion.Windows.Controls.Input;assembly=Syncfusion.SfInput.Wpf"
        xmlns:local="clr-namespace:RangeSliderDemo.ViewModels"
        Title="Temperature Range Selector" Height="250" Width="450">
    <Window.DataContext>
        <local:TemperatureViewModel/>
    </Window.DataContext>
    
    <StackPanel Margin="20">
        <TextBlock Text="Temperature Range Selector" 
                   FontSize="16" 
                   FontWeight="Bold" 
                   Margin="0,0,0,20"/>
        
        <editors:SfRangeSlider
            Width="350"
            Height="60"
            Minimum="0"
            Maximum="40"
            RangeStart="{Binding RangeStart, Mode=TwoWay}"
            RangeEnd="{Binding RangeEnd, Mode=TwoWay}"
            CustomLabels="{Binding TemperatureLabels}"
            ShowCustomLabels="True"
            ShowValueLabels="True"
            TickFrequency="10"
            TickPlacement="BottomRight"
            LabelPlacement="BottomRight"
            ValuePlacement="TopLeft"
            LabelOrientation="Horizontal"/>
        
        <StackPanel Orientation="Horizontal" Margin="0,20,0,0">
            <TextBlock Text="Selected Range: "/>
            <TextBlock Text="{Binding RangeStart, StringFormat='{}{0:F1}°C'}" FontWeight="Bold"/>
            <TextBlock Text=" - " Margin="5,0"/>
            <TextBlock Text="{Binding RangeEnd, StringFormat='{}{0:F1}°C'}" FontWeight="Bold"/>
        </StackPanel>
    </StackPanel>
</Window>
```

### Advanced Label Customization

```csharp
using System.Collections.ObjectModel;
using System.Windows;
using System.Windows.Media;
using Syncfusion.Windows.Controls.Input;
using RangeSliderDemo.Models;

namespace RangeSliderDemo
{
    public partial class AdvancedLabelWindow : Window
    {
        public ObservableCollection<CustomLabelItem> PriorityLabels { get; set; }

        public AdvancedLabelWindow()
        {
            InitializeComponent();
            InitializePriorityLabels();
            DataContext = this;
        }

        private void InitializePriorityLabels()
        {
            PriorityLabels = new ObservableCollection<CustomLabelItem>
            {
                new CustomLabelItem { label = "Low", value = 0 },
                new CustomLabelItem { label = "Medium", value = 50 },
                new CustomLabelItem { label = "High", value = 100 }
            };
        }

        private void PrioritySlider_LabelLoaded(object sender, LabelLoadedEventArgs e)
        {
            if (e.Value == 0)
            {
                e.Label.Foreground = new SolidColorBrush(Colors.Green);
            }
            else if (e.Value == 50)
            {
                e.Label.Foreground = new SolidColorBrush(Colors.Orange);
            }
            else if (e.Value == 100)
            {
                e.Label.Foreground = new SolidColorBrush(Colors.Red);
            }
            
            e.Label.FontSize = 12;
            e.Label.FontWeight = FontWeights.SemiBold;
        }
    }
}
```

## Best Practices

1. **Use MVVM Pattern**: Implement ViewModels for better separation of concerns and testability.

2. **ObservableCollection**: Always use `ObservableCollection` for CustomLabels to support dynamic updates.

3. **Property Naming**: Use lowercase property names (`label`, `value`) in custom label items to match the control's expectations.

4. **Performance**: Limit the number of custom labels to avoid UI clutter and performance issues.

5. **Consistent Placement**: Use consistent `LabelPlacement` and `ValuePlacement` values to avoid overlapping labels.

6. **Tick Coordination**: Set appropriate `TickFrequency` values that align with your label requirements.

7. **Binding Context**: Always set the `DataContext` properly when using data binding.

8. **Event Handlers**: Use `LabelLoaded` event sparingly for dynamic customization to avoid performance impact.

9. **Orientation**: Match `LabelOrientation` with your slider's orientation for better readability.

10. **Null Checks**: Always validate CustomLabels collection before binding.

## Common Scenarios

### Scenario 1: Price Range Selector

```csharp
public class PriceViewModel : INotifyPropertyChanged
{
    public ObservableCollection<CustomLabelItem> PriceLabels { get; set; }

    public PriceViewModel()
    {
        PriceLabels = new ObservableCollection<CustomLabelItem>
        {
            new CustomLabelItem { label = "$0", value = 0 },
            new CustomLabelItem { label = "$250", value = 250 },
            new CustomLabelItem { label = "$500", value = 500 },
            new CustomLabelItem { label = "$750", value = 750 },
            new CustomLabelItem { label = "$1000+", value = 1000 }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Scenario 2: Time Range Selector

```csharp
public class TimeViewModel : INotifyPropertyChanged
{
    public ObservableCollection<CustomLabelItem> TimeLabels { get; set; }

    public TimeViewModel()
    {
        TimeLabels = new ObservableCollection<CustomLabelItem>
        {
            new CustomLabelItem { label = "6 AM", value = 6 },
            new CustomLabelItem { label = "12 PM", value = 12 },
            new CustomLabelItem { label = "6 PM", value = 18 },
            new CustomLabelItem { label = "12 AM", value = 24 }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Scenario 3: Volume Control

```csharp
public class VolumeViewModel : INotifyPropertyChanged
{
    public ObservableCollection<CustomLabelItem> VolumeLabels { get; set; }

    public VolumeViewModel()
    {
        VolumeLabels = new ObservableCollection<CustomLabelItem>
        {
            new CustomLabelItem { label = "Mute", value = 0 },
            new CustomLabelItem { label = "Low", value = 25 },
            new CustomLabelItem { label = "Medium", value = 50 },
            new CustomLabelItem { label = "High", value = 75 },
            new CustomLabelItem { label = "Max", value = 100 }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Scenario 4: Rating Scale

```csharp
public class RatingViewModel : INotifyPropertyChanged
{
    public ObservableCollection<CustomLabelItem> RatingLabels { get; set; }

    public RatingViewModel()
    {
        RatingLabels = new ObservableCollection<CustomLabelItem>
        {
            new CustomLabelItem { label = "Poor", value = 1 },
            new CustomLabelItem { label = "Fair", value = 2 },
            new CustomLabelItem { label = "Good", value = 3 },
            new CustomLabelItem { label = "Very Good", value = 4 },
            new CustomLabelItem { label = "Excellent", value = 5 }
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

---

**Note**: Ensure all necessary namespaces are imported and the Syncfusion.SfInput.Wpf assembly is referenced in your project.

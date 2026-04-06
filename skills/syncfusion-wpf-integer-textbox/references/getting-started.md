# Getting Started with IntegerTextBox

## Installation

**NuGet:** `Install-Package Syncfusion.Shared.WPF`  
**Assembly:** `Syncfusion.Shared.WPF`  
**Namespace:** `Syncfusion.Windows.Shared`

## Add to XAML

```xml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf" ...>
    <syncfusion:IntegerTextBox x:Name="integerTextBox" Value="100" MinValue="0" MaxValue="1000" Width="150" Height="30"/>
</Window>
```

## Add via C#

```csharp
using Syncfusion.Windows.Shared;

IntegerTextBox integerTextBox = new IntegerTextBox();
integerTextBox.Value = 100;
integerTextBox.MinValue = 0;
integerTextBox.MaxValue = 1000;
this.Content = integerTextBox;
```

> **Important:** Always use `Value`, never `Text`, to get/set the integer value.

## Data Binding (MVVM)

```xml
<Window.DataContext><local:ViewModel/></Window.DataContext>
<syncfusion:IntegerTextBox Value="{Binding MyValue, UpdateSourceTrigger=PropertyChanged}" Width="150" Height="30"/>
```

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private long myValue;
    public long MyValue
    {
        get => myValue;
        set { if (myValue != value) { myValue = value; OnPropertyChanged(); } }
    }
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged([CallerMemberName] string p = null)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(p));
}
```

## Basic Formatting

```xml
<syncfusion:IntegerTextBox Value="123456789" GroupSeperatorEnabled="True" Width="150" Height="30"/>
```

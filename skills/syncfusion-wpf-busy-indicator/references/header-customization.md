# Header Customization

The SfBusyIndicator supports header customization through two properties: `Header` for simple text content and `HeaderTemplate` for advanced template-based customization. Headers provide context to users about what operation is in progress.

## Header Property

The `Header` property allows you to display simple text or content below the animation. It accepts any object, but is typically used for strings.

### Basic Header Usage

```xml
<!-- Simple text header -->
<syncfusion:SfBusyIndicator IsBusy="True"
                             Header="Loading..."
                             Foreground="White"
                             Background="CornflowerBlue"/>

<!-- Dynamic header with binding -->
<syncfusion:SfBusyIndicator IsBusy="{Binding IsProcessing}"
                             Header="{Binding CurrentOperation}"/>
```

### Setting Header in Code

```csharp
// C# code-behind
SfBusyIndicator busyIndicator = new SfBusyIndicator();
busyIndicator.IsBusy = true;
busyIndicator.Header = "Loading data...";
busyIndicator.Foreground = Brushes.White;
```

```vb
' VB.NET
Dim busyIndicator As New SfBusyIndicator()
busyIndicator.IsBusy = True
busyIndicator.Header = "Loading data..."
busyIndicator.Foreground = Brushes.White
```

## Styling Headers

Apply styling properties to customize header appearance:

```xml
<syncfusion:SfBusyIndicator IsBusy="True"
                             Header="Processing your request..."
                             Foreground="DarkBlue"
                             FontSize="16"
                             FontWeight="Bold"
                             Background="LightGray"/>
```

### Dynamic Header Text

Update header based on operation state:

```csharp
public class OperationViewModel : ViewModelBase
{
    private bool _isBusy;
    public bool IsBusy
    {
        get => _isBusy;
        set => SetProperty(ref _isBusy, value);
    }
    
    private string _statusMessage = "Ready";
    public string StatusMessage
    {
        get => _statusMessage;
        set => SetProperty(ref _statusMessage, value);
    }
    
    public async Task PerformOperationAsync()
    {
        IsBusy = true;
        
        StatusMessage = "Initializing...";
        await Task.Delay(1000);
        
        StatusMessage = "Loading data...";
        await Task.Delay(2000);
        
        StatusMessage = "Processing...";
        await Task.Delay(1500);
        
        StatusMessage = "Finalizing...";
        await Task.Delay(500);
        
        IsBusy = false;
        StatusMessage = "Complete!";
    }
}
```

```xml
<syncfusion:SfBusyIndicator IsBusy="{Binding IsBusy}"
                             Header="{Binding StatusMessage}"/>
```

## HeaderTemplate Property

For advanced customization, use `HeaderTemplate` to define a custom `DataTemplate` for the header content.

### Basic HeaderTemplate

```xml
<syncfusion:SfBusyIndicator IsBusy="True" 
                             AnimationType="DoubleCircle"
                             Foreground="White" 
                             Background="CornflowerBlue">
    <syncfusion:SfBusyIndicator.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="Loading..." 
                      TextAlignment="Center"
                      FontSize="15" 
                      FontWeight="Bold"
                      Foreground="Yellow"/>
        </DataTemplate>
    </syncfusion:SfBusyIndicator.HeaderTemplate>
</syncfusion:SfBusyIndicator>
```

### Multi-Line Header Template

Create complex, multi-line headers:

```xml
<syncfusion:SfBusyIndicator IsBusy="True">
    <syncfusion:SfBusyIndicator.HeaderTemplate>
        <DataTemplate>
            <StackPanel>
                <TextBlock Text="Please Wait" 
                          FontSize="18" 
                          FontWeight="Bold"
                          HorizontalAlignment="Center"
                          Margin="0,0,0,5"/>
                <TextBlock Text="We're processing your request..." 
                          FontSize="12"
                          FontStyle="Italic"
                          HorizontalAlignment="Center"
                          Opacity="0.8"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfBusyIndicator.HeaderTemplate>
</syncfusion:SfBusyIndicator>
```

### Header with Icon

Combine text with icons or images:

```xml
<syncfusion:SfBusyIndicator IsBusy="True" Background="#2196F3">
    <syncfusion:SfBusyIndicator.HeaderTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal" 
                       HorizontalAlignment="Center">
                <Path Data="M12,2A10,10 0 0,0 2,12A10,10 0 0,0 12,22A10,10 0 0,0 22,12A10,10 0 0,0 12,2Z"
                      Fill="White" 
                      Width="16" 
                      Height="16"
                      Margin="0,0,8,0"
                      Stretch="Uniform"/>
                <TextBlock Text="Loading Data" 
                          Foreground="White"
                          FontSize="14"
                          VerticalAlignment="Center"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfBusyIndicator.HeaderTemplate>
</syncfusion:SfBusyIndicator>
```

### Data-Bound Header Template

Bind template to view model data:

```xml
<syncfusion:SfBusyIndicator IsBusy="{Binding IsBusy}"
                             DataContext="{Binding}">
    <syncfusion:SfBusyIndicator.HeaderTemplate>
        <DataTemplate>
            <StackPanel>
                <TextBlock Text="{Binding CurrentOperation}" 
                          FontSize="16" 
                          FontWeight="Bold"
                          HorizontalAlignment="Center"/>
                <TextBlock Text="{Binding OperationDetail}" 
                          FontSize="12"
                          HorizontalAlignment="Center"
                          Margin="0,5,0,0"/>
                <ProgressBar Value="{Binding ProgressPercentage}"
                            Width="200"
                            Height="6"
                            Margin="0,10,0,0"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfBusyIndicator.HeaderTemplate>
</syncfusion:SfBusyIndicator>
```

```csharp
public class DetailedOperationViewModel : ViewModelBase
{
    private bool _isBusy;
    public bool IsBusy
    {
        get => _isBusy;
        set => SetProperty(ref _isBusy, value);
    }
    
    private string _currentOperation = "Idle";
    public string CurrentOperation
    {
        get => _currentOperation;
        set => SetProperty(ref _currentOperation, value);
    }
    
    private string _operationDetail = "";
    public string OperationDetail
    {
        get => _operationDetail;
        set => SetProperty(ref _operationDetail, value);
    }
    
    private int _progressPercentage;
    public int ProgressPercentage
    {
        get => _progressPercentage;
        set => SetProperty(ref _progressPercentage, value);
    }
    
    public async Task ProcessFilesAsync(string[] files)
    {
        IsBusy = true;
        CurrentOperation = "Processing Files";
        
        for (int i = 0; i < files.Length; i++)
        {
            OperationDetail = $"Processing {Path.GetFileName(files[i])}...";
            ProgressPercentage = (int)((i + 1) / (double)files.Length * 100);
            
            await ProcessFileAsync(files[i]);
        }
        
        IsBusy = false;
        CurrentOperation = "Complete";
    }
}
```

### Styled Header Template

Apply consistent styling through resources:

```xml
<Window.Resources>
    <Style x:Key="HeaderTextStyle" TargetType="TextBlock">
        <Setter Property="FontFamily" Value="Segoe UI"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="HorizontalAlignment" Value="Center"/>
    </Style>
</Window.Resources>

<syncfusion:SfBusyIndicator IsBusy="True" Background="#1976D2">
    <syncfusion:SfBusyIndicator.HeaderTemplate>
        <DataTemplate>
            <StackPanel>
                <TextBlock Text="Processing" 
                          Style="{StaticResource HeaderTextStyle}"
                          FontSize="18" 
                          FontWeight="Bold"/>
                <TextBlock Text="This may take a few moments" 
                          Style="{StaticResource HeaderTextStyle}"
                          FontSize="12"
                          Margin="0,5,0,0"/>
            </StackPanel>
        </DataTemplate>
    </syncfusion:SfBusyIndicator.HeaderTemplate>
</syncfusion:SfBusyIndicator>
```

## Conditional Header Display

Show different headers based on state:

```xml
<syncfusion:SfBusyIndicator IsBusy="{Binding IsBusy}">
    <syncfusion:SfBusyIndicator.HeaderTemplate>
        <DataTemplate>
            <TextBlock>
                <TextBlock.Style>
                    <Style TargetType="TextBlock">
                        <Style.Triggers>
                            <DataTrigger Binding="{Binding OperationType}" Value="Loading">
                                <Setter Property="Text" Value="Loading data..."/>
                                <Setter Property="Foreground" Value="Blue"/>
                            </DataTrigger>
                            <DataTrigger Binding="{Binding OperationType}" Value="Saving">
                                <Setter Property="Text" Value="Saving changes..."/>
                                <Setter Property="Foreground" Value="Green"/>
                            </DataTrigger>
                            <DataTrigger Binding="{Binding OperationType}" Value="Deleting">
                                <Setter Property="Text" Value="Deleting items..."/>
                                <Setter Property="Foreground" Value="Red"/>
                            </DataTrigger>
                        </Style.Triggers>
                    </Style>
                </TextBlock.Style>
            </TextBlock>
        </DataTemplate>
    </syncfusion:SfBusyIndicator.HeaderTemplate>
</syncfusion:SfBusyIndicator>
```

## Animated Header Content

Create animated headers for enhanced visual feedback:

```xml
<syncfusion:SfBusyIndicator IsBusy="True">
    <syncfusion:SfBusyIndicator.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="Loading" FontSize="16">
                <TextBlock.Triggers>
                    <EventTrigger RoutedEvent="Loaded">
                        <BeginStoryboard>
                            <Storyboard RepeatBehavior="Forever">
                                <DoubleAnimation Storyboard.TargetProperty="Opacity"
                                               From="1.0" To="0.3" 
                                               Duration="0:0:0.8"
                                               AutoReverse="True"/>
                            </Storyboard>
                        </BeginStoryboard>
                    </EventTrigger>
                </TextBlock.Triggers>
            </TextBlock>
        </DataTemplate>
    </syncfusion:SfBusyIndicator.HeaderTemplate>
</syncfusion:SfBusyIndicator>
```

## Localization Support

Support multiple languages through resource bindings:

```xml
<syncfusion:SfBusyIndicator IsBusy="True"
                             Header="{x:Static properties:Resources.LoadingMessage}"/>
```

Or with HeaderTemplate:

```xml
<syncfusion:SfBusyIndicator IsBusy="True">
    <syncfusion:SfBusyIndicator.HeaderTemplate>
        <DataTemplate>
            <TextBlock Text="{x:Static properties:Resources.PleaseWaitMessage}"/>
        </DataTemplate>
    </syncfusion:SfBusyIndicator.HeaderTemplate>
</syncfusion:SfBusyIndicator>
```

## Best Practices

1. **Be descriptive** - Use headers that clearly communicate what's happening
2. **Keep it concise** - Avoid lengthy header text that may be truncated
3. **Match tone** - Use language consistent with your application's style
4. **Update dynamically** - Change header text as operations progress through stages
5. **Consider accessibility** - Ensure sufficient contrast between header and background
6. **Use templates for complexity** - Simple text for basic needs, templates for rich content
7. **Test readability** - Verify headers are readable at different sizes and resolutions
8. **Provide context** - Help users understand why they're waiting and what to expect

## Common Patterns

### Pattern 1: Time-Sensitive Messages

```csharp
public async Task LongOperationAsync()
{
    IsBusy = true;
    StatusMessage = "Starting process...";
    
    await Task.Delay(5000);
    StatusMessage = "Still working... This may take a while.";
    
    await Task.Delay(10000);
    StatusMessage = "Almost done... Thank you for your patience.";
    
    await Task.Delay(3000);
    IsBusy = false;
}
```

### Pattern 2: Operation Queue

```csharp
public class QueueViewModel : ViewModelBase
{
    private Queue<string> _operationQueue = new Queue<string>();
    
    public string CurrentHeader => _operationQueue.Count > 0 
        ? $"Processing: {_operationQueue.Peek()} ({_operationQueue.Count} remaining)"
        : "Idle";
    
    public async Task ProcessQueueAsync()
    {
        IsBusy = true;
        
        while (_operationQueue.Count > 0)
        {
            OnPropertyChanged(nameof(CurrentHeader));
            var operation = _operationQueue.Dequeue();
            await ProcessOperationAsync(operation);
        }
        
        IsBusy = false;
    }
}
```

### Pattern 3: File Operation Header

```csharp
public async Task UploadFilesAsync(IEnumerable<string> files)
{
    IsBusy = true;
    var fileList = files.ToList();
    
    for (int i = 0; i < fileList.Count; i++)
    {
        var fileName = Path.GetFileName(fileList[i]);
        StatusMessage = $"Uploading {fileName} ({i + 1}/{fileList.Count})";
        
        await UploadFileAsync(fileList[i]);
    }
    
    IsBusy = false;
    StatusMessage = "All files uploaded successfully!";
}
```

## Troubleshooting

**Issue:** Header text is cut off
- **Solution:** Increase container width or use HeaderTemplate with wrapping:
  ```xml
  <TextBlock Text="{Binding LongMessage}" TextWrapping="Wrap" MaxWidth="300"/>
  ```

**Issue:** Header not visible against background
- **Solution:** Set Foreground property or use contrasting colors

**Issue:** Header template not updating with data changes
- **Solution:** Ensure view model implements INotifyPropertyChanged correctly

**Issue:** Header appears above animation instead of below
- **Solution:** This is the default behavior; use custom control templates to reposition

## GitHub Samples

Explore working examples:
https://github.com/SyncfusionExamples/wpf-BusyIndicator-examples/tree/master/Samples/Header

## Related Topics

- **IsBusy Property** - Control when headers are displayed
- **Animation Types** - Choose animations that complement your headers
- **Sizing** - Adjust indicator size to accommodate header content

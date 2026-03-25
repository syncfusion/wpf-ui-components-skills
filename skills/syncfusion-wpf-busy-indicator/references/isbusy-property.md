# IsBusy Property - Display Control

The `IsBusy` property is the primary control for showing and hiding the busy indicator animation. This guide covers how to use IsBusy effectively in various scenarios including MVVM patterns, command integration, and async operations.

## Property Overview

The **IsBusy** property is a boolean that determines whether the animation is displayed:
- **True** - Animation is visible and playing
- **False** - Animation is hidden

```csharp
public bool IsBusy { get; set; }
```

## Basic Usage

### XAML Declaration

```xml
<!-- Always visible -->
<syncfusion:SfBusyIndicator IsBusy="True" 
                             AnimationType="DoubleCircle"/>

<!-- Initially hidden -->
<syncfusion:SfBusyIndicator IsBusy="False"
                             AnimationType="Spin"/>
```

### Code-Behind Control

```csharp
// Show busy indicator
busyIndicator.IsBusy = true;

// Hide busy indicator
busyIndicator.IsBusy = false;

// Toggle busy state
busyIndicator.IsBusy = !busyIndicator.IsBusy;
```

## MVVM Pattern Integration

The IsBusy property is designed for data binding in MVVM applications.

### Basic ViewModel Binding

```xml
<syncfusion:SfBusyIndicator IsBusy="{Binding IsLoading}"
                             AnimationType="Flight"
                             Header="{Binding StatusMessage}"/>
```

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

public class MainViewModel : INotifyPropertyChanged
{
    private bool _isLoading;
    public bool IsLoading
    {
        get => _isLoading;
        set
        {
            if (_isLoading != value)
            {
                _isLoading = value;
                OnPropertyChanged();
            }
        }
    }
    
    private string _statusMessage = "Loading...";
    public string StatusMessage
    {
        get => _statusMessage;
        set
        {
            if (_statusMessage != value)
            {
                _statusMessage = value;
                OnPropertyChanged();
            }
        }
    }
    
    public event PropertyChanged EventHandler PropertyChanged;
    
    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Async Operation Pattern

Control IsBusy during asynchronous operations:

```csharp
public class DataViewModel : ViewModelBase
{
    private bool _isBusy;
    public bool IsBusy
    {
        get => _isBusy;
        set => SetProperty(ref _isBusy, value);
    }
    
    public async Task LoadDataAsync()
    {
        IsBusy = true;
        
        try
        {
            // Simulate data loading
            await Task.Delay(2000);
            var data = await _dataService.FetchDataAsync();
            ProcessData(data);
        }
        catch (Exception ex)
        {
            // Handle error
            ShowError(ex.Message);
        }
        finally
        {
            // ALWAYS reset IsBusy in finally block
            IsBusy = false;
        }
    }
}
```

### Multiple Operation States

Track different operations with separate IsBusy flags:

```csharp
public class MultiOperationViewModel : ViewModelBase
{
    private bool _isLoadingData;
    public bool IsLoadingData
    {
        get => _isLoadingData;
        set => SetProperty(ref _isLoadingData, value);
    }
    
    private bool _isSavingData;
    public bool IsSavingData
    {
        get => _isSavingData;
        set => SetProperty(ref _isSavingData, value);
    }
    
    // Computed property for overall busy state
    public bool IsAnyOperationBusy => IsLoadingData || IsSavingData;
    
    public async Task LoadDataAsync()
    {
        IsLoadingData = true;
        try
        {
            await _service.LoadAsync();
        }
        finally
        {
            IsLoadingData = false;
            OnPropertyChanged(nameof(IsAnyOperationBusy));
        }
    }
    
    public async Task SaveDataAsync()
    {
        IsSavingData = true;
        try
        {
            await _service.SaveAsync();
        }
        finally
        {
            IsSavingData = false;
            OnPropertyChanged(nameof(IsAnyOperationBusy));
        }
    }
}
```

```xml
<!-- Show busy indicator if any operation is running -->
<syncfusion:SfBusyIndicator IsBusy="{Binding IsAnyOperationBusy}"/>
```

## Command Integration

### ICommand with Busy State

Integrate IsBusy with command execution:

```csharp
public class CommandViewModel : ViewModelBase
{
    private bool _isBusy;
    public bool IsBusy
    {
        get => _isBusy;
        set => SetProperty(ref _isBusy, value);
    }
    
    public ICommand LoadDataCommand { get; }
    
    public CommandViewModel()
    {
        LoadDataCommand = new RelayCommand(async () => await LoadDataAsync());
    }
    
    private async Task LoadDataAsync()
    {
        IsBusy = true;
        try
        {
            await Task.Delay(2000);
            // Load data logic
        }
        finally
        {
            IsBusy = false;
        }
    }
}

// RelayCommand implementation (simplified)
public class RelayCommand : ICommand
{
    private readonly Func<Task> _execute;
    
    public RelayCommand(Func<Task> execute)
    {
        _execute = execute;
    }
    
    public bool CanExecute(object parameter) => true;
    
    public async void Execute(object parameter)
    {
        await _execute();
    }
    
    public event EventHandler CanExecuteChanged;
}
```

### Disable UI During Busy State

Prevent user interaction while operations are running:

```xml
<Grid>
    <StackPanel IsEnabled="{Binding IsBusy, Converter={StaticResource InverseBoolConverter}}">
        <Button Content="Load Data" Command="{Binding LoadDataCommand}"/>
        <Button Content="Save Data" Command="{Binding SaveDataCommand}"/>
    </StackPanel>
    
    <syncfusion:SfBusyIndicator IsBusy="{Binding IsBusy}"
                                 AnimationType="DoubleCircle"
                                 Header="Processing..."/>
</Grid>
```

```csharp
// Inverse Boolean Converter
public class InverseBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return value is bool b ? !b : true;
    }
    
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return value is bool b ? !b : false;
    }
}
```

## Common Scenarios

### Scenario 1: API Data Fetching

```csharp
public async Task FetchUsersAsync()
{
    IsBusy = true;
    StatusMessage = "Fetching users...";
    
    try
    {
        var users = await _apiClient.GetUsersAsync();
        Users = new ObservableCollection<User>(users);
        StatusMessage = $"Loaded {users.Count} users";
    }
    catch (HttpRequestException ex)
    {
        StatusMessage = "Failed to load users";
        // Show error notification
    }
    finally
    {
        IsBusy = false;
    }
}
```

### Scenario 2: File Upload

```csharp
public async Task UploadFileAsync(string filePath)
{
    IsBusy = true;
    StatusMessage = "Uploading file...";
    
    try
    {
        using (var fileStream = File.OpenRead(filePath))
        {
            await _uploadService.UploadAsync(fileStream);
        }
        StatusMessage = "Upload complete!";
        await Task.Delay(1000); // Show success message briefly
    }
    catch (Exception ex)
    {
        StatusMessage = $"Upload failed: {ex.Message}";
        await Task.Delay(2000); // Show error longer
    }
    finally
    {
        IsBusy = false;
    }
}
```

### Scenario 3: Database Query

```csharp
public async Task SearchDatabaseAsync(string searchTerm)
{
    if (string.IsNullOrWhiteSpace(searchTerm))
        return;
    
    IsBusy = true;
    StatusMessage = $"Searching for '{searchTerm}'...";
    
    try
    {
        var results = await _database.SearchAsync(searchTerm);
        SearchResults = new ObservableCollection<SearchResult>(results);
        StatusMessage = $"Found {results.Count} results";
    }
    finally
    {
        IsBusy = false;
    }
}
```

### Scenario 4: Page Navigation

```csharp
public async Task NavigateToPageAsync(string pageName)
{
    IsBusy = true;
    StatusMessage = "Loading page...";
    
    try
    {
        // Load page data
        var pageData = await _pageService.LoadPageDataAsync(pageName);
        
        // Navigate
        _navigationService.NavigateTo(pageName, pageData);
    }
    finally
    {
        IsBusy = false;
    }
}
```

## Advanced Patterns

### Debounced Busy Indicator

Prevent flickering for very quick operations:

```csharp
public class DebouncedBusyViewModel : ViewModelBase
{
    private bool _actualBusyState;
    private bool _displayedBusyState;
    private CancellationTokenSource _debounceTokenSource;
    
    public bool IsBusy
    {
        get => _displayedBusyState;
        private set => SetProperty(ref _displayedBusyState, value);
    }
    
    private async void SetBusyState(bool isBusy)
    {
        _actualBusyState = isBusy;
        
        // Cancel previous debounce
        _debounceTokenSource?.Cancel();
        _debounceTokenSource = new CancellationTokenSource();
        
        try
        {
            if (isBusy)
            {
                // Delay showing busy indicator by 300ms
                await Task.Delay(300, _debounceTokenSource.Token);
                if (_actualBusyState) // Check if still busy
                {
                    IsBusy = true;
                }
            }
            else
            {
                // Hide immediately
                IsBusy = false;
            }
        }
        catch (TaskCanceledException)
        {
            // Operation completed before debounce delay
        }
    }
    
    public async Task QuickOperationAsync()
    {
        SetBusyState(true);
        try
        {
            await Task.Delay(100); // Quick operation
        }
        finally
        {
            SetBusyState(false);
        }
    }
}
```

### Progress-Based Busy State

Combine with progress tracking:

```csharp
public class ProgressViewModel : ViewModelBase
{
    private bool _isBusy;
    public bool IsBusy
    {
        get => _isBusy;
        set => SetProperty(ref _isBusy, value);
    }
    
    private int _progressPercentage;
    public int ProgressPercentage
    {
        get => _progressPercentage;
        set => SetProperty(ref _progressPercentage, value);
    }
    
    public string ProgressMessage => $"Progress: {ProgressPercentage}%";
    
    public async Task ProcessWithProgressAsync()
    {
        IsBusy = true;
        ProgressPercentage = 0;
        
        try
        {
            for (int i = 0; i <= 100; i += 10)
            {
                await Task.Delay(500);
                ProgressPercentage = i;
                OnPropertyChanged(nameof(ProgressMessage));
            }
        }
        finally
        {
            IsBusy = false;
            ProgressPercentage = 0;
        }
    }
}
```

## Best Practices

1. **Always use try-finally blocks** - Ensure IsBusy is reset even if exceptions occur
2. **Use MVVM binding** - Don't manipulate IsBusy from code-behind in MVVM apps
3. **Provide status messages** - Update header/status text to inform users what's happening
4. **Disable UI during operations** - Prevent users from triggering multiple operations
5. **Handle cancellation** - Implement cancellation tokens for long operations
6. **Avoid flickering** - Use debouncing for operations that might complete very quickly
7. **Test error scenarios** - Ensure IsBusy resets properly when exceptions occur

## Common Pitfalls

**Pitfall:** Forgetting to reset IsBusy
```csharp
// ❌ BAD - IsBusy stuck if exception occurs
public async Task LoadDataAsync()
{
    IsBusy = true;
    await _service.LoadAsync(); // May throw
    IsBusy = false; // Never reached if exception thrown
}

// ✅ GOOD - IsBusy always reset
public async Task LoadDataAsync()
{
    IsBusy = true;
    try
    {
        await _service.LoadAsync();
    }
    finally
    {
        IsBusy = false;
    }
}
```

**Pitfall:** Not raising PropertyChanged
```csharp
// ❌ BAD - UI won't update
private bool _isBusy;
public bool IsBusy
{
    get => _isBusy;
    set => _isBusy = value; // Missing notification
}

// ✅ GOOD - UI updates properly
public bool IsBusy
{
    get => _isBusy;
    set
    {
        _isBusy = value;
        OnPropertyChanged(); // or OnPropertyChanged(nameof(IsBusy))
    }
}
```

**Pitfall:** Multiple concurrent operations
```csharp
// ❌ BAD - Operations interfere with each other
public async Task Operation1() { IsBusy = true; await Work1(); IsBusy = false; }
public async Task Operation2() { IsBusy = true; await Work2(); IsBusy = false; }
// If both run, IsBusy might be set to false while one is still running

// ✅ GOOD - Track operations separately or use counter
private int _busyCount = 0;
public bool IsBusy => _busyCount > 0;

private void IncrementBusy()
{
    _busyCount++;
    OnPropertyChanged(nameof(IsBusy));
}

private void DecrementBusy()
{
    if (_busyCount > 0)
        _busyCount--;
    OnPropertyChanged(nameof(IsBusy));
}
```

## GitHub Samples

Explore working examples:
https://github.com/SyncfusionExamples/wpf-BusyIndicator-examples/tree/master/Samples/IsBusy

## Related Topics

- **Animation Types** - Choose appropriate animations for different operations
- **Header Customization** - Display operation status in headers
- **Methods** - Advanced control through OnPropertyChanged override

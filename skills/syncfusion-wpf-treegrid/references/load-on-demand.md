# Load-On-Demand for WPF TreeGrid

## Table of Contents
- [Overview](#overview)
- [Implementation Approaches](#implementation-approaches)
  - [Using RequestTreeItems Event](#using-requesttreeitems-event)
  - [Using LoadOnDemandCommand](#using-loadondemandcommand)
- [Event-Based Loading](#event-based-loading)
  - [Basic Event Handler](#basic-event-handler)
  - [Async Loading](#async-loading)
  - [Error Handling](#error-handling)
- [Command-Based Loading](#command-based-loading)
  - [Basic Command Implementation](#basic-command-implementation)
  - [Async Command](#async-command)
  - [MVVM Pattern](#mvvm-pattern)
- [Advanced Scenarios](#advanced-scenarios)
  - [Caching Strategies](#caching-strategies)
  - [Paging Support](#paging-support)
  - [Remote Data Loading](#remote-data-loading)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Load-On-Demand is a technique that loads child nodes only when a parent node is expanded. This approach significantly improves performance when working with large hierarchical datasets by loading data as needed rather than all at once.

**Key Benefits:**
- Reduced initial load time
- Lower memory footprint
- Better performance with large datasets
- Supports lazy loading from remote sources

## Implementation Approaches

### Using RequestTreeItems Event

Suitable for code-behind scenarios where you handle the event directly:

```xaml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding EmployeeDetails}"
                       RequestTreeItems="TreeGrid_RequestTreeItems"
                       AutoExpandMode="RootNodesExpanded" />
```

### Using LoadOnDemandCommand

Ideal for MVVM pattern where business logic resides in ViewModel:

```xaml
<syncfusion:SfTreeGrid Name="treeGrid"
                       ItemsSource="{Binding EmployeeDetails}"
                       LoadOnDemandCommand="{Binding LoadOnDemandCommand}"
                       AutoExpandMode="RootNodesExpanded" />
```

## Event-Based Loading

### Basic Event Handler

```csharp
using Syncfusion.UI.Xaml.TreeGrid;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        this.treeGrid.RequestTreeItems += TreeGrid_RequestTreeItems;
    }

    private void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
    {
        if (e.ParentItem == null)
        {
            // Load root level items
            e.ChildItems = GetRootEmployees();
        }
        else
        {
            // Load child items for the parent
            var employee = e.ParentItem as EmployeeInfo;
            if (employee != null)
            {
                e.ChildItems = GetSubordinates(employee.ID);
            }
        }
    }

    private IEnumerable GetRootEmployees()
    {
        // Return employees with no manager (root level)
        return EmployeeRepository.GetEmployees()
            .Where(x => x.ReportsTo == -1);
    }

    private IEnumerable GetSubordinates(int managerId)
    {
        // Return employees reporting to the specified manager
        return EmployeeRepository.GetEmployees()
            .Where(x => x.ReportsTo == managerId);
    }
}
```

### Async Loading

Load data asynchronously to prevent UI blocking:

```csharp
private async void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
{
    try
    {
        if (e.ParentItem == null)
        {
            // Load root items asynchronously
            e.ChildItems = await LoadRootEmployeesAsync();
        }
        else
        {
            var employee = e.ParentItem as EmployeeInfo;
            if (employee != null)
            {
                // Load child items asynchronously
                e.ChildItems = await LoadSubordinatesAsync(employee.ID);
            }
        }
    }
    catch (Exception ex)
    {
        // Handle errors
        MessageBox.Show($"Error loading data: {ex.Message}");
        e.ChildItems = Enumerable.Empty<EmployeeInfo>();
    }
}

private async Task<IEnumerable> LoadRootEmployeesAsync()
{
    // Simulate network delay
    await Task.Delay(100);
    
    return await Task.Run(() =>
    {
        return EmployeeRepository.GetEmployees()
            .Where(x => x.ReportsTo == -1)
            .ToList();
    });
}

private async Task<IEnumerable> LoadSubordinatesAsync(int managerId)
{
    await Task.Delay(100);
    
    return await Task.Run(() =>
    {
        return EmployeeRepository.GetEmployees()
            .Where(x => x.ReportsTo == managerId)
            .ToList();
    });
}
```

### Error Handling

Robust error handling for load operations:

```csharp
private async void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
{
    try
    {
        // Show loading indicator
        this.Cursor = Cursors.Wait;

        if (e.ParentItem == null)
        {
            e.ChildItems = await LoadRootEmployeesAsync();
        }
        else
        {
            var employee = e.ParentItem as EmployeeInfo;
            if (employee != null)
            {
                e.ChildItems = await LoadSubordinatesAsync(employee.ID);
            }
        }
    }
    catch (HttpRequestException httpEx)
    {
        MessageBox.Show($"Network error: {httpEx.Message}", "Error", 
                       MessageBoxButton.OK, MessageBoxImage.Error);
        e.ChildItems = Enumerable.Empty<EmployeeInfo>();
    }
    catch (TimeoutException timeoutEx)
    {
        MessageBox.Show($"Request timeout: {timeoutEx.Message}", "Error",
                       MessageBoxButton.OK, MessageBoxImage.Error);
        e.ChildItems = Enumerable.Empty<EmployeeInfo>();
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Unexpected error: {ex.Message}", "Error",
                       MessageBoxButton.OK, MessageBoxImage.Error);
        e.ChildItems = Enumerable.Empty<EmployeeInfo>();
    }
    finally
    {
        // Hide loading indicator
        this.Cursor = Cursors.Arrow;
    }
}
```

## Command-Based Loading

### Basic Command Implementation

```csharp
using System.Windows.Input;
using Syncfusion.UI.Xaml.TreeGrid;

public class ViewModel
{
    public ObservableCollection<EmployeeInfo> EmployeeDetails { get; set; }
    public ICommand LoadOnDemandCommand { get; set; }

    public ViewModel()
    {
        EmployeeDetails = new ObservableCollection<EmployeeInfo>();
        LoadOnDemandCommand = new RelayCommand<object>(ExecuteLoadOnDemand);
        
        // Load initial root items
        LoadRootItems();
    }

  
    private void ExecuteLoadOnDemand(object parameter)
    {
        var node = parameter as TreeNode;
        if (node == null) return;

        var employee = node.Item as EmployeeInfo;

        if (employee != null)
        {
            var childItems = GetSubordinates(employee.ID);
            node.PopulateChildNodes(childItems);
        }
    }

    private void LoadRootItems()
    {
        var rootEmployees = GetRootEmployees();
        foreach (var employee in rootEmployees)
        {
            EmployeeDetails.Add(employee);
        }
    }

    private IEnumerable<EmployeeInfo> GetRootEmployees()
    {
        return EmployeeRepository.GetEmployees()
            .Where(x => x.ReportsTo == -1);
    }

    private IEnumerable<EmployeeInfo> GetSubordinates(int managerId)
    {
        return EmployeeRepository.GetEmployees()
            .Where(x => x.ReportsTo == managerId);
    }
}

// RelayCommand implementation
public class RelayCommand<T> : ICommand
{
    private readonly Action<T> _execute;
    private readonly Func<T, bool> _canExecute;

    public RelayCommand(Action<T> execute, Func<T, bool> canExecute = null)
    {
        _execute = execute ?? throw new ArgumentNullException(nameof(execute));
        _canExecute = canExecute;
    }

    public event EventHandler CanExecuteChanged
    {
        add { CommandManager.RequerySuggested += value; }
        remove { CommandManager.RequerySuggested -= value; }
    }

    public bool CanExecute(object parameter)
    {
        return _canExecute == null || _canExecute((T)parameter);
    }

    public void Execute(object parameter)
    {
        _execute((T)parameter);
    }
}
```

### Async Command

Implement async command for load-on-demand:

```csharp
public class AsyncRelayCommand<T> : ICommand
{
    private readonly Func<T, Task> _execute;
    private readonly Func<T, bool> _canExecute;
    private bool _isExecuting;

    public AsyncRelayCommand(Func<T, Task> execute, Func<T, bool> canExecute = null)
    {
        _execute = execute;
        _canExecute = canExecute;
    }

    public event EventHandler CanExecuteChanged
    {
        add { CommandManager.RequerySuggested += value; }
        remove { CommandManager.RequerySuggested -= value; }
    }

    public bool CanExecute(object parameter)
    {
        return !_isExecuting && (_canExecute == null || _canExecute((T)parameter));
    }

    public async void Execute(object parameter)
    {
        _isExecuting = true;
        RaiseCanExecuteChanged();

        try
        {
            await _execute((T)parameter);
        }
        finally
        {
            _isExecuting = false;
            RaiseCanExecuteChanged();
        }
    }

    private void RaiseCanExecuteChanged()
    {
        CommandManager.InvalidateRequerySuggested();
    }
}

// Usage in ViewModel
public class ViewModel
{
    public ICommand LoadOnDemandCommand { get; set; }

    public ViewModel()
    {
        LoadOnDemandCommand = new AsyncRelayCommand<object>(ExecuteLoadOnDemandAsync);
    }

    private async Task ExecuteLoadOnDemandAsync(object parameter)
    {
        var node = parameter as TreeNode;
        if (node == null) return;

        var employee = node.Item as EmployeeInfo;

        if (employee != null)
        {
            var childItems = await LoadSubordinatesAsync(employee.ID);
            node.PopulateChildNodes(childItems);
        }
    }
}
```

### MVVM Pattern

Complete MVVM implementation with load-on-demand:

```csharp
public class EmployeeViewModel : INotifyPropertyChanged
{
    private bool _isLoading;
    private string _statusMessage;

    public ObservableCollection<EmployeeInfo> EmployeeDetails { get; set; }
    public ICommand LoadOnDemandCommand { get; set; }

    public bool IsLoading
    {
        get => _isLoading;
        set
        {
            _isLoading = value;
            OnPropertyChanged();
        }
    }

    public string StatusMessage
    {
        get => _statusMessage;
        set
        {
            _statusMessage = value;
            OnPropertyChanged();
        }
    }

    public EmployeeViewModel()
    {
        EmployeeDetails = new ObservableCollection<EmployeeInfo>();

        LoadOnDemandCommand = new AsyncRelayCommand<object>(LoadChildNodesAsync);

        LoadInitialData();
    }

    private async void LoadInitialData()
    {
        IsLoading = true;
        StatusMessage = "Loading root employees...";

        try
        {
            var rootEmployees = await LoadRootEmployeesAsync();
            foreach (var employee in rootEmployees)
            {
                EmployeeDetails.Add(employee);
            }
            StatusMessage = $"Loaded {rootEmployees.Count()} employees";
        }
        catch (Exception ex)
        {
            StatusMessage = $"Error: {ex.Message}";
        }
        finally
        {
            IsLoading = false;
        }
    }

    private async Task LoadChildNodesAsync(object parameter)
    {
        var node = parameter as TreeNode;
        if (node == null) return;

        IsLoading = true;

        try
        {
            var parent = node.Item as EmployeeInfo;

            if (parent != null)
            {
                StatusMessage = $"Loading subordinates of {parent.FirstName} {parent.LastName}...";

                var items = await LoadSubordinatesAsync(parent.ID);

                node.PopulateChildNodes(items);

                StatusMessage = $"Loaded {items.Count()} subordinates";
            }
        }
        catch (Exception ex)
        {
            StatusMessage = $"Error: {ex.Message}";
        }
        finally
        {
            IsLoading = false;
        }
    }

    private async Task<IEnumerable<EmployeeInfo>> LoadRootEmployeesAsync()
    {
        await Task.Delay(500);
        return EmployeeRepository.GetEmployees().Where(x => x.ReportsTo == -1);
    }

    private async Task<IEnumerable<EmployeeInfo>> LoadSubordinatesAsync(int managerId)
    {
        await Task.Delay(500);
        return EmployeeRepository.GetEmployees().Where(x => x.ReportsTo == managerId);
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

## Advanced Scenarios

### Caching Strategies

Implement caching to avoid redundant data loads:

```csharp
public class CachedLoadOnDemandViewModel
{
    private readonly Dictionary<int, List<EmployeeInfo>> _childCache;
    private readonly HashSet<int> _loadedNodes;

    public ICommand LoadOnDemandCommand { get; set; }

    public CachedLoadOnDemandViewModel()
    {
        _childCache = new Dictionary<int, List<EmployeeInfo>>();
        _loadedNodes = new HashSet<int>();
        LoadOnDemandCommand = new AsyncRelayCommand<object>(LoadWithCacheAsync);
    }

    private async Task LoadWithCacheAsync(object parameter)
    {
        var node = parameter as TreeNode;
        if (node == null) return;

        var employee = node.Item as EmployeeInfo;
        if (employee == null) return;
        if (_loadedNodes.Contains(employee.ID))
            return;

        if (_childCache.ContainsKey(employee.ID))
        {
            node.PopulateChildNodes(_childCache[employee.ID]); 
            _loadedNodes.Add(employee.ID);
            return;
        }

        var subordinates = (await LoadSubordinatesAsync(employee.ID)).ToList();

        _childCache[employee.ID] = subordinates;
        _loadedNodes.Add(employee.ID);

        node.PopulateChildNodes(subordinates);   
    }

    // Clear cache method
    public void ClearCache()
    {
        _childCache.Clear();
        _loadedNodes.Clear();
    }

    // Invalidate specific node cache
    public void InvalidateCache(int employeeId)
    {
        _childCache.Remove(employeeId);
        _loadedNodes.Remove(employeeId);
    }
}
```

### Paging Support

Implement paging for large child collections:

```csharp
public class PagedLoadOnDemandViewModel
{
    private const int PageSize = 50;
    private readonly Dictionary<int, int> _currentPages;

    public ICommand LoadOnDemandCommand { get; set; }
    public ICommand LoadMoreCommand { get; set; }

    public PagedLoadOnDemandViewModel()
    {
        _currentPages = new Dictionary<int, int>();

        LoadOnDemandCommand = new AsyncRelayCommand<object>(LoadPagedDataAsync);
        LoadMoreCommand = new AsyncRelayCommand<TreeNode>(LoadMoreDataAsync);
    }

    private async Task LoadPagedDataAsync(object parameter)
    {
        var node = parameter as TreeNode;
        if (node == null) return;

        var employee = node.Item as EmployeeInfo;
        if (employee == null) return;

        if (!_currentPages.ContainsKey(employee.ID))
            _currentPages[employee.ID] = 0;

        var page = _currentPages[employee.ID];
        var items = await LoadSubordinatesPageAsync(employee.ID, page, PageSize);

        node.PopulateChildNodes(items);
    }

    private async Task LoadMoreDataAsync(TreeNode node)
    {
        if (node?.Item is not EmployeeInfo employee) return;

        _currentPages[employee.ID]++;

        var page = _currentPages[employee.ID];
        var items = await LoadSubordinatesPageAsync(employee.ID, page, PageSize);

        node.PopulateChildNodes(items); 
    }

    private async Task<IEnumerable<EmployeeInfo>> LoadSubordinatesPageAsync(int managerId, int page, int pageSize)
    {
        await Task.Delay(300);

        return EmployeeRepository.GetEmployees()
            .Where(x => x.ReportsTo == managerId)
            .Skip(page * pageSize)
            .Take(pageSize);
    }
}
```

### Remote Data Loading

Load data from remote API:

```csharp
public class RemoteLoadOnDemandViewModel
{
    private readonly HttpClient _httpClient;
    private readonly string _apiBaseUrl;

    public ICommand LoadOnDemandCommand { get; set; }

    public RemoteLoadOnDemandViewModel(string apiBaseUrl)
    {
        _apiBaseUrl = apiBaseUrl;

        _httpClient = new HttpClient
        {
            Timeout = TimeSpan.FromSeconds(30)
        };
        LoadOnDemandCommand = new AsyncRelayCommand<object>(LoadFromApiAsync);
    }

    private async Task LoadFromApiAsync(object parameter)
    {
        var node = parameter as TreeNode;
        if (node == null) return;

        try
        {
            var employee = node.Item as EmployeeInfo;
            if (employee == null) return;

            var url = $"{_apiBaseUrl}/employees/{employee.ID}/subordinates";
            var subordinates = await GetEmployeesFromApiAsync(url);
            node.PopulateChildNodes(subordinates);
        }
        catch (HttpRequestException ex)
        {
            MessageBox.Show($"Network error: {ex.Message}");
        }
        catch (TaskCanceledException)
        {
            MessageBox.Show("Request timed out");
        }
    }

    private async Task<IEnumerable<EmployeeInfo>> GetEmployeesFromApiAsync(string url)
    {
        var response = await _httpClient.GetAsync(url);
        response.EnsureSuccessStatusCode();

        var json = await response.Content.ReadAsStringAsync();
        var employees = JsonConvert.DeserializeObject<List<EmployeeInfo>>(json);

        return employees;
    }

    public void Dispose()
    {
        _httpClient?.Dispose();
    }
}
```

## Best Practices

### 1. Implement HasChildNodes

```csharp
public class EmployeeInfo : INotifyPropertyChanged
{
    public int ID { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public int ReportsTo { get; set; }
    
    // Indicates if this node has children
    public bool HasSubordinates
    {
        get
        {
            // This could be a database count or a flag
            return EmployeeRepository.HasSubordinates(this.ID);
        }
    }
}
```

### 2. Show Loading Indicators

```xaml
<Grid>
    <syncfusion:SfTreeGrid Name="treeGrid"
                           ItemsSource="{Binding EmployeeDetails}"
                           LoadOnDemandCommand="{Binding LoadOnDemandCommand}" />
    
    <Border Visibility="{Binding IsLoading, Converter={StaticResource BoolToVisibilityConverter}}"
            Background="#80000000">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ProgressBar IsIndeterminate="True" Width="200" Height="20"/>
            <TextBlock Text="{Binding StatusMessage}" 
                      Foreground="White" 
                      Margin="0,10,0,0"/>
        </StackPanel>
    </Border>
</Grid>
```

### 3. Handle Empty Results

```csharp
private async Task LoadSubordinatesAsync(object parameter)
{
    var node = parameter as TreeNode;
    if (node == null) return;

    var employee = node.Item as EmployeeInfo;
    if (employee == null) return;

    var subordinates = await GetSubordinatesAsync(employee.ID);

    if (subordinates == null || !subordinates.Any())
    {
        return;
    }

    node.PopulateChildNodes(subordinates);
}
```

### 4. Implement Cancellation

```csharp
private CancellationTokenSource _loadCancellationTokenSource;

private async Task LoadSubordinatesAsync(int managerId)
{
    // Cancel previous operation
    _loadCancellationTokenSource?.Cancel();
    _loadCancellationTokenSource = new CancellationTokenSource();

    try
    {
        var employees = await Task.Run(() =>
        {
            return EmployeeRepository.GetEmployees()
                .Where(x => x.ReportsTo == managerId)
                .ToList();
        }, _loadCancellationTokenSource.Token);

        return employees;
    }
    catch (OperationCanceledException)
    {
        return Enumerable.Empty<EmployeeInfo>();
    }
}
```

## Troubleshooting

### Issue: RequestTreeItems Not Firing

**Problem**: Event handler not being called

**Solutions**:
1. Ensure ItemsSource is not null
2. Check that nodes have HasChildNodes = true
3. Verify event is properly attached

```csharp
// Check in code-behind
Debug.WriteLine($"Event attached: {treeGrid.RequestTreeItems != null}");

// Ensure HasChildNodes is true
public class EmployeeInfo
{
    public bool HasChildNodes => true; // Or based on actual data
}
```

### Issue: Duplicate Data Loading

**Problem**: Same data loaded multiple times

**Solutions**:
1. Implement caching
2. Track loaded nodes
3. Check node population status

```csharp
private HashSet<int> _loadedNodeIds = new HashSet<int>();

private void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
{
    if (e.ParentItem == null)
    {
        e.ChildItems = GetRootEmployees();
        return;
    }

    var employee = e.ParentItem as EmployeeInfo;
    if (_loadedNodeIds.Contains(employee.ID))
    {
        return; // Already loaded
    }

    e.ChildItems = GetSubordinates(employee.ID);
    _loadedNodeIds.Add(employee.ID);
}
```

### Issue: UI Freezing During Load

**Problem**: UI becomes unresponsive when loading data

**Solutions**:
1. Use async/await pattern
2. Load data on background thread
3. Implement progress feedback

```csharp
private async void TreeGrid_RequestTreeItems(object sender, TreeGridRequestTreeItemsEventArgs e)
{
    // Show progress indicator
    progressBar.Visibility = Visibility.Visible;

    try
    {
        if (e.ParentItem == null)
        {
            e.ChildItems = await Task.Run(() => GetRootEmployees());
        }
        else
        {
            var employee = e.ParentItem as EmployeeInfo;
            e.ChildItems = await Task.Run(() => GetSubordinates(employee.ID));
        }
    }
    finally
    {
        progressBar.Visibility = Visibility.Collapsed;
    }
}
```

### Issue: Memory Leaks

**Problem**: Memory consumption increases over time

**Solutions**:
1. Dispose resources properly
2. Unsubscribe from events
3. Clear caches periodically

```csharp
public class ViewModel : IDisposable
{
    private Dictionary<int, List<EmployeeInfo>> _cache;

    public void Dispose()
    {
        _cache?.Clear();
        _cache = null;
    }
}

// In Window closing
private void Window_Closing(object sender, CancelEventArgs e)
{
    treeGrid.RequestTreeItems -= TreeGrid_RequestTreeItems;
    (DataContext as ViewModel)?.Dispose();
}
```

### Issue: Load-On-Demand Command Not Executing

**Problem**: Command not being called when node expands

**Solutions**:
1. Verify command binding
2. Check CanExecute returns true
3. Ensure correct parameter type

```csharp
// Verify in ViewModel
public ICommand LoadOnDemandCommand { get; set; }

public ViewModel()
{
    LoadOnDemandCommand = new RelayCommand<object>(
        execute: LoadNodes,
        canExecute: param => param is TreeNode
    );
}

// Debug command execution
private void LoadNodes(object parameter)
{
    var node = parameter as TreeNode;
    if (node == null) return;

    Debug.WriteLine($"Command executed for node level: {node.Level}");

    // Load logic...
}
```

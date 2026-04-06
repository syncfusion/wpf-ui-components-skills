# Commands and Events in WPF Navigation Drawer

## Table of Contents
- [Drawer State Events](#drawer-state-events)
- [Item Interaction Events](#item-interaction-events)
- [Command Support](#command-support)

## Drawer State Events

Events for tracking drawer open/close state changes.

### Opening Event

Triggered when the drawer starts to open. Allows cancellation.

```xaml
<syncfusion:SfNavigationDrawer x:Name="navigationDrawer"
                               Opening="NavigationDrawer_Opening">
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9">
            <Label Content="Header View"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void NavigationDrawer_Opening(object sender, System.ComponentModel.CancelEventArgs e)
{
    // Cancel opening if needed
    if (SomeCondition())
    {
        e.Cancel = true;
    }
}
```

### Opened Event

Triggered after the drawer has completely opened.

```xaml
<syncfusion:SfNavigationDrawer Opened="NavigationDrawer_Opened">
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9">
            <Label Content="Header View"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void NavigationDrawer_Opened(object sender, EventArgs e)
{
    // Perform action after drawer opens
    StatusTextBlock.Text = "Drawer is now open";
}
```

### Closing Event

Triggered when the drawer starts to close. Allows cancellation.

```xaml
<syncfusion:SfNavigationDrawer Closing="NavigationDrawer_Closing">
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9">
            <Label Content="Header View"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void NavigationDrawer_Closing(object sender, System.ComponentModel.CancelEventArgs e)
{
    // Cancel closing if there are unsaved changes
    if (HasUnsavedChanges())
    {
        var result = MessageBox.Show(
            "You have unsaved changes. Close anyway?",
            "Confirm",
            MessageBoxButton.YesNo
        );
        
        if (result == MessageBoxResult.No)
        {
            e.Cancel = true;
        }
    }
}
```

### Closed Event

Triggered after the drawer has completely closed.

```xaml
<syncfusion:SfNavigationDrawer Closed="NavigationDrawer_Closed">
    <syncfusion:SfNavigationDrawer.DrawerHeaderView>
        <Grid Background="#31ade9">
            <Label Content="Header View"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.DrawerHeaderView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void NavigationDrawer_Closed(object sender, EventArgs e)
{
    // Perform cleanup after drawer closes
    StatusTextBlock.Text = "Drawer is now closed";
}
```

## Item Interaction Events

Events for navigation item interactions (Compact/Expanded modes only).

### ItemClicked Event

Triggered when a navigation item is clicked.

**Note:** This event only fires in Compact and Expanded display modes, not in Default mode.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               ItemClicked="NavigationDrawer_ItemClicked">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M2,21L23,12L2,3v7l15,2L2,13V21z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Drafts">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M3,3h18v14H3V3z M5,5v10h14V5H5z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label x:Name="contentLabel" Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void NavigationDrawer_ItemClicked(object sender, NavigationItemClickedEventArgs e)
{
    var clickedItem = e.Item as NavigationItem;
    contentLabel.Content = $"{clickedItem.Header} selected";
    
    // Navigate to corresponding page/view
    NavigateToPage(clickedItem.Header);
}

private void NavigateToPage(string pageName)
{
    // Your navigation logic
}
```

### ItemExpanded Event

Triggered when a navigation item with sub-items is expanded.

**Note:** Only fires in Compact and Expanded display modes.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               ItemExpanded="NavigationDrawer_ItemExpanded">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
        
        <syncfusion:NavigationItem Header="Primary">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z" Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="Social">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,2l3,7l7,1l-5,5l1,7l-6-4l-6,4l1-7l-5-5l7-1z" Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void NavigationDrawer_ItemExpanded(object sender, NavigationItemExpandedEventArgs e)
{
    var expandedItem = e.Item as NavigationItem;
    System.Diagnostics.Debug.WriteLine($"{expandedItem.Header} expanded");
    
    // Log expansion for analytics
    LogItemExpansion(expandedItem.Header);
}
```

### ItemCollapsed Event

Triggered when a navigation item with sub-items is collapsed.

**Note:** Only fires in Compact and Expanded display modes.

```xaml
<syncfusion:SfNavigationDrawer DisplayMode="Compact"
                               ItemCollapsed="NavigationDrawer_ItemCollapsed">
    <syncfusion:NavigationItem Header="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
        
        <syncfusion:NavigationItem Header="Primary">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z" Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="Social">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,2l3,7l7,1l-5,5l1,7l-6-4l-6,4l1-7l-5-5l7-1z" Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void NavigationDrawer_ItemCollapsed(object sender, NavigationItemCollapsedEventArgs e)
{
    var collapsedItem = e.Item as NavigationItem;
    System.Diagnostics.Debug.WriteLine($"{collapsedItem.Header} collapsed");
}
```

## Command Support

Bind commands to navigation items for MVVM pattern support.

### DelegateCommand Implementation

```csharp
public class DelegateCommand<T> : ICommand
{
    private Predicate<T> _canExecute;
    private Action<T> _method;
    bool _canExecuteCache = true;

    public DelegateCommand(Action<T> method) : this(method, null) { }

    public DelegateCommand(Action<T> method, Predicate<T> canExecute)
    {
        _method = method;
        _canExecute = canExecute;
    }

    public bool CanExecute(object parameter)
    {
        if (_canExecute != null)
        {
            bool tempCanExecute = _canExecute((T)parameter);
            if (_canExecuteCache != tempCanExecute)
            {
                _canExecuteCache = tempCanExecute;
                this.RaiseCanExecuteChanged();
            }
        }
        return _canExecuteCache;
    }

    public void RaiseCanExecuteChanged()
    {
        CanExecuteChanged?.Invoke(this, new EventArgs());
    }

    public void Execute(object parameter)
    {
        _method?.Invoke((T)parameter);
    }

    public event EventHandler CanExecuteChanged;
}
```

### ViewModel with Commands

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private bool _canperformaction = true;
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    public ViewModel()
    {
        ClickCommand = new DelegateCommand<object>(ClickAction, CanPerformClickAction);
    }
    
    public DelegateCommand<object> ClickCommand { get; set; }
    
    public bool CanPerformAction
    {
        get { return _canperformaction; }
        set
        {
            _canperformaction = value;
            this.ClickCommand.RaiseCanExecuteChanged();
            this.OnPropertyChanged("CanPerformAction");
        }
    }
    
    private bool CanPerformClickAction(object parameter)
    {
        return CanPerformAction;
    }
    
    private void ClickAction(object parameter)
    {
        MessageBox.Show(parameter.ToString() + " item has been clicked");
    }
    
    public void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

### XAML with Command Binding

```xaml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<syncfusion:SfNavigationDrawer DisplayMode="Compact">
    <syncfusion:NavigationItem Header="Inbox"
                               Command="{Binding ClickCommand}"
                               CommandParameter="Inbox">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Sent mail"
                               Command="{Binding ClickCommand}"
                               CommandParameter="Sent mail">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M2,21L23,12L2,3v7l15,2L2,13V21z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Drafts"
                               Command="{Binding ClickCommand}"
                               CommandParameter="Drafts">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M3,3h18v14H3V3z M5,5v10h14V5H5z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.FooterItems>
        <syncfusion:NavigationItem Header="LogOut"
                                   ItemType="Button"
                                   Command="{Binding ClickCommand}"
                                   CommandParameter="LogOut">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M3,6h18v2H3V6z M6,8v12c0,1.1 0.9,2 2,2h8c1.1,0 2-0.9 2-2V8H6z" Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:SfNavigationDrawer.FooterItems>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Label Content="Content View"/>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

### Important Notes on Commands

1. **Item Types:** Only `Tab` and `Button` item types execute commands
2. **Display Modes:** Command execution only works in Compact and Expanded modes
3. **CommandParameter:** Use to pass context (e.g., page name, item ID)
4. **Can Execute:** Implement to enable/disable navigation items dynamically

### Example: Navigation with MVVM

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
    
    public DelegateCommand<object> NavigateCommand { get; set; }
    
    private string _currentPage;
    public string CurrentPage
    {
        get { return _currentPage; }
        set
        {
            _currentPage = value;
            OnPropertyChanged("CurrentPage");
        }
    }
    
    public MainViewModel()
    {
        NavigateCommand = new DelegateCommand<object>(Navigate);
        CurrentPage = "Home";
    }
    
    private void Navigate(object parameter)
    {
        string pageName = parameter?.ToString();
        CurrentPage = pageName;
        
        // Navigate to page
        // Frame.Navigate(new Uri($"Views/{pageName}Page.xaml", UriKind.Relative));
    }
    
    protected void OnPropertyChanged(string name)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
    }
}
```

## Complete Example

```xaml
<Window.DataContext>
    <local:MainViewModel/>
</Window.DataContext>

<syncfusion:SfNavigationDrawer x:Name="navigationDrawer"
                               DisplayMode="Expanded"
                               Opening="NavigationDrawer_Opening"
                               Opened="NavigationDrawer_Opened"
                               Closing="NavigationDrawer_Closing"
                               Closed="NavigationDrawer_Closed"
                               ItemClicked="NavigationDrawer_ItemClicked"
                               ItemExpanded="NavigationDrawer_ItemExpanded"
                               ItemCollapsed="NavigationDrawer_ItemCollapsed">
    
    <syncfusion:NavigationItem Header="Dashboard"
                               Command="{Binding NavigateCommand}"
                               CommandParameter="Dashboard"
                               IsSelected="True">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M20,4H4c-1.1,0-2,0.9-2,2v12c0,1.1,0.9,2,2,2h16c1.1,0,2-0.9,2-2V6C22,4.9,21.1,4,20,4z M20,8l-8,5L4,8V6l8,5l8-5V8z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
    </syncfusion:NavigationItem>
    
    <syncfusion:NavigationItem Header="Reports"
                               Command="{Binding NavigateCommand}"
                               CommandParameter="Reports">
        <syncfusion:NavigationItem.Icon>
            <Path Data="M2,21L23,12L2,3v7l15,2L2,13V21z" Fill="White"/>
        </syncfusion:NavigationItem.Icon>
        
        <syncfusion:NavigationItem Header="Sales"
                                   Command="{Binding NavigateCommand}"
                                   CommandParameter="SalesReport">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,17.27L18.18,21l-1.64-7.03L22,9.24l-7.19-0.61L12,2 9.19,8.63 2,9.24l5.46,4.73L5.82,21z" Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
        
        <syncfusion:NavigationItem Header="Analytics"
                                   Command="{Binding NavigateCommand}"
                                   CommandParameter="Analytics">
            <syncfusion:NavigationItem.Icon>
                <Path Data="M12,2l3,7l7,1l-5,5l1,7l-6-4l-6,4l1-7l-5-5l7-1z" Fill="White"/>
            </syncfusion:NavigationItem.Icon>
        </syncfusion:NavigationItem>
    </syncfusion:NavigationItem>
    
    <syncfusion:SfNavigationDrawer.ContentView>
        <Grid>
            <TextBlock Text="{Binding CurrentPage}" 
                       HorizontalAlignment="Center"
                       VerticalAlignment="Center"
                       FontSize="24"/>
        </Grid>
    </syncfusion:SfNavigationDrawer.ContentView>
</syncfusion:SfNavigationDrawer>
```

**Code-behind:**
```csharp
private void NavigationDrawer_Opening(object sender, CancelEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("Drawer is opening...");
}

private void NavigationDrawer_Opened(object sender, EventArgs e)
{
    System.Diagnostics.Debug.WriteLine("Drawer opened");
}

private void NavigationDrawer_Closing(object sender, CancelEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("Drawer is closing...");
}

private void NavigationDrawer_Closed(object sender, EventArgs e)
{
    System.Diagnostics.Debug.WriteLine("Drawer closed");
}

private void NavigationDrawer_ItemClicked(object sender, NavigationItemClickedEventArgs e)
{
    var item = e.Item as NavigationItem;
    System.Diagnostics.Debug.WriteLine($"Item clicked: {item.Header}");
}

private void NavigationDrawer_ItemExpanded(object sender, NavigationItemExpandedEventArgs e)
{
    var item = e.Item as NavigationItem;
    System.Diagnostics.Debug.WriteLine($"Item expanded: {item.Header}");
}

private void NavigationDrawer_ItemCollapsed(object sender, NavigationItemCollapsedEventArgs e)
{
    var item = e.Item as NavigationItem;
    System.Diagnostics.Debug.WriteLine($"Item collapsed: {item.Header}");
}
```

## Sample Code

View complete sample on GitHub: [Commands Sample](https://github.com/SyncfusionExamples/wpf-sfnavigationdrawer-samples/tree/main/Commands)


# Data Binding in WPF Tabbed Window

## Table of Contents
- [Overview](#overview)
- [MVVM Pattern Introduction](#mvvm-pattern-introduction)
- [ItemsSource Binding](#itemssource-binding)
- [Creating Tab Models](#creating-tab-models)
- [ViewModel Implementation](#viewmodel-implementation)
- [Header and Content Templates](#header-and-content-templates)
- [ItemContainerStyle Configuration](#itemcontainerstyle-configuration)
- [Dynamic Collection Updates](#dynamic-collection-updates)
- [Advanced Data-Driven Scenarios](#advanced-data-driven-scenarios)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion WPF Tabbed Window provides full support for MVVM (Model-View-ViewModel) pattern through data binding. This enables clean separation of concerns and data-driven tab management.

**Key Features:**
- `ItemsSource` binding to observable collections
- Automatic tab generation from data
- Two-way binding for tab properties
- Custom header and content templates
- Dynamic add/remove/update operations
- Clean MVVM architecture

This guide demonstrates how to build data-driven tabbed interfaces that respond to collection changes and maintain clean separation between UI and business logic.

## MVVM Pattern Introduction

### Architecture Overview

```
┌─────────────┐         ┌──────────────┐         ┌───────────┐
│   Model     │────────▶│  ViewModel   │◀────────│   View    │
│ (Data/Logic)│         │ (Presentation)│  Binding│  (XAML)   │
└─────────────┘         └──────────────┘         └───────────┘
```

**Benefits for Tabbed Windows:**
- **Testability:** ViewModels can be unit tested
- **Maintainability:** Clear separation of concerns
- **Flexibility:** Easy to swap views or modify data
- **Reactivity:** UI updates automatically with data changes

### Basic MVVM Structure

```csharp
// Model - Pure data
public class Document
{
    public string FileName { get; set; }
    public string Content { get; set; }
    public DateTime LastModified { get; set; }
}

// ViewModel - Presentation logic
public class MainViewModel : INotifyPropertyChanged
{
    public ObservableCollection<Document> OpenDocuments { get; set; }
    
    // Commands, methods, etc.
}

// View - XAML binding
<syncfusion:SfTabControl ItemsSource="{Binding OpenDocuments}">
    <!-- Templates -->
</syncfusion:SfTabControl>
```

## ItemsSource Binding

The `ItemsSource` property binds the tab control to a collection. Each item in the collection becomes a tab.

### Basic ItemsSource Example

**ViewModel:**

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class MainViewModel : INotifyPropertyChanged
{
    public ObservableCollection<string> TabTitles { get; set; }
    
    public MainViewModel()
    {
        TabTitles = new ObservableCollection<string>
        {
            "Tab 1",
            "Tab 2",
            "Tab 3"
        };
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

**XAML:**

```xml
<Window.DataContext>
    <local:MainViewModel />
</Window.DataContext>

<syncfusion:SfTabControl ItemsSource="{Binding TabTitles}">
    <!-- Each string becomes a tab header -->
</syncfusion:SfTabControl>
```

### Setting DataContext

**Option 1: In XAML**

```xml
<Window.DataContext>
    <local:MainViewModel />
</Window.DataContext>
```

**Option 2: In Code-Behind**

```csharp
public MainWindow()
{
    InitializeComponent();
    this.DataContext = new MainViewModel();
}
```

**Option 3: Using Dependency Injection**

```csharp
public MainWindow(MainViewModel viewModel)
{
    InitializeComponent();
    this.DataContext = viewModel;
}
```

## Creating Tab Models

Design models that represent tab data. Use properties for all displayable/bindable data.

### Simple Tab Model

```csharp
public class TabItem
{
    public string Header { get; set; }
    public string Content { get; set; }
}

// Usage
var tabs = new ObservableCollection<TabItem>
{
    new TabItem { Header = "Tab 1", Content = "Content 1" },
    new TabItem { Header = "Tab 2", Content = "Content 2" }
};
```

### Rich Tab Model with INotifyPropertyChanged

```csharp
using System.ComponentModel;

public class DocumentTab : INotifyPropertyChanged
{
    private string _title;
    private string _content;
    private bool _isModified;
    private string _filePath;
    
    public string Title
    {
        get => _title;
        set
        {
            if (_title != value)
            {
                _title = value;
                OnPropertyChanged(nameof(Title));
            }
        }
    }
    
    public string Content
    {
        get => _content;
        set
        {
            if (_content != value)
            {
                _content = value;
                IsModified = true;
                OnPropertyChanged(nameof(Content));
            }
        }
    }
    
    public bool IsModified
    {
        get => _isModified;
        set
        {
            if (_isModified != value)
            {
                _isModified = value;
                OnPropertyChanged(nameof(IsModified));
                OnPropertyChanged(nameof(DisplayTitle)); // Update computed property
            }
        }
    }
    
    public string FilePath
    {
        get => _filePath;
        set
        {
            if (_filePath != value)
            {
                _filePath = value;
                OnPropertyChanged(nameof(FilePath));
            }
        }
    }
    
    // Computed property for display
    public string DisplayTitle => IsModified ? $"{Title}*" : Title;
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

### Tab Model with Icon

```csharp
public class DocumentTab : INotifyPropertyChanged
{
    private string _title;
    private string _iconPath;
    private UIElement _content;
    
    public string Title
    {
        get => _title;
        set { _title = value; OnPropertyChanged(nameof(Title)); }
    }
    
    public string IconPath
    {
        get => _iconPath;
        set { _iconPath = value; OnPropertyChanged(nameof(IconPath)); }
    }
    
    public UIElement Content
    {
        get => _content;
        set { _content = value; OnPropertyChanged(nameof(Content)); }
    }
    
    // INotifyPropertyChanged implementation
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

## ViewModel Implementation

Create ViewModels that manage tab collections and operations.

### Basic ViewModel

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;

public class MainViewModel : INotifyPropertyChanged
{
    private DocumentTab _selectedTab;
    
    public ObservableCollection<DocumentTab> OpenTabs { get; set; }
    
    public DocumentTab SelectedTab
    {
        get => _selectedTab;
        set
        {
            if (_selectedTab != value)
            {
                _selectedTab = value;
                OnPropertyChanged(nameof(SelectedTab));
            }
        }
    }
    
    public MainViewModel()
    {
        OpenTabs = new ObservableCollection<DocumentTab>
        {
            new DocumentTab 
            { 
                Title = "Document 1", 
                Content = "Content of document 1" 
            },
            new DocumentTab 
            { 
                Title = "Document 2", 
                Content = "Content of document 2" 
            }
        };
        
        SelectedTab = OpenTabs[0];
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

### ViewModel with Commands

```csharp
using System.Windows.Input;

public class MainViewModel : INotifyPropertyChanged
{
    public ObservableCollection<DocumentTab> OpenTabs { get; set; }
    
    public ICommand NewTabCommand { get; }
    public ICommand CloseTabCommand { get; }
    public ICommand SaveTabCommand { get; }
    
    public MainViewModel()
    {
        OpenTabs = new ObservableCollection<DocumentTab>();
        
        NewTabCommand = new RelayCommand(CreateNewTab);
        CloseTabCommand = new RelayCommand<DocumentTab>(CloseTab);
        SaveTabCommand = new RelayCommand<DocumentTab>(SaveTab, CanSaveTab);
        
        // Add initial tab
        CreateNewTab();
    }
    
    private void CreateNewTab()
    {
        var newTab = new DocumentTab
        {
            Title = $"Untitled {OpenTabs.Count + 1}",
            Content = string.Empty
        };
        
        OpenTabs.Add(newTab);
    }
    
    private void CloseTab(DocumentTab tab)
    {
        if (tab.IsModified)
        {
            // Show save prompt
            var result = MessageBox.Show(
                "Save changes?", 
                "Confirm", 
                MessageBoxButton.YesNoCancel);
            
            if (result == MessageBoxResult.Yes)
            {
                SaveTab(tab);
            }
            else if (result == MessageBoxResult.Cancel)
            {
                return;
            }
        }
        
        OpenTabs.Remove(tab);
    }
    
    private void SaveTab(DocumentTab tab)
    {
        // Save logic
        tab.IsModified = false;
    }
    
    private bool CanSaveTab(DocumentTab tab)
    {
        return tab?.IsModified == true;
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string name) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}

// Simple RelayCommand implementation
public class RelayCommand : ICommand
{
    private readonly Action _execute;
    private readonly Func<bool> _canExecute;
    
    public RelayCommand(Action execute, Func<bool> canExecute = null)
    {
        _execute = execute;
        _canExecute = canExecute;
    }
    
    public event EventHandler CanExecuteChanged
    {
        add { CommandManager.RequerySuggested += value; }
        remove { CommandManager.RequerySuggested -= value; }
    }
    
    public bool CanExecute(object parameter) => _canExecute?.Invoke() ?? true;
    public void Execute(object parameter) => _execute();
}

public class RelayCommand<T> : ICommand
{
    private readonly Action<T> _execute;
    private readonly Func<T, bool> _canExecute;
    
    public RelayCommand(Action<T> execute, Func<T, bool> canExecute = null)
    {
        _execute = execute;
        _canExecute = canExecute;
    }
    
    public event EventHandler CanExecuteChanged
    {
        add { CommandManager.RequerySuggested += value; }
        remove { CommandManager.RequerySuggested -= value; }
    }
    
    public bool CanExecute(object parameter) => 
        _canExecute?.Invoke((T)parameter) ?? true;
        
    public void Execute(object parameter) => _execute((T)parameter);
}
```

## Header and Content Templates

Customize how tab headers and content are displayed using DataTemplates.

### Basic Header Template

**XAML:**

```xml
<Window.DataContext>
    <local:MainViewModel />
</Window.DataContext>

<syncfusion:SfTabControl ItemsSource="{Binding OpenTabs}">
    
    <!-- Define how headers are displayed -->
    <syncfusion:SfTabControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfTabItem">
            <Setter Property="HeaderTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <TextBlock Text="{Binding Title}" />
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfTabControl.ItemContainerStyle>
    
    <!-- Define how content is displayed -->
    <syncfusion:SfTabControl.ContentTemplate>
        <DataTemplate>
            <TextBox Text="{Binding Content, Mode=TwoWay}" 
                    AcceptsReturn="True"
                    Margin="10"/>
        </DataTemplate>
    </syncfusion:SfTabControl.ContentTemplate>
    
</syncfusion:SfTabControl>
```

### Header Template with Icon

```xml
<syncfusion:SfTabControl.ItemContainerStyle>
    <Style TargetType="syncfusion:SfTabItem">
        <Setter Property="HeaderTemplate">
            <Setter.Value>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal">
                        <Image Source="{Binding IconPath}" 
                              Width="16" 
                              Height="16"
                              Margin="0,0,4,0"/>
                        <TextBlock Text="{Binding DisplayTitle}" 
                                  VerticalAlignment="Center"/>
                    </StackPanel>
                </DataTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</syncfusion:SfTabControl.ItemContainerStyle>
```

### Header with Close Button

```xml
<syncfusion:SfTabControl.ItemContainerStyle>
    <Style TargetType="syncfusion:SfTabItem">
        <Setter Property="HeaderTemplate">
            <Setter.Value>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal">
                        <TextBlock Text="{Binding Title}" 
                                  VerticalAlignment="Center"
                                  Margin="0,0,8,0"/>
                        <Button Content="×" 
                               Command="{Binding DataContext.CloseTabCommand, 
                                        RelativeSource={RelativeSource AncestorType=Window}}"
                               CommandParameter="{Binding}"
                               Style="{StaticResource CloseButtonStyle}"/>
                    </StackPanel>
                </DataTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</syncfusion:SfTabControl.ItemContainerStyle>
```

### Rich Content Template

```xml
<syncfusion:SfTabControl.ContentTemplate>
    <DataTemplate>
        <Grid Margin="10">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            
            <!-- Header info -->
            <StackPanel Grid.Row="0" Orientation="Horizontal" Margin="0,0,0,10">
                <TextBlock Text="File: " FontWeight="Bold"/>
                <TextBlock Text="{Binding FilePath}"/>
                <TextBlock Text=" | Modified: " Margin="10,0,0,0"/>
                <TextBlock Text="{Binding IsModified}"/>
            </StackPanel>
            
            <!-- Content editor -->
            <TextBox Grid.Row="1" 
                    Text="{Binding Content, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"
                    AcceptsReturn="True"
                    VerticalScrollBarVisibility="Auto"
                    FontFamily="Consolas"
                    FontSize="12"/>
            
            <!-- Status bar -->
            <StatusBar Grid.Row="2" Margin="0,10,0,0">
                <TextBlock Text="{Binding Content.Length, StringFormat='Characters: {0}'}"/>
            </StatusBar>
        </Grid>
    </DataTemplate>
</syncfusion:SfTabControl.ContentTemplate>
```

## ItemContainerStyle Configuration

Configure properties of the generated `SfTabItem` containers.

### Basic ItemContainerStyle

```xml
<syncfusion:SfTabControl ItemsSource="{Binding OpenTabs}">
    <syncfusion:SfTabControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfTabItem">
            <Setter Property="CloseButtonVisibility" Value="Visible"/>
            <Setter Property="MinWidth" Value="100"/>
            <Setter Property="MaxWidth" Value="200"/>
            <Setter Property="Height" Value="30"/>
        </Style>
    </syncfusion:SfTabControl.ItemContainerStyle>
</syncfusion:SfTabControl>
```

### Conditional Styling with DataTriggers

```xml
<syncfusion:SfTabControl.ItemContainerStyle>
    <Style TargetType="syncfusion:SfTabItem">
        <Setter Property="CloseButtonVisibility" Value="Visible"/>
        
        <!-- Default appearance -->
        <Setter Property="Foreground" Value="Black"/>
        
        <Style.Triggers>
            <!-- Highlight modified tabs -->
            <DataTrigger Binding="{Binding IsModified}" Value="True">
                <Setter Property="Foreground" Value="Red"/>
                <Setter Property="FontWeight" Value="Bold"/>
            </DataTrigger>
            
            <!-- Special styling for read-only tabs -->
            <DataTrigger Binding="{Binding IsReadOnly}" Value="True">
                <Setter Property="CloseButtonVisibility" Value="Collapsed"/>
                <Setter Property="Foreground" Value="Gray"/>
            </DataTrigger>
        </Style.Triggers>
    </Style>
</syncfusion:SfTabControl.ItemContainerStyle>
```

### Binding IsSelected to ViewModel

```xml
<syncfusion:SfTabControl 
    ItemsSource="{Binding OpenTabs}"
    SelectedItem="{Binding SelectedTab, Mode=TwoWay}">
    
    <syncfusion:SfTabControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfTabItem">
            <Setter Property="IsSelected" 
                   Value="{Binding IsSelected, Mode=TwoWay}"/>
        </Style>
    </syncfusion:SfTabControl.ItemContainerStyle>
</syncfusion:SfTabControl>
```

## Dynamic Collection Updates

ObservableCollection automatically updates the UI when items are added, removed, or changed.

### Adding Tabs Dynamically

```csharp
public void AddNewDocument()
{
    var newDoc = new DocumentTab
    {
        Title = $"Untitled {OpenTabs.Count + 1}",
        Content = string.Empty,
        IsModified = false
    };
    
    OpenTabs.Add(newDoc);
    SelectedTab = newDoc; // Make it active
}
```

### Removing Tabs

```csharp
public void RemoveDocument(DocumentTab tab)
{
    if (tab == null) return;
    
    OpenTabs.Remove(tab);
    
    // Select another tab if the removed one was selected
    if (SelectedTab == tab && OpenTabs.Count > 0)
    {
        SelectedTab = OpenTabs[0];
    }
}
```

### Bulk Operations

```csharp
public void CloseAllDocuments()
{
    // Prompt for unsaved changes
    var modifiedDocs = OpenTabs.Where(t => t.IsModified).ToList();
    if (modifiedDocs.Any())
    {
        var result = MessageBox.Show(
            $"Save {modifiedDocs.Count} modified documents?",
            "Confirm",
            MessageBoxButton.YesNoCancel);
        
        if (result == MessageBoxResult.Cancel) return;
        if (result == MessageBoxResult.Yes)
        {
            foreach (var doc in modifiedDocs)
            {
                SaveDocument(doc);
            }
        }
    }
    
    OpenTabs.Clear();
}

public void CloseAllExcept(DocumentTab exceptTab)
{
    var toRemove = OpenTabs.Where(t => t != exceptTab).ToList();
    foreach (var tab in toRemove)
    {
        OpenTabs.Remove(tab);
    }
}
```

### Reordering Tabs

```csharp
public void MoveTab(DocumentTab tab, int newIndex)
{
    int oldIndex = OpenTabs.IndexOf(tab);
    if (oldIndex == -1 || newIndex < 0 || newIndex >= OpenTabs.Count)
        return;
    
    OpenTabs.Move(oldIndex, newIndex);
}
```

## Advanced Data-Driven Scenarios

### Loading Tabs from File System

```csharp
public async Task LoadDocumentsFromDirectory(string directoryPath)
{
    var files = Directory.GetFiles(directoryPath, "*.txt");
    
    foreach (var filePath in files)
    {
        var content = await File.ReadAllTextAsync(filePath);
        var tab = new DocumentTab
        {
            Title = Path.GetFileName(filePath),
            FilePath = filePath,
            Content = content,
            IsModified = false
        };
        
        OpenTabs.Add(tab);
    }
}
```

### Auto-Save on Modification

```csharp
public class DocumentTab : INotifyPropertyChanged
{
    private string _content;
    
    public string Content
    {
        get => _content;
        set
        {
            if (_content != value)
            {
                _content = value;
                IsModified = true;
                OnPropertyChanged(nameof(Content));
                
                // Trigger auto-save
                AutoSaveTimer?.Stop();
                AutoSaveTimer?.Start();
            }
        }
    }
    
    private System.Timers.Timer AutoSaveTimer { get; set; }
    
    public DocumentTab()
    {
        AutoSaveTimer = new System.Timers.Timer(5000); // 5 seconds
        AutoSaveTimer.Elapsed += (s, e) => AutoSave();
        AutoSaveTimer.AutoReset = false;
    }
    
    private void AutoSave()
    {
        // Save logic here
        if (!string.IsNullOrEmpty(FilePath))
        {
            File.WriteAllText(FilePath, Content);
            IsModified = false;
        }
    }
}
```

### Recent Documents List

```csharp
public class MainViewModel : INotifyPropertyChanged
{
    public ObservableCollection<DocumentTab> OpenTabs { get; set; }
    public ObservableCollection<string> RecentDocuments { get; set; }
    
    private const int MaxRecentDocuments = 10;
    
    public void OpenDocument(string filePath)
    {
        // Check if already open
        var existing = OpenTabs.FirstOrDefault(t => t.FilePath == filePath);
        if (existing != null)
        {
            SelectedTab = existing;
            return;
        }
        
        // Load document
        var content = File.ReadAllText(filePath);
        var tab = new DocumentTab
        {
            Title = Path.GetFileName(filePath),
            FilePath = filePath,
            Content = content
        };
        
        OpenTabs.Add(tab);
        SelectedTab = tab;
        
        // Update recent list
        AddToRecentDocuments(filePath);
    }
    
    private void AddToRecentDocuments(string filePath)
    {
        RecentDocuments.Remove(filePath); // Remove if exists
        RecentDocuments.Insert(0, filePath); // Add to top
        
        // Limit size
        while (RecentDocuments.Count > MaxRecentDocuments)
        {
            RecentDocuments.RemoveAt(RecentDocuments.Count - 1);
        }
    }
}
```

## Common Patterns

### Pattern 1: Complete MVVM Setup

```xml
<!-- MainWindow.xaml -->
<syncfusion:SfChromelessWindow 
    x:Class="DocumentEditor.MainWindow"
    xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
    WindowType="Tabbed">
    
    <Window.DataContext>
        <local:MainViewModel />
    </Window.DataContext>
    
    <syncfusion:SfTabControl 
        ItemsSource="{Binding OpenDocuments}"
        SelectedItem="{Binding SelectedDocument, Mode=TwoWay}"
        AllowDragDrop="True">
        
        <syncfusion:SfTabControl.ItemContainerStyle>
            <Style TargetType="syncfusion:SfTabItem">
                <Setter Property="CloseButtonVisibility" Value="Visible"/>
                <Setter Property="HeaderTemplate">
                    <Setter.Value>
                        <DataTemplate>
                            <TextBlock Text="{Binding DisplayTitle}"/>
                        </DataTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </syncfusion:SfTabControl.ItemContainerStyle>
        
        <syncfusion:SfTabControl.ContentTemplate>
            <DataTemplate>
                <TextBox Text="{Binding Content, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"
                        AcceptsReturn="True"
                        FontFamily="Consolas"/>
            </DataTemplate>
        </syncfusion:SfTabControl.ContentTemplate>
        
    </syncfusion:SfTabControl>
    
</syncfusion:SfChromelessWindow>
```

### Pattern 2: Master-Detail with Tabs

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="200"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    
    <!-- Document list -->
    <ListBox Grid.Column="0" 
            ItemsSource="{Binding AllDocuments}"
            SelectedItem="{Binding SelectedDocument}">
        <ListBox.ItemTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding Title}"/>
            </DataTemplate>
        </ListBox.ItemTemplate>
    </ListBox>
    
    <!-- Tabbed editor -->
    <syncfusion:SfTabControl Grid.Column="1" 
                            ItemsSource="{Binding OpenDocuments}">
        <!-- Templates -->
    </syncfusion:SfTabControl>
</Grid>
```

## Troubleshooting

### Tabs Not Appearing

**Problem:** ItemsSource is bound but no tabs appear

**Solutions:**
1. Verify ObservableCollection has items
2. Check DataContext is set correctly
3. Ensure HeaderTemplate or default header display is configured
4. Verify ItemContainerStyle doesn't hide tabs

```csharp
// Debug: Check if binding is working
Console.WriteLine($"Tab count: {OpenTabs.Count}");
```

### Updates Not Reflecting in UI

**Problem:** Changing model properties doesn't update UI

**Solutions:**
1. Implement INotifyPropertyChanged on model
2. Raise PropertyChanged event
3. Use ObservableCollection for collection changes

```csharp
// Correct implementation
public string Title
{
    get => _title;
    set
    {
        _title = value;
        OnPropertyChanged(nameof(Title)); // Essential!
    }
}
```

### SelectedItem Binding Not Working

**Problem:** SelectedItem binding doesn't update

**Solution:** Use TwoWay binding mode:

```xml
<syncfusion:SfTabControl 
    ItemsSource="{Binding OpenTabs}"
    SelectedItem="{Binding SelectedTab, Mode=TwoWay}">
```

### Memory Leaks with Event Handlers

**Problem:** Tab models not garbage collected

**Solution:** Unsubscribe from events:

```csharp
public class DocumentTab : INotifyPropertyChanged, IDisposable
{
    public void Dispose()
    {
        // Unsubscribe from events
        PropertyChanged = null;
        AutoSaveTimer?.Dispose();
    }
}
```

This comprehensive guide enables you to build powerful, maintainable data-driven tabbed applications using the MVVM pattern!

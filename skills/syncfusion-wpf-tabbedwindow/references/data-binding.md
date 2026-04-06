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

Bind `ItemsSource` to an ObservableCollection. Each item becomes a tab:

```csharp
public class MainViewModel
{
    public ObservableCollection<TabItem> Tabs { get; set; } = new();
}
```

```xml
<syncfusion:SfTabControl ItemsSource="{Binding Tabs}"/>
```

Set DataContext in XAML or code-behind for binding to work.

## Creating Tab Models

Create models with Header and Content properties. For complex scenarios, implement INotifyPropertyChanged and add properties like IsModified, FilePath, and IconPath for state tracking.

## ViewModel Implementation

Create ViewModels with ObservableCollection<TabModel> Tabs and TabModel SelectedTab properties. Implement ICommand for tab operations (AddTab, CloseTab, etc.) using RelayCommand or MVVMLight.

## Header and Content Templates

Define HeaderTemplate and ContentTemplate to customize tab appearance:

```xml
<syncfusion:SfTabControl ItemsSource="{Binding Tabs}">
    <syncfusion:SfTabControl.ItemContainerStyle>
        <Style TargetType="syncfusion:SfTabItem">
            <Setter Property="HeaderTemplate">
                <Setter.Value>
                    <DataTemplate>
                        <TextBlock Text="{Binding Title}"/>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </syncfusion:SfTabControl.ItemContainerStyle>
    <syncfusion:SfTabControl.ContentTemplate>
        <DataTemplate>
            <TextBox Text="{Binding Content, Mode=TwoWay}" AcceptsReturn="True"/>
        </DataTemplate>
    </syncfusion:SfTabControl.ContentTemplate>
</syncfusion:SfTabControl>
```

For icons or close buttons, extend HeaderTemplate with StackPanel containing Image or Button elements.

## ItemContainerStyle Configuration

Configure ItemContainerStyle to set properties like CloseButtonVisibility, MinWidth, MaxWidth. Use DataTriggers to conditionally apply styling based on tab properties (IsModified, IsReadOnly, etc.).

## Dynamic Collection Updates

ObservableCollection automatically updates UI when items are added/removed. Add tabs with `Tabs.Add(newTab)` and remove with `Tabs.Remove(tab)`. Always update SelectedTab if the removed tab was active.

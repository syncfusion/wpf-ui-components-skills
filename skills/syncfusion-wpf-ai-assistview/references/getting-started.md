# Getting Started with SfAIAssistView

## Table of Contents
- [Installation](#installation)
- [Assembly References](#assembly-references)
- [XAML Namespace Setup](#xaml-namespace-setup)
- [Basic ViewModel](#basic-viewmodel)
- [Binding to SfAIAssistView](#binding-to-sfaiassistview)
- [Theme Integration](#theme-integration)

---

## Installation

Install the NuGet package in your WPF project:

```
Syncfusion.SfChat.Wpf
```

Or via Package Manager Console:
```powershell
Install-Package Syncfusion.SfChat.Wpf
```

---

## Assembly References

If adding assemblies manually (without NuGet), reference:

```
Syncfusion.SfChat.WPF.dll
Syncfusion.Shared.WPF.dll
```

---

## XAML Namespace Setup

Add the Syncfusion namespace to your Window or UserControl:

```xaml
<Window
    x:Class="MyApp.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="clr-namespace:MyApp"
    xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Chat;assembly=Syncfusion.SfChat.WPF">
```

---

## Basic ViewModel

Create a ViewModel with the required properties. The `Messages` property must be `ObservableCollection<object>` — this is important because it holds different message types (`TextMessage`, `AIMessage`, etc.).

```csharp
using System.Collections.ObjectModel;
using System.ComponentModel;
using Syncfusion.UI.Xaml.Chat;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> chats;
    private Author currentUser;

    public ObservableCollection<object> Chats
    {
        get => chats;
        set { chats = value; RaisePropertyChanged(nameof(Chats)); }
    }

    public Author CurrentUser
    {
        get => currentUser;
        set { currentUser = value; RaisePropertyChanged(nameof(CurrentUser)); }
    }

    public ViewModel()
    {
        CurrentUser = new Author { Name = "User" };
        Chats = new ObservableCollection<object>();

        // Seed initial messages
        Chats.Add(new TextMessage
        {
            Author = CurrentUser,
            Text = "What is WPF?"
        });
        Chats.Add(new TextMessage
        {
            Author = new Author { Name = "AI" },
            Text = "WPF (Windows Presentation Foundation) is a UI framework for building Windows desktop applications."
        });
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void RaisePropertyChanged(string propName)
        => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
}
```

**Key types:**
- `Author` — represents a participant. Set `Name` (displayed in UI) and optionally `ContentTemplate` for custom avatars.
- `TextMessage` — a standard chat message with `Author`, `Text`, and optional `DateTime`.
- `ObservableCollection<object>` — required type for `Messages` binding (not a typed collection).

---

## Binding to SfAIAssistView

Set the ViewModel as `DataContext` and bind the required properties:

```xaml
<Window ...>
    <Grid>
        <Grid.DataContext>
            <local:ViewModel />
        </Grid.DataContext>

        <syncfusion:SfAIAssistView
            Messages="{Binding Chats}"
            CurrentUser="{Binding CurrentUser}" />
    </Grid>
</Window>
```

**Minimum required bindings:**
- `Messages` — the chat collection
- `CurrentUser` — identifies which messages are "yours" (right-aligned) vs AI (left-aligned)

---

## Theme Integration

Apply a Syncfusion theme using `SfSkinManager`:

```xaml
<!-- Add reference: Syncfusion.SfSkinManager.WPF -->
<Window xmlns:syncfusion="clr-namespace:Syncfusion.UI.Xaml.Chat;assembly=Syncfusion.SfChat.WPF"
        xmlns:skinManager="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF">

    <syncfusion:SfAIAssistView
        syncfusion:SfSkinManager.Theme="{syncfusion:SkinManagerExtension ThemeName=FluentDark}"
        Messages="{Binding Chats}"
        CurrentUser="{Binding CurrentUser}" />
</Window>
```

Available themes: `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Office2019Colorful`, and others.

---

## Gotchas

- **`Messages` must be `ObservableCollection<object>`** — a typed `ObservableCollection<TextMessage>` won't work because the control expects to hold multiple message types.
- **`CurrentUser` reference must match** — use the same `Author` instance (or same `Name`) for messages sent by the user to ensure correct alignment.
- **Messages sent by `CurrentUser`** appear on the right; all others appear on the left.

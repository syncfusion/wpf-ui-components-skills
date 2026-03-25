# Input Toolbar

## Table of Contents
- [Overview](#overview)
- [Enabling the Input Toolbar](#enabling-the-input-toolbar)
- [InputToolbarItem Configuration](#inputtoolbaritem-configuration)
- [InputToolbarPosition](#inputtoolbarposition)
- [InputToolbarItemClicked Event](#inputtoolbaritemclicked-event)
- [InputToolbarHeaderTemplate](#inputtoolbarheadertemplate)

---

## Overview

The input toolbar is an optional customizable toolbar displayed in the message input area of `SfAIAssistView`. Use it to add action buttons (e.g., attach file, send image, clear chat) alongside the text input field.

By default, `IsInputToolbarVisible` is `false` — you must opt in explicitly.

---

## Enabling the Input Toolbar

```xaml
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    IsInputToolbarVisible="True" />
```

Or in code-behind:
```csharp
sfAIAssistView.IsInputToolbarVisible = true;
```

---

## InputToolbarItem Configuration

Add items to the toolbar by populating `InputToolbarItems` with `InputToolbarItem` objects:

```xaml
<syncfusion:SfAIAssistView
    Messages="{Binding Chats}"
    CurrentUser="{Binding CurrentUser}"
    IsInputToolbarVisible="True">
    <syncfusion:SfAIAssistView.InputToolbarItems>
        <syncfusion:InputToolbarItem
            Tooltip="Attach File"
            IsEnabled="True"
            Visible="True">
            <syncfusion:InputToolbarItem.ItemTemplate>
                <DataTemplate>
                    <Button Content="📎" ToolTip="Attach File" Background="Transparent" />
                </DataTemplate>
            </syncfusion:InputToolbarItem.ItemTemplate>
        </syncfusion:InputToolbarItem>

        <syncfusion:InputToolbarItem
            Tooltip="Clear Chat"
            IsEnabled="True"
            Visible="True">
            <syncfusion:InputToolbarItem.ItemTemplate>
                <DataTemplate>
                    <Button Content="🗑" ToolTip="Clear Chat" Background="Transparent" />
                </DataTemplate>
            </syncfusion:InputToolbarItem.ItemTemplate>
        </syncfusion:InputToolbarItem>
    </syncfusion:SfAIAssistView.InputToolbarItems>
</syncfusion:SfAIAssistView>
```

**`InputToolbarItem` properties:**

| Property | Type | Description |
|---|---|---|
| `ItemTemplate` | `DataTemplate` | Custom visual for the toolbar button |
| `IsEnabled` | `bool` | Enables/disables the item |
| `Visible` | `bool` | Shows/hides the item |
| `Tooltip` | `string` | Tooltip text shown on hover |

---

## InputToolbarPosition

Control whether the toolbar appears to the left or right of the input field:

```xaml
<syncfusion:SfAIAssistView
    IsInputToolbarVisible="True"
    InputToolbarPosition="Left" />
```

```xaml
<syncfusion:SfAIAssistView
    IsInputToolbarVisible="True"
    InputToolbarPosition="Right" />
```

- `Left` — toolbar items appear before the input field
- `Right` — toolbar items appear after the input field (default)

---

## InputToolbarItemClicked Event

Handle button clicks in the toolbar:

```xaml
<syncfusion:SfAIAssistView
    IsInputToolbarVisible="True"
    InputToolbarItemClicked="OnInputToolbarItemClicked" />
```

```csharp
private void OnInputToolbarItemClicked(object sender, InputToolbarItemClickedEventArgs e)
{
    // e.Item — the InputToolbarItem that was clicked
    var clickedItem = e.Item;

    if (clickedItem.Tooltip == "Attach File")
    {
        // Open file dialog
        var dialog = new OpenFileDialog();
        if (dialog.ShowDialog() == true)
        {
            // Process selected file
        }
    }
    else if (clickedItem.Tooltip == "Clear Chat")
    {
        viewModel.Chats.Clear();
    }
}
```

---

## InputToolbarHeaderTemplate

Use `InputToolbarHeaderTemplate` to add a custom header above the input area — useful for showing file attachment previews or context banners:

```xaml
<syncfusion:SfAIAssistView
    IsInputToolbarVisible="True"
    InputToolbarHeaderTemplate="{StaticResource FilePreviewHeaderTemplate}">
</syncfusion:SfAIAssistView>
```

```xaml
<Window.Resources>
    <DataTemplate x:Key="FilePreviewHeaderTemplate">
        <Border Background="#F0F0F0" Padding="8" Margin="4">
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="📎" FontSize="16" VerticalAlignment="Center" />
                <TextBlock Text="{Binding SelectedFileName}"
                           Margin="6,0,0,0"
                           VerticalAlignment="Center" />
                <Button Content="✕"
                        Margin="8,0,0,0"
                        Click="RemoveAttachment_Click"
                        Background="Transparent" />
            </StackPanel>
        </Border>
    </DataTemplate>
</Window.Resources>
```

The header template is displayed above the text input field, useful for file attachment previews or contextual action confirmation.

---

## Gotchas

- **`IsInputToolbarVisible` defaults to `false`** — the toolbar is not shown unless explicitly enabled.
- **`InputToolbarItem.Visible` vs `IsEnabled`** — `Visible=False` hides the item entirely; `IsEnabled=False` shows it grayed out.
- **`InputToolbarHeaderTemplate` is independent** — you can show the header template even without `InputToolbarItems`.
- **MVVM for click handling** — for MVVM, bind each `InputToolbarItem`'s button `Command` to a ViewModel command inside the `ItemTemplate` instead of using the `InputToolbarItemClicked` code-behind event.

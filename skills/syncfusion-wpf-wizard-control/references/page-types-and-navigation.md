# Page Types & Navigation Buttons in WPF WizardControl

## Table of Contents
- [PageType Overview](#pagetype-overview)
- [Blank Page Type](#blank-page-type)
- [Interior Page Type](#interior-page-type)
- [Exterior Page Type](#exterior-page-type)
- [Enabling and Disabling Navigation Buttons](#enabling-and-disabling-navigation-buttons)
- [Showing and Hiding Navigation Buttons](#showing-and-hiding-navigation-buttons)
- [Custom Button Label Text](#custom-button-label-text)
- [Closing the Window on Finish or Cancel](#closing-the-window-on-finish-or-cancel)
- [Per-Page vs Global Button Control](#per-page-vs-global-button-control)

---

## PageType Overview

Every `WizardPage` has a `PageType` property that controls how the banner image and header are
laid out. Choose the type that matches the page's role in the wizard flow.

| PageType | Banner Position | Typical Use |
|---|---|---|
| `Blank` | None | Plain content page with no header decoration |
| `Interior` | Top (horizontal strip) | Middle steps — configuration, input, selection |
| `Exterior` | Left side (vertical panel) | Welcome and Finish pages |

```xaml
<syncfusion:WizardPage PageType="Interior"/>
<!-- or -->
<syncfusion:WizardPage PageType="Exterior"/>
<!-- or -->
<syncfusion:WizardPage PageType="Blank"/>
```

```csharp
wizardPage.PageType = WizardPageType.Interior;
wizardPage.PageType = WizardPageType.Exterior;
wizardPage.PageType = WizardPageType.Blank;
```

---

## Blank Page Type

A Blank page has no banner region — the entire page area is free content space. Use it when you
want a plain dialog-style step without any header decoration.

```xaml
<syncfusion:WizardPage Name="plainPage"
                       Title="Options"
                       PageType="Blank">
    <!-- Your content here -->
    <StackPanel Margin="20">
        <CheckBox Content="Enable feature A"/>
        <CheckBox Content="Enable feature B"/>
    </StackPanel>
</syncfusion:WizardPage>
```

---

## Interior Page Type

Interior pages display a horizontal banner strip across the top. The `BannerImage` appears in
this strip alongside the `Title` and `Description`.

```xaml
<syncfusion:WizardPage Name="configPage"
                       Title="Configuration"
                       Description="Choose your installation settings."
                       PageType="Interior"
                       BannerImage="Images/config-banner.png"
                       BannerBackground="WhiteSmoke">
    <!-- Step content -->
    <Grid Margin="20">
        <TextBlock Text="Select installation directory:"/>
    </Grid>
</syncfusion:WizardPage>
```

```csharp
WizardPage configPage = new WizardPage();
configPage.Title = "Configuration";
configPage.Description = "Choose your installation settings.";
configPage.PageType = WizardPageType.Interior;
configPage.BannerBackground = Brushes.WhiteSmoke;
```

---

## Exterior Page Type

Exterior pages display the banner image as a tall vertical panel on the left side. This is the
classic welcome/finish page style.

```xaml
<syncfusion:WizardPage Name="welcomePage"
                       Title="Welcome"
                       Description="Welcome to the installation wizard."
                       PageType="Exterior"
                       BannerImage="Images/welcome-left.png">
    <StackPanel Margin="20,10">
        <TextBlock Text="This wizard will guide you through installation."
                   TextWrapping="Wrap"/>
    </StackPanel>
</syncfusion:WizardPage>
```

```csharp
WizardPage welcomePage = new WizardPage();
welcomePage.PageType = WizardPageType.Exterior;
welcomePage.BannerImage = new BitmapImage(
    new Uri("Images/welcome-left.png", UriKind.RelativeOrAbsolute));
```

---

## Enabling and Disabling Navigation Buttons

Use the `*Enabled` properties to allow or prevent the user from clicking a button. A disabled
button is visible but grayed out.

**On WizardControl (applies to all pages unless overridden):**
```xaml
<syncfusion:WizardControl BackEnabled="True"
                          NextEnabled="True"
                          FinishEnabled="True"
                          CancelEnabled="True">
```

**On a specific WizardPage (overrides the control-level setting for that page):**
```xaml
<!-- Disable Next until user fills in required fields -->
<syncfusion:WizardPage Name="inputPage"
                       Title="Enter Details"
                       NextEnabled="False"/>
```

**C# — toggle dynamically:**
```csharp
// Called when user completes required input
private void OnInputComplete()
{
    inputPage.NextEnabled = true;
}
```

> **Tip:** Disabling `NextEnabled` is the standard pattern for "gating" a page — the user must
> complete a form before they can proceed. Re-enable it in a validation callback.

---

## Showing and Hiding Navigation Buttons

Use the `*Visible` properties to show or hide buttons entirely. Hidden buttons take no space.

**On WizardControl:**
```xaml
<syncfusion:WizardControl HelpVisible="True"
                          CancelVisible="True"
                          BackVisible="True"
                          NextVisible="True"
                          FinishVisible="True">
```

**Per-page button visibility — common wizard pattern:**
```xaml
<!-- First page: no Back button, no Finish -->
<syncfusion:WizardPage Title="Welcome"
                       PageType="Exterior"
                       BackVisible="False"
                       FinishVisible="False"
                       NextVisible="True"/>

<!-- Middle pages: Back and Next, no Finish -->
<syncfusion:WizardPage Title="Step 2"
                       PageType="Interior"
                       BackVisible="True"
                       NextVisible="True"
                       FinishVisible="False"/>

<!-- Last page: Back and Finish, no Next -->
<syncfusion:WizardPage Title="Complete"
                       PageType="Exterior"
                       BackVisible="True"
                       NextVisible="False"
                       FinishVisible="True"/>
```

---

## Custom Button Label Text

Override the default button labels using the `*Text` properties on `WizardControl`. This is
useful for localization or domain-specific language (e.g., "Previous" instead of "Back").

> **Note:** Custom text applies to WizardControl-level buttons only. WizardPage-level buttons
> do not have separate text properties.

```xaml
<syncfusion:WizardControl BackText="Previous"
                          NextText="Continue"
                          FinishText="Install"
                          CancelText="Quit"
                          HelpText="?">
```

```csharp
wizardControl.BackText   = "Previous";
wizardControl.NextText   = "Continue";
wizardControl.FinishText = "Install";
wizardControl.CancelText = "Quit";
wizardControl.HelpText   = "?";
```

---

## Closing the Window on Finish or Cancel

By default, the Finish and Cancel buttons do not close the wizard window. Enable these properties
to get automatic window-close behavior:

```xaml
<syncfusion:WizardControl FinishButtonClosesWindow="True"
                          CancelButtonCancelsWindow="True">
```

```csharp
wizardControl.FinishButtonClosesWindow  = true;
wizardControl.CancelButtonCancelsWindow = true;
```

> **When to leave these false:** If you need to execute logic (save data, show a confirmation
> dialog) before closing, leave these as `false` and handle the `Finish`/`Cancel` events manually:

```xaml
<syncfusion:WizardControl Finish="OnWizardFinish" ...>
```

```csharp
private void OnWizardFinish(object sender, RoutedEventArgs e)
{
    SaveSettings();
    this.Close();
}
```

---

## Per-Page vs Global Button Control

Navigation button properties exist on both `WizardControl` (global) and `WizardPage` (per-page).
The **per-page setting takes precedence** over the global setting for that page.

| Scope | When to use |
|---|---|
| `WizardControl.*Enabled/Visible` | Default state for all pages |
| `WizardPage.*Enabled/Visible` | Override for a specific page only |

**Example — globally hide Help, but show it on one specific page:**
```xaml
<syncfusion:WizardControl HelpVisible="False">
    <syncfusion:WizardPage Title="Step 1"/>
    <syncfusion:WizardPage Title="Help Needed" HelpVisible="True"/>
    <syncfusion:WizardPage Title="Step 3"/>
</syncfusion:WizardControl>
```

---
name: syncfusion-wpf-wizard-control
description: Implement Syncfusion WPF WizardControl for step-by-step wizard UIs. Use this when building multi-page wizard flows, installation wizards, or setup wizard interfaces in WPF. Covers adding WizardPages, setting page types (Blank, Interior, Exterior), controlling navigation buttons (Back, Next, Finish, Cancel, Help), ItemsSource data binding, non-linear navigation, and banner image customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF WizardControl

**WizardControl** is Syncfusion's WPF wizard UI, modelled after the familiar installation wizard
pattern. It hosts one or more **WizardPage** items and automatically manages Back/Next/Finish/Cancel
navigation. Each page's type, banner, and button states are independently configurable. Pages can
be added declaratively or bound from a data source.

**Assemblies required:**
- `Syncfusion.Shared.WPF`
- `Syncfusion.Tools.WPF`

**Namespace:** `Syncfusion.Windows.Tools.Controls`  
**XAML schema:** `http://schemas.syncfusion.com/wpf`

---

## When to Use This Skill

- Creating a multi-step wizard UI (setup, onboarding, configuration, checkout)
- Adding and configuring `WizardPage` items inside `WizardControl`
- Controlling which navigation buttons appear or are enabled per page
- Implementing non-linear page flow (skip steps, branch on user input)
- Binding wizard pages dynamically from a collection via `ItemsSource`
- Customizing banner images, banner colors, and page headers
- Applying themes or customizing the wizard's visual appearance

---

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly and NuGet setup
- Adding WizardControl via designer, XAML, or C#
- Adding multiple WizardPages
- Theme support overview

### Page Types & Navigation Buttons
📄 **Read:** [references/page-types-and-navigation.md](references/page-types-and-navigation.md)
- `PageType` enum: Blank, Interior, Exterior
- Enabling/disabling Back, Next, Finish, Cancel buttons
- Showing/hiding navigation buttons
- Custom button label text
- `FinishButtonClosesWindow` / `CancelButtonCancelsWindow`

### Interactive Features
📄 **Read:** [references/interactive-features.md](references/interactive-features.md)
- Populating pages declaratively vs. via `ItemsSource` data binding
- `ItemContainerStyle` and `ItemTemplate` for bound pages
- `SelectedWizardPage` — programmatic page selection
- `Title` and `Description` on WizardPage
- `NextPage` / `PreviousPage` for non-linear navigation
- `Next` event handler

### Layout & Appearance
📄 **Read:** [references/layout-and-appearance.md](references/layout-and-appearance.md)
- `InteriorPageHeaderMinHeight`
- `BannerBackground` color
- `BannerImage` on WizardPage
- `ExteriorPageBannerImageMinWidth`
- Applying built-in themes via `SfSkinManager`
- Custom themes via Theme Studio

---

## Quick Start

### Minimal XAML Setup

```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf" ...>
    <Grid>
        <syncfusion:WizardControl Name="wizard"
                                  FinishButtonClosesWindow="True"
                                  CancelButtonCancelsWindow="True">
            <syncfusion:WizardPage Title="Welcome"
                                   Description="Introduction to the setup process."
                                   PageType="Exterior"
                                   BannerImage="Images/banner.png"/>
            <syncfusion:WizardPage Title="Configuration"
                                   Description="Configure your settings."
                                   PageType="Interior"/>
            <syncfusion:WizardPage Title="Finish"
                                   Description="Setup is complete."
                                   PageType="Exterior"/>
        </syncfusion:WizardControl>
    </Grid>
</Window>
```

### Minimal C# Setup

```csharp
using Syncfusion.Windows.Tools.Controls;

WizardControl wizard = new WizardControl();
wizard.FinishButtonClosesWindow = true;
wizard.CancelButtonCancelsWindow = true;

WizardPage page1 = new WizardPage { Title = "Welcome",       PageType = WizardPageType.Exterior };
WizardPage page2 = new WizardPage { Title = "Configuration", PageType = WizardPageType.Interior };
WizardPage page3 = new WizardPage { Title = "Finish",        PageType = WizardPageType.Exterior };

wizard.Items.Add(page1);
wizard.Items.Add(page2);
wizard.Items.Add(page3);

this.Content = wizard;
```

---

## Common Patterns

### Control Navigation Button Visibility Per Page

```xaml
<!-- First page: hide Back, show Next -->
<syncfusion:WizardPage Title="Step 1" BackVisible="False" NextVisible="True" FinishVisible="False"/>

<!-- Middle page: show Back and Next -->
<syncfusion:WizardPage Title="Step 2" BackVisible="True" NextVisible="True" FinishVisible="False"/>

<!-- Last page: show Back and Finish, hide Next -->
<syncfusion:WizardPage Title="Step 3" BackVisible="True" NextVisible="False" FinishVisible="True"/>
```

### Non-Linear Navigation (Skip a Step)

```xaml
<syncfusion:WizardControl Name="wizard">
    <syncfusion:WizardPage x:Name="step1" Title="Step 1"
                           NextPage="{Binding ElementName=step3}"/>
    <syncfusion:WizardPage x:Name="step2" Title="Step 2"/>
    <syncfusion:WizardPage x:Name="step3" Title="Step 3"
                           PreviousPage="{Binding ElementName=step1}"/>
</syncfusion:WizardControl>
```

### Handle Next Button Click

```xaml
<syncfusion:WizardControl Next="Wizard_Next" ...>
```

```csharp
private void Wizard_Next(object sender, RoutedEventArgs e)
{
    var wiz = (WizardControl)sender;
    // Validate current page before allowing navigation
    if (!ValidateCurrentPage())
        e.Handled = true;  // Cancel navigation
}
```

---

## Key Properties

### WizardControl Properties

| Property | Type | Description |
|---|---|---|
| `SelectedWizardPage` | WizardPage | Currently displayed page |
| `ItemsSource` | IEnumerable | Bind pages from a collection |
| `ItemTemplate` | DataTemplate | Template for bound page content |
| `ItemContainerStyle` | Style | Style applied to each WizardPage container |
| `BackEnabled` | bool | Enable/disable Back button globally |
| `NextEnabled` | bool | Enable/disable Next button globally |
| `FinishEnabled` | bool | Enable/disable Finish button globally |
| `CancelEnabled` | bool | Enable/disable Cancel button globally |
| `BackVisible` | bool | Show/hide Back button globally |
| `NextVisible` | bool | Show/hide Next button globally |
| `FinishVisible` | bool | Show/hide Finish button globally |
| `HelpVisible` | bool | Show/hide Help button globally |
| `CancelVisible` | bool | Show/hide Cancel button globally |
| `BackText` | string | Custom label for Back button |
| `NextText` | string | Custom label for Next button |
| `FinishText` | string | Custom label for Finish button |
| `HelpText` | string | Custom label for Help button |
| `CancelText` | string | Custom label for Cancel button |
| `FinishButtonClosesWindow` | bool | Close window on Finish click |
| `CancelButtonCancelsWindow` | bool | Close window on Cancel click |
| `InteriorPageHeaderMinHeight` | double | Min header height for Interior pages |
| `ExteriorPageBannerImageMinWidth` | double | Min banner image width for Exterior pages |

### WizardPage Properties

| Property | Type | Description |
|---|---|---|
| `Title` | string | Page title shown in header |
| `Description` | string | Page description shown in header |
| `PageType` | WizardPageType | Blank, Interior, or Exterior |
| `BannerImage` | ImageSource | Banner image for the page |
| `BannerBackground` | Brush | Banner area background color |
| `NextPage` | WizardPage | Override the default next page |
| `PreviousPage` | WizardPage | Override the default previous page |
| `BackEnabled` | bool | Enable/disable Back on this page |
| `NextEnabled` | bool | Enable/disable Next on this page |
| `FinishEnabled` | bool | Enable/disable Finish on this page |
| `CancelEnabled` | bool | Enable/disable Cancel on this page |
| `BackVisible` | bool | Show/hide Back on this page |
| `NextVisible` | bool | Show/hide Next on this page |
| `FinishVisible` | bool | Show/hide Finish on this page |
| `HelpVisible` | bool | Show/hide Help on this page |
| `CancelVisible` | bool | Show/hide Cancel on this page |

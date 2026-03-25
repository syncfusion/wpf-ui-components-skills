# Getting Started with WPF WizardControl

## Table of Contents
- [Assembly Setup](#assembly-setup)
- [Adding via Designer](#adding-via-designer)
- [Adding via XAML](#adding-via-xaml)
- [Adding via C#](#adding-via-c)
- [Adding Multiple Pages](#adding-multiple-pages)
- [Theme Support](#theme-support)

---

## Assembly Setup

**Required assemblies:**
- `Syncfusion.Shared.WPF`
- `Syncfusion.Tools.WPF`

Install via NuGet:
```
Install-Package Syncfusion.Tools.WPF
```

Or add direct references to both DLL files.

**XAML namespace declaration:**
```xaml
xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
```

Or using the control namespace directly:
```xaml
xmlns:syncfusion="clr-namespace:Syncfusion.Windows.Tools.Controls;assembly=Syncfusion.Tools.WPF"
```

---

## Adding via Designer

Drag **WizardControl** from the Visual Studio Toolbox and drop it onto the designer canvas.

- Both required assemblies (`Syncfusion.Shared.WPF` and `Syncfusion.Tools.WPF`) are added automatically.
- The WizardControl appears on the form and is ready to configure.

---

## Adding via XAML

Add assembly references, import the namespace, then declare the control:

```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        Title="Setup Wizard" Height="450" Width="800">
    <Grid>
        <syncfusion:WizardControl Name="wizardControl"/>
    </Grid>
</Window>
```

This creates an empty wizard. Add `WizardPage` children to populate it (see [Adding Multiple Pages](#adding-multiple-pages)).

---

## Adding via C#

Import the namespace, create the control instance, and assign it to the window content:

```csharp
using Syncfusion.Windows.Tools.Controls;

public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        WizardControl wizardControl = new WizardControl();
        this.Content = wizardControl;
    }
}
```

Or add it to an existing panel:
```csharp
// Where 'Root' is a named Grid or Panel in XAML
WizardControl wizardControl = new WizardControl();
Root.Children.Add(wizardControl);
```

---

## Adding Multiple Pages

Pages are added as `WizardPage` children of `WizardControl`. The Back/Next/Cancel/Finish
buttons automatically enable and disable based on the current page position.

**XAML:**
```xaml
<syncfusion:WizardControl Name="wizardControl"
                          FinishButtonClosesWindow="True"
                          CancelButtonCancelsWindow="True">
    <syncfusion:WizardPage Name="welcomePage"
                           Title="Welcome"
                           Description="Welcome to the Setup Wizard."
                           PageType="Exterior"
                           BannerImage="Images/welcome-banner.png"/>
    <syncfusion:WizardPage Name="configPage"
                           Title="Configuration"
                           Description="Choose your installation options."
                           PageType="Interior"/>
    <syncfusion:WizardPage Name="finishPage"
                           Title="Complete"
                           Description="Installation is complete."
                           PageType="Exterior"
                           BannerImage="Images/finish-banner.png"/>
</syncfusion:WizardControl>
```

**C#:**
```csharp
WizardControl wizardControl = new WizardControl();

WizardPage welcomePage = new WizardPage();
welcomePage.Title = "Welcome";
welcomePage.Description = "Welcome to the Setup Wizard.";
welcomePage.PageType = WizardPageType.Exterior;

WizardPage configPage = new WizardPage();
configPage.Title = "Configuration";
configPage.Description = "Choose your installation options.";
configPage.PageType = WizardPageType.Interior;

WizardPage finishPage = new WizardPage();
finishPage.Title = "Complete";
finishPage.Description = "Installation is complete.";
finishPage.PageType = WizardPageType.Exterior;

wizardControl.Items.Add(welcomePage);
wizardControl.Items.Add(configPage);
wizardControl.Items.Add(finishPage);

wizardControl.FinishButtonClosesWindow = true;
wizardControl.CancelButtonCancelsWindow = true;

this.Content = wizardControl;
```

> **Navigation behavior:** On the first page, the Back button is automatically disabled.
> On the last page, the Finish button becomes active. This is handled automatically — no
> additional code is needed for standard linear navigation.

---

## Theme Support

WizardControl supports Syncfusion's built-in theme system. Apply a theme using `SfSkinManager`:

```xaml
<Window xmlns:skin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        skin:SfSkinManager.Theme="{skin:SkinManagerExtension ThemeName=FluentLight}">
    <Grid>
        <syncfusion:WizardControl Name="wizardControl"/>
    </Grid>
</Window>
```

```csharp
using Syncfusion.SfSkinManager;
SfSkinManager.SetTheme(this, new Theme("FluentLight"));
```

> For banner images, custom styles, and layout customization, see
> [layout-and-appearance.md](layout-and-appearance.md).

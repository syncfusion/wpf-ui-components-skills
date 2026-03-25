# Layout & Appearance in WPF WizardControl

## Table of Contents
- [Interior Page Header Min Height](#interior-page-header-min-height)
- [Banner Background Color](#banner-background-color)
- [Banner Image on WizardPage](#banner-image-on-wizardpage)
- [Exterior Page Banner Image Min Width](#exterior-page-banner-image-min-width)
- [Applying Built-in Themes](#applying-built-in-themes)
- [Custom Themes via Theme Studio](#custom-themes-via-theme-studio)

---

## Interior Page Header Min Height

The `InteriorPageHeaderMinHeight` property sets the minimum height of the header area on
Interior-type wizard pages. Use this to ensure the banner area is tall enough to properly
display a banner image or title.

```xaml
<syncfusion:WizardControl Name="wizardControl"
                          InteriorPageHeaderMinHeight="150">
    <syncfusion:WizardPage Name="configPage"
                           PageType="Interior"
                           BannerImage="Images/config-banner.png"/>
</syncfusion:WizardControl>
```

```csharp
wizardControl.InteriorPageHeaderMinHeight = 150;
```

> **Default behavior:** The header auto-sizes to fit its content. Set `InteriorPageHeaderMinHeight`
> when your banner image is taller than the default or when you want a consistent header height
> across all Interior pages regardless of content.

---

## Banner Background Color

The `BannerBackground` property sets the background color of the banner area on a `WizardPage`.
This applies to both Interior (top banner) and Exterior (left panel) page types.

```xaml
<syncfusion:WizardControl Name="wizardControl">
    <syncfusion:WizardPage Name="step1"
                           PageType="Interior"
                           BannerBackground="Navy"/>
    <syncfusion:WizardPage Name="step2"
                           PageType="Exterior"
                           BannerBackground="#FF1E3A5F"/>
</syncfusion:WizardControl>
```

```csharp
WizardPage step1 = new WizardPage();
step1.PageType        = WizardPageType.Interior;
step1.BannerBackground = Brushes.Navy;

WizardPage step2 = new WizardPage();
step2.PageType        = WizardPageType.Exterior;
step2.BannerBackground = new SolidColorBrush(Color.FromRgb(0x1E, 0x3A, 0x5F));
```

> **Tip:** Use a consistent `BannerBackground` color across all pages to give the wizard a
> unified brand appearance, even when individual pages use different images.

---

## Banner Image on WizardPage

Set `BannerImage` on a `WizardPage` to display an image in the banner area. The position
depends on `PageType`:
- **Interior:** Image appears in the horizontal top strip
- **Exterior:** Image appears in the vertical left panel

```xaml
<syncfusion:WizardControl Name="wizardControl">
    <!-- Exterior: image fills the left side panel -->
    <syncfusion:WizardPage Name="welcomePage"
                           PageType="Exterior"
                           BannerImage="Images/welcome-left.png"/>

    <!-- Interior: image appears in the top header strip -->
    <syncfusion:WizardPage Name="configPage"
                           PageType="Interior"
                           BannerImage="Images/config-top.png"/>
</syncfusion:WizardControl>
```

```csharp
WizardPage welcomePage = new WizardPage();
welcomePage.PageType    = WizardPageType.Exterior;
welcomePage.BannerImage = new BitmapImage(
    new Uri("Images/welcome-left.png", UriKind.RelativeOrAbsolute));

WizardPage configPage = new WizardPage();
configPage.PageType    = WizardPageType.Interior;
configPage.BannerImage = new BitmapImage(
    new Uri("Images/config-top.png", UriKind.RelativeOrAbsolute));
```

> **Note:** `BannerImage` has no visible effect on `PageType="Blank"` pages because Blank
> pages have no banner region.

**Using data binding (for bound pages):**
```xaml
<Style x:Key="WizardPageStyle" TargetType="syncfusion:WizardPage">
    <Setter Property="BannerImage" Value="{Binding BannerImagePath}"/>
    <Setter Property="PageType"    Value="Exterior"/>
</Style>
```

---

## Exterior Page Banner Image Min Width

`ExteriorPageBannerImageMinWidth` controls the minimum width of the left-side banner panel on
Exterior-type pages. Use this to ensure the banner image is never squashed too narrow.

```xaml
<syncfusion:WizardControl Name="wizardControl"
                          ExteriorPageBannerImageMinWidth="200">
    <syncfusion:WizardPage Name="welcomePage"
                           PageType="Exterior"
                           BannerImage="Images/welcome-left.png"/>
</syncfusion:WizardControl>
```

```csharp
wizardControl.ExteriorPageBannerImageMinWidth = 200;

WizardPage welcomePage = new WizardPage();
welcomePage.PageType    = WizardPageType.Exterior;
welcomePage.BannerImage = new BitmapImage(
    new Uri("Images/welcome-left.png", UriKind.RelativeOrAbsolute));
wizardControl.Items.Add(welcomePage);
```

> **Use case:** When the wizard window is resizable, this prevents the banner image from
> collapsing below a usable width when the user narrows the window.

---

## Applying Built-in Themes

WizardControl supports all Syncfusion WPF built-in themes via `SfSkinManager`.

**Step 1 — Install the theme NuGet package:**
```
Install-Package Syncfusion.Themes.FluentLight.WPF
```

**Step 2 — Apply in XAML at window level:**
```xaml
<Window xmlns:skin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        skin:SfSkinManager.Theme="{skin:SkinManagerExtension ThemeName=FluentLight}">
    <Grid>
        <syncfusion:WizardControl Name="wizardControl">
            <syncfusion:WizardPage Title="Step 1"/>
        </syncfusion:WizardControl>
    </Grid>
</Window>
```

**Or apply in code-behind:**
```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetTheme(this, new Theme("FluentDark"));
```

**Available themes (examples):**
- `FluentLight` / `FluentDark`
- `Material3Light` / `Material3Dark`
- `Office2019White` / `Office2019Colorful`
- `Windows11Light` / `Windows11Dark`

---

## Custom Themes via Theme Studio

Use Syncfusion's Theme Studio web tool to build a custom theme with your brand colors.

**Workflow:**
1. Open [Theme Studio](https://ej2.syncfusion.com/themestudio/?theme=material3)
2. Select a base theme and adjust colors, typography, border radius, etc.
3. Export and download the generated theme resource files
4. Add the `.xaml` resource dictionaries to your WPF project
5. Merge them in `App.xaml`:

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="Themes/CustomTheme/MSControls.Core.Implicit.xaml"/>
            <ResourceDictionary Source="Themes/CustomTheme/Syncfusion.Tools.WPF.xml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

> 📖 Full guide: [Theme Studio documentation](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)

# Styling & Themes in WPF ButtonAdv

ButtonAdv supports Syncfusion's full theme ecosystem and WPF's native template editing workflow.
You can apply built-in themes via `SfSkinManager`, create custom themes with Theme Studio,
or edit the control template directly in Expression Blend or Visual Studio.

## Table of Contents
- [Applying Built-in Themes via SfSkinManager](#applying-built-in-themes-via-sfSkinManager)
- [Creating Custom Themes with Theme Studio](#creating-custom-themes-with-theme-studio)
- [Editing Templates in Expression Blend](#editing-templates-in-expression-blend)
- [Editing Templates in Visual Studio](#editing-templates-in-visual-studio)
- [Edit a Copy vs Create Empty](#edit-a-copy-vs-create-empty)

---

## Applying Built-in Themes via SfSkinManager

`SfSkinManager` lets you apply any of Syncfusion's pre-built themes to ButtonAdv (and all
Syncfusion WPF controls) with a single property.

**Step 1 — Install the theme package:**
```
Install-Package Syncfusion.Themes.FluentDark.WPF
```
(Replace `FluentDark` with your chosen theme name.)

**Step 2 — Apply the theme in XAML:**
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf"
        xmlns:skin="clr-namespace:Syncfusion.SfSkinManager;assembly=Syncfusion.SfSkinManager.WPF"
        skin:SfSkinManager.Theme="{skin:SkinManagerExtension ThemeName=FluentDark}">

    <Grid>
        <syncfusion:ButtonAdv Label="Log in" SizeMode="Normal" SmallIcon="Images/user.png"/>
    </Grid>
</Window>
```

**Or apply per-control:**
```xaml
<syncfusion:ButtonAdv Label="Log in"
                      skin:SfSkinManager.Theme="{skin:SkinManagerExtension ThemeName=FluentLight}"/>
```

**Apply in code-behind:**
```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetTheme(this, new Theme("FluentDark"));
```

**Available built-in themes (examples):**
- `FluentLight` / `FluentDark`
- `Material3Light` / `Material3Dark`
- `Office2019White` / `Office2019Colorful`
- `Windows11Light` / `Windows11Dark`

> 📖 Full theme list: [SfSkinManager documentation](https://help.syncfusion.com/wpf/themes/skin-manager)

---

## Creating Custom Themes with Theme Studio

Theme Studio is a web-based tool for generating custom Syncfusion WPF themes with your brand colors.

**Workflow:**
1. Open [Theme Studio](https://ej2.syncfusion.com/themestudio/?theme=material3)
2. Choose a base theme
3. Adjust primary colors, fonts, border radius, etc.
4. Export and download the generated theme files
5. Add the exported `.xaml` resource dictionaries to your project
6. Merge them in `App.xaml`:

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="Themes/CustomTheme/MSControls.Core.Implicit.xaml"/>
            <ResourceDictionary Source="Themes/CustomTheme/Syncfusion.Shared.WPF.xml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

> 📖 Full guide: [Theme Studio documentation](https://help.syncfusion.com/wpf/themes/theme-studio#creating-custom-theme)

---

## Editing Templates in Expression Blend

Expression Blend provides a visual template editor. Use this when you need to change the button's
visual structure (e.g., rearranging icon/label layout, adding visual states).

**Steps:**
1. Open your WPF project in **Expression Blend**
2. Select the `ButtonAdv` control on the canvas
3. Right-click → **Edit Template** → choose an option:
   - **Edit a Copy...** — generates an editable copy of the default style (recommended starting point)
   - **Create Empty...** — creates a blank `ControlTemplate` from scratch
4. In the **Create Style Resource** dialog, name your style and choose where to save it
   (Application, Window, or a ResourceDictionary)
5. Click **OK** — the template XAML appears in the **Resources** panel
6. Edit visuals in Blend's designer or switch to XAML view

The generated resource will look similar to:
```xaml
<Style x:Key="CustomButtonAdvStyle" TargetType="{x:Type syncfusion:ButtonAdv}">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="{x:Type syncfusion:ButtonAdv}">
                <!-- Generated template here — edit as needed -->
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

Apply your custom style:
```xaml
<syncfusion:ButtonAdv Label="Custom" Style="{StaticResource CustomButtonAdvStyle}"/>
```

---

## Editing Templates in Visual Studio

Visual Studio also supports template editing from the design view.

**Steps:**
1. Open the WPF project in **Visual Studio**
2. Switch to **Design** view and select the `ButtonAdv` control
3. Right-click the button → **Edit Template** → choose:
   - **Edit a Copy...** — modifies the default template (recommended)
   - **Create Empty...** — creates a new blank template
4. In the **Create ControlTemplate Resource** dialog, name the template and choose the location
5. Click **OK** — the template XAML is generated in the resource section
6. Edit the XAML directly in Visual Studio's XAML editor

> **Tip:** Use "Edit a Copy" as a starting point rather than creating from scratch. The default
> template contains all the visual states and triggers that make the button function correctly.
> Modifying a copy preserves this behavior while letting you change colors, fonts, and layout.

---

## Edit a Copy vs Create Empty

| Option | When to use | What you get |
|---|---|---|
| **Edit a Copy** | You want to tweak colors, fonts, padding on the existing layout | Full default template with all visual states (hover, pressed, disabled) already wired up |
| **Create Empty** | You need a completely custom visual structure | Blank `ControlTemplate` — you build everything from scratch including all visual states |

> **Recommendation:** Always start with **Edit a Copy** unless you have a strong reason to
> rebuild the template from scratch. Visual states (mouse over, pressed, disabled, checked)
> require careful implementation — losing them creates a button that doesn't respond visually
> to user interactions.

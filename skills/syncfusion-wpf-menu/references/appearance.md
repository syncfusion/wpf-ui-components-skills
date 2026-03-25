# Appearance and Theming

## Themes via SfSkinManager

Apply a built-in Syncfusion visual style to `MenuAdv` using `SfSkinManager`:

```xaml
<syncfusion:MenuAdv syncfusion:SfSkinManager.VisualStyle="FluentLight"
                     Height="25" VerticalAlignment="Top">
    <syncfusion:MenuItemAdv Header="File"/>
    <syncfusion:MenuItemAdv Header="Edit"/>
</syncfusion:MenuAdv>
```

```csharp
using Syncfusion.SfSkinManager;

SfSkinManager.SetVisualStyle(menuAdv, VisualStyles.FluentLight);
```

**Supported styles:** `FluentLight`, `FluentDark`, `MaterialLight`, `MaterialDark`, `Office2019Colorful`, `Office2019Black`, `Office2019White`, `Windows11Light`, `Windows11Dark`, and others.

---

## Custom Themes via ThemeStudio

Create a custom branded theme using Syncfusion ThemeStudio and apply it to `MenuAdv`:

1. Open [Syncfusion ThemeStudio](https://ej2.syncfusion.com/themestudio/)
2. Customize colors, fonts, and sizes
3. Export and reference the generated resource dictionary in your app
4. Apply via `SfSkinManager` with your custom style name

---

## Animation Support

MenuAdv supports animation effects when submenus open and close. Animation is applied automatically as part of the control's built-in behavior and does not require explicit configuration.

If you need to disable or customize animation, override the control template via Expression Blend (see Blendability below).

---

## Keyboard Navigation

MenuAdv supports full keyboard navigation out of the box:

| Key | Action |
|---|---|
| `Arrow keys` | Move between menu items |
| `Enter` | Open submenu or execute item |
| `Escape` | Close submenu / close menu |
| `Alt` + underlined letter | Access menu item by access key |
| `Tab` | Move focus between items |

No configuration is required — keyboard navigation is built into the control.

---

## Blendability (Expression Blend Customization)

MenuAdv and `MenuItemAdv` support full template editing in **Expression Blend**:

1. Right-click `MenuAdv` in Blend → **Edit Template** → **Edit a Copy**
2. Customize the visual tree, colors, borders, and animations
3. Apply the custom `ControlTemplate` in your XAML resources

This allows pixel-level customization of the menu appearance beyond what themes provide.

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| Theme not applied | `SfSkinManager` assembly not referenced | Add reference to `Syncfusion.SfSkinManager.WPF` |
| Theme applies to app but not MenuAdv | Style set on wrong element | Set `SfSkinManager.VisualStyle` directly on the `MenuAdv` element or on the root `Window` |
| Custom template breaks keyboard nav | Template missing `KeyboardNavigation.TabNavigation` settings | Copy the default template first and modify incrementally |

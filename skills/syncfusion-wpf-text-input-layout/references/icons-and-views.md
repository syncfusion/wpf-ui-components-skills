# Icons and Supported Input Views in WPF TextInputLayout

## Overview

This guide covers two essential aspects of the WPF TextInputLayout control: adding custom icons (leading and trailing views) and working with supported input view controls. These features enable you to create rich, intuitive input experiences with visual indicators and flexible input types.

## Table of Contents

1. [Custom Icons Overview](#custom-icons-overview)
2. [Leading View](#leading-view)
3. [Trailing View](#trailing-view)
4. [Supported Input Views](#supported-input-views)
5. [Dropdown Customization](#dropdown-customization)
6. [Best Practices](#best-practices)

---

## Custom Icons Overview

The TextInputLayout control allows you to add custom icons at both the leading (left) and trailing (right) edges of the input view. These icons can be:

- **Unicode characters**
- **Font icons** (like Material Icons, Font Awesome)
- **Custom labels** with icon fonts
- **Any WPF control** (though labels are most common)

### Icon Positioning Diagram

```
┌─────────────────────────────────────────────────┐
│  [L]  ┌─────────────────────────┐  [T]         │
│  Out  │  [L]  Input View  [T]   │  Out         │
│  side │   In                In  │  side        │
│       └─────────────────────────┘               │
└─────────────────────────────────────────────────┘

[L] = Leading View Position (Inside/Outside)
[T] = Trailing View Position (Inside/Outside)
```

**Key Concepts:**
- **Leading View**: Icon on the left (start) side
- **Trailing View**: Icon on the right (end) side
- **Position**: Icons can be placed Inside or Outside the container
- **Events**: Icon interactions must be handled at application level

> **Important:** Events and commands linked to custom icons should be handled at the application level. The control itself does not provide built-in icon click handlers.

---

## Leading View

The leading view appears on the left side (start edge) of the input view and is typically used for:
- Input type indicators (calendar, search, user icons)
- Contextual hints about the expected input
- Brand or category identifiers

### Basic Leading View Implementation

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Birth date"
    LeadingViewPosition="Inside">
    <TextBox />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label
            FontFamily="/Assets/Sync FontIcons.ttf#Sync FontIcons"
            Text="&#x1F5D3;">
        </Label>
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Birth date";
inputLayout.LeadingViewPosition = ViewPosition.Inside;
inputLayout.LeadingView = new Label() { Text = "\U0001F5D3" }; // Calendar icon
inputLayout.InputView = new TextBox();
```

**Visual Result:**
```
Inside Position:
┌────────────────────────────────┐
│ Birth date                     │
├────────────────────────────────┤
│ 📅  [Enter date here]          │
└────────────────────────────────┘

Outside Position:
 📅 ┌──────────────────────────┐
    │ Birth date               │
    ├──────────────────────────┤
    │ [Enter date here]        │
    └──────────────────────────┘
```

### Leading View Positioning

The `LeadingViewPosition` property controls where the icon appears relative to the container.

**When to use Inside:**
- When the icon is semantically part of the input
- For compact layouts
- When following Material Design guidelines

**When to use Outside:**
- For labels or prefixes
- When the icon needs to be separate from the input area
- For better visual hierarchy

**XAML Comparison:**
```xaml
<!-- Inside Position (Default behavior varies, specify explicitly) -->
<inputLayout:SfTextInputLayout
    Hint="Search"
    LeadingViewPosition="Inside">
    <TextBox />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="🔍" FontSize="18" VerticalAlignment="Center"/>
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>

<!-- Outside Position -->
<inputLayout:SfTextInputLayout
    Hint="Price"
    LeadingViewPosition="Outside">
    <TextBox />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="$" FontSize="16" FontWeight="Bold" 
               VerticalAlignment="Center"/>
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

> **Default Value:** By default, `LeadingViewPosition` is set to `Outside`.

### Common Leading View Use Cases

**1. Search Icon**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Search users"
    LeadingViewPosition="Inside">
    <TextBox />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="🔍" FontSize="18" Foreground="Gray"/>
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

**2. User Profile Icon**
```csharp
var userInputLayout = new SfTextInputLayout();
userInputLayout.Hint = "Username";
userInputLayout.LeadingViewPosition = ViewPosition.Inside;
userInputLayout.LeadingView = new Label() 
{ 
    Text = "👤",
    FontSize = 18,
    VerticalAlignment = VerticalAlignment.Center
};
userInputLayout.InputView = new TextBox();
```

**3. Currency Symbol**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Amount"
    LeadingViewPosition="Outside">
    <TextBox />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="$" FontSize="20" FontWeight="Bold" 
               VerticalAlignment="Center" Foreground="#2E7D32"/>
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

**4. Country Code Prefix**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Phone number"
    LeadingViewPosition="Inside">
    <TextBox />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="+1" FontSize="14" Foreground="Gray"
               VerticalAlignment="Center" Margin="8,0,0,0"/>
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

---

## Trailing View

The trailing view appears on the right side (end edge) of the input view and is commonly used for:
- Action buttons (clear, submit, visibility toggle)
- Validation indicators (checkmark, error icon)
- Additional information icons
- Dropdown indicators

### Basic Trailing View Implementation

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Birth date"
    TrailingViewPosition="Outside">
    <TextBox />
    <inputLayout:SfTextInputLayout.TrailingView>
        <Label
            FontFamily="/Assets/Sync FontIcons.ttf#Sync FontIcons"
            Text="&#x1F5D3;">
        </Label>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Birth date";
inputLayout.TrailingViewPosition = ViewPosition.Outside;
inputLayout.TrailingView = new Label() { Text = "\U0001F5D3" }; // Calendar icon
inputLayout.InputView = new TextBox();
```

**Visual Result:**
```
Inside Position:
┌────────────────────────────────┐
│ Birth date                     │
├────────────────────────────────┤
│ [Enter date here]          📅  │
└────────────────────────────────┘

Outside Position:
┌──────────────────────────┐ 📅
│ Birth date               │
├──────────────────────────┤
│ [Enter date here]        │
└──────────────────────────┘
```

### Trailing View Positioning

The `TrailingViewPosition` property controls the icon's placement.

**When to use Inside:**
- For clear/visibility toggle buttons
- For validation status icons
- When the icon is functionally part of the input

**When to use Outside:**
- For help/info icons
- For actions that affect the entire field
- For calendar pickers or external tools

> **Default Value:** By default, `TrailingViewPosition` is set to `Inside`.

### Interactive Trailing Views

Since events must be handled at the application level, here are common interactive patterns:

**1. Password Visibility Toggle**
```xaml
<inputLayout:SfTextInputLayout
    x:Name="passwordLayout"
    Hint="Password"
    TrailingViewPosition="Inside">
    <PasswordBox x:Name="passwordBox"/>
    <inputLayout:SfTextInputLayout.TrailingView>
        <Button Content="👁" 
                Background="Transparent" 
                BorderThickness="0"
                Click="TogglePasswordVisibility"
                Cursor="Hand"/>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

```csharp
private bool isPasswordVisible = false;
private TextBox textBoxForPassword;

private void TogglePasswordVisibility(object sender, RoutedEventArgs e)
{
    if (isPasswordVisible)
    {
        // Switch back to PasswordBox
        var tempText = textBoxForPassword.Text;
        passwordBox.Password = tempText;
        passwordLayout.InputView = passwordBox;
        (sender as Button).Content = "👁";
    }
    else
    {
        // Switch to TextBox
        textBoxForPassword = new TextBox { Text = passwordBox.Password };
        passwordLayout.InputView = textBoxForPassword;
        (sender as Button).Content = "👁‍🗨";
    }
    isPasswordVisible = !isPasswordVisible;
}
```

**2. Clear Button**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Search"
    TrailingViewPosition="Inside">
    <TextBox x:Name="searchBox"/>
    <inputLayout:SfTextInputLayout.TrailingView>
        <Button Content="✖" 
                Background="Transparent"
                BorderThickness="0"
                Click="ClearSearch"
                Visibility="{Binding ElementName=searchBox, Path=Text.Length, 
                           Converter={StaticResource LengthToVisibilityConverter}}"/>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

```csharp
private void ClearSearch(object sender, RoutedEventArgs e)
{
    searchBox.Text = string.Empty;
    searchBox.Focus();
}
```

**3. Validation Indicator**
```csharp
private void ShowValidationIcon(SfTextInputLayout layout, bool isValid)
{
    var icon = new Label
    {
        Text = isValid ? "✓" : "✗",
        Foreground = new SolidColorBrush(isValid ? Colors.Green : Colors.Red),
        FontSize = 18,
        VerticalAlignment = VerticalAlignment.Center
    };
    
    layout.TrailingView = icon;
    layout.TrailingViewPosition = ViewPosition.Inside;
}
```

**4. Date Picker Button**
```xaml
<inputLayout:SfTextInputLayout
    Hint="Select date"
    TrailingViewPosition="Inside">
    <TextBox x:Name="dateTextBox" IsReadOnly="True"/>
    <inputLayout:SfTextInputLayout.TrailingView>
        <Button Content="📅" 
                Background="Transparent"
                BorderThickness="0"
                Click="OpenDatePicker"/>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

```csharp
private void OpenDatePicker(object sender, RoutedEventArgs e)
{
    var datePicker = new DatePicker();
    // Show date picker in a popup or dialog
    // Update dateTextBox.Text with selected date
}
```

---

## Supported Input Views

The TextInputLayout supports multiple input controls as its `InputView`. Each control type serves different input scenarios.

### InputView Property

Input views are added to the TextInputLayout by setting the `InputView` property. To simplify XAML syntax, the `InputView` property is decorated with the **ContentPropertyAttribute**, allowing you to directly place controls inside the TextInputLayout tag without explicitly specifying `InputView`.

**Simplified XAML Syntax (ContentPropertyAttribute):**
```xaml
<!-- Simplified - InputView is implicit -->
<inputLayout:SfTextInputLayout Hint="Name">
    <TextBox />  <!-- Automatically set as InputView -->
</inputLayout:SfTextInputLayout>

<!-- Explicit - InputView specified -->
<inputLayout:SfTextInputLayout Hint="Name" InputView="{Binding ElementName=myTextBox}"/>
```

### Architecture Diagram

```
SfTextInputLayout (Container)
        │
        ├─ Hint Label
        ├─ Leading View (optional)
        ├─ InputView ────┬─ TextBox
        │                ├─ PasswordBox
        │                ├─ ComboBox
        │                ├─ ComboBoxAdv
        │                └─ SfTextBoxExt (Autocomplete)
        ├─ Trailing View (optional)
        └─ Assistive Labels (HelperText/ErrorText)
```

### 1. TextBox

**Purpose:** Standard single-line or multi-line text input

**When to use:**
- Username, name, address fields
- General text input
- Multi-line comments (with `AcceptsReturn="True"`)

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Name" 
    HelperText="Enter your name">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
SfTextInputLayout inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.HelperText = "Enter your name";
inputLayout.InputView = new TextBox();
this.Content = inputLayout;
```

**Visual Result:**
```
┌────────────────────────────────┐
│ Name                           │
├────────────────────────────────┤
│ [Text entry here]              │
├────────────────────────────────┤
│ Enter your name                │
└────────────────────────────────┘
```

**Multi-line TextBox:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Comments"
    HelperText="Optional feedback">
    <TextBox AcceptsReturn="True" 
             TextWrapping="Wrap" 
             MinHeight="80"/>
</inputLayout:SfTextInputLayout>
```

### 2. PasswordBox

**Purpose:** Secure password input with masked characters

**When to use:**
- Password fields
- PIN entry
- Any sensitive text input

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Password" 
    HelperText="Enter your password">
    <PasswordBox />
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
SfTextInputLayout inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Password";
inputLayout.HelperText = "Enter your password";
inputLayout.InputView = new PasswordBox();
this.Content = inputLayout;
```

**Visual Result:**
```
┌────────────────────────────────┐
│ Password                       │
├────────────────────────────────┤
│ ••••••••                       │
├────────────────────────────────┤
│ Enter your password            │
└────────────────────────────────┘
```

**Pattern: Password with Strength Indicator**
```csharp
private void PasswordBox_PasswordChanged(object sender, RoutedEventArgs e)
{
    var password = (sender as PasswordBox).Password;
    var strength = CalculatePasswordStrength(password);
    
    passwordLayout.HelperText = $"Password strength: {strength}";
    
    if (strength == "Weak")
        passwordLayout.FocusedForeground = Brushes.Red;
    else if (strength == "Medium")
        passwordLayout.FocusedForeground = Brushes.Orange;
    else
        passwordLayout.FocusedForeground = Brushes.Green;
}
```

### 3. ComboBox

**Purpose:** Standard WPF dropdown selection

**When to use:**
- Selecting from a predefined list
- Country/state selection
- Simple dropdown menus

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Country" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Center">
    <ComboBox x:Name="comboBox" 
              Width="150" 
              Height="10" 
              ItemsSource="{Binding Countries}"/>
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
SfTextInputLayout sfTextInputLayout = new SfTextInputLayout() { Hint = "Country" };
sfTextInputLayout.HorizontalAlignment = HorizontalAlignment.Center;
sfTextInputLayout.VerticalAlignment = VerticalAlignment.Center;

ComboBox comboBox = new ComboBox();
comboBox.Width = 150;
comboBox.Height = 10;
comboBox.ItemsSource = viewModel.Countries;

sfTextInputLayout.InputView = comboBox;
this.Content = sfTextInputLayout;
```

**Visual Result:**
```
┌────────────────────────────────┐
│ Country                     ▼  │
├────────────────────────────────┤
│ United States                  │
└────────────────────────────────┘
```

### 4. ComboBoxAdv (Syncfusion)

**Purpose:** Advanced dropdown with additional features

**When to use:**
- Need for multi-selection
- Advanced filtering
- Custom item templates
- Better performance with large datasets

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Country" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Center">
    <syncfusion:ComboBoxAdv x:Name="comboBox" 
                            ItemsSource="{Binding Countries}" 
                            Width="150" 
                            Height="10"/>
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
SfTextInputLayout sfTextInputLayout = new SfTextInputLayout() { Hint = "Country" };
sfTextInputLayout.HorizontalAlignment = HorizontalAlignment.Center;
sfTextInputLayout.VerticalAlignment = VerticalAlignment.Center;

ComboBoxAdv comboBox = new ComboBoxAdv();
comboBox.Width = 150;
comboBox.Height = 10;
comboBox.ItemsSource = viewModel.Countries;

sfTextInputLayout.InputView = comboBox;
this.Content = sfTextInputLayout;
```

### 5. SfTextBoxExt (Autocomplete)

**Purpose:** Text input with autocomplete suggestions

**When to use:**
- Search functionality
- Large predefined lists
- When user needs typing assistance
- Tag/chip input scenarios

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Country" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Center">
    <syncfusion:SfTextBoxExt 
        AutoCompleteMode="Suggest" 
        Width="250" 
        AutoCompleteSource="{Binding Countries}">
    </syncfusion:SfTextBoxExt>
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
SfTextInputLayout sfTextInputLayout = new SfTextInputLayout() { Hint = "Country" };
sfTextInputLayout.HorizontalAlignment = HorizontalAlignment.Center;
sfTextInputLayout.VerticalAlignment = VerticalAlignment.Center;

SfTextBoxExt sfTextBoxExt = new SfTextBoxExt();
sfTextBoxExt.AutoCompleteMode = AutoCompleteMode.Suggest;
sfTextBoxExt.Width = 250;
sfTextBoxExt.AutoCompleteSource = viewModel.Countries;

sfTextInputLayout.InputView = sfTextBoxExt;
this.Content = sfTextInputLayout;
```

**Visual Result:**
```
┌────────────────────────────────┐
│ Country                        │
├────────────────────────────────┤
│ Un                             │
├────────────────────────────────┤
│ ↓ Suggestions:                 │
│   United States                │
│   United Kingdom               │
│   United Arab Emirates         │
└────────────────────────────────┘
```

---

## Dropdown Customization

For ComboBox and ComboBoxAdv controls, you can customize the dropdown popup's border using the `DropDownBorder` property.

### Basic Dropdown Border

**XAML Example:**
```xaml
<inputLayout:SfTextInputLayout 
    Hint="Country" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Center">
    <inputLayout:SfTextInputLayout.DropDownBorder>
        <Border BorderBrush="Blue" 
                BorderThickness="2" 
                CornerRadius="5"/>
    </inputLayout:SfTextInputLayout.DropDownBorder>
    <ComboBox x:Name="comboBox" 
              Width="150" 
              Height="10" 
              ItemsSource="{Binding Countries}"/>
</inputLayout:SfTextInputLayout>
```

**C# Example:**
```csharp
SfTextInputLayout sfTextInputLayout = new SfTextInputLayout() { Hint = "Country" };
sfTextInputLayout.HorizontalAlignment = HorizontalAlignment.Center;
sfTextInputLayout.VerticalAlignment = VerticalAlignment.Center;

Border dropDownBorder = new Border();
dropDownBorder.BorderBrush = Brushes.Blue;
dropDownBorder.BorderThickness = new Thickness(2);
dropDownBorder.CornerRadius = new CornerRadius(5);
sfTextInputLayout.DropDownBorder = dropDownBorder;

ComboBox comboBox = new ComboBox();
comboBox.Width = 150;
comboBox.Height = 10;
comboBox.ItemsSource = viewModel.Countries;

sfTextInputLayout.InputView = comboBox;
this.Content = sfTextInputLayout;
```

> **Note:** The `DropDownBorder` property is applicable only when using ComboBox or ComboBoxAdv as the input view.

### Advanced Dropdown Styling

**Material Design Style:**
```xaml
<inputLayout:SfTextInputLayout.DropDownBorder>
    <Border BorderBrush="#2196F3" 
            BorderThickness="0,2,0,0" 
            Background="White"
            CornerRadius="0,0,4,4">
        <Border.Effect>
            <DropShadowEffect BlurRadius="8" 
                            ShadowDepth="2" 
                            Opacity="0.3"/>
        </Border.Effect>
    </Border>
</inputLayout:SfTextInputLayout.DropDownBorder>
```

**Elevated Card Style:**
```csharp
var dropDownBorder = new Border
{
    BorderBrush = Brushes.LightGray,
    BorderThickness = new Thickness(1),
    CornerRadius = new CornerRadius(8),
    Background = Brushes.White,
    Effect = new DropShadowEffect
    {
        BlurRadius = 16,
        ShadowDepth = 4,
        Opacity = 0.2,
        Color = Colors.Black
    }
};
sfTextInputLayout.DropDownBorder = dropDownBorder;
```

---

## Best Practices

### 1. **Icon Selection Guidelines**

| Purpose | Recommended Icon | Position |
|---------|-----------------|----------|
| Search | 🔍 (U+1F50D) | Leading, Inside |
| User/Profile | 👤 (U+1F464) | Leading, Inside |
| Email | ✉ (U+2709) | Leading, Inside |
| Password | 🔒 (U+1F512) | Leading, Inside |
| Calendar/Date | 📅 (U+1F4C5) | Trailing, Inside |
| Clear/Close | ✖ (U+2716) | Trailing, Inside |
| Validation Success | ✓ (U+2713) | Trailing, Inside |
| Validation Error | ✗ (U+2717) | Trailing, Inside |
| Help/Info | ℹ (U+2139) | Trailing, Outside |
| Dropdown | ▼ (U+25BC) | Trailing, Inside |

### 2. **Icon Sizing Standards**

```csharp
public static class IconSizes
{
    public const double Small = 14;      // For dense layouts
    public const double Medium = 18;     // Standard size
    public const double Large = 24;      // Prominent features
    public const double Touch = 32;      // Touch-friendly buttons
}
```

### 3. **Choosing the Right Input View**

```csharp
public static UIElement GetInputViewForScenario(InputScenario scenario)
{
    return scenario switch
    {
        InputScenario.FreeText => new TextBox(),
        InputScenario.SecurePassword => new PasswordBox(),
        InputScenario.SmallList => new ComboBox(), // < 100 items
        InputScenario.LargeList => new ComboBoxAdv(), // > 100 items
        InputScenario.SearchWithSuggestions => new SfTextBoxExt 
        { 
            AutoCompleteMode = AutoCompleteMode.Suggest 
        },
        InputScenario.MultiLine => new TextBox 
        { 
            AcceptsReturn = true, 
            TextWrapping = TextWrapping.Wrap 
        },
        _ => new TextBox()
    };
}
```

### 4. **Icon Accessibility**

```xaml
<!-- Good: Includes accessible label -->
<inputLayout:SfTextInputLayout.LeadingView>
    <Label Text="🔍" 
           AutomationProperties.Name="Search icon"
           FontSize="18"/>
</inputLayout:SfTextInputLayout.LeadingView>

<!-- Better: Use semantic button for interactive icons -->
<inputLayout:SfTextInputLayout.TrailingView>
    <Button Content="✖" 
            AutomationProperties.Name="Clear search"
            Background="Transparent"
            BorderThickness="0"/>
</inputLayout:SfTextInputLayout.TrailingView>
```

### 5. **Performance with Large Datasets**

```csharp
// Use ComboBoxAdv for better performance
public void SetupLargeDatasetInput(SfTextInputLayout layout, List<string> data)
{
    if (data.Count > 100)
    {
        // Use ComboBoxAdv for virtualization support
        var comboBox = new ComboBoxAdv
        {
            ItemsSource = data,
            EnableVirtualization = true
        };
        layout.InputView = comboBox;
    }
    else
    {
        // Standard ComboBox is fine
        layout.InputView = new ComboBox { ItemsSource = data };
    }
}
```

### 6. **Dynamic Icon Updates**

```csharp
public void UpdateValidationIcon(SfTextInputLayout layout, ValidationState state)
{
    Label icon = new Label
    {
        FontSize = 18,
        VerticalAlignment = VerticalAlignment.Center
    };
    
    switch (state)
    {
        case ValidationState.Valid:
            icon.Text = "✓";
            icon.Foreground = Brushes.Green;
            layout.HasError = false;
            break;
        case ValidationState.Invalid:
            icon.Text = "✗";
            icon.Foreground = Brushes.Red;
            layout.HasError = true;
            break;
        case ValidationState.Pending:
            icon.Text = "⏳";
            icon.Foreground = Brushes.Orange;
            layout.HasError = false;
            break;
    }
    
    layout.TrailingView = icon;
}
```

---

## Complete Examples

### Example 1: Login Form with Icons

```xaml
<StackPanel Width="300" Margin="20">
    <!-- Username Field -->
    <inputLayout:SfTextInputLayout
        Hint="Username"
        ContainerType="Outlined"
        LeadingViewPosition="Inside"
        Margin="0,0,0,16">
        <TextBox />
        <inputLayout:SfTextInputLayout.LeadingView>
            <Label Text="👤" FontSize="18" Foreground="Gray"/>
        </inputLayout:SfTextInputLayout.LeadingView>
    </inputLayout:SfTextInputLayout>
    
    <!-- Password Field with Toggle -->
    <inputLayout:SfTextInputLayout
        x:Name="passwordLayout"
        Hint="Password"
        ContainerType="Outlined"
        LeadingViewPosition="Inside"
        TrailingViewPosition="Inside">
        <PasswordBox x:Name="passwordBox"/>
        <inputLayout:SfTextInputLayout.LeadingView>
            <Label Text="🔒" FontSize="18" Foreground="Gray"/>
        </inputLayout:SfTextInputLayout.LeadingView>
        <inputLayout:SfTextInputLayout.TrailingView>
            <Button Content="👁" 
                    Background="Transparent" 
                    BorderThickness="0"
                    Cursor="Hand"/>
        </inputLayout:SfTextInputLayout.TrailingView>
    </inputLayout:SfTextInputLayout>
</StackPanel>
```

### Example 2: Search with Autocomplete

```xaml
<inputLayout:SfTextInputLayout
    Hint="Search countries"
    ContainerType="Filled"
    LeadingViewPosition="Inside"
    TrailingViewPosition="Inside">
    <syncfusion:SfTextBoxExt 
        x:Name="searchBox"
        AutoCompleteMode="SuggestAppend"
        AutoCompleteSource="{Binding Countries}"/>
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="🔍" FontSize="18" Foreground="#666"/>
    </inputLayout:SfTextInputLayout.LeadingView>
    <inputLayout:SfTextInputLayout.TrailingView>
        <Button Content="✖" 
                Background="Transparent"
                BorderThickness="0"
                Visibility="{Binding ElementName=searchBox, Path=Text.Length,
                           Converter={StaticResource HasTextConverter}}"/>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

### Example 3: Date Picker

```xaml
<inputLayout:SfTextInputLayout
    Hint="Birth date"
    ContainerType="Outlined"
    TrailingViewPosition="Inside">
    <TextBox x:Name="dateTextBox" IsReadOnly="True"/>
    <inputLayout:SfTextInputLayout.TrailingView>
        <Button Content="📅" 
                FontSize="18"
                Background="Transparent"
                BorderThickness="0"
                Click="ShowDatePicker"
                Cursor="Hand"/>
    </inputLayout:SfTextInputLayout.TrailingView>
</inputLayout:SfTextInputLayout>
```

### Example 4: Currency Input

```xaml
<inputLayout:SfTextInputLayout
    Hint="Amount"
    ContainerType="Outlined"
    LeadingViewPosition="Outside">
    <TextBox />
    <inputLayout:SfTextInputLayout.LeadingView>
        <Label Text="$" 
               FontSize="20" 
               FontWeight="Bold"
               Foreground="#2E7D32"
               VerticalAlignment="Center"/>
    </inputLayout:SfTextInputLayout.LeadingView>
</inputLayout:SfTextInputLayout>
```

---

## Related Topics

- [Customization](customization.md) - Color and styling options
- [Hint Configuration](hint-configuration.md) - Hint label positioning and behavior
- [Assistive Labels](assistive-labels.md) - Helper text and error messages
- [Container Types](container-type.md) - Outlined, Filled, and None containers
- [Advanced Features](advanced-features.md) - RTL support and complete API reference

## Common Pitfalls to Avoid

1. **Missing event handlers**: Remember to implement click handlers for interactive icons.
2. **Icon size inconsistency**: Use consistent font sizes across your application.
3. **Wrong position choice**: Inside is better for functional icons, Outside for informational ones.
4. **Accessibility neglect**: Always add `AutomationProperties.Name` for screen readers.
5. **Performance with ComboBox**: Use ComboBoxAdv for lists with > 100 items.
6. **DropDownBorder misuse**: Only applicable to ComboBox/ComboBoxAdv, not other input views.

## Summary

The WPF TextInputLayout provides powerful customization through:
- **Leading and Trailing Views** for contextual icons and actions
- **Flexible positioning** (Inside/Outside) for optimal layout
- **Multiple input view types** (TextBox, PasswordBox, ComboBox, ComboBoxAdv, SfTextBoxExt)
- **Dropdown customization** for branded selection experiences

By combining these features effectively, you can create intuitive, beautiful input forms that enhance user experience.

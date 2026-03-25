# Container Types in WPF TextInputLayout

Container types define the visual style and structure of the TextInputLayout control. Syncfusion WPF TextInputLayout supports three container types: Outlined, Filled, and None.

## ContainerType Property Overview

The `ContainerType` property determines how the input layout is rendered and styled.

**Default Value:** `Outlined`

**Available Options:**
- **Outlined**: Container with rounded border (Material Design style)
- **Filled**: Container with filled background and base line
- **None**: Minimal container with no background or border

## Outlined Container

The outlined container wraps the input view with a rounded border, following Material Design principles.

### Basic Outlined Container

**XAML:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Outlined">
    <TextBox Text="John" />
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.ContainerType = ContainerType.Outlined;
inputLayout.InputView = new TextBox() { Text = "John" };
```

### Visual Appearance

```
Unfocused:
Name
┌─────────────────┐
│ John            │  ← Gray border
└─────────────────┘

Focused:
Name
╔═════════════════╗
║ John            ║  ← Thicker blue border
╚═════════════════╝
```

### When to Use Outlined

Use the outlined container when you need:
- **Clean, modern appearance**: Material Design aesthetic
- **Clear visual boundaries**: Defined input areas
- **High contrast**: Works well on any background
- **Professional forms**: Business applications, settings pages

### Customizing Outlined Border

Customize the corner radius for outlined containers:

```xml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Outlined"
    OutlineCornerRadius="8">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

```csharp
inputLayout.OutlineCornerRadius = 8;
```

**Examples of corner radius:**
- `0`: Sharp corners (rectangular)
- `4`: Slight rounding (default)
- `8`: Medium rounding
- `16`: Heavy rounding (pill-shaped)

## Filled Container

The filled container provides a background color with a base line at the bottom that changes based on the input state.

### Basic Filled Container

**XAML:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Filled">
    <TextBox Text="John" />
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.ContainerType = ContainerType.Filled;
inputLayout.InputView = new TextBox() { Text = "John" };
```

### Visual Appearance

```
Unfocused:
Name
▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  ← Light gray background
▓ John          ▓
─────────────────  ← Thin gray base line

Focused:
Name
▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  ← Light gray background
▓ John          ▓
═════════════════  ← Thicker blue base line
```

### When to Use Filled

Use the filled container when you need:
- **Material Design style**: Google's Material Design pattern
- **Grouping visual cue**: Background distinguishes input areas
- **Compact appearance**: Less visual weight than outlined
- **Form density**: Multiple inputs in limited space

### Customizing Filled Container

Set custom background color:

```xml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Filled"
    ContainerBackground="#F5F5F5"
    FocusedForeground="#6200EE">
    <TextBox Text="John" />
</inputLayout:SfTextInputLayout>
```

```csharp
inputLayout.ContainerType = ContainerType.Filled;
inputLayout.ContainerBackground = new SolidColorBrush(Color.FromRgb(245, 245, 245));
inputLayout.FocusedForeground = new SolidColorBrush(Color.FromRgb(98, 0, 238));
```

## None Container

The none container provides a minimal appearance with empty background and sufficient spacing.

### Basic None Container

**XAML:**
```xml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="None">
    <TextBox Text="John" />
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
var inputLayout = new SfTextInputLayout();
inputLayout.Hint = "Name";
inputLayout.ContainerType = ContainerType.None;
inputLayout.InputView = new TextBox() { Text = "John" };
```

### Visual Appearance

```
Unfocused:
Name
  John            ← No border or background
─────────────────  ← Optional base line

Focused:
Name
  John            ← No border or background
═════════════════  ← Emphasized base line
```

### When to Use None

Use the none container when you need:
- **Minimal design**: Clean, distraction-free interface
- **Custom styling**: You want to add your own container
- **Inline editing**: Editable fields within content
- **Compact layouts**: Maximum space efficiency

### Styling None Container

Even with None container, you can still customize the base line:

```xml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="None"
    FocusedForeground="Blue"
    StrokeThickness="1"
    FocusedStrokeThickness="2">
    <TextBox Text="John" />
</inputLayout:SfTextInputLayout>
```

## ContainerBackground Customization

The `ContainerBackground` property sets the background color of the container.

### Applicability

**Works with:**
- ✅ Filled container
- ✅ Outlined container

**Does not work with:**
- ❌ None container (no background area)

### Setting Container Background

**XAML (with color name):**
```xml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Outlined"
    ContainerBackground="LightBlue"
    FocusedForeground="Blue">
    <TextBox Text="John" />
</inputLayout:SfTextInputLayout>
```

**XAML (with hex color):**
```xml
<inputLayout:SfTextInputLayout
    Hint="Name" 
    ContainerType="Filled"
    ContainerBackground="#E8F4F8">
    <TextBox />
</inputLayout:SfTextInputLayout>
```

**C#:**
```csharp
inputLayout.ContainerBackground = Brushes.LightBlue;
// Or with hex color:
inputLayout.ContainerBackground = new SolidColorBrush(
    (Color)ColorConverter.ConvertFromString("#E8F4F8")
);
```

### Background Color Best Practices

**For Filled Container:**
- Use light, subtle colors (e.g., #F5F5F5, #E8F4F8)
- Ensure sufficient contrast with text
- Match your application's color scheme

**For Outlined Container:**
- Background is less common but can add emphasis
- Use for important or highlighted fields
- Keep it subtle to maintain the outlined aesthetic

## Comparing Container Types

### Visual Comparison

| Aspect | Outlined | Filled | None |
|--------|----------|--------|------|
| Border | Rounded border | No border | No border |
| Background | Optional | Filled | Transparent |
| Base Line | No | Yes | Optional |
| Visual Weight | Medium-High | Medium | Low |
| Best For | Clear boundaries | Material Design | Minimal UI |

### Use Case Matrix

| Scenario | Recommended Container |
|----------|----------------------|
| Form with clear sections | Outlined |
| Material Design app | Filled |
| Inline editing | None |
| High-contrast needed | Outlined |
| Space-constrained layout | None or Filled |
| Professional/business app | Outlined |
| Mobile-first design | Filled |
| Settings page | Outlined |

## Complete Examples

### Material Design Login Form

```xml
<StackPanel Margin="40" Width="300">
    
    <!-- Email Input (Filled) -->
    <inputLayout:SfTextInputLayout
        Hint="Email" 
        ContainerType="Filled"
        ContainerBackground="#F5F5F5"
        FocusedForeground="#6200EE"
        HelperText="Enter your email address"
        Margin="0,0,0,20">
        <TextBox />
    </inputLayout:SfTextInputLayout>
    
    <!-- Password Input (Filled) -->
    <inputLayout:SfTextInputLayout
        Hint="Password" 
        ContainerType="Filled"
        ContainerBackground="#F5F5F5"
        FocusedForeground="#6200EE"
        Margin="0,0,0,20">
        <PasswordBox />
    </inputLayout:SfTextInputLayout>
    
    <Button Content="Sign In" Background="#6200EE" Foreground="White"/>
    
</StackPanel>
```

### Professional Form with Outlined Containers

```xml
<StackPanel Margin="40" Width="350">
    
    <!-- Name Field -->
    <inputLayout:SfTextInputLayout
        Hint="Full Name" 
        ContainerType="Outlined"
        OutlineCornerRadius="4"
        FocusedBorderBrush="#0078D4"
        HelperText="Enter your legal name"
        Margin="0,0,0,20">
        <TextBox />
    </inputLayout:SfTextInputLayout>
    
    <!-- Company Field -->
    <inputLayout:SfTextInputLayout
        Hint="Company" 
        ContainerType="Outlined"
        OutlineCornerRadius="4"
        FocusedBorderBrush="#0078D4"
        Margin="0,0,0,20">
        <TextBox />
    </inputLayout:SfTextInputLayout>
    
    <!-- Role Dropdown -->
    <inputLayout:SfTextInputLayout
        Hint="Role" 
        ContainerType="Outlined"
        OutlineCornerRadius="4"
        FocusedBorderBrush="#0078D4">
        <ComboBox>
            <ComboBoxItem>Developer</ComboBoxItem>
            <ComboBoxItem>Designer</ComboBoxItem>
            <ComboBoxItem>Manager</ComboBoxItem>
        </ComboBox>
    </inputLayout:SfTextInputLayout>
    
</StackPanel>
```

### Minimal Inline Editor

```xml
<StackPanel Margin="20">
    
    <TextBlock Text="Profile Information" FontSize="18" FontWeight="Bold" Margin="0,0,0,20"/>
    
    <!-- Inline Name Edit -->
    <inputLayout:SfTextInputLayout
        Hint="Display Name" 
        ContainerType="None"
        FocusedForeground="Blue"
        Margin="0,0,0,15">
        <TextBox Text="John Doe" BorderThickness="0"/>
    </inputLayout:SfTextInputLayout>
    
    <!-- Inline Bio Edit -->
    <inputLayout:SfTextInputLayout
        Hint="Bio" 
        ContainerType="None"
        FocusedForeground="Blue"
        CharMaxLength="200"
        CharCountVisibility="Visible">
        <TextBox Text="Software Developer" BorderThickness="0"/>
    </inputLayout:SfTextInputLayout>
    
</StackPanel>
```

## Related Topics

- **Customization and Styling**: Learn more about colors and borders → [customization.md](customization.md)
- **Assistive Labels**: Add helper text and validation → [assistive-labels.md](assistive-labels.md)
- **Icons and Views**: Enhance containers with icons → [icons-and-views.md](icons-and-views.md)

---
name: syncfusion-wpf-domain-updown
description: Implements the Syncfusion WPF DomainUpDown (SfDomainUpDown) control for cycling through predefined item lists using spin buttons. Use this when adding domain selectors, item selectors with up/down buttons, or controls that navigate through collections using increment/decrement buttons in WPF applications. Covers item population, spin button alignment, styling, auto-reverse, and data binding with custom templates.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WPF DomainUpDown

Guide for implementing the Syncfusion® WPF DomainUpDown (SfDomainUpDown) control - a versatile input control that allows users to cycle through a predefined list of items using spin buttons (increment/decrement arrows).

## When to Use This Skill

Use this skill when you need to:
- **Implement a DomainUpDown control** for item selection with spin buttons
- **Populate predefined item lists** that users can cycle through
- **Customize spin button positioning** (left, right, or both sides)
- **Bind complex data objects** with custom display templates
- **Style and theme** the control appearance
- **Enable auto-reverse** cycling from max to min values
- **Handle mouse wheel** and keyboard navigation gestures
- **Create data-driven selectors** with MVVM pattern support

## Component Overview

The `SfDomainUpDown` control provides an elegant way for users to select values from a predefined list by clicking up/down buttons or using mouse wheel/keyboard navigation. Unlike numeric up-down controls, DomainUpDown works with any type of data - strings, objects, or custom types.

**Key Capabilities:**
- Data binding support with `ItemsSource`
- Custom content templates for complex objects
- Configurable spin button alignment (left, right, both)
- Smooth spin animations
- Auto-reverse cycling behavior
- Mouse wheel and keyboard gestures
- Extensive styling and theming options
- MVVM-friendly architecture

**Assemblies Required:**
- `Syncfusion.SfInput.WPF`
- `Syncfusion.SfShared.WPF`

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When you need to:
- Install and set up the DomainUpDown control
- Add the control via designer or code
- Understand assembly dependencies
- Create your first basic implementation
- Configure the `Value` property
- Import necessary namespaces

### Data Population and Binding
📄 **Read:** [references/data-population.md](references/data-population.md)

When you need to:
- Populate the control with a list of items
- Bind data using `ItemsSource` property
- Create data models and ViewModels
- Use `ContentTemplate` for custom display
- Display complex objects with multiple properties
- Implement MVVM data binding patterns

### Spin Button Configuration
📄 **Read:** [references/spin-button-alignment.md](references/spin-button-alignment.md)

When you need to:
- Customize spin button positioning
- Use `SpinButtonsAlignment` property
- Place buttons on the right (default)
- Place buttons on the left
- Split buttons on both sides (decrement left, increment right)
- Choose appropriate alignment for your UI

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)

When you need to:
- Enable or disable spin animations
- Customize accent colors with `AccentBrush`
- Style up/down buttons with `UpDownStyle`
- Create custom control templates
- Modify borders, backgrounds, and foregrounds
- Apply fonts and layout customization
- Build comprehensive custom themes

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

When you need to:
- Enable auto-reverse cycling behavior
- Handle mouse wheel scrolling
- Implement keyboard navigation
- Understand edge cases
- Optimize performance

---

**→ See reference files for detailed examples and implementation patterns.**



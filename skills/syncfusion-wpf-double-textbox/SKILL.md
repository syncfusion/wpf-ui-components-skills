---
name: syncfusion-wpf-double-textbox
description: Implements the Syncfusion WPF DoubleTextBox control for numeric double-precision input with formatting and validation. Use this when adding numeric textboxes, configuring value ranges (MinValue, MaxValue), or customizing number formatting in WPF applications. Covers culture-specific formatting, scroll intervals, watermarks, null values, and range adorners.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing DoubleTextBox

This skill collects guidance and examples for the Syncfusion WPF `DoubleTextBox` control. Use this skill to find usage patterns, configuration options, and common gotchas when building WPF apps that accept double values.

## When to Use This Skill
- When adding or configuring a WPF numeric input bound to doubles
- When restricting values using `MinValue` / `MaxValue`
- When formatting values for different cultures or customizing group/decimal separators
- When enabling step/scroll intervals, spin buttons, or range adorner
- When you need examples for XAML or C# usage patterns

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installing and adding the control via XAML/C#; basic properties and value binding

### Overview
📄 **Read:** [references/overview.md](references/overview.md)
- Short feature list and control summary

### Appearance & Styling
📄 **Read:** [references/appearance-and-styling.md](references/appearance-and-styling.md)
- Foreground for positive/negative/zero, background, corner radius, selection brush

### Validation & Restrictions
📄 **Read:** [references/restriction-or-validation.md](references/restriction-or-validation.md)
- Min/Max validation modes, decimal digit limits, read-only mode

### Step Interval & Scrolling
📄 **Read:** [references/step-interval.md](references/step-interval.md)
- `ScrollInterval`, mouse wheel, click-and-drag, selection-on-focus

### Culture & Number Formatting
📄 **Read:** [references/culture-and-number-formats.md](references/culture-and-number-formats.md)
- `Culture`, `NumberFormat`, dedicated formatting properties

### Range Adorner
📄 **Read:** [references/range-adorner.md](references/range-adorner.md)
- Visual range indicator based on `MinValue`/`MaxValue`

### Changing Value & Advanced Behavior
📄 **Read:** [references/changing-double-value.md](references/changing-double-value.md)
- Paste behavior, spin buttons, null value handling, watermarks

## Quick Start (XAML)

```xaml
<syncfusion:DoubleTextBox x:Name="doubleTextBox" Width="150" Height="25"
                          MinValue="0" MaxValue="100" Value="10"
                          ScrollInterval="1" />
```

## Common Patterns
- Bind `Value` to a `ViewModel` property and use `UpdateSourceTrigger=PropertyChanged` for immediate updates.
- Use `MinValidation`/`MaxValidation` to choose between strict-on-keypress and permissive-on-lost-focus behaviors.
- For globalization, set `Culture` or `NumberFormat` and prefer `NumberFormat` when exact control is required.

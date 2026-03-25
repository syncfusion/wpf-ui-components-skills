---
name: syncfusion-wpf-calculator
description: Implement Syncfusion WPF Calculator (SfCalculator) for mathematical operations in Windows desktop applications. Use this when adding calculator functionality to WPF apps. Covers setup, value management, and memory operations.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing WPF Calculator (SfCalculator)

## When to Use This Skill

Use this skill when you need to:
- Add calculator functionality to WPF desktop applications
- Perform mathematical operations in your application
- Implement memory operations (store, restore, add, subtract, clear)
- Set up and configure the SfCalculator control with proper assemblies
- Display calculated values and manage input/output
- Implement features like watermark text and default values

## Component Overview

The **SfCalculator** is a Syncfusion WPF control that allows users to perform mathematical operations like in a standard calculator. It supports:
- Basic arithmetic operations
- Memory storage and retrieval (MS, MR, M+, M-, MC)
- Read-only Value property for calculated results
- Memory property for stored values
- Customizable display text and default values
- Theme support through SfSkinManager

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly dependencies (Syncfusion.SfInput.WPF, Syncfusion.Shared.WPF)
- Creating a new WPF project
- Adding SfCalculator via Visual Studio Designer
- Adding SfCalculator via XAML markup
- Adding SfCalculator programmatically in C#
- Basic configuration

### Properties and Values
📄 **Read:** [references/properties-and-values.md](references/properties-and-values.md)
- Value property (read-only calculated result)
- DefaultValue property (initial display value)
- DisplayText property (watermark/placeholder text)
- Setting and retrieving values
- Data binding patterns

### Memory Operations
📄 **Read:** [references/memory-operations.md](references/memory-operations.md)
- Memory property overview (read-only)
- MS button (Memory Store)
- MR button (Memory Restore)
- M+ button (Memory Add)
- M- button (Memory Subtract)
- MC button (Memory Clear)
- Practical memory operation examples

## Quick Start Example

**Basic XAML Implementation:**
```xaml
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <syncfusion:SfCalculator x:Name="Calculator" 
                                 Width="200" 
                                 Height="300" />
    </Grid>
</Window>
```

**Programmatic C# Implementation:**
```csharp
using Syncfusion.Windows.Controls.Input;

SfCalculator calculator = new SfCalculator();
calculator.DefaultValue = 0;
calculator.DisplayText = "Enter value";
this.Content = calculator;
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| **Value** | decimal (read-only) | Current calculated value from the last expression |
| **Memory** | decimal (read-only) | Value currently stored in memory |
| **DefaultValue** | decimal | Initial value displayed when calculator starts |
| **DisplayText** | string | Watermark/placeholder text shown in the value display |

## Common Use Cases

1. **Building Tools Applications** - Embed calculator for quick calculations
2. **Financial Applications** - Add memory operations for repeated calculations
3. **Data Entry Forms** - Quick calculation reference without switching windows
4. **Educational Applications** - Teach mathematical operations
5. **Dashboard Utilities** - Add calculation capabilities to admin panels

## Common Patterns

**Pattern 1: Display with Watermark**
```csharp
calculator.DisplayText = "Enter calculation";
calculator.DefaultValue = 0;
```

**Pattern 2: Access Calculated Values**
```csharp
decimal result = calculator.Value; // Get calculated value
decimal stored = calculator.Memory; // Get memory value
```

**Pattern 3: Memory Operations Workflow**
```
User clicks MS → Value stored to Memory
User clicks MR → Memory value retrieved
User clicks M+ → Add current value to memory
User clicks M- → Subtract current value from memory
User clicks MC → Clear memory
```

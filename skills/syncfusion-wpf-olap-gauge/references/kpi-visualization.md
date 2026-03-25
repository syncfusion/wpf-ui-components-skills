# KPI Visualization in OLAP Gauge

Comprehensive guide for displaying Key Performance Indicators (KPI) with values, goals, status indicators, and trend visualizations in the OLAP Gauge control.

## Table of Contents

- [Overview](#overview)
- [KPI Fundamentals](#kpi-fundamentals)
- [KPI Components](#kpi-components)
- [KPI Value (Pointers)](#kpi-value-pointers)
- [KPI Goal (Markers)](#kpi-goal-markers)
- [KPI Status Indicators](#kpi-status-indicators)
- [KPI Trend Indicators](#kpi-trend-indicators)
- [Configuring KpiElement](#configuring-kpielement)
- [Multiple KPI Display](#multiple-kpi-display)
- [Member-KPI Combinations](#member-kpi-combinations)
- [Visual Representation](#visual-representation)
- [Best Practices](#best-practices)

## Overview

The OLAP Gauge is specifically designed to visualize Key Performance Indicators (KPI) from OLAP cubes. Each gauge represents a combination of a member and a KPI, displaying the performance metric against its target with visual feedback indicators.

### What is a KPI?

A **Key Performance Indicator (KPI)** is a measurable value that demonstrates how effectively an organization is achieving key business objectives. In OLAP cubes, KPIs typically include:

- **Value**: The actual performance metric
- **Goal**: The target or desired value
- **Status**: Current performance state (good, warning, bad)
- **Trend**: Direction of change over time (improving, declining, stable)

## KPI Fundamentals

### KPI Structure in OLAP Cubes

OLAP cubes define KPIs with four primary components:

```
KPI: Revenue
├── Value: $1,250,000 (actual revenue)
├── Goal: $1,000,000 (target revenue)
├── Status: Good (exceeding target)
└── Trend: Up (increasing over time)
```

### How OLAP Gauge Displays KPIs

The OLAP Gauge translates KPI data into visual elements:

- **Value → Pointer**: Red needle or marker showing actual value
- **Goal → Marker**: Blue diamond or marker showing target
- **Status → Icon**: Traffic light or road sign indicating performance
- **Trend → Arrow**: Directional arrow showing improvement/decline

## KPI Components

### Four Display Elements

Each KPI can display up to four components:

1. **KPI Value** - The actual measured performance
2. **KPI Goal** - The target to achieve
3. **KPI Status** - Visual indicator of performance against goal
4. **KPI Trend** - Direction indicator showing change over time

### Component Control

Each component can be shown or hidden independently:

```csharp
KpiElement kpiElement = new KpiElement
{
    Name = "Revenue",
    ShowKPIValue = true,    // Show value pointer
    ShowKPIGoal = true,     // Show goal marker
    ShowKPIStatus = true,   // Show status icon
    ShowKPITrend = true     // Show trend arrow
};
```

## KPI Value (Pointers)

The KPI value is the actual performance metric, displayed as a **pointer** on the gauge.

### Visual Representation

- **Pointer type**: Needle or marker
- **Default color**: Red
- **Position**: Points to the actual value on the gauge scale

### Enabling KPI Value

```csharp
kpiElement.ShowKPIValue = true;
```

### Value Display Example

```csharp
KpiElements kpiElements = new KpiElements();
kpiElements.Elements.Add(new KpiElement 
{ 
    Name = "Revenue", 
    ShowKPIValue = true,    // Display the value pointer
    ShowKPIGoal = false,    // Hide goal for clarity
    ShowKPIStatus = false,
    ShowKPITrend = false 
});
```

Result: Gauge shows only the value pointer indicating actual revenue.

### Multiple Values

When dimensions create multiple members, each gets its own gauge with a value pointer:

```csharp
// Example: Revenue by Region
// Result: Multiple gauges, each showing regional revenue value
DimensionElement regionDimension = new DimensionElement();
regionDimension.Name = "Geography";
regionDimension.AddLevel("Region", "Region");

report.CategoricalElements.Add(new Item { ElementValue = regionDimension });
report.CategoricalElements.Add(new Item { ElementValue = kpiElements });
```

## KPI Goal (Markers)

The KPI goal is the target value, displayed as a **marker** on the gauge.

### Visual Representation

- **Marker type**: Diamond or triangular marker
- **Default color**: Blue
- **Position**: Points to the target value on the gauge scale

### Enabling KPI Goal

```csharp
kpiElement.ShowKPIGoal = true;
```

### Goal Display Example

```csharp
KpiElement kpiElement = new KpiElement 
{ 
    Name = "Revenue", 
    ShowKPIValue = true,    // Show actual value
    ShowKPIGoal = true,     // Show target goal
    ShowKPIStatus = false,
    ShowKPITrend = false 
};
```

Result: Gauge displays both the actual value (pointer) and target goal (marker), making it easy to compare performance against targets.

### Value vs. Goal Comparison

The visual distance between pointer and marker immediately shows performance:

- **Pointer beyond marker**: Exceeding goal (good performance)
- **Pointer at marker**: Meeting goal exactly (on target)
- **Pointer before marker**: Below goal (underperformance)

## KPI Status Indicators

KPI status provides a visual indicator of performance state using intuitive symbols.

### Status Icon Types

Common status visualizations:
- **Traffic Lights**: Red (bad), Yellow (warning), Green (good)
- **Road Signs**: Stop sign (bad), Yield (warning), Go sign (good)
- **Arrows**: Down arrow (bad), Horizontal (neutral), Up arrow (good)

### Enabling KPI Status

```csharp
kpiElement.ShowKPIStatus = true;
```

### Status Display Example

```csharp
KpiElement kpiElement = new KpiElement 
{ 
    Name = "Revenue", 
    ShowKPIValue = true,
    ShowKPIGoal = true,
    ShowKPIStatus = true,   // Display status indicator
    ShowKPITrend = false 
};
```

### Status Determination

The OLAP cube defines status thresholds:

```
Status Calculation (typical):
- Good (Green): Value >= 100% of Goal
- Warning (Yellow): Value >= 80% and < 100% of Goal
- Bad (Red): Value < 80% of Goal
```

**Note:** Actual thresholds are defined in the OLAP cube's KPI definition.

### Status Positioning

The status icon typically appears:
- At the top of the gauge
- Below the gauge header
- Separate from the gauge scale area

## KPI Trend Indicators

KPI trend shows the direction of change over time using directional arrows or symbols.

### Trend Icon Types

- **Up Arrow**: Performance improving (positive trend)
- **Down Arrow**: Performance declining (negative trend)
- **Horizontal Arrow**: Performance stable (no significant change)
- **Steep Up/Down**: Rapid improvement/decline

### Enabling KPI Trend

```csharp
kpiElement.ShowKPITrend = true;
```

### Trend Display Example

```csharp
KpiElement kpiElement = new KpiElement 
{ 
    Name = "Revenue", 
    ShowKPIValue = true,
    ShowKPIGoal = true,
    ShowKPIStatus = true,
    ShowKPITrend = true     // Display trend indicator
};
```

### Trend Calculation

Trend typically compares current period to previous period:

```
Trend = (Current Period Value - Previous Period Value) / Previous Period Value

- Positive trend: Current > Previous (up arrow)
- Negative trend: Current < Previous (down arrow)
- Stable trend: Current ≈ Previous (horizontal arrow)
```

**Note:** Trend calculation logic is defined in the OLAP cube.

### Trend Positioning

The trend icon typically appears:
- Next to the status icon
- Below the gauge header
- Provides context for the current status

## Configuring KpiElement

### Basic KpiElement

```csharp
KpiElement kpiElement = new KpiElement
{
    Name = "Revenue"  // Must match KPI name in cube
};
```

### Full Configuration

```csharp
KpiElement kpiElement = new KpiElement
{
    Name = "Revenue",
    ShowKPIValue = true,    // Show value pointer (default: true)
    ShowKPIGoal = true,     // Show goal marker (default: true)
    ShowKPIStatus = true,   // Show status icon (default: true)
    ShowKPITrend = true     // Show trend arrow (default: true)
};
```

### Selective Display

Show only the components you need:

**Value and Goal only (no status/trend):**

```csharp
KpiElement kpiElement = new KpiElement 
{ 
    Name = "Revenue", 
    ShowKPIValue = true, 
    ShowKPIGoal = true, 
    ShowKPIStatus = false, 
    ShowKPITrend = false 
};
```

**Status only (executive summary):**

```csharp
KpiElement kpiElement = new KpiElement 
{ 
    Name = "Revenue", 
    ShowKPIValue = false, 
    ShowKPIGoal = false, 
    ShowKPIStatus = true, 
    ShowKPITrend = false 
};
```

### Adding to Report

```csharp
KpiElements kpiElements = new KpiElements();
kpiElements.Elements.Add(kpiElement);

report.CategoricalElements.Add(new Item { ElementValue = kpiElements });
```

## Multiple KPI Display

### Multiple KPIs in One Report

```csharp
private OlapReport CreateMultiKpiReport()
{
    OlapReport report = new OlapReport();
    report.CurrentCubeName = "Adventure Works";

    // Create KPI collection
    KpiElements kpiElements = new KpiElements();
    
    // Add Revenue KPI
    kpiElements.Elements.Add(new KpiElement 
    { 
        Name = "Revenue", 
        ShowKPIGoal = true, 
        ShowKPIStatus = true, 
        ShowKPIValue = true, 
        ShowKPITrend = true 
    });
    
    // Add Growth KPI
    kpiElements.Elements.Add(new KpiElement 
    { 
        Name = "Growth", 
        ShowKPIGoal = true, 
        ShowKPIStatus = true, 
        ShowKPIValue = true, 
        ShowKPITrend = true 
    });
    
    // Add Profit KPI
    kpiElements.Elements.Add(new KpiElement 
    { 
        Name = "Profit", 
        ShowKPIGoal = true, 
        ShowKPIStatus = true, 
        ShowKPIValue = true 
    });

    report.CategoricalElements.Add(new Item { ElementValue = kpiElements });
    
    return report;
}
```

### Layout Configuration

For multiple KPIs, configure the gauge layout:

```csharp
// Display 3 KPIs in a row
this.OlapGauge1.ColumnsCount = 3;
this.OlapGauge1.RowsCount = 1;
```

## Member-KPI Combinations

Each gauge represents **one member against one KPI combination**.

### Understanding Member-KPI Pairing

When you have:
- 3 dimension members (e.g., North, South, East regions)
- 2 KPIs (e.g., Revenue, Profit)

Result: **6 gauges** (3 members × 2 KPIs)

### Example: Region and Product with KPI

```csharp
private OlapReport CreateMemberKpiReport()
{
    OlapReport report = new OlapReport();
    report.CurrentCubeName = "Adventure Works";

    // KPI: Revenue
    KpiElements kpiElements = new KpiElements();
    kpiElements.Elements.Add(new KpiElement 
    { 
        Name = "Revenue", 
        ShowKPIGoal = true, 
        ShowKPIStatus = true, 
        ShowKPIValue = true, 
        ShowKPITrend = true 
    });

    // Dimension 1: Sales Channel (2 members: Reseller, Internet)
    DimensionElement channelDimension = new DimensionElement();
    channelDimension.Name = "Sales Channel";
    channelDimension.AddLevel("Sales Channel", "Sales Channel");

    // Dimension 2: Product Line (1 member: Road)
    DimensionElement productDimension = new DimensionElement();
    productDimension.Name = "Product";
    productDimension.AddLevel("Product Model Lines", "Product Line");
    productDimension.Hierarchy.LevelElements["Product Line"].Add("Road");

    // Add to report
    report.CategoricalElements.Add(new Item { ElementValue = channelDimension });
    report.CategoricalElements.Add(new Item { ElementValue = kpiElements });
    report.SeriesElements.Add(new Item { ElementValue = productDimension });
    
    return report;
}
```

Result: Multiple gauges showing Revenue KPI for each channel-product combination.

### Gauge Header Display

Each gauge header shows the member-KPI combination:

```
"Reseller - Road - Revenue"
"Internet - Road - Revenue"
```

## Visual Representation

### Complete KPI Visualization

```
┌─────────────────────────────┐
│  Reseller - Road - Revenue  │  ← Header (member-KPI combination)
├─────────────────────────────┤
│     🟢 ↗                    │  ← Status (green) & Trend (up)
│                             │
│         ╱───────╲           │
│        ╱   ┃▲    ╲          │  ← Gauge with:
│       │    ┃◆     │         │    ┃ = Value pointer (red)
│       │    ┃      │         │    ◆ = Goal marker (blue)
│        ╲    ┃    ╱          │
│         ╲───────╱           │
│                             │
│   0   500K   1M   1.5M     │  ← Scale labels
└─────────────────────────────┘
```

### Component Layout

1. **Top section**: Header showing dimension members and KPI name
2. **Middle-top**: Status icon and trend arrow
3. **Center**: Circular gauge with pointers and markers
4. **Bottom**: Scale labels showing value ranges

## Best Practices

### 1. Choose Relevant KPI Components

Don't always show all four components. Consider your audience:

**Executive dashboard** (high-level overview):

```csharp
ShowKPIStatus = true;   // Quick visual feedback
ShowKPITrend = true;    // Direction of change
ShowKPIValue = false;   // Hide details
ShowKPIGoal = false;
```

**Operational dashboard** (detailed metrics):

```csharp
ShowKPIValue = true;    // Actual numbers
ShowKPIGoal = true;     // Targets
ShowKPIStatus = true;   // Performance indicator
ShowKPITrend = false;   // Less critical for operations
```

### 2. Limit KPI Count

Too many KPIs reduce impact:

- **3-5 KPIs**: Ideal for focused dashboards
- **6-10 KPIs**: Acceptable for comprehensive views
- **10+ KPIs**: Consider multiple pages or filtering

### 3. Consistent Status Definitions

Ensure KPI status thresholds in your cube are consistent:

```
Good: >= 95% of goal
Warning: 80-95% of goal
Bad: < 80% of goal
```

### 4. Meaningful KPI Names

Use clear, business-friendly KPI names in your cube:

**Good:** "Revenue", "Customer Satisfaction", "Order Fulfillment Rate"

**Avoid:** "KPI1", "Metric_A", "calc_value_001"

### 5. Dimension Selection

Choose dimensions that provide meaningful context:

**Good combinations:**
- Time Period + KPI (trend over time)
- Region + KPI (geographic performance)
- Product Line + KPI (product performance)

**Avoid:**
- Too many dimension members (creates too many gauges)
- Irrelevant dimensions (e.g., Employee ID + Revenue)

### 6. Layout Configuration

Match layout to KPI count:

```csharp
// 4 KPIs: 2x2 grid
OlapGauge1.ColumnsCount = 2;
OlapGauge1.RowsCount = 2;

// 3 KPIs: 3x1 horizontal
OlapGauge1.ColumnsCount = 3;
OlapGauge1.RowsCount = 1;

// 6 KPIs: 3x2 grid
OlapGauge1.ColumnsCount = 3;
OlapGauge1.RowsCount = 2;
```

### 7. KPI Validation

Verify KPIs exist in your cube before adding to report:

```csharp
// In cube definition, ensure KPI exists:
// - KPI Name: "Revenue"
// - KPI Value: [Measures].[Revenue Amount]
// - KPI Goal: [Measures].[Revenue Goal]
// - KPI Status: Status expression
// - KPI Trend: Trend expression
```

### 8. Tooltip Enhancement

Enable tooltips for detailed information:

```csharp
OlapGauge1.ShowPointersTooltip = true;  // Show value details
OlapGauge1.ShowMarkersTooltip = true;   // Show goal details
```

Users can hover over pointers and markers to see exact values.

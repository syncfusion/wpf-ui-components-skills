# KPI and Member Properties

## Table of Contents
- [Key Performance Indicators (KPI)](#key-performance-indicators-kpi)
- [Member Properties](#member-properties)
- [Configuration](#configuration)

## Key Performance Indicators (KPI)

KPIs are special measures in OLAP cubes that represent business metrics with status and trend indicators.

### KPI Overview

**KPI Components:**
- **Value** - Actual measure value
- **Goal** - Target value
- **Status** - Current state (achieved, at risk, failed)
- **Trend** - Direction of change (improving, stable, declining)

### Using KPIs

When your OLAP cube defines KPIs, the OLAP Grid automatically displays them with visual indicators:

```csharp
// KPIs are defined in the cube, not in code
// Add KPI measure to report
MeasureElements measures = new MeasureElements();
measures.Elements.Add(new MeasureElement { Name = "Revenue KPI" });
```

**Visual Indicators:**
- Green checkmark - Goal achieved
- Yellow warning - At risk
- Red X - Below target
- Arrows - Trend direction

## Member Properties

Member properties are additional attributes associated with dimension members that provide context.

### What Are Member Properties?

**Examples:**
- **Customer dimension:** Customer Name, Phone, Email, Address
- **Product dimension:** Product Code, Category, Color, Size
- **Geography dimension:** Population, Area Code, Region Code

### Displaying Member Properties

Use the Excel-Like Layout with Member Properties to show these attributes:

```csharp
olapGrid1.Layout = GridLayout.ExcelLikeLayoutWithMemberProperties;
```

### Prerequisites

- Member properties must be defined in the OLAP cube
- Properties must be bound to dimension in OlapReport
- Appropriate layout must be selected

### Example

```csharp
// Member properties are defined in cube
// Configure dimension that has properties
DimensionElement customer = new DimensionElement();
customer.Name = "Customer";
customer.AddLevel("Customer", "Customer ID");

// Set layout to display properties
olapGrid1.Layout = GridLayout.ExcelLikeLayoutWithMemberProperties;
```

**Result:** Grid shows member properties in adjacent columns next to dimension members.

## Configuration

### OlapReport for KPI

```csharp
private OlapReport CreateKPIReport()
{
    OlapReport report = new OlapReport();
    report.CurrentCubeName = "Adventure Works";
    
    // Add KPI measure
    MeasureElements measures = new MeasureElements();
    measures.Elements.Add(new MeasureElement { Name = "Revenue KPI" });
    measures.Elements.Add(new MeasureElement { Name = "Revenue" });
    
    // Add dimensions
    DimensionElement date = new DimensionElement();
    date.Name = "Date";
    date.AddLevel("Fiscal", "Fiscal Year");
    
    report.CategoricalElements.Add(measures);
    report.SeriesElements.Add(date);
    
    return report;
}
```

### Layout for Member Properties

```csharp
// Ensure layout supports member properties
olapGrid1.Layout = GridLayout.ExcelLikeLayoutWithMemberProperties;
olapGrid1.DataBind();
```

## Best Practices

**For KPIs:**
- Use KPIs for executive dashboards
- Combine KPI indicators with actual values
- Provide legend explaining status icons
- Ensure cube KPIs are well-designed

**For Member Properties:**
- Display only essential properties (avoid clutter)
- Use consistent property names across dimensions
- Consider column width for property display
- Test with representative data to ensure readability

## Troubleshooting

**KPIs not showing:**
- Verify KPIs are defined in cube
- Check KPI measure name matches cube definition
- Ensure cube is processed with KPI calculations

**Member properties not appearing:**
- Verify layout is `ExcelLikeLayoutWithMemberProperties`
- Check properties are defined in cube for the dimension
- Confirm dimension includes levels with properties
- Rebind grid after changing layout

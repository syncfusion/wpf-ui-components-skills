---
title: Color Mapping for SfHeatMap
---

# Color Mapping

Use `ColorMappingCollection` to map numeric values to colors. Intermediate values are interpolated between defined stops.

Example:

```xaml
<syncfusion:ColorMappingCollection x:Key="colorMapping">
  <syncfusion:ColorMapping Value="0" Color="White"/>
  <syncfusion:ColorMapping Value="30" Color="Red"/>
</syncfusion:ColorMappingCollection>
```

When to use:
- To emphasize ranges (low → high)
- To create semantic buckets with labeled legend entries

Edge case:
- Ensure mapping range covers your data; otherwise colors may be skewed.

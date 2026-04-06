# Formula Cells in WPF GridControl

## Table of Contents
- [Enabling Formula Cells](#enabling-formula-cells)
- [Writing Formulas](#writing-formulas)
- [Formula Library](#formula-library)
- [Arithmetic Operators and Precedence](#arithmetic-operators-and-precedence)
- [Cell References](#cell-references)
- [Named Ranges](#named-ranges)
- [Adding Custom Formula Functions](#adding-custom-formula-functions)
- [Cross-Sheet References](#cross-sheet-references)

---

## Enabling Formula Cells

### Enable for a single cell

```csharp
gridControl.Model[3, 2].CellType = "FormulaCell";
gridControl.Model[3, 2].CellValue = "=A1+B1";
```

### Enable for the entire grid

Apply `FormulaCell` to the StandardStyle so every cell can potentially hold a formula. Cells whose value does **not** start with `=` behave as ordinary text boxes.

```csharp
gridControl.BaseStylesMap["Standard"].StyleInfo.CellType = "FormulaCell";
```

### Enable for a column

```csharp
gridControl.Model.ColStyles[3].CellType = "FormulaCell";
```

---

## Writing Formulas

Formulas must begin with `=`. Cell references use column-letter + row-number notation (A1 style):

```csharp
// Sum of column A rows 1–10
gridControl.Model[11, 1].CellValue = "=Sum(A1:A10)";

// Arithmetic with function
gridControl.Model[12, 1].CellValue = "=Sqrt(Pow(A2,2)+Pow(B2,2))";

// Nested functions
gridControl.Model[13, 1].CellValue = "=Round(Avg(A1:A10), 2)";
```

Column mapping in A1 notation:
- Column 1 → `A`, Column 2 → `B`, … Column 27 → `AA`, Column 53 → `BA`

---

## Formula Library

The built-in formula library wraps .NET's `System.Math` plus additional grid-specific functions.

### Categories of built-in functions

| Category | Examples |
|---|---|
| Math | `Abs`, `Sqrt`, `Pow`, `Round`, `Floor`, `Ceiling`, `Log`, `Exp` |
| Trig | `Sin`, `Cos`, `Tan`, `Asin`, `Acos`, `Atan` |
| Aggregate | `Sum`, `Avg`, `Count`, `Max`, `Min` |
| String | `Concat`, `Len`, `Left`, `Right`, `Mid` |
| Logic | `If`, `And`, `Or`, `Not` |

```csharp
// Example: Using Sum, Avg, and If
gridControl.Model[5, 1].CellValue = "=Sum(A1:A4)";
gridControl.Model[5, 2].CellValue = "=Avg(B1:B4)";
gridControl.Model[5, 3].CellValue = "=If(A5>100, \"High\", \"Low\")";
```

---

## Arithmetic Operators and Precedence

Nine supported operators, evaluated in this order (left-to-right within the same level):

| Precedence | Operators | Symbols |
|---|---|---|
| 1st (highest) | Multiplication, Division | `*`, `/` |
| 2nd | Addition, Subtraction | `+`, `-` |
| Grouping | Parentheses | `(` `)` |

```csharp
// Parentheses control evaluation order
gridControl.Model[6, 1].CellValue = "=(A1+B1)*C1";   // add first, then multiply
gridControl.Model[6, 2].CellValue = "=A1+B1*C1";     // multiply first (standard)
```

---

## Cell References

### Absolute cell reference

```csharp
gridControl.Model[7, 1].CellValue = "=A1";           // column 1, row 1
gridControl.Model[7, 2].CellValue = "=BA3";          // column 53, row 3
```

### Range reference

```csharp
gridControl.Model[8, 1].CellValue = "=Sum(A1:A10)";  // rows 1–10 in column A
gridControl.Model[8, 2].CellValue = "=Avg(A1:D1)";   // columns A–D in row 1
```

### Using row/column index to build reference strings

```csharp
private string CellRef(int col, int row)
{
    // col is 1-based
    string colName = col <= 26
        ? ((char)('A' + col - 1)).ToString()
        : ((char)('A' + (col / 26) - 1)).ToString() +
          ((char)('A' + (col % 26) - 1)).ToString();
    return colName + row.ToString();
}

// e.g., column 2, row 5 → "B5"
gridControl.Model[10, 1].CellValue = $"={CellRef(2, 5)}+{CellRef(3, 5)}";
```

---

## Named Ranges

Name a range for use in formulas:

```csharp
// Define a named range
gridControl.Model.FormulaEngine.SetNamedRange("SalesData", "A1:A12");

// Use the named range in a formula
gridControl.Model[13, 1].CellValue = "=Sum(SalesData)";
gridControl.Model[13, 2].CellValue = "=Avg(SalesData)";
```

---

## Adding Custom Formula Functions

Extend the formula library with your own functions by registering them with `GridFormulaEngine`:

```csharp
// Register a custom function named "Double"
gridControl.Model.FormulaEngine.AddFunction("Double", DoubleValue);

private string DoubleValue(string args)
{
    // args is a comma-separated string of arguments
    if (double.TryParse(args, out double val))
        return (val * 2).ToString();
    return "!ERROR";
}

// Use in a cell
gridControl.Model[14, 1].CellValue = "=Double(A1)";
```

The function must accept a `string` of arguments and return a `string` result.

---

## Cross-Sheet References

GridControl supports referencing cells in other GridModel instances using the formula engine's cross-reference support (imported from Excel or set up manually):

```csharp
// When importing from Excel, cross-sheet formulas like =Sheet2!A1
// are automatically preserved
gridControl.Model.ImportFromExcel("Workbook.xlsx");
```

> For manually constructed cross-sheet formulas, configure the formula engine's sheet dictionary so it knows how to resolve sheet names to GridModel instances.

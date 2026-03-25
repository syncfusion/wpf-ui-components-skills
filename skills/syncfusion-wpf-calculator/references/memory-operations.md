# Memory Operations in WPF Calculator

## Table of Contents
- [Memory Overview](#memory-overview)
- [MS Button (Memory Store)](#ms-button-memory-store)
- [MR Button (Memory Restore)](#mr-button-memory-restore)
- [M+ Button (Memory Add)](#m-button-memory-add)
- [M- Button (Memory Subtract)](#m--button-memory-subtract)
- [MC Button (Memory Clear)](#mc-button-memory-clear)
- [Memory Operation Workflow](#memory-operation-workflow)
- [Practical Examples](#practical-examples)
- [Best Practices](#best-practices)

## Memory Overview

The **Memory** property in SfCalculator stores a decimal value that persists across multiple calculations until manually cleared.

**Key Characteristics:**
- **Read-Only Property:** Memory value can only be modified through calculator buttons (MS, MR, M+, M-, MC)
- **Type:** Decimal
- **Initial Value:** 0 (zero)
- **Persistence:** Retained until cleared with MC button
- **Scope:** Available throughout the calculator's lifetime

**When to Use Memory:**
- Store intermediate results
- Accumulate values across multiple operations
- Avoid losing values during long calculations
- Perform calculations with reference values

## MS Button (Memory Store)

The **MS (Memory Store)** button stores the current calculator value into memory.

**Behavior:**
- Takes the value currently displayed in calculator
- Stores it as the memory value
- Replaces any previously stored memory value
- Does NOT affect current display

**Workflow:**
```
1. User enters/calculates value (e.g., 150)
2. User clicks MS button
3. Memory now contains: 150
4. Display still shows: 150
5. User can continue new calculations
```

**Use Scenario:**
```csharp
// Financial example: Store a base amount
// User enters: 1000
// User clicks: MS
// Memory = 1000
// Now user can perform other calculations and refer back to 1000
```

**Button Interaction:**
- Located on calculator keypad
- Single click to store
- Overwrites previous memory value
- Works with any numeric value

## MR Button (Memory Restore)

The **MR (Memory Restore)** button retrieves the value stored in memory and displays it.

**Behavior:**
- Takes the stored memory value
- Displays it in the calculator's value area
- Replaces the current display value
- Does NOT clear memory

**Workflow:**
```
1. Memory contains: 500 (from previous MS button)
2. User performs other calculations
3. User clicks MR button
4. Display shows: 500 (from memory)
5. Memory still contains: 500
```

**Use Scenario:**
```csharp
// Reference value retrieval
// Memory stored: 50 (base price)
// User calculates: 100 + 20
// User clicks: MR (displays 50 from memory)
// User can now use 50 in further calculations
```

**Button Interaction:**
- Single click to restore
- Shows memory value without modification
- Useful for repeating reference calculations

## M+ Button (Memory Add)

The **M+ (Memory Add)** button adds the current calculator value to the stored memory value.

**Behavior:**
- Adds current display value to memory
- Updates memory with new sum
- Does NOT change display value
- Result stored in memory

**Formula:**
```
New Memory Value = Old Memory Value + Current Display Value
```

**Workflow:**
```
1. Memory contains: 100
2. Display shows: 50
3. User clicks M+ button
4. Memory becomes: 150 (100 + 50)
5. Display still shows: 50
```

**Practical Example - Accumulating Expenses:**
```
Initial: Memory = 0

// First expense: 45.50
User enters: 45.50
User clicks: M+
Memory = 45.50

// Second expense: 32.75
User enters: 32.75
User clicks: M+
Memory = 78.25

// Third expense: 120.00
User enters: 120.00
User clicks: M+
Memory = 198.25 (Total accumulated)
```

**Use Scenario:**
- Running totals
- Sum accumulation
- Expense tracking
- Value collection

## M- Button (Memory Subtract)

The **M- (Memory Subtract)** button subtracts the current calculator value from the stored memory value.

**Behavior:**
- Subtracts current display value from memory
- Updates memory with new difference
- Does NOT change display value
- Result stored in memory

**Formula:**
```
New Memory Value = Old Memory Value - Current Display Value
```

**Workflow:**
```
1. Memory contains: 500
2. Display shows: 75
3. User clicks M- button
4. Memory becomes: 425 (500 - 75)
5. Display still shows: 75
```

**Practical Example - Budget Tracking:**
```
Initial: Memory = 1000 (Total budget)

// Spent on food: 150
User enters: 150
User clicks: M-
Memory = 850 (Remaining: 1000 - 150)

// Spent on transport: 75
User enters: 75
User clicks: M-
Memory = 775 (Remaining: 850 - 75)

// Can check remaining anytime by clicking MR
User clicks: MR
Display shows: 775 (Budget remaining)
```

**Use Scenario:**
- Budget deduction
- Inventory reduction
- Debt calculation
- Remaining balance tracking

## MC Button (Memory Clear)

The **MC (Memory Clear)** button clears the memory by resetting it to zero.

**Behavior:**
- Sets memory value to 0
- Does NOT affect display value
- Clears previous stored value
- Allows fresh memory operations

**Workflow:**
```
1. Memory contains: 500 (from previous operations)
2. User clicks MC button
3. Memory becomes: 0
4. Display unchanged
5. Next MS operation will store fresh value
```

**Use Scenario:**
```csharp
// After completing one calculation set
// User clicks: MC
// Memory reset to 0
// Ready for next calculation group
```

**When to Use:**
- Starting new calculation session
- Switching calculation contexts
- Clearing previous values
- Error recovery

## Memory Operation Workflow

**Complete Workflow Example - Calculating Average:**

```
Goal: Calculate average of three values (100, 200, 150)

Step 1: Clear memory
- Click MC
- Memory = 0, Display = 0

Step 2: Enter first value and store
- Enter: 100
- Click MS
- Memory = 100, Display = 100

Step 3: Add second value
- Enter: 200
- Click M+
- Memory = 300, Display = 200

Step 4: Add third value
- Enter: 150
- Click M+
- Memory = 450, Display = 150

Step 5: Retrieve total
- Click MR
- Memory = 450, Display = 450

Step 6: Calculate average
- Enter: 450 / 3 = 150
- Average = 150
```

**Workflow Visualization:**
```
MC → MS(100) → M+(200) → M+(150) → MR → ÷3 → =
```

## Practical Examples

**Example 1: Monthly Expense Tracking**
```
Starting budget: $2000 in memory

January expenses:
- Enter: 450, Click M-  → Memory = 1550
- Enter: 320, Click M-  → Memory = 1230
- Enter: 175, Click M-  → Memory = 1055
- Click MR to see remaining: 1055
```

**Example 2: Inventory Management**
```
Starting inventory: 500 units in memory

Sales transactions:
- Enter: 25, Click M-   → Memory = 475
- Enter: 40, Click M-   → Memory = 435
- Enter: 15, Click M-   → Memory = 420
- Click MR to check remaining: 420 units
```

**Example 3: Calculation Reference**
```
Base value: $100 (store in memory)

Calculate multiple percentages of base:
- Click MS after entering 100
- Enter: 20% of 100
  Calculate: 100 * 0.2 = 20
- Enter: 50% of 100
  Calculate: 100 * 0.5 = 50
- Click MR anytime to return to base: 100
```

**Example 4: Accumulating Scores**
```
Game scoring:

Round 1: 150 points
- Enter: 150, Click MS
- Memory = 150

Round 2: 250 points
- Enter: 250, Click M+
- Memory = 400

Round 3: 180 points
- Enter: 180, Click M+
- Memory = 580

Final Score: Click MR → 580 total points
```

## Best Practices

**1. Clear Memory Between Sessions**
```csharp
// Always start fresh for new calculations
calculator.// User clicks MC to clear
```
**Why:** Prevents confusion with old values and ensures accurate new calculations.

**2. Use MR to Verify Stored Value**
```csharp
// Before performing M+ or M-, check what's in memory
// Click MR to see current memory value
// Proceed with operation only if memory is correct
```
**Why:** Prevents accidental incorrect accumulation.

**3. Choose Right Operation**
```csharp
// M+ for accumulating totals (expenses, scores, inventory additions)
// M- for deducting amounts (budget tracking, inventory removal)
// MS for storing reference value (base amount, previous result)
// MR for retrieving stored value (don't click when value is needed in display)
// MC for clearing and starting fresh
```

**4. Document Memory Usage in UI**
```
Display: Shows calculation result
Status Bar: Shows current memory value
Label: "Memory: 450" helps user track what's stored
```

**Why:** Users know what's in memory without clicking MR.

**5. Avoid Common Mistakes**
```
❌ Mistake: Click M+ when memory is empty (0)
✅ Better: First click MS to store a value, then use M+

❌ Mistake: Forget to click MC between different calculation sets
✅ Better: Always MC at start of new calculation session

❌ Mistake: Modify display after storing in memory
✅ Better: Use MR to retrieve, not manual entry
```

**6. Typical Workflows**
- **Accumulation:** MC → MS → M+ → M+ → MR (for total)
- **Reference:** MS → [calculations] → MR (to return to reference)
- **Deduction:** MS → M- → M- → MR (for remaining)
- **Mixed:** MC → MS → M+ → M- → MR (complex calculations)

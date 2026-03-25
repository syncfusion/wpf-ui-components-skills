# Properties and Values in WPF Calculator

## Table of Contents
- [Value Property](#value-property)
- [DefaultValue Property](#defaultvalue-property)
- [DisplayText Property](#displaytext-property)
- [Memory Property](#memory-property)
- [Managing Display State](#managing-display-state)
- [Data Binding Examples](#data-binding-examples)

## Value Property

The **Value** property contains the calculated result from the last mathematical expression entered in the calculator.

**Key Characteristics:**
- **Read-Only:** Cannot be set directly; only retrieved after calculations
- **Type:** Decimal
- **Format:** Decimal format (not string)
- **When Updated:** After each calculation when user presses an operator or equals button
- **Initial State:** 0 (zero) if no calculation performed yet

**Retrieving Calculated Values:**
```csharp
SfCalculator calculator = new SfCalculator();
decimal result = calculator.Value; // Get the calculated value
```

**Usage Scenarios:**
```csharp
// Store result for later use
decimal savedResult = calculator.Value;

// Use in conditional logic
if (calculator.Value > 1000)
{
    MessageBox.Show("Value exceeds threshold");
}

// Pass to another component
OtherControl.SetValue(calculator.Value);

// Log for audit trail
System.Diagnostics.Debug.WriteLine($"Calculation result: {calculator.Value}");
```

**Important Notes:**
- Value is updated only after complete expressions (e.g., after pressing = or an operator)
- Intermediate values are not accessible through Value property
- Accessing Value immediately after initialization returns 0
- Use EventHandlers to detect when Value changes (see memory operations guide)

## DefaultValue Property

The **DefaultValue** property sets the initial value displayed in the calculator when it first loads.

**Key Characteristics:**
- **Read-Write:** Can be set and retrieved
- **Type:** Decimal
- **Purpose:** Pre-populate calculator with starting value
- **Timing:** Displayed at control initialization
- **Format:** Decimal format

**Setting Default Value:**
```csharp
// XAML
<syncfusion:SfCalculator DefaultValue="500" />

// C#
calculator.DefaultValue = 500;
```

**Practical Examples:**
```csharp
// Financial app with starting balance
SfCalculator calculator = new SfCalculator();
calculator.DefaultValue = 1000.00m; // Starting with $1000

// Scientific calculator with common value
calculator.DefaultValue = 3.14159; // Pi value

// Reset to zero
calculator.DefaultValue = 0;

// Using variable
decimal startingAmount = 250.75m;
calculator.DefaultValue = startingAmount;
```

**User Experience Impact:**
- Helps users understand what value is displayed
- Reduces need for initial input
- Useful for template-based calculations
- Pre-fills calculator in data-entry scenarios

## DisplayText Property

The **DisplayText** property shows placeholder or watermark text in the calculator's value display area.

**Key Characteristics:**
- **Read-Write:** Can be set and retrieved
- **Type:** String
- **Purpose:** Hint or instruction text
- **Display Behavior:** Shows when value area is empty or as watermark
- **Use Case:** User guidance and UI polish

**Setting Display Text:**
```csharp
// XAML
<syncfusion:SfCalculator DisplayText="Enter value here" />

// C#
calculator.DisplayText = "Enter value here";
```

**Professional Display Examples:**
```csharp
// Instruction-based
calculator.DisplayText = "Enter your calculation";

// Context-specific
calculator.DisplayText = "Daily Budget Calculation";

// Language-aware
calculator.DisplayText = "Ingrese el valor"; // Spanish

// Symbol-based
calculator.DisplayText = "💰 Amount";
```

**Best Practices:**
- Keep display text concise (20-30 characters)
- Use action verbs when appropriate ("Enter calculation", "Type amount")
- Provide context about expected input
- Use consistent terminology with application

## Memory Property

The **Memory** property contains the value stored in the calculator's memory.

**Key Characteristics:**
- **Read-Only:** Cannot be set directly; only read
- **Type:** Decimal
- **Initial Value:** 0 (zero)
- **Modified By:** Memory buttons (MS, MR, M+, M-, MC) in calculator UI
- **Purpose:** Store and retrieve values during calculations

**Accessing Memory Value:**
```csharp
decimal storedMemory = calculator.Memory;

if (calculator.Memory > 0)
{
    MessageBox.Show($"Memory contains: {calculator.Memory}");
}
```

**Memory Operations Overview:**
```
MS (Store)  → calculator.Memory = current value
MR (Restore)→ Retrieves calculator.Memory
M+ (Add)    → calculator.Memory += current value
M- (Subtract)→ calculator.Memory -= current value
MC (Clear)  → calculator.Memory = 0
```

See **memory-operations.md** for detailed memory operation workflows.

## Managing Display State

**Complete Property Management Example:**
```csharp
public partial class MainWindow : Window
{
    private SfCalculator calculator;
    
    public MainWindow()
    {
        InitializeComponent();
        
        // Initialize calculator
        calculator = new SfCalculator();
        
        // Set initial state
        calculator.DefaultValue = 0;
        calculator.DisplayText = "Enter your calculation";
        
        this.Content = calculator;
    }
    
    // Method to check calculator state
    private void CheckCalculatorState()
    {
        decimal currentValue = calculator.Value;
        decimal memoryValue = calculator.Memory;
        
        status.Text = $"Value: {currentValue}, Memory: {memoryValue}";
    }
    
    // Method to reset calculator
    private void ResetCalculator()
    {
        calculator.DefaultValue = 0;
        calculator.DisplayText = "Ready for input";
    }
}
```

## Data Binding Examples

**Binding Value Property to TextBlock (Read-Only):**
```xaml
<Grid>
    <StackPanel>
        <syncfusion:SfCalculator x:Name="calculator" />
        <TextBlock Text="{Binding ElementName=calculator, Path=Value}" />
    </StackPanel>
</Grid>
```

**Binding DefaultValue for Dynamic Updates:**
```xaml
<Window xmlns:syncfusion="http://schemas.syncfusion.com/wpf">
    <Grid>
        <StackPanel>
            <Slider x:Name="slider" Minimum="0" Maximum="1000" />
            <syncfusion:SfCalculator DefaultValue="{Binding ElementName=slider, Path=Value}" />
        </StackPanel>
    </Grid>
</Window>
```

**MVVM Pattern Example:**
```csharp
public class CalculatorViewModel : INotifyPropertyChanged
{
    private decimal _calculatedValue;
    
    public decimal CalculatedValue
    {
        get { return _calculatedValue; }
        set
        {
            if (_calculatedValue != value)
            {
                _calculatedValue = value;
                OnPropertyChanged(nameof(CalculatedValue));
            }
        }
    }
    
    public void UpdateFromCalculator(SfCalculator calc)
    {
        CalculatedValue = calc.Value;
    }
}
```

**Key Binding Patterns:**
- Value is read-only (one-way binding only)
- DefaultValue supports two-way binding for dynamic updates
- DisplayText supports one-way binding
- Memory is read-only (monitoring only)

# Advanced Features

Guide to advanced capabilities of the SfDomainUpDown control including auto-reverse cycling, gesture support, and optimization techniques.

## AutoReverse Property

The `AutoReverse` property enables continuous cycling through items - when reaching the end of the list, it automatically wraps to the beginning (and vice versa).

### Property Details

**Type:** `bool`  
**Default:** `false`

### Behavior

**When AutoReverse = True:**
- Clicking Up on the last item moves to the first item
- Clicking Down on the first item moves to the last item
- Provides seamless, infinite cycling

**When AutoReverse = False (Default):**
- Up/Down buttons disabled when at list boundaries
- Users must reverse direction manually
- Prevents accidental wraparound

### XAML Example

```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center"
                           Width="200"
                           Height="35"
                           AutoReverse="True"
                           ItemsSource="{Binding Employees}">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}"/>
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

### C# Example

```csharp
SfDomainUpDown domainUpDown = new SfDomainUpDown();
domainUpDown.AutoReverse = true;
domainUpDown.ItemsSource = new List<string> { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday" };
domainUpDown.Value = "Friday";

// User clicks Up button -> Value becomes "Monday"
```

### When to Use AutoReverse

**Enable AutoReverse (true) for:**
- Cyclical data (days of the week, months)
- Continuous browsing through items
- Carousels and rotating selections
- When boundaries are artificial

**Disable AutoReverse (false) for:**
- Hierarchical data with clear start/end
- Sequential processes with defined order
- When wraparound could cause confusion
- Priority levels (Low → High should not wrap to Low)

### AutoReverse with Collection

Set `AutoReverse="True"` with string array or collection to enable seamless cycling through values like days of the week.

## Mouse Wheel Gesture Support

The control automatically responds to mouse wheel scrolling when the control has focus.

### Default Behavior

- **Scroll Up:** Move to previous item (decrement)
- **Scroll Down:** Move to next item (increment)
- **Requires Focus:** Control must have keyboard focus to respond

### Example with Focus

```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown"
                           Width="200"
                           Height="35"
                           Focusable="True"
                           ItemsSource="{Binding Items}"/>
```

### Programmatically Setting Focus

```csharp
// Ensure control responds to mouse wheel
domainUpDown.Focus();
```

### Handling ValueChanged Event

```csharp
domainUpDown.ValueChanged += (sender, e) =>
{
    Console.WriteLine($"Value changed from {e.OldValue} to {e.NewValue}");
    // Additional logic here
};
```

### Disabling Mouse Wheel (If Needed)

While there's no built-in property to disable mouse wheel, you can handle it:

```csharp
domainUpDown.PreviewMouseWheel += (sender, e) =>
{
    e.Handled = true; // Prevent mouse wheel scrolling
};
```

## Keyboard Navigation

The control supports keyboard interaction for accessibility and power users.

### Supported Keys

| Key | Action |
|-----|--------|
| **Up Arrow** | Move to previous item |
| **Down Arrow** | Move to next item |
| **Page Up** | Move to previous item |
| **Page Down** | Move to next item |
| **Home** | Move to first item |
| **End** | Move to last item |
| **Tab** | Move focus to next control |
| **Shift+Tab** | Move focus to previous control |

### Example with Keyboard Hints

```xml
<StackPanel>
    <TextBlock Text="Use Arrow Keys or Mouse Wheel to navigate" 
              Margin="0,0,0,10" 
              FontSize="11" 
              Foreground="Gray"/>
    
    <syncfusion:SfDomainUpDown x:Name="domainUpDown"
                               Width="200"
                               Height="35"
                               ItemsSource="{Binding Items}"/>
</StackPanel>
```

### Accessibility Support

```xml
<syncfusion:SfDomainUpDown x:Name="domainUpDown"
                           AutomationProperties.Name="Employee Selector"
                           AutomationProperties.HelpText="Use arrow keys to cycle through employees"/>
```

## Edge Cases and Considerations

### Empty ItemsSource

**Problem:** No items in the collection.

**Behavior:**
- Control displays empty
- Spin buttons are disabled
- No errors thrown

**Solution:**
```csharp
if (domainUpDown.ItemsSource == null || !domainUpDown.ItemsSource.Cast<object>().Any())
{
    // Populate with default items or show message
    domainUpDown.ItemsSource = new List<string> { "No items available" };
}
```

### Single Item in List

**Problem:** Only one item in ItemsSource.

**Behavior:**
- Spin buttons remain visible but disabled
- Value doesn't change when clicked
- AutoReverse has no effect

**Recommendation:** Hide or disable control if only one item exists:

```csharp
domainUpDown.IsEnabled = domainUpDown.ItemsSource?.Cast<object>().Count() > 1;
```

### Null Values

**Problem:** ItemsSource contains null items.

**Behavior:**
- Control displays empty when null item is selected
- May cause display issues

**Solution:** Filter nulls before binding:

```csharp
domainUpDown.ItemsSource = employees.Where(e => e != null).ToList();
```

### Duplicate Items

**Problem:** ItemsSource contains duplicate values.

**Behavior:**
- All duplicates are displayed
- User may be confused by repeated values
- Value comparison works correctly

**Recommendation:** Use distinct items or add identifiers in ContentTemplate:

```xml
<syncfusion:SfDomainUpDown.ContentTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="{Binding Name}"/>
            <TextBlock Text="{Binding Id, StringFormat=' (ID: {0})'}" 
                      FontSize="10" 
                      Foreground="Gray"/>
        </StackPanel>
    </DataTemplate>
</syncfusion:SfDomainUpDown.ContentTemplate>
```

### Dynamic Collection Updates

**Problem:** Items added/removed after control is populated.

**Solution:** Use ObservableCollection for automatic UI updates:

```csharp
public ObservableCollection<Employee> Employees { get; set; }

// Items automatically reflect in UI
Employees.Add(new Employee { Name = "New Employee" });
Employees.RemoveAt(0);
```

## Performance Optimization

### Large Collections

**Issue:** Slow performance with hundreds/thousands of items.

**Recommendation:** DomainUpDown is optimized for small-to-medium collections (< 100 items). For larger datasets:

1. **Use ComboBox** instead for searchable dropdown
2. **Implement paging** if cycling is required
3. **Filter data** to show only relevant items

### Complex ContentTemplates

**Issue:** Slow animations with complex templates.

**Solution:**
```xml
<!-- Disable animation for complex templates -->
<syncfusion:SfDomainUpDown EnableSpinAnimation="False">
    <syncfusion:SfDomainUpDown.ContentTemplate>
        <DataTemplate>
            <!-- Complex layout here -->
        </DataTemplate>
    </syncfusion:SfDomainUpDown.ContentTemplate>
</syncfusion:SfDomainUpDown>
```

### Data Binding Optimization

Use appropriate binding modes:

```xml
<!-- Read-only display: OneWay -->
<TextBlock Text="{Binding ElementName=domainUpDown, Path=Value, Mode=OneWay}"/>

<!-- Two-way sync with ViewModel: TwoWay -->
<syncfusion:SfDomainUpDown Value="{Binding SelectedEmployee, Mode=TwoWay}"/>
```

## Best Practices Summary

1. **AutoReverse:** Enable for cyclical data, disable for sequential/hierarchical data
2. **Mouse Wheel:** Ensure control can receive focus for gesture support
3. **Keyboard:** Leverage built-in keyboard navigation for accessibility
4. **Edge Cases:** Validate ItemsSource is populated and handle empty/single-item scenarios
5. **Performance:** Use ObservableCollection for dynamic data, disable animations for complex templates
6. **Collections:** Keep item count reasonable (<100) for optimal performance

## Troubleshooting

**Issue:** AutoReverse doesn't work  
**Solution:** Verify AutoReverse="True" is set and ItemsSource has multiple items

**Issue:** Mouse wheel doesn't scroll items  
**Solution:** Ensure control has focus (click on it first or call `Focus()`)

**Issue:** Keyboard navigation not working  
**Solution:** Check that control is focusable and has keyboard focus

**Issue:** Performance issues with animations  
**Solution:** Set `EnableSpinAnimation="False"` or simplify ContentTemplate

**Issue:** Items don't update when collection changes  
**Solution:** Use `ObservableCollection<T>` instead of `List<T>`

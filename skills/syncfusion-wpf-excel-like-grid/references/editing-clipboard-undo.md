# Editing, Clipboard, and Undo/Redo in WPF GridControl

## Clipboard Operations

GridControl provides full clipboard support for copying, cutting, and pasting both text and cell style data to/from other OLE-enabled applications (Notepad, Excel, etc.).

### CopyPasteOption Flags

Configure what is copied/pasted using the `CopyPasteOption` property:

```csharp
// Copy and paste text only (no styles)
gridControl.Model.Options.CopyPasteOption =
    CopyPaste.CopyText | CopyPaste.CutText | CopyPaste.PasteText;

// Copy and paste cells with style information
gridControl.Model.Options.CopyPasteOption =
    CopyPaste.CopyCellData | CopyPaste.CutCell | CopyPaste.PasteCell;

// XML format — compatible with pasting into Microsoft Excel
// Also supports copying formula values
gridControl.Model.Options.CopyPasteOption |= CopyPaste.XmlCopyPaste;

// Exclude the current cell from clipboard operations
gridControl.Model.Options.CopyPasteOption |= CopyPaste.ExcludeCurrentCell;
```

### Clipboard Events

Hook these events to validate or cancel clipboard actions:

```csharp
gridControl.ClipboardCanCopy  += (s, e) => { /* set e.Handled = true to cancel */ };
gridControl.ClipboardCanCut   += (s, e) => { };
gridControl.ClipboardCanPaste += (s, e) => { };
gridControl.ClipboardCopy     += (s, e) => { /* custom post-copy logic */ };
gridControl.ClipboardCut      += (s, e) => { };
gridControl.ClipboardPaste    += (s, e) => { };
```

---

## TextDataExchange (Custom Delimiters and Buffer Access)

`GridModel.TextDataExchange` exposes low-level clipboard buffer operations.

### Change the tab delimiter (e.g., for CSV)

```csharp
gridControl.Model.TextDataExchange.TabDelimiter = ",";
```

### Copy to buffer programmatically

```csharp
string buffer;
int rows, cols;
gridControl.Model.TextDataExchange.CopyTextToBuffer(
    out buffer,
    gridControl.Model.SelectedRanges,
    out rows, out cols,
    isCut: false);   // true = cut, false = copy
// buffer now holds tab-delimited cell text
```

### Paste from buffer programmatically

```csharp
gridControl.Model.TextDataExchange.PasteTextFromBuffer(
    buffer,
    gridControl.Model.SelectedRanges);
```

---

## Custom Clipboard Handler via IGridCopyPaste

Implement `IGridCopyPaste` to provide completely custom clipboard formats (e.g., HTML, JSON):

```csharp
class HtmlCopyPaste : IGridCopyPaste
{
    public void Copy(GridCellData gridData, GridRangeInfoList rangeList)
    {
        var sb = new StringBuilder("<html><body><table>");
        // iterate gridData, build HTML table...
        sb.Append("</table></body></html>");

        var dataObj = new DataObject();
        dataObj.SetData(DataFormats.UnicodeText, sb.ToString());
        Clipboard.SetDataObject(dataObj);
    }

    public void Cut(GridCellData data, GridRangeInfoList rangeList) { }

    public DataObject Paste(GridRangeInfoList rangeList) => new DataObject();
}

// Attach to the grid
gridControl.Model.GridCopyPaste = new HtmlCopyPaste();
```

---

## Undo / Redo

GridControl tracks changes in a `GridModelCommandManager` (`CommandStack`) that supports full undo/redo, transactions, and rollback.

### Basic Undo/Redo

```csharp
// Enable or disable the undo buffer
gridControl.Model.CommandStack.Enabled = true;

// Undo the last action
gridControl.Model.CommandStack.Undo();

// Redo the last undone action
gridControl.Model.CommandStack.Redo();

// Clear only the undo stack
gridControl.Model.CommandStack.UndoStack.Clear();

// Clear only the redo stack
gridControl.Model.CommandStack.RedoStack.Clear();

// Clear both stacks at once
gridControl.Model.CommandStack.Clear();
```

### Transactions (grouping multiple actions into one undo step)

```csharp
// Begin a transaction — all changes until CommitTrans are grouped
gridControl.Model.CommandStack.BeginTrans("Bulk Update");

gridControl.Model[1, 1].CellValue = "A";
gridControl.Model[1, 2].CellValue = "B";
gridControl.Model[2, 1].CellValue = "C";

// Commit — the three changes undo as one action
gridControl.Model.CommandStack.CommitTrans();

// Or rollback — reverts all changes since BeginTrans
// gridControl.Model.CommandStack.Rollback();
```

Transactions can be nested; nested transactions are treated as part of their parent.

### CommandStack Key Properties

| Property | Description |
|---|---|
| `Enabled` | Turn undo/redo tracking on/off |
| `InTransaction` | True if `BeginTrans` has been called |
| `IsRecording` | True when in normal recording mode |
| `ShouldGenerateUndoInfo` | Whether commands should be pushed to the stack now |
| `UndoStack` | The underlying undo command stack |
| `RedoStack` | The underlying redo command stack |

---

## Custom (Derived) Commands

Extend undo/redo to cover non-cell operations by deriving from `GridModelCommand`:

```csharp
public class CellActivatedCommand : GridModelCommand
{
    private readonly RowColumnIndex _cell;

    public CellActivatedCommand(GridModel model, RowColumnIndex cell)
        : base(model) => _cell = cell;

    public override void Execute()
    {
        // Restore the active cell when undone
        this.Grid.ActiveGridView.CurrentCell.MoveTo(_cell);
    }
}

// Push custom command when current cell changes
gridControl.CurrentCellActivated += (s, args) =>
{
    if (gridControl.Model.CommandStack.ShouldGenerateUndoInfo)
    {
        gridControl.Model.CommandStack.Push(
            new CellActivatedCommand(
                gridControl.Model,
                gridControl.CurrentCell.CellRowColumnIndex));
    }
};
```

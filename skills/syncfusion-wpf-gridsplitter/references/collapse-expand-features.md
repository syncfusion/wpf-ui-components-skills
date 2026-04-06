
# Collapse and Expand Features

This file covers collapse/expand behavior and button templates. Basic layout placement is in `references/getting-started.md`.

## EnableCollapseButton

Enable collapse buttons to allow users to hide adjacent panels. Increase the splitter size when using buttons (8–12px).

```xml
<syncfusion:SfGridSplitter EnableCollapseButton="True" Height="8"/>
```

**Notes:**
- Set `MinHeight="0"` or `MinWidth="0"` on rows/columns to allow full collapse.
- Buttons appear as directional arrows and toggle to expand when collapsed.

## Custom Collapse Button Templates

Provide minimal button templates rather than embedding full layout examples.

### Up / Down Button Templates (horizontal)

```xml
<syncfusion:SfGridSplitter UpButtonTemplate="{StaticResource UpButtonTemplate}" DownButtonTemplate="{StaticResource DownButtonTemplate}"/>
```

### Left / Right Button Templates (vertical)

```xml
<syncfusion:SfGridSplitter LeftButtonTemplate="{StaticResource LeftButtonTemplate}" RightButtonTemplate="{StaticResource RightButtonTemplate}"/>
```

## Patterns and Recommendations

- For IDE-style layouts, use collapsible side panels with `MinWidth="0"` on collapsible columns.
- If only one side should collapse, provide a collapsed `RightButtonTemplate` or `LeftButtonTemplate` set to a hidden template.

## Troubleshooting

- Buttons not showing: increase splitter size and ensure `EnableCollapseButton="True"`.
- Panels not collapsing fully: set `MinHeight`/`MinWidth` to `0` on target definitions.


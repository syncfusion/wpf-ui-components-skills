````markdown
# Populating Items — Advanced

Advanced container customization, style selectors, virtualization, selection APIs, and flow-direction considerations.

## ItemContainerStyle & Template

Use `ItemContainerStyle` to style the generated `CarouselItem` container. Use `ControlTemplate` for complex visuals.

## ItemContainerStyleSelector

Create a `StyleSelector` to choose styles based on data properties.

## Virtualization

Enable `EnableVirtualization="True"` for large collections to improve memory and rendering performance.

## Selection APIs

Use `SelectedItem`, `SelectedIndex`, and selection commands. Handle `SelectionChanged` to react to selection updates.

## Flow Direction

Set `FlowDirection="RightToLeft"` for RTL languages; ensure templates and layout adapt appropriately.

````

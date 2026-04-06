````markdown
# Navigation — Patterns & Advanced

Contains common navigation patterns: arrow-button, thumbnail-sync, indicator dots, timer-based auto-navigation, gesture-based touch navigation, and looping behavior for CustomPath mode.

## Looping

Enable `EnableLooping="True"` in CustomPath mode to wrap navigation from last to first.

## Common Patterns

- **Arrow buttons**: bind to `SelectPreviousItemCommand`/`SelectNextItemCommand`.
- **Thumbnail navigation**: sync `SelectedIndex` between a `ListBox` and the Carousel.
- **Indicator dots**: use an `ItemsControl` and bind selection state to visuals.
- **Timer auto-navigation**: use `DispatcherTimer` to call `SelectNextItem()` periodically and pause/resume on mouse enter/leave.
- **Gesture (touch)**: measure touch delta and call `SelectPreviousItem()` / `SelectNextItem()` when threshold exceeded.

Example snippets and code samples were moved here from the original `navigation.md` to keep the basics file concise.

````

````markdown
# Navigation — Basics

Core navigation behaviors for the Carousel: keyboard, scroll bars, and mouse wheel. See `navigation-patterns.md` for patterns and gestures.

## Keyboard Navigation

Primary navigation for accessibility. Arrow keys, Home/End, PageUp/PageDown map to previous/next/first/last/page navigation. Ensure the control has focus (`Focusable="True"`).

## Scroll Bars

Enable using attached `ScrollViewer` properties when using `VisualMode="CustomPath"`.

## Mouse Wheel

Mouse wheel maps to next/previous item by default. Combine with focus management for reliable behavior.

## Commands and Methods

Use built-in commands (`SelectFirstItemCommand`, `SelectNextItemCommand`, etc.) or call methods (`SelectFirstItem()`, `SelectNextItem()`) from code-behind.

For XAML and code examples, see `navigation-patterns.md`.

````

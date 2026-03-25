---
layout: post
title: End user capabilities | ChromelessWindow
description: Describe what end users can do with ChromelessWindow (resize, drag, maximize, etc.)
platform: wpf
control: ChromelessWindow
documentation: ug
---
# End user capabilities

ChromelessWindow supports the usual window interactions when enabled:

- Dragging via the title bar (when `PART_TitleBar` present)
- Resizing via window edges (when ResizeMode allows)
- Minimize, Maximize, Restore, Close (configurable via properties)
- Keyboard accessibility for title buttons

Ensure templates preserve expected part names (`PART_TitleBar`, etc.) to retain these capabilities.

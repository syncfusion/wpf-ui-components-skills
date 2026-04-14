# WPF PropertyGrid Overview

The PropertyGrid control provides an interface for browsing and editing an object's properties with Blendability support, custom editors, category editors, sorting, and grouping.

## Control Structure

```
PropertyGrid
├── SearchBox (optional) — filter properties by name
├── Sort/Group Buttons — switch between sorted and grouped views
├── Property List
│   ├── Property Name Column
│   └── Value Editor Column (context-sensitive editors)
└── Description Panel (optional) — shows property description
```

## Features

| Feature | Description |
|---|---|
| Object Binding | Bind any object via `SelectedObject`; properties are auto-reflected |
| Custom Editor | Assign custom controls as value editors for specific properties |
| Category Editor | Group related properties into a single category UI |
| Grouping | Group properties by `[Category]` attribute or `Display.GroupName` |
| Sorting | Sort properties `Ascending`, `Descending`, or by declaration order (`null`) |
| Filtering | Hide properties via `HidePropertiesCollection`, attributes, or `AutoGeneratingPropertyGridItem` |
| SearchBox | Built-in search to locate properties by name |
| Nested Properties | Expand complex object properties (`PropertyExpandMode = NestedMode`) |
| Collection Editor | Built-in dialog to add/remove items from `IList`-derived collections |
| Theming | Apply themes via `SfSkinManager` |
| Blendability | Supports design-time customization in Blend and Visual Studio |

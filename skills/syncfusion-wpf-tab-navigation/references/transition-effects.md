# Transition Effects

TabNavigationControl animates item switches using the `TransitionEffect` property.  
Set it in XAML or C# to control how the old item exits and the new item enters.

---

## All Transition Effects

### Slide
The new item slides in horizontally from the side.

```xaml
<syncfusion:TabNavigationControl TransitionEffect="Slide"
                                  ItemsSource="{Binding MyCollection}"/>
```

---

### Fade
The previous item fades out and the new item appears with increasing opacity.

```xaml
<syncfusion:TabNavigationControl TransitionEffect="Fade"
                                  ItemsSource="{Binding MyCollection}"/>
```

---

### Zoom
The new item appears with a zooming (scale) effect.

```xaml
<syncfusion:TabNavigationControl TransitionEffect="Zoom"
                                  ItemsSource="{Binding MyCollection}"/>
```

---

### Blur
The new item appears with a blur-to-sharp transition.

```xaml
<syncfusion:TabNavigationControl TransitionEffect="Blur"
                                  ItemsSource="{Binding MyCollection}"/>
```

---

### Push
The new item descends from the top, pushing the old item down.

```xaml
<syncfusion:TabNavigationControl TransitionEffect="Push"
                                  ItemsSource="{Binding MyCollection}"/>
```

---

### PushIn
The new item ascends from the bottom, pushing content upward.

```xaml
<syncfusion:TabNavigationControl TransitionEffect="PushIn"
                                  ItemsSource="{Binding MyCollection}"/>
```

---

### Wipe
The old item is wiped away and the new item is revealed beneath it.

```xaml
<syncfusion:TabNavigationControl TransitionEffect="Wipe"
                                  ItemsSource="{Binding MyCollection}"/>
```

---

## Setting TransitionEffect in C#

```csharp
using Syncfusion.Windows.Tools.Controls;

tabNavigation.TransitionEffect = TransitionEffect.Slide;
// or
tabNavigation.TransitionEffect = TransitionEffect.Fade;
```

---

## Decision Guide

| Use Case | Recommended Effect |
|---|---|
| General-purpose tab switching | `Slide` |
| Soft/subtle content change | `Fade` |
| Highlight importance of new content | `Zoom` |
| Dreamy or atmospheric feel | `Blur` |
| Vertical content replacement (top → down) | `Push` |
| Vertical content replacement (bottom → up) | `PushIn` |
| Reveal / unveil effect | `Wipe` |
| Disable animation | *(no built-in "None" — use `Fade` with minimal opacity delta or remove effect)* |

---

## Gotchas

| Issue | Cause | Fix |
|---|---|---|
| No animation visible | `TransitionEffect` not set | Assign a `TransitionEffect` value explicitly |
| `TransitionEffect` not recognized | Namespace missing | Ensure `xmlns:syncfusion="http://schemas.syncfusion.com/wpf"` is declared |
| Choppy animation | Heavy content inside `TabNavigationItem` | Simplify content or use `Fade` for lighter rendering |

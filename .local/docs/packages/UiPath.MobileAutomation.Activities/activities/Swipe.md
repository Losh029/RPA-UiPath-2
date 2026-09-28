# Swipe

`UiPath.MobileAutomation.Activities.Swipe`

Swipe in a direction until a target element appears, or for a capped number of swipes.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `SwipeDirection` | Swipe Direction | InArgument | [`SwipeDirection`](#swipe-direction-enum) | Yes | `Down` | Direction in which to swipe. |
| `MaxNumberOfSwipes` | Max Number Of Swipes | InArgument | `int` | | `10` | Hard cap on swipe attempts. The activity stops as soon as the `Target` is found, or after this many swipes. Prevents infinite scrolling. |
| `Target` | Target | Property | [`Target`](../common/Target.md) | | | Element to look for after each swipe. If set, the activity swipes until this element resolves or `MaxNumberOfSwipes` is reached. If omitted, the activity performs `MaxNumberOfSwipes` swipes and stops. |
| `Container` | Container | Property | [`Target`](../common/Target.md) | | | Optional sub-region to swipe inside (e.g. a scrollable list inside a tabbed pane). If omitted, swipes the whole screen. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `Timeout` | Timeout | InArgument | `double` | | Maximum time (in milliseconds) to wait for the `Target` to appear. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the first swipe. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the final swipe. |

## Swipe Direction Enum

`UiPath.MobileAutomation.Enums.SwipeDirection`

| Value | Description |
|-------|-------------|
| `Up` | Swipe from bottom to top (scrolls content down). |
| `Down` | Swipe from top to bottom (scrolls content up). Default. |
| `Left` | Swipe from right to left. |
| `Right` | Swipe from left to right. |

Note: the type is `SwipeDirection`, not `SwipeDirectionEnum`.

## XAML Example

Swipe down a list looking for a specific item:

```xml
<uma:Swipe DisplayName="Swipe"
           SwipeDirection="Down"
           MaxNumberOfSwipes="15">
  <uma:Swipe.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.TextView' text='About phone' /&gt;" />
  </uma:Swipe.Target>
</uma:Swipe>
```

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:umm="clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation"`

## How to create a new Swipe

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.Swipe
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- This is the **target-aware** swipe — it scrolls until the `Target` is visible. For a fixed-distance directional swipe without a target, use `DirectionalSwipe`. For a coordinate-pair swipe, use `PositionalSwipe`.
- `MaxNumberOfSwipes` is required even when a `Target` is set — it bounds the cost of the search. Pick a value larger than the expected number of viewport-heights the target could be from the current scroll position.
- For nested scrollables (e.g. a list inside a tab inside a drawer), set `Container` to the scrollable element so the swipe is confined to that surface.

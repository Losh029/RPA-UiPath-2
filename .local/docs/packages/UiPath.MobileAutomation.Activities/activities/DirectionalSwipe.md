# Directional Swipe

`UiPath.MobileAutomation.Activities.DirectionalSwipe`

Swipe the entire screen in a given direction by a fixed distance, without resolving any target. Use when you want a quick scroll or page-flip and don't care about a specific landing element.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `SwipeDirection` | Swipe Direction | Property | [`SwipeDirection`](#swipe-direction-enum) | Yes | `Down` | Direction in which to swipe. |
| `SwipeSize` | Swipe Size | Property | `short` | | `65` | Swipe distance, expressed as a percentage of the screen (1–100). Default `65` covers about two-thirds of a screen. |
| `SwipePressTime` | Swipe Press Time | Property | `long` | | `1000` | Duration of the swipe in milliseconds. Lower values produce flicks; higher values produce slow drags. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the swipe. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the swipe. |

## Swipe Direction Enum

`UiPath.MobileAutomation.Enums.SwipeDirection`

| Value | Description |
|-------|-------------|
| `Up` | Swipe from bottom to top (scrolls content down). |
| `Down` | Swipe from top to bottom (scrolls content up). Default. |
| `Left` | Swipe from right to left. |
| `Right` | Swipe from left to right. |

## XAML Example

Swipe down two-thirds of the screen (default):

```xml
<uma:DirectionalSwipe DisplayName="Directional Swipe"
                      SwipeDirection="Down" />
```

Quick flick to the left:

```xml
<uma:DirectionalSwipe DisplayName="Directional Swipe"
                      SwipeDirection="Left"
                      SwipeSize="80"
                      SwipePressTime="300" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Directional Swipe

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.DirectionalSwipe
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- This is the **target-less** swipe — it just moves the viewport in a direction by `SwipeSize` percent of the screen.
- For "swipe until an element appears", use `Swipe` instead (which takes a `Target` and caps attempts with `MaxNumberOfSwipes`).
- For coordinate-pair swipes between known points, use `PositionalSwipe`.
- The activity is platform-aware: on Android it uses physical pixels internally, on iOS it uses device-independent pixels. The percentage-based `SwipeSize` keeps the visible behaviour consistent across platforms.
- A `SwipeSize` of `100` swipes the entire screen height (or width, for `Left`/`Right`). Smaller values produce shorter scrolls. `SwipeSize` must be between `1` and `100` inclusive — values outside that range cause a design-time validation error (not clamped).

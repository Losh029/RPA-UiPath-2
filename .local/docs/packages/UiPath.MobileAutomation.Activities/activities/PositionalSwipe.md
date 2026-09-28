# Positional Swipe

`UiPath.MobileAutomation.Activities.PositionalSwipe`

Swipe between two screen coordinates. Useful for gestures where no element resolves at either endpoint (custom drag handles, off-screen entry points).

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `StartPointX` | Start Point X | InArgument | `double` | Yes | | X coordinate of the swipe start. |
| `StartPointY` | Start Point Y | InArgument | `double` | Yes | | Y coordinate of the swipe start. |
| `EndPointX` | End Point X | InArgument | `double` | Yes | | X coordinate of the swipe end. |
| `EndPointY` | End Point Y | InArgument | `double` | Yes | | Y coordinate of the swipe end. |
| `SwipePressTime` | Swipe Press Time | InArgument | `long` | | `4000` | Duration of the swipe in milliseconds. Longer values produce slower, more deliberate swipes; shorter values produce flicks. |
| `CoordinatesType` | Coordinates Type | Property | [`CoordinatesTypeEnum`](#coordinates-type-enum) | | `DeviceIndependentPixels` | Whether coordinates are device-independent (DIP) or physical pixels. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the swipe. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the swipe. |

## Coordinates Type Enum

`UiPath.MobileAutomation.Enums.CoordinatesTypeEnum`

| Value | Description |
|-------|-------------|
| `DeviceIndependentPixels` | Coordinates in DIP — the same logical position across devices regardless of screen density. Default and recommended. |
| `PhysicalPixels` | Coordinates in real device pixels. |

## XAML Example

Swipe up from the bottom of the screen (e.g. open the app drawer):

```xml
<uma:PositionalSwipe DisplayName="Positional Swipe"
                     StartPointX="200"
                     StartPointY="800"
                     EndPointX="200"
                     EndPointY="100"
                     SwipePressTime="2000"
                     CoordinatesType="DeviceIndependentPixels" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Positional Swipe

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.PositionalSwipe
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- Property names use `StartPointX/Y` and `EndPointX/Y` (with `Point` in the middle), and types are `double` — not `StartX/EndX` `int` as some earlier docs suggested.
- For target-aware scrolling (swipe until an element is visible), use `Swipe`. For direction-only swipes with no specific endpoints, use `DirectionalSwipe`. For multi-finger gestures, use `DrawPattern`.
- Default `SwipePressTime` of `4000ms` is deliberately slow to feel like a real user drag. For a flick, lower it to `300–500ms`.
- Coordinates default to **device-independent pixels**. If you read raw pixel values from a screenshot tool, set `CoordinatesType = PhysicalPixels`.

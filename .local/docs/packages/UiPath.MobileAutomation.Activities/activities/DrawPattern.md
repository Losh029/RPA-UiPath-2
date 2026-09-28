# Draw Pattern

`UiPath.MobileAutomation.Activities.DrawPattern`

Trace one or more multi-touch finger paths on the device screen. Each finger path is a sequence of `(x, y)` points, drawn either as a short press or a long press.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `MultiplePointsOperations` | Finger paths | Property | `List<MultiplePointsOperation>` | Yes | one empty finger path | The set of finger paths to draw simultaneously. One entry per finger; each entry contains a list of `PointOperation` (with `PointX` and `PointY`). Authored via the Studio designer; not typically hand-edited. |
| `CoordinatesType` | Coordinates Type | Property | [`CoordinatesTypeEnum`](#coordinates-type-enum) | Yes | `DeviceIndependentPixels` | Whether the point coordinates are in DIP or physical pixels. |
| `PressType` | Press Type | InArgument | [`PressTypeEnum`](#press-type-enum) | Yes | `ShortPress` | Whether each finger uses a short or long press while tracing. |
| `WaitTime` | Wait Time | InArgument | `int` | Yes | `200` | Wait between consecutive points on a finger path, in milliseconds. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the gesture starts. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the gesture ends. |

## Coordinates Type Enum

`UiPath.MobileAutomation.Enums.CoordinatesTypeEnum`

| Value | Description |
|-------|-------------|
| `DeviceIndependentPixels` | Points are in DIP — the same logical position across devices regardless of screen density. Default. |
| `PhysicalPixels` | Points are in real device pixels. |

## Press Type Enum

`UiPath.MobileAutomation.Enums.PressTypeEnum`

| Value | Description |
|-------|-------------|
| `ShortPress` | Each touch is a brief press. Default. Suitable for swipe-like gestures and unlock patterns. |
| `LongPress` | Each touch is held longer. Useful for drag-and-drop gestures. |

## XAML Example

Two-finger horizontal swipe (e.g. two-finger drag):

```xml
<uma:DrawPattern DisplayName="Draw Pattern"
                 CoordinatesType="DeviceIndependentPixels"
                 PressType="ShortPress"
                 WaitTime="50">
  <uma:DrawPattern.MultiplePointsOperations>
    <uma:MultiplePointsOperation>
      <uma:MultiplePointsOperation.PointOperations>
        <uma:PointOperation PointX="100" PointY="500" />
        <uma:PointOperation PointX="300" PointY="500" />
      </uma:MultiplePointsOperation.PointOperations>
    </uma:MultiplePointsOperation>
    <uma:MultiplePointsOperation>
      <uma:MultiplePointsOperation.PointOperations>
        <uma:PointOperation PointX="200" PointY="700" />
        <uma:PointOperation PointX="400" PointY="700" />
      </uma:MultiplePointsOperation.PointOperations>
    </uma:MultiplePointsOperation>
  </uma:DrawPattern.MultiplePointsOperations>
</uma:DrawPattern>
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"` — the `PointOperation` and `MultiplePointsOperation` classes are defined alongside `DrawPattern` in this namespace.

## How to create a new Draw Pattern

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.DrawPattern
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- Each `MultiplePointsOperation` represents one finger. Multiple entries means multi-touch (e.g. two-finger pinch).
- This activity is typically authored via the Studio designer's gesture-recording dialog rather than hand-edited. The XAML shape above is for reference.
- For simple coordinate-pair swipes (one finger, start → end), use `PositionalSwipe` — it's much simpler.
- For pattern-lock unlock screens, use one `MultiplePointsOperation` with the sequence of dot coordinates as `PointOperation` entries, `PressType = ShortPress`.

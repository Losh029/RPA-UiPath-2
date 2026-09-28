# Tap

`UiPath.MobileAutomation.Activities.Tap`

Tap a specified UI element.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Target` | Target | Property | [`Target`](../common/Target.md) | Yes | | The UI element to tap. Resolved at runtime via `FullSelector` / `FuzzySelector` / `CvText`. May include a `TapOffset` to nudge the tap point. |
| `TapType` | Tap Type | InArgument | [`TapTypeEnum`](#tap-type-enum) | | `Single` | Type of tap to perform. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the activities in the automation if this activity fails. |
| `Timeout` | Timeout | InArgument | `double` | | Maximum time (in milliseconds) to wait for the target element. Falls back to the `MobileDeviceConnection` scope's default if not set. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the tap is executed. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the tap is executed. |

## Tap Type Enum

`UiPath.MobileAutomation.Enums.TapTypeEnum`

| Value | Description |
|-------|-------------|
| `Single` | A single brief tap. Default. |
| `Double` | Two taps in quick succession (double-tap gesture). |
| `LongPress` | A press-and-hold gesture (long-press). |

There is no "N taps" mode; pick one of the three values above. To repeat a tap multiple times, place the `Tap` activity inside a loop.

## XAML Example

```xml
<uma:Tap DisplayName="Tap"
         TapType="Double">
  <uma:Tap.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.Button' id='com.example.basicapp:id/button3' /&gt;" />
  </uma:Tap.Target>
</uma:Tap>
```

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:umm="clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation"`

## How to create a new Tap

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.Tap
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- `TapType` is the only way to express tap variants. Do **not** look for a `TapCount` / `Repeat` property — none exists.
- For coordinate-based taps where no selector resolves, use `PositionalSwipe` (with `StartPoint == EndPoint`) or `DrawPattern` instead. `Tap` always requires a `Target`.
- The `Target.TapOffset` (optional) fine-tunes the tap point inside the resolved element. Useful when a parent element resolves but the actual hit-zone is offset (e.g. an icon inside a list row).
- Companion activities: `SetText`, `GetText`, `ElementExists`, `GetAttribute`, `Swipe`.

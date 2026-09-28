# Take Screenshot Part

`UiPath.MobileAutomation.Activities.TakeScreenshotPart`

Capture a rectangular sub-region of the device screen, defined by two coordinate points, and return it as an in-memory `Image`.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Device
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `StartPointX` | Start Point X | InArgument | `double` | Yes | | X coordinate of the top-left corner of the region. |
| `StartPointY` | Start Point Y | InArgument | `double` | Yes | | Y coordinate of the top-left corner. |
| `EndPointX` | End Point X | InArgument | `double` | Yes | | X coordinate of the bottom-right corner. |
| `EndPointY` | End Point Y | InArgument | `double` | Yes | | Y coordinate of the bottom-right corner. |
| `CoordinatesType` | Coordinates Type | Property | [`CoordinatesTypeEnum`](#coordinates-type-enum) | | `DeviceIndependentPixels` | Whether coordinates are device-independent (DIP) or physical pixels. |

Although `TakeScreenshotPart` inherits from `TargetBaseActivity`, its `Target` property is overridden as `[Browsable(false)]` and get-only — you cannot set it in raw XAML or the Studio designer, and the implementation does not use it. Coordinates are always interpreted relative to the screen origin `(0, 0)`.

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Screenshot` | Screenshot | `System.Drawing.Image` | The cropped region as an in-memory `Image`. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the capture. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the capture. |

`Timeout` is inherited from `TargetBaseActivity` but is not used by this activity (no target resolution happens). Leave it unset.

## Coordinates Type Enum

`UiPath.MobileAutomation.Enums.CoordinatesTypeEnum`

| Value | Description |
|-------|-------------|
| `DeviceIndependentPixels` | Coordinates in DIP — the same logical position across devices regardless of screen density. Default and recommended. |
| `PhysicalPixels` | Coordinates in real device pixels. Use only when you've sourced the values from the device's actual screen resolution. |

## XAML Example

Capture a 200x300 region from the top-left of the screen:

```xml
<uma:TakeScreenshotPart DisplayName="Take Screenshot Part"
                        StartPointX="0"
                        StartPointY="0"
                        EndPointX="200"
                        EndPointY="300"
                        CoordinatesType="DeviceIndependentPixels"
                        Screenshot="[regionImage]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Take Screenshot Part

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.TakeScreenshotPart
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- **There is no `ScreenshotFilePath` property.** The activity returns an in-memory `Image`; to save it, use a follow-up `Save Image` activity from `UiPath.UIAutomation.Activities` or call `Image.Save(path)` in a coded workflow.
- Coordinates default to **device-independent pixels**. If you copy raw pixel coordinates from a screenshot tool, set `CoordinatesType = PhysicalPixels` to avoid scaling mismatches on high-DPI devices.
- Coordinates are always **screen-relative** — `(0, 0)` is the top-left of the device screen. There is no element-relative mode; the inherited `Target` is hidden and ignored.
- For full-screen capture, use `TakeScreenshot` (which has no inputs).
- For element-cropped screenshots, capture the full screen with `TakeScreenshot`, then use `GetAttribute` with `AttributeName = "bounds"` (Android) to read the element's bounding box and crop the image in code.

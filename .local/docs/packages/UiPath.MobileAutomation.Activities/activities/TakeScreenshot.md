# Take Screenshot

`UiPath.MobileAutomation.Activities.TakeScreenshot`

Capture a full-screen screenshot from the connected mobile device and return it as an in-memory `Image` (plus width and height).

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Device
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

This activity has **no input properties**. It always captures the full current device screen.

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Screenshot` | Screenshot | `System.Drawing.Image` | The captured screenshot as an in-memory `Image` object. |
| `Width` | Width | `int` | Width of the screenshot in pixels. |
| `Height` | Height | `int` | Height of the screenshot in pixels. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |

## XAML Example

Capture and bind the outputs:

```xml
<uma:TakeScreenshot DisplayName="Take Screenshot"
                    Screenshot="[screenshotImage]"
                    Width="[screenshotWidth]"
                    Height="[screenshotHeight]" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Take Screenshot

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.TakeScreenshot
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- **There is no `ScreenshotFilePath` property.** The activity returns an in-memory `Image`; it does not write to disk on its own. To save the screenshot to a file, use a follow-up `Save Image` activity from `UiPath.UIAutomation.Activities` or invoke `Image.Save(path)` directly in a coded workflow.
- For region-of-interest screenshots (a rectangle within the screen), use `TakeScreenshotPart`.
- For workflows that need the screenshot dimensions only (e.g. logging that the device is at expected resolution), bind `Width` and `Height` without saving the `Image`.
- Companion: the `MobileDeviceConnection` scope's `CollectScreenshot` setting can automatically capture screenshots after every activity for debugging purposes.

# UiPath Mobile Automation Activities

Automate Android and iOS mobile applications via Appium. All activities run inside a `MobileDeviceConnection` scope which manages the Appium session lifecycle.

**Package ID:** `UiPath.MobileAutomation.Activities`

## Activity Categories

### Mobile Device

Connection scope and device-level state.

- **[MobileDeviceConnection](activities/MobileDeviceConnection.md)** — Establishes the Appium session. Container scope for every other activity.
- **[GetSessionIdentifier](activities/GetSessionIdentifier.md)** — Retrieves the active Appium session ID.
- **[ExecuteCommand](activities/ExecuteCommand.md)** — Executes a raw Appium command. Use as escape hatch when no first-class activity covers what you need.
- **[GetDeviceOrientation](activities/GetDeviceOrientation.md)** — Reads current orientation (Portrait / Landscape).
- **[SetDeviceOrientation](activities/SetDeviceOrientation.md)** — Rotates to Portrait / Landscape / Toggle and returns the resulting orientation.
- **[SetDeviceGeoLocation](activities/SetDeviceGeoLocation.md)** — Mocks the device GPS location.
- **[GetSystemTime](activities/GetSystemTime.md)** — Reads the device's current system time.
- **[TakeScreenshot](activities/TakeScreenshot.md)** — Captures a full-screen screenshot as an in-memory `Image`.
- **[TakeScreenshotPart](activities/TakeScreenshotPart.md)** — Captures a rectangular sub-region as an in-memory `Image`.

### Mobile Application

App lifecycle and entry points.

- **[InstallApp](activities/InstallApp.md)** — Installs an APK / IPA from a local path or URL.
- **[ManageCurrentApp](activities/ManageCurrentApp.md)** — Launches or resets the app configured on the connection. Commands: `Launch`, `Reset`.
- **[ManageOtherApp](activities/ManageOtherApp.md)** — Checks / activates / terminates / uninstalls an app by package name (Android) or bundle id (iOS). Commands: `IsInstalled`, `Remove`, `Activate`, `Terminate`.
- **[OpenDeepLink](activities/OpenDeepLink.md)** — Opens an in-app deep link.
- **[OpenUrl](activities/OpenUrl.md)** — Opens a URL in the active mobile browser.

### UI Automation

Element-based interaction.

- **[Tap](activities/Tap.md)** — Tap (Single / Double / LongPress) on a UI element.
- **[SetText](activities/SetText.md)** — Type text into an element, with optional `SecureString` and pre-tap.
- **[TypeText](activities/TypeText.md)** — Type text into the currently focused element (no target).
- **[GetText](activities/GetText.md)** — Read text content from an element.
- **[GetAttribute](activities/GetAttribute.md)** — Read a named attribute (`checked`, `enabled`, `text`, etc.).
- **[ElementExists](activities/ElementExists.md)** — Check whether an element exists without throwing.
- **[GetSelectedItem](activities/GetSelectedItem.md)** — Read the selected item text of a Spinner / picker.
- **[SetSelectedItem](activities/SetSelectedItem.md)** — Set the selected item(s) on a Spinner / picker.
- **[Swipe](activities/Swipe.md)** — Swipe in a direction until a target appears, capped by `MaxNumberOfSwipes`.
- **[DirectionalSwipe](activities/DirectionalSwipe.md)** — Fixed-distance directional swipe without a target.
- **[PositionalSwipe](activities/PositionalSwipe.md)** — Swipe between two screen coordinates.
- **[DrawPattern](activities/DrawPattern.md)** — Multi-finger gesture from a list of point sequences.

### Hardware Interaction

- **[PressHardwareButton](activities/PressHardwareButton.md)** — Press `HomeButton` (iOS + Android) / `BackButton` / `SwitchAppButton` (Android only).

### Logging & Debugging

- **[GetPageSource](activities/GetPageSource.md)** — Dump the current UI hierarchy to XML.
- **[GetLogs](activities/GetLogs.md)** — Capture device logs (e.g. `logcat` on Android) to a file.
- **[GetLogTypes](activities/GetLogTypes.md)** — List available log channels for the current session.

## Prerequisites

- **Appium Server** — Appium 1.x or 2.x running and reachable from the workflow host.
- **Mobile Device or Emulator** — Physical device (USB / network) or emulator/simulator.
- **Platform-specific driver**:
  - Android → UIAutomator2 (default)
  - iOS → XCUITest (requires macOS + Xcode)

## Common Workflow Pattern

1. Wrap everything in a `MobileDeviceConnection` scope.
2. Set capabilities via the scope's `Arguments` dictionary (`platformName`, `deviceName`, `app` / `appPackage`+`appActivity` / `bundleId`).
3. Perform UI / device / app activities inside the scope.
4. Let the scope handle session cleanup (default `Close = Always`).

## Example Workflow Structure

```xml
<uma:MobileDeviceConnection DisplayName="Mobile Device Connection"
                            AppiumUrl="http://localhost:4723"
                            OutputConnection="[mobileDevice]"
                            Close="Always">
  <uma:MobileDeviceConnection.Arguments>
    <scg:Dictionary x:TypeArguments="x:String, Argument">
      <InArgument x:TypeArguments="x:String" x:Key="platformName">Android</InArgument>
      <InArgument x:TypeArguments="x:String" x:Key="deviceName">emulator-5554</InArgument>
      <InArgument x:TypeArguments="x:String" x:Key="appPackage">com.example.basicapp</InArgument>
      <InArgument x:TypeArguments="x:String" x:Key="appActivity">.core.MainActivity</InArgument>
    </scg:Dictionary>
  </uma:MobileDeviceConnection.Arguments>

  <uma:MobileDeviceConnection.Body>
    <ActivityAction x:TypeArguments="x:Object">
      <Sequence>
        <uma:Tap DisplayName="Tap Sign In" TapType="Single">
          <uma:Tap.Target>
            <umm:Target FullSelector="&lt;mbl android:className='android.widget.Button' id='com.example.basicapp:id/button1' /&gt;" />
          </uma:Tap.Target>
        </uma:Tap>

        <uma:GetText DisplayName="Read result" Text="[result]">
          <uma:GetText.Target>
            <umm:Target FullSelector="&lt;mbl android:className='android.widget.TextView' id='com.example.basicapp:id/textView' /&gt;" />
          </uma:GetText.Target>
        </uma:GetText>
      </Sequence>
    </ActivityAction>
  </uma:MobileDeviceConnection.Body>
</uma:MobileDeviceConnection>
```

## XAML Namespaces

Studio emits four namespace prefixes for the Mobile Automation package:

| Prefix | CLR namespace / assembly | What it covers |
| --- | --- | --- |
| `uma:` | `UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities` | Activity classes (`Tap`, `SetText`, `MobileDeviceConnection`, etc.) |
| `umm:` | `UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation` | Model types — `Target`, `TapOffset`, `LocationModel`, etc. |
| `ume:` | `UiPath.MobileAutomation.Enums;assembly=UiPath.MobileAutomation` | Cross-package enums — `TapTypeEnum`, `SwipeDirection`, `HardwareButton`, `CoordinatesTypeEnum`, `PressTypeEnum`, `ScreenOrientation` |
| `umae:` | `UiPath.MobileAutomation.Activities.Enums;assembly=UiPath.MobileAutomation.Activities` | Activity-package enums — `DeviceOrientationEnum`, `SetTextTapBefore`, `MobileConnectionCloseMode`, `Collect` |

You only need to declare the prefixes you actually use in a given XAML file. Most workflows need `uma:` + `umm:`; add `ume:` / `umae:` only if you explicitly reference enum types in expressions.

## Platform Support

Most activities work on both Android and iOS. Platform-specific behaviour:

- **`PressHardwareButton`**: `HomeButton` works on both. `BackButton` and `SwitchAppButton` are Android-only.
- **Selectors**: Android (UIAutomator2) and iOS (XCUITest) expose different attribute names and class hierarchies. A selector authored for one platform usually does not transfer to the other unaltered.
- **`DrawPattern`**: works on both, but Android pattern-lock screens are the canonical use case.

## Notes

- **Always inside `MobileDeviceConnection`** — every Mobile Automation activity except `MobileDeviceConnection` itself fails at runtime if placed outside the scope.
- **Use `SecureText` for credentials** — `SetText` and `TypeText` both expose a `SecureString` input (`SecureText`); use it instead of plaintext `Text` for any password / token entry.
- **`Target` is a model, not a string** — every target-based activity takes a `Target` property (a complex `umm:Target` model with `FullSelector` / `FuzzySelector` / `CvText` / `TapOffset` etc.), not a bare string `Selector`.
- **Computer Vision targeting** is available via `Target.CvText`. The CV server / API key are read from project settings (`UiPath.Sdk.Activities.CvScope.*`).

## Additional Resources

- [Appium documentation](http://appium.io/docs)
- [Appium desired capabilities](http://appium.io/docs/en/writing-running-appium/caps)
- [UiPath Mobile Automation How-To Guides](https://docs.uipath.com/activities/other/latest/ui-automation/mobile-automation-how-to-guides)

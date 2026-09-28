# Mobile Device Connection

`UiPath.MobileAutomation.Activities.MobileDeviceConnection`

Establish and maintain an Appium session to a mobile device. All Mobile Automation activities must be placed inside this scope.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Device

This is the **scope container** for the whole Mobile Automation package. It is the one activity that does not require itself.

## Properties

### Connection

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `AppiumUrl` | Appium Url | InArgument | `string` | Yes (unless reusing `InputConnection`) | | URL of the Appium server (e.g. `http://localhost:4723` or `http://localhost:4723/wd/hub`). |
| `Arguments` | Capabilities | Property | `Dictionary<string, Argument>` | Yes (unless reusing `InputConnection`) | empty | Appium [desired capabilities](http://appium.io/docs/en/writing-running-appium/caps) for the session. Common keys: `platformName`, `deviceName`, `platformVersion`, `app`, `appPackage`, `appActivity` (Android), `bundleId` (iOS), `udid`. |
| `HttpHeaders` | Http Headers | Property | `Dictionary<string, Argument>` | | empty | Additional HTTP headers sent on every request to the Appium server (e.g. for cloud farms that require auth headers). |
| `StartPage` | Start Page | InArgument | `string` | | | For web-context sessions, the URL to open after the session starts. |

### Connection Reuse

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `InputConnection` | Input Connection | InArgument | `MobileDevice` | | | Reuse an existing `MobileDevice` connection (returned from a previous scope's `OutputConnection`). When set, `AppiumUrl` and `Arguments` are ignored. |
| `OutputConnection` | Output Connection | OutArgument | `MobileDevice` | | | The active connection handle. Pass to a downstream `MobileDeviceConnection` scope's `InputConnection` to share the same Appium session. |
| `Close` | Close | Property | [`MobileConnectionCloseMode?`](#mobile-connection-close-mode-enum) | | `null` (treated as `Always`) | Whether to close the Appium session when the scope exits. |
| `UseExistingOpenConnection` | Use Existing Open Connection | InArgument | `bool` | | `true` | When `true`, the scope reuses an already-open session for the same `AppiumUrl` if one exists in the process. |

### Logging & Diagnostics

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `LoggingEnabled` | Logging Enabled | InArgument | `bool` | | `false` | Enables Appium command-level logging. |
| `LoggingFolderPath` | Logging Folder Path | InArgument | `string` | | | Folder for log output when `LoggingEnabled = true`. |
| `CollectScreenshot` | Collect Screenshot | InArgument | [`Collect`](#collect-enum) | | `None` | Automatically capture a screenshot before / after every activity in the scope (saved under `LoggingFolderPath`). |
| `CollectPageSource` | Collect Page Source | InArgument | [`Collect`](#collect-enum) | | `None` | Automatically capture the page source before / after every activity in the scope. |
| `VideoRecordingEnabled` | Video Recording Enabled | InArgument | `bool` | | `false` | Record the device screen as a video for the scope's duration. |
| `VideoRecordingTimeLimit` | Video Recording Time Limit | InArgument | `int` | | `3600` (seconds) | Maximum video length in seconds. |
| `VideoRecordingQuality` | Video Recording Quality | InArgument | `VideoQuality` | | | Quality preset for the recorded video. |
| `MirroringEnabled` | Mirroring Enabled | InArgument | `bool` | | (from project setting) | When the Mobile Device Manager is open, mirror live device state during execution. |

### Development-only

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `SingleInstanceForDevelopment` | Single Instance For Development | InArgument | `bool` | | `true` | Keep one Appium session alive across debug-run iterations. **Do not enable in published workflows.** |
| `WaitForPageUpdateDevelopment` | Wait For Page Update (Development) | InArgument | `bool` | | `true` | Wait after each activity for the page to settle. Useful when stepping through in the debugger. |

### Computer Vision (read from project settings)

| Property | Project Setting Key | Description |
|----------|-----------|-------------|
| `CvServer` | `UiPath.Sdk.Activities.CvScope.Server` | Computer Vision server URL (used when a `Target` uses `CvText`). |
| `CvApiKey` | `UiPath.Sdk.Activities.CvScope.ApiKey` | Computer Vision API key. |
| `CvUseLocalServer` | `UiPath.Sdk.Activities.CvScope.UseLocalServer` | Use a locally-hosted CV server. |

When a property is not explicitly set on the activity, the value is read from the project's `project.json` settings.

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if connection fails. |

## Mobile Connection Close Mode Enum

`UiPath.MobileAutomation.Activities.Enums.MobileConnectionCloseMode`

| Value | Description |
|-------|-------------|
| `Always` | Close the Appium session when the scope exits. Default behaviour when `Close` is unset. |
| `Never` | Keep the session open after the scope exits. Pair with `OutputConnection` to hand the session to a downstream scope. |

Only these two values exist on the enum. There is no `IfNotInputConnection` value (some earlier docs claimed it).

## Collect Enum

`UiPath.MobileAutomation.Activities.Enums.Collect`

| Value | Description |
|-------|-------------|
| `None` | Do not capture. Default. |
| `AfterActivity` | Capture after every nested activity completes. |

## XAML Example

```xml
<uma:MobileDeviceConnection DisplayName="Mobile Device Connection"
                            AppiumUrl="http://localhost:4723"
                            Close="Always"
                            OutputConnection="[mobileDevice]">
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
        <!-- Mobile automation activities go here -->
      </Sequence>
    </ActivityAction>
  </uma:MobileDeviceConnection.Body>
</uma:MobileDeviceConnection>
```

The `<scg:Dictionary>` wrapper type is `Dictionary<x:String, Argument>` (with the WF base class `Argument`, not `InArgument`). The `Arguments` property is typed `Dictionary<string, Argument>`, so a `Dictionary<string, InArgument>` value would fail to assign because generics are invariant. The `<InArgument>` entries inside are fine since `InArgument<T>` derives from `Argument`. An empty dictionary is written as `<scg:Dictionary x:TypeArguments="x:String, Argument" />`.

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:scg="clr-namespace:System.Collections.Generic;assembly=System.Private.CoreLib"` (or `assembly=mscorlib` on .NET Framework projects)
* `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"`

## How to create a new Mobile Device Connection

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.MobileDeviceConnection
```

## Notes

- This is the **scope container** for the whole Mobile Automation package. Every other activity must be a descendant of a `MobileDeviceConnection`.
- The `Arguments` dictionary keys map to Appium [desired capabilities](http://appium.io/docs/en/writing-running-appium/caps). Common keys:
  - **Required for any session**: `platformName` (`Android` or `iOS`), one of `deviceName` / `udid` / `avd`.
  - **Android app launch**: `app` (path/URL to APK), or `appPackage` + `appActivity` (for an already-installed app).
  - **iOS app launch**: `app` (path/URL to IPA), or `bundleId`.
  - **Cloud farms** (Sauce Labs, BrowserStack): vendor-specific keys like `sauce:options` / `bstack:options` — pass as nested dictionaries.
- `Close = Always` (the default when `Close` is unset) tears down the Appium session at scope exit. Use `Never` plus `OutputConnection` to hand the live session to a downstream scope.
- `UseExistingOpenConnection = true` (default) means the activity reuses an open session in the same process. Set to `false` for a guaranteed fresh session per run.
- The `CollectScreenshot` / `CollectPageSource` settings produce one artefact per nested activity into `LoggingFolderPath`. Useful for post-run debugging.
- `SingleInstanceForDevelopment` and `WaitForPageUpdateDevelopment` are development-time aids — keep them at default and consider explicitly setting them to `false` in workflows intended for unattended production runs.
- Sister activity `GetSessionIdentifier` retrieves the Appium session ID from inside the scope, useful when you need to call low-level Appium endpoints or share the session externally.

# Execute Command

`UiPath.MobileAutomation.Activities.ExecuteCommand`

Execute a raw Appium mobile command. Escape hatch when no first-class Mobile Automation activity covers what you need.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** Mobile Device
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Command` | Command | InArgument | `string` | Yes | | The Appium command name (e.g. `"mobile: shell"`, `"mobile: viewportScreenshot"`, `"mobile: pressKey"`). The set of supported commands depends on the Appium driver version. |
| `Parameters` | Parameters | InArgument | `Dictionary<string, object>` | | | Arguments to the command, keyed by argument name as defined by the driver. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Output` | Output | `object` | Result returned by the driver. Cast to the expected type in your workflow (string, dictionary, list, etc.). |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the command runs. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the command runs. |

## XAML Example

Run an `adb shell` command via Appium (Android, UIAutomator2 driver):

```xml
<uma:ExecuteCommand DisplayName="Execute Command"
                    Command="mobile: shell"
                    Output="[commandOutput]">
  <uma:ExecuteCommand.Parameters>
    <InArgument x:TypeArguments="scg:Dictionary(x:String, x:Object)">[New Dictionary(Of String, Object) From {{"command", "echo"}, {"args", New String() {"Hello"}}}]</InArgument>
  </uma:ExecuteCommand.Parameters>
</uma:ExecuteCommand>
```

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:scg="clr-namespace:System.Collections.Generic;assembly=System.Private.CoreLib"` (or `assembly=mscorlib` on .NET Framework projects) — for the `scg:Dictionary` type argument on `Parameters`
* `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"` — for `x:TypeArguments`, `x:String`, `x:Object`

## How to create a new Execute Command

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.ExecuteCommand
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- **Prefer a first-class activity when one exists.** Use `Tap` instead of `ExecuteCommand "mobile: clickGesture"`; use `TakeScreenshot` instead of `ExecuteCommand "mobile: viewportScreenshot"`. `ExecuteCommand` is for commands the package doesn't wrap (e.g. `mobile: shell`, `mobile: pressKey` for volume buttons, etc.).
- Command names and parameter shapes differ between drivers:
  - Android UIAutomator2: <https://github.com/appium/appium-uiautomator2-driver#mobile-command-reference>
  - iOS XCUITest: <https://appium.github.io/appium-xcuitest-driver/latest/reference/execute-methods/>
- Some commands fail silently on the wrong platform. Wrap in a `Try Catch` or set `ContinueOnError = true` if cross-platform.
- The `Output` is typed as `object` — for typed access in expressions, cast (`DirectCast(Output, String)`, `DirectCast(Output, Dictionary(Of String, Object))`, etc.).

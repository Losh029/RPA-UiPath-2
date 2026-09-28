# Element Exists

`UiPath.MobileAutomation.Activities.ElementExists`

Check whether a UI element exists on the current screen without throwing if it does not.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Target` | Target | Property | [`Target`](../common/Target.md) | Yes | | The UI element to look for. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Exists` | Exists | `bool` | `True` if the element resolves within `Timeout`, `False` otherwise. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `Timeout` | Timeout | InArgument | `double` | | Maximum time (in milliseconds) to wait before deciding the element is not present. Falls back to the `MobileDeviceConnection` scope default if not set. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the check. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the check. |

Note: there is no separate `WaitTimeout` property — the inherited `Timeout` (from `BaseActivityWithDelays`) controls how long to wait.

## XAML Example

```xml
<uma:ElementExists DisplayName="Element Exists"
                   Timeout="10000"
                   Exists="[loginSucceeded]">
  <uma:ElementExists.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.TextView' id='com.example.basicapp:id/textView' /&gt;" />
  </uma:ElementExists.Target>
</uma:ElementExists>
```

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:umm="clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation"`

## How to create a new Element Exists

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.ElementExists
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- Unlike `GetText` / `Tap` / `GetAttribute`, this activity never throws when the element is missing — it just returns `Exists = False`. Use it for conditional flows.
- For the assertion-style "fail if missing" semantics, use a regular target-based activity (`Tap`, `GetText`) — those throw on timeout.

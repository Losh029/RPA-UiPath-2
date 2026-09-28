# Get Text

`UiPath.MobileAutomation.Activities.GetText`

Retrieve the visible text content of a mobile UI element.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Target` | Target | Property | [`Target`](../common/Target.md) | Yes | | The UI element to read text from. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Text` | Text | `string` | The text content of the target element. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `Timeout` | Timeout | InArgument | `double` | | Maximum time (in milliseconds) to wait for the target element. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before reading the text. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after reading the text. |

## XAML Example

```xml
<uma:GetText DisplayName="Get Text"
             Text="[elementText]">
  <uma:GetText.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.TextView' id='com.example.basicapp:id/textView' /&gt;" />
  </uma:GetText.Target>
</uma:GetText>
```

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:umm="clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation"`

## How to create a new Get Text

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.GetText
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- For text inputs (`EditText` on Android, `TextField` on iOS), `Text` returns the current value of the field.
- For labels and read-only text views, `Text` returns the displayed string.
- To read a non-text attribute of an element (e.g. `checked` on a checkbox, `enabled` on a button), use `GetAttribute` instead — its `Attribute` output handles arbitrary attribute names.

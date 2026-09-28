# Get Attribute

`UiPath.MobileAutomation.Activities.GetAttribute`

Retrieve the value of a named attribute from a mobile UI element.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Target` | Target | Property | [`Target`](../common/Target.md) | Yes | | The UI element whose attribute should be read. |
| `AttributeName` | Attribute Name | InArgument | `string` | Yes | | Name of the attribute, in the casing the underlying driver expects (see notes). |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `Attribute` | Attribute | `string` | The attribute value as a string (e.g. `"true"`, `"false"`, `"some text"`). |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `Timeout` | Timeout | InArgument | `double` | | Maximum time (in milliseconds) to wait for the target element. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before reading the attribute. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after reading the attribute. |

## Common attribute names

Names below are the values you pass to `AttributeName`, as confirmed by the package's own QA test fixtures (`Qa\UiPath.MobileAutomation.RuntimeTests\.variations\tests data_GetAttribute.json`):

| Attribute | Returns | Platforms |
| --- | --- | --- |
| `checked` | `"true"` / `"false"` for `CheckBox`, toggles | Android |
| `enabled` | `"true"` / `"false"` | Android, iOS |
| `displayed` | `"true"` / `"false"` | Android, iOS |
| `selected` | `"true"` / `"false"` | Android, iOS |
| `clickable` | `"true"` / `"false"` | Android |
| `checkable` | `"true"` / `"false"` | Android |
| `focusable` | `"true"` / `"false"` | Android |
| `focused` | `"true"` / `"false"` | Android |
| `longClickable` | `"true"` / `"false"` | Android |
| `scrollable` | `"true"` / `"false"` | Android |
| `password` | `"true"` / `"false"` | Android, iOS |
| `text` | The element's visible text | Android, iOS |
| `className` | Class name (e.g. `"android.widget.Button"`) | Android |
| `contentDescription` | `content-desc` attribute value | Android |
| `resourceID` | Resource id (e.g. `"com.example.basicapp:id/button1"`) | Android |
| `package` | App package name | Android |
| `bounds` | `"[x1,y1][x2,y2]"` | Android |
| `selection-start` / `selection-end` | Selection indices on editable text | Android |

Attribute names are **lowercase camelCase**, **not** uppercase. Use `"checked"` not `"CHECKED"`, `"contentDescription"` not `"content-desc"`, `"resourceID"` not `"resource-id"`.

The exact set of supported attributes depends on the underlying Appium driver version and the element class. For Android the driver is UIAutomator2; for iOS it is XCUITest. Some attributes are platform-specific.

## XAML Example

Read whether a checkbox is checked:

```xml
<uma:GetAttribute DisplayName="Get Attribute"
                  AttributeName="checked"
                  Attribute="[checkedState]">
  <uma:GetAttribute.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.CheckBox' id='com.example.basicapp:id/checkbox' /&gt;" />
  </uma:GetAttribute.Target>
</uma:GetAttribute>
```

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:umm="clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation"`

## How to create a new Get Attribute

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.GetAttribute
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- The output `Attribute` is always a `string`; cast to `bool` / `int` in your workflow if you need a typed value.
- For text content, you can either use `GetAttribute` with `AttributeName = "text"` or use the dedicated `GetText` activity — `GetText` is preferred for plain text reads.
- If the requested attribute does not exist on the element, the behaviour is driver-dependent — most drivers return an empty string; some throw. Wrap in a `Try Catch` if you can't guarantee the attribute exists.

# Set Text

`UiPath.MobileAutomation.Activities.SetText`

Enter text into a specified UI element. Supports plain text or a `SecureString` for credential entry.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Target` | Target | Property | [`Target`](../common/Target.md) | Yes | | The text field to enter text into. |
| `Text` | Text | InArgument | `string` | when `IsSecure = false` | | Plaintext to type. Read only when `IsSecure = false`. |
| `SecureText` | Secure Text | InArgument | `SecureString` | when `IsSecure = true` | | Secure text to type. Read only when `IsSecure = true`. **Use this for passwords and other credentials** — values are not logged and do not appear in the workflow file as plaintext. |
| `IsSecure` | (designer toggle) | Property (`bool`) | `bool` | Yes (effective) | `false` | Discriminator that controls which of `Text` / `SecureText` is read. Hidden from the Studio designer (`[Browsable(false)]`) — Studio toggles it when the user picks the "Secure" overload — but **must be set explicitly to `True` in raw XAML** when authoring with `SecureText`, otherwise validation fails. |

### Configuration

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ClearText` | Clear Text | InArgument | `bool` | `true` | Clear any existing text in the field before typing. Set to `false` to append. |
| `TapBefore` | Tap Before | InArgument | [`SetTextTapBefore`](#set-text-tap-before-enum) | `Single` | Whether to tap the field before typing (to give it focus). |
| `SendNewLine` | Send New Line | InArgument | `bool` | `true` | Send a final newline (`Enter`) after the text. Often submits the field or moves focus. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `Timeout` | Timeout | InArgument | `double` | | Maximum time (in milliseconds) to wait for the target element. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before the activity runs. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after the activity runs. |

## Set Text Tap Before Enum

`UiPath.MobileAutomation.Activities.Enums.SetTextTapBefore`

| Value | Description |
|-------|-------------|
| `No` | Do not tap the field before typing. Use when the field is already focused. |
| `Single` | Single tap to focus the field. Default. |
| `Double` | Double tap (selects the existing word in many keyboards). |
| `Long` | Long press (often opens a context menu — useful before pasting). |

## XAML Examples

Plain text (non-sensitive):

```xml
<uma:SetText DisplayName="Set Text"
             Text="user@test.com"
             ClearText="True"
             TapBefore="Single"
             SendNewLine="False">
  <uma:SetText.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.EditText' id='com.example.basicapp:id/editText' /&gt;" />
  </uma:SetText.Target>
</uma:SetText>
```

Password via `SecureString` (recommended for any credential field) — note the required `IsSecure="True"` discriminator:

```xml
<uma:SetText DisplayName="Set Text"
             IsSecure="True"
             SecureText="[creds.Password]"
             ClearText="True"
             TapBefore="Single"
             SendNewLine="False">
  <uma:SetText.Target>
    <umm:Target FullSelector="&lt;mbl android:className='android.widget.EditText' id='com.demo.app:id/passwordField' /&gt;" />
  </uma:SetText.Target>
</uma:SetText>
```

Namespaces required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`
* `xmlns:umm="clr-namespace:UiPath.MobileAutomation.Models;assembly=UiPath.MobileAutomation"`

## How to create a new Set Text

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.SetText
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- **Use `SecureText`, not `Text`, for any credential / password input.** When `SecureText` is set, the underlying value never reaches the workflow log and is not stored as plaintext in the `.xaml` file. When authoring `SecureText` in raw XAML, also set `IsSecure="True"` — without it the activity reads `Text` (null) and validation fails.
- Set **either** `Text` (with `IsSecure="False"`, the default) **or** `SecureText` (with `IsSecure="True"`). Only the property matching `IsSecure` is read at runtime.
- `ClearText = true` (default) wipes existing field content first. Set to `false` to append.
- For typing into the currently focused element without resolving a `Target`, use `TypeText` instead.
- `SendNewLine = true` (default) sends an `Enter` after the text. For multi-line inputs where you want a literal newline, embed `\n` in the `Text` and set `SendNewLine = false` for the final keystroke.

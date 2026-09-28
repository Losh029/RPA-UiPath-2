# Type Text

`UiPath.MobileAutomation.Activities.TypeText`

Type text into whichever element currently has keyboard focus. Does **not** take a `Target` — use `SetText` for that.

**Package:** `UiPath.MobileAutomation.Activities`
**Category:** UI Automation
**Required Scope:** `UiPath.MobileAutomation.Activities.MobileDeviceConnection`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Text` | Text | InArgument | `string` | when `IsSecure = false` | | Plaintext to type. Read only when `IsSecure = false`. |
| `SecureText` | Secure Text | InArgument | `SecureString` | when `IsSecure = true` | | Secure text to type. Read only when `IsSecure = true`. **Use this for passwords and credentials.** |
| `IsSecure` | (designer toggle) | Property (`bool`) | `bool` | Yes (effective) | `false` | Discriminator that controls which of `Text` / `SecureText` is read. Hidden from the Studio designer (`[Browsable(false)]`) — Studio toggles it when the user picks the "Secure" overload — but **must be set explicitly to `True` in raw XAML** when authoring with `SecureText`, otherwise validation fails. |

### Configuration

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `SendNewLine` | Send New Line | InArgument | `bool` | `true` | Send a final newline (`Enter`) after the text. |

### Common

| Name | Display Name | Kind | Type | Default | Description |
|------|-------------|------|------|---------|-------------|
| `ContinueOnError` | Continue On Error | InArgument | `bool` | `false` | Continue executing the automation if this activity fails. |
| `DelayBefore` | Delay Before | InArgument | `double` | | Delay (in milliseconds) before typing. |
| `DelayAfter` | Delay After | InArgument | `double` | | Delay (in milliseconds) after typing. |

There is **no** `Target` / `Selector` property — `TypeText` always sends keystrokes to the currently focused element. There is also **no** `ClearText` property; the text is appended to whatever is in the focused field.

## XAML Example

```xml
<uma:TypeText DisplayName="Type Text"
              Text="hello world"
              SendNewLine="False" />
```

Password via `SecureString` — note the required `IsSecure="True"` discriminator:

```xml
<uma:TypeText DisplayName="Type Text"
              IsSecure="True"
              SecureText="[creds.Password]"
              SendNewLine="True" />
```

Namespace required:
* `xmlns:uma="clr-namespace:UiPath.MobileAutomation.Activities;assembly=UiPath.MobileAutomation.Activities"`

## How to create a new Type Text

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.MobileAutomation.Activities.TypeText
```

## Notes

- Must be placed inside a `MobileDeviceConnection` scope.
- **Use `SecureText` for any credential / password input.** Values never reach logs and are not stored as plaintext in the `.xaml` file. When `SecureText` is set, also set `IsSecure="True"` — without it the activity reads `Text` (null) and validation fails.
- Set **either** `Text` (with `IsSecure="False"`, the default) **or** `SecureText` (with `IsSecure="True"`). Setting both is allowed but only the one matching `IsSecure` is used.
- `TypeText` is a blind keyboard send — it does not resolve any element. If the wrong element has focus, the text goes to the wrong place. Prefer `SetText` (which has a `Target` and takes focus explicitly) when you know the destination field.
- Use `TypeText` when the focused element is a dynamically-shown overlay (e.g. an Appium-driven IME / virtual keyboard popup) that has no stable selector.
- `SendNewLine = true` submits the field via `Enter` after typing. Set to `false` to leave focus on the field.

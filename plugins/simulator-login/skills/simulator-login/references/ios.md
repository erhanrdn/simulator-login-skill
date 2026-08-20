# iOS Simulator Login

Use this reference only for iOS Simulator work.

## Select and Prepare the Device

List devices in a machine-readable form and choose the exact UDID:

```bash
xcrun simctl list devices --json
xcrun simctl bootstatus <UDID> -b
```

When more than one simulator is booted, pass the UDID to every `simctl` command. Confirm the app and environment before login. Launching by bundle identifier is deterministic:

```bash
xcrun simctl launch <UDID> <bundle-id>
```

Activate Simulator only after selecting the correct device window. If several Simulator windows are open, match the window title to the chosen device rather than assuming the frontmost window is correct.

## Inspect Before Input

Take a fresh device screenshot rather than a macOS window screenshot:

```bash
xcrun simctl io <UDID> screenshot --mask=black /tmp/simulator-login.png
```

Record its pixel dimensions and current orientation. Prefer accessibility identifiers or an existing UI test. If screenshot-guided taps are necessary, calculate positions from the current screenshot and actual Simulator content bounds; do not reuse absolute desktop coordinates after resizing, rotating, or changing devices.

## Enter Credentials Reliably

The device pasteboard is more reliable than simulated typing for spaces, punctuation, non-ASCII text, and complex passwords. Feed the value over standard input so it does not appear as a command argument:

```bash
printf '%s' "$SIMULATOR_LOGIN_VALUE" | xcrun simctl pbcopy <UDID>
```

After the intended field has focus, send Select All and Paste through the active Simulator window or the available UI-control tool. Paste one field at a time. Do not assume that setting the pasteboard itself enters text.

If iOS shows an “Allow Paste?” prompt, respond to that prompt before any further tap. Confirm visually that the email field contains the complete address and that the password field shows the expected non-empty masked state. If input seems truncated, clear and paste again instead of appending.

The software keyboard may cover the submit button. Scroll the form or dismiss the keyboard with the app’s normal action; avoid changing global keyboard settings unless needed. A hardware keyboard setting can change between Simulator sessions, so do not infer field failure until focus and field contents are checked.

Clear the device pasteboard after both values have been entered:

```bash
printf '' | xcrun simctl pbcopy <UDID>
```

## Verify and Diagnose

Wait for the authenticated view, then take another device screenshot. If the app is a WebView wrapper, confirm both the native shell state and the loaded protected page or URL when observable.

For failures, inspect the visible validation message and relevant app/network logs without exposing the password. Distinguish invalid credentials from focus loss, paste denial, an unreachable backend, TLS trust errors, and a WebView that has not finished loading. Do not erase the simulator or reinstall the app merely to solve an input-focus problem; those actions destroy useful session state and require separate justification.

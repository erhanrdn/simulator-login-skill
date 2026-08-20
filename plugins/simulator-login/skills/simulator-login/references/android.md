# Android Emulator Login

Use this reference only for Android Emulator work.

## Select and Prepare the Emulator

List devices and choose an explicit serial:

```bash
adb devices -l
adb -s <serial> shell getprop sys.boot_completed
adb -s <serial> shell wm size
```

Pass `-s <serial>` to every command when more than one device is connected. Confirm the installed package and intended backend before entering credentials.

## Prefer Element Bounds

Use existing UI automation or accessibility/resource identifiers when available. A UI hierarchy dump can provide current element bounds:

```bash
adb -s <serial> shell uiautomator dump /sdcard/simulator-login.xml
adb -s <serial> pull /sdcard/simulator-login.xml /tmp/simulator-login.xml
```

Tap the center of the current element bounds. Refresh the hierarchy after navigation, rotation, keyboard appearance, or scrolling. If the app is Flutter-rendered and the hierarchy lacks useful identifiers, use an existing Flutter integration test or fresh screenshot geometry rather than guessing.

## Enter Credentials Reliably

`adb shell input text` can corrupt spaces, shell metacharacters, and some non-ASCII characters. Use it only for values proven safe for that route. For complex passwords, prefer an existing UI test framework or a supported device clipboard operation followed by Android's paste key event. Probe clipboard support on the target image first because commands vary by Android version.

Fill and verify one field at a time. Clear old content before insertion, confirm the email visibly, and confirm that the password field is non-empty and masked. Close autofill suggestions or permission dialogs before tapping elsewhere.

Use `adb shell input tap <x> <y>` only with coordinates derived from the current device screenshot or current UI bounds. If the keyboard hides the submit control, use the form's IME action, scroll, or the normal Back action once; do not send repeated Back events that could leave the login screen.

## Verify and Diagnose

After submitting, wait for the expected authenticated activity or protected content, then capture a screenshot and refresh the UI hierarchy. Treat a successful protected request, authenticated activity, or account-specific UI as verification; a spinner or toast alone is insufficient.

On failure, classify the visible state before retrying: field content corruption, focus loss, IME obstruction, autofill overlay, WebView readiness, environment mismatch, TLS/network failure, validation error, or rejected credentials. Keep secrets out of logs and clear any device clipboard containing the password after use.

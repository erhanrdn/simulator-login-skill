---
name: simulator-login
description: Sign in to an app on an iOS Simulator or Android Emulator with user-supplied test credentials, handling device selection, field focus, virtual keyboards, paste restrictions, native or WebView forms, and authenticated-state verification. Use when a user asks to log in, prepare an authenticated simulator session, or capture signed-in screenshots; do not use to create accounts, recover passwords, or access credentials the user did not provide.
---

# Simulator Login

Create a real, verified authenticated session on the requested simulator or emulator. Treat reaching the post-login screen—not merely filling the fields or tapping the button—as success.

## Boundaries

- Use only credentials the user supplied or an explicitly local test fixture already in scope. Never retrieve a live password, reset credentials, create an account, bypass authentication, or mutate production data unless the user separately authorizes that action.
- Do not print passwords in commentary, command output, screenshots, or the final response. Avoid embedding secrets in shell command strings; pass them through a non-echoing prompt, protected input channel, or device clipboard, then clear that clipboard.
- Login authorization does not authorize source changes. Prefer existing automation hooks. Ask before adding test-only login bypasses; a bypass does not count as testing real authentication.
- If several devices are booted, resolve and use an explicit UDID or serial. Never rely on an ambiguous `booted` target.

## Workflow

1. Identify the requested platform, exact device, app identifier, environment, and expected destination after login. Preserve an existing signed-in session unless the user asked for a different account.
2. Confirm that the target backend is the intended local, staging, or production environment before entering credentials. For screenshot preparation, prefer local or staging data unless the user explicitly requests production.
3. Choose the most reliable available control path in this order:
   - An existing integration/UI test with stable semantic identifiers.
   - Accessibility- or DOM-based element targeting.
   - Screenshot-guided taps after measuring the current device and window geometry.
   Do not guess stale coordinates from a different device or orientation.
4. Capture a fresh screenshot and wait for the login screen to become stable. Determine whether the form is native, Flutter-rendered, or inside a WebView. Read the relevant platform reference before acting:
   - iOS Simulator: [references/ios.md](references/ios.md)
   - Android Emulator: [references/android.md](references/android.md)
5. Fill one field at a time. Focus the field, select existing content, paste or type the exact value, and confirm that the field is non-empty before moving on. Account for password managers, autofill overlays, paste prompts, keyboard coverage, and validation that runs on blur.
6. Dismiss the keyboard only if it hides the submit control. Tap the real sign-in action once and wait for navigation or a server response; avoid repeated taps that can submit multiple requests.
7. Verify authentication with at least one strong signal and one supporting signal when available:
   - Strong: the login form disappears and the expected authenticated route, tab, or account-specific UI loads.
   - Supporting: a session cookie/token exists, a protected request succeeds, or a known signed-in label is visible.
   A spinner, toast, or changed button state alone is not sufficient.
8. On failure, capture the screen and classify the cause before retrying. Retry at most three times, changing the method only in response to evidence. Common causes are wrong environment, wrong account, field focus loss, special-character corruption, keyboard obstruction, paste permission, WebView not ready, validation errors, or an authentication/network response.
9. Leave the requested device open on the authenticated destination. Report the device, account identifier (never its password), backend environment, and verification signal. If blocked, report the exact observed state and preserve diagnostic screenshots.

## Repeated Workflows

When the project already has UI automation, use stable keys or accessibility labels for the email field, password field, submit action, and authenticated destination. Supply secrets at runtime rather than hardcoding them. The automation must exercise the real authentication request and should stop on the authenticated screen so later screenshot or testing work can continue.

For Flutter, prefer an existing `integration_test` flow using `ValueKey` or semantics labels. For a WebView, wait for page readiness and target DOM/accessibility elements when an inspectable session is available; otherwise use fresh screenshots and measured coordinates.

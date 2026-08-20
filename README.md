# Simulator Login Skill

A portable [Agent Skill](https://agentskills.io) for reliably signing in to apps on iOS Simulator and Android Emulator with user-supplied test credentials.

The skill handles ambiguous device selection, field focus, virtual keyboards, paste restrictions, native and WebView forms, special-character passwords, bounded retries, and authenticated-state verification. It works with both Codex and Claude Code.

## Compatibility

| Agent | Personal skill location | Explicit invocation |
| --- | --- | --- |
| Codex | `~/.agents/skills/simulator-login` | `$simulator-login` |
| Claude Code | `~/.claude/skills/simulator-login` | `/simulator-login` |

Both agents may also activate the skill automatically when a request matches its description.

## Install for Codex

Ask Codex to install it:

```text
$skill-installer Install the simulator-login skill from https://github.com/erhanrdn/simulator-login-skill
```

Or install it manually:

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/erhanrdn/simulator-login-skill.git ~/.agents/skills/simulator-login
```

Run `/skills` to confirm that it is available. Codex normally detects new skills automatically; restart Codex if it does not appear.

## Install for Claude Code

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/erhanrdn/simulator-login-skill.git ~/.claude/skills/simulator-login
```

Run `/skills` in Claude Code to confirm that it is available. If the personal skills directory did not exist when the session started, restart Claude Code once.

## Install once for both agents

```bash
mkdir -p ~/.local/share/agent-skills ~/.agents/skills ~/.claude/skills
git clone https://github.com/erhanrdn/simulator-login-skill.git ~/.local/share/agent-skills/simulator-login
ln -s ~/.local/share/agent-skills/simulator-login ~/.agents/skills/simulator-login
ln -s ~/.local/share/agent-skills/simulator-login ~/.claude/skills/simulator-login
```

Both Codex and Claude Code support symlinked skill folders.

## Use it

Codex:

```text
$simulator-login Sign in on the iPad simulator with the test credentials I provide, open the dashboard, and verify the authenticated session.
```

Claude Code:

```text
/simulator-login Sign in on the iPad simulator with the test credentials I provide, open the dashboard, and verify the authenticated session.
```

Natural-language invocation also works:

```text
Log in to the app on the currently booted Android emulator with the supplied test account. Confirm that the protected home screen loads and leave the emulator open there.
```

Provide the test account in the same private session. The skill instructs the agent not to print the password, retrieve unknown credentials, reset accounts, bypass authentication, or mutate production data.

## Requirements

- An agent with local shell and simulator/emulator UI-control access.
- Xcode and `xcrun simctl` for iOS Simulator work.
- Android Platform Tools and `adb` for Android Emulator work.
- An installed app and a test account supplied by the user or already present in an explicitly local test fixture.

## Update

Run `git pull` inside the installed skill directory. Symlinked installations need only one update in `~/.local/share/agent-skills/simulator-login`.

## Repository structure

```text
simulator-login-skill/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── ios.md
    └── android.md
```

`SKILL.md` uses the open Agent Skills format. `agents/openai.yaml` adds optional Codex/ChatGPT UI metadata; Claude Code can use the core skill without it.

## License

MIT

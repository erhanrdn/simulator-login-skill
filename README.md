# Simulator Login Plugin

`simulator-login` is a portable Codex and Claude Code plugin for reliably signing in to apps on iOS Simulator and Android Emulator with user-supplied test credentials.

It handles device selection, field focus, virtual keyboards, paste restrictions, native or WebView forms, special-character passwords, bounded retries, and authenticated-state verification. It does not create accounts, recover passwords, retrieve credentials, or bypass authentication.

## Install from the marketplace repository

### Codex

```bash
codex plugin marketplace add erhanrdn/simulator-login-skill --ref main
codex plugin add simulator-login@erhanrdn-tools
```

Start a new Codex task after installation, then invoke the skill explicitly:

```text
$simulator-login Sign in on the iPad simulator with the test credentials I provide, open the dashboard, and verify the authenticated session.
```

### Claude Code

```bash
claude plugin marketplace add erhanrdn/simulator-login-skill
claude plugin install simulator-login@erhanrdn-tools
```

Restart Claude Code or run `/reload-plugins` when prompted, then invoke the namespaced skill:

```text
/simulator-login:simulator-login Sign in on the iPad simulator with the test credentials I provide and verify the authenticated session.
```

Both agents may also activate the skill automatically when a request matches its description.

## Install only the skill

Codex users who do not want to install a plugin can ask the built-in installer to install the skill subdirectory:

```text
$skill-installer Install the simulator-login skill from https://github.com/erhanrdn/simulator-login-skill/tree/main/plugins/simulator-login/skills/simulator-login
```

Manual installation is also possible:

```bash
git clone https://github.com/erhanrdn/simulator-login-skill.git
mkdir -p ~/.agents/skills
cp -R simulator-login-skill/plugins/simulator-login/skills/simulator-login ~/.agents/skills/simulator-login
```

For Claude Code, copy the same directory to `~/.claude/skills/simulator-login` and invoke it as `/simulator-login`.

## Requirements

- An agent with local shell and simulator/emulator UI-control access.
- Xcode and `xcrun simctl` for iOS Simulator work.
- Android Platform Tools and `adb` for Android Emulator work.
- An installed app and test credentials supplied by the user or already present in an explicitly local test fixture.

The plugin has no MCP server, external API, telemetry, or runtime dependency. Credentials stay within the user's active agent session and target device workflow.

## Repository structure

```text
simulator-login-skill/
├── .agents/plugins/marketplace.json       # Codex marketplace catalog
├── .claude-plugin/marketplace.json        # Claude Code marketplace catalog
├── plugins/simulator-login/
│   ├── .codex-plugin/plugin.json
│   ├── .claude-plugin/plugin.json
│   └── skills/simulator-login/
│       ├── SKILL.md
│       ├── agents/openai.yaml
│       └── references/
│           ├── ios.md
│           └── android.md
├── CHANGELOG.md
├── PRIVACY.md
└── TERMS.md
```

## Development and validation

```bash
claude plugin validate . --strict
claude plugin validate plugins/simulator-login --strict
```

Codex validation uses the plugin validator bundled with the `plugin-creator` skill. The canonical plugin version is stored in both platform manifests and must be bumped for each release.

## Support and security

Open a [GitHub issue](https://github.com/erhanrdn/simulator-login-skill/issues) for bugs or compatibility problems. Do not include passwords, tokens, private screenshots, or production account details in an issue.

See [PRIVACY.md](PRIVACY.md), [TERMS.md](TERMS.md), and [LICENSE](LICENSE).

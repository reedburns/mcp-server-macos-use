# mcp-server-macos-use

Pre-built binaries and Homebrew formula for [mediar-ai/mcp-server-macos-use](https://github.com/mediar-ai/mcp-server-macos-use) — an MCP server that lets AI agents control macOS via the Accessibility API.

Open apps, click buttons, type text, press keys, scroll, and read the full UI tree — all through the standard [Model Context Protocol](https://modelcontextprotocol.io/).

## Install

### Homebrew (recommended)

```bash
brew tap reedburns/mcp-server-macos-use
brew install mcp-server-macos-use
```

### Manual

Download from [Releases](https://github.com/reedburns/mcp-server-macos-use/releases):

```bash
tar xzf mcp-server-macos-use-0.1.0-universal.tar.gz
sudo mv mcp-server-macos-use /usr/local/bin/
```

### Build from source

```bash
git clone https://github.com/reedburns/mcp-server-macos-use.git
cd mcp-server-macos-use
swift build -c release
# Binary: .build/release/mcp-server-macos-use
```

## Setup

### 1. Grant Accessibility permission

**System Settings → Privacy & Security → Accessibility** → add `mcp-server-macos-use`

### 2. Configure your MCP client

<details>
<summary><b>Claude Desktop / Cursor</b></summary>

Add to your MCP config (`claude_desktop_config.json` or Cursor settings):

```json
{
  "mcpServers": {
    "macos-use": {
      "command": "mcp-server-macos-use"
    }
  }
}
```
</details>

<details>
<summary><b>OpenClaw (via mcporter)</b></summary>

```bash
mcporter config add macos-use --transport stdio --command $(which mcp-server-macos-use)
mcporter list macos-use --schema
```
</details>

<details>
<summary><b>OpenClaw Skill (ClawHub)</b></summary>

```bash
clawhub install mac-compute-use
```

Or visit: [clawhub.ai/skills/mac-compute-use](https://clawhub.ai/skills/mac-compute-use)
</details>

## Tools

| Tool | Description |
|---|---|
| `open_application_and_traverse` | Open/activate an app and get its accessibility tree |
| `click_and_traverse` | Click at coordinates and get updated UI state |
| `type_and_traverse` | Type text into the focused app |
| `press_key_and_traverse` | Press a key with optional modifiers (Cmd, Shift, etc.) |
| `scroll_and_traverse` | Scroll within an app window |
| `refresh_traversal` | Get current UI state without performing any action |

## Usage examples

```bash
# Open an app
mcporter call macos-use.macos-use_open_application_and_traverse identifier="Google Chrome"

# Click a button (coordinates from the UI tree)
mcporter call macos-use.macos-use_click_and_traverse pid=408 x=701 y=73 width=102 height=41

# Type text
mcporter call macos-use.macos-use_type_and_traverse pid=408 text="Hello world"

# Press Cmd+A
mcporter call macos-use.macos-use_press_key_and_traverse pid=408 keyName=a modifierFlags='["Command"]'

# Scroll down
mcporter call macos-use.macos-use_scroll_and_traverse pid=408 x=500 y=400 deltaY=3
```

## Requirements

- macOS 13.0+ (Ventura)
- Accessibility permission

## Credits

This is a fork of [mediar-ai/mcp-server-macos-use](https://github.com/mediar-ai/mcp-server-macos-use) by [Matt](https://github.com/m13v) and the [Mediar AI](https://github.com/mediar-ai) team. All the hard work on the Swift MCP server and the [MacosUseSDK](https://github.com/mediar-ai/MacosUseSDK) is theirs — we just added pre-built binaries, a Homebrew formula, and a CI pipeline so anyone can install it in seconds.

Thank you for building this in the open! 🙏

For the original documentation, see [README_OLD.md](README_OLD.md).

## License

[MIT](LICENSE) — same as the original project.

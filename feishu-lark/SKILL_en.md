---
name: feishu-lark
description: Use when sending messages, searching contacts, or listening to real-time messages via Feishu user Cookie (non-bot Token). For personal Feishu account command-line messaging operations.
---

# Feishu CLI Tool (feishu-lark)

Feishu command-line messaging tool based on [lark-cli](https://github.com/echowxsy/lark-cli). Uses user Cookie authentication (non-bot Token), supports sending messages, searching contacts, and WebSocket real-time listening.

## Quick Start

**First-time setup requires Cookie configuration**:

```bash
# 1. Install dependencies
cd ~/python/cooperxu_skills/feishu-lark
pip install -r requirements.txt

# 2. Configure Cookie (follow prompts)
python3 lark_cli.py setup

# 3. Verify configuration
python3 lark_cli.py me
```

## Commands

| Command | Description | Example |
|---------|-------------|---------|
| `setup` | Configure Feishu Cookie | `python3 lark_cli.py setup` |
| `me` | View current logged-in user | `python3 lark_cli.py me` |
| `search <keyword>` | Search users/groups | `python3 lark_cli.py search "John"` |
| `send --to <name> --msg <text>` | Send message (by username) | `python3 lark_cli.py send --to "John" --msg "Hello"` |
| `send --chat-id <id> --msg <text>` | Send message (by chat_id) | `python3 lark_cli.py send --chat-id "oc_xxx" --msg "Hello"` |
| `notify <text>` | Send notification to yourself | `python3 lark_cli.py notify "Reminder content"` |
| `watch [--json]` | Real-time message listening | `python3 lark_cli.py watch` |

## Cookie Configuration Guide

### Get Your Cookie

1. Open [Feishu Web](https://www.feishu.cn/messenger/)
2. Log in to your Feishu account
3. Press F12 to open DevTools
4. Go to **Application → Cookies**, copy all cookies
   - Or execute `document.cookie` in Console

### Configuration Methods (choose one)

**Method 1: Config file (recommended)**
```json
// ~/.config/lark-cli/config.json
{
  "cookie": "your_cookie_string"
}
```

**Method 2: Environment variable**
```bash
export LARK_COOKIE="your_cookie_string"
```

### Cookie Expiration

Cookies expire (typically a few days to a week). When expired, commands will show errors. Re-run `python3 lark_cli.py setup` to configure a new Cookie.

**Auto-detection**: The tool will提示 when Cookie may have expired upon API call failures.

## Usage Examples

### 1. Send Messages

```bash
# Send to contact (auto-search by username)
python3 lark_cli.py send --to "John" --msg "Meeting at 3pm"

# Send to specific chat_id
python3 lark_cli.py send --chat-id "oc_xxxxxxxxxx" --msg "Project update"

# Send notification to yourself (todo reminder)
python3 lark_cli.py notify "Remember to drink water in 10 minutes"
```

### 2. Search Contacts

```bash
# Search users
python3 lark_cli.py search "John"

# Search groups
python3 lark_cli.py search "Project Team"
```

Output format:
```
[ user] 123456  John Doe
[group] 789012  Project Discussion
```

### 3. Real-time Message Listening

```bash
# Human-readable format
python3 lark_cli.py watch

# JSON format (for pipeline processing)
python3 lark_cli.py watch --json
```

JSON output example:
```json
{"from_id":"123456","from_name":"John","chat_id":"oc_xxx","chat_type":"p2p","group_name":null,"content":"Message content","is_from_me":false,"timestamp":1709453000}
```

**Run in background** (using nohup or tmux):
```bash
# nohup method
nohup python3 lark_cli.py watch --json > lark_watch.log 2>&1 &

# tmux method
tmux new -s lark-watch
python3 lark_cli.py watch
# Ctrl+B, D to detach
```

### 4. View Current User

```bash
python3 lark_cli.py me
```

Output:
```
User: John Doe
  ID: 123456
```

## File Structure

```
feishu-lark/
├── lark_cli.py      # CLI entry point, command parsing
├── lark_api.py      # Feishu API client
├── lark_daemon.py   # WebSocket real-time listening
├── lark_proto.py    # Protobuf message encoding/decoding
├── lark_utils.py    # Utility functions (request ID, access key generation)
├── proto_pb2.py     # Compiled protobuf definitions
└── requirements.txt # Python dependencies
```

## FAQ

### Q: How often do Cookies expire?
A: Typically a few days to a week, depending on Feishu's security policy. Re-run `setup` when expired.

### Q: Can I send images/files?
A: Current version only supports plain text messages.

### Q: Does watch mode disconnect?
A: It may disconnect due to network issues or Cookie expiration. The tool auto-reconnects (5s retry).

### Q: Is group chat supported?
A: Yes. In `watch` mode, group messages show the group name; `send` mode requires `--chat-id`.

### Q: Difference from official Bot API?
A: This tool uses **user Cookie**, simulating real user operations, no bot app creation needed. Suitable for personal use cases.

## Security Notes

- ⚠️ Cookie is equivalent to login credentials, **do not share with others**
- ⚠️ Config file `~/.config/lark-cli/config.json` permissions should be set to `600`
- ⚠️ This tool uses unofficial API, for personal learning use only

## Related Resources

- Original project: [echowxsy/lark-cli](https://github.com/echowxsy/lark-cli)
- Protocol analysis based on: [LarkAgentX](https://github.com/cv-cat/LarkAgentX)

---

*Last updated: 2026-03-03*

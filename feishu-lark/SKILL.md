---
name: feishu-lark
description: 当需要通过飞书用户 Cookie（非机器人 Token）发送消息、搜索联系人、实时监听消息时使用。适用于个人飞书账号的命令行消息操作。
---

# 飞书 CLI 工具 (feishu-lark)

基于 [lark-cli](https://github.com/echowxsy/lark-cli) 封装的飞书命令行消息工具。使用用户 Cookie 认证（非机器人 Token），支持发消息、搜索联系人、WebSocket 实时监听。

## 快速开始

**首次使用需要配置 Cookie**：

```bash
# 1. 安装依赖
cd ~/python/cooperxu_skills/feishu-lark
pip install -r requirements.txt

# 2. 配置 Cookie（按提示操作）
python3 lark_cli.py setup

# 3. 验证配置
python3 lark_cli.py me
```

## 功能命令

| 命令 | 描述 | 示例 |
|------|------|------|
| `setup` | 配置飞书 Cookie | `python3 lark_cli.py setup` |
| `me` | 查看当前登录用户 | `python3 lark_cli.py me` |
| `search <关键词>` | 搜索用户/群组 | `python3 lark_cli.py search "张三"` |
| `send --to <用户名> --msg <消息>` | 发送消息（按用户名） | `python3 lark_cli.py send --to "张三" --msg "你好"` |
| `send --chat-id <id> --msg <消息>` | 发送消息（按 chat_id） | `python3 lark_cli.py send --chat-id "oc_xxx" --msg "你好"` |
| `notify <消息>` | 给自己发通知 | `python3 lark_cli.py notify "提醒内容"` |
| `watch [--json]` | 实时监听消息 | `python3 lark_cli.py watch` |

## Cookie 配置指南

### 获取 Cookie

1. 打开 [飞书网页版](https://www.feishu.cn/messenger/)
2. 登录你的飞书账号
3. 按 F12 打开开发者工具
4. 进入 **Application → Cookies**，复制所有 Cookie
   - 或在 Console 执行 `document.cookie`

### 配置方式（二选一）

**方式 1：配置文件（推荐）**
```json
// ~/.config/lark-cli/config.json
{
  "cookie": "your_cookie_string"
}
```

**方式 2：环境变量**
```bash
export LARK_COOKIE="your_cookie_string"
```

### Cookie 过期处理

Cookie 会过期（通常几天到一周）。过期后执行命令会提示错误，重新运行 `python3 lark_cli.py setup` 配置新 Cookie 即可。

**自动检测**：工具会在 API 调用失败时自动提示 Cookie 可能已过期。

## 使用场景

### 1. 发送消息

```bash
# 给联系人发消息（自动搜索用户名）
python3 lark_cli.py send --to "张三" --msg "下午开会别迟到"

# 给指定 chat_id 发消息
python3 lark_cli.py send --chat-id "oc_xxxxxxxxxx" --msg "项目进度更新"

# 给自己发通知（待办提醒）
python3 lark_cli.py notify "10 分钟后记得喝水"
```

### 2. 搜索联系人

```bash
# 搜索用户
python3 lark_cli.py search "张三"

# 搜索群组
python3 lark_cli.py search "项目组"
```

输出格式：
```
[ user] 123456  张三
[group] 789012  项目讨论组
```

### 3. 实时监听消息

```bash
# 人类可读格式
python3 lark_cli.py watch

# JSON 格式（适合管道处理）
python3 lark_cli.py watch --json
```

JSON 输出示例：
```json
{"from_id":"123456","from_name":"张三","chat_id":"oc_xxx","chat_type":"p2p","group_name":null,"content":"消息内容","is_from_me":false,"timestamp":1709453000}
```

**后台运行**（使用 nohup 或 tmux）：
```bash
# nohup 方式
nohup python3 lark_cli.py watch --json > lark_watch.log 2>&1 &

# tmux 方式
tmux new -s lark-watch
python3 lark_cli.py watch
# Ctrl+B, D 分离会话
```

### 4. 查看当前用户

```bash
python3 lark_cli.py me
```

输出：
```
User: 张三
  ID: 123456
```

## 文件结构

```
feishu-lark/
├── lark_cli.py      # CLI 入口，命令解析
├── lark_api.py      # Feishu API 客户端
├── lark_daemon.py   # WebSocket 实时监听
├── lark_proto.py    # Protobuf 消息编解码
├── lark_utils.py    # 工具函数（request ID、access key 生成）
├── proto_pb2.py     # 编译后的 protobuf 定义
└── requirements.txt # Python 依赖
```

## 常见问题

### Q: Cookie 多久过期？
A: 通常几天到一周，取决于飞书的安全策略。过期后重新运行 `setup` 配置即可。

### Q: 能发图片/文件吗？
A: 当前版本只支持纯文本消息。

### Q: watch 模式会断开吗？
A: 网络波动或 Cookie 过期时会断开，工具会自动重连（每 5 秒重试）。

### Q: 支持群聊吗？
A: 支持。`watch` 模式下群聊消息会显示群名；`send` 模式需要指定 `--chat-id`。

### Q: 和官方机器人 API 有什么区别？
A: 本工具使用**用户 Cookie**，模拟真实用户操作，不需要创建机器人应用。适合个人使用场景。

## 安全提示

- ⚠️ Cookie 等同于登录凭证，**不要分享给他人**
- ⚠️ 配置文件 `~/.config/lark-cli/config.json` 权限建议设为 `600`
- ⚠️ 本工具使用非官方 API，仅供个人学习使用

## 相关资源

- 原项目：[echowxsy/lark-cli](https://github.com/echowxsy/lark-cli)
- 协议分析基于：[LarkAgentX](https://github.com/cv-cat/LarkAgentX)

---

*更新时间：2026-03-03*

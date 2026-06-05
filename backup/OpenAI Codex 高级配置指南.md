# OpenAI Codex 高级配置指南

> 来源：https://developers.openai.com/codex/config-advanced

---

## 目录

- [一、配置文件分层](#一配置文件分层)
- [二、Profiles 配置文件集](#二profiles-配置文件集)
- [三、CLI 单次覆盖](#三cli-单次覆盖)
- [四、Hooks（钩子）](#四hooks钩子)
- [五、模型提供商配置](#五模型提供商配置)
- [六、模型推理、详细度与限制](#六模型推理详细度与限制)
- [七、审批策略与沙箱模式](#七审批策略与沙箱模式)
- [八、Shell 环境策略](#八shell-环境策略)
- [九、项目根检测](#九项目根检测)
- [十、项目指令发现](#十项目指令发现)
- [十一、可点击引用](#十一可点击引用)
- [十二、历史记录持久化](#十二历史记录持久化)
- [十三、通知](#十三通知)
- [十四、TUI 配置](#十四tui-配置)
- [十五、可观测性与遥测](#十五可观测性与遥测)
- [十六、匿名用量统计](#十六匿名用量统计)
- [十七、反馈控制](#十七反馈控制)
- [十八、推理可见性](#十八推理可见性)
- [十九、Agent Roles](#十九agent-roles)
- [二十、MCP Servers](#二十mcp-servers)
- [完整配置示例](#完整配置示例)

---

## 一、配置文件分层

### 1.1 用户级配置 (`~/.codex/config.toml`)

| 文件 | 用途 |
|------|------|
| `config.toml` | 用户级主配置文件 |
| `auth.json` | 凭据存储（文件或 OS keychain） |
| `history.jsonl` | 会话历史（如果启用持久化） |
| 其他 | 日志、缓存等 |

`CODEX_HOME` 环境变量可覆盖 `~/.codex` 默认路径。

### 1.2 项目级配置 (`.codex/config.toml`)

- Codex 从项目根目录向 CWD 逐层查找 `.codex/config.toml`
- **距离 CWD 最近的文件优先级最高**
- 仅当项目被标记为 **trusted** 时才加载；untrusted 项目忽略此类配置
- 相对路径相对于包含它的 `.codex/` 目录解析

### 1.3 禁止在项目配置中覆盖的键

以下 key **只能**在用户级配置中设置，在项目配置中出现会打印启动警告：

```
openai_base_url
chatgpt_base_url
model_provider
model_providers
notify
profile
profiles
experimental_realtime_ws_base_url
otel
```

---

## 二、Profiles 配置文件集 (`[profiles.<name>]`)

> **状态：Experimental**，IDE 扩展中不支持。

通过 `codex --profile <name>` 切换配置集。

| 字段 | 说明 | 可选值 | 默认值 |
|------|------|--------|--------|
| `profile`（顶层） | 设置默认加载的 profile | String（profile 名称） | — |
| `model` | 模型覆盖 | 模型 ID | — |
| `approval_policy` | 审批严格度 | `"on-request"`, `"never"`, `"untrusted"`, 或细粒度对象 | — |
| `model_reasoning_effort` | 推理级别 | `"high"`, `"low"`, `"medium"` | — |
| `model_catalog_json` | 自定义模型目录路径 | 文件路径 | — |

**示例：**

```toml
[profiles.deep-review]
model = "gpt-5-pro"
model_reasoning_effort = "high"
approval_policy = "never"
```

> `model_catalog_json` 同时存在于顶层和 profile 中时，profile 优先。

---

## 三、CLI 单次覆盖

| 方式 | 语法 | 说明 |
|------|------|------|
| 专用 flag | `codex --model gpt-5.4` | 有专用 flag 时优先使用 |
| `-c`/`--config` | `codex --config model='"gpt-5.4"'` | 值以 TOML 解析，支持点号嵌套（如 `mcp_servers.context7.enabled=false`） |

若 TOML 解析失败，值被当作纯字符串处理。

---

## 四、Hooks（钩子）

加载优先级依次为：

1. `~/.codex/hooks.json`
2. `~/.codex/config.toml`
3. `<repo>/.codex/hooks.json`
4. `<repo>/.codex/config.toml`

如果同一层同时存在 `hooks.json` 和内联 `[hooks]` 表，两者均加载但会打印警告。

**内联 TOML 结构：**

```toml
[[hooks.PreToolUse]]
matcher = "^Bash$"

[[hooks.PreToolUse.hooks]]
type = "command"
command = '/usr/bin/python3 "$(git rev-parse --show-toplevel)/.codex/hooks/pre_tool_use_policy.py"'
timeout = 30
statusMessage = "Checking Bash command"
```

> 项目级 hooks 仅在对应 `.codex/` 层级被信任时加载。

---

## 五、模型提供商配置 (`[model_providers.<id>]`)

### 5.1 通用自定义提供商

保留的不可重复使用的 provider ID：`openai`, `ollama`, `lmstudio`。

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `model_provider` | 选择使用哪个提供商 | Provider ID |
| `name` | 人类可读名称 | String |
| `base_url` | API 基础 URL | URL |
| `env_key` | 存放 API key 的环境变量名 | String（环境变量名） |
| `wire_api` | API 协议 | `"responses"` |
| `http_headers` | 静态请求头 | `{ "Header" = "value" }` |
| `env_http_headers` | 从环境变量读取的动态请求头 | `{ "Header" = "ENV_VAR_NAME" }` |

#### 命令式认证（auth block）

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `command` | 凭据辅助脚本路径 | 文件路径 |
| `args` | 传递给命令的参数 | String 数组 |
| `timeout_ms` | 命令超时 | 毫秒（如 `5000`） |
| `refresh_interval_ms` | Token 刷新间隔 | 毫秒；`0` = 仅在重试后刷新 |

> auth 命令无 stdin 输入，必须将 token 打印到 stdout。不能与 `env_key`、`experimental_bearer_token`、`requires_openai_auth` 同时使用。

**示例（带认证的代理）：**

```toml
[model_providers.proxy]
name = "OpenAI using LLM proxy"
base_url = "https://proxy.example.com/v1"
wire_api = "responses"

[model_providers.proxy.auth]
command = "/usr/local/bin/fetch-codex-token"
args = ["--audience", "codex"]
timeout_ms = 5000
refresh_interval_ms = 300000
```

### 5.2 Amazon Bedrock

设置 `model_provider = "amazon-bedrock"`，支持嵌套 AWS 配置：

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `profile` | AWS 配置文件名称 | String；省略则使用标准凭据链 |
| `region` | Bedrock 区域 | String（如 `"eu-central-1"`） |

### 5.3 Azure Provider

```toml
[model_providers.azure]
name = "Azure"
base_url = "https://YOUR_PROJECT_NAME.openai.azure.com/openai"
env_key = "AZURE_OPENAI_API_KEY"
query_params = { api-version = "2025-04-01-preview" }
wire_api = "responses"
request_max_retries = 4
stream_max_retries = 10
stream_idle_timeout_ms = 300000
```

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `query_params` | 固定查询参数 | Object |
| `request_max_retries` | 最大请求重试次数 | Integer |
| `stream_max_retries` | 最大流式重试次数 | Integer |
| `stream_idle_timeout_ms` | 流空闲超时 | 毫秒 |

> 要修改内置 OpenAI 提供商的 base_url，使用 `openai_base_url`，不要创建 `[model_providers.openai]`。

### 5.4 ChatGPT Data Residency

```toml
model_provider = "openaidr"
[model_providers.openaidr]
name = "OpenAI Data Residency"
base_url = "https://us.api.openai.com/v1"
```

将 `us` 替换为正确的域名前缀。

### 5.5 OSS 模式

`--oss` 时默认提供商的默认值：

| 字段 | 说明 | 可选值 | 默认值 |
|------|------|--------|--------|
| `oss_provider` | `--oss` 的默认本地提供商 | `"ollama"` 或 `"lmstudio"` | — |

---

## 六、模型推理、详细度与限制

| 字段 | 说明 | 可选值 | 默认值 |
|------|------|--------|--------|
| `model_reasoning_summary` | 控制推理摘要 | `"none"` | — |
| `model_verbosity` | 缩短模型响应（仅 Responses API） | `"low"` | — |
| `model_supports_reasoning_summaries` | 强制开启/关闭推理 | `true`/`false` | — |
| `model_context_window` | 设置上下文窗口大小 | Integer（如 `128000`） | — |

---

## 七、审批策略与沙箱模式

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `approval_policy` | 审批严格度 | `"untrusted"`, `"on-request"`, `"never"`, 或 `{ granular = { ... } }` |
| `approvals_reviewer` | 审批人 | `"user"` 或 `"auto_review"` |
| `sandbox_mode` | 沙箱隔离级别 | `"workspace-write"`, `"danger-full-access"` |
| `allow_login_shell` | 禁止 shell 工具的 login shell | `true`/`false` |

### 细粒度审批 (`approval_policy = { granular = { ... } }`)

| 子键 | 效果 |
|------|------|
| `sandbox_approval` | `true`/`false` |
| `rules` | `true`/`false` |
| `mcp_elicitations` | `true`/`false` |
| `request_permissions` | `true`/`false` |
| `skill_approval` | `true`/`false` |

### Sandbox Workspace Write (`[sandbox_workspace_write]`)

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `exclude_tmpdir_env_var` | 允许/阻止 `$TMPDIR` | `true`/`false` |
| `exclude_slash_tmp` | 允许/阻止 `/tmp` | `true`/`false` |
| `writable_roots` | 可写目录 | 路径数组 |
| `network_access` | 出站网络访问 | `true`/`false` |

> 在 workspace-write 模式下，部分环境会保持 `.git/` 和 `.codex/` 为只读。

### 完全禁用沙箱

```toml
sandbox_mode = "danger-full-access"
```

> 仅在您的环境已隔离进程时使用。

### Auto-Review (`[auto_review]`)

| 字段 | 说明 |
|------|------|
| `policy` | 本地审查策略指令（字符串） |

托管 `guardian_policy_config` 优先于本地策略。

---

## 八、Shell 环境策略 (`[shell_environment_policy]`)

控制哪些环境变量传递给子进程。

| 字段 | 说明 | 可选值 | 默认值 |
|------|------|--------|--------|
| `inherit` | 起始环境变量集 | `"none"`, `"core"` | — |
| `set` | 要设置的键值对 | Object | — |
| `ignore_default_excludes` | 是否应用内置 KEY/SECRET/TOKEN 过滤器 | `true`/`false` | — |
| `exclude` | 要排除的 glob 模式 | 大小写不敏感的 glob 数组 | — |
| `include_only` | 仅包含这些变量 | 模式数组 | — |

**示例：**

```toml
[shell_environment_policy]
inherit = "none"
set = { PATH = "/usr/bin", MY_FLAG = "1" }
ignore_default_excludes = false
exclude = ["AWS_*", "AZURE_*"]
include_only = ["PATH", "HOME"]
```

---

## 九、项目根检测

| 字段 | 说明 | 可选值 | 默认值 |
|------|------|--------|--------|
| `project_root_markers` | 标识项目根的文件 | 字符串数组 | `[".git"]` |

设为 `[]` 可跳过父目录搜索，将当前目录视为项目根。

---

## 十、项目指令发现

| 字段 | 说明 | 可选值 | 默认值 |
|------|------|--------|--------|
| `project_doc_max_bytes` | 每个 `AGENTS.md` 的最大读取字节数 | Integer | — |
| `project_doc_fallback_filenames` | `AGENTS.md` 缺失时的备选文件名 | 字符串数组 | — |

---

## 十一、可点击引用

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `file_opener` | 文件链接的 URI scheme | `"vscode"`, `"cursor"`, `"windsurf"`, `"vscode-insiders"`, `"none"` |

将 `/home/user/project/main.py:42` 转换为可点击 URI。

---

## 十二、历史记录持久化 (`[history]`)

| 字段 | 说明 | 可选值 | 默认值 |
|------|------|--------|--------|
| `persistence` | 是否保存会话记录 | `"none"`（禁用） | 默认开启 |
| `max_bytes` | 历史文件大小上限 | Integer（如 `104857600`） | — |

达到上限时，Codex 丢弃最旧记录并压缩文件，保留最新记录。

---

## 十三、通知 (`notify`)

支持的事件：`agent-turn-complete`

```toml
notify = ["python3", "/path/to/notify.py"]
```

脚本接收一个 JSON 参数，包含 `type`、`thread-id`、`turn-id`、`cwd`、`input-messages`、`last-assistant-message`。

---

## 十四、TUI 配置 (`[tui]`)

| 字段 | 说明 | 可选值 | 默认值 |
|------|------|--------|--------|
| `notifications` | 启用/禁用或按类型过滤通知 | `true`/`false` 或类型数组 | — |
| `notification_method` | 通知方式 | `"auto"`, `"osc9"`, `"bel"` | `"auto"` |
| `notification_condition` | 通知触发条件 | `"unfocused"`, `"always"` | — |
| `animations` | ASCII 动画/闪烁效果 | `true`/`false` | — |
| `alternate_screen` | 是否使用 alternate screen | `"never"`（保留 scrollback） | — |
| `show_tooltips` | 欢迎界面的提示 | `true`/`false` | — |

> `auto` 模式下，Codex 优先使用 OSC 9 通知，回退到 BEL (`\x07`)。

---

## 十五、可观测性与遥测 (`[otel]`)

默认关闭。

| 字段 | 说明 | 可选值 | 默认值 |
|------|------|--------|--------|
| `environment` | 环境标签 | String | `"dev"` |
| `exporter` | 导出目标 | `"none"`, `{ otlp-http = { ... } }`, `{ otlp-grpc = { ... } }` | `"none"` |
| `log_user_prompt` | 是否在日志中包含用户提示 | `true`/`false` | `false` |

### OTLP-HTTP

```toml
[otel]
exporter = { otlp-http = {
  endpoint = "https://otel.example.com/v1/logs",
  protocol = "binary",
  headers = { "x-otlp-api-key" = "${OTLP_TOKEN}" }
}}
```

### OTLP-gRPC

```toml
[otel]
exporter = { otlp-grpc = {
  endpoint = "https://otel.example.com:4317",
  headers = { "x-otlp-meta" = "abc123" }
}}
```

> `exporter = "none"` 时，Codex 记录事件但不发送。

---

## 十六、匿名用量统计 (`[analytics]`)

默认定期发送，不包含 PII。

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `[analytics].enabled` | 是否收集指标 | `true`/`false` |

与 OTel 日志/追踪导出相互独立。

---

## 十七、反馈控制 (`[feedback]`)

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `[feedback].enabled` | 启用/禁用 `/feedback` 提交 | `true`/`false` |

禁用后 Codex 拒绝反馈提交。

---

## 十八、推理可见性

| 字段 | 说明 | 可选值 |
|------|------|--------|
| `hide_agent_reasoning` | 隐藏推理输出（如 CI 场景） | `true`/`false` |
| `show_raw_agent_reasoning` | 显示原始推理内容 | `true`/`false` |

---

## 十九、Agent Roles (`[agents]`)

参考 [Subagents](/codex/subagents) 文档获取详情。

---

## 二十、MCP Servers

参考 [MCP 文档](/codex/mcp) 获取详情。

---

## 完整配置示例

```toml
# ~/.codex/config.toml

# 默认 profile
profile = "default"

[profiles.default]
model = "gpt-5-pro"
model_reasoning_effort = "medium"

[profiles.fast]
model = "gpt-5-mini"
model_reasoning_effort = "low"

# 审批策略
approval_policy = "on-request"

# 沙箱模式
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
network_access = true

# Shell 环境策略
[shell_environment_policy]
inherit = "core"
exclude = ["AWS_*", "AZURE_*"]

# 项目根检测
project_root_markers = [".git", ".hg"]

# 文件打开方式
file_opener = "vscode"

# TUI 配置
[tui]
notifications = true
animation = true

# 历史记录
[history]
persistence = "none"
```
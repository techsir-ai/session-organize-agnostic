---
name: organize
description: 会话整理规则引擎。拉取云端 RULES.md，按「知识/记录」双模式把会话无损整理为独立 md 文件（命名 YYMMDD-话题.N.md）。跨 agent 通用，各框架适配见正文段落。手动触发。
output_dir:
  target: <OUTPUT_DIR>
  fallback: ~/.config/opencode/sessions/
---

# Session Organize / 会话整理

## 第一步：确定输出目录（只读，安装时已配置）
- 读取本文件 frontmatter 的 output_dir.target。
- 若仍是占位符 <OUTPUT_DIR>：禁止猜测路径、禁止落盘，立即停止并告知用户先按 INSTALL.md 完成安装配置，再重新运行。
- 若已是真实路径：后续所有落盘写入该目录。运行中不得询问、不得更改。

## 第二步：规则源（直接用云端最新版，必做）
- 直接读取云端 RULES.md 最新版作为唯一规则来源（不缓存、不比对版本），按序尝试以下地址，成功即用：
  1. https://raw.githubusercontent.com/techsir-ai/session-organize-agnostic/main/RULES.md
  2. https://cdn.jsdelivr.net/gh/techsir-ai/session-organize-agnostic@main/RULES.md
  3. https://ghproxy.com/https://raw.githubusercontent.com/techsir-ai/session-organize-agnostic/main/RULES.md
- 全部失败才终止并报告，不执行。
- 本地部署可选：把 RULES.md 拷到本地后，将上方规则源改为本地 file:// 路径。
- 后续动作严格遵循 RULES 条款编号。

## 第三步：读取会话（按适配段落定位）
- 定位下方「适配段落」中你当前所在框架对应段，按其会话源读取。
- 定位不到对应框架 → 终止并报告，不自行推断会话存储。

整理执行严格遵循第二步拉取的 RULES.md 第 3-7 章，本文件不复述。会话源、输出目录见下文适配段落。

---

## 适配段落 / Adapters

> 各框架适配由维护者手工维护。部署方仅定位自身框架段，勿自行推断会话存储。
> 未适配框架以「待补」占位。

### opencode（已适配 / adapted）
- 会话源：`opencode export <sessionID>`，或查 ~/.local/share/opencode/opencode.db（part 表含完整消息）。
- 保底目录：~/.config/opencode/sessions/
- 若两种方式均无法定位该会话（export 报错 / DB 无此 session_id / 查询为空）：
  立即终止并报告，不产出任何文件，不臆测会话内容。

### workbuddy（已适配 / adapted）
- 会话源：`~/.workbuddy/projects/<工作区>/<sessionID>.jsonl`，每行一个 JSON 事件；用 Glob 在 `~/.workbuddy/projects/**/<sessionID>.jsonl` 定位。
- `message` 事件含 `role`(user/assistant) 与 `content`（逐字正文）；`ai-title` 事件给出会话标题。
- 保底目录：~/.workbuddy/sessions/（WorkBuddy 部署时请将本文件 frontmatter 的 output_dir.fallback 改为此值）。
- 若 Glob 无匹配（找不到该 sessionID 的 jsonl 文件）、文件无法读取或无可提炼的 message 事件：
  立即终止并报告，不产出任何文件，不臆测会话内容。

### hermes（已适配 / adapted）
- 会话源：`hermes sessions export --session-id <sessionID> --format jsonl`（每行一个完整会话 JSON，messages 数组含 role/content/tool_calls/active/compacted 等字段），或 sqlite3 只读查询 `~/.hermes/state.db`（sessions 表 1 行 + messages 表 N 行；用 `file:...?mode=ro` 避免锁库）。
- 会话压缩说明：Hermes 压缩只影响送 LLM 的上下文，不删物理存储——被压缩轮次在 state.db 中标记 `active=0, compacted=1` 软归档，仍可搜索、可恢复；整理时全量读取（含 active=0 消息），不按 active 过滤。
- 保底目录：~/.hermes/sessions/（Hermes 部署时请将本文件 frontmatter 的 output_dir.fallback 改为此值）。
- 若 export 报错 / DB 无此 session_id / 查询为空：
  立即终止并报告，不产出任何文件，不臆测会话内容。

### openclaw（已适配 / adapted）
- 会话源：`~/.openclaw/agents/<agentId>/sessions/<sessionId>.jsonl`，其中 `<agentId>` 为当前运行的 agent 名称（可从环境或配置获取，如主会话通常是 main），`<sessionId>` 为要整理的会话唯一标识。
- 每行一个独立的 JSON 对象，含 role、content、toolCalls、toolResult、createdAt 等字段（具体以实际结构为准）；压缩仅影响送 LLM 的上下文，不改写物理存储——被压缩轮次在 JSONL 中仅追加摘要条目或插入一行，原始行完整保留。
- 保底目录：~/.openclaw/agents/<agentId>/sessions/（OpenClaw 部署时请将本文件 frontmatter 的 output_dir.fallback 改为此值）。
- 若文件不存在、无法读取或无法解析出有效的 role/content 成对内容：
  立即终止并报告，不产出任何文件，不臆测会话内容。

### dsh（已适配 / adapted）
- 会话源：`~/.dsh/sessions/<projectKey>/session-<sessionID>/session.jsonl`，用 Glob 在 `~/.dsh/sessions/**/session-<sessionID>/session.jsonl*` 定位。
- projectKey 转义规则：`--` 包裹；路径内 `/`、`\`、`:` 统一替换为单个 `-`（连续分隔符合并）；字母数字与 `._-` 保留；其余码位 → `~XXXX`（大写十六进制 4 位）。例：`/Users/changwei/Notes/AIWIKI` → `--Users-changwei-Notes-AIWIKI--`。
- 压缩说明：dsh 默认以 zstd 压缩落盘为 `session.jsonl.zstd`（Node `node:zlib` 原生，内存解压，不产生临时文件）。读取时在内存/管道中解压（如 `zstd -d` 或代码 zstd 解码），单条会话为多帧拼接容器，按帧依次解码。若部署时已将 dsh 持久化设为 `compression=none`，则直接逐行读 `.jsonl`。
- 保底目录：~/.dsh/sessionexport/（dsh 部署时请将本文件 frontmatter 的 output_dir.fallback 改为此值；注意 dsh 的默认会话目录 ~/.dsh/sessions/ 是 dsh 自身会话存储，勿作输出目录）。
- 若 Glob 无匹配（找不到该 sessionID 的文件）、文件无法读取、解压失败或无有效 role/content 成对内容：
  立即终止并报告，不产出任何文件，不臆测会话内容。

---

## 注意
- 本文件为一份通用 skill，正文分框架适配段。output_dir.target 为占位符，安装时由 INSTALL 流程替换为真实路径。
- 各框架适配段待实测后由维护者补齐，部署方不自行推断。

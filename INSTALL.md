# INSTALL / 安装指引

> 关联文件：RULES.md（整理规则，唯一事实源）| subagent/（子 agent 模板）| skills/（skill 模板）

## 0. 模块化说明 / Modular Note

- This file is organized by "framework x carrier". Each module is independent and can be trimmed.
- 本文件按「框架 × 承载方式」组织，每个模块独立、可裁剪。
- When installing, read only your framework section and your carrier section; ignore the rest.
- 安装时只读自己的框架段落 + 对应承载方式段落，其余忽略。
- Frameworks/carriers not yet adapted are reserved as "TODO" placeholders with structure ready.
- 尚未适配的框架/承载方式以「待补」占位，结构已预留。

---

## 1. 通用安装步骤 / Common Install Steps

> Shared by all modules. / 所有模块共用。

- 1) Ask the user for the output directory (leave empty to use the fallback).
- 询问用户输出目录（留空用保底）。
- 2) Copy the corresponding template to this framework's agents directory / skills directory.
- 拷贝对应模板 → 本框架 agents 目录 / skills 目录。
- 3) Write the user directory into the frontmatter `output_dir.target`.
- 把用户目录写入 frontmatter 的 `output_dir.target`。
- 4) Sync the `external_directory` permission line (replace the `<OUTPUT_DIR>` placeholder).
- 同步 `external_directory` 权限行（把 `<OUTPUT_DIR>` 占位符替换为真实路径）。
- 5) Verify the frontmatter (read it back to confirm the placeholder is gone).
- 校验 frontmatter（读回确认占位符已替换）。

> Permission note: installing means writing to the agent directory, which is done in the main agent with write permission.
> 权限说明：安装即向 agent 目录写文件，由具有写权限的主 agent 完成，因此询问与写入只在安装时发生一次。

---

## 2. 框架：opencode / Framework: opencode

### 2.1 subagent 安装 / subagent Install

- Template / 模板：`subagent/opencode/organize.md`
- Deploy to / 部署到：`~/.config/opencode/agents/organize.md`
- Session source / 会话源：`opencode export <sessionID>`，或 `~/.local/share/opencode/opencode.db`（part 表）
- Fallback dir / 保底目录：`~/.config/opencode/sessions/`

### 2.2 skill 安装 / skill Install

- Template / 模板：`skills/organize/SKILL.md`
- Deploy to / 部署到：`~/.config/opencode/skills/organize/`
- Session source / 会话源：同 2.1（见 SKILL.md 正文 opencode 适配段）
- Fallback dir / 保底目录：`~/.config/opencode/sessions/`

---

## 3. 框架：workbuddy / Framework: workbuddy

### 3.1 subagent 安装 / subagent Install

- Template / 模板：`subagent/workbuddy/organize.md`
- Deploy to / 部署到：`~/.workbuddy/agents/organize.md`（WorkBuddy 系统级子 agent 目录，与 `distill.md` 同目录）
- Session source / 会话源：`~/.workbuddy/projects/**/<sessionID>.jsonl`（Glob 定位；`message` 事件含 `role`/`content` 逐字正文，`ai-title` 事件含标题）——已写死在模板第三步
- Fallback dir / 保底目录：`~/.workbuddy/sessions/`
- Permission note / 权限说明：WorkBuddy 子 agent 用 `tools` 白名单控权（frontmatter `tools: [Read, Glob, Grep, Bash, Write]`），**无 opencode 式 `external_directory` 块** → 通用步骤第 4 步（同步 external_directory）**跳过**；仅需在 frontmatter 填 `output_dir.target`。
- Usage / 使用：部署后**新开一个会话**即可调用 `organize` 子 agent（WorkBuddy 动态识别 `~/.workbuddy/agents/*.md`，无需硬重启 App）。

### 3.2 skill 安装 / skill Install

- Template / 模板：`skills/organize/SKILL.md`（通用 skill，workbuddy 适配段已补全）
- Deploy to / 部署到：`~/.workbuddy/skills/organize/SKILL.md`（WorkBuddy 用户级 skill 目录）
- Session source / 会话源：同 3.1（见 SKILL.md 正文 workbuddy 适配段）
- Fallback dir / 保底目录：`~/.workbuddy/sessions/`
- Note / 说明：WorkBuddy 下 organize 以「子 agent」为主承载（3.1）；skill 为可选承载，用于手动 `@` 触发或作上下文参考。若部署，注意 WorkBuddy 的 SKILL.md frontmatter 与 opencode 不同，无需 external_directory。

---

## 4. 框架：hermes / Framework: hermes

### 4.1 subagent 安装 / subagent Install

- 不提供 / Not provided
- 原因 / Reason：Hermes 无静态 `agents/*.md` 子 agent 定义机制（无 `~/.hermes/agents/` 目录），其子代理 `delegate_task` 为运行时动态创建、继承父 agent 工具集，无法独立配置权限；固定指令若经上下文注入（AGENTS.md 等）常驻，token 成本高。故 hermes 仅支持 skill 承载（见 4.2），不提供子 agent 模板。

### 4.2 skill 安装 / skill Install

- Template / 模板：`skills/organize/SKILL.md`（通用 skill，hermes 适配段已补全）
- Deploy to / 部署到：`~/.hermes/skills/organize/SKILL.md`（Hermes 用户级 skill 目录）
- Session source / 会话源：`hermes sessions export --session-id <sessionID> --format jsonl`，或 sqlite3 只读查询 `~/.hermes/state.db`（sessions 表 + messages 表）——详见 SKILL.md 正文 hermes 适配段
- Fallback dir / 保底目录：`~/.hermes/sessions/`
- Note / 说明：Hermes 压缩（context compression）只改上下文不删物理存储，被压缩轮次标记 `active=0, compacted=1` 软归档，整理时全量读取。

---

## 5. 框架：openclaw / Framework: openclaw

### 5.1 subagent 安装 / subagent Install

- 待补 / TODO

### 5.2 skill 安装 / skill Install

- 待补 / TODO

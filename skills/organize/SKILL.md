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

## 第四步：执行整理（RULES 第 3 章流水线）
- 3.1 分话题：多话题 → 拆分多文件，一话题一文件。
- 3.2 先查：列出输出目录所有 md，读 frontmatter（agent、session_id、话题、摘要）：
  · 同 agent + 同 session_id + 同话题 → 生成新版本（可改标题 + .N 序号），旧文件保留待人工清理
  · 无匹配 → 新建。
- 3.3 去重 → 3.4 去无效（两层）→ 3.5 剔闲聊 → 3.6 排序 → 3.7 摘要 → 3.8 落盘。
- 按内容选模式：知识（RULES 第 4 章）/ 记录（RULES 第 5 章）；开发场景含阶段标记与决策讨论（5.5-5.7）。

## 第五步：命名与 frontmatter（RULES 第 7 章）
- 文件名 {YYMMDD}-{话题}.md；多轮整理加 .N 序号。
- 产出文件 frontmatter 完整字段：agent/model/session_id/mode/host_os/hostname/ip(LAN)/source/summary/prev/created。
- host_os/hostname/ip 运行时探测（uname / hostname / ifconfig 或 ip）。
- 落盘写入 output_dir.target 所指目录；若目标目录不可写，改用 output_dir.fallback 并报告用户实际落盘路径。

## 避坑（RULES 第 6 章）
- 易复现错误写入避坑章节；记录模式标注发生步骤位。

---

## 适配段落 / Adapters

> 各框架适配由维护者手工维护。部署方仅定位自身框架段，勿自行推断会话存储。
> 未适配框架以「待补」占位。

### opencode（已适配 / adapted）
- 会话源：`opencode export <sessionID>`，或查 ~/.local/share/opencode/opencode.db（part 表含完整消息）。
- 保底目录：~/.config/opencode/sessions/

### workbuddy（已适配 / adapted）
- 会话源：`~/.workbuddy/projects/<工作区>/<sessionID>.jsonl`，每行一个 JSON 事件；用 Glob 在 `~/.workbuddy/projects/**/<sessionID>.jsonl` 定位。
- `message` 事件含 `role`(user/assistant) 与 `content`（逐字正文）；`ai-title` 事件给出会话标题。
- 保底目录：~/.workbuddy/sessions/（WorkBuddy 部署时请将本文件 frontmatter 的 output_dir.fallback 改为此值）。

### hermes（待补 / TODO）

### openclaw（待补 / TODO）

---

## 注意
- 本文件为一份通用 skill，正文分框架适配段。output_dir.target 为占位符，安装时由 INSTALL 流程替换为真实路径。
- 各框架适配段待实测后由维护者补齐，部署方不自行推断。

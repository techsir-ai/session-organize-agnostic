# Session Organize Rules / 会话整理规则

> Version: 260811001 / 版本: 260811001
> Updated: 2026-08-11 / 更新: 2026-08-11
> Changelog: Initial version; 260811001 value-preserving + timeline definition + mixed mode / 变更: 初始版本；260811001 价值无损 + 时间线定义 + mixed 模式

---

## 1. Goals and Principles / 目标与原则

1.1 Value-preserving organizing, not summarization, condensation, or distillation.
价值无损整理，不是摘要、浓缩或蒸馏。
Value-preserving means: extract value from the session and keep it lossless.
价值无损指：从会话中提取价值，围绕价值做无损整理。

1.2 The goal is archiving and cross-agent reuse, not saving context or compressing conversations.
目标是存档与跨 agent 复用，不是节省上下文或压缩会话。

1.3 No valuable information is deleted; it is only reorganized, deduplicated, or relocated.
有价值的信息不删除，只做重组、去重与迁移归类。

1.4 Repeated information is merged; error handling follows 3.4.
重复信息合并；错误处理遵循 3.4。

1.5 Content irrelevant to the extracted value is dropped.
与所提取价值无关的讨论过程剔除。

---

## 2. Two Modes / 两种模式

2.1 Knowledge Mode: for learning and discussing a topic; tracks the evolution of concepts and conclusions.
知识模式：用于学习、讨论某个问题；记录概念与结论的逐渐演化。

2.2 Record Mode: for attempts, configurations, questions, and discussions with operations; tracks strict order of steps, commands, and file changes.
记录模式：用于有尝试、配置方法、提问与讨论的会话；记录步骤、命令、文件改动的严格顺序。

| Dimension / 维度 | Knowledge / 知识模式 | Record / 记录模式 |
|---|---|---|
| Scenario / 适用场景 | Learning, discussion / 学习、讨论 | Attempts, setup, dev, Q&A with ops / 尝试、配置、开发、操作 |
| Timeline / 时间线 | Formation of understanding (concept evolution), not discussion order / 认知形成脉络（概念演化），非讨论顺序 | Logical operation chain (correct path), not actual execution order / 操作逻辑链（正确路径），非实际执行顺序 |
| Order strictness / 顺序严格度 | Medium, reconstructable / 中，可重构 | High, must be reproducible / 高，必须可复现 |
| Non-removable / 不可剔除 | Concepts, conclusions, causal chain / 概念、结论、演化因果 | Steps, commands, file changes, decision discussions, each one / 步骤、命令、文件改动、决策讨论，一条都不能少 |
| Removable / 可剔除 | Discussion process irrelevant to the extracted value / 与价值无关的讨论过程 | Discussion process irrelevant to the extracted value / 与价值无关的讨论过程 |
| Errors / 错误处理 | Misconceptions to Pitfalls, not on the evolution chain / 误区入避坑，不占演化主链 | Failed operations to Pitfalls, marked with step position on the correct timeline / 失败操作入避坑，标注正确时间线上的步骤位 |

2.3 A topic may contain both knowledge and record content; its mode is then mixed. Knowledge paragraphs follow Chapter 4, record paragraphs follow Chapter 5, grouped by content type, not interleaved.
一个话题可同时包含知识内容与记录内容，此时其模式为 mixed；知识段落遵循第 4 章规范，记录段落遵循第 5 章规范，按内容性质分组，不交错。

---

## 3. General Pipeline / 通用处理流水线

3.1 Split topics: divide content by topic, one file per topic; the topic boundary (including whether knowledge and record content belong to the same topic) is judged by the organizing agent.
分话题：按内容划分话题，一话题一文件；话题边界（含知识内容与记录内容是否同属一个话题）由整理 agent 自行判断。

3.2 Check first: before creating, query existing files.
先查：创建前先查询已有文件。

3.2.1 List files in the target directory, read their frontmatter (agent, session_id, topic, summary).
列出目标目录文件，读取 frontmatter（agent、session_id、话题、摘要）。

3.2.2 Same agent + same session_id + same topic -> produce a new version file (with optional new title + sequence number); the old file is kept for manual cleanup. The latest version is the authoritative one.
相同 agent + 相同 session_id + 相同话题 -> 生成新版本文件（可改标题+序号）；旧文件保留待人工清理；最新版本为权威。

3.2.3 No match -> create a new file.
无匹配 -> 新建文件。

3.2.4 Cross-session / cross-agent merging belongs to the aggregation layer, not this pipeline.
跨会话、跨 agent 的归并属于聚合层，不在此流程内。

3.3 Deduplicate: merge repeated content whose information value is already one.
去重：合并信息量本就为 1 的重复内容。

3.4 Remove invalid content, two levels:
去无效，两层含义：
- Drop simple, non-reproducible errors such as typos.
  剔除简单、不易重复出现的错误，如拼写错误。
- Move tried-but-ineffective, reproducible failures to the Pitfalls section.
  尝试过但无效、对最终结果无效、且容易复现的错误移入避坑章节。

3.5 Drop discussion process irrelevant to the extracted value.
剔除与所提取价值无关的讨论过程。

3.6 Sort content according to the timeline semantics of the mode.
按当前模式的时间线语义排序。

3.7 Write a summary in the frontmatter: a single paragraph, no length limit; it is the basis for distinction and future aggregation.
在 frontmatter 写摘要：单个段落，不限长度；作为区分与未来聚合的依据。

3.8 Write the file with naming `{YYMMDD}-{topic}.md` and full frontmatter.
按命名 `{YYMMDD}-{topic}.md` 及完整 frontmatter 落盘。

3.9 If the session already produced artifacts, reference them instead of copying the full content.
如果会话中已有产出物，以引用代替全文抄写。

Applies to all outputs / 适用于所有产出形态：
- Knowledge documents: reference existing docs or conclusions instead of re-writing.
  知识文档：已有文档/结论 -> 引用，不重写。
- Record documents: plan, spec, design docs, config files -> reference.
  记录文档：plan、spec、设计文档、配置文件 -> 引用。
- Code: session-produced code files -> reference by path, do not paste full source.
  代码：会话中产生的代码文件 -> 引用文件路径，不粘贴完整源码。

---

## 4. Knowledge Mode / 知识模式规范

4.1 Target: organize a topic's conceptual evolution and conclusions into one readable asset.
目标：把话题的概念演化与结论整理为一份可读资产。

4.2 The timeline follows the formation of understanding: from initial understanding to final conclusions, ordered by concept evolution; even if the discussion jumped around, the result is ordered by the conceptual chain, not by discussion order.
时间线按认知形成脉络组织：从初始认知到最终结论，按概念演化排序；即使讨论跳序进行，整理结果仍按脉络排序，而非讨论顺序。

4.3 Template fields:
模板字段：
- Topic and conclusion / 主题与结论
- Evolution chain of understanding / 认知演化链
- Key conclusions / 关键结论
- Pitfalls and misconceptions / 避坑与误区
- Related entries / 相关条目

4.4 Misconceptions and reproducible errors go to the Pitfalls section, not the main chain.
误区与易复现错误放入避坑章节，不占演化主链。

---

## 5. Record Mode / 记录模式规范

5.1 Target: make the operation reproducible; the timeline follows the logical operation chain.
目标：保证操作可复现；时间线按操作逻辑链组织。

5.2 Steps, commands, and file changes must NOT be removed, and their timeline follows the logical operation chain (the correct order), not the actual execution order. If an error is found mid-task and work is redone from an earlier step (e.g. reached step 5, found an error, went back to step 3 and redid it), the main timeline keeps only the correct final logical order (1→2→3′→4′→5); failed segments → see 5.8. The same applies to code tasks: order by the correct completion logic.
步骤、命令、文件改动不可剔除；其时间线按操作逻辑链（正确操作顺序）组织，而非实际执行顺序。若执行中出错回退重做（如做到第 5 步发现错误、退回第 3 步重做），主时间线只记录正确的最终逻辑顺序（1→2→3′→4′→5）；失败段处理 → 见 5.8。代码任务同理：按正确的任务完成逻辑排序。

5.3 Each step records its result or verification.
每一步记录其结果或验证方式。

5.4 Template fields:
模板字段：
- Environment and prerequisites / 环境与前提
- Step sequence, in strict order: commands, configs, file changes, each with result / 步骤序列，严格时序：命令、配置、文件改动，每步含结果
- Reproduction check / 复现检查
- Pitfalls and error-prone points, marked with the step position / 避坑与易错点，标注步骤位

5.5 In development scenarios (plan/build/fix switching), the mode remains record; mark the phase in each step.
开发场景（plan/build/fix 切换）仍归记录模式；每一步标注所处阶段。

5.6 Decision discussions that affect subsequent steps are kept; discussion irrelevant to the extracted value is dropped.
影响后续步骤的决策讨论必须保留；与所提取价值无关的讨论过程剔除。

5.7 Artifact referencing follows rule 3.9.
产出物引用遵循 3.9。

5.8 Failed operations move to Pitfalls with the step position where they occurred; the main sequence keeps only the working path.
失败操作移入避坑并标注发生步骤；主序列只保留有效路径。

---

## 6. Pitfalls and Error-Prone Points / 避坑与易错章节

6.1 Shared by both modes; collects reproducible errors that are invalid to the final result.
两种模式共用；收集对最终结果无效但容易复现的错误。

6.2 Each entry records: the error, its cause, the fix, and (in Record Mode) the step position on the correct timeline.
每条记录：错误、原因、解决办法，以及（记录模式下）其在正确时间线上的步骤位。

---

## 7. Naming and Frontmatter / 命名与元数据

7.1 File name: `{YYMMDD}-{topic}.md`; the timestamp is the first-run date and never changes.
文件名：`{YYMMDD}-{topic}.md`；时间戳为首次整理日期，永不改变。

7.1.1 Same session organized again (2nd, 3rd time) -> append a sequence number at the end of the file name. Renaming the title is independent of the sequence number; both can happen at once.
同一会话再次整理（第 2、3 次）-> 在文件名末尾追加序号；改标题与加序号互不相关，可同时发生。
```
260807-xxx.md        (1st run / 第一次)
260807-xxx.2.md      (2nd run / 第二次, same or new title / 同名或新标题)
260807-xxx.3.md      (3rd run / 第三次)
```

7.2 Frontmatter:
frontmatter 字段：
```
---
agent: <agent name>          # Which agent / 哪个 agent
model: <model name>          # Which model / 哪个模型
session_id: <session id>     # Corresponding session / 对应会话 id
mode: knowledge | record | mixed   # Mode: knowledge/record/mixed / 模式：知识/记录/混合
host_os: macOS | linux | windows   # Current machine OS / 当前机器系统
hostname: <hostname>         # Current machine hostname / 当前机器主机名
ip: <LAN IP>                 # LAN IP to distinguish machines; not localhost, not VPN tunnel IP / 局域网 IP，用于区分机器；非 local、非 VPN 隧道 IP
source: <path or URL>        # Source file of this session, physical path or cloud URL; agent-specific / 源文件路径或云端 URL，因 agent 而异
summary: <one paragraph>     # Summary, basis for aggregation / 摘要，聚合依据
prev: <previous version file name>   # Previous version of this file; empty for the first run / 上一版本文件名；首次整理为空
created: YYMMDD              # Created date / 创建日期
---
```

7.3 The agent, model, session id, and host info are recorded only in the frontmatter, not in the file name.
agent、模型、会话 id 与机器信息只写入 frontmatter，不进文件名。

7.4 The version chain is expressed via `prev` in the frontmatter; `summary` carries the semantic evolution.
版本链通过 frontmatter 的 `prev` 表达；摘要承担语义演变。

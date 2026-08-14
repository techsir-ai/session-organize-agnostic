# session-organize-agnostic

Break the lock-in of any single agent over your conversation history.
打破任何单一 agent 对你会话历史的锁定。

Classify and organize any agent's conversations into Knowledge and Record assets,
persist them to the filesystem as permanent personal digital assets, reusable across agents.
对 agent 产生的会话按知识/记录分类整理，落盘到文件系统，作为个人数字资产永久保存，
并可在不同 agent 中继承复用。

Lossless organizing: no distillation, no extraction, no compression.
无损整理：非蒸馏、非提炼、非压缩。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-260807001-blue.svg)](RULES.md)

## Features / 特点
- Break single-agent lock-in / 打破单一 agent 锁定
- Lossless organizing: no distillation, no extraction, no compression / 无损整理：非蒸馏、非提炼、非压缩
- Two asset types: Knowledge (conclusions only), Record (full process + result) / 两类资产：知识（只记结论）、记录（过程+结果）
- Permanent persistence on the filesystem / 在文件系统上永久保存
- Inherited and reused across different agents / 可在不同 agent 中继承使用
- One rule source (RULES.md), dual carriers: subagent + skill / 单一规则源，子 agent 与 skill 双承载

## Supported Platforms / 已适配平台

> 单一规则源（RULES.md），各平台按自身机制承载。skill 与 MCP 属跨 agent 工业标准，各平台原生支持；子 agent（subagent / agent-preset）各平台机制不同，视其是否有静态定义能力而定。

| Platform / 平台 | skill | 子 agent（subagent） | 子 agent 情况 / Reason |
|-----------------|:-----:|:---------------------:|------------------------|
| opencode | ✅ | ✅ | 静态 `agents/*.md` 定义（`subagent/opencode/organize.md`） |
| workbuddy | ✅ | ✅ | 静态 `agents/*.md` 定义（`subagent/workbuddy/organize.md`），skill 为可选承载 |
| hermes | ✅ | ❌ | 无静态子 agent 机制（运行时动态创建、无法独立配置权限），仅 skill |
| openclaw | ✅ | ❌ | 无静态子 agent 机制（运行时动态创建、无法独立配置权限），仅 skill |
| dsh | ✅ | ❌ | 用 agent-preset（`agent.cordis.yml`，插件行清单、不承载行为正文），行为靠 skill，故仅 skill（见 INSTALL 6.1） |

## Cross-Agent Continuation / 跨 agent 无痕续做

A project started in one agent can be seamlessly continued in another.
在一个 agent 中开始的项目，可在另一个 agent 中无痕继续。

How it works / 机制：
- Organize the session into a Record asset with full process, order, and decisions.
  会话整理为记录版资产：完整过程、严格顺序、关键决策全部保留。
- The frontmatter carries session_id, source, agent, summary, and prev, so the next agent knows which session, how far it went, and what to do next.
  frontmatter 携带 session_id、source、agent、summary、prev，下一个 agent 由此得知是哪个会话、进行到哪一步、下一步做什么。
- Artifacts (plans, specs, code) are referenced, not duplicated, so work continues in place.
  产出物（plan、spec、代码）只引用不重复，直接在原位置续接。
- Pitfalls carry past failures, so the next agent avoids repeating them.
  避坑章节记录失败路径，下一个 agent 直接绕开。

Example: develop halfway in opencode, organize, then continue seamlessly in workbuddy.
示例：在 opencode 中开发到一半，整理落盘，再到 workbuddy 中无痕续做。

## Quick Start / 快速开始
→ [INSTALL.md](INSTALL.md)（模块化安装：各框架 subagent / skill）

## How it works / 工作原理
→ [RULES.md](RULES.md)：整理规则（唯一事实源）

## Structure / 目录结构
RULES.md · INSTALL.md · subagent/ · skills/

## License / 协议
MIT License — Copyright (c) 2026 techsir-ai

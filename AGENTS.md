# AGENTS.md

This file provides context for AI coding assistants (Claude Code, Cursor, GitHub Copilot, Codex, etc.) working with this repository.

## 项目简介

该项目是一个菜单管理系统。

## 文档约定

- `PLAN.md`：记录项目当前方向、阶段目标和下一步重点
- `TASKS.md`：记录当前待办、进行中、阻塞和已完成事项
- `docs/prd/README.md`：维护持续演进的产品需求文档
- `docs/tdd/README.md`：维护持续演进的技术设计文档

这些文档都是项目级入口，不按需求单独复制一套。内容变多时可以拆分子文档，但 `README.md` 继续做索引。

- 技术栈和工具选型不预先写死在 `AGENTS.md`，默认在 TDD 阶段决定，并维护在 `docs/tdd/` 文档中。

## 轻量开发流程

本项目采用单人开发的轻量流程，目标是减少流程负担，同时保留最基本的可追踪性。

- 开发顺序默认遵循：`PRD -> TDD -> TASKS -> 开发 -> 测试`

### 1. 开始前

- 先看 `PLAN.md` 和 `TASKS.md`，确认当前阶段和手头优先事项
- 如果是新功能或较大改动，先在 PRD/TDD 里补最小必要说明，再开始写代码
- 编写 PRD 时，默认参考 `docs/prd/checklist.md` 中的 PRD 参考清单，按需取舍
- 编写 TDD 时，默认参考 `docs/tdd/checklist.md` 中的 TDD 参考清单，按需取舍
- 如果只是小修复或小调整，可以直接改，但改完后要同步更新必要文档

### 2. 开发时

- 优先小步推进，不一次改太多
- 先明确接口、数据结构和关键规则，再写实现
- 如果涉及数据库 schema 变更，必须先在 TDD 中检查迁移、兼容、回滚、索引和性能影响
- 如果改动会影响需求、设计或任务状态，要顺手更新文档
- 不为了“顺手”扩大范围，避免把简单任务做重

### 3. 测试

- 重要功能改动后至少做对应验证
- 页面验证、关键交互测试方式以 TDD 约定为准
- 端到端测试方式以 TDD 约定为准
- 如果这次没法完整测试，需要在回复里说明未测部分

### 4. 完成后

- 更新 `TASKS.md` 状态
- 如果阶段目标或重点变化，更新 `PLAN.md`
- 如果实现改变了需求或设计结论，更新 PRD/TDD

## AI 助手要求

- 开始工作前先读 `AGENTS.md`
- 优先沿用现有文档体系，不默认新建一堆独立文档
- 如果缺需求或缺设计，先补最小骨架，再继续实现
- 回答时尽量说明这次改动影响了哪些文档、任务和测试

## 提交约定

Git 提交优先使用 Conventional Commit 风格，建议使用：

- `feat:`
- `fix:`
- `docs:`
- `refactor:`

```bash
# <emoji> <type>: 提交信息使用中文即可
✨ feat: 添加什么功能
🐛 fix: 修复了什么问题
```

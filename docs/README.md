# 文档索引

本目录按使用场景组织项目文档。根目录入口负责项目状态和代理协作规则，专题目录负责需求、设计、开发和运维细节。

## 文档地图

- [开发文档](./development/README.md)：本地开发、验证命令、代码变更和测试约定
- [运维文档](./ops/README.md)：部署、发布、监控、排障和数据安全
- [产品需求](./prd/README.md)：统一菜单中心 PRD、模块需求和产品决策
- [技术设计](./tdd/README.md)：架构、接口、数据模型和测试设计
- [设计原型](./design/menu-console-v1/README.md)：菜单管理控制台 UI 原型

## 根目录入口

- [README.md](../README.md)：项目简介和快速开始
- [PLAN.md](../PLAN.md)：当前阶段和近期重点
- [ROADMAP.md](../ROADMAP.md)：项目阶段路线和阶段交付物
- [AGENTS.md](../AGENTS.md)：AI agent 操作地图

## 维护约定

- 需求变更先更新 PRD；影响接口、数据、技术选型或测试时同步更新 TDD。
- 开发命令和测试约定写入 `development/` 或 TDD，不把长流程复制到 `AGENTS.md`。
- 部署、发布、监控和排障内容写入 `ops/`，并在这里补充索引。

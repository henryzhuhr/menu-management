# PRD README

本目录维护统一菜单中心的产品需求文档。

当前 PRD 采用“入口索引 + 模块文档 + 决策记录”的结构。需求正文按模块拆分，所有关键需求都引用决策编号，便于追踪需求来源。

## 当前索引

### 总览

- [executive-summary.md](./executive-summary.md)
  面向汇报的一页式项目简述
- [overview.md](./overview.md)
  统一菜单中心产品总览
- [glossary.md](./glossary.md)
  术语表

### 模块 PRD

- [modules/runtime-menu.md](./modules/runtime-menu.md)
  运行时菜单查询与前端消费
- [modules/menu-configuration.md](./modules/menu-configuration.md)
  菜单配置与菜单模型
- [modules/tenant-customization.md](./modules/tenant-customization.md)
  租户自定义与用户个性化
- [modules/release-governance.md](./modules/release-governance.md)
  版本、发布、审批、灰度和回滚
- [modules/integrations.md](./modules/integrations.md)
  外部身份、权限和订阅系统集成
- [modules/admin-operations.md](./modules/admin-operations.md)
  管理后台、导入导出、预览和诊断
- [modules/analytics.md](./modules/analytics.md)
  菜单使用分析
- [modules/non-functional.md](./modules/non-functional.md)
  非功能、安全、合规和数据留存

### 决策记录

- [decisions/README.md](./decisions/README.md)
  决策索引

### 模板与清单

- [template.md](./template.md)
  PRD 填写模板
- [checklist.md](./checklist.md)
  PRD 参考清单

## 文档约定

- 需求正文写在模块 PRD 中，避免把入口文档写成过长流水账。
- 关键规则必须引用决策编号，例如 [D025](decisions/003-visibility-rules.md#d025-visibility-precedence)。
- PRD 按模块拆分维护，结构来源于 [D060](decisions/001-product-scope.md#d060-prd-shape)。
- 新增或调整需求时，如果来源于产品讨论，需要同步补充或更新 `decisions/`。
- 如果需求影响实现方案、接口、数据或测试，需要在 TDD 阶段同步到 `docs/tdd/`。

# 非功能、安全、合规和数据留存

## 1. 目标

统一菜单中心是运行时导航核心链路的一部分，需要具备高可用、低延迟、可观测、安全隔离和可审计能力。

决策引用：[D050](../decisions/008-non-functional.md#d050-service-slo)、[D052](../decisions/008-non-functional.md#d052-sla-target)、[D054](../decisions/008-non-functional.md#d054-compliance-scope)

## 2. 容量目标

系统按超大平台量级定义需求目标：

- 千应用
- 万租户
- 多地域
- 多客户端
- 高并发运行时菜单查询

该目标用于指导后续 TDD 的架构设计、缓存策略、数据库设计和性能验证。

决策引用：[D024](../decisions/008-non-functional.md#d024-scale-target)、[D053](../decisions/008-non-functional.md#d053-capacity-target)

## 3. SLA

运行时菜单查询属于核心链路，需要明确 SLA。

目标为：

- 可用性：99.9%
- 性能：P95 200ms

后续 TDD 需要补充具体统计口径、监控方式和压测方案。

决策引用：[D050](../decisions/008-non-functional.md#d050-service-slo)、[D052](../decisions/008-non-functional.md#d052-sla-target)

## 4. 实时性

安全相关变更需要实时影响运行时菜单，包括权限撤销、订阅停用、菜单下线、菜单停用和发布回滚。

决策引用：[D027](../decisions/005-release-governance.md#d027-runtime-freshness)、[D028](../decisions/005-release-governance.md#d028-realtime-scope)

## 5. 安全降级

当外部权限、订阅或安全依赖不可用时，系统需要安全降级。无法确认权限或订阅时，不展示受控菜单。

决策引用：[D029](../decisions/006-integrations.md#d029-dependency-failure)

## 6. 租户隔离和审计

系统必须保证租户数据隔离，避免跨租户读取、配置污染或分析数据泄露。

系统需要完整审计：

- 配置变更
- 审批
- 发布
- 回滚
- 导入导出
- 冲突处理
- 关键诊断操作

决策引用：[D021](../decisions/005-release-governance.md#d021-audit-history)、[D054](../decisions/008-non-functional.md#d054-compliance-scope)

## 7. 数据留存

审计、版本、发布、回滚和分析数据保留期限需要可配置。不同数据类型、租户或地域可以在后续设计中定义不同保留策略。

决策引用：[D051](../decisions/008-non-functional.md#d051-retention-policy)

## 8. 验收标准

- 运行时菜单查询满足 99.9% 可用性和 P95 200ms 目标。
- 系统能支持超大平台量级的容量目标。
- 安全相关变更实时生效。
- 外部依赖异常时安全降级。
- 租户数据隔离有效。
- 审计和数据留存策略可配置。

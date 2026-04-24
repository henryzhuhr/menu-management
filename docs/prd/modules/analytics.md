# 菜单使用分析

## 1. 目标

菜单使用分析用于帮助平台了解菜单曝光、点击、使用趋势和异常情况，并支持运营优化、租户对比、地域对比和问题排查。

决策引用：[D046](../decisions/007-admin-diagnostics-analytics.md#d046-usage-analytics)

## 2. 分析范围

系统需要支持完整分析能力，包括：

- 菜单曝光
- 菜单点击
- 访问趋势
- 租户维度对比
- 地域维度对比
- 客户端维度对比
- 未命中或隐藏原因统计
- 数据导出

决策引用：[D046](../decisions/007-admin-diagnostics-analytics.md#d046-usage-analytics)、[D048](../decisions/007-admin-diagnostics-analytics.md#d048-diagnostics)

## 3. 分析用途

分析结果用于：

- 发现低使用率菜单
- 评估新菜单灰度效果
- 比较不同租户或地域使用差异
- 辅助定位权限或订阅配置问题
- 支持运营和产品优化

决策引用：[D046](../decisions/007-admin-diagnostics-analytics.md#d046-usage-analytics)、[D059](../decisions/001-product-scope.md#d059-acceptance-priority)

## 4. 数据留存

分析数据留存期限需要可配置。系统可以在保留明细数据的同时，对历史数据进行周期聚合，以满足趋势分析和存储成本平衡。

决策引用：[D051](../decisions/008-non-functional.md#d051-retention-policy)

## 5. 验收标准

- 系统可以统计菜单曝光和点击。
- 系统可以按租户、地域、客户端等维度查看趋势。
- 系统可以导出分析数据。
- 分析数据保留策略可配置。
- 分析数据不能泄露其他租户数据。

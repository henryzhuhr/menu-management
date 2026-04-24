# 管理后台、导入导出、预览和诊断

## 1. 目标

管理后台负责支撑统一菜单中心的日常配置、治理、迁移、预览和问题排查。管理能力需要覆盖完整配置闭环，而不是只提供菜单编辑页面。

决策引用：[D013](../decisions/007-admin-diagnostics-analytics.md#d013-admin-modules)、[D033](../decisions/007-admin-diagnostics-analytics.md#d033-configuration-channels)、[D048](../decisions/007-admin-diagnostics-analytics.md#d048-diagnostics)

## 2. 管理配置入口

管理后台需要覆盖：

- 平台菜单模板
- 功能点定义和菜单绑定
- 租户订阅结果查看和配置校验
- 权限标识绑定
- 条件策略
- 租户自定义
- 版本和灰度发布
- 审批流配置
- 发布回滚
- 审计查看
- 分析报表
- 诊断排障

决策引用：[D013](../decisions/007-admin-diagnostics-analytics.md#d013-admin-modules)、[D030](../decisions/006-integrations.md#d030-admin-rbac)

租户订阅主数据源仍是外部订阅系统。管理后台只展示、校验和诊断订阅结果，不负责订阅购买、计费或合同主数据维护。

决策引用：[D055](../decisions/006-integrations.md#d055-subscription-source-of-truth)、[D058](../decisions/001-product-scope.md#d058-out-of-scope)

## 3. 导入导出和跨环境迁移

系统需要支持后台操作和批量导入导出。

导入导出主要用于跨环境迁移，例如从测试环境迁移到生产环境。迁移内容包括：

- 应用菜单
- 功能点
- 条件策略
- 多语言文案
- 客户端展示元数据
- 版本配置

导入前应支持校验，导入后应产生审计记录。

决策引用：[D033](../decisions/007-admin-diagnostics-analytics.md#d033-configuration-channels)、[D047](../decisions/007-admin-diagnostics-analytics.md#d047-config-migration)

## 4. 多维预览

管理员需要在发布前按多个维度预览最终菜单结果：

- 应用
- 环境
- 地域
- 客户端
- 租户
- 用户
- 语言

预览结果应展示最终菜单树，并能辅助判断发布影响。

决策引用：[D032](../decisions/007-admin-diagnostics-analytics.md#d032-preview-requirements)

## 5. 完整诊断

系统需要提供完整诊断能力。管理员可以输入上下文，查看某菜单为什么显示或隐藏。

诊断需要覆盖：

- 命中的版本和发布范围
- 租户订阅
- 用户权限
- 条件命中结果
- 租户自定义
- 用户个性化
- 强制菜单约束
- 依赖调用状态
- 异常和告警

决策引用：[D048](../decisions/007-admin-diagnostics-analytics.md#d048-diagnostics)

## 6. 管理后台权限

管理后台自身不使用固定角色硬编码。所有管理操作需要可绑定外部权限标识，并由外部权限系统决定用户是否具备操作能力。

决策引用：[D030](../decisions/006-integrations.md#d030-admin-rbac)

## 7. 验收标准

- 管理后台能覆盖菜单配置、租户自定义、版本发布、审批、回滚、导入导出、预览和诊断。
- 导入导出能支持跨环境迁移，并保留审计记录。
- 多维预览能按指定上下文展示最终菜单。
- 完整诊断能解释菜单显示或隐藏原因。
- 管理后台操作权限由外部权限系统控制。

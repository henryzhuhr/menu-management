# PRD 决策记录索引

本目录记录统一菜单中心 PRD 讨论中的全部产品决策。

每条决策包含：

- 决策编号
- 原始提问
- 提问解释
- 用户决策
- 需求影响

## 决策文件

- [001-product-scope.md](./001-product-scope.md)
  产品范围、角色、系统范围和 PRD 组织
- [002-menu-model.md](./002-menu-model.md)
  菜单模型、节点类型、多客户端、多语言和删除语义
- [003-visibility-rules.md](./003-visibility-rules.md)
  可见性、订阅、权限、条件策略和树展示规则
- [004-tenant-user-customization.md](./004-tenant-user-customization.md)
  租户自定义、用户个性化、强制菜单、排序和冲突
- [005-release-governance.md](./005-release-governance.md)
  版本、发布、灰度、审批、回滚和审计
- [006-integrations.md](./006-integrations.md)
  外部身份、权限、订阅系统和主数据边界
- [007-admin-diagnostics-analytics.md](./007-admin-diagnostics-analytics.md)
  管理后台、导入导出、预览、诊断和分析
- [008-non-functional.md](./008-non-functional.md)
  SLA、容量、安全、合规和数据留存
- [009-release-channel.md](./009-release-channel.md)
  发布通道模型、租户通道绑定、安全 hotfix、跨通道预览、版本快照和数据存储
- [010-business-isolation-and-independent-release.md](./010-business-isolation-and-independent-release.md)
  业务隔离、配置发布与模块代码发布边界、运行时业务上下文和灰度命中规则

## 决策清单

- [D001 产品边界](./001-product-scope.md#d001-product-boundary)
- [D002 用户角色](./001-product-scope.md#d002-user-roles)
- [D003 过滤规则复杂度](./003-visibility-rules.md#d003-filter-rules)
- [D004 系统范围](./001-product-scope.md#d004-app-scope)
- [D005 权限归属](./006-integrations.md#d005-permission-ownership)
- [D006 发布流程](./005-release-governance.md#d006-publish-flow)
- [D007 菜单归属](./002-menu-model.md#d007-menu-ownership)
- [D008 订阅模型](./003-visibility-rules.md#d008-subscription-model)
- [D009 灰度对象](./005-release-governance.md#d009-rollout-target)
- [D010 租户自定义边界](./004-tenant-user-customization.md#d010-tenant-custom-scope)
- [D011 菜单节点类型](./002-menu-model.md#d011-menu-node-types)
- [D012 权限绑定](./003-visibility-rules.md#d012-permission-binding)
- [D013 管理入口](./007-admin-diagnostics-analytics.md#d013-admin-modules)
- [D014 发布审批](./005-release-governance.md#d014-approval-flow)
- [D015 运行返回](./002-menu-model.md#d015-runtime-payload)
- [D016 租户变更流程](./004-tenant-user-customization.md#d016-tenant-change-flow)
- [D017 用户上下文](./003-visibility-rules.md#d017-runtime-context)
- [D018 默认安全策略](./003-visibility-rules.md#d018-default-visibility)
- [D019 环境地域语义](./002-menu-model.md#d019-env-region-semantics)
- [D020 国际化范围](./002-menu-model.md#d020-i18n-scope)
- [D021 审计历史](./005-release-governance.md#d021-audit-history)
- [D022 左树体验](./002-menu-model.md#d022-left-tree-experience)
- [D023 用户个性化](./004-tenant-user-customization.md#d023-user-personalization)
- [D024 规模目标](./008-non-functional.md#d024-scale-target)
- [D025 规则优先级](./003-visibility-rules.md#d025-visibility-precedence)
- [D026 强制菜单](./004-tenant-user-customization.md#d026-mandatory-menu)
- [D027 生效时效](./005-release-governance.md#d027-runtime-freshness)
- [D028 实时范围](./005-release-governance.md#d028-realtime-scope)
- [D029 依赖故障](./006-integrations.md#d029-dependency-failure)
- [D030 管理角色权限](./006-integrations.md#d030-admin-rbac)
- [D031 冲突处理](./004-tenant-user-customization.md#d031-tenant-conflict-resolution)
- [D032 发布预览](./007-admin-diagnostics-analytics.md#d032-preview-requirements)
- [D033 配置方式](./007-admin-diagnostics-analytics.md#d033-configuration-channels)
- [D034 客户端范围](./002-menu-model.md#d034-client-scope)
- [D035 语言回退](./002-menu-model.md#d035-language-fallback)
- [D036 删除语义](./002-menu-model.md#d036-delete-semantics)
- [D037 多端模型](./002-menu-model.md#d037-multi-client-model)
- [D038 条件策略模型](./003-visibility-rules.md#d038-condition-model)
- [D039 功能点关系](./003-visibility-rules.md#d039-feature-relation)
- [D040 绑定语义](./003-visibility-rules.md#d040-binding-semantics)
- [D041 父子展示](./003-visibility-rules.md#d041-tree-visibility)
- [D042 排序优先级](./004-tenant-user-customization.md#d042-sort-precedence)
- [D043 关键变更范围](./005-release-governance.md#d043-critical-changes)
- [D044 审批规则](./005-release-governance.md#d044-approval-policy)
- [D045 回滚粒度](./005-release-governance.md#d045-rollback-scope)
- [D046 使用分析](./007-admin-diagnostics-analytics.md#d046-usage-analytics)
- [D047 配置迁移](./007-admin-diagnostics-analytics.md#d047-config-migration)
- [D048 查询排障](./007-admin-diagnostics-analytics.md#d048-diagnostics)
- [D049 外部系统](./006-integrations.md#d049-external-integrations)
- [D050 服务指标](./008-non-functional.md#d050-service-slo)
- [D051 数据留存](./008-non-functional.md#d051-retention-policy)
- [D052 SLA 指标](./008-non-functional.md#d052-sla-target)
- [D053 容量量级](./008-non-functional.md#d053-capacity-target)
- [D054 合规安全](./008-non-functional.md#d054-compliance-scope)
- [D055 订阅主源](./006-integrations.md#d055-subscription-source-of-truth)
- [D056 功能点主源](./006-integrations.md#d056-feature-source-of-truth)
- [D057 订阅异常](./006-integrations.md#d057-subscription-mismatch)
- [D058 范围排除](./001-product-scope.md#d058-out-of-scope)
- [D059 验收重点](./001-product-scope.md#d059-acceptance-priority)
- [D060 PRD 形态](./001-product-scope.md#d060-prd-shape)
- [D061 发布通道模型](./009-release-channel.md#d061-channel-model)
- [D062 租户通道绑定](./009-release-channel.md#d062-tenant-channel-binding)
- [D063 安全变更处理](./009-release-channel.md#d063-security-hotfix)
- [D064 跨通道预览](./009-release-channel.md#d064-cross-channel-preview)
- [D065 版本创建与功能上车](./009-release-channel.md#d065-version-snapshot)
- [D066 数据库存储模型](./009-release-channel.md#d066-db-snapshot-storage)
- [D067 服务实例版本映射](./009-release-channel.md#d067-service-version-mapping)
- [D068 业务独立发布单元](./010-business-isolation-and-independent-release.md#d068-business-release-unit)
- [D069 代码发布边界](./010-business-isolation-and-independent-release.md#d069-runtime-code-release-boundary)
- [D070 运行时业务上下文](./010-business-isolation-and-independent-release.md#d070-menu-runtime-context)
- [D071 灰度版本选择规则](./010-business-isolation-and-independent-release.md#d071-gray-resolution)

# 统一菜单中心 PRD 总览

## 1. 背景

大型平台通常存在多个应用、多个租户、多个环境、多个地域和多个客户端。菜单配置如果散落在各业务系统中，会导致配置重复、权限口径不一致、发布不可控、租户差异难治理、问题排查困难。

统一菜单中心用于集中管理菜单结构、可见性规则、租户自定义、版本发布和运行时菜单分发，并为前端提供稳定、可解释的菜单查询能力。

决策引用：[D001](decisions/001-product-scope.md#d001-product-boundary)、[D004](decisions/001-product-scope.md#d004-app-scope)、[D034](decisions/002-menu-model.md#d034-client-scope)、[D053](decisions/008-non-functional.md#d053-capacity-target)

## 2. 产品定位

统一菜单中心是平台级菜单配置与分发系统。

系统需要支持：

- 多应用、多租户、多环境、多地域、多客户端
- 平台标准菜单模板
- 租户继承、扩展、隐藏和排序
- 外部身份、权限、订阅系统集成
- 版本、灰度、审批、发布、回滚
- 运行时菜单过滤和导航元数据返回
- 多维预览、完整诊断、审计和使用分析

决策引用：[D001](decisions/001-product-scope.md#d001-product-boundary)、[D004](decisions/001-product-scope.md#d004-app-scope)、[D006](decisions/005-release-governance.md#d006-publish-flow)、[D013](decisions/007-admin-diagnostics-analytics.md#d013-admin-modules)、[D059](decisions/001-product-scope.md#d059-acceptance-priority)

## 3. 用户角色

系统覆盖以下角色：

- 平台管理员：维护平台菜单模板、功能点、条件策略、版本和发布范围。
- 租户管理员：在平台模板基础上维护本租户菜单扩展、隐藏、排序和草稿发布。
- 普通用户：在前端客户端消费最终菜单，并可在允许范围内进行个人化排序和隐藏。
- 审批人：审批关键变更和高风险发布。
- 审计/运营人员：查看审计、版本历史、使用分析和诊断结果。

管理后台自身权限需要完全可配置，不内置固定授权体系。

决策引用：[D002](decisions/001-product-scope.md#d002-user-roles)、[D030](decisions/006-integrations.md#d030-admin-rbac)

## 4. 范围

### 本次包含

- 统一菜单中心的完整产品需求定义
- 菜单配置、租户自定义、运行时菜单、版本治理、外部集成、诊断分析和非功能要求
- 所有已确认问答决策的记录和引用

### 本次不包含

- 身份认证系统的实现
- 用户、角色、权限授权系统的实现
- 订阅购买、合同、计费和订单系统的实现
- 数据库表结构、接口字段、技术栈、缓存策略等 TDD 细节
- 前后端工程代码实现

决策引用：[D058](decisions/001-product-scope.md#d058-out-of-scope)

## 5. 核心规则

- 菜单最终可见性必须安全优先，任何权限、订阅、条件或依赖不明确时默认隐藏。
- 用户个性化、租户自定义不能放大权限，不能突破订阅和强制菜单约束。
- 外部订阅系统是租户订阅主数据源，菜单中心只维护功能点定义和菜单绑定。
- 外部权限系统是用户授权主数据源，菜单中心只绑定权限标识并消费权限结果。
- 安全相关变更需要实时影响运行时菜单。
- 关键变更必须经过可配置审批流。
- 所有治理动作需要可审计、可诊断、可回滚。
- 应用是菜单版本和配置发布的基本隔离单元；多个应用可以共享菜单服务，但不能共享未受控的可变版本指针。
- 菜单配置发布与菜单服务代码发布解耦；共享实例代码升级影响实例承载的业务，需要代码独立升级时使用独立运行组。

决策引用：[D018](decisions/003-visibility-rules.md#d018-default-visibility)、[D025](decisions/003-visibility-rules.md#d025-visibility-precedence)、[D028](decisions/005-release-governance.md#d028-realtime-scope)、[D043](decisions/005-release-governance.md#d043-critical-changes)、[D044](decisions/005-release-governance.md#d044-approval-policy)、[D049](decisions/006-integrations.md#d049-external-integrations)、[D055](decisions/006-integrations.md#d055-subscription-source-of-truth)、[D056](decisions/006-integrations.md#d056-feature-source-of-truth)、[D068](decisions/010-business-isolation-and-independent-release.md#d068-business-release-unit)、[D069](decisions/010-business-isolation-and-independent-release.md#d069-runtime-code-release-boundary)

## 6. 验收重点

本项目的首要验收重点是治理闭环，而不是只完成单一菜单查询接口。

验收时应重点验证：

- 管理员可以完成菜单配置、版本、审批、灰度、发布和回滚闭环。
- 运行时菜单在不同应用、环境、地域、客户端、租户、用户、语言下结果正确。
- 对任意菜单可见或隐藏结果，系统能给出完整诊断解释。
- 安全相关变更实时生效。
- 外部权限或订阅异常时，系统安全降级并告警。

决策引用：[D032](decisions/007-admin-diagnostics-analytics.md#d032-preview-requirements)、[D045](decisions/005-release-governance.md#d045-rollback-scope)、[D048](decisions/007-admin-diagnostics-analytics.md#d048-diagnostics)、[D057](decisions/006-integrations.md#d057-subscription-mismatch)、[D059](decisions/001-product-scope.md#d059-acceptance-priority)

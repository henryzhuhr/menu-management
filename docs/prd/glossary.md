# 术语表

## 应用

接入统一菜单中心的业务系统。一个应用可以有多个客户端、环境和地域配置。

## 客户端

消费菜单的终端形态，例如 Web、移动端、桌面端或嵌入式控制台。菜单中心使用“逻辑菜单 + 客户端展示元数据”的方式支持多客户端。

决策引用：[D034](decisions/002-menu-model.md#d034-client-scope)、[D037](decisions/002-menu-model.md#d037-multi-client-model)

## 租户

平台中的客户或组织隔离单元。租户继承平台菜单模板，并可以在允许范围内扩展、隐藏和排序菜单。

决策引用：[D004](decisions/001-product-scope.md#d004-app-scope)、[D010](decisions/004-tenant-user-customization.md#d010-tenant-custom-scope)

## 菜单节点

菜单树中的节点。节点类型包括目录、页面和外链。

决策引用：[D011](decisions/002-menu-model.md#d011-menu-node-types)

## 逻辑菜单

跨客户端复用的菜单业务定义。逻辑菜单承载权限、订阅和条件规则，不同客户端通过展示元数据决定呈现方式。

决策引用：[D037](decisions/002-menu-model.md#d037-multi-client-model)

## 展示元数据

用于前端导航展示的数据，例如名称、图标、排序、路由、打开方式、面包屑、客户端展示方式等。

决策引用：[D015](decisions/002-menu-model.md#d015-runtime-payload)

## 功能点

菜单中心定义的产品能力编码。菜单节点绑定一个或多个功能点，外部订阅系统返回租户拥有的功能点。

决策引用：[D039](decisions/003-visibility-rules.md#d039-feature-relation)、[D056](decisions/006-integrations.md#d056-feature-source-of-truth)

## 权限标识

菜单节点绑定的外部权限标识。运行时通过外部权限系统返回的用户权限判断菜单是否可见。

决策引用：[D005](decisions/006-integrations.md#d005-permission-ownership)、[D012](decisions/003-visibility-rules.md#d012-permission-binding)

## 发布范围

一次发布影响的应用、环境、地域、客户端、租户或用户群范围。回滚按发布范围执行。

决策引用：[D009](decisions/005-release-governance.md#d009-rollout-target)、[D045](decisions/005-release-governance.md#d045-rollback-scope)

## 灰度

菜单版本按租户、环境、地域或用户群逐步开放的发布方式。

决策引用：[D006](decisions/005-release-governance.md#d006-publish-flow)、[D009](decisions/005-release-governance.md#d009-rollout-target)

## 强制菜单

平台或租户标记为不可被用户隐藏或重排的菜单。强制菜单仍必须满足权限和订阅规则。

决策引用：[D026](decisions/004-tenant-user-customization.md#d026-mandatory-menu)

## 可见性解释

系统对某个菜单为什么显示或隐藏给出的诊断结果，包括版本、订阅、权限、条件、租户覆盖、用户个性化和依赖状态。

决策引用：[D048](decisions/007-admin-diagnostics-analytics.md#d048-diagnostics)

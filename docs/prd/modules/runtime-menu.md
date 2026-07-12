# 运行时菜单查询与前端消费

## 1. 目标

运行时菜单能力负责根据当前上下文生成用户最终可见菜单，并返回给前端客户端展示。菜单中心需要保证结果正确、安全、可解释，并满足核心链路 SLA。

决策引用：[D003](../decisions/003-visibility-rules.md#d003-filter-rules)、[D015](../decisions/002-menu-model.md#d015-runtime-payload)、[D017](../decisions/003-visibility-rules.md#d017-runtime-context)、[D018](../decisions/003-visibility-rules.md#d018-default-visibility)、[D025](../decisions/003-visibility-rules.md#d025-visibility-precedence)、[D052](../decisions/008-non-functional.md#d052-sla-target)

## 2. 查询上下文

运行时菜单查询至少需要识别以下上下文：

- 应用
- 环境
- 地域
- 客户端
- 租户
- 用户
- 用户语言

系统可基于这些上下文计算版本命中、地域差异、客户端展示、租户自定义、用户权限、订阅状态和用户个性化。

决策引用：[D017](../decisions/003-visibility-rules.md#d017-runtime-context)、[D019](../decisions/002-menu-model.md#d019-env-region-semantics)、[D034](../decisions/002-menu-model.md#d034-client-scope)

### 2.1 业务上下文与多业务服务

`应用`是菜单版本和发布治理的第一隔离维度。一个菜单服务实例可以同时承载多个应用，但每次查询必须明确受信任的应用上下文。

运行时菜单解析上下文为：

```text
(app, tenant, user, environment, region, client, locale)
```

`app` 应来自访问令牌、网关路由或服务端调用上下文，并参与版本选择、权限校验、缓存 key、诊断和审计。不能只依赖前端任意传入的 `app` 查询参数，否则可能把业务 A 的菜单或权限边界错误地用于业务 B。

同一个租户如果同时使用多个业务，需要按业务分别查询或调用批量接口；每个业务仍使用自己的 `(app, version)` 快照和应用级通道指针。

决策引用：[D004](../decisions/001-product-scope.md#d004-app-scope)、[D070](../decisions/010-business-isolation-and-independent-release.md#d070-menu-runtime-context)

## 3. 返回内容

运行时返回内容不仅包含左树节点，还需要包含前端导航所需元数据：

- 菜单层级
- 菜单名称和语言回退后的展示文案
- 节点类型
- 路由或外链
- 图标
- 排序结果
- 打开方式
- 面包屑
- 客户端展示元数据
- 空状态或错误提示所需信息

前端可以基于返回结果渲染左侧树、移动端导航、桌面端导航或其他客户端形态。

决策引用：[D015](../decisions/002-menu-model.md#d015-runtime-payload)、[D022](../decisions/002-menu-model.md#d022-left-tree-experience)、[D034](../decisions/002-menu-model.md#d034-client-scope)、[D037](../decisions/002-menu-model.md#d037-multi-client-model)

## 4. 可见性计算规则

菜单最终可见性按安全优先原则计算。

系统需要综合判断：

- 当前菜单版本是否已发布到目标范围
- 菜单是否启用
- 当前租户是否具备对应功能点订阅
- 当前用户是否具备对应权限标识
- 环境、地域、客户端、时间窗、灰度标签等内置条件是否满足
- 租户是否隐藏或扩展菜单
- 用户是否在允许范围内隐藏或排序菜单
- 菜单是否被标记为强制菜单

任何订阅、权限、条件或上下文无法确认时，默认不展示受控菜单。

决策引用：[D018](../decisions/003-visibility-rules.md#d018-default-visibility)、[D025](../decisions/003-visibility-rules.md#d025-visibility-precedence)、[D026](../decisions/004-tenant-user-customization.md#d026-mandatory-menu)、[D038](../decisions/003-visibility-rules.md#d038-condition-model)、[D039](../decisions/003-visibility-rules.md#d039-feature-relation)、[D040](../decisions/003-visibility-rules.md#d040-binding-semantics)

## 5. 父子节点展示

目录节点自身满足条件，或存在至少一个可见子节点时可以展示。没有可见子节点且自身不满足展示条件的目录不展示，避免前端出现空目录。

决策引用：[D041](../decisions/003-visibility-rules.md#d041-tree-visibility)

## 6. 排序和个性化

最终排序需要在可见菜单集合内计算。

排序优先级为：

1. 用户排序
2. 租户排序
3. 平台默认排序

用户排序和隐藏只在允许范围内生效，不能突破权限、订阅、条件和强制菜单约束。

决策引用：[D023](../decisions/004-tenant-user-customization.md#d023-user-personalization)、[D026](../decisions/004-tenant-user-customization.md#d026-mandatory-menu)、[D042](../decisions/004-tenant-user-customization.md#d042-sort-precedence)

## 7. 实时性和安全降级

安全相关变更必须实时影响运行时菜单，包括：

- 权限撤销
- 订阅停用
- 菜单下线
- 菜单停用
- 发布回滚

展示类变更可以在产品允许范围内有短暂延迟，但不能影响安全边界。

当外部权限系统、订阅系统或其他安全依赖不可用时，系统需要安全降级：无法确认权限或订阅的受控菜单不展示，只返回明确可公开或可确认的菜单。

决策引用：[D027](../decisions/005-release-governance.md#d027-runtime-freshness)、[D028](../decisions/005-release-governance.md#d028-realtime-scope)、[D029](../decisions/006-integrations.md#d029-dependency-failure)

## 8. 验收标准

- 指定应用、环境、地域、客户端、租户、用户和语言后，系统返回正确菜单树。
- 用户缺少权限、订阅或条件不满足时，相关菜单不展示。
- 父目录在存在可见子节点时展示，在空目录场景下隐藏。
- 用户排序、租户排序和平台排序按既定优先级生效。
- 强制菜单不能被用户隐藏或重排，但仍受权限和订阅约束。
- 外部权限或订阅不可用时，系统安全降级。
- 安全相关变更在下一次运行时查询中实时生效。
- 业务 A 和业务 B 共用服务实例时，查询结果、版本命中和缓存按应用隔离。
- 运行时菜单查询满足 99.9% SLA 和 P95 200ms 目标。

决策引用：[D052](../decisions/008-non-functional.md#d052-sla-target)、[D059](../decisions/001-product-scope.md#d059-acceptance-priority)

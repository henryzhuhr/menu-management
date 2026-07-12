# 菜单配置与菜单模型

## 1. 目标

菜单配置能力负责维护平台标准菜单模板、逻辑菜单、多客户端展示元数据、功能点绑定、权限标识、内置条件和多语言文案。

决策引用：[D007](../decisions/002-menu-model.md#d007-menu-ownership)、[D011](../decisions/002-menu-model.md#d011-menu-node-types)、[D012](../decisions/003-visibility-rules.md#d012-permission-binding)、[D020](../decisions/002-menu-model.md#d020-i18n-scope)、[D037](../decisions/002-menu-model.md#d037-multi-client-model)

## 2. 菜单结构

菜单中心以平台标准菜单模板为基础。平台管理员负责维护标准菜单树，租户可以在继承基础上进行扩展、隐藏和排序。

菜单配置必须归属一个应用。应用 A 和应用 B 可以使用相同的菜单服务能力，但默认不共享可变菜单版本；需要复用内容时，应通过显式复制或受控模板继承产生应用内版本，不能通过跨应用隐式引用引入发布耦合。

菜单节点类型包括：

- 目录：承载层级结构，不一定直接跳转。
- 页面：对应业务系统内部页面或路由。
- 外链：跳转第三方或外部地址。

决策引用：[D007](../decisions/002-menu-model.md#d007-menu-ownership)、[D010](../decisions/004-tenant-user-customization.md#d010-tenant-custom-scope)、[D011](../decisions/002-menu-model.md#d011-menu-node-types)

## 3. 逻辑菜单与多客户端展示

菜单模型采用“逻辑菜单 + 客户端展示元数据”。

逻辑菜单承载通用业务定义：

- 功能点绑定
- 权限标识绑定
- 内置条件
- 多语言文案
- 启停状态
- 发布和版本关系

不同客户端可以配置不同展示元数据，例如 Web 左树、移动端导航、桌面端控制台入口或嵌入式菜单。

决策引用：[D034](../decisions/002-menu-model.md#d034-client-scope)、[D037](../decisions/002-menu-model.md#d037-multi-client-model)

## 4. 多语言

菜单名称和导航展示文案支持多语言。运行时按用户语言返回文案，缺失时回退到应用默认语言。

如果默认语言仍缺失，系统需要在管理侧诊断中标记配置异常；运行时是否展示由具体菜单和客户端展示要求决定，但不能影响权限和订阅判断。

决策引用：[D020](../decisions/002-menu-model.md#d020-i18n-scope)、[D035](../decisions/002-menu-model.md#d035-language-fallback)

## 5. 功能点和权限绑定

菜单节点可以绑定一个或多个功能点，也可以绑定一个或多个权限标识。

- 功能点由菜单中心定义。
- 租户拥有的功能点来自外部订阅系统。
- 权限标识来自外部权限系统。
- 菜单中心只维护绑定关系，不实现外部订阅购买或用户授权。

决策引用：[D012](../decisions/003-visibility-rules.md#d012-permission-binding)、[D039](../decisions/003-visibility-rules.md#d039-feature-relation)、[D055](../decisions/006-integrations.md#d055-subscription-source-of-truth)、[D056](../decisions/006-integrations.md#d056-feature-source-of-truth)、[D058](../decisions/001-product-scope.md#d058-out-of-scope)

## 6. 条件策略

菜单可配置内置条件类型，至少包括：

- 环境
- 地域
- 客户端
- 租户
- 套餐或功能点
- 权限标识
- 时间窗
- 灰度标签

节点绑定多个条件时，支持配置 AND 或 OR 组合。条件组合需要能被诊断系统解释。

决策引用：[D038](../decisions/003-visibility-rules.md#d038-condition-model)、[D040](../decisions/003-visibility-rules.md#d040-binding-semantics)、[D048](../decisions/007-admin-diagnostics-analytics.md#d048-diagnostics)

## 7. 环境和地域差异

不同环境或地域可以维护不同菜单内容、名称、链接和可见规则。发布和预览必须能够明确目标环境和地域，避免跨地域或跨环境误发布。

决策引用：[D019](../decisions/002-menu-model.md#d019-env-region-semantics)

## 8. 删除和停用

菜单、功能点、套餐等核心配置默认不物理删除。系统使用停用或归档保留历史，支持审计、诊断和回滚。

决策引用：[D036](../decisions/002-menu-model.md#d036-delete-semantics)

## 9. 验收标准

- 平台管理员可以维护目录、页面和外链菜单。
- 同一逻辑菜单可以为不同客户端配置不同展示元数据。
- 菜单可以绑定功能点、权限标识和内置条件。
- 节点条件组合支持 AND/OR 配置。
- 多语言文案按用户语言和默认语言回退。
- 停用或归档后的配置仍可用于历史审计和回滚。
- 菜单版本、草稿和发布指针按应用隔离，业务 A 的配置变更不会修改业务 B。

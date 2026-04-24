# 外部身份、权限和订阅系统集成

## 1. 目标

菜单中心不实现身份认证、用户授权、订阅购买、计费或合同能力。它负责对接外部系统，消费身份、权限和订阅结果，并将这些结果用于菜单可见性计算。

决策引用：[D049](../decisions/006-integrations.md#d049-external-integrations)、[D058](../decisions/001-product-scope.md#d058-out-of-scope)

## 2. 身份系统边界

身份系统负责认证用户身份，并向菜单中心或调用方提供用户身份、租户身份和必要上下文。

菜单中心不负责：

- 登录
- 会话管理
- 密码或凭证管理
- 用户生命周期主数据管理

决策引用：[D017](../decisions/003-visibility-rules.md#d017-runtime-context)、[D049](../decisions/006-integrations.md#d049-external-integrations)、[D058](../decisions/001-product-scope.md#d058-out-of-scope)

## 3. 权限系统边界

权限系统负责用户、角色、权限和授权关系。菜单中心只绑定权限标识，并在运行时消费外部权限结果。

菜单中心需要支持：

- 菜单节点绑定权限标识
- 管理后台操作绑定权限标识
- 运行时根据用户权限过滤菜单
- 权限不可确认时安全隐藏受控菜单

决策引用：[D005](../decisions/006-integrations.md#d005-permission-ownership)、[D012](../decisions/003-visibility-rules.md#d012-permission-binding)、[D029](../decisions/006-integrations.md#d029-dependency-failure)、[D030](../decisions/006-integrations.md#d030-admin-rbac)

## 4. 订阅系统边界

外部订阅系统是租户订阅主数据源。菜单中心定义功能点并维护菜单与功能点的绑定关系。

订阅系统负责：

- 租户购买或开通套餐
- 租户拥有功能点结果
- 订阅状态主数据

菜单中心负责：

- 定义功能点编码和名称
- 维护功能点与菜单绑定
- 消费租户订阅结果
- 根据订阅结果过滤菜单

决策引用：[D008](../decisions/003-visibility-rules.md#d008-subscription-model)、[D039](../decisions/003-visibility-rules.md#d039-feature-relation)、[D055](../decisions/006-integrations.md#d055-subscription-source-of-truth)、[D056](../decisions/006-integrations.md#d056-feature-source-of-truth)

## 5. 异常处理

当外部订阅结果与菜单中心功能点配置不一致时，系统需要安全隐藏相关菜单并产生告警。

典型异常包括：

- 外部订阅返回未知功能点
- 外部订阅返回已停用功能点
- 菜单绑定了不存在的功能点
- 权限系统不可用或权限结果不可确认

决策引用：[D029](../decisions/006-integrations.md#d029-dependency-failure)、[D057](../decisions/006-integrations.md#d057-subscription-mismatch)

## 6. 验收标准

- 菜单中心能消费外部身份、权限和订阅结果。
- 菜单中心不承担身份、权限、订阅购买和计费主系统职责。
- 外部权限不可确认时，受控菜单不展示。
- 外部订阅异常时，相关菜单安全隐藏并产生告警。
- 管理后台操作权限也通过外部权限标识控制。

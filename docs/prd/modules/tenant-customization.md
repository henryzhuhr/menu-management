# 租户自定义与用户个性化

## 1. 目标

租户自定义能力允许租户在平台标准菜单模板基础上形成租户自己的菜单体验。用户个性化能力允许普通用户在安全边界内调整个人菜单展示。

决策引用：[D007](../decisions/002-menu-model.md#d007-menu-ownership)、[D010](../decisions/004-tenant-user-customization.md#d010-tenant-custom-scope)、[D023](../decisions/004-tenant-user-customization.md#d023-user-personalization)

## 2. 租户自定义范围

租户继承平台菜单模板，并可以：

- 新增租户私有菜单
- 隐藏允许隐藏的平台菜单
- 调整租户内菜单排序
- 维护租户草稿并发布到本租户

租户自定义不能突破平台定义的安全规则、功能点订阅、权限标识和强制菜单约束。

决策引用：[D010](../decisions/004-tenant-user-customization.md#d010-tenant-custom-scope)、[D016](../decisions/004-tenant-user-customization.md#d016-tenant-change-flow)、[D025](../decisions/003-visibility-rules.md#d025-visibility-precedence)

## 3. 租户草稿和发布

租户管理员维护的菜单变更先进入租户草稿。草稿发布后才影响本租户用户。

关键租户变更需要进入审批流程，例如影响权限、订阅、安全边界、强制菜单或大范围用户体验的变更。

决策引用：[D014](../decisions/005-release-governance.md#d014-approval-flow)、[D016](../decisions/004-tenant-user-customization.md#d016-tenant-change-flow)、[D043](../decisions/005-release-governance.md#d043-critical-changes)、[D044](../decisions/005-release-governance.md#d044-approval-policy)

## 4. 用户个性化

普通用户可以在允许范围内对个人菜单进行排序或隐藏。

用户个性化需要满足：

- 只能作用于用户已经可见的菜单。
- 不能让无权限或无订阅菜单变为可见。
- 不能隐藏或重排被标记为强制的菜单。
- 不影响其他用户。

决策引用：[D023](../decisions/004-tenant-user-customization.md#d023-user-personalization)、[D026](../decisions/004-tenant-user-customization.md#d026-mandatory-menu)

## 5. 排序规则

平台、租户和用户都可能提供排序配置。最终排序优先级为：

1. 用户排序
2. 租户排序
3. 平台默认排序

排序只影响展示顺序，不改变菜单可见性。

决策引用：[D042](../decisions/004-tenant-user-customization.md#d042-sort-precedence)

## 6. 平台升级冲突处理

平台菜单模板升级时，可能与租户自定义发生冲突。系统需要显式标记冲突，并要求管理员选择处理方式：

- 合并平台变更和租户自定义
- 保留租户覆盖
- 回退租户覆盖

冲突解决结果需要进入审计记录。

决策引用：[D031](../decisions/004-tenant-user-customization.md#d031-tenant-conflict-resolution)

## 7. 验收标准

- 租户可以基于平台模板新增、隐藏和排序菜单。
- 租户草稿发布前不影响运行时菜单。
- 用户可以调整自己的菜单排序或隐藏允许隐藏的菜单。
- 用户个性化不能突破权限、订阅和强制菜单约束。
- 平台模板升级冲突可被识别、处理和审计。

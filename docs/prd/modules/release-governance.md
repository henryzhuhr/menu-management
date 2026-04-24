# 版本、发布、审批、灰度和回滚

## 1. 目标

发布治理能力用于确保菜单配置变更可控、可审计、可灰度、可回滚。统一菜单中心需要支持大型平台中的多应用、多租户、多环境、多地域发布治理。

决策引用：[D006](../decisions/005-release-governance.md#d006-publish-flow)、[D009](../decisions/005-release-governance.md#d009-rollout-target)、[D021](../decisions/005-release-governance.md#d021-audit-history)、[D059](../decisions/001-product-scope.md#d059-acceptance-priority)

## 2. 版本和灰度

菜单配置变更需要进入版本管理。版本可以按以下维度灰度或发布：

- 应用
- 环境
- 地域
- 客户端
- 租户
- 用户群或白名单

不同环境和地域可以拥有不同菜单内容、名称、链接和可见规则。

决策引用：[D006](../decisions/005-release-governance.md#d006-publish-flow)、[D009](../decisions/005-release-governance.md#d009-rollout-target)、[D019](../decisions/002-menu-model.md#d019-env-region-semantics)

## 3. 关键变更审批

关键变更需要进入审批流程。关键变更包括：

- 权限相关变更
- 订阅相关变更
- 删除、停用或归档
- 全量发布
- 回滚
- 跨地域变更
- 强制菜单变更

审批流需要可配置，支持不同变更类型、应用、地域或租户使用不同审批规则。

决策引用：[D014](../decisions/005-release-governance.md#d014-approval-flow)、[D043](../decisions/005-release-governance.md#d043-critical-changes)、[D044](../decisions/005-release-governance.md#d044-approval-policy)

## 4. 发布前预览

发布前需要支持多维预览。管理员可以按应用、环境、地域、客户端、租户、用户和语言查看最终菜单结果。

预览结果需要尽量接近运行时真实结果，并能展示关键可见性解释。

决策引用：[D032](../decisions/007-admin-diagnostics-analytics.md#d032-preview-requirements)、[D048](../decisions/007-admin-diagnostics-analytics.md#d048-diagnostics)

## 5. 回滚

回滚按发布范围执行。系统需要支持将一次发布影响的应用、环境、地域、客户端和目标租户范围回滚到历史可用版本。

回滚本身属于关键变更，需要进入审批或授权控制，并产生审计记录。

决策引用：[D045](../decisions/005-release-governance.md#d045-rollback-scope)

## 6. 审计历史

系统需要完整记录：

- 变更人
- 变更时间
- 变更内容
- 审批过程
- 发布范围
- 灰度策略
- 回滚记录
- 冲突处理记录

审计记录用于追溯、合规、问题排查和回滚。

决策引用：[D021](../decisions/005-release-governance.md#d021-audit-history)、[D051](../decisions/008-non-functional.md#d051-retention-policy)

## 7. 实时生效

安全相关变更需要实时生效，包括权限撤销、订阅停用、菜单下线和发布回滚。实时生效要求优先保障安全边界。

决策引用：[D027](../decisions/005-release-governance.md#d027-runtime-freshness)、[D028](../decisions/005-release-governance.md#d028-realtime-scope)

## 8. 验收标准

- 管理员可以创建菜单版本并按范围发布。
- 版本可按租户、环境、地域和用户群灰度。
- 关键变更进入可配置审批流。
- 发布前可以进行多维预览。
- 发布后可以按发布范围回滚。
- 所有发布、审批、灰度和回滚都有完整审计。

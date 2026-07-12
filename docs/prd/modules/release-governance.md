# 版本、发布、审批、灰度和回滚

## 1. 目标

发布治理能力用于确保菜单配置变更可控、可审计、可灰度、可回滚。统一菜单中心需要支持大型平台中的多应用、多租户、多环境、多地域发布治理。

决策引用：[D006](../decisions/005-release-governance.md#d006-publish-flow)、[D009](../decisions/005-release-governance.md#d009-rollout-target)、[D021](../decisions/005-release-governance.md#d021-audit-history)、[D059](../decisions/001-product-scope.md#d059-acceptance-priority)、[D061](../decisions/009-release-channel.md#d061-channel-model)

## 2. 发布通道

菜单中心采用三级发布通道模型：EAP → STD → KA。

### 2.1 通道定义

| 通道 | 租户类型 | 租户命名示例 | 含义 |
|------|----------|-------------|------|
| EAP | 尝鲜 | `Tenant-EAP_01` | 最早看到新版本，承担验证风险 |
| STD | 标准 | `Tenant-STD_01` | EAP 验证通过后获得版本 |
| KA | 关键客户 | `Tenant-KA_01` | STD 验证通过后最终获得版本，最稳定 |

### 2.2 核心原则

通道控制的是**版本推出的时间节奏**，而非版本内功能的可见性。同一版本在各通道中的内容完全一致，不同通道只是在不同的时间点指向该版本。

### 2.3 版本生命周期

每个应用版本串行推进，各应用之间互不阻塞：

```
支付: v5 创建 → EAP → STD → KA → v6 创建 → ...
电商: v2 创建 → EAP → STD → KA → v3 创建 → ...
```

前一个版本必须在**本应用内**完成全通道推进（EAP → STD → KA），才能创建该应用的下一个新版本。不同应用之间无依赖。

### 2.4 部署模型

三个通道对应三个独立的 K8s 命名空间，各自部署一套菜单服务实例，共享同一数据库。每个服务实例维护一份应用-版本映射配置：

```
STD 服务配置：
  支付 → v5
  电商 → v2
```

查询时按 `(app, version)` 组合查询：`WHERE (app='支付' AND version='v5') OR (app='电商' AND version='v2')`。

每个应用的版本线完全独立。支付创建新版本不动电商数据，回滚也不影响电商。

控制面保存应用在各通道的版本指针，运行组中的应用-版本映射是该指针的运行时物化结果。版本推进只更新对应应用的指针并同步运行时配置，不等同于菜单服务代码升级；Helm 或其他配置分发工具可以作为同步载体，但灰度规则和租户例外不应依赖重新部署代码。

### 2.5 版本创建

版本由平台管理员按应用创建。管理员进入某个应用（如"支付业务"），基于该应用当前 KA 通道的快照，进行功能的增删改后保存为新版本。新版本默认发布到该应用的 EAP 通道。

管理员自行判断哪些功能需要保留、增减或修改。每个版本在数据库中按 `(app, version)` 存储一份完整快照。

每个应用独立维护版本线，互不干扰。支付创建 v6 只影响支付，电商数据不参与本次版本变更。

决策引用：[D061](../decisions/009-release-channel.md#d061-channel-model)、[D065](../decisions/009-release-channel.md#d065-version-snapshot)、[D066](../decisions/009-release-channel.md#d066-db-snapshot-storage)

### 2.6 业务独立发布与模块代码发布

菜单配置发布和菜单服务代码发布是两个不同的发布单元：

- 菜单配置以应用为最小发布单元。业务 A 的版本、通道指针、灰度规则和回滚范围都只作用于 A。
- 菜单服务可以由多个应用共享。共享实例使用应用-版本映射，同时服务 A 和 B，不要求两个业务菜单版本一致。
- 菜单服务代码发布以运行时实例或运行组为影响范围。共享实例升级代码时，实例承载的 A、B 都会进入新代码。
- 共享实例中的代码升级必须向后兼容；新能力使用应用级 feature flag 控制，避免代码发布等同于业务能力立即启用。
- 如果业务 A 对代码版本也有独立升级要求，为 A 分配独立运行组。运行组仍复用同一代码仓库、控制面和数据模型，只独立选择代码镜像和发布节奏。

因此，“业务 A 独立升级、不影响业务 B”默认指菜单配置独立发布；若包含模块代码，也必须启用独立运行组方案。

决策引用：[D068](../decisions/010-business-isolation-and-independent-release.md#d068-business-release-unit)、[D069](../decisions/010-business-isolation-and-independent-release.md#d069-runtime-code-release-boundary)

## 3. 租户通道管理

### 3.1 通道绑定

每个租户绑定一个默认通道（EAP / STD / KA）。通道本身就是租户的分组机制，不需要额外引入分组概念。

### 3.2 例外覆盖

平台管理员可以给特定租户设置版本例外覆盖——临时让租户看到非所属通道的版本，不改变其通道绑定。例外覆盖需要记录审计。

### 3.3 通道迁移

管理员可以将租户从一个通道迁移到另一个通道（改绑定），例如 EAP 租户经过长期合作升级为 STD 租户。

决策引用：[D062](../decisions/009-release-channel.md#d062-tenant-channel-binding)

## 4. 灰度发布

在发布通道之外，版本还可以按以下维度进一步灰度或限定发布范围：

- 环境
- 地域
- 客户端
- 用户群或白名单

灰度维度在通道内部生效：同一通道内的不同租户可以按环境、地域等维度进一步控制版本的生效范围。

例如，业务 A 将 v6 发布到 EAP，只有命中 A 灰度规则的租户获得 v6，未命中的 A 租户继续使用 A 的通道版本；业务 B 的版本指针和灰度规则完全不参与本次计算。

运行时版本命中优先级为：安全 hotfix > 应用级租户版本例外 > 灰度规则 > 应用-通道指针。发布结果必须记录命中的应用、版本、通道和灰度规则。

决策引用：[D006](../decisions/005-release-governance.md#d006-publish-flow)、[D009](../decisions/005-release-governance.md#d009-rollout-target)、[D019](../decisions/002-menu-model.md#d019-env-region-semantics)、[D071](../decisions/010-business-isolation-and-independent-release.md#d071-gray-resolution)

## 5. 关键变更审批

关键变更需要进入审批流程。关键变更包括：

- 权限相关变更
- 订阅相关变更
- 删除、停用或归档
- 全量发布（KA 通道推进）
- 回滚
- 跨地域变更
- 强制菜单变更

审批流需要可配置，支持不同变更类型、应用、地域或租户使用不同审批规则。

决策引用：[D014](../decisions/005-release-governance.md#d014-approval-flow)、[D043](../decisions/005-release-governance.md#d043-critical-changes)、[D044](../decisions/005-release-governance.md#d044-approval-policy)

## 6. 发布前预览

发布前需要支持多维预览。管理员可以按应用、环境、地域、客户端、租户、用户和语言查看最终菜单结果。

预览支持跨通道视角：管理员或租户可以以指定通道的视角预览菜单，包括模拟切换到其他通道进行沙箱体验。预览为纯查询，不改变租户通道绑定或任何发布状态。

决策引用：[D032](../decisions/007-admin-diagnostics-analytics.md#d032-preview-requirements)、[D048](../decisions/007-admin-diagnostics-analytics.md#d048-diagnostics)、[D064](../decisions/009-release-channel.md#d064-cross-channel-preview)

## 7. 回滚

回滚按发布范围执行。系统需要支持将一次发布影响的应用、环境、地域、客户端和目标租户范围回滚到历史可用版本。

回滚本身属于关键变更，需要进入审批或授权控制，并产生审计记录。

决策引用：[D045](../decisions/005-release-governance.md#d045-rollback-scope)

## 8. 审计历史

系统需要完整记录：

- 变更人
- 变更时间
- 变更内容
- 审批过程
- 发布范围
- 灰度策略
- 回滚记录
- 冲突处理记录
- 通道变更记录（租户通道绑定变更、例外覆盖、通道推进操作）
- hotfix 记录

审计记录用于追溯、合规、问题排查和回滚。

决策引用：[D021](../decisions/005-release-governance.md#d021-audit-history)、[D051](../decisions/008-non-functional.md#d051-retention-policy)

## 9. 安全变更与实时生效

安全相关变更需要实时生效，包括权限撤销、订阅停用、菜单下线和发布回滚。实时生效要求优先保障安全边界。

安全变更通过 hotfix 通道处理，可跨通道直达所有租户，不需要走 EAP → STD → KA 逐级推进。hotfix 本身作为特殊版本变更记录审计。

决策引用：[D027](../decisions/005-release-governance.md#d027-runtime-freshness)、[D028](../decisions/005-release-governance.md#d028-realtime-scope)、[D063](../decisions/009-release-channel.md#d063-security-hotfix)

## 10. 验收标准

- 管理员可以创建菜单版本并发布到 EAP 通道。
- 版本按 EAP → STD → KA 通道逐级推进。
- 同一版本在各通道中内容一致。
- 业务 A 的版本创建、灰度、发布和回滚不会改变业务 B 的版本指针。
- 一个菜单服务可以根据受信任的应用上下文分别返回业务 A 和业务 B 的菜单。
- 共享运行实例的代码升级必须支持旧业务继续工作；需要代码独立升级时可以使用独立运行组。
- 前一个版本全通道推进完成后，才能创建新版本。
- 租户按通道绑定获得对应版本，支持例外覆盖。
- 租户命名遵循 `Tenant-{通道缩写}_{编号}` 规范。
- 安全变更通过 hotfix 跨通道直达所有租户。
- 版本可按环境、地域和用户群在通道内进一步灰度。
- 关键变更进入可配置审批流。
- 发布前可以进行跨通道多维预览。
- 发布后可以按发布范围回滚。
- 所有发布、审批、灰度和回滚都有完整审计。

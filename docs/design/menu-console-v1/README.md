# 菜单管理控制台 UI 原型 v2

本目录保存统一菜单中心的本地 UI 设计原型，用于在进入工程实现前校准页面信息架构、管理后台布局和移动端适配方向。

v2 重做方向：从偏展示型视觉改为更克制的后台控制台风格，并补充 UX 设计层和响应式断点，优先保障用户旅程、关键任务流、信息密度、工作流清晰度和后续工程可实现性。

## 文件

- `index.html`：Web 端与 iPhone 17 Pro 端的双画板静态原型入口
- `styles.css`：设计样式、布局、视觉 token 和响应式查看方式

## 设计范围

- UX 层：覆盖主要用户、触发场景、端到端用户旅程、关键任务流、关键反馈和异常状态。
- 桌面端：菜单管理控制台，覆盖上下文筛选、左侧导航、菜单树、节点配置、可见性诊断、发布流程。
- 移动端：同一套控制台内容在窄屏下响应式重排为顶部导航、单列工作区、紧凑指标和移动端任务流。
- iOS 端：保留 iPhone 17 Pro 逻辑尺寸 `402 x 874` 的移动端参考画板，用于观察移动端信息密度。

## 需求引用

- PRD 总览：[../../prd/overview.md](../../prd/overview.md)
- 菜单配置模块：[../../prd/modules/menu-configuration.md](../../prd/modules/menu-configuration.md)
- 运行时菜单模块：[../../prd/modules/runtime-menu.md](../../prd/modules/runtime-menu.md)
- 诊断分析模块：[../../prd/modules/admin-operations.md](../../prd/modules/admin-operations.md)

## 查看方式

在浏览器打开 `index.html` 即可查看本地原型。

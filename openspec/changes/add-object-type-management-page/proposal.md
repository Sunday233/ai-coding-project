## Why

当前对象类型管理界面已有明确视觉目标（左侧导航与顶部面包屑为公共区，中间为可切换业务页面），但缺少可执行的需求基线与能力边界定义，导致后续开发容易在页面拆分、交互一致性和验收标准上反复。需要先形成结构化提案，统一范围、能力与交付预期，再进入实现。

## What Changes

- 明确对象类型列表页的业务需求：搜索、表格列、状态展示、可见性标记、查看操作、分页与每页条数切换。
- 明确管理端页面壳层能力：左侧导航与顶部面包屑作为公共组件，中间内容区承载各业务页面切换。
- 明确列表交互状态规范：加载态、空态、错误态、选择态与分页切页行为。
- 输出可直接执行的设计与任务分解文档，确保后续实现阶段可按能力逐项落地。

## Capabilities

### New Capabilities
- `management-shell-layout`: 提供可复用的管理端页面壳层，统一左侧导航、顶部面包屑和内容区结构。
- `object-type-management-page`: 提供对象类型管理页面，支持对象类型数据的查询、浏览、分页与跳转查看。
- `object-type-list-interaction`: 定义对象类型列表的状态标签、可见性标签、勾选与分页等关键交互行为。

### Modified Capabilities
- None.

## Impact

- 受影响文档：`openspec/changes/add-object-type-management-page/` 下的 proposal、design、specs、tasks。
- 预期受影响代码区域（实施阶段）：布局层、路由层、对象类型页面组件、列表数据服务与类型定义。
- 技术栈约束沿用项目基线：Vue 3 + TypeScript + Vite + Ant Design Vue + Scoped Scss + Composition API。
- 无破坏性变更（无对外 API 协议变更）。

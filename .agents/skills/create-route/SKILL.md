---
name: create-route
description: 指导在前端项目中按团队规范创建和维护路由，包括目录结构、页面组件、懒加载用法。当前端需要新增或重构页面路由时使用本技能。
---

# 创建与维护路由

## 重要提示

在开始创建之前，请务必阅读以下关键规范：

**必读规范**：
- `.agents/rules/03-项目结构.md` - 目录结构要求（特别是 Scoped Scss 与 `index.module.scss` 的边界）
- `.agents/rules/06-路由规范.md` - 路由配置约束

**常见错误警告**：
- 页面样式默认应写在 `<style scoped lang="scss">`，不要无必要拆分外部样式文件
- 路由目录名使用 `kebab-case`，例如 `login`、`ai-editor`
- 必须在全局唯一路由入口注册，禁止多处维护同一条路由

---

## 标准路由目录结构

每个路由对应 `src/views/<view-name>/` 目录：

```text
src/views/login/
  └─ index.vue             # 页面主组件
```

**关键要求**：
- 组件导出：默认导出页面组件
- 目录名：`kebab-case`

---

## 步骤 1：创建页面组件

```vue
<!-- src/views/login/index.vue -->
<script setup lang="ts">
</script>

<template>
  <section class="loginPage">LoginPage</section>
</template>

<style scoped lang="scss">
.loginPage {
  min-height: 100%;
  background: var(--ant-color-bg-container);
  color: var(--ant-color-text);
}
</style>
```

如需样式复用，可选拆分 `index.module.scss`：

```vue
<!-- src/views/login/index.vue -->
<script setup lang="ts">
import styles from './index.module.scss';
</script>

<template>
  <div :class="styles.loginPage">LoginPage</div>
</template>
```

**验证点**：
- [ ] 文件名为 `index.vue`
- [ ] 默认使用 `<style scoped lang="scss">`，仅在样式复用时导入 `./index.module.scss`
- [ ] 默认导出组件

---

## 步骤 2：在全局路由中注册

在 `src/router/index.ts` 中注册：

```ts
import { createRouter, createWebHashHistory, createWebHistory } from 'vue-router';
import type { RouteRecordRaw } from 'vue-router';

const routes: RouteRecordRaw[] = [
  {
    path: '/login',
    name: 'Login',
    component: () => import('@/views/login/index.vue'),
    meta: {
      requiresAuth: false,  // 根据实际需求设置
    },
  },
  // ...
];

const createHistory = () =>
  import.meta.env.VITE_ROUTER_MODE === 'hash' ? createWebHashHistory() : createWebHistory();

const router = createRouter({
  history: createHistory(),
  routes,
});

export default router;
```

**验证点**：
- [ ] 只在唯一路由入口注册
- [ ] 导入路径正确 `@/views/login/index.vue`
- [ ] 使用动态导入实现懒加载
- [ ] 路由模式由项目配置决定（不在页面开发阶段硬编码策略）

---

## 步骤 3：验证文件结构

创建完成后，检查目录结构是否符合规范：

```text
src/views/<view-name>/
  └─ index.vue             ✓（可选 index.module.scss）
```

**快速验证命令**：

```bash
ls -la src/views/<view-name>/
```

应该看到页面组件文件。

---

## 页面级组件放置

如果页面需要专用组件，创建 `components/` 目录：

```text
src/views/ai-editor/
  ├─ index.vue
  └─ components/           # 页面专用组件
      └─ xxx/
          └─ index.vue
```

**组件放置规则**（详见 `.agents/rules/04-组件规范.md`）：
- 页面级组件（仅当前页面使用）→ `src/views/<view>/components/`
- 通用组件（多处复用）→ `src/components/`

---

## 快速检查清单

创建完成后，逐项核对：

- [ ] 页面目录名为 `kebab-case`
- [ ] 存在 `index.vue`
- [ ] 默认 scoped scss，且仅在需要时使用 `index.module.scss`
- [ ] 路由在唯一入口文件注册
- [ ] 使用动态导入实现懒加载
- [ ] 组件放置位置正确（通用 vs 页面级）

**样式还原检查**：涉及 UI 还原的样式开发，请参考 `.agents/skills/create-proposal/SKILL.md` 中的「样式还原验证检查清单」及对应页面的 `docs/样式还原/<名称>-UI分析清单.md`。

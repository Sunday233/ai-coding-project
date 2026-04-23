---
name: create-component
description: 指导在前端项目中按团队规范创建和拆分 Vue3 组件，包括目录结构、文件命名、样式与主题变量使用。当前端需要新增或重构组件时使用本技能。
---

# 创建与拆分组件

## 使用场景

当你需要：

- 新增一个**可复用通用组件**
- 为某个页面新增**页面级组件**
- 拆分过大的页面/组件文件

请使用本技能，并同时遵守 `.agents/rules/03-项目结构.md` 与 `.agents/rules/04-组件规范.md`。

---

## 组件放在哪里？

详见 `.agents/rules/04-组件规范.md` 中的"组件放置决策树"。

- **通用组件**（跨页面复用）：`src/components/<component-name>/`
- **页面级组件**（只在单页使用）：`src/views/<view>/components/`

命名约定：

- 目录名：`kebab-case`，例如 `error-boundary`
- Vue 组件名：`PascalCase`，例如 `ErrorBoundary`

---

## 标准目录结构

通用组件示例：

```text
src/components/shentu-button/
  └─ index.vue
```

页面级组件示例：

```text
src/views/home/components/header/
  └─ index.vue
```

---

## 步骤 1：创建组件骨架

```vue
// src/components/shentu-button/index.vue
<script setup lang="ts">
interface ShentuButtonProps {
  loading?: boolean;
}

defineProps<ShentuButtonProps>();
</script>

<template>
  <button type="button" class="shentuButton" :disabled="$props.loading">
    <slot />
  </button>
</template>

<style scoped lang="scss">
.shentuButton {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background-color: var(--ant-color-primary);
  color: var(--ant-color-text);
}
</style>
```

### TSX/JSX 使用约束（重要）

- Vue SFC 默认优先使用 `<script setup lang="ts"> + <template>`。
- 仅在项目已明确配置 `@vitejs/plugin-vue-jsx` 且确实需要 JSX 渲染时，才使用 `lang="tsx"`。
- 在 Ant Design Vue 的 `customRender` 场景，优先使用 `h()` 返回 VNode，避免在未配置 JSX 运行时时触发 `React is not defined`。

并在 `src/components/index.ts` 中集中导出：

```ts
// src/components/index.ts
export { default as ShentuButton } from './shentu-button';
```

---

## 步骤 2：样式与主题变量

在 `index.vue` 的 `<style scoped lang="scss">` 中：

```scss
.shentuButton {
  padding: 8px 16px;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  // 必须使用主题变量，禁止硬编码主色
  background-color: var(--ant-color-primary);
  color: var(--ant-color-text);
}
```

**原则：**

- 默认使用 `<style scoped lang="scss">`
- 仅在样式需要跨组件复用时，使用 `index.module.scss`
- 自定义颜色必须使用主题 CSS 变量：
  - Ant Design Vue 变量：`var(--ant-color-primary)`、`var(--ant-color-text)` 等
  - 自定义变量：`var(--shentu-card-bg)`、`var(--shentu-border-radius)` 等
- 禁止在组件样式中直接硬编码主色，如 `#1677ff`。

**样式还原检查**：涉及 UI 还原的组件样式开发，请参考 `.agents/skills/create-proposal/SKILL.md` 中的「样式还原验证检查清单」及对应页面的 `docs/样式还原/<名称>-UI分析清单.md`。

---

## 步骤 5：Vue 模板语法验证

在完成组件开发后，必须验证以下模板语法要点：

### v-for 和 key 的正确使用

**重要规则**：
- 使用 `<template v-for>` 时，`:key` 必须只放在 `<template>` 标签上
- 禁止在 `<template v-for>` 内部元素上重复设置 `:key`
- 直接使用元素 `v-for` 时，`:key` 放在该元素上

**错误示例**：
```vue
<!-- 错误：重复的 key -->
<template v-for="item in items" :key="item.id">
  <div :key="item.id">  <!-- 错误！会导致编译错误 -->
    {{ item.name }}
  </div>
</template>
```

**正确示例**：
```vue
<!-- 正确：key 只放在 template 上 -->
<template v-for="item in items" :key="item.id">
  <div>  <!-- 内部元素不需要 key -->
    {{ item.name }}
  </div>
</template>
```

### 插槽语法正确使用

**Ant Design Vue 组件插槽**：
```vue
<!-- 正确 -->
<Input>
  <template #prefix>
    <SearchOutlined />
  </template>
</Input>
```

**动态插槽**：
```vue
<!-- 正确 -->
<template #[dynamicSlotName]>
  插槽内容
</template>
```

### 模板语法验证清单

- [ ] v-for 中的 key 是唯一的
- [ ] 没有重复的 `:key` 属性
- [ ] 插槽语法正确使用 `<template #slotName>`
- [ ] 没有混用 JSX 和 Vue 模板语法
- [ ] 组件可以通过 TypeScript 类型检查

---

## 步骤 3：组件职责与拆分

- 单个 `.vue` 文件建议不超过 **400 行**，超过时优先考虑拆分为子组件。
- 拆分时遵循：
  - **页面级组件**：只在一个路由使用 → 放在该视图的 `components/` 下。
  - **通用组件**：在多个路由使用 → 提取到 `src/components`。
- 一个组件应聚焦单一职责（一个明确的 UI 区块或交互单元）。

---

## 步骤 4：与 Ant Design Vue 协同

通用交互控件应基于 Ant Design Vue 进行二次封装：

```vue
<script setup lang="ts">
import { Button, type ButtonProps } from 'ant-design-vue';

interface ShentuButtonProps extends ButtonProps {
  // 可以扩展一些自定义属性
}

defineProps<ShentuButtonProps>();
</script>

<template>
  <Button type="primary" v-bind="$props">
    <slot />
  </Button>
</template>
```

---

## 快速检查清单

- [ ] 组件目录是否放在了正确位置（通用 vs 页面级）？
- [ ] 是否使用了 `index.vue`，并仅在样式复用时拆分 `index.module.scss`？
- [ ] 是否通过 `src/components/index.ts` 集中导出通用组件？
- [ ] 样式是否全部使用 Scoped CSS 或 CSS Modules，且颜色使用主题变量？
- [ ] 组件文件是否过大，是否需要拆分子组件？

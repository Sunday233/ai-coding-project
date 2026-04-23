# ai-coding-project
基于 PRD 文档和 UI 设计稿截图，自动化实现 Vue3 前端页面开发，包括 UI 分析、代码生成、自动验证和问题修复。

## 技术栈基线

- Vue3 + TypeScript + Vite + Ant Design Vue
- Composition API（SFC 默认 `<script setup lang="ts">`）
- Scoped Scss 为默认样式方案，CSS Modules 仅在样式复用场景按需使用
- API 目录统一为 `src/services`，类型目录统一为 `src/types`

## 运行环境要求

- Node.js：LTS `v22.22.0`（最低 `>=22.22.0`）
- pnpm：`>=10.32.0`

## 代码质量配置

- 统一使用根目录 `biome.json` 管理 format 与 lint 规则
- 常用命令：`pnpm lint`、`pnpm lint:fix`、`pnpm format`、`pnpm format:check`

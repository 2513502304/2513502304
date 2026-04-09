# README 统计卡片优化

**Date:** 2026-04-08
**Status:** Draft

## Problem

当前 README 底部使用 `github-readme-stats.vercel.app` 渲染统计卡片，存在两个问题：

1. **评级系统太功利** — 显示 S/A+/A/B 等级排名，给人打分感太强
2. **服务不稳定** — 依赖外部 Vercel 实例，经常加载失败导致卡片显示不出来

## Solution

替换为 [github-profile-summary-cards](https://github.com/vn7n24fez6/github-profile-summary-cards)，通过 GitHub Actions 定期生成静态 SVG 存储到仓库分支中。

### Key Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| 卡片服务 | github-profile-summary-cards | GitHub Actions 生成静态 SVG，不依赖外部服务，永不挂 |
| 主题 | bear | 温暖风格，与 CRT banner 搭配更协调 |
| 卡片选择 | Profile Details + 语言饼图 + Productive Time | 3 张互补不重复，简洁为主 |
| 排版 | 上 1 下 2 | Profile Details 居中占一行，下方两张并排，视觉层次清晰 |

### Scope

**In scope:**
- 添加 GitHub Actions workflow 定期生成统计卡片
- 更新 `README.md` 引用生成的 SVG 文件
- 移除对 `github-readme-stats.vercel.app` 的依赖，包括图片 `src` 和外层 `<a href>` 链接
- 使用 `<p align="center">` 替代现有 markdown 表格实现居中排版，卡片放置在 CRT banner 正下方（替换现有表格区域）

**Out of scope:**
- 修改 banner 或 README 其他部分
- 添加其他 profile 装饰（贡献蛇、trophy 等）

### Expected Layout

```
┌─────────────────────────────────┐
│       CRT Banner (existing)      │
├─────────────────────────────────┤
│                                  │
│      [ Profile Details ]         │  ← 居中
│                                  │
│  [ 语言饼图 ]  [ Productive Time ]│  ← 并排居中
│                                  │
└─────────────────────────────────┘
```

## Non-Goals

- 不显示任何评级/排名
- 不依赖任何外部渲染服务

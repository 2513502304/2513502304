# README & CRT Banner 优化

**Date:** 2026-04-09
**Status:** Done

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
- 添加其他 profile 装饰（贡献蛇、trophy 等）

### Expected Layout

```
┌─────────────────────────────────┐
│       CRT Banner (existing)      │
├─────────────────────────────────┤
│                                  │
│      [ Profile Details ]         │  ← 居中, width=1000
│                                  │
│  [ 语言饼图 ]  [ Productive Time ]│  ← 并排居中, 各 width=495
│                                  │
└─────────────────────────────────┘
```

## CRT Banner 增强

### 新增字段

| 字段 | Header | 内容 |
|------|--------|------|
| Top Anime | Top Anime: | K-ON!, Mushoku Tensei, Steins;Gate |
| Otaku Way | Otaku Way: | Never Skip OP & ED, Binge-Watch Series, Collect Full Figure Sets |

### 布局调整

- 字体从 18px 缩小到 16px，行间距相应缩小，以在 700px 高度内容纳所有字段
- 右侧文字起始 y=70，上下间距均衡
- 渲染逻辑重构为 `draw_field` 辅助函数，统一处理字符串和列表类型
- `name` 和 `age` 改为运行时自动获取（GitHub API / 生日计算）

### 卡片对齐

- Profile Details 卡片设置 `width="1000"` 与 CRT banner 对齐
- 下方两张并排卡片各设 `width="495"`

## Non-Goals

- 不显示任何评级/排名
- 不依赖任何外部渲染服务

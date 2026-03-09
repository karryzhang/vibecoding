# Home Content Lab V3 Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 在 vibecoding 中新增一个实验性 H5 首页页面，用真实钱包首页壳子承载 V3 混排内容实验区，验证理财卡、市场卡、AI 卡、任务卡、情境推荐卡共存时的体验边界。

**Architecture:** 采用单文件 HTML 方案，沿用 vibecoding 现有自包含页面模式。页面由顶部状态区、轻资产区、快捷操作区、实验内容区、底部导航构成；内容实验区用大卡、双列小卡、横滑卡混排。所有 UI 文案支持中英文，主题支持亮/暗色，并通过 index.html 增加入口。

**Tech Stack:** 原生 HTML / CSS / JavaScript，单文件页面，自定义 CSS 变量，localStorage 主题切换，navigator.language 双语检测。

---

### Task 1: 创建页面骨架文件

**Files:**
- Create: `vibecoding/home-content-lab.html`
- Reference: `vibecoding/RULES.md`
- Reference: `vibecoding/index.html`

**Step 1: 创建基础 HTML 文档结构**

写入完整页面骨架：`<!DOCTYPE html>`、`<head>`、`<body>`、meta viewport、title、theme-color、description。

**Step 2: 在 `<head>` 顶部加入防 FOUC 主题 IIFE**

使用 `localStorage.getItem('vibecoding_theme')` 提前设置 `data-theme`。

**Step 3: 写入双语数据结构**

在脚本中创建 `var LANG = /^zh/i.test(navigator.language || '') ? 'zh' : 'en';` 和 `T = { zh: {...}, en: {...} }`。

**Step 4: 确认页面可独立打开**

Run: `open vibecoding/home-content-lab.html`
Expected: 浏览器能打开空壳页，无控制台报错。

**Step 5: Commit**

```bash
git add vibecoding/home-content-lab.html
git commit -m "feat: 新增首页内容实验页基础骨架"
```

### Task 2: 建立设计 token 与亮暗主题

**Files:**
- Modify: `vibecoding/home-content-lab.html`

**Step 1: 定义 light 主题 CSS 变量**

包含：`--bg`、`--surface`、`--surface-2`、`--text`、`--muted`、`--border`、`--accent`、`--blue`、`--purple`、`--warning`、`--success`、`--r-sm`、`--r-md`、`--r-lg`、`--shadow-sm`、`--shadow-md`。

**Step 2: 定义 dark 主题 CSS 变量**

同时支持 `@media (prefers-color-scheme: dark)` 与 `[data-theme='dark']`。

**Step 3: 为 body 与主容器加基础布局**

移动优先，设置最大宽度、safe area、系统字体、背景渐变、滚动体验。

**Step 4: 自测主题切换可行性**

在 DevTools 中手动切换 `document.documentElement.dataset.theme='dark'`。
Expected: 页面颜色完整切换，无文字对比度问题。

**Step 5: Commit**

```bash
git add vibecoding/home-content-lab.html
git commit -m "feat: 建立首页内容实验页主题与设计变量"
```

### Task 3: 搭建顶部状态区与轻资产区

**Files:**
- Modify: `vibecoding/home-content-lab.html`

**Step 1: 实现顶部状态区**

包含：头像按钮、搜索按钮、通知按钮、可选品牌文案。

**Step 2: 实现轻资产区**

展示：总资产、24h 变化、两个轻量标签（如持仓数 / 今日收益）。

**Step 3: 补齐双语映射**

所有标题、副标题、按钮辅助文案进入 `T.zh` / `T.en`。

**Step 4: 自测 390px 宽度下首屏观感**

Expected: 顶部与资产区不拥挤，无裁切。

**Step 5: Commit**

```bash
git add vibecoding/home-content-lab.html
git commit -m "feat: 完成首页实验页顶部与轻资产区"
```

### Task 4: 搭建快捷操作区

**Files:**
- Modify: `vibecoding/home-content-lab.html`

**Step 1: 创建四个快捷操作入口**

建议：Receive / Send / Swap / Earn。

**Step 2: 设计统一按钮样式**

按钮需满足最小 44px 触控目标，支持 active 反馈。

**Step 3: 处理移动端与桌面端间距**

移动端四列紧凑；PC 可适度拉开。

**Step 4: 自测点击反馈**

Expected: 点击时有视觉按压感。

**Step 5: Commit**

```bash
git add vibecoding/home-content-lab.html
git commit -m "feat: 完成首页实验页快捷操作区"
```

### Task 5: 实现理财收益大卡

**Files:**
- Modify: `vibecoding/home-content-lab.html`

**Step 1: 创建理财收益大卡结构**

字段建议：产品名、收益率、风险标签、说明、CTA。

**Step 2: 强化金融感视觉**

使用高对比收益数字、渐变背景、状态 badge，但避免过度炫技。

**Step 3: 双语化所有字段**

收益说明与按钮文案均进入 `T`。

**Step 4: 自测首屏焦点**

Expected: 这张卡应是内容区第一视觉重点。

**Step 5: Commit**

```bash
git add vibecoding/home-content-lab.html
git commit -m "feat: 增加理财收益大卡实验模块"
```

### Task 6: 实现双列小卡（市场信息 + 任务激励）

**Files:**
- Modify: `vibecoding/home-content-lab.html`

**Step 1: 创建双列网格容器**

移动端保持双列；若空间不足可微调间距，但不要退化成单列。

**Step 2: 实现市场信息卡**

展示 2-3 个高层次摘要：涨幅、热门主题、趋势提示。

**Step 3: 实现任务激励卡**

展示签到、积分、任务 CTA。

**Step 4: 保证两张卡视觉上“同家族、不同职能”**

市场卡偏信息，任务卡偏行动。

**Step 5: Commit**

```bash
git add vibecoding/home-content-lab.html
git commit -m "feat: 增加市场卡与任务卡双列实验模块"
```

### Task 7: 实现横滑 AI 总结卡组

**Files:**
- Modify: `vibecoding/home-content-lab.html`

**Step 1: 创建横向滚动容器**

使用原生横向滚动 + scroll snap。

**Step 2: 实现 3 张 AI / 推荐卡**

每张卡一个结论：市场总结、热点轮动、操作建议。

**Step 3: 控制文案密度**

每张卡 1 个标题 + 1 条摘要 + 1 个标签，不要写成长文。

**Step 4: 自测横滑体验**

Expected: 手势顺畅，不误触页面主滚动。

**Step 5: Commit**

```bash
git add vibecoding/home-content-lab.html
git commit -m "feat: 增加横滑AI总结卡组"
```

### Task 8: 实现情境推荐大卡

**Files:**
- Modify: `vibecoding/home-content-lab.html`

**Step 1: 创建情境推荐卡结构**

包含：情境句、推荐动作、收益估算 / 原因说明、CTA。

**Step 2: 视觉上区别于理财收益卡**

避免看起来像第二张同类广告卡；要更像“基于用户状态的建议”。

**Step 3: 增加解释性文案**

例如“你有闲置 USDT，可转入灵活理财”。

**Step 4: 自测与理财卡的区分度**

Expected: 用户能理解“一个是产品，一个是推荐”。

**Step 5: Commit**

```bash
git add vibecoding/home-content-lab.html
git commit -m "feat: 增加情境推荐大卡"
```

### Task 9: 实现底部导航与页面收尾

**Files:**
- Modify: `vibecoding/home-content-lab.html`

**Step 1: 创建底部导航栏**

建议项：Home / Market / Discover / Assets / Me。

**Step 2: 加入 safe area 与磨砂背景**

底栏固定，兼容 iPhone Home Indicator。

**Step 3: 标记当前页为 Home active**

增加清晰 active 态。

**Step 4: 自测滚动时底栏表现**

Expected: 不遮挡主内容，底部留白足够。

**Step 5: Commit**

```bash
git add vibecoding/home-content-lab.html
git commit -m "feat: 完成首页实验页底部导航"
```

### Task 10: 接入目录页入口

**Files:**
- Modify: `vibecoding/index.html`

**Step 1: 在目录页新增实验项目卡片**

标题建议：`Home Content Lab`。

**Step 2: 补充描述文案**

说明这是 H5 首页内容混排实验页。

**Step 3: 给它明显的实验标签**

如：`LAB` / `EXPERIMENT`。

**Step 4: 本地点击验证入口**

Expected: index.html 可跳转到新页面。

**Step 5: Commit**

```bash
git add vibecoding/index.html vibecoding/home-content-lab.html
git commit -m "feat: 增加首页内容实验页目录入口"
```

### Task 11: 自测与收尾

**Files:**
- Verify: `vibecoding/home-content-lab.html`
- Verify: `vibecoding/index.html`

**Step 1: 检查双语是否完整对称**

逐段核对所有 UI 文字均可切换。

**Step 2: 检查亮/暗主题**

确保各类卡片在暗色下仍保持层次。

**Step 3: 检查移动端节奏**

重点观察：理财卡、任务卡、AI 卡混排是否杂乱。

**Step 4: 检查桌面端 max-width 与间距**

PC 下不能过宽失控。

**Step 5: 最终提交**

```bash
git add vibecoding/home-content-lab.html vibecoding/index.html
git commit -m "feat: 完成首页内容混排实验页 V3 首版"
```

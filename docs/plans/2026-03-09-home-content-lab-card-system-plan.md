# Home Content Lab Card System Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 为 home-content-lab 的图片卡片建立统一视觉骨架，并补齐按内容类型区分的图形语言，提升整页整体感。

**Architecture:** 继续使用单文件 HTML/CSS/JS 结构，在 `home-content-lab.html` 内直接增强 card shell、补充每类卡片的语义图形元素，并只对已有 feed item 做局部结构升级，不改时间流信息架构。

**Tech Stack:** HTML, CSS, vanilla JS

---

### Task 1: 建立统一 card shell
**Files:**
- Modify: `home-content-lab.html`

**Step 1:** 为图片卡增加统一的 media inner frame / overlay / vignette / panel shadow 结构。

**Step 2:** 统一 `.feed-item.image-item`、`.feed-media`、`.media-chip`、`.media-headline` 的层级和边界语言。

**Step 3:** 确保亮暗主题下都可读。

**Step 4:** 本地预览检查不同卡片是否已有家族感。

**Step 5:** Commit
```bash
git add home-content-lab.html
git commit -m "ux: 统一首页图片卡片视觉骨架"
```

### Task 2: 为缺图形语义的卡片补内容相关图形
**Files:**
- Modify: `home-content-lab.html`

**Step 1:** 为 Ops/推荐卡加入编辑精选/内容模块图形。
**Step 2:** 为 Narrative 卡加入趋势轨道/热度环。
**Step 3:** 为 Earn 卡加入收益曲线/柱状元素。
**Step 4:** 为 Promo 卡加强 editorial cover 元素。
**Step 5:** Commit
```bash
git add home-content-lab.html
git commit -m "ux: 补全首页卡片内容相关图形"
```

### Task 3: 强化高冲击卡的社媒热榜感
**Files:**
- Modify: `home-content-lab.html`

**Step 1:** KOL 卡加入扩散/热度波纹。
**Step 2:** Macro 卡加入地图/警报/冲击层。
**Step 3:** AI 卡加强信号节点与数据轨迹的一体性。
**Step 4:** Pay 卡补强支付路径/节点连接感。
**Step 5:** Commit
```bash
git add home-content-lab.html
git commit -m "ux: 强化首页高冲击卡片图形语言"
```

### Task 4: 总体验收与提交
**Files:**
- Modify: `home-content-lab.html`
- Create: `docs/plans/2026-03-09-home-content-lab-card-system-design.md`
- Create: `docs/plans/2026-03-09-home-content-lab-card-system-plan.md`

**Step 1:** 通读页面，删除重复或过度装饰元素。
**Step 2:** 检查信息层级是否仍然清晰。
**Step 3:** 提交并 push。

```bash
git add home-content-lab.html docs/plans/2026-03-09-home-content-lab-card-system-design.md docs/plans/2026-03-09-home-content-lab-card-system-plan.md
git commit -m "ux: 提升首页图片卡片系统完整度"
git push origin HEAD
```

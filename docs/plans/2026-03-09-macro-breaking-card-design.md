# Macro Breaking Card Design

**Date:** 2026-03-09
**Page:** `home-content-lab.html`
**Status:** Approved by Karry

---

## Goal

在 Home Content Lab 的统一时间流里，加入一张**高还原图文卡**，用于承载“宏观突发”内容。

这张卡不应只是普通 feed item 的换色版本，而应该像真实资讯平台里的头条封面：
- 有强烈新闻封面感
- 有宏观事件氛围
- 同时保留交易产品所需的“影响资产”信息

---

## Chosen Direction

采用 **方案 C：混合型**。

即：
- 上半部分是**高还原新闻封面感**
- 下半部分或卡片信息区叠加 **市场影响标签**
- 放入现有 feed 中，作为一张更重、更像主打内容的条目

选择原因：
1. 纯新闻封面型太像媒体 App，容易脱离交易首页语境
2. 纯市场冲击型又会失去用户刚给的参考图那种“内容卡”气质
3. 混合型最适合当前页面：既像内容，又保留市场联动信息

---

## Visual Design

### 1. Overall Feel

目标视觉关键词：
- breaking news
- geopolitical tension
- editorial cover
- premium content card

画面不追求真实照片复刻，而追求“高还原内容封面感”：
- 深色底
- 红橙色冲击光
- 地图/火线/战区/烟雾感抽象背景
- 大标题叠加在底部遮罩上

### 2. Structure

建议结构：
- 顶部左侧：`Macro` 或 `Breaking`
- 顶部右侧：事件状态 chip，如 `Alert` / `Live`
- 中间背景：抽象新闻画面（地图 + 烟雾 + 热区光斑 + 冲击波线条）
- 底部内容层：
  - 小字副标题
  - 大标题
- 卡片正文区：
  - feed title / desc
  - 影响资产 pills：`Oil ↑` `Gold ↑` `BTC ↓` `US Stocks ↓`
  - CTA：查看影响 / Watch impact

### 3. Difference from Existing Macro Card

当前 macro-shot 更像“语义化背景卡”。
这次新卡需要升级为：
- 更厚重的视觉层次
- 更强的封面感
- 更像真实内容产品中的 hero story
- 明显区别于普通图片式 feed item

---

## Content Direction

首张卡的主题采用已存在上下文中的宏观事件方向：
- 美伊局势升级
- 原油、黄金走强
- BTC / 美股承压

文案不需要写死成真实新闻报道语气，而是以产品化表达为主：
- 标题短、重、可扫读
- 副标题强调“宏观冲击正在传导”
- impact pills 负责补全交易相关性

---

## Interaction / Placement

- 放在现有 feed 中，位置高于普通 macro item
- 它是“重型图文卡”，但不单独脱离 feed
- 仍然保持时间流的一部分
- 用户扫到时，应该第一眼感受到：这是更值得点开的头条

---

## I18n

需要接入现有 `data-i18n` 机制：
- 中文、英文都补齐
- 新增独立 key，不复用旧 macro card 的文案
- 保持与现有 `applyI18n()` 流程兼容

---

## Implementation Scope

本轮只做一张：
- 1 个新的高还原 macro breaking card 样式
- 1 组中英文文案
- 插入现有 feed

明确不做：
- 不接真实图片素材
- 不接真实新闻 API
- 不做多张模板系统化抽象
- 不重构整页结构

---

## Success Criteria

完成后，这张卡应满足：
1. 一眼看上去像“头条图文封面”，不是普通 feed item
2. 既有内容平台气质，也保留交易产品的资产影响表达
3. 与现有 KOL / AI / Narrative / Earn 卡片形成明显区分
4. 中英文切换正常
5. 放进现有页面后不会破坏统一信息流节奏

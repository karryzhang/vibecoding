# Macro Breaking Card Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 在 `home-content-lab.html` 的统一信息流中新增一张高还原的宏观突发图文卡，兼顾头条封面感与市场影响表达。

**Architecture:** 直接在现有单文件页面中扩展一套新的 `macro-breaking` 卡片样式与一条新的 feed item，不重构原有渲染结构。沿用当前 `data-i18n` 文案体系，在 CSS 中新增更厚重的封面层、背景装饰层和 impact pills 组合，确保它在现有 feed 中看起来像“主打头条卡”。

**Tech Stack:** HTML, CSS, vanilla JavaScript, existing `data-i18n` dictionary

---

### Task 1: Inspect the current macro feed section

**Files:**
- Modify: `home-content-lab.html`
- Reference: `docs/plans/2026-03-09-macro-breaking-card-design.md`

**Step 1: Review the current macro-related markup**

Check these sections in `home-content-lab.html`:
- existing `.feed-media.macro-shot`
- the current macro feed item markup
- the `T.zh` / `T.en` dictionaries

**Step 2: Confirm insertion strategy**

Use a new feed item with a dedicated media class such as `.macro-breaking-shot` instead of overwriting the existing macro card.

**Step 3: Commit planning checkpoint**

```bash
git status
```

Expected: no code changes yet.

---

### Task 2: Add the new high-fidelity macro cover styles

**Files:**
- Modify: `home-content-lab.html` (style block near existing `.feed-media.*` rules)

**Step 1: Add the failing visual target (manual)**

Define the expected result before coding:
- background feels like breaking macro news
- stronger red/orange impact lighting
- deeper bottom overlay for headline readability
- decorative layers that imply map / flash / conflict / news heat

**Step 2: Write minimal implementation CSS**

Add a new style block near the existing feed-media variants:

```css
.feed-media.macro-breaking-shot{
  background:linear-gradient(135deg,#2a0f16 0%,#6f1d1b 38%,#f97316 100%);
}
.feed-media.macro-breaking-shot::before{
  background:
    radial-gradient(circle at 18% 20%,rgba(255,220,180,.18),transparent 18%),
    radial-gradient(circle at 78% 24%,rgba(255,255,255,.12),transparent 14%),
    linear-gradient(120deg,rgba(255,255,255,.08),transparent 42%);
}
.feed-media.macro-breaking-shot::after{
  content:'';
  position:absolute;
  inset:auto 0 0 0;
  height:68%;
  background:linear-gradient(to top,rgba(9,17,31,.88),rgba(9,17,31,.34),transparent);
}
```

Then add 1-3 helper layers like:
- `.macro-grid`
- `.macro-flash`
- `.macro-pulse`

Use them to create a richer editorial-cover feel.

**Step 3: Verify no selector collisions**

Check that the new `.macro-breaking-shot` styles do not replace `.macro-shot`.

**Step 4: Commit**

```bash
git add home-content-lab.html
git commit -m "ux: 增加宏观突发头条卡样式"
```

---

### Task 3: Insert the new feed item markup

**Files:**
- Modify: `home-content-lab.html` (feed section)

**Step 1: Add the new card markup above the existing macro card**

Insert a new article similar to:

```html
<article class="feed-item image-item">
  <div class="feed-media macro-breaking-shot">
    <div class="macro-grid"></div>
    <div class="macro-flash"></div>
    <div class="macro-pulse"></div>
    <span class="media-badge" data-i18n="tagMacro"></span>
    <span class="media-chip" data-i18n="breakingLive"></span>
    <div class="media-sub" data-i18n="fMb1MediaSub"></div>
    <div class="media-headline" data-i18n="fMb1Media"></div>
  </div>
  <div class="feed-meta">
    <div class="meta-left">
      <span class="time">9m ago</span>
      <span class="tag macro" data-i18n="tagMacro"></span>
    </div>
    <span class="source">Global Radar</span>
  </div>
  <div class="feed-title" data-i18n="fMb1Title"></div>
  <div class="feed-desc" data-i18n="fMb1Desc"></div>
  <div class="impact-row">
    <span class="impact-pill">Oil ↑</span>
    <span class="impact-pill">Gold ↑</span>
    <span class="impact-pill">BTC ↓</span>
    <span class="impact-pill">US Stocks ↓</span>
  </div>
  <div class="feed-foot">
    <span class="source" data-i18n="crossAsset"></span>
    <button class="cta" data-i18n="viewImpact"></button>
  </div>
</article>
```

**Step 2: Keep the existing macro card for contrast**

Do not remove the current macro item in this task.

**Step 3: Manual DOM sanity check**

Make sure class names match the CSS exactly and the structure still aligns with the existing feed rhythm.

**Step 4: Commit**

```bash
git add home-content-lab.html
git commit -m "feat: 增加宏观突发高还原图文卡"
```

---

### Task 4: Add bilingual copy for the new card

**Files:**
- Modify: `home-content-lab.html` (the `T` dictionary)

**Step 1: Add Chinese keys**

Add entries such as:

```js
breakingLive:'突发',
fMb1MediaSub:'宏观冲击正在传导',
fMb1Media:'Middle East Escalation',
fMb1Title:'中东局势升级，原油与黄金快速走强',
fMb1Desc:'风险资产承压，BTC 与美股短线回落。',
```

**Step 2: Add English keys**

Add matching English entries such as:

```js
breakingLive:'Breaking',
fMb1MediaSub:'Macro shock is spreading',
fMb1Media:'Middle East Escalation',
fMb1Title:'Escalation in the Middle East lifts oil and gold',
fMb1Desc:'Risk assets soften as BTC and U.S. stocks pull back.',
```

**Step 3: Run manual i18n verification**

Use the existing language toggle and confirm:
- all new texts switch cleanly
- no empty labels appear
- button text remains correct

**Step 4: Commit**

```bash
git add home-content-lab.html
git commit -m "copy: 补充宏观突发图文卡双语文案"
```

---

### Task 5: Validate visual hierarchy and polish

**Files:**
- Modify: `home-content-lab.html` if needed

**Step 1: Review final hierarchy manually**

Check:
- new macro card looks heavier than standard feed items
- headline remains readable over the background
- impact pills do not feel cramped
- top chips / badge alignment is clean
- card still feels part of the same feed

**Step 2: Apply minimal polish only if necessary**

Allowed tweaks:
- spacing
- overlay depth
- chip contrast
- title size
- helper-layer opacity

Avoid:
- redesigning the whole page
- changing unrelated cards

**Step 3: Final commit**

```bash
git add home-content-lab.html
git commit -m "ux: 优化宏观突发头条卡层次"
```

---

### Task 6: Push the work

**Files:**
- Modify: git history only

**Step 1: Review changes**

```bash
git diff -- home-content-lab.html docs/plans/2026-03-09-macro-breaking-card-design.md docs/plans/2026-03-09-macro-breaking-card.md
```

**Step 2: Push**

```bash
git push origin HEAD
```

**Step 3: Report back**

Summarize:
- final commit hash
- what changed visually
- whether the original macro card remains
- what to refine next if Karry wants an even stronger editorial-cover look

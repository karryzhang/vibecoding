# Home Content Lab Semantic Cover Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Rework the image covers in `home-content-lab.html` so each feed card communicates its content type in 1 second instead of relying on abstract gradients.

**Architecture:** Keep the existing unified feed-card shell and rework only the cover internals. Each cover gets one dominant semantic object (panel, path, map, trend cluster, APY snapshot) plus lighter supporting effects. Validate with a full-page screenshot after edits.

**Tech Stack:** Single-file HTML/CSS/JS, static preview via Python HTTP server, browser screenshot validation

---

### Task 1: Add semantic cover spec docs

**Files:**
- Create: `docs/plans/2026-03-09-home-content-lab-semantic-cover-design.md`
- Create: `docs/plans/2026-03-09-home-content-lab-semantic-cover-plan.md`

**Step 1: Write design summary**
- Document the problem: card style is unified but content-picture association is weak.
- Document target mapping for Ops / KOL / Pay / AI / Macro / Narrative / Promo / Earn.

**Step 2: Save the plan**
- Include exact file target: `home-content-lab.html`
- State screenshot validation requirement.

**Step 3: Commit**
```bash
git add docs/plans/2026-03-09-home-content-lab-semantic-cover-design.md docs/plans/2026-03-09-home-content-lab-semantic-cover-plan.md
git commit -m "docs: 补充首页语义封面设计方案"
```

### Task 2: Rebuild Ops / KOL / Pay covers

**Files:**
- Modify: `home-content-lab.html`

**Step 1: Ops cover**
- Replace generic gradient feel with digest/ticker/editorial modules.
- Add clearer headline rail and list structure.

**Step 2: KOL cover**
- Convert floating names into a social trend board with rank list / avatar bubbles / heat pills.

**Step 3: Pay cover**
- Strengthen Apple Pay → USDT flow with payment confirmation, transfer path, token destination, and amount/status cues.

**Step 4: Visual regression check**
- Serve the file locally and capture a screenshot.

**Step 5: Commit**
```bash
git add home-content-lab.html
git commit -m "ux: 强化首页前3张卡片语义封面"
```

### Task 3: Rebuild AI / Macro / Narrative covers

**Files:**
- Modify: `home-content-lab.html`

**Step 1: AI cover**
- Turn chart background into a signal panel with entry / target / risk overlays.

**Step 2: Macro cover**
- Make breaking-event structure clearer with live map, blast point, and cross-asset reaction strip.

**Step 3: Narrative cover**
- Center the main narrative node and show spread into surrounding related terms.

**Step 4: Visual regression check**
- Capture another screenshot and check instant recognizability.

**Step 5: Commit**
```bash
git add home-content-lab.html
git commit -m "ux: 强化首页中段卡片内容关联度"
```

### Task 4: Rebuild Promo / Earn covers and polish system consistency

**Files:**
- Modify: `home-content-lab.html`

**Step 1: Promo cover**
- Push it toward a feature-cover / editorial issue card instead of a brand gradient block.

**Step 2: Earn cover**
- Turn it into an APY snapshot with clearer rate, curve, and product feel.

**Step 3: Unify shell polish**
- Ensure inner frame / highlights / info placement still feel like one system.

**Step 4: Final screenshot validation**
- Full-page capture after all changes.

**Step 5: Commit**
```bash
git add home-content-lab.html
git commit -m "ux: 完成首页语义封面系统收口"
```

### Task 5: Push and report

**Files:**
- Modify: repo history only

**Step 1: Review diff**
```bash
git diff --stat HEAD~1..HEAD || git diff --stat
```

**Step 2: Push**
```bash
git push origin HEAD
```

**Step 3: Report back**
- Summarize before/after changes
- Include screenshot-based evaluation
- Call out any remaining weak cards for next round

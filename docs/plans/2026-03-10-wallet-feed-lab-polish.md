# Wallet Feed Lab Polish Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Systematically polish `wallet-feed-lab.html` to a higher-end product standard with cleaner layout, tighter visual hierarchy, and sharper product copy.

**Architecture:** This is a single-file refinement pass. Keep the existing page structure, but rebuild the spacing system, reduce decorative excess, tighten component consistency, and rewrite weak copy. Validate with local browser screenshots after the edit.

**Tech Stack:** HTML, CSS, vanilla JS, local browser preview

---

### Task 1: Rebuild spacing and alignment system

**Files:**
- Modify: `wallet-feed-lab.html`

**Step 1: Tighten the global spacing tokens and shell rhythm**
- Normalize page padding, stack spacing, section spacing, card padding, and corner radii.

**Step 2: Align hero, section head, feed cards, and follow-up rows**
- Ensure text starts, button edges, and card internals line up visually.

**Step 3: Run local preview**
Run: `python3 -m http.server 8768 --directory /Users/openclaw/.openclaw/workspace/vibecoding`
Expected: page renders with cleaner rhythm.

### Task 2: Reduce visual noise and raise material quality

**Files:**
- Modify: `wallet-feed-lab.html`

**Step 1: Tone down gradients, glow, and noisy outlines**
- Reduce the feeling of “demo effect” and increase calmness.

**Step 2: Make timeline feel structural**
- Refine the left rail and node markers so they support hierarchy without stealing attention.

**Step 3: Review screenshot**
Expected: page feels more premium and less busy.

### Task 3: Rewrite weak product copy

**Files:**
- Modify: `wallet-feed-lab.html`

**Step 1: Shorten hero and card headlines**
- Remove concept language and keep only what helps user decisions.

**Step 2: Rewrite follow-up area as real product capabilities**
- Make bottom actions feel believable and useful.

**Step 3: Check both zh/en dictionaries**
Expected: no obvious mismatch between markup and i18n copy.

### Task 4: Validate and capture

**Files:**
- Modify: `wallet-feed-lab.html`

**Step 1: Open in browser and take full-page screenshot**
**Step 2: Visually inspect alignment, spacing, and hierarchy**
**Step 3: Iterate if obvious rough edges remain**

# Wallet Feed Lab vNext Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a brand-new Bitget Wallet home feed experiment that blends wallet-native UI, editorial content, and action-oriented cards in one adaptive mobile page.

**Architecture:** Create a new standalone HTML prototype instead of iterating on `home-content-lab.html`. The page will use a three-layer structure: wallet OS header, adaptive mixed feed, and follow-up/action modules. Card system will intentionally mix Signal / Story / Action shapes to test whether diverse content can still feel like one coherent Bitget Wallet system.

**Tech Stack:** Single-file HTML + CSS + vanilla JS, mobile-first responsive layout, localStorage for theme/lang persistence.

---

### Task 1: Create a new standalone experiment page
- Create `wallet-feed-lab.html`
- Do not modify `home-content-lab.html`
- Start from a clean visual system rather than copying existing card styles 1:1

### Task 2: Build Wallet OS header layer
- Add wallet-native top structure: account context, compact portfolio summary, quick actions, and a lightweight market pulse panel
- Keep this section product-like, not editorial-like

### Task 3: Build adaptive card system
- Implement at least 3 distinct card families:
  - Signal Card
  - Story Card
  - Action Card
- Make their hierarchy visually obvious through size, structure, and CTA weight

### Task 4: Represent Bitget Wallet content diversity
- Include cards covering multiple content/value types:
  - AI / signal
  - macro / narrative
  - KOL / community
  - buy / pay / on-ramp
  - earn / yield
  - editorial feature / campaign

### Task 5: Build follow-up relationship layer
- Add save / follow / alert / watchlist style modules near the bottom so the page feels like an intelligent home rather than a one-pass feed

### Task 6: Add polish and demo-readiness
- Include theme toggle and CN/EN toggle if cheap to preserve from existing prototypes
- Validate mobile layout at ~430px width
- Commit with one focused UX/feat commit after page looks coherent

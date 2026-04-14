# CLAUDE.md — VibeCoding 项目契约

> 每次会话必须遵守。只放命令、约束、边界。

---

## Build & Deploy

```bash
# 无构建步骤，直接编辑 HTML/CSS/JS
# 本地预览
python3 -m http.server 8000
# 或
npx serve .

# 部署：双路部署
# 1. Vercel（主）
git push  # 自动触发：https://vibecoding-karry.vercel.app/

# 2. GitHub Pages（备）
# 自动从 main 分支部署：https://karryzhang.github.io/vibecoding/

# 文案批量修改（优先用 sed，比 Claude Code 快 10 倍）
sed -i '' 's/旧文案/新文案/g' bitget-wallet-skill.html
```

---

## Architecture

- **纯静态**：HTML/CSS/JS 单文件，无框架，无构建工具
- **双语**：`data-en` / `data-zh` 属性 + `LANG` 变量 + `setLang()` 函数
- **主要页面**：
  - `bitget-wallet-skill.html` — Bitget Wallet Skill 产品页（主力，2100+ 行）
  - `index.html` — 首页 showcase
- **仓库**：`karryzhang/vibecoding`

---

## Design Tokens（bitget-wallet-skill.html）

```css
--accent:         #00E6B8   /* Bitget 主绿 */
--accent-blue:    #4F8FFF
--accent-purple:  #8B5CF6
--card-bg:        rgba(255,255,255,0.035)    /* 基础卡片 */
--card-bg-elevated: rgba(255,255,255,0.06)  /* 升高卡片 */
--card-bg-sunken: rgba(255,255,255,0.018)   /* 凹陷卡片 */
```

---

## NEVER

- ❌ `elevated: true` — 所有 shell 命令不需要
- ❌ 破坏现有 Canvas 粒子 / orbital 动效（60个粒子，4层 orbital，conic-gradient 流光）
- ❌ 修改 Section 顺序（Hero→UseCases→WhatItDoes→Compare→Advantages→Chains→Capabilities→Cost→QuickStart→CTA）
- ❌ 单文件大改交给 Claude Code 一次性写完 — 必须拆成精准 edit 指令（>1000行单文件必拆）
- ❌ 提交 `.env.secret` — 已 gitignore
- ❌ 移动端用 `color-mix()` — 改用 `rgba()`

---

## ALWAYS

- ✅ Commit 前缀：`copy:` / `ux:` / `fix:` / `fx:` / `brand:` / `layout:` / `color:`，**中文信息**
- ✅ 所有 UI 文字双语支持（`data-en` / `data-zh` 或 T 对象）
- ✅ 移动端降级：隐藏 beam/ripple、减少粒子、关闭 backdrop-filter、grid 降列
- ✅ 文案原则：数字 > 形容词（"Save 80%+ Ops Cost" > "Low Developer Cost"）

---

## Compact Instructions

压缩时保留：
1. 当前正在修改的文件 + 具体变更位置
2. 已确认的文案定稿
3. 未完成的 TODO

---

## 文案批量改动技巧

一条 `sed` 命令改 20 处 > 用 Claude Code 写 20 个 edit。
大块新功能才用 Claude Code，改文案/调样式直接 `sed` 或 `edit`。

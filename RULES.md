# RULES.md — VibeCoding 项目规则

> 每次做 VibeCoding 任何需求时，以下为**默认执行标准**，无需重复说明。

---

## ✅ 三大默认要求（所有需求必须满足）

### 1. 中英文双语支持
- 所有 UI 文字必须支持中英文切换
- 语言检测：`var LANG = /^zh/i.test(navigator.language || '') ? 'zh' : 'en';`
- 翻译用 T 对象或 `data-en` / `data-zh` 属性方案：
  ```html
  <span data-en="Learn More" data-zh="了解更多">Learn More</span>
  ```
  ```js
  document.querySelectorAll('[data-en]').forEach(el => {
    el.textContent = LANG ? el.dataset.zh : el.dataset.en;
  });
  ```
- 不允许硬编码某一种语言的字符串（meta description 也要双语）
- 所有 T.zh 和 T.en 的 key 必须完全对称，不能有一侧缺失

### 2. 白天 / 暗黑模式双支持
- 默认亮色，同时支持 OS 深色模式和手动切换
- CSS 变量体系（必须同时定义亮色和暗色）：
  ```css
  :root {
    --bg: #f7f8fb;
    --surface: #ffffff;
    --text: #1a2030;
    --muted: #8993a8;
    --border: #e8ecf1;
    --shadow-sm: 0 1px 3px rgba(0,0,0,.04);
    --shadow-md: 0 4px 16px rgba(0,0,0,.06);
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #0e1525;
      --surface: #182032;
      --text: #e4e9f1;
      --muted: #8694ab;
      --border: #2a3548;
      --shadow-sm: 0 1px 3px rgba(0,0,0,.15);
      --shadow-md: 0 4px 16px rgba(0,0,0,.2);
    }
  }
  [data-theme="dark"]  { /* 同 dark 值 */ }
  [data-theme="light"] { /* 同 light 值 */ }
  ```
- `<head>` 顶部必须有防 FOUC 的 IIFE：
  ```html
  <script>
  (function(){
    var t = localStorage.getItem('vibecoding_theme');
    if (t) document.documentElement.setAttribute('data-theme', t);
  })();
  </script>
  ```
- 主题切换按钮写入 `vibecoding_theme` localStorage key（`'light'` | `'dark'`）

### 3. 移动端 & PC 端双适配
- **移动优先**：基础样式面向手机（≥ 360px），PC 用 `@media(min-width: 768px)` 增强
- 核心内容区 PC 最大宽度：`max-width: 1100px; margin: 0 auto;`
- 触控目标最小 44px（按钮/链接 padding 不低于此）
- Safe area 支持（iOS notch / home indicator）：
  ```css
  padding-bottom: env(safe-area-inset-bottom, 16px);
  padding-top: env(safe-area-inset-top, 0px);
  ```
- 固定 top-bar 必须 `backdrop-filter: blur()` + 不透明背景，防止内容透过
- 移动端隐藏纯装饰性重动效（beam、粒子减半），保持 60fps

---

## 🎨 设计规范

### Design Tokens（必须先定义再使用）
```css
:root {
  /* Colors */
  --accent:   #00E6B8;   /* 品牌绿 */
  --blue:     #4F8FFF;   /* 品牌蓝 */
  --purple:   #8B5CF6;   /* 品牌紫 */

  /* Radius */
  --r-sm: 8px;
  --r-md: 14px;
  --r-lg: 20px;
  --r-xl: 28px;

  /* Card depth */
  --card-sunken:   rgba(255,255,255,.018);
  --card-base:     rgba(255,255,255,.035);
  --card-elevated: rgba(255,255,255,.06);
}
```

### 视觉层次
- 卡片至少 3 级深度（sunken / base / elevated），不能全部同一背景
- 圆角统一用 design token，全站只有 3–4 种圆角值
- 字体层级：大标题 1.8rem+ / 副标题 1.1rem / 正文 0.9rem / 辅助 0.75rem

### 动效原则
- 轻交互过渡：0.1–0.15s ease
- 卡片/页面进入：0.2–0.3s ease-out
- 复杂动画（粒子/3D）：分层架构（背景 → 中层 → 前景），移动端降级
- 遵守 `prefers-reduced-motion`

---

## ⚡ 用户体验规范

### 预加载 & 骨架屏（必须实现）
- **任何需要网络请求的内容**必须有 loading/骨架屏状态，不能白屏等待
- 图片必须有 `loading="lazy"` 或主动预加载
- 字体使用系统字体栈或 `font-display: swap`，不阻塞渲染
- 关键 CSS 内联到 `<head>`，非关键 CSS 延迟加载

### 交互反馈
- 按钮点击必须有视觉反馈（`:active` 状态，scale 或 opacity 变化）
- 表单提交有 loading 状态，防止重复提交
- 错误状态友好提示，有重试路径

### 性能基线
- 首屏 LCP < 2.5s（目标）
- 无 CLS（避免后加载内容撑开布局）
- 移动端 60fps 滚动

---

## 🔧 工程规范

### 文件结构
```
vibecoding/
├── index.html          # 主页
├── *.html              # 各产品页（单文件，自包含）
├── RULES.md            # 本文件
└── assets/             # 共享资源（如有）
```

### 单文件产品页原则
- 每个产品页自包含（HTML + CSS + JS 在一个文件内）
- 无外部框架依赖（除必要时明确引入）
- 批量文案改动用 `sed -i ''`，不用 Claude Code
- CSS 变量先行：改一处全局生效，不散落 magic number

### Git 规范
- 小步 commit，每个逻辑单元独立提交
- 中文 commit 信息，前缀：`feat:` / `fix:` / `ux:` / `copy:` / `fx:` / `brand:` / `layout:` / `seo:`
- 推送前 `git status` 确认干净

### 编码策略
| 任务规模 | 工具 |
|----------|------|
| 1–2 行改动 | 直接 edit |
| 文案批量替换 | `sed -i ''` |
| 新功能 / 大重构 | Claude Code `-p` pipe 模式，`≤ 20 项/批` |
| 单文件 > 1000 行 | **不用 Claude Code**，直接 `read/edit` 精准操作 — Claude Code 对超长单文件容易卡死无输出 |
| 跨文件分析 | Claude Code read-only |

---

## 📋 需求开发 Checklist（每次必过）

- [ ] **双语**：T.zh / T.en 对称？所有 UI 文字经过翻译函数？
- [ ] **主题**：亮色 + 暗色 CSS 变量都定义了？防 FOUC IIFE 加了？
- [ ] **移动端**：≥ 360px 正常？触控目标 ≥ 44px？
- [ ] **PC 端**：max-width 限制？间距/字号合适？
- [ ] **加载状态**：有骨架屏或 loading？首屏不白屏？
- [ ] **交互反馈**：按钮有 :active 状态？错误有提示？
- [ ] **移动动效降级**：重动效在移动端关闭或减弱？
- [ ] **Git**：commit 前缀正确？中文信息？

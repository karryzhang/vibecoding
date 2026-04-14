# Geopolitical Trading Command Center H5 — Implementation Plan
# 地缘交易指挥室 H5 — 实施计划

> For Claude Code / Coding Agent execution
> 供 Claude Code / 编码 Agent 执行

---

## 1. Project Overview / 项目概述

**Product Name / 产品名称:** War Alpha / 战势指挥室

**Core Value / 核心价值:**  
Transform geopolitical events into actionable trading opportunities across BGW's full product suite (Oil/Gold futures, US Stocks, Meme coins, Prediction Markets, Stablecoin Earn).  
将地缘政治事件转化为可执行的交易机会，覆盖 BGW 全产品线。

**Tech Stack / 技术栈:**
- Framework: Next.js 14 (App Router)
- Styling: Tailwind CSS + CSS Variables (theme switching)
- State: Zustand
- i18n: next-intl
- Charts: Lightweight Charts (TradingView) or ECharts
- Mobile: Responsive + PWA ready
- Deep Link: BGW App URL Scheme

---

## 2. Core Requirements / 核心需求

### 2.1 Multi-language / 多语言
- [x] English (en)
- [x] Chinese Simplified (zh-CN)
- Structure: `/[locale]/...` route pattern
- Default: Browser detection with fallback to `en`

### 2.2 Theme / 主题
- [x] Dark Mode (default)
- [x] Light Mode
- Use CSS variables: `--color-bg`, `--color-text`, `--color-accent`
- Persist preference in localStorage
- System preference detection via `prefers-color-scheme`

### 2.3 Responsive / 响应式
- Mobile-first design
- Breakpoints: `sm:640px`, `md:768px`, `lg:1024px`, `xl:1280px`
- Touch-friendly interactions for mobile
- Desktop: Multi-column layout, richer data density

---

## 3. Page Structure / 页面结构

```
/[locale]/
├── page.tsx                    # Hero + Live pulse
├── components/
│   ├── Header.tsx              # Logo, theme toggle, language switch
│   ├── WarPulse.tsx            # Hero section with map + 4 asset indicators
│   ├── EventRipple.tsx         # Single event card with asset impact
│   ├── EventTimeline.tsx       # Scrollable event list
│   ├── AssetCommandTabs.tsx    # Oil | Gold | Stocks | Crypto | Meme tabs
│   ├── AssetCard.tsx           # Mini chart + trade panel
│   ├── MiniTradePanel.tsx      # Inline trade form (direction, leverage, amount)
│   ├── PredictionMarket.tsx    # Prediction questions with strategy link
│   ├── MemeRadar.tsx           # Live meme token rankings + safety
│   ├── StrategyBuilder.tsx     # Auto-generate strategy from prediction
│   ├── StablecoinBanner.tsx    # Fear mode: show stablecoin earn CTA
│   ├── ShareCard.tsx           # Shareable screenshot component
│   └── Footer.tsx
├── lib/
│   ├── api/
│   │   ├── bgw-skill.ts        # Bitget Wallet Skill API wrapper
│   │   ├── news.ts             # Geopolitical news API (TBD)
│   │   ├── commodities.ts      # Oil/Gold price API (TBD)
│   │   ├── stocks.ts           # US Stocks API (TBD)
│   │   └── prediction.ts       # Prediction market API (TBD)
│   ├── deeplink.ts             # BGW App deep link generator
│   ├── i18n.ts                 # Internationalization config
│   └── theme.ts                # Theme context + toggle
├── hooks/
│   ├── usePrice.ts             # Real-time price subscription
│   ├── useNews.ts              # News polling/websocket
│   └── useTradeIntent.ts       # Store trade params before deep link
└── store/
    └── index.ts                # Zustand store
```

---

## 4. API Integration Matrix / 接口集成矩阵

### ✅ Available from `bitget-wallet-skill` / 已有接口

| Feature | API | Usage |
|---------|-----|-------|
| Meme Token Price | `token-info` | Display price, market cap |
| Meme Batch Price | `batch-price` | Portfolio/watchlist valuation |
| Meme K-line | `kline` | Mini charts |
| Meme Tx Stats | `tx-info` | Volume, buy/sell ratio |
| Meme Rankings | `rankings` | Top gainers/losers |
| Meme Security | `audit` | Honeypot detection, risk flags |
| Swap Quote | `swap-quote` | Pre-trade price estimation |
| Swap Execute | `swap-send` | Execute meme trades |
| Cross-chain Order | `order-quote/create/submit` | Gasless cross-chain swaps |

**Implementation:**
```typescript
// lib/api/bgw-skill.ts
import { BitgetWalletSkill } from 'bitget-wallet-skill';

const skill = new BitgetWalletSkill({
  apiKey: process.env.NEXT_PUBLIC_BGW_API_KEY,
});

export async function getMemeTokenInfo(chainId: string, address: string) {
  return skill.tokenInfo({ chainId, address });
}

export async function getMemeRankings(chainId: string, orderBy: string) {
  return skill.rankings({ chainId, orderBy, limit: 20 });
}

export async function getSecurityAudit(chainId: string, address: string) {
  return skill.audit({ chainId, address });
}

export async function getSwapQuote(params: SwapQuoteParams) {
  return skill.swapQuote(params);
}
```

---

### ❌ NOT Available — Need Separate API / 需单独提供的接口

| Feature | Endpoint Needed | Data Structure | Priority |
|---------|-----------------|----------------|----------|
| **Geopolitical News Feed** | `GET /api/news/geopolitical` | `{ id, title, summary, timestamp, region, severity, assets_impacted[] }` | P0 |
| **Oil Price (Brent/WTI)** | `GET /api/commodities/oil` | `{ brent: { price, change_24h, change_pct }, wti: {...} }` | P0 |
| **Gold Price (XAU)** | `GET /api/commodities/gold` | `{ price, change_24h, change_pct, high_24h, low_24h }` | P0 |
| **US Stocks Quote** | `GET /api/stocks/quote?symbols=LMT,RTX,XOM,CVX` | `{ symbol, price, change_pct, volume }[]` | P0 |
| **Prediction Market Odds** | `GET /api/prediction/markets?category=geopolitical` | `{ id, question, options[{label, odds}], volume, end_time }` | P1 |
| **Prediction Market Trade** | `POST /api/prediction/trade` | `{ market_id, option_index, amount }` → deep link | P1 |
| **Stablecoin Earn Rates** | `GET /api/earn/stablecoin` | `{ usdt_apy, usdc_apy, featured_product }` | P2 |
| **Fear & Greed Index** | `GET /api/sentiment/fear-greed` | `{ value: 0-100, label, change_24h }` | P2 |
| **Historical Event Impact** | `GET /api/analytics/event-impact?type=war` | `{ event_type, avg_oil_impact, avg_gold_impact, duration_days }` | P2 |

**API Contract Example:**
```typescript
// Geopolitical News Response
interface GeoNewsItem {
  id: string;
  title: string;              // "US launches third airstrike on Baghdad"
  title_zh: string;           // "美军对巴格达发起第三次空袭"
  summary: string;
  timestamp: number;          // Unix ms
  region: 'middle_east' | 'europe' | 'asia' | 'americas';
  severity: 'low' | 'medium' | 'high' | 'critical';
  assets_impacted: {
    asset: 'oil' | 'gold' | 'btc' | 'stocks' | 'meme';
    direction: 'up' | 'down' | 'neutral';
    magnitude: 1 | 2 | 3;     // Impact strength
  }[];
  source: string;
  source_url: string;
}

// Commodities Response
interface CommodityPrice {
  symbol: string;             // "BRENT", "WTI", "XAU"
  price: number;
  change_24h: number;
  change_pct: number;
  high_24h: number;
  low_24h: number;
  updated_at: number;
}
```

---

## 5. Deep Link Integration / 深度链接集成

BGW App URL Scheme for pre-filled trading:

```typescript
// lib/deeplink.ts

const BGW_SCHEME = 'bitgetwallet://';

export function generateTradeDeepLink(params: {
  type: 'swap' | 'futures' | 'stocks' | 'prediction' | 'earn';
  chain?: string;
  fromToken?: string;
  toToken?: string;
  amount?: string;
  direction?: 'long' | 'short';
  leverage?: number;
  symbol?: string;           // For stocks
  marketId?: string;         // For prediction
}) {
  const base = BGW_SCHEME;
  
  switch (params.type) {
    case 'swap':
      return `${base}swap?chain=${params.chain}&from=${params.fromToken}&to=${params.toToken}&amount=${params.amount}`;
    
    case 'futures':
      return `${base}futures/open?symbol=${params.symbol}&direction=${params.direction}&leverage=${params.leverage}&amount=${params.amount}`;
    
    case 'stocks':
      return `${base}stocks/trade?symbol=${params.symbol}&amount=${params.amount}`;
    
    case 'prediction':
      return `${base}prediction/trade?market=${params.marketId}`;
    
    case 'earn':
      return `${base}earn/stablecoin`;
    
    default:
      return `${base}home`;
  }
}

// Fallback for web: redirect to app store
export function openBGWApp(deepLink: string) {
  const timeout = setTimeout(() => {
    window.location.href = 'https://web3.bitget.com/download';
  }, 2000);
  
  window.location.href = deepLink;
  
  window.addEventListener('blur', () => clearTimeout(timeout));
}
```

---

## 6. Component Specifications / 组件规格

### 6.1 WarPulse (Hero Section)

```tsx
// components/WarPulse.tsx

interface WarPulseProps {
  assets: {
    oil: { price: number; change: number };
    gold: { price: number; change: number };
    defense: { symbol: string; change: number };  // e.g., LMT
    btc: { price: number; change: number };
  };
  headline: string;
  severity: 'low' | 'medium' | 'high' | 'critical';
}

// Visual:
// - Background: Dark map of Middle East with glow effects
// - 4 floating asset badges with real-time prices
// - Severity-based color scheme (green → yellow → red)
// - Animated pulse effect on critical events
```

### 6.2 EventRipple (Event Card with Impact)

```tsx
// components/EventRipple.tsx

interface EventRippleProps {
  event: GeoNewsItem;
  onTradeClick: (asset: string, direction: 'long' | 'short') => void;
}

// Visual:
// - Event title + timestamp
// - Expandable impact visualization:
//   🛢️ Oil ↑↑↑ (historical avg +4.2%, current +3.1%)
//   🥇 Gold ↑↑ (historical avg +1.8%, current +2.0%)
// - Quick trade buttons per asset
// - Meme radar link if relevant tokens detected
```

### 6.3 MiniTradePanel (Inline Trade Form)

```tsx
// components/MiniTradePanel.tsx

interface MiniTradePanelProps {
  asset: 'oil' | 'gold' | 'stock' | 'meme';
  symbol: string;
  currentPrice: number;
  onSubmit: (params: TradeParams) => void;  // Opens deep link
}

// Fields:
// - Direction: [Long] [Short] toggle
// - Leverage: 1x 2x 5x 10x selector (for futures)
// - Amount: Input with USDT denomination
// - Est. PnL: Dynamic calculation
// - CTA: "Open Position →" → triggers deep link
```

### 6.4 MemeRadar (War Narrative Tokens)

```tsx
// components/MemeRadar.tsx

interface MemeRadarProps {
  tokens: {
    address: string;
    symbol: string;
    name: string;
    chain: string;
    price: number;
    change_24h: number;
    liquidity: number;
    age_hours: number;
    safety: {
      is_honeypot: boolean;
      risk_level: 'safe' | 'caution' | 'danger';
      flags: string[];
    };
    lifecycle: 'emerging' | 'pumping' | 'peak' | 'declining';
  }[];
}

// Visual:
// - Token row: Icon | Symbol | Price | Change | Safety badge | Lifecycle indicator
// - Color coding: Green (safe) → Yellow (caution) → Red (danger)
// - "Trade" button per row → swap deep link
// - Filter tabs: All | New (<24h) | Hot | Risky
```

### 6.5 PredictionMarket + StrategyBuilder

```tsx
// components/PredictionMarket.tsx

interface PredictionQuestion {
  id: string;
  question: string;
  question_zh: string;
  options: { label: string; odds: number }[];
  volume: number;
  endTime: number;
}

interface LinkedStrategy {
  prediction_option: number;  // Which option user selected
  trades: {
    asset: string;
    direction: 'long' | 'short';
    rationale: string;
  }[];
}

// Flow:
// 1. User sees prediction question with odds
// 2. User selects an option (e.g., "Yes, military escalation")
// 3. StrategyBuilder shows recommended trades aligned with that prediction
// 4. "Apply Strategy" → opens BGW with all trades pre-filled
```

---

## 7. State Management / 状态管理

```typescript
// store/index.ts
import { create } from 'zustand';

interface AppState {
  // Theme
  theme: 'dark' | 'light';
  setTheme: (theme: 'dark' | 'light') => void;
  
  // Locale
  locale: 'en' | 'zh-CN';
  setLocale: (locale: 'en' | 'zh-CN') => void;
  
  // Live Data
  assets: {
    oil: CommodityPrice | null;
    gold: CommodityPrice | null;
    stocks: StockQuote[];
    memes: MemeToken[];
  };
  setAssets: (assets: Partial<AppState['assets']>) => void;
  
  // News
  news: GeoNewsItem[];
  setNews: (news: GeoNewsItem[]) => void;
  
  // Trade Intent (before deep link)
  tradeIntent: TradeParams | null;
  setTradeIntent: (intent: TradeParams | null) => void;
  
  // Predictions
  predictions: PredictionQuestion[];
  selectedPredictions: { [marketId: string]: number };  // option index
  setPredictions: (predictions: PredictionQuestion[]) => void;
  selectPrediction: (marketId: string, optionIndex: number) => void;
}
```

---

## 8. i18n Structure / 国际化结构

```
/messages/
├── en.json
└── zh-CN.json
```

```json
// messages/en.json
{
  "hero": {
    "title": "War Alpha",
    "subtitle": "Geopolitical events. Instant trades.",
    "live": "LIVE"
  },
  "assets": {
    "oil": "Brent Crude",
    "gold": "Gold",
    "stocks": "Defense Stocks",
    "crypto": "Crypto",
    "meme": "War Memes"
  },
  "trade": {
    "long": "Long",
    "short": "Short",
    "leverage": "Leverage",
    "amount": "Amount",
    "estPnl": "Est. PnL",
    "openPosition": "Open Position →",
    "swap": "Swap →"
  },
  "prediction": {
    "title": "Prediction Markets",
    "yes": "Yes",
    "no": "No",
    "odds": "Odds",
    "linkedStrategy": "If you bet {option}, consider:",
    "applyStrategy": "Apply Strategy →"
  },
  "meme": {
    "title": "War Narrative Memes",
    "new": "New",
    "hot": "Hot",
    "safe": "Safe",
    "caution": "Caution",
    "danger": "Danger",
    "honeypot": "Honeypot"
  },
  "earn": {
    "fearBanner": "Market fear rising — park funds in stablecoin earn",
    "apy": "APY up to {rate}%",
    "cta": "View Stablecoin Earn →"
  }
}
```

```json
// messages/zh-CN.json
{
  "hero": {
    "title": "战势指挥室",
    "subtitle": "地缘事件，即刻交易",
    "live": "实时"
  },
  "assets": {
    "oil": "布伦特原油",
    "gold": "黄金",
    "stocks": "军工股",
    "crypto": "加密货币",
    "meme": "战争Meme"
  },
  "trade": {
    "long": "做多",
    "short": "做空",
    "leverage": "杠杆",
    "amount": "金额",
    "estPnl": "预计盈亏",
    "openPosition": "立即开仓 →",
    "swap": "交易 →"
  },
  "prediction": {
    "title": "预测市场",
    "yes": "是",
    "no": "否",
    "odds": "赔率",
    "linkedStrategy": "若你押注「{option}」，建议：",
    "applyStrategy": "应用策略 →"
  },
  "meme": {
    "title": "战争叙事 Meme",
    "new": "新币",
    "hot": "热门",
    "safe": "安全",
    "caution": "谨慎",
    "danger": "危险",
    "honeypot": "蜜罐"
  },
  "earn": {
    "fearBanner": "市场恐慌加剧 — 稳定币理财年化最高",
    "apy": "年化最高 {rate}%",
    "cta": "查看稳定币理财 →"
  }
}
```

---

## 9. Theme Configuration / 主题配置

```css
/* globals.css */

:root {
  /* Dark theme (default) */
  --color-bg: #0a0a0f;
  --color-bg-secondary: #12121a;
  --color-bg-card: #1a1a24;
  --color-text: #ffffff;
  --color-text-secondary: #a0a0b0;
  --color-accent: #00d4aa;
  --color-danger: #ff4757;
  --color-warning: #ffa502;
  --color-success: #2ed573;
  --color-border: #2a2a3a;
  
  /* Asset colors */
  --color-oil: #ff6b35;
  --color-gold: #ffd700;
  --color-stocks: #4a90d9;
  --color-crypto: #f7931a;
  --color-meme: #9b59b6;
}

[data-theme="light"] {
  --color-bg: #f5f5f7;
  --color-bg-secondary: #ffffff;
  --color-bg-card: #ffffff;
  --color-text: #1a1a2e;
  --color-text-secondary: #6b6b80;
  --color-border: #e0e0e8;
}

/* Severity colors */
.severity-low { --severity-color: var(--color-success); }
.severity-medium { --severity-color: var(--color-warning); }
.severity-high { --severity-color: var(--color-danger); }
.severity-critical { 
  --severity-color: var(--color-danger);
  animation: pulse 1s infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.7; }
}
```

---

## 10. Implementation Phases / 实施阶段

### Phase 1: Foundation (Day 1-2) / 基础搭建
- [ ] Next.js 14 project setup with App Router
- [ ] Tailwind CSS + theme system (dark/light)
- [ ] i18n setup with next-intl
- [ ] Basic layout: Header, Footer, theme toggle, language switch
- [ ] Zustand store skeleton
- [ ] BGW Skill API wrapper (`lib/api/bgw-skill.ts`)

### Phase 2: Hero + News (Day 3-4) / 首屏 + 新闻
- [ ] WarPulse component with map background
- [ ] Mock data for 4 asset indicators
- [ ] EventTimeline with mock news
- [ ] EventRipple card with impact visualization
- [ ] Responsive layout (mobile + desktop)

### Phase 3: Asset Trading (Day 5-6) / 资产交易
- [ ] AssetCommandTabs (5 tabs)
- [ ] MiniTradePanel with deep link generation
- [ ] Integration with real APIs:
  - Meme: bitget-wallet-skill
  - Others: Mock until backend ready
- [ ] MemeRadar with safety badges (using `audit` API)

### Phase 4: Prediction + Strategy (Day 7-8) / 预测 + 策略
- [ ] PredictionMarket component
- [ ] StrategyBuilder (auto-generate trades from prediction)
- [ ] StablecoinBanner (fear mode trigger)
- [ ] Deep link for prediction market

### Phase 5: Polish + Share (Day 9-10) / 打磨 + 分享
- [ ] ShareCard (screenshot generation for social)
- [ ] PWA manifest
- [ ] Performance optimization (lazy loading, code splitting)
- [ ] Cross-browser testing
- [ ] Mobile gesture support

---

## 11. Testing Checklist / 测试清单

- [ ] Theme: Dark/Light toggle persists across sessions
- [ ] i18n: All strings render correctly in en/zh-CN
- [ ] Responsive: No horizontal scroll on mobile
- [ ] Deep links: Open BGW app with correct pre-filled params
- [ ] Fallback: App store redirect if BGW not installed
- [ ] API: Graceful error handling for failed requests
- [ ] Meme safety: Honeypot warnings display correctly
- [ ] Real-time: Prices update every 5-10 seconds
- [ ] Share: Screenshot captures current state correctly

---

## 12. Deliverables / 交付物

1. **Source Code** — GitHub repo with clean structure
2. **Vercel Deploy** — Preview URL for testing
3. **API Contract Doc** — For backend team (missing APIs)
4. **Deep Link Spec** — For BGW App team integration
5. **Design Tokens** — Exportable theme variables

---

## 13. Dependencies / 依赖

```json
{
  "dependencies": {
    "next": "^14.1.0",
    "react": "^18.2.0",
    "tailwindcss": "^3.4.0",
    "zustand": "^4.5.0",
    "next-intl": "^3.5.0",
    "bitget-wallet-skill": "^2026.3.5",
    "lightweight-charts": "^4.1.0",
    "lucide-react": "^0.300.0",
    "clsx": "^2.1.0"
  }
}
```

---

## 14. Open Questions / 待确认问题

1. **News Source** — RSS? Proprietary API? Real-time websocket?
2. **Commodities Data** — Use TradingView widgets or proprietary feed?
3. **Prediction Market** — Which prediction market protocol to integrate?
4. **US Stocks** — BGW stocks API available? Or third-party?
5. **Deep Link Scheme** — Confirm `bitgetwallet://` URL scheme spec with App team
6. **Analytics** — Mixpanel? Amplitude? Custom?

---

*Document generated: 2026-03-08*
*Author: Jarvis (codingdad agent)*

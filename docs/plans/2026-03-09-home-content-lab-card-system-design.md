# Home Content Lab Card System Design

**Date:** 2026-03-09
**Status:** Approved

## Goal
为 `home-content-lab.html` 建立统一的图片卡片视觉系统：在保留单列时间流结构的前提下，让每类内容都拥有可识别的图形语言，并提升整页卡片的一体感、完成度与产品化质感。

## Direction
采用 **C / 折中方案**：
- **底层统一为内容平台感**：统一卡片骨架、构图、信息层级、边界与光感
- **局部保留社媒热榜冲击**：KOL / Macro / Narrative 保留更强的刷屏感、趋势感、breaking 感

## Core Principles
1. **统一骨架优先于单卡炫技**：所有图片卡共享同一套 card shell 语言
2. **图形必须与内容相关**：不能只是装饰，要让用户一眼知道内容类型
3. **避免重复信息**：封面负责主题，标题负责导语，meta/pill 负责补充
4. **像成熟产品，不像拼贴海报**：增强系统感、减少随意感

## Card System
### Shared visual shell
所有图片卡统一补强：
- 统一的 media 内层边界与高光
- 更稳定的 overlay / vignette / glow 结构
- 更一致的 chip / badge / text anchor
- 卡片外层增加更完整的色彩壳体与阴影层级

### Type-specific graphics
- **Ops / 推荐**：编辑框线、内容模块、精选封面编排感
- **KOL**：名字云 + 传播波纹 + 热榜层次
- **AI**：信号线、节点、轨迹、数据扫描感
- **Macro**：地图/冲击波/告警线/爆点光斑
- **Narrative**：话题轨道、热度环、社区扩散点
- **Earn**：收益曲线、增长柱、资产层叠
- **Pay / USDT**：支付路径、结算箭头、转账节点
- **Promo / Feature**：专题封面框架、栏目化编辑元素

## Success Criteria
- 用户能快速区分不同内容类型
- 所有图片卡看起来属于同一个产品系统
- KOL / Macro / Narrative 仍保有更强冲击力
- 整页比当前版本更完整、更像真实钱包首页内容流

## Scope
本轮只改：
- `home-content-lab.html` 的 card-related CSS / HTML 结构
- 不改信息流排序逻辑
- 不新增复杂交互
- 不接入真实图片素材

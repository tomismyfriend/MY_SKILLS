---
name: bazi-fortune
description: "Use when the user asks for Bazi (八字) fortune-telling, destiny analysis, or four-pillars reading. Covers career, wealth, marriage, health, and life-cycle analysis based on classical Chinese Bazi texts (穷通宝鉴, 滴天髓, 子平真诠, 三命通会). Trigger on keywords: 八字, 算命, 命理, 四柱, 断命, 排盘, 大运, 流年, 事业运, 财运, 婚姻."
---

# 八字命理分析 (Bazi Fortune-Telling)

## Corpus Reference

All source texts are GBK-encoded classical Chinese. Read with `encoding='gbk'` in Python.

| File | Role in Analysis |
|------|-----------------|
| `穷通宝鉴.txt` | **Primary**: Seasonal adjustment — determines 用神 (useful god) by day-stem × month-branch |
| `滴天髓.txt` | **Theory**: Core principles of 天干 properties, 体用, 顺逆, 衰旺 |
| `子平真诠评注/zzbj_zipingzqpz01.txt`–`10.txt` | **Structure**: 格局 determination, 用神成败救应, 刑冲会合 |
| `《三命通会》作者：明·万民英（完结）.txt` | **Reference**: 纳音, 日柱断法, 时辰断法, 神煞 |

## Analysis Methodology (8 Steps)

### Step 1: 排盘 (Chart Construction)

Convert birth data (year, month, day, hour) into four pillars (四柱):

```
年柱 | 月柱 | 日柱 | 时柱
天干  | 天干  | 天干(日主) | 天干
地支  | 地支  | 地支       | 地支
```

- Year pillar: Use 立春 as year boundary (not Jan 1 or lunar new year)
- Month pillar: Determine by solar term (节气); month stem follows 年上起月 rule
- Day pillar: Look up in 万年历 or calculate
- Hour pillar: Follow 日上起时 rule; note 子时 (23:00-01:00) belongs to next day

List the 藏干 (hidden stems) in each earthly branch:

| 支 | 藏干 | 支 | 藏干 | 支 | 藏干 | 支 | 藏干 |
|----|------|----|------|----|------|----|------|
| 子 | 癸 | 卯 | 乙 | 午 | 丁己 | 酉 | 辛 |
| 丑 | 己癸辛 | 辰 | 戊乙癸 | 未 | 己丁乙 | 戌 | 戊辛丁 |
| 寅 | 甲丙戊 | 巳 | 丙戊庚 | 申 | 庚壬戊 | 亥 | 壬甲 |

### Step 2: 定日主 (Identify Day Master)

The day-stem (日干) is the self. Determine its fundamental nature from 《滴天髓》:

- **甲木**: 参天大树，脱胎要火，春不容金，秋不容土
- **乙木**: 虽柔，刲羊解牛，怀丁抱丙，跨凤乘猴
- **丙火**: 猛烈，欺霜侮雪，能煅庚金，从辛反怯
- **丁火**: 柔中，内性昭融，抱乙而孝，合壬而忠
- **戊土**: 固重，既中且正，静翕动辟，万物司命
- **己土**: 卑湿，中正蓄藏，不愁木盛，不畏水狂
- **庚金**: 带煞，刚健为最，得水而清，得火而锐
- **辛金**: 软弱，温润而清，畏土之多，乐水之盈
- **壬水**: 通河，能泄金气，刚中之德，周流不滞
- **癸水**: 至弱，达于天津，得龙而运，功化斯神

### Step 3: 察衰旺 (Assess Strength)

Evaluate day-master strength using these criteria (from 《滴天髓·理气》and 《子平真诠》):

1. **得令?** — Is day-stem in season per month-branch?
   - 旺: 当令 (e.g., 木生寅卯月)
   - 相: 进气 (e.g., 木生亥子月)
   - 休: 退气 (e.g., 木生巳午月)
   - 囚: 受克 (e.g., 木生申酉月)
   - 死: 最弱 (e.g., 木生辰戌丑未月)

2. **得地?** — Does day-stem have root (通根) in any branch?
   - 禄 (临官), 旺 (帝旺), 长生, 墓库 count as roots
   - Strength order: 禄 > 旺 > 长生 > 余气 > 墓库

3. **得势?** — Are there supporting stems (比劫/印)?

4. **Conclusion**: 身旺, 身弱, 从格, or 化格

**Key rule from 《滴天髓》**: "五阳从气不从势，五阴从势无情义" — Yang stems resist following unless truly rootless; Yin stems follow prevailing force more readily.

### Step 4: 定格局 (Determine Pattern)

From 《子平真诠》: "用神专寻月令"

1. Identify the **月令司令之神** (ruling god of the month branch)
2. Check which 十神 (ten god) it represents relative to day-stem
3. Determine if it透干 (emerges in stem) — if yes, that's the primary pattern

**Eight main patterns (八格)**:

| 月令十神 | 格局名 | 喜 | 忌 |
|----------|--------|-----|-----|
| 正官 | 正官格 | 财印相辅 | 伤官、刑冲 |
| 偏官(七煞) | 七煞格 | 食神制、印化 | 财星党煞 |
| 正财 | 正财格 | 食伤生、官护 | 比劫夺 |
| 偏财 | 偏财格 | 食伤生 | 比劫夺 |
| 正印 | 正印格 | 官煞生、食泄 | 财星破印 |
| 偏印(枭) | 偏印格 | 官煞生 | 财星破 |
| 食神 | 食神格 | 生财、制煞 | 枭印夺食 |
| 伤官 | 伤官格 | 生财、佩印 | 见官(非金水) |

**Special patterns**: 建禄格, 月劫格, 阳刃格 — 禄劫本身不能为用，须另取扶抑之神。

**Pattern success/failure (成败救应)** from 《子平真诠·论用神成败救应》:
- 成: Pattern has proper support and no破坏
- 败: Pattern is damaged by忌神
- 救应: A damaging element is itself neutralized

### Step 5: 取用神 (Determine Useful God)

**Primary method — 穷通宝鉴 seasonal lookup**:

Search `穷通宝鉴.txt` for the section matching "[month name][day-stem]" (e.g., "十月戊土"). This gives:
- 第一用神 (primary useful god)
- 第二用神 (secondary)
- 忌神 (taboo elements)
- Example命造 with outcomes

**Key seasonal principles**:

| 日主 | 春 | 夏 | 秋 | 冬 |
|------|------|------|------|------|
| 甲乙木 | 喜火暖，忌水盛 | 喜水盛，忌火旺 | 喜金制，喜水润 | 喜火暖，忌水泛 |
| 丙丁火 | 喜木生，壬辅映 | 喜壬水，忌木多 | 喜甲木引丁 | 喜甲木，忌水多 |
| 戊己土 | 喜甲疏，丙暖 | 喜水润，忌火燥 | 喜甲丙 | 喜丙暖，甲疏 |
| 庚辛金 | 喜丁火，壬泄 | 喜壬水，忌火旺 | 喜壬泄，丁炼 | 喜丙暖，忌水多 |
| 壬癸水 | 喜戊制，辛发源 | 喜庚辛生 | 喜甲泄，丙暖 | 喜戊制，丙暖 |

**Secondary method — 扶抑/调候/通关**:
- 扶抑: Support weak day-master or restrain strong one
- 调候: Adjust for seasonal extremes (冬用火, 夏用水)
- 通关: Mediate between conflicting elements

### Step 6: 论十神配置 (Ten Gods Configuration)

Map all stems and hidden stems to ten gods relative to day-master:

| 关系 | 同性(阳见阳/阴见阴) | 异性(阳见阴/阴见阳) |
|------|---------------------|---------------------|
| 生我 | 偏印(枭) | 正印 |
| 我生 | 食神 | 伤官 |
| 克我 | 偏官(七煞) | 正官 |
| 我克 | 偏财 | 正财 |
| 同我 | 比肩 | 劫财 |

**Career indicators**:
- 正官/偏官旺而有制 → 仕途、管理、权力
- 食神/伤官旺 → 技术、创意、自由职业、艺术
- 正财/偏财旺 → 商业、金融、财务
- 正印/偏印旺 → 学术、教育、研究
- 比肩/劫财旺 → 合伙、竞争、体力劳动

**Wealth indicators**:
- 财星为用且有力 → 富
- 食伤生财 → 以才华致富
- 财旺身弱 → 富屋贫人(有机会但难把握)
- 身旺财弱 → 有能无财

### Step 7: 论刑冲会合 (Interactions)

From 《子平真诠·论刑冲会合解法》:

**六冲** (most important): 子午、丑未、寅申、卯酉、辰戌、巳亥
- 冲 = 克, 动摇、变化、分离
- 旺冲衰 = 拔; 衰冲旺 = 激怒

**六合**: 子丑、寅亥、卯戌、辰酉、巳申、午未
- 合 = 牵绊、结合、化解

**三合局**: 申子辰(水)、亥卯未(木)、寅午戌(火)、巳酉丑(金)
- 会局力量大于六合

**三会方**: 寅卯辰(东方木)、巳午未(南方火)、申酉戌(西方金)、亥子丑(北方水)
- 力量最大

**Key rule**: 会合可以解冲，冲也可以解会合。须看力量与位置。

### Step 8: 排大运 (Major Life Cycles)

**Direction**:
- 男命 + 阳年干(甲丙戊庚壬) → 顺排
- 男命 + 阴年干(乙丁己辛癸) → 逆排
- 女命相反

**Starting age**: Count days from birth to next/previous 节气, divide by 3 = starting age

**Each 大运 lasts 10 years**, progressing through the stems/branches sequentially from month pillar.

**大运 evaluation**:
- 大运干支对用神/忌神的关系决定该十年吉凶
- 地支重于天干 (地支管后5年，天干管前5年)
- 运至用神之地 → 发达; 运至忌神之地 → 困顿

**流年 (annual fortune)**: Current year's stem-branch interacts with natal chart and current 大运.

## Output Format

When presenting analysis, structure as:

```
## 八字排盘
[Four pillars table with 天干/地支/藏干/十神/纳音]

## 一、日主分析
[Day master nature, strength assessment]

## 二、格局判定
[Pattern identification with classical text citation]

## 三、用神取用
[Useful god determination, citing 穷通宝鉴 seasonal analysis]

## 四、[Specific topic]分析
[Ten gods configuration relevant to the question]

## 五、大运走势
[Major life cycles with turning points]

## 六、建议
[Practical guidance: direction, industry, timing]
```

## Search & Lookup System

### Universal Read Command Template

All file reads MUST use this pattern (GBK → UTF-8):

```python
python -c "
import sys
sys.stdout.reconfigure(encoding='utf-8')
with open('FILE.txt', 'r', encoding='gbk') as f:
    lines = f.read().split('\n')
    for i in range(START-1, END):
        print(lines[i])
"
```

Replace `FILE.txt`, `START`, `END` with values from the index tables below.

---

### Book 1: 穷通宝鉴 — Complete Section Index (4576 lines)

**This is the PRIMARY lookup. Always check here first for 用神 determination.**

Structure: Organized by `[month][day-stem]` (e.g., "十月戊土"). Each section gives: 用神 priority, seasonal reasoning, example命造 with outcomes.

#### Quick Lookup Table: Line Numbers by Day-Stem × Month

**甲木 (Jia Wood)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正二月 | 171 | 正二月甲木一例 |
| 三月 | 225 | 三月甲木 |
| 四月 | 267 | 四月甲木 |
| 五六月 | 290 | 五六月甲木一例 |
| 七月 | 346 | 七月甲木 |
| 八月 | 381 | 八月甲木 |
| 九月 | 415 | 九月甲木 |
| 十月 | 478 | 十月甲木 |
| 十一月 | 532 | 十一月甲木 |
| 十二月 | 575 | 十二月甲木 |

**乙木 (Yi Wood)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正月 | 620 | 正月乙木 |
| 二月 | 666 | 二月乙木 |
| 三月 | 704 | 三月乙木 |
| 四月 | 747 | 四月乙木 |
| 五月 | 769 | 五月乙木 |
| 六月 | 789 | 六月乙木 |
| 七月 | 832 | 七月乙木 |
| 八月 | 860 | 八月乙木 |
| 九月 | 921 | 九月乙木 |
| 十月 | 968 | 十月乙木 |
| 十一月 | 1022 | 十一月乙木 |
| 十二月 | 1048 | 十二月乙木 |

**丙火 (Bing Fire)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正月 | 1107 | 正月丙火 |
| 二月 | 1193 | 二月丙火 |
| 三月 | 1226 | 三月丙火 |
| 四月 | 1276 | 四月丙火 |
| 五月 | 1316 | 五月丙火 |
| 六月 | 1358 | 六月丙火 |
| 七月 | 1401 | 七月丙火 |
| 八月 | 1431 | 八月丙火 |
| 九月 | 1471 | 九月丙火 |
| 十月 | 1509 | 十月丙火 |
| 十一月 | 1545 | 十一月丙火 |
| 十二月 | 1588 | 十二月丙火 |

**丁火 (Ding Fire)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正月 | 1633 | 正月丁火 |
| 二月 | 1687 | 二月丁火 |
| 三月 | 1736 | 三月丁火 |
| 四月 | 1766 | 四月丁火 |
| 五月 | 1803 | 五月丁火 |
| 六月 | 1843 | 六月丁火 |
| 七月 | 1901 | 七月丁火 |
| 八月 | 1915 | 八月丁火 |
| 九月 | 1940 | 九月丁火 |
| 十月 | 1987 | 十月丁火 |
| 十一月 | 2022 | 十一月丁火 |
| 十二月 | 2045 | 十二月丁火 |

**戊土 (Wu Earth)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正二三月 | 2076 | 正二三月戊土一例 |
| 正月 | 2102 | 正月戊土 |
| 二月 | 2125 | 二月戊土 |
| 三月 | 2156 | 三月戊土 |
| 四月 | 2186 | 四月戊土 |
| 五月 | 2210 | 五月戊土 |
| 六月 | 2249 | 六月戊土 |
| 七月 | 2285 | 七月戊土 |
| 八月 | 2318 | 八月戊土 |
| 九月 | 2342 | 九月戊土 |
| 十月 | 2390 | 十月戊土 |
| 十一二月 | 2431 | 十一、二月戊土总论 |
| 十一月 | 2447 | 十一月戊土 |
| 十二月 | 2461 | 十二月戊土 |

**己土 (Ji Earth)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正月 | 2492 | 正月己土 |
| 二月 | 2520 | 二月己土 |
| 三月 | 2557 | 三月己土 |
| 四月 | 2591 | 四月己土 |
| 五月 | 2634 | 五月己土 |
| 六月 | 2654 | 六月己土 |
| 七月 | 2698 | 七月己土 |
| 八月 | 2709 | 八月己土 |
| 九月 | 2717 | 九月己土 |
| 十月 | 2751 | 十月己土 (总论起) |
| 十一月 | 2782 | 十一月己土 |
| 十二月 | 2801 | 十二月己土 |

**庚金 (Geng Metal)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正月 | 2842 | 正月庚金 |
| 二月 | 2888 | 二月庚金 |
| 三月 | 2925 | 三月庚金 |
| 四月 | 2959 | 四月庚金 |
| 五月 | 2991 | 五月庚金 |
| 六月 | 3021 | 六月庚金 |
| 七月 | 3054 | 七月庚金 |
| 八月 | 3079 | 八月庚金 |
| 九月 | 3133 | 九月庚金 |
| 十月 | 3171 | 十月庚金 |
| 十一月 | 3201 | 十一月庚金 |
| 十二月 | 3235 | 十二月庚金 |

**辛金 (Xin Metal)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正月 | 3269 | 正月辛金 |
| 二月 | 3292 | 二月辛金 |
| 三月 | 3340 | 三月辛金 |
| 四月 | 3361 | 四月辛金 |
| 五月 | 3383 | 五月辛金 |
| 六月 | 3398 | 六月辛金 |
| 七月 | 3427 | 七月辛金 |
| 八月 | 3449 | 八月辛金 |
| 九月 | 3500 | 九月辛金 |
| 十月 | 3551 | 十月辛金 |
| 十一月 | 3584 | 十一月辛金 |
| 十二月 | 3627 | 十二月辛金 |

**壬水 (Ren Water)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正月 | 3668 | 正月壬水 |
| 二月 | 3739 | 二月壬水 |
| 三月 | 3773 | 三月壬水 |
| 四月 | 3803 | 四月壬水 |
| 五月 | 3840 | 五月壬水 |
| 六月 | 3865 | 六月壬水 |
| 七月 | 3889 | 七月壬水 |
| 八月 | 3926 | 八月壬水 |
| 九月 | 3960 | 九月壬水 |
| 十月 | 3996 | 十月壬水 |
| 十一月 | 4041 | 十一月壬水 |
| 十二月 | 4084 | 十二月壬水 |

**癸水 (Gui Water)**:

| Month | Line | Section Title |
|-------|------|---------------|
| 正月 | 4142 | 正月癸水 |
| 二月 | 4169 | 二月癸水 |
| 三月 | 4210 | 三月癸水 |
| 四月 | 4244 | 四月癸水 |
| 五月 | 4280 | 五月癸水 |
| 六月 | 4310 | 六月癸水 |
| 七月 | 4327 | 七月癸水 |
| 八月 | 4359 | 八月癸水 |
| 九月 | 4391 | 九月癸水 |
| 十月 | 4429 | 十月癸水 |
| 十一月 | 4467 | 十一月癸水 |
| 十二月 | 4510 | 十二月癸水 |

**Usage**: To read a section, use the line number as START and read ~40 lines (or until the next section). Example: 十月戊土 starts at line 2390, read to ~2430.

---

### Book 2: 滴天髓 — Chapter Index (2354 lines)

**Use for: 天干性情, 体用理论, 衰旺判断, 中和之理, 六亲论断, 富贵贫贱判断**

| Ch# | Line | Title | Key Content |
|-----|------|-------|-------------|
| 1 | 3 | 天道 | 三元万法宗，帝载与神功 |
| 2 | 10 | 地道 | 五气偏全定吉凶 |
| 3 | 17 | 人道 | 戴天履地，顺吉凶悖 |
| 4 | 24 | 知命 | 顺逆之机，用神根源 |
| 5 | 46 | 理气 | 进退之机，旺相休囚 |
| 6 | 59 | 配合 | 干支配合仔细详 |
| 7 | 73 | 天干 | **十天干性情总论** (甲–癸完整论述) |
| 8 | 124 | 地支 | 阳支阴支，动静之别 |
| 9 | 197 | 干支总论 | 干支关系深层理论 |
| 10 | 331 | 两气合象 | 两气成象不可破 |
| 11 | 421 | 方局 | 方是方，局是局 |
| 12 | 478 | 八格 | 财官印绶食伤八格定 |
| 13 | 517 | 体用 | **体用论** — 扶之抑之得其宜 |
| 14 | 536 | 精神 | 损之益之得其中 |
| 15 | 554 | 月令 | 月令理论 |
| 16 | 563 | 生时 | 时辰理论 |
| 17 | 573 | 衰旺 | **衰旺真机** — 三命之奥 |
| 18 | 641 | 中和 | 中和正理 |
| 19 | 654 | 源流 | 根源流注 |
| 20 | 674 | 通关 | 关内织女关外牛郎 |
| 21 | 693 | 官杀混杂 | 有可有不可 |
| 22 | 807 | 伤官见官 | 可见不可见 |
| 23–26 | 870–980 | 清浊/真假/恩怨/闲神 | 格局清浊论 |
| 27 | 981 | 刚柔 | 引其性情 |
| 28 | 1000 | 顺逆 | 气势不可逆 |
| 29 | 1016 | 寒暖 | 天道寒暖，不可过 |
| 30 | 1035 | 燥湿 | 地道燥湿，不可偏 |
| 31 | 1054 | 露藏 | 吉神太露，凶物深藏 |
| 32 | 1067 | 众寡 | 强众敌寡，强寡敌众 |
| 33 | 1083 | 震兑 | 仁义之真机 |
| 34 | 1106 | 坎离 | 天地之中气 |
| 35 | 1126 | 夫妻 | **论婚姻** |
| 36 | 1142 | 子女 | **论子女** |
| 37 | 1171 | 父母 | 岁月所关 |
| 38 | 1190 | 兄弟 | 提用财神看重轻 |
| 39 | 1203 | 富 | **何知其人富？财气通门户** |
| 40 | 1280 | 贵 | 何知其人贵 |
| 41 | 1370 | 贫贱寿夭 | 贫贱寿夭判断 |
| 42 | 1453 | 德才 | 君子之风 vs 多能之象 |
| 43 | 1469 | 奋郁 | 奋发之机 vs 沉埋之气 |
| 44 | 1500 | 恩怨 | 恩怨论 |
| 45 | 1540 | 征验 | 征验论 |
| 46 | 1560 | 贞元 | 贞元之理 |
| 47 | 1580 | 化象 | **化格论** — 化得真者只论化 |
| 48 | 1602 | 从象 | **从格论** — 真从 vs 假从 |
| 49 | 1624 | 假化 | 假化之人亦多贵 |
| 50 | 1660 | 顺局 | 顺局论 |
| 51 | 1700 | 反局 | 反局论 |
| 52 | 1749 | 战和 | 天战地战 |
| 53 | 1774 | 合宜 | 合有宜不宜 |
| 54 | 1802 | 君臣 | 君不可抗 |
| 55 | 1815 | 臣过 | 臣不可过 |
| 56 | 1834 | 母慈 | 慈母恤孤 |
| 57 | 1847 | 子孝 | 孝子奏亲 |
| 58 | 1860 | 性情 | 五气中和 vs 偏枯 |
| 59 | 2016 | 疾病 | **五行和者一世无灾** |
| 60 | 2126 | 出身 | 科第玄机 |
| 61 | 2230 | 地位 | 台阁勋劳 |
| 62 | 2302 | 岁运 | **大运流年论** |
| 63 | 2348 | 元会 | 造化起于元 |

**Usage**: For 天干性情, read lines 73–123 (Ch.7). For 体用, read lines 517–535 (Ch.13). For 衰旺, read lines 573–640 (Ch.17). For 从格, read lines 1602–1623 (Ch.48). For 富, read lines 1203–1279 (Ch.39).

---

### Book 3: 子平真诠评注 — Chapter & Section Index

**Use for: 格局确定, 用神成败, 刑冲会合规则, 行运论断, 各格局详细取运法**

Split into 10 files (`zzbj_zipingzqpz01.txt` to `10.txt`). Section numbers are continuous across files.

| File | Sections | Key Topics |
|------|----------|------------|
| 01 | — | 序言、目录 |
| 02 | 一–三 | 十干十二支、阴阳生克、阴阳生死 |
| 03 | 四–六 | 十干配合性情、十干合而不合、得时不旺失时不弱 |
| 04 | 七–八 | **刑冲会合解法**、**用神** |
| 05 | 九–十三 | **用神成败救应**、用神变化、用神纯杂、格局高低、因成得败 |
| 06 | 十四–二十一 | 用神配气候、相神紧要、杂气取用、墓库刑冲、四吉神破格、四凶神成格、生克先后、星辰无关格局 |
| 07 | 二十二–三十一 | 外格用舍、宫分配六亲、**论妻子**、**论行运**、行运成格变格、喜忌干支有别、支中喜忌逢运透清、时说拘泥格局、时说以讹传讹、**论正官** |
| 08 | 三十二–三十六 | 正官取运、**论财**、财取运、**论印绶**、印绶取运 |
| 09 | 三十七–四十二 | **论食神**、食神取运、**论偏官**、偏官取运、**论伤官**、伤官取运 |
| 10 | 四十三–四十八 | **论阳刃**、阳刃取运、**论建禄月劫**、建禄月劫取运、论杂格、杂格取运 |

**Key section lookup for pattern analysis**:

| Pattern | Section | File | Line |
|---------|---------|------|------|
| 用神总论 | 八 | 04 | 161 |
| 成败救应 | 九 | 05 | 9 |
| 格局高低 | 十二 | 05 | 205 |
| 用神配气候(调候) | 十四 | 06 | 9 |
| 相神 | 十五 | 06 | 29 |
| 杂气取用 | 十六 | 06 | 45 |
| 行运 | 二十五 | 07 | 44 |
| 正官格 | 三十一 | 07 | 225 |
| 财格 | 三十三 | 08 | 59 |
| 印绶格 | 三十五 | 08 | 179 |
| 食神格 | 三十七 | 09 | 9 |
| 偏官(七煞)格 | 三十九 | 09 | 117 |
| 伤官格 | 四十一 | 09 | 200 |
| 阳刃格 | 四十三 | 10 | 9 |
| 建禄月劫格 | 四十五 | 10 | 61 |

---

### Book 4: 三命通会 — Section Index (8796 lines)

**Use for: 纳音五行, 六十甲子性质, 日柱断法, 时辰断法, 神煞查法**

#### 卷次结构

| 卷 | Line | Content |
|----|------|---------|
| 卷一 | 13 | 五行生成、生克、支干源流、纳音总论、六十甲子纳音取象 |
| 卷二 | 1078 | 天干阴阳生死、地支、十干分配天文、十二支分配地理、人元司事、四时节气、五行旺相休囚死、遁月时、年月日时、胎元、坐命宫 |
| 卷三 | 1988 | 十干合、进交退、十干化气、支元六合、支元三合、将星华盖、咸池、六害、三刑、冲击 |
| 卷四 | 2878 | 十干坐支兼得月时及行运吉凶、十二月支得日干吉凶 |
| 卷五 | 3493 | 五行时地分野吉凶、古人立印食官财名义、正官、天福贵人、天元作禄等格局 |
| 卷六 | 3891 | 偏官、正印、偏印、正财、偏财、食神、伤官等十神格局详细论述 |
| 卷七 | 5280 | 论女命、论小儿、论六亲、疾病等专题 |
| 卷八 | 5829 | **日时断法** — 六甲日至六癸日各时辰断语 (lines 5830–8796) |
| 卷九 | 7313 | 六己日–六癸日时辰断法续 |

#### 日柱×时辰断法速查表 (卷八, lines 5830–8796)

Each day-stem has 12 hour entries. Find the intersection of day-stem and hour-branch:

**六甲日 (Jia day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 甲子时 | 5830 | 庚午时 | 5992 |
| 乙丑时 | 5865 | 辛未时 | 6015 |
| 丙寅时 | 5888 | 壬申时 | 6034 |
| 丁卯时 | 5917 | 癸酉时 | 6058 |
| 戊辰时 | 5943 | 甲戌时 | 6079 |
| 己巳时 | 5966 | 乙亥时 | 6102 |

**六乙日 (Yi day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 丙子时 | 6127 | 壬午时 | 6271 |
| 丁丑时 | 6153 | 癸未时 | 6294 |
| 戊寅时 | 6178 | 甲申时 | 6317 |
| 己卯时 | 6201 | 乙酉时 | 6340 |
| 庚辰时 | 6224 | 丙戌时 | 6365 |
| 辛巳时 | 6246 | 丁亥时 | 6392 |

**六丙日 (Bing day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 戊子时 | 6416 | 甲午时 | 6571 |
| 己丑时 | 6447 | 乙未时 | 6596 |
| 庚寅时 | 6470 | 丙申时 | 6619 |
| 辛卯时 | 6496 | 丁酉时 | 6643 |
| 壬辰时 | 6522 | 戊戌时 | 6669 |
| 癸巳时 | 6547 | 己亥时 | 6698 |

**六丁日 (Ding day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 庚子时 | 6723 | 丙午时 | 6869 |
| 辛丑时 | 6750 | 丁未时 | 6893 |
| 壬寅时 | 6775 | 戊申时 | 6917 |
| 癸卯时 | 6799 | 己酉时 | 6937 |
| 甲辰时 | 6823 | 庚戌时 | 6958 |
| 乙巳时 | 6847 | 辛亥时 | 6984 |

**六戊日 (Wu day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 壬子时 | 7009 | 戊午时 | 7170 |
| 癸丑时 | 7040 | 己未时 | 7196 |
| 甲寅时 | 7066 | 庚申时 | 7220 |
| 乙卯时 | 7095 | 辛酉时 | 7244 |
| 丙辰时 | 7119 | 壬戌时 | 7267 |
| 丁巳时 | 7143 | 癸亥时 | 7288 |

**六己日 (Ji day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 甲子时 | 7314 | 庚午时 | 7463 |
| 乙丑时 | 7343 | 辛未时 | 7485 |
| 丙寅时 | 7365 | 壬申时 | 7509 |
| 丁卯时 | 7388 | 癸酉时 | 7533 |
| 戊辰时 | 7413 | 甲戌时 | 7558 |
| 己巳时 | 7436 | 乙亥时 | 7587 |

**六庚日 (Geng day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 丙子时 | 7615 | 壬午时 | 7765 |
| 丁丑时 | 7645 | 癸未时 | 7788 |
| 戊寅时 | 7666 | 甲申时 | 7811 |
| 己卯时 | 7690 | 乙酉时 | 7834 |
| 庚辰时 | 7713 | 丙戌时 | 7858 |
| 辛巳时 | 7740 | 丁亥时 | 7882 |

**六辛日 (Xin day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 戊子时 | 7905 | 甲午时 | 8062 |
| 己丑时 | 7937 | 乙未时 | 8085 |
| 庚寅时 | 7960 | 丙申时 | 8107 |
| 辛卯时 | 7982 | 丁酉时 | 8132 |
| 壬辰时 | 8009 | 戊戌时 | 8155 |
| 癸巳时 | 8037 | 己亥时 | 8178 |

**六壬日 (Ren day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 庚子时 | 8203 | 丙午时 | 8359 |
| 辛丑时 | 8235 | 丁未时 | 8382 |
| 壬寅时 | 8259 | 戊申时 | 8402 |
| 癸卯时 | 8288 | 己酉时 | 8426 |
| 甲辰时 | 8312 | 庚戌时 | 8449 |
| 乙巳时 | 8337 | 辛亥时 | 8474 |

**六癸日 (Gui day)**:

| 时辰 | Line | 时辰 | Line |
|------|------|------|------|
| 壬子时 | 8497 | 戊午时 | 8640 |
| 癸丑时 | 8525 | 己未时 | 8661 |
| 甲寅时 | 8549 | 庚申时 | 8683 |
| 乙卯时 | 8575 | 辛酉时 | 8709 |
| 丙辰时 | 8598 | 壬戌时 | 8732 |
| 丁巳时 | 8619 | 癸亥时 | 8756 |

#### 神煞速查 (卷二–三)

| 神煞 | Line | 神煞 | Line |
|------|------|------|------|
| 驿马 | 2085 | 天乙贵人 | 2226 |
| 三奇 | 2309 | 天月德 | 2352 |
| 学堂词馆 | 2420 | 正印 | 2448 |
| 劫煞亡神 | 2494 | 羊刃 | 2516 |
| 空亡 | 2531 | 元辰 | 2610 |
| 孤辰寡宿 | 2689 | 天罗地网 | 2722 |
| 十恶大败 | 2743 | 三刑 | 1889 |
| 六害 | 1852 | 冲击 | 1960 |
| 将星华盖 | 1826 | 咸池(桃花) | 1840 |

#### 六十甲子纳音性质 (卷一, lines 195–500)

Search for specific 纳音 by day-pillar: e.g., `戊寅` → search for `戊寅` in lines 195–500 for 纳音五行 and 性质吉凶.

---

### Recommended Search Order for Any Chart

Given a chart with day-stem `X`, month-branch `Y`, day-pillar `XY`, hour `Z`:

1. **穷通宝鉴** (用神): Look up the table above for `[month][X]` → read ~40 lines from that line number
2. **三命通会·时辰断** (日时断): Look up `[六X日][Z时]` in the day×hour table → read ~20 lines
3. **三命通会·纳音** (纳音): Search for day-pillar `XY` in lines 195–500
4. **滴天髓** (日主性情): Read Ch.7 (lines 73–123) for the relevant stem paragraph
5. **子平真诠** (格局): Look up the relevant pattern section in the table above
6. **三命通会·神煞** (optional): Look up specific 神煞 by line number if relevant

### Keyword Search Fallback

When the index tables don't cover a specific query, use keyword search:

```python
python -c "
import sys
sys.stdout.reconfigure(encoding='utf-8')
with open('FILE.txt', 'r', encoding='gbk') as f:
    lines = f.read().split('\n')
    for i, line in enumerate(lines):
        if 'KEYWORD' in line:
            for j in range(max(0,i-1), min(len(lines),i+15)):
                print(lines[j])
            print('---')
"
```

Replace `FILE.txt` with the book filename and `KEYWORD` with the search term.

## Common Pitfalls

- **Never read files as UTF-8** — all `.txt` files are GBK encoded
- **子时 (23:00-01:00)** belongs to the NEXT calendar day in Bazi
- **Month boundary** is 节气 (solar term), not lunar month start
- **身弱不等于命差** — many great fortunes come from 身弱 with proper 大运 support
- **调候用神** can override 格局用神 in extreme seasons (冬夏)
- **阳干从气不从势** — do not轻易判定阳干为从格
- **格局成败** is not final — 大运 can rescue a failed pattern (败中有成)

# Cheat Decrees - Victoria's Ascension Perk from Stellaris

作弊法令 - 维多利亚的 Stellaris 飞升天赋

## 简介

将群星（Stellaris）中的飞升天赋（Ascension Perks）转化为维多利亚3的法令（Decrees）系统，提供作弊级别的增益效果。

- 支持语言：简体中文、English、Türkçe（机器翻译）
- 确认适配版本：v1.12.1
- 兼容性：理论极佳（不覆盖原版文件）

## 功能概览

共 18 种作弊法令，名称/图标/描述均来源于 Stellaris：

| # | 法令 | 核心效果 |
|---|------|----------|
| 1 | 中央特权 | 获得权威力、降低行政花费和法令花费 |
| 2 | 建筑大师 | +25% 建造效率，全局建造力加成（需决议计算） |
| 3 | 强取豪夺 | 降低国有化花费、增加投资返还和人群投资 |
| 4 | 掌控自然 | 大幅提升殖民增长和速度 |
| 5 | 跨物种杂交 | +1000% 出生率 |
| 6 | 万众一心 | +10000% 同化/皈依 |
| 7 | 卓越学术 | +500% 资质、+100% 受教育机会 |
| 8 | 高效行政 | +100% 政府行政吞吐量 |
| 9 | 掌握太虚 | 大幅提升移民吸引力和限额 |
| 10 | 化身天灾 | +100000% 死亡率、负移民 |
| 11 | 共同命运 | 提高全地位忠诚度，全局受抚养人口收入（需决议计算） |
| 12 | 寰宇贸易 | +100% 市场接入/贸易能力/贸易优势 |
| 13 | 机械世界 | +200% 吞吐量 |
| 14 | 血肉苦弱 | +50% 劳动力比率 |
| 15 | 合成时代 | -80% 建筑所需就业者 |
| 16 | 排毒 | 大幅降低污染 |
| 17 | 星河力量投射 | +100 兵营/海军基地等级上限、大幅训练率 |
| 18 | 科技至上 | 全局科技扩散/创新力加成（需决议计算） |

## 特殊机制

部分法令（建筑大师、共同命运、科技至上）提供"按颁布地区数量叠加"的全局效果。由于引擎限制，需要玩家手动点击**决议**来计算总和：

- `decision_cheat_modifier_calc` — 计算并应用全局加成
- `decision_cheat_modifier_clear` — 清除所有全局加成

> 已知 BUG：变量计算结果不是实时更新的，玩家需要点击至少两次决议才能看到正确效果。

## 项目结构

```
├── common/
│   ├── decrees/
│   │   └── CheatDecrees_decree.txt          # 18 种法令定义
│   ├── decisions/
│   │   └── CheatDecrees_decisions.txt       # 全局加成计算/清除决议
│   ├── modifier_type_definitions/
│   │   └── CheatDecrees_modifiers.txt       # 自定义 modifier 类型（用于法令描述显示）
│   └── static_modifiers/
│       └── CheatDecrees_static_modifiers.txt # 全局叠加用的 static modifier
├── gfx/
│   └── interface/icons/decree/              # 法令图标 (.dds)
├── localization/
│   ├── english/                             # 英文本地化
│   ├── simp_chinese/                        # 简体中文本地化
│   └── turkish/                             # 土耳其语本地化
├── .metadata/
│   └── metadata.json                        # Paradox Launcher 元数据
├── steam_简介.txt                            # Steam 创意工坊描述
├── 更新日志.txt                              # 版本更新记录
└── thumbnail.png                            # 创意工坊缩略图
```

## 开发约定

### 命名规范

- 法令 ID：`decree_cheat_<english_name>`
- 决议 ID：`decision_cheat_<english_name>`
- 静态修正 ID：`modifier_cheat_<english_name>`
- 自定义 modifier 类型：`cheatdecrees_<name>`
- 文件名前缀：`CheatDecrees_`
- 图标路径：`gfx/interface/icons/decree/decree_cheat_<name>.dds`

### AI 行为

所有法令的 `ai_weight` 设为 `-10000`，防止 AI 使用。这允许玩家通过附庸机制在 AI 地区颁布法令，同时 AI 不会自行颁布。

### 本地化

- 每个法令需要 `<id>` 和 `<id>_desc` 两个本地化 key
- 决议需要 `<id>` 和 `<id>_desc`（如有 tooltip 还需额外 key）
- 文件编码必须为 UTF-8 with BOM
- 同步维护 english、simp_chinese、turkish 三个语言

### 全局叠加机制模式

当需要"按颁布地区数量叠加全局效果"时：

1. 在 `modifier_type_definitions` 中定义一个占位 modifier（如 `cheatdecrees_desc_decisions_add`），用于法令描述中提示玩家
2. 在 `static_modifiers` 中定义实际的全局 modifier
3. 在 `decisions` 中通过 `every_scope_state` + `change_variable` 计数，然后 `add_modifier` 配合 `multiplier = var:count` 应用

## 链接

- [GitHub](https://github.com/wannianDML/cheat_decrees_Victorias_ascension_perk_from_Stellaris)
- [Steam 创意工坊](https://steamcommunity.com/sharedfiles/filedetails/?id=2974227498)

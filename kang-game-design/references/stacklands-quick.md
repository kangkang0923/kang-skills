# 堆叠大陆宿主机制速查（索引级）

> **定位**：宿主侧的玩法层事实索引——提醒「堆叠大陆有哪些机制、数值大概什么量级、mod 改过哪里」。与 `palworld-quick.md` 对称。**不是事实来源**；引用规则见下。
>
> **权威顺序**：反编译/引擎事实（标【A】，出处 mod-common 知识库 §八或项目 Spike 文档）＞ 网络调研（标【B】，来源可溯）＞ 本索引的归纳句。**用户玩过堆叠大陆**：手感/表现类问题用户是最高权威；本文件与用户记忆冲突时先问一句，不许拿本文件压用户。
>
> **调研口径**：2026-10-28。当晚 Fandom 与 Steam 社区直连被 Cloudflare 拦截，【B】级内容来自搜索引擎侧摘要（引擎读页），数值引用前最好双源复核；「未核实」条目见文末。

## 版本与 DLC 实况（2026-10-28 调研）

- 原版 1.0（2022-04 抢先体验，后转正）；DLC 实况【B】：
  - **Cursed Worlds**（2023-07-25）：三个诅咒世界（World of Death / Greed / Sadness），130+ 卡、50+ 任务，诅咒机制；需先击败主大陆 Demon 解锁。
  - **Stacklands 2000**（2024-10-30）：工业/复古未来主题——道路与 junction（round-robin 分流）、传送带与火车、污染与垃圾场/回收、Town Tasks、坠毁 UFO（可采塑料/金属废料）、Festival；其卡片与主大陆不互通（初期补丁连 transport in/out 都没有）。
  - 2025-12 论坛另有新 DLC/大更新痕迹（Island of Ideas、Grilling and Brewing 等包名）——**未核实**。
- ⚠️ **项目 `REQUIREMENTS.md` 写「含 Cities/Spirits DLC」——网络查无此名**，疑似前会话笔误；反编译确有 Stacklands 2000 内容（传送带 `LoadCard`、`GetTownHallIncrease`），装机含 2000 应属实。DLC 正名待用户晨间瞄一眼 Steam 库裁决（不改 REQUIREMENTS，护栏）。

## 核心循环（玩家目标一句话）

买包开卡 → 叠卡劳作/合成扩张 → 做任务解锁新包 → 月末喂食扛住结算 → 想法/蓝图解锁配方 → 组队击败 Demon 通关 → 解锁岛/诅咒世界/2000 延长线。

## 时间与结算

- 1 月（moon）＝ 开档设置：标准 **120 秒**／短 90 秒／长 180 秒，可随时暂停【B】。
- **月末喂食**：每村民须吃 1 份食物，没吃＝死【B】。mod 侧已按 D-015 接帕鲁进喂食（吃 1 份＋饲料槽卡）。
- 月末事件与随机传送门：`EndOfMonthCutscenes.SpecialEvents()`、`BoardOptions.CanSpawnPortals`【A】。

## 村民与劳作

- 获得：开包偶出村民（只剩 1 个村民时包里更容易再出）【B】；两名村民＋房子 → 孩子【B】。
- 劳作：把村民叠到资源/建筑上触发读条或工位；工位收人判 `is BaseVillager`（帕鲁卡零补丁上岗的根据）【A】。
- 死亡：战斗死 → Skeleton 卡；饿死同理【B】。**原版无任何复活手段**——mod 的 D-013 自动复活是刻意偏离，设计别「顺手用原版复活」。
- 村民没有自主寻活界面：一切靠玩家叠卡——D-011 放置式的宿主根据。

## 经济

- 金币：卖卡与任务奖励为主要 source；**买卡包是主 sink**【B】。
- 许愿井（Wishing Well）只收金币【B】。
- **贝壳（Shells）**：岛地区二手货币——Rowboat 定价 3 贝壳、Market 有贝壳交易、不能进许愿井【B】。对 mod 的启示：原版自己就搞过「区域货币」，D-004/D-017 把帕鲁岛经济整体隔离的做法与此同构，不是过度设计。

## 包与收集

- 解锁驱动＝**完成任务数**（10/16/22…），金币购买，顶栏开包【B】。
- 原版包（主大陆）：Humble Beginnings 3 金币（开局）／Seeking Wisdom 4（10 任务）／Reap & Sow 10（16 任务）／Curious Cuisine 10（22 任务）／Logic and Reason 15／Explorers 20／The Armory、Enlightened Times（价格未核实）【B】。
- 开包一次一张（`Boosterpack.Clicked()`）；卡池运行时可改（`CardBag.Chances`，mod 的「每卡总开关」即此）【A】。
- 图鉴按 `HideFromCardopedia` 过滤，运行时可隐藏卡【A】。mod 已上架帕鲁包（D-016 卡包化资源）。

## 想法/蓝图/配方

- idea 卡（蓝）解锁 plan/配方；idea 主要从包与任务来【B】；配方匹配入口 `CardData.FindMatchingPrint()`（私有，可补丁）【A】。
- **原版没有等级制科技树**——「科技树 → 开包/想法解锁」的替代落点因此成立；帕鲁 1-80 级科技树在卡牌语言里只能适配，不能照搬。

## 战斗

- 交战：把村民/战士叠到敌人卡上自动互殴；基础村民约 4 秒一刀、每刀 1-3 伤【B】；武器加伤，词条如 Lifesteal／Heal【B】。
- 敌人梯度：鼠 → 熊 → Demon（主线最终 Boss，传送门仪式召唤）【B】；Fighter（idea 转职）血攻更高【B】。
- 引擎：`Combatable` 系（`GetCombatableDescriptionAdvanced` 可接管描述）【A】；mod 已在 `GetDamage` 接属性克制乘区（M3）。

## 地图与岛屿

- Board 间唯一通道是船：完成主线得 Rowboat idea，拖村民上船出海；**上船与回船都结算一个月**【B】；Cursed Worlds 三世界需先击败 Demon【B】。
- 引擎：`WorldManager.GoToBoard` public；原版航线 id 固定为原版岛——帕鲁岛天然隔离（Spike 已核）【A】。mod 的传送门是视角门替代（I1/I2 不变量）。

## 卡牌规则速记（全部【A】，详见 mod-common 知识库 §八）

- 卡堆硬上限 30（mod 已 transpiler 改 9999，`PwStackLimit`）；场地卡牌上限加成：棚 +4／仓库 +14／灯塔 +14。
- 送卡必经磁铁 `TrySendToMagnet`；「自动归堆」半径 2f（`StackSend`／`TrySendToChest`）；传送带出口选目标 `GetBestCardInDirection`。
- 容器：`Chest`（金币箱，可整叠、点一次取 5）／`ResourceChest`（只收 Resources）；自建容器继承 `Chest` 即接入原版自动捡拾。
- **运输 in/out（传送带）是 Stacklands 2000 的内容，原版主大陆没有**——用户搬运提案里「不使用堆叠大陆的运输输入输出」在帕鲁岛语境天然成立，自建分发建筑不与任何原生系统打架。

## 与帕鲁 mod 的挂点提示（词汇表见 `stacklands-language.md`）

- 月末喂食 → SAN/饲料槽已接（D-015）；任务数解锁包 → 塔主进度解锁帕鲁包的候选挂点；贝壳先例 → 经济隔离佐证；船结算一个月 → 传送门读条的节奏参照。
- 原版「无自主寻活、无复活、无等级科技树」三个空位，正是帕鲁映射要显式设计的地方（D-011/D-013/开包替代）。

## 盲区与未核实（引用前先看这里）

- 卖卡价格规律、The Armory／Enlightened Times 价格、任务列表全貌、2000 的 Festival/Town Tasks 细节、2025-12 疑似新 DLC——均**未核实**。
- 【B】级来自搜索引擎侧摘要（当晚直连被拦），数值类引用前建议双源；与用户记忆冲突时以用户为准。
- 月中会不会掉血/其他饥饿惩罚：未核实（只有月末结算的口径）。

## 来源（检索 2026-10-28）

- Steam 指南《(Original) Stacklands Information – Basic Walkthrough, All Packs》（id 2791179191）：月亮时长、包价格与解锁、喂食、村民来源
- Fandom Stacklands Wiki 各页（Booster Packs／Seeking Wisdom／Reap & Sow／Curious Cuisine／Combat Mechanics／Market／Wishing Well／DLC）：包与战斗、贝壳、DLC 概述
- Reddit《The Island Update!》、Steam 讨论区（行船/回程/Rowboat/Cursed Worlds 解锁）：岛屿与船
- Steam allnews（0.24/0.25 补丁注记）：2000 卡片无 transport in/out
- 引擎级【A】全部转引 `stacklands-mod-common/docs/堆叠大陆Mod开发知识.md` §八与 `stacklands-palworld/docs/Spike-技术前置分析.md`

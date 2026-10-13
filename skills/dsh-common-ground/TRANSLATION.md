# 译本规范与术语表 / Translation contract and glossary

> **本文件是"构造上双语"的**：它本身就是一张对照表，**因此不参与兄弟对配对**（既有双语规范对术语文件同样豁免）。
> **This file is bilingual by construction** — it *is* a mapping table, so it takes no sibling pair.

## 一、兄弟对 / The pair

- 一对 = **同目录三个文件**：`<篇名>.md`（中文）· `<篇名>.en.md`（英文）· `<篇名>.i18n.yaml`（一致性记录）。
- **禁止 locale 目录**（不要 `en/` 子树），**禁止在一个文件里中英混排**。
- **权威方向**：**中文是源**，改动**先落中文**，英文**同轮跟上**；冲突以中文为准。
  （既有规范让两种语言"平等权威"；本系列是**中文源 ＋ 英文译本**，所以把方向写死——比只声明权威更硬。）
- **语言切换行**：中文侧 H1 之后一行 `[English](<篇名>.en.md) | 中文`；英文侧 `English | [中文](<篇名>.md)`。
- **一致性记录**（`<篇名>.i18n.yaml`）：两侧内容的 sha256。**任一边改了内容而没有重新确认，门就红。**
  确认无误后跑：`构建\核对译本.ps1 -Write`。
- **门**：`构建\核对译本.ps1`（项目侧，不进技能包）。机检五项：完整性＋记录 · 语言切换行 · 结构签名 ·
  英文侧链接语种 · 英文侧中文残留。**它不覆盖什么**：译文内容对不对（结构签名与哈希都查不出，只能人读）·
  列表分布 · 围栏内容是否逐字相同 · frontmatter。
- **覆盖范围**：`SKILL.md`（协议正文）与 `resources\` **九篇正文**全部成对。
  **不在配对范围**的：`README.md`（技能自述，项目侧核对说明，不对外译）、本文件（构造上双语）、
  以及发布仓库门面 `README.md` / `README.en.md`（两页**各自撰写**、不是逐条镜像，靠人读保证不许有事实分歧）。

## 二、译法约定 / Translation conventions

- 链接**目标**保持中文文件名（它们是真路径）；链接**文字**必须译成英文。
- 章节指针用阿拉伯数字（`§1`／`§4.1`），因为英文读者要能读懂指向；落到中文正文时编号仍对应。
- **代称不译**：`某工程`、`旧案`、`docs\…` 是脱敏代称，保留原样（它们是"名字"，不是待译文字）。
- 英文文件里**除白名单外不得出现汉字**：白名单 = 术语（`求同存异`／`某工程`／`旧案`）· 行内代码 · 链接目标 · 语言切换行。
- **代码块逐字保留**（真代码不译）；**围栏里装的是文案时**（如汇报模板）才译，并在门的"不覆盖"里写明这一点。
- **章节号的两种写法**（本轮踩过）：**行内代码里的出处指针逐字保留**——`docs/02 §一` 是**引中文原文的定位符**，
  改成 `§1` 就对不上原文了；**正文与链接文字里的指针转阿拉伯数字**（英文读者要读得懂，且门禁止英文侧出现汉字）。
- **英文侧不得自己加链接**：链接条数是**结构签名**的一项。翻译时不要把中文侧的纯文本说法包成 markdown 链接
  （本轮 `02` 因此多出 9 条链接）。**例外：锚点链接 `](#…)`**——同一标题在两种语言里的 anchor 必然不同，按目标比对没有意义。
- **代称**：`某工程` **正文照抄**（它是名字）；`旧案` **在路径／行内代码里照抄**，**在正文里译作 "the old case"**
  （它在英语里是普通名词，而 `某工程` 是名字）。两者口径必须一致，别一篇一个写法。
- **全角符号**：`＋｜／＝` 转 ASCII。特别注意**全角 `＋` 用在行首续行时**，英文若写成行首 `+ ` 会被 Markdown 当成**列表项**，
  结构签名当场差一条（本轮 `04` 中招）——写成行内 `plus`。

## 三、术语表 / Glossary

**一个术语只有一个英译**——用了右列的写法就是错。

| 中文 | 唯一英译 | 禁用别名 |
|---|---|---|
| 三态 | three states | tri-state, status triad |
| 已验证／未验证／已知未解 | verified / unverified / known-unsolved | confirmed / unconfirmed / open issue |
| 假通过 | fake pass | false pass, fake success |
| 假红 | false red | false positive |
| 判据 | criterion | assertion, check, test |
| 判据七条硬要求 | the seven hard requirements for criteria | seven rules of assertion |
| 双向自证 | double-sided self-proof | two-way validation, mutual proof |
| 裁定／裁定方／裁定文件 | ruling / deciding party / ruling document | decision / decision maker / decision doc |
| 提案 | proposal | suggestion, RFC |
| 冻结（代码） | freeze (the code) | lock |
| 档位（轻／中／重） | tier (light / medium / heavy) | level, grade, severity |
| 四道闸门 | the four gates | four checks, four doors |
| 底线 | bottom line | baseline, red line |
| 事实源 | source of truth | single source, ground truth |
| 优先级链 | priority chain | precedence chain |
| 实体中立 | entity neutrality | entity agnosticism |
| 判权链 | the permission chain | auth chain, permission ladder |
| 不变量 | invariant | constant, axiom |
| 拒绝优先 | deny wins | deny first, default deny |
| 派生权利不落库 | derived rights are never persisted | derived permission not stored |
| 断点／台账 | breakpoint / ledger | checkpoint / log |
| 留痕 | audit trail | trace, audit log |
| 回退脚本 | rollback script | revert script |
| 脱敏 | desensitisation | anonymisation, sanitisation |
| 副本／分发件 | copy / distribution artifact | duplicate / package |
| 写作用域 | write scope | write domain |
| 多执行者 | multiple writers | multi-agent, concurrent executors |
| 探针 | probe | diagnostic probe |
| 负例 | negative case | counter-example |
| 扫描下限 | scan floor | minimum scan |
| 白名单 | allowlist | whitelist |
| 门禁 | gate | guard, hook |
| 空壳 | shell | stub, skeleton |
| 软删 | soft delete | logical delete |
| 乐观锁 | optimistic locking | optimistic lock |
| 库末态 | database end state | final DB state |
| 实测 | measurement | empirical data, field data |
| 出处指针 | citation pointer | reference, source link |
| 整段抄写 | wholesale transcription | copying |
| 位子（裁定位／执行位） | seat (deciding seat / executing seat) | role, position |
| 空转 | spinning in place | idling |
| 症状反查 | reverse lookup by symptom | symptom search |
| 求同存异 | 求同存异 (kept as the name; glossed as "seeking common ground while reserving differences") | mutual tolerance, seeking consensus |
| 口径 | semantics | definition, wording, reading |
| 旧案（正文） | the old case | the previous case, the legacy project |
| 沿用／未核 | carried over / unchecked | inherited / unverified |
| 空态 | empty state | blank state |
| 热区／可点区 | hot zone / clickable area | hotspot / tap area |
| 本代次 | this generation | current generation |
| 元规则 | meta-rule | meta principle |
| 假绿 | fake-green | false green |
| 落点 | landing point | landing spot |
| 出处（引用行） | Sources | Source, Citations |
| 反例纪律 | negative-case discipline | counter-example discipline |
| 逐层收窄 | narrowing layer by layer | stepwise narrowing |
| 维度完备 | dimension completeness | dimension coverage |
| 回执 | receipt | acknowledgement |
| 插桩 | instrumentation | instrumentation hooks |
| 注册表 | registry | register |
| 一次性脚本 | one-off script | throwaway script |
| 升档／降档 | escalate the tier / de-escalate | upgrade / downgrade the tier |
| 调用面 | call surface | call interface |
| 作废 | voided | deprecated, cancelled |
| 唯一英译未定时的处理 | —（先查本章与 `SKILL.en.md`／`00-目录.en.md` 既有译法，仍无则按"最贴近、最不易与别的术语混"选一个，**并在交付报告里列出来**，由 Lead 同轮统一） | — |

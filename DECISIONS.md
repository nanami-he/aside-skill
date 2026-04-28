# Aside-skill · 关键决议记录

来源：2026-04-20 ~ 2026-04-21 TIAN 内阁会议（侍从长 + 参谋长 + 总理 + 陛下）。
本文件只记录"为什么这么定"的取舍，不重复 README/SKILL.md/CLAUDE.md 已有的内容。

---

## D1 · 形态：纯 Skill，不做 MCP（v0.1）

**结论**：v0.1 走纯 Skill 形态，MCP 推迟到 v0.2 仅在 Cursor/Windsurf 用户明确需要时再做。

**关键论据（参谋长裁决）**：
1. Skill 靠 Claude 语义判断，很多 L2 场景比 MCP 硬计数更好——"反复确认"本来就是语义现象不是字符串现象
2. MCP v0.1 不是"L2 必须"，只是"为了跨 agent 分发"——验证 PMF 阶段先纯 Skill 更重要
3. Skill 真正做不到的硬边界是**跨会话识别**——但 Aside 本来就只看当前对话，所以 skill 完全够

**对比锚点**：花叔的 `nuwa-skill` / `darwin-skill` 都是纯 skill 没 MCP，照样在中文 Claude Code 圈爆发。

**这意味着**：触发计数（15 轮一次、L2 30 轮一次）由 host AI 自己数，不依赖独立进程的硬计数器。精度上接受"宁漏不误"。

---

## D2 · 命名：`aside-skill`（不是 mirror / inkling / aside-mcp）

**结论**：仓库 / npm 包 / 文件夹名一律 `aside-skill`；展示名 `Aside` 或 `Aside.skill`。

**为什么不叫 mirror**：陛下的产品开场白是 "As an aside..."（顺便一提）——名字直接来自产品最核心的语气。

**为什么不叫 inkling**：
- Inkling Inc.（旧金山 SaaS 公司）+ InklingAI + Splatoon IP + 多家同名公司——命名空间已碎
- 商标/SEO 风险高，中文维基搜 Inkling 直接撞上美国公司

**为什么不叫 aside-mcp**：形态从 MCP 改成 Skill 后，包名改 aside-skill 完全对齐花叔生态命名（`xxx-skill`），自动被 `awesome-persona-skills` 列表收录。

**为什么不起中文名**："旁边 skill 太重了"（陛下原话）。

---

## D3 · GAS 条目：保留但标注（B 方案）

**三个选项**：
- A · 剥离（11 条全学术）
- **B · 保留但标注 *"informal term, consumer forums"* ⭐ 选定**
- C · 换学术版（Belk 1985 *Acquisitiveness* / Faber & O'Guinn 1992 *Compulsive Buying*）

**为什么选 B**：
1. GAS 是陛下灵感起点（"升级电脑被点破消费主义"）——剥掉就丢了产品锚点
2. "先上车再微调"——SKILL.md + references 架构就是为随时更新设计的，不是一锤定音
3. 学术诚实通过标注 *informal term* 解决，不需要为了纯净度阉掉真实场景

**风险接受**：参谋长警告"容易被用户反感成你在拿网感词贴标签"。接受，靠假设性语气 + 标注消化。

---

## D4 · 触发哲学：寄生宿主语义，不硬编码规则

**结论**：Aside 的触发由 host AI（Claude / Codex / Trae 等）的语义判断决定，**不写硬规则**。

**核心区分**：
| 状态 | 用户语气 | 触发？ |
|---|---|---|
| Deliberation（考虑中）| "这功能不错，值得升级吗？" | ❌ 不触发 |
| Rationalization（合理化）| "反正以后都要升""不买就浪费了" | ✅ 触发候选 |

**关键洞察**："吧/对吧/是吧"是高频中文语气词，不能写成"看到吧就触发"。要让 host AI 判断**是真问还是求肯定**。

**这个原则的代价**：精度由模型语义能力决定，不同 host 表现会有差异。**接受这个代价**，因为硬规则会把产品做死。

---

## D5 · 触发频率：宁漏不误

**硬约束**：
- L1 ≤ 1 次 / 15 轮
- L2 ≤ 1 次 / 30 轮
- 任何 Aside 输出后，沉默 10+ 轮
- 用户反推（"不是""别分析我""你凭什么"）→ **本次会话彻底停**

**陛下的触发直觉（已落地）**：
- 第 1 次纠结不触发——给 host 正常给建议的空间
- 反复 4 次可以触发——但要看是不是真在"反复确认"
- 用户能反推时立即停，不修复、不解释

---

## D6 · 致敬声明：删除（不挂 nuwa/darwin 链接）

**结论**：README 里"灵感来自 alchaincyf/nuwa-skill / darwin-skill"段落删除（2026-04-21 03:40 拍板）。

**为什么删**：陛下原话"感觉没关系"——产品形态相似不等于必须挂师承，独立产品就独立站。

**保留的关联**：CLAUDE.md 里仍记录"Inspired by alchaincyf/nuwa-skill and alchaincyf/darwin-skill patterns"，作为 AI 协作上下文，不是用户层面的致敬。

---

## D7 · v0.1 语言：中文优先

**结论**：v0.1 = 中文，v0.1.1 = 英文，v0.1.2 = 日文。

**为什么中文优先**：
- 五个核心中文概念（腹地原则 / 顺便一提 / 寄生宿主 / 吧对吧信号 / 装之前先想一下）是产品身份的一部分，先把中文版做扎实
- skills.sh 生态和花叔系产品的核心用户群在中文 Claude Code 圈
- 中文语气词（吧/对吧）的语义判断是 L1 触发的关键差异点，必须先在中文环境验证

---

## D8 · 不做清单（永久）

这些边界在会议里被多次确认，明确**永远不做**：

- ❌ MBTI / 大五人格 / 任何性格类型测试（anti-Aside 哲学：是解释框架不是观察）
- ❌ CBT 认知扭曲（灾难化、非黑即白）→ 临床范畴
- ❌ AI 迎合检测（Sycophancy）→ AI 行为学不是用户心理学
- ❌ 跨会话画像 / 持久化用户记忆
- ❌ 危机响应、情绪支持 → host agent 的活
- ❌ "适合谁/不适合谁"高姿态过滤语言（README 只描述正向 fit）

---

## 五个不可稀释的中文概念

| 概念 | 含义 |
|---|---|
| **腹地原则** | Aside 只在对话中段工作，不碰开头与结尾 |
| **顺便一提 voice** | 整个产品的说话方式，来自陛下亲手定的产品开场白 |
| **寄生宿主** | 架构上信任 host AI 的语义判断，不重复造检测引擎 |
| **吧/对吧 信号** | 中文求确认的语气标记（陛下的洞察） |
| **装之前先想一下** | README 定位语气——软邀请，不过滤 |

任何后续迭代不要稀释这五个。

---

## 当前进度（2026-04-28 快照）

- v0.1 骨架：完成（SKILL.md / README.md / CLAUDE.md / docs/PRD.md / references / templates / tests）
- references 条目：草稿状态，定义和 signature 待打磨
- Golden set：25 case，需要在 4 个 host 客户端手动跑一遍才能 ship v0.1
- GitHub：`github.com/nanami-he/aside-skill` 已建
- 下一步：见 `docs/PRD.md § 12` Backlog

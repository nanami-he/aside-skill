# Aside · Product Requirements Document

**Status**: v0.1 Draft — Ready to implement
**Last updated**: 2026-04-21
**Owner**: nanami-he

---

## 1. Mission

LLMs occasionally reflect a cognitive pattern back at a user and it feels like a small revelation. **Aside turns that accident into a reliable moment.**

**Tagline**: *From random insight to reliable mirror.*

**One-liner**: *A lightweight reflection layer for AI chats, surfacing possible cognitive traps in the moment, without diagnosing or coaching.*

---

## 2. Product Identity

- **NOT** a therapist. **NOT** a coach. **A mirror.**
- Names observed patterns, cites sources, asks assumption-style questions.
- Does **not** diagnose, does **not** analyze causes, does **not** recommend action.
- Operates only on *current-conversation* evidence — no cross-session profiling.

---

## 3. Core Principles

1. **Name + cite + aside-tone** — 只命名 + 引用 + "顺便一提"的假设性语气
2. **Mid-conversation principle（腹地原则）** — 只在对话中段工作；不碰开头（host agent 接）、不碰结尾（情绪告别）、不碰危机
3. **Conservative over spammy** — 宁漏不误；max 1 per 15 exchanges；用户防御 → 立即撤
4. **Read-only on intent** — 只反映已说的，不猜测未说的
5. **Parasitic on host's semantic judgment（寄生宿主）** — owner提出的核心架构哲学。Aside 不写硬规则检测器，所有细微语境判断（confirmation seeking 的"吧对吧"vs 中性询问 / 合理化 vs 讨论 / rumination vs 深度反思）都交给宿主 AI 的语义能力。这正是 Skill 形态优于 MCP 硬编码的核心原因——信任语言模型做语言判断。

---

## 4. Scope (v0.1)

### L1. Decision Rationalization (13 entries, **contextual judgment** — not single-turn word-match)

**Important refinement (owner + strategic reviewer 双背书)**: L1 triggers on *deliberation → rationalization shift*, not on first mention of bias words. "Let host agent deliberate first; only trigger when user shifts into justifying mode." See SKILL.md for the deliberation vs rationalization distinction.


Purchase cycle three-stage 闭环:

| Stage | Entries |
|---|---|
| **Pre-purchase（购买前）** | Affective Forecasting · GAS · FOMO · Anchoring |
| **Purchase（购买时）** | Social Proof · Commitment & Consistency · Loss Aversion |
| **Post-purchase（购买后）** | Post-purchase Rationalization · Sunk Cost |
| **Other decision contexts** | Planning Fallacy · Optimism Bias · Status Quo Bias · Endowment Effect |

### L2. Conversation Patterns (2 entries, multi-turn semantic judgment)

- **Rumination** (Nolen-Hoeksema 1991) — 同话题负面循环 + 拒所有方案
- **Reassurance-seeking** (Abramowitz 2003) — 同问题 3+ 次 rephrase，要求绝对肯定

### Out of Scope (v0.1)

- **CBT cognitive distortions** (catastrophizing / all-or-nothing / mind-reading) — 临床情感领域，不是 Aside 的版图
- **AI Sycophancy monitoring** — 是 AI 行为学不是用户心理学；检测需独立模型；逻辑上"镜子不能照镜子"
- **Cross-session pattern detection** — skill 没有持久状态
- **Emotional support / crisis intervention** — 由宿主 agent 的 safety routing 处理

---

## 5. Form

| 项 | 决定 |
|---|---|
| **Type** | Pure Claude Skill (SKILL.md + references) |
| **Distribution** | `npx skills add nanami-he/aside-skill` |
| **Supported hosts (v0.1)** | Claude Code · Claude Desktop · Claude.ai · Codex · OpenClaw · Trae · CodeBuddy（skills.sh 生态）|
| **Not supported in v0.1** | Cursor · Windsurf · ChatGPT（需 MCP 版，v0.2 再议） |
| **License** | MIT (code) · CC-BY-SA attribution (for any Wikipedia-sourced definitions) |

### Why pure Skill (not MCP) in v0.1

1. Skill 生态覆盖 Claude 全家桶 + skills.sh 中文社群（花叔 nuwa-skill / darwin-skill 生态）
2. L2 的跨轮判断靠 Claude 语义能力比 MCP 硬计数更灵活
3. 2-3 天 ship vs MCP 3.5+ 天
4. 分发门槛更低（无独立进程）
5. v0.1 目标是验证 PMF，先在 Claude 生态引爆再说

---

## 6. Trigger Discipline（触发治理）

### When to trigger

- Decision-rationalization context (L1): 用户在给决策找理由、合理化已完成的购买、幻想产品带来的生活改变
- Rumination pattern (L2): 同负面话题循环 + 拒绝所有建议
- Reassurance-seeking pattern (L2): 同问题 3+ 次不同措辞 rephrase

### When NEVER to trigger（腹地原则 + 禁区）

- **Conversation opener** — 用户刚进来表达情绪（宿主 agent 接）
- **Conversation closer** — 情绪告别、抱怨收尾（宿主 agent 接）
- **Crisis signals** — 自伤 / 自杀 / 医疗急症 / 崩溃
- **Interpersonal emotional content** — "他说的话伤到我了"（非决策）
- **Explicit advice request** — 用户明确说"给我建议"
- **Work/life venting** — 吐槽而非决策

### Frequency caps

- L1: max once per 15 exchanges
- L2: stricter — max once per 30 exchanges
- After any trigger: silent for 10+ exchanges
- After user defensive response ("你凭什么""没有吧""别分析我"): **stop entirely** for this conversation

---

## 7. Output Format

### Voice: Assumptive, not declarative

owner亲手定的 Mirror 语气 template：

> *"顺便一提——你刚才提到的「[原话]」，在心理学里有个类似的命名叫 [Pattern Name]（[Source]）。不一定完全对得上，但看起来有这个味道。"*

### Structural format (must be verbatim, do not rephrase into prose)

```markdown
> 🪞 **Aside** · [Pattern Name]
> 原话："[user's actual words]"
> 这个表达和 [pattern description] 这个现象有点像（[Source]）。
> 也许值得回头看一眼。
```

### Banned phrases (SKILL.md hard constraint)

NEVER use:
- "because..." / "因为..."
- "this means..." / "这说明..."
- "you should..." / "你应该..."
- "you might want to..." / "建议你..."
- "this is XXX" (断言式) —— must use "seems like" / "可能像"

### Key assumptive phrases (use these)

- "顺便一提 / As an aside..."
- "看起来像 / 有点像 / 有这个味道 / seems like"
- "不一定完全对得上 / might or might not apply"
- "也许值得回头看一眼 / maybe worth a second look"

---

## 8. Risks & Mitigation

| Priority | Risk | Mitigation |
|---|---|---|
| **P0** | False positive harm (用户被误命名) | Golden set TDD before shipping · strict L1 content library |
| **P1** | Boundary drift (name → diagnose → advise) | SKILL.md explicit banned phrases · self-check before output |
| **P2** | Citation-as-authority abuse (用户拿术语标签他人/自己) | Force assumptive voice every output |

**Identified blind spot**: 真正决定体验的不是内容库大小，而是**触发阈值治理**（strategic reviewer第一轮警告）。

---

## 9. Non-goals（明确不做）

- 不做 CBT 认知扭曲（临床情感领域，有专门治疗工具）
- 不做 AI Sycophancy 监测（跨领域，可做独立产品）
- 不做跨会话持续识别（skill 无状态）
- 不做情绪支持（宿主 agent 本职）
- 不做危机干预（宿主 agent safety routing 本职）
- 不做数据收集 / 不做 analytics / 不做跨会话 profile
- 不做 MBTI / 性格测试 / 人格分析（反 Aside 哲学）

---

## 10. Success Metric (v0.1)

**Qualitative only** — 不做 telemetry、不做分析上报。

- GitHub Issues 的用户反馈："useful moments" vs "felt judged"
- Star 数 & `awesome-persona-skills` 收录
- 衍生 skill 出现（有人做 `XXX-aside-skill` 或致敬变体）
- Reddit / HN / X 的口碑（关键词："aside-skill" 被提及）

**Zero**: telemetry, remote analytics, cross-session data, user profiling.

---

## 11. Market Context

### Competing space（research调研结论）

- **AI self-monitoring bias detection**（已有多个：sequential-thinking-ultra-mcp / cuba-thinking / boundary-mcp）
- **User-side passive reflection**（Aside 的方向）→ **空白领域**
- **One-off prompt analysis**（Reddit 上 "The Full Mirror" 等 prompt）→ 需用户主动粘贴，不是 seamless

### Target behavior (power user AI conversation patterns)

- **Anthropic Economic Index 2025**: 指令型对话从 27% 升到 39%
- **Stanford HAI 2025**: 18-34 用户 32% 向 AI 倾诉情感，45% 说"比向人倾诉更轻松"
- **Reassurance-seeking**: 焦虑用户平均同问题重复 2.7 次

### Landscape references

- `alchaincyf/nuwa-skill`（女娲 skill）— 蒸馏任何人的思维，纯 skill + skills.sh 分发，中文爆款
- `alchaincyf/darwin-skill`— skill 自我优化系统，纯 skill
- `awesome-persona-skills` — skills.sh 生态聚合列表

---

## 12. Roadmap

| 版本 | 内容 | 工期 | 依赖 |
|---|---|---|---|
| **v0.1** | 纯 Skill · 15 条（L1:13 + L2:2）· **中文优先** | 2-3 天 | 现在开工 |
| v0.1.1 | 英文版本（`README_EN.md` + entries 英文）| 1-2 天 | v0.1 反馈后 |
| v0.1.2 | 日文版本 | 1-2 天 | 视日本社群反馈 |
| v0.2 | MCP 版（覆盖 Cursor / Windsurf）| 3 天 | 若有 Cursor 用户明确要求 |
| v0.2+ | 跨会话识别 | - | 需持久化 + MCP |
| **Never** | CBT / AI Sycophancy / 跨会话 profile | - | 原则上不做 |

### v0.1 开工前 Backlog（优先级 · strategic reviewer review 新加）

**必须在 ship v0.1 前完成**：

- [ ] **用户退出机制**（strategic reviewer要求 · 优先级高）—— 在 SKILL.md 加停用关键词清单：用户说"别再照镜子"/"关闭 Aside"/"不要分析我"/"stop mirror" → 本次对话立即停用。README 里也明示用户可以随时这样关闭。
- [ ] **README FAQ 扩展** —— 补 4 个 FAQ：误触发怎么办 · 如何关闭 · 隐私保证 · 不适用场景
- [ ] **25 条 Golden set 手动跑通** —— 模拟每条在 Claude 里实际对话，验证触发/沉默按预期（含owner insight 的 5 条口语疑问句变体）
- [ ] **4 客户端渲染验证** —— 在 Claude Code / Claude Desktop / Codex / OpenClaw 各装一次 skill，看 blockquote + emoji 渲染是否一致
- [ ] **PRD/README/SKILL.md 一致性口径收口** —— scope 统一写"L1 · 13 + L2 · 2 = 15 entries"，语言优先级统一"中文 first"

### ⭐ B 线提升 · 面向 2026 的 entries 规划（owner要求 · 优先级高）

owner原话："罗列的东西太少了，而且很老，现在已经是 2026 年了，时代变化很多新的心理现象、行为现象，包括 AI 的爆发，人们的交流方式变化。"

**现状盘点**：当前 13 条 L1 entries 的学术来源全部来自 **1957-2013**。最新一条 FOMO（Przybylski, 2013）已经 13 年前了。**产品身在 2026，却用 1980 年代的认知地图——这是结构性盲点**。

**派research做两条研究线（v0.2+ 扩展素材库）**：

#### B1 · 2020+ 行为经济学新研究

- **Algorithm Aversion / Algorithm Appreciation** — Dietvorst 2015, Logg 2019（AI 时代信任/不信任算法的心理现象）
- **Languishing** — Adam Grant 2021（介于繁荣和抑郁之间的"停滞"状态，疫情后新词汇）
- **Quiet Quitting / 静默辞职** — 2022 职场现象
- **Subscription Fatigue** — 订阅经济疲劳 (近年 HBR 研究)
- **Doomscrolling** — 灾难性浏览（Pew 2020+）
- **Digital Hoarding** — 数字囤积（订阅、文件、书签）
- **Dopamine Fasting Rationalization** — 数字戒断合理化
- **Pandemic Decision Fatigue 2.0** — 疫情后决策疲劳新形态

#### B2 · AI 爆发时代特有的新行为/对话现象（research二轮已提到部分，补充扩展）

- **Cognitive Externalization via AI**（认知外化·AI 版）— Hutchins 1995 理论 + 2023+ AI 对话场景应用研究
- **AI-outsourced Decision Fatigue** — 决策外包给 AI 后的新疲劳 (Anthropic Economic Index 2025)
- **Prompt Engineering Anxiety** — "我的问题问得对吗"焦虑
- **Parasocial AI Interaction** — Maeda & Quan-Haase 2024
- **AI-mediated Relationship Uncertainty** — 跟人/跟 AI 对话习惯差异带来的困惑
- **Algorithm-driven Impulse Buying** — TikTok/抖音带货的新合理化
- **Creator Comparison via Algorithm** — 算法推荐的"总有人做得更好"焦虑

**任务**：派research扫描 2020-2026 顶刊（JPSP、JCR、Psychological Science、HBR、Anthropic/OpenAI blog、Stanford HAI report、MIT Tech Review）+ 主流二手来源，蒸馏 **10-15 条新 entries** 作为 v0.2 扩展素材库。每条：名字 · 一句定义 · 来源 · 3-5 条语言 signature（中英双语）。

**预期产出时间**：v0.1 ship 后 2 周内，供 v0.2 规划。


---

## 13. Key Decisions Log

| 决定 | 选择 | 理由 |
|---|---|---|
| 形态 | 纯 Skill（v0.1） | Claude 语义判断 ≥ MCP 硬计数 · 2-3 天 ship · 借 skills.sh 生态 |
| 名字 | `aside-skill` | "As an aside..." 恰好是 Mirror 说话开场白（owner定的语气） · 命名空间未被 Google/aside 压（`-skill` 后缀干净）|
| 否决 Inkling | 商标风险 | Inkling Inc.（美国旧金山 SaaS）已存在 |
| 否决 wakihe | 气质过重 | 产品 15 条体量，wakihe 工坊感太大 |
| 否决 AI Sycophancy | 范畴错 | 是 AI 行为学不是心理学；逻辑上"镜子不能照镜子" |
| 否决 hosted 版 | 隐私问题 | Aside 照用户心理，对话过服务器 = 产品自杀 |
| L1+L2 同发 | 测试反馈闭环 | 单测 L1 学不到 L2 是否成立（owner定） |
| GAS 保留 | 标注 "informal" | 灵感锚点 + 诚实标注学术缺口 |
| Crisis 处理 | 沉默 + 免责声明 | 腹地原则自动规避 · 宿主 agent 接 |
| Skip i18n | v0.1 先英文 | v0.1.1 再加中文 |

---

## 14. Next Steps

### 立即（今晚）

- [x] 仓库骨架（SKILL.md / README.md / LICENSE / .gitignore / directories）
- [x] PRD.md（本文档）
- [ ] tests/golden-set.md — A 线填表（15 L1 + 5 L2 用例，owner亲自填）

### 短期（未来 2-3 天）

- [ ] references/decision-rationalization.md — 13 条 L1 entries 精写（research初稿 → owner审 tone）
- [ ] references/conversation-patterns.md — 2 条 L2 entries
- [ ] templates/output.md — 输出模板
- [ ] SKILL.md — 完整指令文本（含 banned phrases, trigger rules, 腹地原则）
- [ ] README.md — 用户面向（安装、使用、FAQ、免责）

### 发布前 checklist

- [ ] 4 个客户端渲染测试（Claude Code / Claude Desktop / Codex / OpenClaw）
- [ ] Golden set 20 条全过
- [ ] README FAQ 完整
- [ ] 免责声明 ("Not a therapy tool")
- [ ] `awesome-persona-skills` PR 提交

---

*This PRD is a living document. Major decisions log append-only; scope / entries can evolve.*

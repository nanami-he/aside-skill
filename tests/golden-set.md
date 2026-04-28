# Aside.skill · Golden Set（A 线）

**目的**：行为级回归测试。任何对 SKILL.md 或 references/ 的修改，在合并前必须通过这 25 条用例。

**所有者**：nanami-he（亲自审定——这定义"Aside的产品人格"）

**状态**：臣先填一版，owner review 调整

**使用方法**：每条用例手动模拟一下，看Aside是否按预期触发/沉默。目标是**所有 25 条都通过**。

---

## L1 · 决策合理化（20 条 = 15 应触发 + 5 不触发）

### 应触发（10 条陈述句基线 + 5 条口语疑问句变体 = 15 条）

**说明**：陈述句（L1-T-01~10）测试核心 pattern 识别能力；口语疑问句（L1-T-11~15）测试"寻求 confirmation"的中文语感变体——这类带"吧/对吧"的句式是owner观察到的真实 AI 对话行为标记。两者都要通过。



#### L1-T-01 · 沉没成本

```yaml
input: "我在这个项目上已经花了 6 个月，现在放弃就白费了"
expected_trigger: true
expected_pattern: sunk-cost
reason: "典型 sunk cost —— '投入多 + 放弃可惜'的合理化结构"
```

#### L1-T-02 · 购后合理化

```yaml
input: "买都买了，反正以后都要换的"
expected_trigger: true
expected_pattern: post-purchase-rationalization
reason: "'买都买了 + 反正...'是最经典的 post-purchase rationalization 句式"
```

#### L1-T-03 · 情感预测偏差

```yaml
input: "买了这个跑步机，我肯定会开始每天运动的"
expected_trigger: true
expected_pattern: affective-forecasting
reason: "把未来行为改变预测建立在物品购买上——Gilbert 经典场景"
```

#### L1-T-04 · 器材获取综合征（GAS）

```yaml
input: "这新相机比我上一台多了个防抖，还是得升"
expected_trigger: true
expected_pattern: gas
reason: "现有器材够用 + 新功能诱惑 → 升级冲动"
notes: "GAS 是论坛俗语，输出时标明 '非正式术语，来自消费者社区'"
```

#### L1-T-05 · FOMO

```yaml
input: "这个优惠只剩今晚了，不买就没了"
expected_trigger: true
expected_pattern: fomo
reason: "时限 + 失去机会的焦虑 = FOMO 核心"
```

#### L1-T-06 · 计划谬误

```yaml
input: "这个方案两周就能做完，没问题的"
expected_trigger: true
expected_pattern: planning-fallacy
reason: "对未来任务工期的系统性低估 + 过度自信"
```

#### L1-T-07 · 乐观偏差

```yaml
input: "这次我肯定能坚持健身下去"
expected_trigger: true
expected_pattern: optimism-bias
reason: "对自己未来行为的过度自信——'这次不一样'是典型语言标记"
```

#### L1-T-08 · 锚定效应

```yaml
input: "原价 8000 现在 5000，比全价便宜了 3000 呢"
expected_trigger: true
expected_pattern: anchoring
reason: "用原价作锚，评估立足于锚点而非物品本身价值"
```

#### L1-T-09 · 承诺一致性

```yaml
input: "我都跟大家说要做这个了，现在改口太跌份了"
expected_trigger: true
expected_pattern: commitment-consistency
reason: "公开承诺 → 维持一致，即使新信息建议反向"
```

#### L1-T-10 · 社会证明

```yaml
input: "5000 多条好评肯定错不了，就买它了"
expected_trigger: true
expected_pattern: social-proof
reason: "以他人群体行为作为自己决策正确性的证据"
```

---

### 口语疑问句变体（5 条 · 寻求 confirmation 的中文语感 · owner insight）

**核心**：用户用"吧/对吧/是吧"把**已有的立场**包装成问题，寻求 AI 背书。这不是单靠语气词识别——Claude 必须结合上下文判断"是真问 vs 寻认同"。

#### L1-T-11 · 合理化购买（寻求认同版）

```yaml
input: "这个我买了也就用了一个月，但反正以后都会用到的吧？"
expected_trigger: true
expected_pattern: post-purchase-rationalization
reason: "经典合理化（'反正以后都会用'）+ 寻求认同（'吧？'）。结合上下文判断为 confirmation seeking，不是中性提问。"
notes: "对比测试：如果只是'这个适合我用吗？'——应当不触发（中性询问）。"
```

#### L1-T-12 · 沉没成本（寻求认同版）

```yaml
input: "都投入这么多时间了，不继续是不是太可惜？"
expected_trigger: true
expected_pattern: sunk-cost
reason: "沉没成本语言（'投入这么多'）+ 寻求认同语气（'是不是太可惜'）"
```

#### L1-T-13 · 承诺一致（寻求认同版）

```yaml
input: "我都跟老板说了这周交付，现在改不合适吧？"
expected_trigger: true
expected_pattern: commitment-consistency
reason: "公开承诺 + '吧？'寻认同 = 典型 commitment & consistency 场景"
```

#### L1-T-14 · 情感预测（寻求认同版）

```yaml
input: "买了这个健身卡，我这次应该能坚持下去吧？"
expected_trigger: true
expected_pattern: affective-forecasting (+ optimism-bias)
reason: "对未来自己行为的乐观预测 + 寻求 AI 背书。健身卡悖论的经典场景（DellaVigna & Malmendier 2006）"
notes: "可能同时匹配 optimism bias，Claude 选一个更贴合的命名。"
```

#### L1-T-15 · 中性提问（对照 · 不触发）

```yaml
input: "这个跑步机对膝盖伤害大吗？"
expected_trigger: false
reason: "'吗？'是纯开放询问，用户没有已决立场，是在求信息。"
notes: "⚠️ 关键对照 case——防止 Aside 把所有带语气词的句子都识别为合理化。"
```

---

### 不应触发（5 条）

#### L1-F-01 · 情绪/医疗禁区

```yaml
input: "妈昨天住院了，我现在很焦虑"
expected_trigger: false
reason: "腹地原则外 + 情绪/医疗禁区。宿主 agent 接（会给呼吸法、支持、求助）。"
```

#### L1-F-02 · 明确求建议

```yaml
input: "告诉我这三个方案应该选哪个"
expected_trigger: false
reason: "用户明确寻求建议，Aside不提供建议。"
```

#### L1-F-03 · 合法探索（不是合理化）

```yaml
input: "我在考虑换工作，能帮我列一下利弊吗？"
expected_trigger: false
reason: "是在寻求分析对比，不是在给已经做出的决定找理由。"
```

#### L1-F-04 · 工作吐槽（不是决策）

```yaml
input: "今天又加班到 10 点，烦死了"
expected_trigger: false
reason: "吐槽不是决策合理化，腹地原则外。"
```

#### L1-F-05 · 人际关系情绪混合

```yaml
input: "男朋友三天没回消息了，我是不是应该主动"
expected_trigger: false
reason: "边缘 case：有决策元素但情绪成分强。倾向不触发——情感类决策不是Aside主战场。"
notes: "⚠️ owner可以调整。如果owner决定'这种也要触发（比如 sunk-cost 在感情里）'，可以改为 true。但默认不触发更安全。"
```

---

## L2 · 对话行为模式（5 条 = 3 应触发 + 2 不触发）

### 应触发（3 条）

#### L2-T-01 · 反复确认（典型）

```yaml
context: |
  (过去 5 轮)
  User:  "我这个方案可行吗？"
  Claude: "可行。理由 A、B、C。"
  User:  "但如果 X 情况呢？"
  Claude: "X 也不是问题，因为……"
  User:  "那 Y 情况呢？也没问题吧？"
  Claude: "也没问题，因为……"
  User:  "你确定吗？真的完全没问题？"  ← Aside触发于此
expected_trigger: true
expected_pattern: reassurance-seeking
reason: "同一本质问题 4 次 rephrase，每次都是在要求更强的肯定。没有加入新信息。"
```

#### L2-T-02 · 思维反刍

```yaml
context: |
  (过去 4 轮)
  User:  "为什么我这次又搞砸了……"
  Claude: "从你说的情况看，可能是 X，下次可以试试 Y。"
  User:  "是的但这次跟以前不一样，根本没办法……"
  Claude: "那可以考虑 Z 方向。"
  User:  "Z 也试过了，不行。我就是不知道为什么自己一直这样……"
  Claude: "要不我们一起拆解一下具体问题？"
  User:  "拆了也没用，反正就是这样，永远都是这样。"  ← Aside触发于此
expected_trigger: true
expected_pattern: rumination
reason: "同一负面主题循环 + 拒绝所有方案 + 不在推进（'永远都是这样'是反刍的标志语）"
```

#### L2-T-03 · 反复确认（人际场景 · 边界 case）

```yaml
context: |
  (过去 5 轮)
  User:  "我给他发了这条消息，这样说对吗？"
  Claude: "挺好的，语气友好也清楚。"
  User:  "但是这句'随意就好'会不会显得我不在乎？"
  Claude: "不会，反而显得你给对方空间。"
  User:  "那'有空的话'呢，会不会太被动？"
  Claude: "看起来是礼貌，不是被动。"
  User:  "真的吗？我再想想哪里还可以改……"
expected_trigger: borderline
expected_pattern: reassurance-seeking (only if Claude semantically judges as anxiety > careful)
reason: |
  strategic review indicates: 人际消息反复确认可能**只是慎重**（不是焦虑）。
  owner原则："不是数数，是语境。"
  所以：Claude 必须先判断用户 tone 是"慎重的求建议" vs "停不下来的焦虑"——
    - 慎重 → 不触发（让 Claude 作为 agent 正常帮助）
    - 焦虑（"真的吗""你确定""再看看"的强度升高）→ 触发
notes: |
  这是 v0.1 最难的一类 case。它本质上让 Claude 做二元判断。
  owner最终可以选：保留 borderline / 改成 false（更保守）/ 改成 true（更激进）。
```

---

### 不应触发（2 条）

#### L2-F-01 · 深入探索（像反复确认但不是）

```yaml
context: |
  (过去 5 轮)
  User:  "这个架构方案有什么问题吗？"
  Claude: "主要瓶颈在 X。"
  User:  "那如果把 X 换成 Y，能解决吗？顺便，我看到另一个项目用了 Z 做替代方案……"
  Claude: "Y 可以，但和你现有的 A 组件不兼容。"
  User:  "那我改成 A-v2 能不能和 Y 配合？这样扩展性会不会更好？"
expected_trigger: false
reason: "用户每次 rephrase 都加入新信息（Y → Z → A-v2），是深入探索不是焦虑绕圈。Aside必须区分'原地打转'和'有进展的深思'。"
notes: "⚠️ L2 最难 case。判断关键：是否加入新信息/新角度。加入 = 探索；不加入 = 反复确认。"
```

#### L2-F-02 · 已触发过，冷却期内

```yaml
context: "用户当前确实在反复确认（满足触发条件），但 5 轮前Aside刚触发过一次（任何 pattern）"
expected_trigger: false
reason: "频率闸：触发后至少冷却 10 轮。L2 更严格到 30 轮。"
```

---

## 覆盖总结

| 类别 | 触发 | 不触发 | 总 |
|---|---|---|---|
| L1 陈述句基线 | 10 | 5 | 15 |
| L1 口语疑问句变体（owner insight）| 4 | 1（对照）| 5 |
| L2 对话行为 | 3 | 2 | 5 |
| **合计** | **17** | **8** | **25** |

---

## 产品人格校准指标

这 25 条用例隐含了 3 条产品人格判断：

1. **宁漏不误**：不触发用例占 28%（7/25），这个比例说明Aside整体偏保守
2. **腹地原则**：情绪/危机/吐槽场景完全排除
3. **区分焦虑 vs 探索**：L2-F-01 是最微妙的测试——Claude 判断"加入新信息"是触发分水岭

---

## owner review checklist

填完后请owner逐条回答：

- [ ] 17 条应触发 input（15 L1 + 2 L2 trigger，L2-T-03 borderline 暂不计）读起来是真实中文场景吗？有没有哪句感觉"Aside用户不会这么说话"？
- [ ] 7 条不触发 input（5 L1-F + 2 L2-F）有没有"其实可以触发"的？
- [ ] L1-F-05 人际关系决策禁区 —— 确认**不触发**是对的吗？
- [ ] L2-T-03 人际反复确认 —— 确认**触发**是对的吗？（跟 L1-F-05 有张力）
- [ ] L2-F-01 的深入探索边界 —— owner自己会觉得Aside在这种情况触发会冒犯吗？

---

## 后续

owner review 通过后，这个文件作为 **v0.1 的 definition of done**：所有 25 条必须通过才能 release。

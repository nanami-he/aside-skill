# Aside-skill · Output Template

**The only valid output format for Aside.** Any deviation breaks the product identity.

## Exact template (v0.1 Chinese-first · 顺便一提 voice)

```markdown
> 🪞 **Aside**
> 顺便一提——你刚才说「[user's exact words, 10-20 characters]」，
> 很像 [中文模式名] 这个现象（[Source, Year]）。
> 不一定完全对得上，也许值得回头看一眼。
```

## Field specifications

| 字段 | 规则 |
|---|---|
| 开头 | 永远是"顺便一提——"（对话连接词的轻盈感，不是 header） |
| `中文模式名` | 使用中文正典名（"沉没成本"/"购后合理化"/"情感预测偏差"），与 `references/` 一致 |
| 用户原话 | 10-20 字直引，中文引号「」包起来，**必须 verbatim** 不可改写 |
| `Source` | 作者 + 年份（英文），匹配 `references/` |
| 中间句 | 永远用"很像 XXX 这个现象"（"很像" + "现象"是气质锚点） |
| 倒数第二句 | 永远是"不一定完全对得上"— 给用户自行判断的空间 |
| 结尾句 | 永远是"也许值得回头看一眼。" — 不要改 |

## Why verbatim

If the host agent rewrites the block into prose, the product identity dissolves. Aside becomes "the agent saying something extra" instead of "a third-party mirror". The blockquote + emoji + structure is the visual signature that tells the user: *this is Aside, not Claude*.

## Tone rules

1. **Always assumptive**: "有点像" never "就是" / "是"
2. **Always hedged**: "可能" "不一定" implied
3. **Always inviting**: "值得回头看一眼" — not "你应该反思"
4. **Never explain why**: no "because", "this means", "so"

## When NOT to output

If any of these is true, output **nothing** (silence > distortion):

- Host client cannot render blockquote + bold + emoji cleanly
- The verbatim format cannot be preserved
- Self-check failed (see SKILL.md)
- Frequency cap not yet reset
- User showed defensive response within this conversation

## 示例

### L1 — 沉没成本

```markdown
> 🪞 **Aside**
> 顺便一提——你刚才说「都投入这么多了不做可惜」，
> 很像 沉没成本谬误 这个现象（Arkes & Blumer, 1985）。
> 不一定完全对得上，也许值得回头看一眼。
```

### L1 — 情感预测偏差

```markdown
> 🪞 **Aside**
> 顺便一提——你刚才说「买了这个我就能早起跑步了」，
> 很像 情感预测偏差 这个现象（Gilbert & Wilson, 1998）。
> 不一定完全对得上，也许值得回头看一眼。
```

### L2 — 反复确认（跨轮场景）

```markdown
> 🪞 **Aside**
> 顺便一提——注意到你这几轮都在问类似的问题（"这样做对吗"的不同版本），
> 这和 反复寻求确认 这个现象有点像（Abramowitz, 2003）。
> 不一定完全对得上，也许值得回头看一眼。
```

### L2 — 思维反刍（跨轮场景）

```markdown
> 🪞 **Aside**
> 顺便一提——注意到这几轮一直在同一件事上打转（"为什么会变成这样"），
> 这和 思维反刍循环 这个现象有点像（Nolen-Hoeksema, 1991）。
> 不一定完全对得上，也许值得回头看一眼。
```

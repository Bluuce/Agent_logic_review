# Scoring specification / 评分规范

Version: **0.1.0**. This file is the authoritative scoring specification. README tables are summaries. The anchors and examples below define the review; they are not measured model performance.

## What the total measures / 总分测量什么

How fully the available argument and evidence support the **core claim at its stated scope and certainty**, under the explicit task and premises.

总分评价“现有依据对核心主张的支持充分程度”。它不是事实准确率、正确概率、模型置信度、写作质量或句子正确比例。首次显示时写明“总分：…/100”；评分只取五个锚点，不插值、不平均、不按文采加分。

## Anchors / 锚点

| Score / 分数 | Verdict / 结论 | Observable basis / 判断依据 |
| --- | --- | --- |
| **0/100** | Unsupported / 不支持 | A counterexample, contradiction, invalid inference, or decisive calculation error defeats the core argument. 明确反例、矛盾、无效推断或决定性计算错误击穿核心论证。 |
| **25/100** | Unsupported / 不支持 | Available evidence is relevant but lacks a decisive prerequisite or inferential link; no useful narrower version of the core claim is yet established. 依据相关，但缺决定性条件或推理环节，尚未建立核心主张的有意义的较弱版本。 |
| **50/100** | Partially supported / 部分支持 | A meaningful narrower or weaker version is established, but the original materially overstates scope or certainty. 依据已支持有意义的较窄或较弱结论，原文明显扩大范围或确定性。 |
| **75/100** | Supported; local repair needed / 支持，需局部修补 | The core conclusion is supported; an identifiable local reasoning omission can be repaired without new material evidence or changing the core conclusion. 核心结论有支持，局部论证省略可用已有材料补齐，不需新关键证据或改变核心结论。 |
| **100/100** | Fully supported within scope / 在审查范围内支持充分 | The stated premises and evidence support the claim at its actual strength; no material reasoning gap is identified within the review scope. 给定前提和证据足以支持原文结论强度，审查范围内未发现实质缺口。 |
| **Not assessable / 暂不可评分** | Insufficient information / 信息不足 | The core claim, necessary context, or audit material is missing so that classification itself is unreliable. 核心主张、必要上下文或审查材料缺失，无法可靠归类。 No numeric score; in structured exports use `null`. |

## Resolve boundaries / 边界判断

**0 versus 50.** A decisive counterexample to a necessary conclusion takes precedence over the fact that a weaker conclusion might be true. “P implies Q; Q; therefore P necessarily” scores 0. “One listed acceptance scenario passed; therefore the whole listed suite passed” scores 50 if that positive subset is genuinely established and the remaining scenarios are not reported. If a required scenario explicitly failed, the all-passed claim scores 0.

**25 versus 50.** Give 50 only when the evidence establishes a meaningful weaker version of the actual claim, not merely a related fact. A request being accepted does not establish any number of durable database writes; that completion claim scores 25 until persistence is checked. A documented subset of completed acceptance requirements is a meaningful narrower completion claim and can score 50.

**25 versus not assessable.** If the supplied record is explicitly complete and shows the necessary evidence was never obtained, that gap can be scored. If the record may contain omitted supporting material and cannot be inspected, do not infer absence of evidence from the reviewer not seeing it. For “the attached derivation proves X” without the attachment, request the derivation and leave the score unavailable.

**75 versus 100.** Reserve 75 for a specific local reasoning omission. Name it and show how existing material fills it. A stylistic preference or an optional extra citation is not a reason to reduce a supported argument's score. A missing condition that could change the main conclusion is not a minor omission.

**Premises versus inference.** In a hypothetical task, accept stipulated premises and review the conditional inference. In a real-world claim, check the necessary facts against the available evidence. If a conditional argument is valid but its factual premises are unverified, explicitly scope the score to the conditional inference; do not present it as a score for the real-world conclusion.

**Deduction versus probability.** “More likely” can be supported without a guarantee. Do not use a possible exception alone to refute a probabilistic claim. A genuinely necessary or universal statement can be defeated by one valid counterexample. Check whether the claimed confidence or causal strength exceeds the evidence.

**Truth versus support.** A failed argument does not prove its conclusion false. A supplied source can support an answer without establishing that the source is independently true. Name whether the review checked source fidelity, conditional inference, or a factual claim. No score substitutes for that scope.

## Several core claims / 多个关键主张

- Identify independently necessary claims from the task goal and dependency structure; do not invent extra claims to depress the total.
- Score each assessable core claim. The total is their **minimum**, not their mean. This is a conservative aggregation policy, not a mathematical truth about argument quality.
- If any necessary core claim is unassessable, the total is not assessable. Retain known per-claim scores and explicit errors in the report.
- Missing irrelevant details do not invalidate an assessable total. An existing critical error must remain visible even if another core claim is unassessable.
- Optional supporting statements can be mentioned as issues, but do not become separate core claims solely to manufacture a lower total.

总分采用关键主张最低分，是为了让关键依赖中的错误保持可见。没有证据说明所有子项都独立或同等重要，因此不做平均。多个评分也不提供统计意义上的整体正确概率。

## Worked anchors / 锚点示例

These examples are authored specifications, not runtime observations.

| Score | Input and assessment |
| --- | --- |
| 0 | 题设下雨则地湿；地湿，所以必然下雨。洒水反例满足前提但推翻结论，条件被倒置。修正为“下雨是可能解释”。 |
| 25 | 完整记录只有导入请求获受理；服务允许稍后异步失败，没有落库查询。声称100条记录已持久化缺少决定性证据，受理不等于已写入任何条数。 |
| 50 | 验收清单明确要求登录和会话过期都通过；只有登录通过记录，没有会话过期结果。可支持登录场景通过，不能宣布整体验收。 |
| 75 | 题设整数字段满足 x=y、y=3；答复只写“由 y=3 可知 x=3”。结论成立，但引用的理由省略了已给出的 x=y，补上即可。 |
| 100 | 题设所有 A 都是 B，x 是 A，所以 x 是 B。按题设推理成立，无需核验现实中 A 与 B 的关系。 |
| Not assessable | “根据上文推导，方案可行”，但没有上文、方案或可行性要求。不能把未收到的推导判错。 |

## Action is separate / 下一步单独判断

Do not implement an “above N means continue” rule. A score does not supply authorization or make an unchecked prerequisite true. Choose the next step from the actual issue: continue from supported work, repair a faulty inference, or obtain missing evidence. In checkpoint mode update the downstream dependencies after a repair; one repair and one recheck per issue, then disclose unresolved gaps.

## Design source / 设计来源

The typed-judgment and descriptive-anchor approach is inspired by [TypeSafe's Jev introduction](https://docs.typesafe.ai/introduction) and [Score documentation](https://docs.typesafe.ai/primitives/score). This project does not call Jev, reproduce its architecture, or report its probability/confidence outputs. Scores here are rubric judgments made by the host model.

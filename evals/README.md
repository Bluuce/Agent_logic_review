# Evaluation / 评测

This is a small, author-written specification set, not an independent benchmark. / 这是项目作者编写的规格样例，不是独立专家标注或通用性能基准。

| File / 文件 | Review / 回答审查 | Checkpoint / 阶段纠错 | Use / 用途 |
| --- | ---: | ---: | --- |
| [dev.jsonl](dev.jsonl) | 3 | 3 | Develop the instructions / 开发规则 |
| [heldout.jsonl](heldout.jsonl) | 3 | 3 | Final behavior check after freezing the skill / 冻结技能后的行为检查 |

Total: 12 cases, 6 per mode. Every line has an `id`, `mode`, a Chinese `prompt`, and `expected`. Expected fields are `score` (0, 25, 50, 75, 100, or null), `verdict`, and human-readable `checks`.

每条输入彼此独立。规范包含中英标签，但本批原始任务均为中文；英文 README 不代表已完成英文行为评测。公开留出集仅适合当前版本的检查；一旦用于修订规则就属于已看过的数据，新的效果声明需要新的独立案例。

## Run without leaking answers / 防止标准答案泄漏

1. Freeze the skill and record its version or file hashes. Extract **only `prompt`** from one case into a fresh temporary task directory outside this repository. The model must not have the expected answer or other case outputs in its input. Do not ask it to open either JSONL file. / 冻结技能；只向被评模型提供单条 `prompt`，放在仓库外的新临时任务目录中，不让它读取标准答案或其他输出。
2. Run two fresh contexts with the same host model and settings. Baseline: prepend `检查下面任务的逻辑；如需继续，请给出最终结果。` Skill: install only the skill folder in the test project, then prepend `使用 $agent-logic-review 按相应模式处理下面任务。` Do not add the rubric to the baseline. / 固定宿主、模型和设置，分别跑普通提示与技能；普通提示不附量表。
3. No networking or external tools are necessary for these closed cases. Keep comparable model-call/tool budgets, and record any differences. Save raw outputs outside the task context before judging them. / 这些封闭案例不需要外部检索；记录调用和工具预算差异，先保存原始输出再评分。
4. A human reviewer reads `expected` only after inference. Mark each check pass/fail with an exact output quotation. Count `not_observable` separately when an output or execution record is missing. Do not convert a failed or missing run into a reasoning error or a pass. / 运行完成后由人工逐项复核，引用实际输出；缺失记录单独计为不可观察，不能默认为通过或推理错误。
5. For checkpoints, inspect the **final dependent conclusion**, not a promise such as “I checked it.” Record both the score of the original claim and the repaired result; do not substitute the latter's score for the original. / 阶段模式检查最终结论真的更新；区分原主张评分与修正后结果。

The repository itself is accessible to its author, so author review is not blinded or independent expert validation. An independent tester should receive only the skill and extracted prompts, with expected answers retained by the reviewer. / 作者复核不是独立盲评；独立测试者仅接收技能和提取的输入，标准答案由评审者保留。

## Report / 报告

Record host and version, exact model identifier and settings, run date, skill snapshot, case ID, baseline/skill condition, raw output path, score/verdict, per-check decisions, downstream result, tool use, and available token/latency/cost records. Unknown cost is `not available`, not zero. / 记录宿主与版本、实际模型与设置、日期、技能快照、案例、条件、原始输出、判断依据、后续结果及可获得的资源使用；费用未知不能记成零。

Report raw counts with explicit denominators:

- Skill score matches: cases with the exact expected anchor and verdict, including unavailable totals. A baseline without a score is `not_reported`, not an invented zero. / 总分和标签匹配；基线没给分则记未报告。
- Error localization: checkable issue identification on faulty cases. / 对错误的实际定位。
- Repair success: requested correction and correct final dependent result. / 修正及下游结论是否正确。
- Harmful changes: supported original results rejected or changed into incorrect results. / 将正确结果改错或无故拒绝。
- Insufficient-information handling: unavailable totals and specific missing material reported without fabricated evidence. / 正确处理信息不足，不伪造证据。

Use semantic checks to compare conditions fairly; the baseline was not taught this rubric, so exact score matching is not evidence of baseline quality. / 跨条件优劣按语义行为比较；基线未学习本量表，不能用锚点匹配率证明技能优于基线。

Do not aggregate “not observable” into a success percentage. List severe failures (fabricated evidence, endorsing a clearly broken core inference, changing a correct answer into a wrong one). Repair those before claiming behavioral readiness, then add fresh targeted cases; do not rewrite held-out expected answers to fit the model. / 不可观察项单列；严重失败需修正规则并增加新案例，不能改标准答案迁就模型。

## Current evidence / 当前证据

See [RESULTS.md](RESULTS.md). Distinguish package validation, runtime invocation, human review, and independent evaluation. A formatting check alone establishes none of the latter three. / 结构检查、实际调用、人工复核和独立评估分别记录。

# Validation results / 验证记录

Version **0.1.0**, run on **2026-11-03**. Status: **experimental; effectiveness beyond these fixtures not established / 实验性，样例之外的效果尚未确立**.

## Evidence / 实际证据

| Layer / 层次 | Result / 结果 | Scope / 范围 |
| --- | --- | --- |
| Official skill validator / 官方技能结构验证器 | Passed / 通过 | Frontmatter and naming; not reasoning performance / 元数据与命名，不证明推理效果 |
| UI and issue template / UI与反馈模板 | Passed / 通过 | YAML parsing and invocation metadata / YAML解析和调用元数据 |
| Package and links / 包结构与链接 | Passed / 通过 | Relative links resolve; installed references are self-contained / 本地链接有效，安装后引用自包含 |
| Fixture data / 案例数据 | Passed structural checks / 结构检查通过 | 12 unique cases, 3 per mode per split; score/verdict consistency / 12例、每组每模式3例、评分标签一致 |
| Isolated installation / 临时项目安装 | Passed / 通过 | Copy matches source; Codex discovers the skill by name and reads its rubric / 复制一致，Codex按名称发现并读取规范 |
| Codex execution / Codex实际调用 | 24/24 completed / 24次全部完成 | 12 cases × baseline and skill, fresh context per call / 每例分别跑基线和技能，每次新上下文 |
| Author-assistant review / 作者助手复核 | Completed / 已完成 | Quoted checks; not human or independent review / 附原文引用，不是人工或独立复核 |
| Independent human review / 独立人工复核 | Not performed / 未执行 | No independent validation claim / 不声称已独立验证 |
| Other hosts / 其他宿主 | Not tested / 未测试 | No cross-host compatibility claim / 不声称已跨宿主验证 |

The first sandboxed CLI startup failed before inference. A permitted launch then completed one smoke check and the comparison below. Startup failure is not a reasoning error; the separate smoke check is excluded from comparison counts. / 初次受限沙箱启动在推理前失败；获准启动后完成一次冒烟检查，再执行下面的对照。启动失败不算推理错误，额外冒烟调用不混入对照成绩。

## Configuration and records / 配置与记录

- Host / 宿主: **Codex CLI 0.166.0-alpha.5.1**.
- Explicitly requested model / 显式指定模型: **gpt-6.1-sol**, reasoning effort **medium**; temperature not overridden / 未覆盖温度设置.
- Fresh temporary project and context per call; `--ignore-user-config`, `--ephemeral`, `--sandbox read-only`, `--skip-git-repo-check`. Baseline receives no project skill; skill condition receives only the package, not expected-answer files. / 每次新临时项目和上下文；基线不安装本项目技能，技能组只收到安装包，不含标准答案文件。
- Six development cases followed by six held-out cases; one sample per case per condition, up to four concurrent calls. Package hashes were frozen throughout. / 先开发集再留出集，每条件每例一次，最多4次并发，运行期间技能哈希未变。
- [Configuration, command template and SHA-256 hashes / 配置、命令模板与哈希](runs/2026-11-03/manifest.json).
- [24 raw assistant-message, command and usage records / 24次实际消息、命令及用量](runs/2026-11-03/outputs.jsonl). Messages preserve emitted assistant text; command records omit full private CLI telemetry. / 保留实际助手消息；命令记录不包含完整私有CLI遥测。
- [Per-check quotations and author-assistant assessments / 逐项引用及作者助手判断](runs/2026-11-03/author-review.json).

The evaluated model did not receive expected answers. The same assistant authored the fixtures and assessed outputs, so this is not blinded or independent expert evaluation. / 被评模型未收到标准答案；编写案例与复核结果的是同一作者助手，因此不是盲评或独立专家评测。

## Observations / 观察结果

These are **author-assistant assessments of authored fixtures**, not population accuracy estimates. / 以下仅为作者助手对规格样例的判断，不是总体准确率估计。

| Check / 检查 | Baseline / 普通提示 | Skill / 启用技能 |
| --- | --- | --- |
| Key semantic outcome aligned / 关键语义结果符合作者预期 | 12/12 | 12/12 |
| Numeric anchor and verdict match / 数值锚点与标签符合规范 | Not assessed; baseline not given the rubric / 未要求基线遵守本量表 | 10/10 assessable cases / 10个可评分案例 |
| Necessary unavailable total stays unavailable / 必要材料缺失时总分留空 | Not assessed as rubric compliance / 不按本规范评基线 | 2/2 |
| Correct final dependent state at checkpoints / 阶段案例最终依赖结论正确 | 6/6 | 6/6 |
| Harmful changes on fully supported originals / 将完全支持的原结果改错 | 0/3 | 0/3 |

No fabricated evidence or endorsement of an explicitly broken core inference was observed in this run. Held-out cases cover probabilistic support, the 75-point boundary, minimum aggregation, preserving a correct intermediate result, explicit failed checks, and an unassessable necessary claim. / 本批未观察到伪造证据或认可明确错误核心推断。留出集覆盖概率主张、75分边界、最低分汇总、保留正确中间结果、明确失败不能放行及必要主张无法评估。

The baseline also reached appropriate semantic conclusions. It chose its own scales, including 10-point and 2-point scales, and sometimes averaged claims. In the package-version case it correctly withheld overall acceptance but gave 50% as an evidence-confirmation ratio; the skill left the total unavailable. These differences show rubric conformance, **not an established accuracy improvement**. / 普通提示也得出了合适的语义结论，但自行选择10分制、2分制或平均方法。版本案例中，基线正确保留整体验收，却给出50%的证据确认比例；技能保留不可评分总分。这体现评分规范一致性，**未证明准确率提高**。

Usage for the 24 comparison calls / 24次对照调用报告的用量：

| Condition / 条件 | Input tokens / 输入token | Output tokens / 输出token | Command records / 工具命令记录 |
| --- | ---: | ---: | ---: |
| Baseline | 179,439 | 2,754 | 0 |
| Skill | 431,051 | 3,688 | 12 |

Cache counters and wall times are in the raw records. Token totals include host/context and skill-reading overhead; this is not an identical-token-budget comparison. Wall time includes CLI startup and tools, not just model latency. **Money cost is not available, not zero.** / 缓存与墙钟时间见原始记录；输入包含宿主上下文和读取技能的额外开销，两组并非等token预算。墙钟时间含CLI启动与工具，不是纯模型延迟。**费用不可得，不记为零。**

## Release interpretation / 首版状态

Packaging, isolated Codex invocation, and the recorded fixture behaviors are checked. English behavior, repeated-run stability, independent human grading, broad-domain effectiveness, and other hosts remain unverified. / 包结构、隔离Codex调用及本批规格行为已有记录；英文行为、重复稳定性、独立人工评分、跨领域效果和其他宿主尚未验证。

Use the small set to inspect the contract, not advertise universal reliability or probability calibration. README and rubric examples remain design illustrations; only the linked runtime records are measured outputs. / 本组用于检查约定，不能宣传通用可靠性或概率校准。README与规范示例仍是设计说明；只有上方实际运行记录是实测输出。

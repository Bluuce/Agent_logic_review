# Agent Logic Review

[简体中文](README.md) · Experimental **v0.1.0** · [MIT](LICENSE)

Check what the evidence actually supports before an agent adopts a key conclusion or declares a task complete. Get **a total out of 100, checkable reasons, and the smallest justified repair**. Also review existing AI answers.

## An example

**Authored design example, not a measured model output.** Acceptance requires both normal login and session expiry to pass. An agent observes only a successful login test and announces that authentication has been fully accepted.

> **Total: 50/100 · Partially supported**  
> **Core claim:** Authentication has passed all acceptance checks.  
> **Evidence and issue:** The login result supports the login scenario. Session expiry is also required, but the complete supplied record contains no result for it. Full acceptance is not established.  
> **Minimal repair:** Normal login passed; full acceptance still lacks a session-expiry result.  
> **Next step:** Preserve the login result, check session expiry, and then reassess full acceptance.

## Use cases

- **Review:** Check the material inferences in an answer, argument, plan, or completion claim.
- **Checkpoint:** Self-check before relying on an intermediate conclusion; update affected downstream conclusions after a repair.

Version 0.1 focuses on evidence-to-conclusion gaps: a local test presented as full acceptance, a successful tool call presented as goal achievement, an unchecked premise used to drive action, and an old plan surviving correction of its inputs.

This skill guides the current agent. Discovery and invocation depend on the host. It does not enforce every checkpoint, run a background monitor, or access hidden chain-of-thought. There is no model training, Jev API, external service, or automatic hook. Tools and permissions come from the host; normal model usage remains subject to the host's account and pricing rules.

## Install in Codex

Codex is the first validation target; see the actual status in [evaluation results](evals/RESULTS.md). Other hosts have not been verified.

Download or clone this repository, then run these commands from its root. Replace `/path/to/target-project` with your actual project directory. This example uses a macOS/Linux shell:

```sh
mkdir -p "/path/to/target-project/.agents/skills"
cp -R ./agent-logic-review "/path/to/target-project/.agents/skills/"
```

Copy only the `agent-logic-review/` skill directory, preserving its internal relative paths. `evals/` is not a runtime dependency. If the destination already contains a skill with this name, compare it before updating. Confirm that Codex discovers `$agent-logic-review`; reload or restart according to the host's instructions if needed. The project skill location follows the [official skills documentation](https://learn.chatgpt.com/docs/build-skills).

## Invoke

**Review an answer:**

```text
Use $agent-logic-review to review this answer. Give a total out of 100,
checkable reasons, and the smallest justified repair.
Question: Has authentication passed all acceptance checks?
Requirements: Both login and session-expiry tests must pass.
Complete test record: Only the login test ran and passed. Session expiry was not tested.
AI answer: Login passed, so authentication has passed all acceptance checks.
```

**Correct at a checkpoint:**

```text
Use $agent-logic-review in checkpoint mode to finish this task.
Check key assumptions, intermediate results, and completion claims before relying on them.
Show only concise, checkable reasons.
Assume 6 parts per box, 3 boxes, and 3 parts per person.
My intermediate result is 3×6=21, so 7 people can receive parts.
Check first, then give the final number of people.
```

The second example's original intermediate claim should score **0/100**, followed by a correction to 18 parts and 6 people. The score describes the original claim; repairing it does not retroactively change its score to 100.

The default card has five fields: **Total, Core claim, Evidence and issue, Minimal repair, Next step**. Recognize sound arguments without inventing errors. If necessary review material is missing, identify exactly what is needed.

## Read the score

The total measures **how fully the stated premises and available evidence support the core claim at its actual scope and certainty**. It is not a correctness probability, confidence estimate, or writing score.

| Total | Meaning |
| --- | --- |
| **0/100** | A counterexample, contradiction, invalid inference, or decisive calculation error defeats the core argument |
| **25/100** | Relevant evidence lacks a decisive prerequisite or inferential link; the core claim is not established |
| **50/100** | A meaningful narrower or weaker claim is supported; the original overstates its scope or certainty |
| **75/100** | The core conclusion is supported, with a local reasoning omission repairable from existing material |
| **100/100** | The evidence supports the stated strength of the conclusion within the review scope |
| **Not assessable** | Necessary context or material is missing; no numeric score |

Use only these five scores, without interpolation or invented confidence. For several core claims, report the minimum; a necessary unassessable claim makes the total unavailable while known errors remain visible. **Zero does not prove the conclusion false; 100 does not guarantee that all real-world premises are true.** A hypothetical argument may earn full support under stipulated premises. Report factual verification separately. See the authoritative [scoring specification](agent-logic-review/references/rubric.md) for boundary cases.

There is no universal “80 means proceed” gate. Continue, repair, or obtain evidence according to the actual issue. For the same checkpoint issue, make at most one repair and one recheck before reporting any unresolved gap.

## Evaluation and limits

The initial [case set](evals/README.md) contains 12 cases: 6 reviews and 6 checkpoints, with 6 development and 6 held-out cases. These are author-written specification fixtures, not independently expert-labeled data or a general benchmark. Never pass expected answers to the model being evaluated.

Fixtures and structural checks do not establish behavioral effectiveness. Actual run status, raw output locations, and failures are recorded in [evaluation results](evals/RESULTS.md). Independent review, baseline comparisons, and cross-host validation count only when performed. No accuracy improvement is claimed.

Limits include missed conditions during claim extraction, judgment calls at boundaries such as 75 versus 100, host-model mistakes or skipped self-checks, and review restricted to supplied material when external verification tools are absent. A total cannot replace appropriate evidence for factual, mathematical, or specialized claims.

## Contribute and references

[Submit a misjudgment](.github/ISSUE_TEMPLATE/misjudgment.md) with the original input, actual output, expected judgment, and checkable evidence. Remove credentials and material you cannot publish. Keep translations, paraphrases, and cases from the same source in one split to avoid leakage.

The design borrows typed decisions and descriptive anchors from Jev: [TypeSafe introduction](https://docs.typesafe.ai/introduction), [Score documentation](https://docs.typesafe.ai/primitives/score). This project does not call Jev, reproduce its architecture, or provide Jev probabilities. Skill packaging follows the [official skills documentation](https://learn.chatgpt.com/docs/build-skills).

Skill instructions, documentation, and original cases are provided under the [MIT license](LICENSE).

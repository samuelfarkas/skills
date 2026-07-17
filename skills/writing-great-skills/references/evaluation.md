# Evaluation Protocol for Agent Skills

Use this protocol for substantial skill creation or revision. Small typo-only changes need mechanical validation, not a new benchmark.

## Contents

- [Define cases and pilot the baseline](#define-cases-and-pilot-the-baseline)
- [Separate discovery from execution](#separate-discovery-from-execution)
- [Tune descriptions without overfitting](#tune-descriptions-without-overfitting)
- [Freeze and isolate variants](#freeze-and-isolate-variants)
- [Grade in layers](#grade-in-layers)
- [Metrics](#metrics)
- [Release gates](#release-gates)
- [Artifacts and reporting](#artifacts-and-reporting)

## Define cases and pilot the baseline

Start from real requests, incidents, review findings, or observed baseline gaps. Before drafting the candidate, record realistic prompts, input files, provenance, and a human-readable expected outcome. Do not construct a benchmark around prose already written for the candidate.

Each case should record:

```yaml
id: stable-case-id
kind: discovery | execution | safety | compatibility
request: exact user-facing prompt
setup: files, tools, permissions, and environment
expected: observable outcome
assertions:
  - observable pass/fail check, added or refined after the baseline pilot
critical_failures:
  - behavior that fails the release regardless of average score
notes: provenance and why the case is representative
```

Use at least three execution cases for a smoke suite and more when the skill has independent branches. Cover the common path, a difficult boundary, and a safety or failure path. Include a real task beyond synthetic fixtures.

Run variant A before scoring B. A may reveal what an observable assertion should say, expose a broken case, or show that the skill is unnecessary. Refine assertions using the task contract, domain evidence, and A's pilot outputs; do not use B's outputs. Freeze assertions, critical failures, and the release gate before inspecting or grading B. If an assertion is added after seeing B, label it as a development diagnostic and reserve a new held-out case for release evidence.

## Separate discovery from execution

Discovery evaluation tests only metadata and selection policy. Include:

- direct positive request;
- paraphrased positive request;
- near-miss that should not select the skill;
- unrelated negative request;
- request where another skill is the better match;
- explicit invocation, when supported.

Execution evaluation explicitly supplies the skill so every variant receives the same body exposure. Otherwise a routing miss can masquerade as an execution failure, or explicit invocation can conceal a bad description.

Use client-native evidence of loading—a skill tool call, trace event, or loader log—for routing results. Similar output is not proof that a skill activated. A catalog-choice simulation is useful for cheap preflight, but it is not a substitute for a native trigger evaluation.

## Tune descriptions without overfitting

For a description revision, start with roughly twenty realistic routing queries: about eight to ten should-trigger prompts and eight to ten should-not-trigger prompts. Favor difficult paraphrases and near-misses over obviously unrelated negatives. Include file paths, casual language, typos, buried intent, and adjacent skills when those occur in real use.

Run each query at least three times and record the native trigger rate. Keep a fixed, stratified split—roughly 60% development and 40% validation. Use only development failures to revise the description; choose the final description by validation performance. Never paste validation queries, labels, or results into the revision prompt. Expand trials or the held-out set when results are close or routing errors are costly.

## Freeze and isolate variants

1. Snapshot variant A before drafting B. A may be the current skill or no skill.
2. Keep task prompts, environment, model, tools, permissions, and budgets equal.
3. Run each trial in a fresh session with no conversation carryover.
4. Do not reveal variant labels, expected answers, goldens, prior failures, or candidate design rationale to the solver.
5. Keep description-validation queries and held-out execution cases outside the candidate revision context.
6. Counterbalance or randomize run order where resource drift or caching could matter.
7. Preserve raw outputs and traces before grading.
8. Use at least three independent trials per stochastic case and condition. Increase trials when results are close, unstable, costly to reverse, or safety-critical.

When using subagents, give each a fresh context and only the skill path, task, necessary fixtures, and tool limits. Do not let one solver grade its own output.

## Grade in layers

Apply graders in this order:

1. structural checks: required files, parseable output, schema, links, exit codes;
2. deterministic behavioral assertions: exact facts, prohibited actions, file changes, tests;
3. safety and boundary assertions: approvals, least privilege, secret handling, side effects;
4. blind qualitative rubric: only for criteria that cannot be made deterministic;
5. transcript review: file navigation, ignored instructions, tool misuse, premature completion, and accidental leakage.

A model judge must receive anonymized outputs in varied order, a concrete rubric, and no knowledge of which variant produced which output. Calibrate it against a small human-graded sample. Do not use eloquence as a proxy for correctness.

## Metrics

Report counts and denominators, not only percentages:

- discovery recall on positive cases;
- discovery precision or false-positive rate on negative and competing cases;
- deterministic assertion pass rate;
- first-trial and all-trials task success;
- critical and non-critical safety violations;
- median and tail latency;
- tokens or cost when available;
- variance across trials, models, and clients;
- regressions by case, even when the average improves.

A small suite estimates behavior; it does not prove universal reliability. Distinguish observed results, inference, and unknowns.

## Release gates

Declare the gate before running B. A reasonable default for a non-safety-critical revision is:

- all mechanical validators pass;
- every critical assertion passes in every trial;
- no new safety violation;
- discovery recall improves or stays within the accepted band without a material precision loss;
- execution success improves over A on the targeted failures and does not regress an unrelated case;
- resource cost is within the declared budget;
- any tie is reported as a tie, not a win.

For high-risk skills, require more cases and trials, stricter bounds, human review, and sandboxed testing. If B fails, inspect the smallest causal hypothesis, change one design variable, and rerun affected cases plus a regression set.

## Artifacts and reporting

Retain:

- frozen variants and their hashes;
- case definitions, train/validation membership, and provenance;
- exact prompts and environment metadata;
- raw outputs, traces, errors, timing, and usage;
- grader implementations and versions;
- per-trial scores and critical failures;
- aggregate comparison, limitations, and decision.

A reproducible result names the model and client, date, trial count, runner command, and known sources of variance. Never overwrite an earlier result after iteration; write a new run and explain the delta.

Primary guidance: [Agent Skills creator best practices](https://agentskills.io/skill-creation/best-practices), [Optimizing skill descriptions](https://agentskills.io/skill-creation/optimizing-descriptions), [Evaluating skill output quality](https://agentskills.io/skill-creation/evaluating-skills), [Claude Code skills](https://code.claude.com/docs/en/skills), [Codex: Build skills](https://learn.chatgpt.com/docs/build-skills), and the [Agent Skills specification](https://agentskills.io/specification).

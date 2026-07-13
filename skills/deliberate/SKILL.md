---
name: deliberate
description: Deliberate before implementing a goal or ticket with a meaningful design choice. Use when the user invokes $deliberate or asks to brainstorm, compare, steelman, or red-team implementation approaches.
---

# Deliberate

Investigate before choosing. Compare credible approaches, select the best-supported one, then implement it when the request authorizes implementation. Communicate only decision-relevant evidence and rationale.

## Scale the depth

- Use a brief pass for low-risk, reversible choices. Use the full workflow when multiple credible paths exist and the choice matters; strengthen evidence and testing as risk increases.
- Stop once the choice is stable and material risks are addressed.

## Workflow

### 1. Reconnoiter

- Inspect the relevant system, code, documentation, logs, or other available evidence before proposing changes.
- Identify the desired outcome, success criteria, and hard constraints.
- Distinguish known facts from assumptions and material unknowns.
- Ask the user when undiscoverable information could change the decision.
- When a material decision depends on user intent or authority, resolve it before dependent decisions: ask one focused question, give a recommendation when it stays within scope or a reversible holding action when it does not, state the main tradeoff, then wait for the answer.
- Before generating alternatives, make the outcome, hard constraints, comparison criteria, and decision-changing unknowns explicit.

### 2. Generate alternatives

- Generate materially distinct alternatives before ranking them; stop when another would repeat a mechanism or tradeoff. For a trivial change with no meaningful alternative, keep the obvious path and continue.

### 3. Steelman

- Steelman each serious candidate until its favorable conditions, strongest case, and requirements for success are explicit.

### 4. Red-team

- For each serious candidate—including whichever currently appears strongest—identify its most plausible failure path and key assumptions.
- Focus on plausible, task-relevant risks.
- Treat unsupported concerns as hypotheses. Test each decision-changing one with a small, safe discriminating test when feasible; otherwise carry it as explicit uncertainty.

### 5. Decide

- Filter out candidates that fail hard constraints, then compare the survivors against the outcome and task-relevant criteria.
- Choose the approach that best fits the objective and constraints given the available evidence. Combine candidates when their parts are compatible and the combination addresses a real tradeoff.
- Prefer a reversible first step when the leading options are close.
- State why the choice fits, what material uncertainty could change it, and how to verify the result. Add rollback or revisit conditions when the risk warrants them.
- Proceed when implementation is authorized. When the request is for analysis or planning, make the decision artifact the final deliverable.

## Communicate the result

- Present the conclusion and the evidence needed to assess it. Narrate the phases when the user asks for the process.
- For a substantial decision, summarize the context and unknowns, credible options with their strongest case and material risk, the selected approach, and its verification plan.

## Guardrails

- Calibrate confidence to evidence quality rather than fluency.
- Investigate material unknowns revealed by critique. Generate new candidates when every current candidate fails a hard constraint.
- Use task-specific skills for domain execution and this skill for choosing among approaches.

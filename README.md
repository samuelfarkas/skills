# Agent Skills

## Available skills

### `deliberate`

Investigate meaningful design choices before implementing. The skill compares credible approaches, steelmans and red-teams them, then selects the best-supported path.

Use it when you want an agent to brainstorm, compare approaches, challenge a proposed design, or make a consequential implementation decision.

### `writing-great-skills` (work in progress)

Designs and audits portable Agent Skills with explicit safety, runtime, validation, and A/B evaluation guidance.

Inspired by [Matt Pocock's `writing-great-skills`](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-great-skills). His original is a compact conceptual guide to predictable skill design; this version is a more operational, cross-client process for Codex, Claude Code, and OpenCode. It is still being refined.

## Install

Install `deliberate` in the current project:

```sh
npx skills add samuelfarkas/skills --skill deliberate
```

Install `writing-great-skills`:

```sh
npx skills add samuelfarkas/skills --skill writing-great-skills
```

# Agent skills

## Available skills

### `deliberate`

Use this before coding when there is more than one reasonable way to solve a problem. It compares the options, makes the strongest case for each one, looks for likely failure points, and recommends an approach.

### `show-me`

Use this when a diagram would explain something faster than another page of text. It creates compact Markdown or plain-text sketches for flows, architecture, state, ownership, layouts, and code changes. These work in terminals and tools without Mermaid support, including OpenCode. For more complex subjects, it can create an HTML diagram, infographic, or short slide deck.

This is a tweak of [Dex Horthy's `show-me` skill](https://x.com/dexhorthy/status/2087569590268391897).

### `writing-great-skills` (work in progress)

Use this to create or review skills that work across different coding agents. It covers safety, runtime behavior, validation, and A/B testing.

Inspired by [Matt Pocock's `writing-great-skills`](https://github.com/mattpocock/skills/tree/main/skills/productivity/writing-great-skills). His version is a short guide to writing predictable skills. This one adds a practical process for Codex, Claude Code, and OpenCode. It is still being refined.

## Install

Install `deliberate` in the current project:

```sh
npx skills add samuelfarkas/skills --skill deliberate
```

Install `show-me`:

```sh
npx skills add samuelfarkas/skills --skill show-me
```

Install `writing-great-skills`:

```sh
npx skills add samuelfarkas/skills --skill writing-great-skills
```

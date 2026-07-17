# Portable Agent Skills Contract

Last verified: 2026-07-14. Recheck owner documentation before relying on host-specific behavior; invocation controls and search paths can change independently of the open format.

## Contents

- [Portable core](#portable-core)
- [Description and discovery](#description-and-discovery)
- [Host matrix](#host-matrix)
- [Portability rules](#portability-rules)
- [Validation](#validation)
- [Primary sources](#primary-sources)

## Portable core

A skill is a directory containing `SKILL.md`. The open Agent Skills format requires YAML frontmatter with:

- `name`: lowercase letters, digits, and hyphens; 1–64 characters; no leading, trailing, or consecutive hyphens; must match the directory name;
- `description`: 1–1024 characters describing both what the skill does and when to use it.

The format also defines optional `license`, `compatibility`, `metadata`, and `allowed-tools` fields. Not every client interprets optional fields identically. For a shared skill, use only fields that every declared client accepts or explicitly document the reduced compatibility.

The body is Markdown. Use relative paths from the skill directory. Keep file references direct and avoid deeply nested disclosure chains.

## Description and discovery

The description is selection metadata, not a body summary. It must stand alone before `SKILL.md` is loaded.

A strong description contains:

1. capability: the concrete transformation or decision the skill performs;
2. trigger: the requests, artifacts, or conditions that should select it;
3. boundary: a discriminating exclusion when adjacent skills would otherwise compete.

Use terms that occur in real requests and project artifacts. Evaluate paraphrases rather than stuffing synonyms. Test the description against positive, near-miss, negative, explicit-invocation, and competing-skill prompts.

## Host matrix

| Concern | Claude Code | Codex | OpenCode |
|---|---|---|---|
| Shared discovery metadata | `name`, `description` | `name`, `description` | `name`, `description` |
| Repository discovery paths | `.claude/skills/<name>/` | `.agents/skills/<name>/` | `.opencode/skills/<name>/`, `.claude/skills/<name>/`, or `.agents/skills/<name>/` |
| User discovery paths | `~/.claude/skills/<name>/` | `~/.agents/skills/<name>/` | `~/.config/opencode/skills/<name>/`, `~/.claude/skills/<name>/`, or `~/.agents/skills/<name>/` |
| Explicit invocation | `/skill-name` or skill UI | `$skill-name` or skill UI | Skill tool or client UI |
| Per-skill implicit policy | `disable-model-invocation` and settings controls | `agents/openai.yaml` → `policy.allow_implicit_invocation` | Skill permission rules support `allow`, `ask`, or `deny`; no equivalent portable frontmatter switch is documented |
| Unknown frontmatter | Supports additional Claude fields | Put Codex-only fields in `agents/openai.yaml` | Documentation says unknown fields are ignored |

Host policy is not part of the portable semantic contract. A Claude-only field in canonical frontmatter may fail a strict portable validator and may be ignored elsewhere. A Codex sidecar should not be treated as instructions for another client.

A canonical source directory is not automatically a discovery path. A managed store may keep one package under `.agents/skills` and link it into Claude's documented `.claude/skills` location, as this repository does. Test the installed or linked paths each client actually scans. Version-gate newer settings behavior such as Claude Code `skillOverrides` before relying on it.

## Portability rules

1. Keep the canonical `SKILL.md` portable; place client-only UI, policy, and tool declarations in documented sidecars or repository settings.
2. Declare the intended clients and test their real loaders. Conformance to the open format does not prove identical runtime behavior.
3. If one behavior has no equivalent on a client, choose a documented fallback or narrow the compatibility claim. Do not invent a universal abstraction.
4. Prefer the shared `.agents/skills` location when the repository intentionally serves multiple clients. Use links or a managed canonical store rather than copied bodies when the environment supports it.
5. Keep host facts in this reference or an adapter, not scattered through the generic workflow.
6. Record the verification date and revisit after client updates.
7. Audit runtime mechanisms one-to-one: resource-root resolution, temporary and PID state, browser launch, port selection, process ownership, dependency isolation, and cleanup. Replace client variables, fixed global paths, and operating-system commands with documented host capabilities or safe stops.
8. When a package contains several host assumptions, report and remediate every occurrence rather than treating one representative example as complete coverage.

## Validation

Use the official `skills-ref` validator or an equivalent implementation of the open specification. Then run every declared host's own diagnostic or metadata listing and an invocation smoke test. Check at least:

- directory and `name` agreement;
- accepted frontmatter and field lengths;
- description visibility;
- direct reference paths;
- sidecar schema;
- explicit and, when claimed, implicit invocation.

## Primary sources

- [Agent Skills specification](https://agentskills.io/specification)
- [Agent Skills creator best practices](https://agentskills.io/skill-creation/best-practices)
- [Optimizing skill descriptions](https://agentskills.io/skill-creation/optimizing-descriptions)
- [Evaluating skill output quality](https://agentskills.io/skill-creation/evaluating-skills)
- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [Claude Agent Skills authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [Codex: Build skills](https://learn.chatgpt.com/docs/build-skills)
- [OpenCode skills](https://opencode.ai/docs/skills/)

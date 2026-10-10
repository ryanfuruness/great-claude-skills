# Great Claude Skills

A small collection of [Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) for Claude, each one refined through real use.

| Skill | What it does |
|---|---|
| [`skill-writing`](skills/skill-writing/SKILL.md) | Guidance for writing the text of a skill so it captures the *thinking* that produced a good result, not a checklist of what one session happened to find. Pairs well with Anthropic's [`skill-creator`](https://github.com/anthropics/skills/tree/main/skills/skill-creator), which covers structure, packaging and evals. |
| [`chrome-walkthrough`](skills/chrome-walkthrough/SKILL.md) | Teaches you to do a task yourself in any web app. Claude plans the expert route, then overlays a coaching panel on the live page: a highlighted target, one step at a time, a tip that teaches the reusable pattern, and auto-advance when each step is done. Say "go" and Claude does the current step for you. Requires [Claude in Chrome](https://claude.ai/chrome). |
| [`human-handoff`](skills/human-handoff/SKILL.md) | A final pass before an AI-generated document, deck or report goes to people. Claude reads it the way a first-time reader would, in order and tracking only what has been introduced so far, then fixes what that exposes: terms used before they are explained, working shorthand from the making, pages carrying too much, and rendering defects. For work someone will act on, it brings in fresh reviewers who see the pages one at a time. |

## Install

**Claude Code** — add this repo as a plugin marketplace, then install whichever skills you want:

```
/plugin marketplace add ryanfuruness/great-claude-skills
/plugin install skill-writing@great-claude-skills
/plugin install chrome-walkthrough@great-claude-skills
/plugin install human-handoff@great-claude-skills
```

Or copy a skill folder into `~/.claude/skills/` (personal) or `.claude/skills/` (per project).

**Claude apps (claude.ai, desktop, Cowork)** — zip a skill folder (e.g. `skills/chrome-walkthrough/`) and upload it in the Skills section of Claude's settings.

## License

[MIT](LICENSE)

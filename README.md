# Great Claude Skills

A small collection of [Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) for Claude, each one refined through real use.

| Skill | What it does |
|---|---|
| [`skill-writing`](skills/skill-writing/SKILL.md) | Guidance for writing the text of a skill so it captures the *thinking* that produced a good result, not a checklist of what one session happened to find. Pairs well with Anthropic's [`skill-creator`](https://github.com/anthropics/skills/tree/main/skills/skill-creator), which covers structure, packaging and evals. |
| [`chrome-walkthrough`](skills/chrome-walkthrough/SKILL.md) | Teaches you to do a task yourself in any web app. Claude plans the expert route, then overlays a coaching panel on the live page: a highlighted target, one step at a time, a tip that teaches the reusable pattern, and auto-advance when each step is done. Say "go" and Claude does the current step for you. Requires [Claude in Chrome](https://claude.ai/chrome). |
| [`human-handoff`](skills/human-handoff/SKILL.md) | Makes AI-generated documents, decks and reports land with the people who read them. Claude writes the artifact fresh from a short brief so the conversation that produced it doesn't leak in, reads it the way a first-time reader would to catch terms used before they're explained, cuts what this reader wouldn't miss, and rewrites any sentence you wouldn't say out loud. For work someone will act on, fresh reviewers read it page by page. Questions about the thinking itself go back to you rather than being smoothed over. |

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

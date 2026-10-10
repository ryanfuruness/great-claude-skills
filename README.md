# Great Claude Skills

A small collection of [Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) for Claude, each one refined through real use.

| Skill | What it does |
|---|---|
| [`skill-writing`](skills/skill-writing/SKILL.md) | Guidance for writing the text of a skill so it captures the *thinking* that produced a good result, not a checklist of what one session happened to find. Pairs well with Anthropic's [`skill-creator`](https://github.com/anthropics/skills/tree/main/skills/skill-creator), which covers structure, packaging and evals. |
| [`chrome-walkthrough`](skills/chrome-walkthrough/SKILL.md) | Teaches you to do a task yourself in any web app. Claude plans the expert route, then overlays a coaching panel on the live page: a highlighted target, one step at a time, a tip that teaches the reusable pattern, and auto-advance when each step is done. Say "go" and Claude does the current step for you. Requires [Claude in Chrome](https://claude.ai/chrome). |
| [`north-star`](skills/north-star/SKILL.md) | Sets the standard for what good looks like before the material can shape it. A fresh agent is given a one-line situation, never the content, and asked what an expert at that kind of work holds in their head. The answer is the target when Claude produces something and the yardstick when it reviews, so a review catches what is missing as well as what is wrong. |
| [`clean-final-pass`](skills/clean-final-pass/SKILL.md) | For producing a final artifact from a chat full of context. Instead of editing what accumulated, Claude writes a purpose-led brief of what was decided and has a fresh writer produce the artifact from it, so the conversation's corrections, detours and order don't carry over. The draft is then checked for fidelity in both directions: nothing that mattered lost, nothing undecided added. |
| [`human-handoff`](skills/human-handoff/SKILL.md) | Makes settled thinking land with the people who will read it. Claude reads the piece as a first-time reader would, in order and tracking only what has been introduced so far, then fixes reading order, how much each section carries, sentences that don't sound said, and visual structure that implies meaning it doesn't have. It hands back finished work, not a list of findings. |

`north-star`, `clean-final-pass` and `human-handoff` are designed to work together on substantial work: the standard first, then a clean write of what was decided, then a pass for the reader. Each also stands on its own. The [`briefs/`](briefs) folder holds the brief each of the three was written from, as the source for future changes.

## Install

**Claude Code** — add this repo as a plugin marketplace, then install whichever skills you want:

```
/plugin marketplace add ryanfuruness/great-claude-skills
/plugin install skill-writing@great-claude-skills
/plugin install chrome-walkthrough@great-claude-skills
/plugin install north-star@great-claude-skills
/plugin install clean-final-pass@great-claude-skills
/plugin install human-handoff@great-claude-skills
```

Or copy a skill folder into `~/.claude/skills/` (personal) or `.claude/skills/` (per project).

**Claude apps (claude.ai, desktop, Cowork)** — zip a skill folder (e.g. `skills/chrome-walkthrough/`) and upload it in the Skills section of Claude's settings.

## Personal instructions template

[`personal-instructions/template.md`](personal-instructions/template.md) is a template for the personal preferences Claude reads in every conversation (Settings → Profile in the Claude apps). It isn't a skill. A skill loads when a task calls for it; these instructions are always present, so they state core ideas about how to work together and leave the how to Claude's judgement: Claude as a collaborator rather than an executor, questions taken as real questions, and artifacts written clean of the conversation that produced them. Fill in the bracketed lines and replace `[Name]`. If you add your own, keep each addition to the idea rather than a procedure, since anything here shapes every chat.

It pairs with `clean-final-pass` and `human-handoff`: the template carries the idea in every conversation, and the skills do the full pass when a final artifact is produced or work is about to go to people.

## License

[MIT](LICENSE)

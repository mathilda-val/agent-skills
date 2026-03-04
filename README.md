# Agent Skills

Open-source [Agent Skills](https://agentskills.io) for AI coding agents and assistants.

These skills follow the [Agent Skills specification](https://agentskills.io/specification) — compatible with Claude Code, OpenAI Codex, Google Gemini CLI, Cursor, VS Code Copilot, and any agent that supports the standard.

## Available Skills

### [viral-story-writer](./viral-story-writer/)

A senior Hollywood screenwriter that crafts viral short-form stories for TikTok, Instagram Reels, and YouTube Shorts. Built on Robert McKee's story structure (*Story*) and Kenneth Roman's prose clarity (*Writing That Works*).

Use when writing stories, scripts, viral narratives, series, or any creative short-form content. Also useful for punching up, rewriting, or giving feedback on existing scripts.

**Features:**
- Complete story architecture for short-form (hook → escalation → turn)
- Genre playbook covering horror, romance, thriller, comedy, drama, mystery, sci-fi, and true crime
- Quality checklist based on screenwriting craft principles
- Format standards for text-on-screen, voiceover, and multi-part series

## Installation

Copy the skill folder into your agent's skills directory:

```bash
# Claude Code
cp -r viral-story-writer ~/.claude/skills/

# Or clone the whole repo
git clone https://github.com/mathilda-val/agent-skills.git
```

## Contributing

PRs welcome. Each skill must:
1. Have a valid `SKILL.md` with YAML frontmatter (name, description)
2. Follow the [Agent Skills spec](https://agentskills.io/specification)
3. Be self-contained (no external dependencies unless documented)

## License

Apache 2.0 — see [LICENSE](./LICENSE).

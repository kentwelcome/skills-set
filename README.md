# Personal Skills

This repository is the source of truth for Kent's personal agent skills. The
complete collection is distributed as one `skills-set` plugin for Claude Code
and Codex.

## Layout

The plugin lives at `plugins/skills-set/`. Each canonical skill lives in
`plugins/skills-set/skills/<skill-name>/` and contains a `SKILL.md` file.
The top-level `skills` symlink preserves existing local paths.

## Install all skills

### Claude Code

```text
/plugin marketplace add kentwelcome/skills-set
/plugin install skills-set@skills-set
```

Claude Code exposes plugin skills under the `skills-set` namespace, such as
`/skills-set:kent-slidev`.

### Codex

```sh
codex plugin marketplace add kentwelcome/skills-set
codex plugin add skills-set@skills-set
```

## Skills

- [`kent-slidev`](plugins/skills-set/skills/kent-slidev/SKILL.md) — Create and refine Slidev presentations in Kent's preferred working style.
- [`skillset-smith`](plugins/skills-set/skills/skillset-smith/SKILL.md) — Create or update personal skills from the current session and open a pull request.

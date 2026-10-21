# Personal Skills

This repository is the source of truth for Kent's personal agent skills. The
complete collection is distributed as one `skills-set` plugin for Claude Code,
Codex, and OMP.

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

### OMP

Run these commands inside OMP:

```text
/marketplace add kentwelcome/skills-set
/marketplace install skills-set@skills-set
/reload-plugins
```

Or install from the terminal:

```sh
omp plugin marketplace add kentwelcome/skills-set
omp plugin install skills-set@skills-set
```

For an already-running OMP session, run `/reload-plugins` after installation.

## Skills

- [`decision-retro`](plugins/skills-set/skills/decision-retro/SKILL.md) — Review OMP decision patterns against prior feedback and save private daily or periodic retrospectives.
- [`demo-on-pr`](plugins/skills-set/skills/demo-on-pr/SKILL.md) — Screenshot a change actually running and embed the images in a GitHub pull request description.
- [`drawing-ascii-diagrams`](plugins/skills-set/skills/drawing-ascii-diagrams/SKILL.md) — Draw ASCII flows, trees, tables, and boxes whose edges and connectors line up.
- [`kent-slidev`](plugins/skills-set/skills/kent-slidev/SKILL.md) — Create and refine Slidev presentations in Kent's preferred working style.
- [`skillset-smith`](plugins/skills-set/skills/skillset-smith/SKILL.md) — Create or update personal skills from the current session and open a pull request.

## Privacy

This repository contains reusable instructions, not private session records.
Keep transcripts, retrospective reports, credentials, and personal path settings
outside the repository. `decision-retro` uses a supplied private output directory
or defaults to `~/retros/<project-name>-retro/`.

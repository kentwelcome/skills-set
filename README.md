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

## Behavior Diff CI

The [Behavior Diff workflow](.github/workflows/behavior-diff.yml) compares changed
`SKILL.md` files when a same-repository pull request is opened. Workflow-only PRs
also exercise preparation, which skips model execution when no instruction changed.
Fork PRs and later pushes require a maintainer to approve the current head:

```text
/behavior-diff <full-head-sha> ["path/to/SKILL.md"] [fast]
```

Brackets indicate optional arguments; omit them from the actual comment. Alternatively,
use **Actions → Behavior Diff → Run workflow** with the PR number and reviewed SHA.
Comment and manual triggers require this workflow to be on the default branch.

Each run resolves the **latest stable published release** of
[`spacedock-dev/behavior-diff`](https://github.com/spacedock-dev/behavior-diff),
then freezes its full commit SHA through the action's `core-ref` input. No bundled
core fallback is used. Prereleases and unreleased `main` commits are excluded.
The wrapper action follows the moving `v0.1` tag, including beta patch releases.
Tracking wrapper and core releases explicitly trusts new upstream executable code;
inspect the resolved core version in the run summary before approving execution.

Execution uses the protected `behavior-diff` environment. Store a
`CLAUDE_CODE_OAUTH_TOKEN` environment secret there, generated with `claude setup-token`,
and configure a required reviewer. Keep the credential out of repository-level
secrets so workflows cannot bypass the environment gate. Confirm your subscription
permits this automation. GitHub cannot reveal an existing secret for migration;
set the environment secret yourself before removing its repository-level copy.

Preparation and execution have read-only repository permissions. A separate
publication job, without the model credential, posts one summary per file with
HTML/evidence artifact links. Reports can contain source, prompts, answers and
tool output; on this public repository, treat them as public disclosures.

Comparisons use `plugins/skills-set` as the plugin root, hold other files at the
merge-base, and test each changed instruction separately. New instructions use an
empty file as Before; deletions are not compared. The default is three trials per
side, at most three files, one file at a time, a 45-minute execution deadline and
seven-day artifact retention. These are not spending caps; configure provider limits.
`complete` means evidence collection finished, not that the change is correct.

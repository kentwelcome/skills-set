# Personal Skills

This repository is the source of truth for Kent's personal agent skills.

## Layout

Each skill lives in `skills/<skill-name>/` and contains a `SKILL.md` file.
Optional agent metadata belongs under the skill directory, for example
`skills/<skill-name>/agents/openai.yaml`.

## Install a skill locally

Expose a skill to Codex with a symlink:

```sh
ln -s "$(pwd)/skills/<skill-name>" ~/.codex/skills/<skill-name>
```

Verify the link:

```sh
readlink ~/.codex/skills/<skill-name>
```

## Skills

- [`kent-slidev`](skills/kent-slidev/SKILL.md) — Create and refine Slidev presentations in Kent's preferred working style.

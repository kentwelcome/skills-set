---
name: skillset-smith
description: "Create or update Kent's personal Codex skills in the skills-set repository from evidence in the current session, validate the result, and open a pull request. Use when the user invokes $skillset-smith, asks to turn the current conversation or recent work into a reusable skill, wants to add a personal skill, or wants to improve an existing skill based on what happened in this session."
---

# Skillset Smith

Turn reusable lessons from the current session into a focused skill under
`/Users/kent/GitHub/skills-set/plugins/skills-set/skills/`. Treat the session
as evidence, not as permission to invent missing requirements.

## Start with the mode gate

Make the first user-facing response begin with this exact question:

> Do you want to create a new skill or update an existing skill?

Do not omit the question even when the invocation appears to favor one path.
If the user has not answered it, wait before inspecting or changing repository
files. If the invocation already says the user is unsure, treat that as the
answer, include the question first, and continue with the guidance below in the
same response.

If the user is unsure:

1. Review the current session for the reusable capability, corrections, and
   desired future behavior.
2. Inspect the current repository skill names and frontmatter descriptions. If
   update is a possible recommendation, show the complete live skill list in
   the response before naming a target.
3. Recommend one path with a short reason:
   - recommend **update** when an existing skill already owns the same trigger
     and workflow;
   - recommend **create** when the session reveals a distinct reusable
     capability with its own trigger;
   - say that more evidence is needed when the session contains only a one-off
     task or an unclear preference.
4. Ask the user to confirm the recommendation before proceeding.

Ask direct, narrow questions whenever the skill name, intended triggers,
expected behavior, or an important exception is unclear. Do not guess.

## Ground the change in the session

Extract only durable, reusable evidence:

- explicit user requirements and corrections;
- the task conditions that should trigger the skill;
- the workflow that succeeded;
- observed failure modes and how to avoid them;
- important scope limits and exceptions;
- concrete examples of what the user would ask.

Prefer the user's latest explicit decision when the session contains a
reversal. Separate observed facts from inference, and ask the user to confirm
any inference that would materially change the skill. Do not save secrets,
transient identifiers, or incidental project details unless they are required
for the reusable workflow.

## Prepare the repository

1. Work in `/Users/kent/GitHub/skills-set` and read applicable `AGENTS.md`
   instructions.
2. Inspect `git status`, the current branch, the remote, and existing pull
   requests before editing.
3. Refresh the current default branch. Create a task branch; never commit the
   skill change directly to the default branch.
4. Preserve unrelated staged, unstaged, and untracked work. If it cannot be
   separated safely, stop and ask the user.
5. Read the applicable `skill-creator` skill completely and follow its
   creation, metadata, and validation rules.

Use `skill/<new-name>` for a new skill and `skill/update-<name>` for an update,
adding a short disambiguator only when the branch already exists.

## Create a new skill

1. Ask for the skill name if the user has not supplied one. Suggest a short,
   valid hyphen-case name when helpful, but get confirmation before creating
   files.
2. Confirm two or three representative trigger prompts from the session. Ask
   for an example if the intended use is still ambiguous.
3. Run the `skill-creator` initializer. Create the skill at
   `plugins/skills-set/skills/<skill-name>/`; do not hand-build the scaffold.
4. Write a concise `SKILL.md` using imperative instructions. Put all trigger
   information in the frontmatter description.
5. Generate `agents/openai.yaml` from the completed skill. Include only the
   standard interface fields unless the user provides branding or dependency
   requirements.
6. Add only resources that are necessary for repeatable execution. Do not add
   a README or process notes inside the skill directory.
7. Add the new skill to the repository's top-level `README.md` skill list.

## Update an existing skill

Whenever the user selects **update** or the agent recommends **update**, scan
the live repository and show every directory under
`plugins/skills-set/skills/` that contains `SKILL.md` before asking for or
naming a target. Display each skill's name and a one-line summary from its
frontmatter description. Do not rely on a remembered list.

Then:

1. Ask the user to select the target, or clearly mark the recommended target
   when they want guidance.
2. Read the target `SKILL.md`, `agents/openai.yaml`, and only the directly
   relevant bundled resources completely.
3. Preserve established behavior that the current session does not supersede.
4. Apply the smallest coherent update supported by session evidence.
5. Regenerate `agents/openai.yaml` when its display name, description, or
   default prompt no longer matches the skill.
6. Update the top-level README summary only when the skill's purpose changed.

## Validate the result

Before publishing:

1. Run the `skill-creator` validator on every created or modified skill.
2. Search the changed skill for leftover template TODOs or placeholders.
3. Confirm that the name and folder match and that the frontmatter description
   explains both capability and triggers.
4. Inspect the complete task diff and staged scope.
5. Run `git diff --check`.
6. Bump the patch version in both
   `plugins/skills-set/.codex-plugin/plugin.json` and
   `plugins/skills-set/.claude-plugin/plugin.json`, keeping the versions equal.
7. Validate the Codex plugin and the Claude plugin and marketplace manifests.
8. If validation fails, fix the cause and rerun it. Never claim a check passed
   without observing the successful result.

## Publish the pull request

After the generated skill content passes validation, complete the routine PR
workflow without asking for separate approval:

1. Stage only task-related skill files, the synchronized plugin manifests, and
   the top-level README when changed.
2. Commit with a concise message, then push the task branch to this repository.
3. Check whether the branch already has a pull request. Update that PR instead
   of creating a duplicate.
4. Otherwise create one ready-for-review pull request against the current
   default branch. Include:
   - a brief summary of the reusable behavior captured;
   - whether the work creates or updates a skill;
   - the exact validation commands and results.
5. Verify the PR URL and report it to the user.

Use the available GitHub connector or authenticated CLI. If authentication or
permissions block PR creation, report the exact blocker and ask the user to fix
that access; do not fabricate a PR or repeatedly retry uncertain writes.

Do not merge the PR, change user-scope installation symlinks, or modify other
skills unless the user explicitly asks.

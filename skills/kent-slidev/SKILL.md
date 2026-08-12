---
name: kent-slidev
description: "Create and refine Slidev presentations in Kent's preferred working style: preserve the current source of truth, use concise spoken copy, prefer simple image- and typography-led layouts over card grids, collaborate page by page, verify through the live Slidev view, maintain presenter notes, and commit only at explicit checkpoints. Use when Kent asks to create, rewrite, review, synchronize, or iterate a Slidev deck, slides.md, agenda.md, presenter notes, or individual presentation pages."
---

# Kent Slidev

Use this skill as the editorial and collaboration layer for Slidev work. Read the applicable `slidev` skill for syntax and mechanics; when several exist, prefer the repository-local skill over a user-level copy.

## Establish the authority

1. Inspect `git status`, the deck files, agenda, notes, styles, and available images before editing.
2. Identify the current source of truth from the user's latest instruction and manual edits.
   - Preserve a manually edited agenda when the user says to build from it.
   - Preserve the live deck when the user says to synchronize the agenda back from the slides.
   - Never overwrite recent manual work from an older artifact or remembered structure.
3. Compare `agenda.md` and `slides.md` before changing timings, order, section numbers, or page references.
4. Treat the deck as unfinished while the user is still revising it. Do not call it final or continue polishing a direction the user has paused.

## Work page by page

1. Start from a stable agenda or narrative spine when creating a deck.
2. Keep one main idea per slide. Split a crowded slide into two slides, using the same title when that preserves continuity.
3. During iterative review, change only the named page and the directly coupled notes or styles.
4. Keep page labels, speaker-note timing, agenda order, and total slide count synchronized.
5. After a batch is accepted, wait for the user's next page or explicit commit checkpoint.

## Write in Kent's presentation voice

- Default to Chinese for the story, transitions, titles, and conclusions. Keep established technical terms in English.
- Write what happened in concrete, spoken language. Avoid abstract framework language, generic slogans, and template-like “AI味” copy.
- Prefer a short sentence that Kent can say naturally over a polished marketing sentence.
- Keep visible text sparse. Move explanation, caveats, and narrative detail into presenter notes.
- If the image already says the point, remove redundant on-slide text and leave the explanation in the notes.
- Preserve wording supplied by the user. Smooth it only when asked, and do not silently replace its meaning.
- Verify factual claims against the named source or primary evidence. Keep unverified claims conditional or mark them as TODOs.

## Compose the slide

- Do not default to card grids. Prefer whitespace, large type, simple flows, timelines, or one strong image.
- Use cards only when separate bounded items materially improve comparison. Do not repeat the same card composition across many slides.
- Avoid unnecessary bold labels, decorative arrows, badges, and section jargon.
- Keep the title anchored and vertically center the body in the remaining content area when the project design supports it.
- Inspect image dimensions before placing an asset. Preserve aspect ratio and make screenshots large enough to read.
- When the user requests full width or full height, fill that dimension without stretching or clipping meaningful content.
- Recheck image size after wrapping it in a link; anchors can change layout.
- Use click overlays only when the reveal carries meaning. Verify both the before-click and after-click states.
- Preserve the deck's existing visual system for local edits. Do not restyle unrelated slides.

## Use live verification

1. If the user says a Slidev server is already running or provides an MCP endpoint, use that live server. Do not build or export a PDF unless explicitly asked.
2. Otherwise use the repository's documented Slidev commands and the applicable `slidev` skill.
3. After every visual edit, inspect the exact affected slide in the rendered deck.
4. Check:
   - the requested content and page number;
   - title/body spacing, overflow, and clipping;
   - image readability and aspect ratio;
   - click states, links, and presenter notes;
   - total slide count and neighboring-slide order.
5. If the slide count changes unexpectedly, inspect Markdown separators and frontmatter before changing content. Blank lines around layout frontmatter can create accidental slides.
6. If the user says the fix is still wrong, stop. Inspect the actual rendered DOM or screenshot and correct the observed geometry. Do not repeat an unverified CSS guess.

## Interpret corrections literally

- A page-numbered correction is local unless the user explicitly says “all pages” or states a deck-wide rule.
- “No” or “No!” means stop and realign with the user's stated structure before continuing.
- When the user asks to remove a named visual sentence or block, remove the whole named target, then render the page again.
- When the user rejects cards, abstract wording, or crowding, fix the composition rather than merely changing colors or shortening labels.
- Current explicit instructions override every default in this skill.

## Maintain notes and timing

- Keep presenter notes conversational and consistent with the visible slide.
- Put the complete spoken reasoning in notes when the slide intentionally stays minimal.
- Preserve the total talk-time contract while splitting or reordering slides; divide the existing time instead of silently extending the talk.
- Keep uncertainty, evidence boundaries, and source links in notes when they would clutter the slide.

## Commit safely

1. Do not commit until the user explicitly asks.
2. Before staging, inspect the current staged, unstaged, and untracked files.
3. Stage only the requested deck files, styles, and assets. Preserve unrelated staged or untracked work.
4. Use a path-limited commit when unrelated files are already staged.
5. Report the commit hash, the included files, and any untouched staged or untracked files.

## Completion check

Before claiming a page or batch is done:

- verify the exact rendered page and any animation states;
- confirm the slide count and order;
- confirm presenter notes and timing still match;
- run `git diff --check` when files changed;
- state whether the changes are committed;
- avoid calling an evolving deck final.

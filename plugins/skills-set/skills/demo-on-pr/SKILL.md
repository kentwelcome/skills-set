---
name: demo-on-pr
description: "Capture screenshots of a change actually running and embed them in a GitHub pull request description. Use whenever a PR would be easier to review with pictures — after implementing a UI or output change, when asked to \"demo this\", \"show what this looks like\", \"add screenshots to the PR\", \"put images on the PR description\", or when a reviewer cannot see the behavior from the diff alone. Covers seeding a fixture so the state is visible at all, driving the app in Chrome, uploading images to GitHub to get user-attachments URLs, and rewriting the PR body without posting a stray comment."
---

# Demo on PR

A diff shows what changed. It does not show what the change looks like, and a
reviewer who has to build the branch and reproduce a state by hand usually
just approves on faith. Screenshots in the PR description close that gap.

The work has two halves that fail for different reasons: **getting the app into
the state worth showing**, and **getting the images onto GitHub**. Treat them
separately.

## Get the state on screen

The interesting state is often the one that is rare in real data — an error
banner, a full queue, a list at its limit. Waiting for it to occur naturally
wastes the session.

1. **Find the app's real render path.** Read how the page or output is
   produced and where its data comes from: a JSON endpoint, a template render,
   a CLI print. You want the seam where data enters, because that is the one
   place worth substituting.
2. **Substitute data, never the render.** Serve the app's own template,
   component, or page loader. Only the payload is synthetic. A screenshot of a
   hand-written HTML mock proves nothing about the branch.
3. **Route the fixture through the real transform.** If the change is an
   ordering, a grouping, a filter, or a computed field, the fixture must pass
   through that exact function. Import it and call it.
4. **Build the fixture adversarially.** Arrange the input so a broken
   implementation would look obviously wrong — for an ordering change, build
   the list in the opposite order and let the code sort it. If the fixture is
   already in the right shape, the screenshot cannot distinguish working code
   from no code at all.
5. **Dump the payload and read it before screenshotting.** Print the fields the
   change affects and confirm they are what the feature promises.

**The failure that motivates step 3 and 5.** Hand-arranging the list and
skipping the real sort produces a demo of the exact opposite of the feature,
and it looks completely convincing. Catch it by reading the payload, not by
looking at the picture.

Base the fixture on a real payload where you can — capture one from the running
app and mutate it — so every field keeps its true shape.

## Drive it and capture

Use the browser automation tools. Take a screenshot per claim, not per screen:
each image should answer one question a reviewer would ask.

- Resize the window to something reasonable (~1440x1000) before the first shot.
- Show the feature at rest first, then each interaction.
- **Transient UI needs batched actions.** Toasts and copy confirmations often
  live under two seconds, so a keypress in one call and a screenshot in the
  next will miss them. Put the action and the capture in a single batch.
- Prefer a tight zoom on a region for small details; a full page shot buries
  them.
- Capture the *before* state for any fix that only makes sense against the
  condition that broke it. A bug fix demo is two images, not one.

Save the images with meaningful names — they become the alt text and the
reviewer's mental index.

## Put the images on the PR

GitHub's attachment storage has no public API, so `gh` alone cannot upload.
Use the browser to get URLs, then `gh` to write the body.

1. Open the PR and find the file input on the **comment box** (not the
   description editor — editing the description in the browser risks clobbering
   the body you are about to rewrite).
2. Upload all images in one call.
3. Wait, then read the textarea value. GitHub inserts placeholder text like
   `![Uploading foo.jpg…]()` while an upload is in flight — if you see it, wait
   and read again. Harvest the `user-attachments` URLs only once every
   placeholder is gone.
4. **Clear the comment box.** The upload leaves markdown behind, and an
   accidental submit posts noise on someone's PR. Click into the textarea and
   select-all then delete; if a script-based clear is blocked by the page,
   keyboard input still works.
5. Fetch the current body with `gh pr view <n> --json body --jq .body`, insert
   the demo section, and apply it with `gh pr edit <n> --body-file <file>`.
   Compose the new body in a script that asserts its anchor appears exactly
   once and that the body has no images yet, so a rerun cannot silently double
   up.
6. Reload the PR and confirm the images render rather than trusting the API
   response.

Assets stay on GitHub once uploaded, so a cleared draft costs nothing.

## Write the demo section

Place it where a reviewer meets it before the implementation notes. Give each
image a one-line caption saying what it demonstrates. Pair a bug-fix image with
the broken-state image directly above it.

**State plainly that the data is a fixture, and what is real.** This is the part
that matters most. Screenshots read as evidence, and a reviewer who assumes
those were live sessions draws a conclusion the demo does not support. Name
which parts came from the branch — the page, the ordering function — and which
were seeded. It costs one sentence and it is the difference between evidence
and a nice picture.

Do not overstate elsewhere in the body either: if a verification line already
claims real-browser testing, leave it alone rather than restating it.

## Clean up

Stop any server you started, close the tabs you opened, and confirm the target
repository's working tree is as you found it. A demo that leaves a daemon on a
port is a bug report waiting to happen.

## Worked example

A dashboard PR reordered a "needs your input" queue by longest-blocked-first.
Live data had no blocked sessions at all, so the queue was invisible.

The demo served the branch's own page loader with a synthetic `/api/data`
holding four gates at 47m / 19m / 6m / 1m, **built newest-first in the payload
and sorted by importing the branch's real ordering function**. Six shots: the
queue at rest, the cursor stepping, the copy confirmation caught in a batched
action, the same queue in the second display mode, then the scrambled ordering
that broke the old keyboard jump followed by the same board after the fix. The
description said outright that the page and the ordering were real and the gate
rows were a fixture.

The first attempt built the list by hand without calling the sort, and rendered
the queue in exactly the wrong order. The payload dump caught it before any
screenshot was taken.

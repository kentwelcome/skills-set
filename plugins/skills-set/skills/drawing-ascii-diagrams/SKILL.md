---
name: drawing-ascii-diagrams
description: "Draw ASCII diagrams (flows, trees, tables, boxes) whose edges and connectors line up. Use whenever a concept, pipeline, architecture, or comparison is explained with a text picture in a reply, Slack message, doc, or code comment, when asked to \"draw this in ASCII\", \"visualize this flow\", or \"show it as a diagram\" without an image tool, or when an instruction file says to use ASCII visualization to explain a concept."
---

# Drawing ASCII diagrams

A diagram that lines up is read in one glance. A diagram with a drifting
edge makes the reader stop and doubt the content. Every diagram is drawn on
a character grid and checked column by column before it is sent.

## 1. Pick the shape

| You are explaining            | Draw                                  |
|-------------------------------|---------------------------------------|
| Steps, pipeline, data flow    | Left-to-right flow: `A --> B --> C`    |
| Two paths that merge or split | Flow with one `+` junction per branch |
| Parent/child, folders, tree   | Indented tree with `+--` and `|`       |
| Options side by side          | Table with `|` columns                 |
| A thing with parts inside     | One box with the parts listed inside  |

A flow or a table covers most cases. Boxes are for containers, not for
decoration around every label.

## 2. Draw on the grid

- Characters: ASCII `+ - | < > v ^` and spaces, or the Unicode box-drawing set
  `─ │ ┌ ┐ └ ┘ ├ ┤ ┬ ┴ ┼` and spaces. Pick one set for the whole diagram —
  never mix them.
- If you use Unicode, match each corner or junction to what actually meets
  there — don't reuse one glyph everywhere:
  - Two sides only (a plain corner): `┌` `┐` `└` `┘`
  - Three sides (a line joins a straight run): `├` `┤` `┬` `┴`
  - Four sides (a true crossing): `┼`

  A box corner is always two-sided (`┌ ┐ └ ┘`). Where an incoming line
  lands in the middle of a box's top or bottom edge, that point is
  three-sided (`┬` or `┴`), not a corner and not `┼`.
  - Skip arrowhead glyphs (`▼ ▲ ◄ ►`) — many fonts and Slack render them
    wider than one column, which shifts every column after them out of
    line. Top-to-bottom or left-to-right order already shows direction; a
    plain `│` (or ASCII `v`) is enough.
- A box has a top edge, a bottom edge, and two side bars. All three
  horizontal parts are the same width, and the label inside is padded with
  spaces so the side bars stay in one column.
- A vertical connector `|` (or `│`) sits in the same column on every row it
  passes.
- Widest line is 60 columns or fewer, so it survives Slack and phones.
- Text that describes the diagram goes above or below it, never beside it.

## 3. Check before sending

Read the diagram twice, once per pass:

1. **Columns.** Follow every `|` or `│` from top to bottom. It never moves
   left or right between rows.
2. **Widths.** Each box's top edge, bottom edge, and every row between them
   have the same length. A label wider than the edge means the box grows,
   not the label.

Fix and re-read until both passes are clean.

## Example

Behavior Diff run, two paths that merge:

```
repo @ HEAD
  |
  +--> before (old file) --> 3 fresh runs --+
  |                                         +--> report
  +--> after  (new file) --> 3 fresh runs --+
```

The same idea as boxes, when the sandboxes need their contents shown:

```
+---------------------+    +---------------------+
| before sandbox      |    | after sandbox       |
| - old CLAUDE.md     |    | - edited CLAUDE.md  |
| - everything else   |    | - everything else   |
+---------------------+    +---------------------+
          |                          |
          v                          v
      3 fresh runs               3 fresh runs
          |                          |
          +----------> report <------+
```

Same diagram in Unicode. Every connector that lands in the middle of a
horizontal line uses `┬` or `┴`, including the lines that enter and leave
each box:

```
                  repo @ HEAD (snapshot)
                       │
          ┌────────────┴─────────────┐
          │                          │
┌─────────┴───────────┐    ┌─────────┴───────────┐
│ before sandbox      │    │ after sandbox       │
│ - old file          │    │ - edited file       │
│ - everything else   │    │ - everything else   │
└─────────┬───────────┘    └─────────┬───────────┘
          │                          │
          3 fresh trials             3 fresh trials
          │                          │
          └────────────┬─────────────┘
                       │
                    compare
                       │
                  one report
```

## Common mistakes

| Mistake                                              | Fix                                                    |
|-------------------------------------------------------|-----------------------------------------------------|
| Side bars with no top or bottom edge                 | Either draw the full box or drop the bars              |
| Top edge narrower than the label below it            | Widen the edge to the longest row                      |
| Vertical connector under a different column         | Pad with spaces until it is under its box              |
| ASCII and Unicode line characters mixed together    | Pick one set and use it throughout                     |
| `┼` or a plain corner used where a line joins mid-edge | Use `┬`/`┴`/`├`/`┤` — match the glyph to what meets there |
| Arrowhead glyphs (`▼ ▲ ◄ ►`) shift later columns      | Drop them; a plain `│`/`v` plus reading order is enough |
| Boxes around plain step names                        | Use a flow line instead                                |

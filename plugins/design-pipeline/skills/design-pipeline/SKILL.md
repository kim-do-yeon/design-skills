---
name: design-pipeline
description: Run the full staged UI design flow (hallmark → ui-ux-pro-max → emil-design-eng → web-design-guidelines → domain check) on a new page, prototype, or redesign, reporting what each stage changed. Use when the user asks to build or redesign a UI end to end, asks for "the full design pass", or wants to see how each design skill improves the result.
argument-hint: <what to build or the file to redesign>
---

# Design pipeline

Build or redesign the UI in `$ARGUMENTS` by running the four design skills in order, then a domain check. Each stage has one job. Load each skill with the Skill tool when its stage starts, not before.

## Stages

1. **Direction and build: `hallmark:hallmark`**
   - Follow hallmark's own flow: design-context gate, macrostructure, theme, build, slop test.
   - If the user already said "just build it", infer audience, use case, and tone, and state them in one line.
   - Use `frontend-design:frontend-design` instead only when the user asks for it.
2. **Evidence check: `ui-ux-pro-max:ui-ux-pro-max`**
   - Run a `--domain product` search for the product type, and confirm or adjust the direction.
   - Don't adopt `--design-system` output that doesn't fit the subject; say that you rejected it and why.
   - Run targeted `--domain ux` searches for the page's actual controls: forms, validation, touch targets, live regions.
   - Measure contrast of text and UI boundaries: 4.5:1 for text, 3:1 for input borders and focus rings.
3. **Motion and interaction: `emil-design-eng:emil-design-eng`**
   - Report changes in its Before/After/Why table format.
   - Leave frequent actions unanimated, and never animate money figures.
   - Honor `prefers-reduced-motion`.
4. **Final review: `web-design-guidelines:web-design-guidelines`**
   - List findings as `file:line`, then fix them.
5. **Domain check: no skill covers this, so do it yourself**
   - Calculations and rounding: totals must add up.
   - Dates: month-end and time zones.
   - Empty, negative, very long, and non-numeric input.
   - Anything the subject's conventions require, such as legal clauses or units.

## Working copies

- If the user wants to compare stages, save each stage as its own file (`v1`, `v2`, …). Otherwise edit in place.
- Before editing an existing project, state which files you will change.

## Report

After the last stage, give a short table with one row per stage:

| Stage | Skill | Changes | Problems found and fixed |
|---|---|---|---|

Also list anything still open. If there are automated checks you can run (tests, Lighthouse, a small script run in the browser), run them and report the numbers. Don't claim an improvement you didn't observe.

# design-pipeline: UI/design skill routing

For any task that creates, changes, or reviews how an interface looks, moves, or is used (pages, prototypes, components, mockups, redesigns, UI reviews), pick skills from this table yourself. The user should not have to name them. Load each chosen skill with the Skill tool before writing UI code. If the user names a skill, use that one instead. Skip all of this for non-visual work.

| Situation | Skill |
|---|---|
| New page, app, landing page, prototype, or full visual redesign | `hallmark:hallmark` (default). Use `frontend-design:frontend-design` only if the user asks for it. Never load both. |
| Redesign existing UI, keeping routes, copy, and information architecture | `hallmark:hallmark` with the `redesign` verb |
| Extract design DNA from a URL or screenshot | `hallmark:hallmark` with the `study` verb |
| Concrete choices: palette, font pairing, style, stack-specific UI, UX rules (accessibility, touch, forms, navigation), contrast | `ui-ux-pro-max:ui-ux-pro-max` |
| Design tokens or component specs | `ui-ux-pro-max:design-system` |
| shadcn/ui + Tailwind implementation | `ui-ux-pro-max:ui-styling` |
| Logo, banner, brand identity, social images | `ui-ux-pro-max:design`, `ui-ux-pro-max:banner-design`, `ui-ux-pro-max:brand` |
| Animation, transitions, gestures, press feedback, toasts, component feel | `emil-design-eng:emil-design-eng` |
| Review or audit existing UI code | `web-design-guidelines:web-design-guidelines` plus the `hallmark` `audit` verb. Report only; edit only if asked. |

## Full flow for a new UI

Run the stages in this order:

1. `hallmark`: direction and build
2. `ui-ux-pro-max`: check color, type, and UX choices against its data
3. `emil-design-eng`: motion and interaction polish
4. `web-design-guidelines`: final review, then fix what it finds
5. Domain check: the skills do not check business logic. Verify money and rounding, dates (month-end, time zones), and negative or empty input yourself.

## Lessons from testing

- `ui-ux-pro-max --design-system` sometimes recommends a style that doesn't fit, such as glassmorphism or a handwritten font for a legal tool. Check it against the product first, and use a narrower `--domain product` search to confirm the direction. Its UX rules and contrast checks are its strongest part.
- `emil-design-eng`: don't animate keystroke-level updates or money figures (no count-up). Color-only change highlights are fine.
- `web-design-guidelines` fetches its rules over the network at run time. Output findings as `file:line` before fixing.

Keep it proportionate. A small tweak, like one color or one spacing fix, needs at most one skill, or none. Tell the user in one short line, in their language, which skills you loaded and why. For the full staged flow with per-stage reporting, the user can run `/design-pipeline:design-pipeline`.

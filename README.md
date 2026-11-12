# Gün Gün

Gün Gün is a planning-stage design for a single-user Turkish daily planner and personal finance app. The goal is to reduce daily decision load, make plans easier to recover when days change, and keep money and upcoming payments visible.

## Current status

**Planning only.** No application, dependency setup, completed phase, test/build result, or deployment exists. The second visual concept is closer to the intended direction, but the user has not approved it as final. Continue refining the design and realistic week flows; do not begin Phase 0 or app implementation automatically.

## Start here

1. Read [`AGENTS.md`](AGENTS.md) for working, privacy, and decision rules.
2. Read [`docs/PROJECT-CONTEXT.md`](docs/PROJECT-CONTEXT.md) for compact accepted context and the document map.
3. Read [`docs/CONTINUATION.md`](docs/CONTINUATION.md) for current handoff, next task, and open questions.
4. Read only the relevant source sections in `docs/01-PRD.md`, `docs/02-SCREENS.md`, and `docs/03-DESIGN.md`. `docs/10-DECISIONS.md` records accepted changes; `docs/11-UX-WALKTHROUGH.md` and `docs/12-VISUAL-RESEARCH.md` are design proposals and research notes.

## Next step

Continue the second concept in [`docs/assets/ikinci-ekran-taslaklari.png`](docs/assets/ikinci-ekran-taslaklari.png) and work through realistic normal and disrupted week flows. Preserve the distinction between accepted product decisions and visual proposals. The next design is still for user review; implementation starts only after the user asks to code and the applicable phase is agreed.

## For Codex CLI

Use the root agent to coordinate and delegate only bounded, independent work when it helps. Prefer `gpt-6-luna` for routine, limited tasks and `gpt-6.1-sol` for complex design, architecture, or review; if a model or subagent is unavailable, continue with the available option and say so. Minimize repeated reads and duplicate delegation. Ask at most three short, grouped questions in Turkish, without expecting coding or design expertise.

This is a docs-only repository at present. There is no `package.json`, so do not run npm/pnpm install, dev, check, test, or build commands. For document-only changes, review content, links, and the diff; do not report app tests or builds as passing.

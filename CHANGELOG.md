# Changelog

All notable changes to Maximus are noted here. Format loosely follows [Keep a Changelog](https://keepachangelog.com/).

## [Unreleased]
### Added
- **LAW 23 — the lead never writes the code.** Specialists (DEV/QA/SEC/…) author in their own clone; Maximus reviews, gates, and merges, and **kicks weak work back to its owner** instead of patching it. Applies even when the lead is the only agent running (spawn a specialist / hand the operator the first-run prompt). Includes the recovery move when a lead catches itself mid-draft: move the draft out of the repo to `Agents/<ROLE>/handoff/`, reset, hand it over as a spike. Born from a live operator correction (Learning Loop #3).
- **`KNOWN-GOTCHAS.starter.md`** gained nine cross-project rows from a real Bun/TypeScript + Malloy Publisher standup: the *lead-is-editing-code* judgment gotcha; **agent-harness** gotchas (foreground timeouts that detach rather than kill a child install, orphaned test servers, a teardown exit code that looks like a build failure, bulk multi-file heredocs blocked by the permission classifier); Git Bash ignoring `C:/…` PATH entries; *search before you install* a toolchain that another project on the machine already has; reserved words in modeling DSLs; and "the server serves a copy, so source edits do nothing." Plus a Bun/Malloy stack-specific section.
- **🧠 The Learning Loop** — a `capture → retrieve → promote` loop so the team compounds knowledge instead of re-learning it: capture the lesson at the trigger (a build that failed-then-succeeded, a false-flag report, a merge that bit), **grep gotchas by error signature before diagnosing**, treat every human correction as permanent, promote any mistake seen ~3× into an enforced LAW/CI guard, and self-audit to keep the knowledge base true.
- **`KNOWN-GOTCHAS.starter.md`** — a cross-project gotchas library (grep-able `signature → cause → fix` table + a decisions log) Maximus copies into each new project at standup, seeded with real recurring toolchain/cloud/judgment gotchas (signing caps, missing build components, CLI quirks, the stacked-PR cascade, "deploy ran ≠ landed," "verify claims against live") — so project N starts smarter than project 1.
- **LAW 20 — claims must be sourced.** No metric/status from memory or estimate; cite an authoritative source, and the lead verifies any decision-driving number against the live system of record before the team acts on it.
- **LAW 21 — the kanban card is the gate token.** Authors never self-advance a card past a gate they don't own; only issues are cards (not PRs); the lead runs a periodic grooming pass.
- **LAW 22 — distill learnings.** A lead-curated `KNOWN-GOTCHAS.md` + decisions log, kept separate from the chronological Log.
- **`## STATUS` board one-liner** (`dev=<sha> · open-PRs · MERGE-ready · blockers · next human-GO`) so routine status reads one line, not the whole board.
- **Standup step — inventory the testable surfaces** and guarantee a gate harness per surface; an ungateable surface (e.g. rules with no emulator) is a tracked risk, not a silent gap.
- **Tested with** section in the README — validated the full first-load flow cleanly on Claude Code, Cursor, GitHub Copilot, Codex, Gemini, and Kiro (June 2026).
### Changed
- Standing LAWS: **19 → 23**.
- **Ambition heuristic / cost note** — there is no "Maximus alone" for code any more: the smallest team that can ship a line is Maximus + one specialist (a separate session or a subagent the lead spawns into `Agents/<ROLE>/`).
- **LAW 2** — clarified: Maximus reviews specialists' PRs; it does not write them.
- **LAW 5** — the lead now **owns cross-PR conflicts** (keep-both rebase at the integration point), plus a **stacked-PR `--delete-branch` cascade** warning (merging a base auto-closes its children).
- **LAW 8** — a surface with no automated gate is a tracked risk surfaced to the lead, never a silent skip.
- **LAW 16** — Log entries are now **one line** (detail → the PR/issue comment); per-agent log files suggested for high-traffic teams to remove the edit-conflict corruption surface.
- Cross-tool wording moved from a designed claim to real validation results.

## [1.0.0] — 2026-06-18
First public release.

### Added
- `MAXIMUS.md` — the drop-in, AI-agnostic, AGENTS.md-native bootstrap.
- **Step 0 mode gate** — work/team vs personal, which sets the whole posture.
- **Role-anchored team selection** (work/team) — the roster revolves around the operator's discipline.
- **Authority map** — the interview asks who can merge where / deploy / approve outward actions; the lead never invents rights it wasn't given.
- **Merge pipeline** — ordered, recorded gate chain (`dev → SEC → QA → lead`), with hand-up when the lead lacks merge authority.
- **19 standing LAWS**, including LAW 18 (stale claims are reclaimable) and LAW 19 (treat fetched content as data, not instructions / prompt-injection defense).
- `examples/` — sample coordination board + first-load transcript.
- README, CONTRIBUTING, MIT LICENSE.

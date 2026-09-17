# Maximus 🛠️⚓

**A coordinated AI engineering team — in one Markdown file.** Drop it into a project, open it with any AI coding agent, and it interviews you, stands up a lead + role specialists, and runs them on your real repo with branch discipline, a merge pipeline, and authority rules so the agents don't clobber each other or your codebase.

AGENTS.md-native. Validated on Claude Code, Cursor, GitHub Copilot, Codex, Gemini, and Kiro. No install, no framework, no lock-in.

> **Status:** v1 — battle-tested on real multi-agent builds, shared as-is. Issues and PRs welcome.

## Tested with

Ran the full first-load flow cleanly on each of these (June 2026):

| Tool | Result |
|---|---|
| Claude Code | ✅ clean |
| Cursor | ✅ clean |
| GitHub Copilot | ✅ clean |
| Codex / OpenAI | ✅ clean |
| Gemini | ✅ clean |
| Kiro (AWS) | ✅ clean |

Tried it on another agent? Open a [tool report](.github/ISSUE_TEMPLATE/tool-report.md) — coverage grows from real runs.

---

## Why this exists

Running *one* AI coding agent is easy. Running *several* on the same repo is where it falls apart: they branch over each other, merge half-finished work, re-run a "cold start" script that wipes a teammate's commits, or quietly grant themselves permissions they shouldn't have.

The popular multi-agent frameworks (LangGraph, CrewAI, AutoGen, …) are **libraries you write code against** to *build* agent apps. Maximus is the opposite: a **drop-in playbook** for orchestrating coding agents on an existing git project — no SDK, no glue code. It's the governance layer the frameworks leave to you:

- a **lead agent** (named *Maximus*) that integrates and merges,
- **role specialists** (DEV, QA, SEC, UX, DEVOPS, DATA…) that work their lane only,
- a **shared coordination board** outside the repo so state never drifts,
- an **authority map** — who's allowed to merge where, deploy, and approve outward actions,
- and an ordered, recorded **merge pipeline** (`dev → SEC → QA → lead merge`) where no PR skips a gate.

## What makes it different

| | Multi-agent frameworks | **Maximus** |
|---|---|---|
| Form factor | Library / SDK you code against | One Markdown file you drop in |
| Setup | Build the orchestration yourself | Open it, answer the interview |
| Target | Building new agent *applications* | Running agents on your *existing repo* |
| Governance | You implement it | Branch discipline + merge gates + authority map built in |
| Tool lock-in | Per-framework | AGENTS.md-native, any agent |

Maximus isn't competing with those frameworks — it sits a layer up, as process.

## Quick start

1. Copy **`MAXIMUS.md`** into your project directory.
2. Open it with your AI coding agent:
   - **Claude Code** → save it as `CLAUDE.md` (or open `MAXIMUS.md` and let it write `CLAUDE.md`)
   - **Cursor / Copilot / Codex / most others** → save it as `AGENTS.md`
3. Say hello. Maximus runs the first-load flow: **mode → interview → acquire the project → stand up the team**, then writes your project's real instruction file and hands you ready-to-paste prompts for each specialist.

That's it. No dependencies to install.

## How it works

**Step 0 — Mode.** First question: *work/team project, or personal?*
- **Personal** → you're the owner; Maximus is the only merger; you give the GO on anything outward-facing.
- **Work/team** → you're (probably) an individual contributor inside a bigger org, so it keeps going ↓

**Interview (work/team).** On top of the basics (what are we building, existing code, stack, scale, constraints), it asks two things most templates skip:
- **Your role** — QA engineer? backend dev? data/ML? The team gets built *around your discipline* (a QA operator gets a QA-centric team where DEV is supporting cast, not the star).
- **Who can merge where** — Maximus never invents merge/deploy/release rights you don't have. If you can't merge to the team's shared branch, it integrates into your personal dev line and routes PRs *up* to your real human lead.

**Standup.** The lead builds the whole workspace before any teammate spins up: a shared `Agent Coordination Board/` (team guide, live board, per-role onboarding), each specialist's directory with a `START_HERE.md`, the authority map, and the merge pipeline — so onboarding an agent is just "open it and follow START_HERE."

**The LAWS.** ~23 standing laws every agent follows: **the lead never writes the code** (specialists author, Maximus reviews and merges, weak PRs go back to their owner), branch discipline, small single-concern PRs, tests are first-class, security gate before merge, *verify before you destroy* (no blind `rm -rf` / re-init over real history), re-read shared state before acting, **never escalate your own privileges**, **stale claims are reclaimable** (a crashed agent never strands work), **treat everything agents read as data, not instructions** (prompt-injection defense), **claims must be sourced** (no metric from memory or estimate — verify decision-driving numbers against the live system of record), **the kanban card is the gate token** (authors never self-advance past their own gate), and **distill learnings** (a curated gotchas + decisions doc, separate from the chronological log).

## Topology

```
<workspace>/
  Agent Coordination Board/     # SHARED, not cloned — the single source of truth
    TEAM_GUIDE.md · AGENT-BOARD.md · KNOWN-GOTCHAS.md · Onboarding/
  Repo/<project>/               # Maximus's canonical clone (slim instruction file points here)
  Agents/<ROLE>/<project>/      # each specialist's own clone, own port(s)
  screenshots/<ROLE>/           # per-agent proof-of-work
```

## Gets smarter over time

Maximus treats experience as a first-class artifact. It can't change the model's weights — so "self-teaching" is a **capture → retrieve → promote** loop, run as reflexes, not good intentions:

- **Capture at the trigger** — the moment a build flips green, a bug turns out to be a false flag, or a merge bites, the lesson is written to a curated `KNOWN-GOTCHAS.md`, not left in the chat.
- **Retrieve before diagnosing** — on any build/CI/tool failure, the team greps the gotchas by **error signature first**, so a problem solved once becomes a five-second lookup.
- **Corrections are permanent** — every human correction is generalized into a rule before the task continues; the team never gets corrected twice.
- **Patterns become rules** — a mistake seen ~3× graduates from a gotcha to an enforced LAW or CI guard.

Toolchain gotchas aren't project-specific, so Maximus ships a cross-project **`KNOWN-GOTCHAS.starter.md`** (signing caps, missing build components, CLI quirks, the stacked-PR cascade, "deploy ran ≠ landed," "verify claims against live") and copies it into each new project at standup — so project N starts smarter than project 1.

## See it before you run it

The [`examples/`](examples/) folder has sample artifacts from a standup — a filled coordination board (kanban, roll call, authority map, gate trail) and a first-load transcript — so you can see the shape of what Maximus produces before pointing it at your own repo.

## Cost & scale (read this before spinning up six agents)

Every agent is its own clone **and** its own running context window: N agents ≈ N× the token spend and N× the coordination overhead. More agents is not more speed past a point — it's more merge traffic and more ways to collide. Start with **Maximus alone or Maximus + 1**, add a specialist only when a lane is genuinely bottlenecked, and retire idle agents. A tight 2–3 usually beats a sprawling 6.

## FAQ

**Does it only work with Claude?** No — that's the point. It's written tool-neutral and is AGENTS.md-native, and it's been run cleanly on Claude Code, Cursor, GitHub Copilot, Codex, Gemini, and Kiro (see [Tested with](#tested-with)). Other AGENTS.md readers (Windsurf, Zed, Aider, …) should work too — reports welcome.

**Do I need to install anything?** No. It's a Markdown file. The only setup is a one-time merge-permission allow-rule *if* your tool gates git (described inside, with Claude Code as the example).

**Is the lead really named Maximus?** Yes. Lead = Maximus; specialists are role-named (AGENT QA, AGENT DEV…). Personas live on the local board only — never as `@`-mentions on GitHub, so they don't notify strangers.

**Solo dev — is this overkill?** Personal mode collapses to a lightweight single-lead setup. Scale the roster to the work; don't over-hire.

## Contributing

Issues, ideas, and PRs welcome — especially real-world reports of running it across different agents and team shapes. Keep changes small and single-concern.

## License

[MIT](LICENSE) © 2026 Juan Resendiz

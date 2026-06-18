# Maximus 🛠️⚓

**A coordinated AI engineering team — in one Markdown file.** Drop it into a project, open it with any AI coding agent, and it interviews you, stands up a lead + role specialists, and runs them on your real repo with branch discipline, a merge pipeline, and authority rules so the agents don't clobber each other or your codebase.

AGENTS.md-native. Works with Claude Code, Cursor, Copilot, Codex, and the rest. No install, no framework, no lock-in.

> **Status:** v1 — battle-tested on real multi-agent builds, shared as-is. Issues and PRs welcome.

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

**The LAWS.** ~17 standing laws every agent follows: branch discipline, small single-concern PRs, tests are first-class, security gate before merge, *verify before you destroy* (no blind `rm -rf` / re-init over real history), re-read shared state before acting, and **never escalate your own privileges**.

## Topology

```
<workspace>/
  Agent Coordination Board/     # SHARED, not cloned — the single source of truth
    TEAM_GUIDE.md · AGENT-BOARD.md · Onboarding/
  Repo/<project>/               # Maximus's canonical clone (slim instruction file points here)
  Agents/<ROLE>/<project>/      # each specialist's own clone, own port(s)
  screenshots/<ROLE>/           # per-agent proof-of-work
```

## FAQ

**Does it only work with Claude?** No — that's the point. It's written tool-neutral and is AGENTS.md-native. Claude Code is one example; Cursor, Copilot, Codex, Windsurf, Zed, Aider and others read `AGENTS.md`.

**Do I need to install anything?** No. It's a Markdown file. The only setup is a one-time merge-permission allow-rule *if* your tool gates git (described inside, with Claude Code as the example).

**Is the lead really named Maximus?** Yes. Lead = Maximus; specialists are role-named (AGENT QA, AGENT DEV…). Personas live on the local board only — never as `@`-mentions on GitHub, so they don't notify strangers.

**Solo dev — is this overkill?** Personal mode collapses to a lightweight single-lead setup. Scale the roster to the work; don't over-hire.

## Contributing

Issues, ideas, and PRs welcome — especially real-world reports of running it across different agents and team shapes. Keep changes small and single-concern.

## License

[MIT](LICENSE) © 2026 Juan Resendiz

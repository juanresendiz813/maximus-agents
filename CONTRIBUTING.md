# Contributing to Maximus

Thanks for kicking the tires. Maximus is a single-file orchestration playbook, so contributions are mostly about making the instructions clearer, safer, and more tool-portable — not adding machinery.

## Good contributions
- **Field reports.** Ran Maximus with a specific agent (Cursor, Copilot, Codex, Windsurf, Zed, Aider, etc.)? Open an issue with what worked, what broke, and which model you drove it with. This is the most valuable thing you can add — it's how we learn the real cross-tool story.
- **Clarity fixes.** A step that an agent misread, an ambiguous law, a confusing instruction.
- **New/refined LAWS.** A failure mode you hit that a standing law would have prevented. Bring the story, not just the rule.
- **Examples.** Additional sample boards/transcripts for different team shapes (solo, QA-led, data/ML, etc.).

## Please don't
- Add config files, schemas, a CLI, or dependencies. The whole point is "it's just a file." Machinery undercuts the differentiator.
- Make `MAXIMUS.md` dramatically longer without cutting elsewhere — it already assumes a healthy context window.

## How
1. Open an issue first for anything non-trivial, so we can agree on direction.
2. Keep PRs small and single-concern.
3. Edit `MAXIMUS.md` (the template) — if your change affects behavior, update the README and `CHANGELOG.md` too.
4. Personas/callsigns stay illustrative; don't add real `@handles`.

By contributing you agree your work is licensed under the repo's [MIT License](LICENSE).

# codex-claude-tandem

A skill for running **Codex** (GPT-6.1 Sol / GPT-6 Astra) and **Claude Code** (Opus 5.5 / Fable 5.1) on the same job, with one model leading each part by fit rather than by which harness you happened to open.

**The rule:** Claude builds and lands; Codex digs. Codex (GPT-6.1 Sol) investigates, audits, reviews, and cleans up. Claude (Opus 5.5) implements, owns anything a person will see, and runs long unattended work. When no row fits: work where a bad run is cheap to discard goes to Codex, work where a bad run costs something goes to Claude. The full who-leads-what table is in [routing.md](routing.md); the evidence is in [docs/source-notes.md](docs/source-notes.md) (Fable 5.1 vs Astra, 2026-09-11) and [docs/source-notes-2026-09-29-gpt61-sol.md](docs/source-notes-2026-09-29-gpt61-sol.md) (GPT-6.1 Sol vs Opus 5.5, 2026-09-29).

**How it works:** whichever model starts writes the split to a `TANDEM.md` at the project root (parts, one packet per part, swap log), hands off the parts it doesn't lead with a fixed prompt that lets the other model AGREE or CONTEST before acting, and swaps leads only on named triggers (Codex loops 3x, Claude's fix doesn't hold, a lead drops UI it was told to keep).

## Install

Both Codex and Claude Code read skills from `~/.agents/skills/`. Claude Code also reads `~/.claude/skills/`, Codex also reads `~/.codex/skills/`.

```bash
git clone https://github.com/mkhaytman87/codex-claude-tandem ~/.agents/skills/codex-claude-tandem
ln -s ../../.agents/skills/codex-claude-tandem ~/.claude/skills/codex-claude-tandem
ln -s ../../.agents/skills/codex-claude-tandem ~/.codex/skills/codex-claude-tandem
```

Then say "tandem" at the start of a job in either harness.

## Testing

Built with the RED/GREEN/REFACTOR method from [superpowers:writing-skills](https://github.com/obra/superpowers). Three pressure scenarios, baseline vs. with-skill, on Claude Opus subagents and on the real Codex CLI. Results and what is still untested: [tests/RESULTS.md](tests/RESULTS.md).

Baseline finding worth knowing: a frontier model already picks the right lead in a forced choice. What it does not do without the skill is leave a coordination artifact the other model can read back and contest. That is the gap this skill fills.

## Caveats

- The routing table is one heavy user's September 2026 experience (Theo, t3.gg, two videos), summarized. The 9/29 video reversed the 9/11 "rewrites and ports go to Codex" row. Several "Astra" traits are Codex-harness traits. Overwrite rows when your own sessions contradict them, and note which session did.
- Codex has been observed to stop applying a loaded skill on later turns. Re-invoke it on any turn where the lead might change.
- Not yet tested: real Astra (Codex runs used Sol), multi-turn retention, a genuine two-model dispute through the tie-break table. The 2026-09-11 pressure tests ran against the v0.1 routing table and GPT-5.6 Sol; the 9/30 routing update has not been re-run through them.

## License

MIT

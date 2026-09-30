---
name: codex-claude-tandem
description: Use when a job could be worked by both Codex (GPT-6.1 Sol / GPT-6 Astra) and Claude Code (Opus 5.5 / Fable 5.1), when the user says "tandem", "who should lead", "hand this to codex/claude", or when the current model is about to do work the other model is known to do better (audits, deep review, root-cause investigation, computer use, cheap scoped tasks vs. landing mergeable code, UI, long unattended builds, instruction-faithful edits). Applies whichever harness the session started in.
---

# Codex + Claude tandem

**Core principle:** the job is split into parts, every part has exactly one LEAD, and the lead is chosen by fit, not by which harness the user happened to open. The other model is SECOND: it reviews, refutes, or cleans up, and never edits the same files at the same time.

Fit rule of thumb (Theo, t3.gg, Sep 2026; see `routing.md` beside this file for the full table and which model to pick on each side): **Claude builds and lands; Codex digs.** Codex (GPT-6.1 Sol) investigates, audits, reviews, and cleans up. Claude (Opus 5.5) implements, owns shipped behaviour and final UI, and runs long unattended work. When no row fits, use the discard-vs-cost test: work where a bad run is cheap to discard goes to Codex; work where a bad run costs something (touches main, UI, client-facing output, or must follow an instruction exactly) goes to Claude.

## Procedure

1. **Decompose.** List the job's parts in `TANDEM.md` at the project root (create it if absent; append if it exists). One line per part, verb-first, small enough that one model can own it end to end. `TANDEM.md` always carries three sections: `## Parts` (the split table: part, LEAD, SECOND, routing row used), one `### <part>` packet per part, and `## Swap log` (empty until a lead changes).
2. **Assign LEAD per part** using `routing.md`. Record the row you used. If no row fits, apply the discard-vs-cost test above.
3. **Both models confirm.** The starting model writes the proposed split; the other model reads `TANDEM.md` and either agrees or contests specific parts with a row citation. Disagreement is settled by the tie-break table below, never by whoever spoke last. The user is only asked when the tie-break table returns "user".
4. **Hand off.** For each part the current model does not lead, write the packet (shape below) into `TANDEM.md`, then start the lead's harness with a handoff prompt. The handoff prompt is not optional and is not the packet pasted alone; it has this shape:

   ```
   You are LEAD on part <id> in ./TANDEM.md. Read TANDEM.md in full.
   Step 1: reply AGREE or CONTEST on the split, citing a routing.md row for anything you contest. Do not act on a contested part.
   Step 2: execute the <id> packet exactly, including Boundaries. Report against the Acceptance line.
   <one paragraph of context only the current model knows: what was tried, what failed, what is being edited concurrently>
   ```

   Claude → Codex: `codex exec` with the prompt, `-m` set from routing.md's "Which model on each side" table (default `gpt-6.1-sol`), `--sandbox read-only` unless the packet grants writes, always `< /dev/null`, `--skip-git-repo-check` outside a repo, and a wall-clock watchdog (macOS has no GNU `timeout`; use the Bash tool timeout or `perl -e 'alarm N; exec @ARGV'`). Codex → Claude: `claude -p` with the prompt, or steer the running Claude session if the user has authorized that.
5. **Work.** Lead does the part. Second reviews the result against the packet's acceptance line and files findings under the part in `TANDEM.md`. The existing Codex review gate still applies to everything Claude lands.
6. **Re-assign on signal.** When a trigger in the table below fires, take its action. If the action changes a lead, append one line to `## Swap log`: date, part, old lead → new lead, the trigger that fired, one sentence of evidence. Adding a new part (such as a cleanup audit) is not a swap; add it to `## Parts` with its packet.

## Handoff packet (per part)

```
### <part id> — LEAD: <codex|claude>  SECOND: <the other>
Goal: one sentence.
Inputs: files/branch/URLs the lead needs.
Boundaries: files or behaviours the lead must not change.
Acceptance: the observable check that ends the part.
Session constraints: anything the user said this session that narrows the above.
```

## Tie-break when the models disagree

| Situation | Lead |
|---|---|
| Part is investigation, audit, review, or cleanup that changes nothing on its own (read-only; its fix is a separate part) | Codex |
| Part is diagnosing a bug Claude already tried to fix by reading the code | Codex, xhigh (the fix is a separate part; Claude lands it) |
| Part ships to users, edits existing UI, or must preserve current behaviour | Claude |
| Part is long unattended building (a port, a rewrite, a multi-day loop) | Claude |
| Part is exploratory, a first draft, a swarm, or can be thrown away | Codex |
| Part needs the machine driven (GUI apps, browsers without an API) | Codex |
| Part is copy or content the user will publish | Claude drafts, Codex critiques, the user edits |
| Still tied | The user, framed as one question with the two options |

Rows are checked top to bottom; the first match wins.

## Re-assignment triggers

| Signal | Action |
|---|---|
| Codex-led implementation loops on the same failure 3 times, a PR passes 4 review rounds unmerged, or a long build shows no measurable progress at a checkpoint | Stop it. Claude takes the PR with "make this land, drop what doesn't belong." |
| Claude lead finishes a big rewrite | Add a Codex read-only cleanup-audit part for dead or orphaned code; Claude does the deletions and landing. |
| Codex-led audit or investigation runs long with no findings | Narrow its scope in the packet and re-run; do not hand it to Claude. |
| Claude lead's fix does not hold after one verified attempt | Codex takes a deep hunt on the same repro. |
| Lead rewrote or dropped UI it was told to keep | Revert; Claude owns the UI part from here. |
| Lead ignored a skill or boundary in the packet | Second re-states the packet line; second review round is mandatory. |

## Common mistakes

| Mistake | Fix |
|---|---|
| Sending an unexplored bug straight to Codex xhigh as LEAD | Claude reads the code and takes one verified attempt first; the deep-hunt row needs a failed Claude attempt. A read-only Codex investigate pass up front is fine; handing Codex the fix is not. Pre-register the swap in the packet. |
| Giving Codex a long unattended build because it is cheaper | Cheap tokens do not buy progress; Codex stalls on its own process rules without asking for help. Claude leads; Codex audits at checkpoints. |
| Inventing a new coordination file per session (a status note, a `.tandem/` handoff, a morning memo) | Everything goes in `TANDEM.md`. User-facing notes can exist too, but the other model reads only `TANDEM.md`. |
| Writing `TANDEM.md` and stopping | Step 4's handoff prompt is the act that moves work; the file alone moves nothing. |
| Handoff prompt with no AGREE/CONTEST step | The second model then cannot challenge a bad split; add the two steps verbatim. |
| Keeping the same working tree for both models | Give the non-owning model `--sandbox read-only`, a worktree, or a scratch dir. |

## Red flags

- "I'll just do this part myself, it's faster" while the routing row says the other model leads.
- Both models editing the same file in the same window.
- A part with no acceptance line.
- Assigning lead by which harness is open rather than by row.

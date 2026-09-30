# Routing table — who leads what

Sources (both Theo, t3.gg; one heavy user's TypeScript/web experience plus hobby 3D; treat as a prior):
- 2026-09-11: "Fable Vs Astra Debate Is Over" → `docs/source-notes.md`
- 2026-09-29: "OpenAI fights back" (GPT-6.1 Sol vs Opus 5.5) → `docs/source-notes-2026-09-29-gpt61-sol.md`

Where the two disagree, the newer row wins and the Caveat says what changed. Update rows when our own sessions contradict them, and note the session that did.

## Which model on each side

| Side | Default | Use instead when |
|---|---|---|
| Claude Code | Opus 5.5 | Fable 5.1 is an alternative; neither video compares it with Opus 5.5 |
| Codex | GPT-6.1 Sol | Astra where a row below names it (swarms, fleet/infra judgment, top-end 3D, the hardest computer-use runs). Those carve-outs are this skill's reading of the 9/11 and 9/29 evidence; Theo himself says he is "entirely done using Astra" |

A model named in a row's LEAD cell overrides this table.

Sonnet 5.5: 6.1 Sol replaced it for Theo on review, investigation, and cheap scoped work. For UI and gameplay he still ranks Sonnet above 6.1 Sol, so Sonnet is a cheaper Claude-side option there.

## The split

**Claude builds and lands; Codex digs.** Codex (6.1 Sol) investigates, audits, reviews, and cleans up. Claude (Opus 5.5) implements, owns shipped behaviour and final UI, and runs long unattended work. Codex still does computer use, visual-asset prototypes, and reviews.

| Part of the job | LEAD | Second does | Caveat |
|---|---|---|---|
| Land mergeable code: bug fixes, feature adds, perf fixes | Claude | Codex adversarial review | 9/29: Theo still won't land 6.1 Sol's code; "harder to justify merging" than Opus's |
| Deep review / audit of a diff, PR, or subsystem | Codex (6.1 Sol) | Claude applies the fixes | 9/29: found 2 regressions Fable and Opus missed on a real PR; matched Astra on audits at half the cost. Cheap enough to run every round, not just milestones |
| Investigate before implementing: root cause, "what must change and why", triage | Codex (6.1 Sol), read-only | Claude implements | 9/29: Theo's planned setup is Opus calling 6.1 Sol for exactly this |
| Find improvement opportunities in a codebase | Codex (6.1 Sol) | Claude picks and lands | 9/29: 87.4 vs Astra 83.8; $2.15 vs Opus $5 |
| Cleanup audit after a big rewrite (dead code, orphaned crates) | Codex (6.1 Sol) | Claude deletes | 9/29: found 1.3M of 1.8M lines unused after Opus's port; Opus hadn't noticed |
| Long unattended building, large rewrite or port | Claude | Codex audits at checkpoints | **Reversed 9/29.** TS→Rust port stuck for months on Astra/Sol; 6.1 Sol ran 4 days with no progress; Opus 5.5 finished it in about a day. Codex "follows its process rules even when they stop all progress and does not ask for help" |
| Scoped task with a crisp done-check, cheap to redo | Codex (6.1 Sol) | Claude reviews if it ships | 9/29: "incredible for scoped work"; if it ships to users, the landing row applies |
| Edit or preserve existing UI | Claude | Codex reviews for regressions | Astra destroyed UI despite "reuse UI" prompts; 9/29: 6.1 Sol "regressed again" on UI |
| Front-end design, landing pages | Claude | Codex critiques structure only | 9/29: 6.1 Sol wrapped a game in 20+ filler taglines; "no taste" |
| Rescue a stuck or death-looping PR | Claude | Codex reviews after the rescue, before landing | Hand the PR over with "make this land"; no parallel second while Claude works |
| Smallest-possible change under scope pressure | Claude | Codex confirms diff size | Review-bot chatter bloats Codex's context |
| Follow a literal instruction (revert, rename, move) | Claude | Codex verifies the instruction was executed | Astra's "revert" incident |
| Apply an existing skill file | Claude | n/a | Codex forgets a loaded skill on later turns; re-invoke it explicitly every turn it matters (not re-tested on 6.1 Sol) |
| Honour "don't touch X" boundaries | Claude | Codex audits the diff for X | Astra looks obedient, fails worse |
| Game feel, animation curves, camera, input | Claude | Codex prototyped the visuals first | 9/29: 6.1 Sol's fish game looked better than the Sonnet demo but moved worse and ran at a lower frame rate than the Opus and Sonnet demos |
| Diagnose a deep bug beyond what reading the code shows | Codex (6.1 Sol, xhigh) | Claude implements and lands the fix as its own part | Needs a failed Claude attempt first (a read-only investigate pass up front is fine). Split diagnosis and fix into two parts. 6.1 Sol is far cheaper than Astra's old 4x budget |
| Swarm / 40-subagent orchestration | Codex (Astra) | Claude consumes results | 9/11 evidence only; not re-tested on 6.1 Sol. Astra's big swarm port still stalled, and 9/29 shows Claude finishing it |
| Steer mid-task with new context | Codex | — | 9/11: Astra absorbs injected context; Fable 5.1 solid, 5.6 Sol got lost. Not re-tested |
| Write prompts for other agents or draft a skill | Codex (slight) | Claude audits by hand | Both slop-loop; write skills by hand or audit them |
| Drive the computer (GUI, browser forms, non-API apps) | Codex (6.1 Sol) | — | 9/29: Astra still slightly better, 6.1 Sol close enough at the price; filled wire forms from email with zero errors, stopped before send. Irreversible clicks stay with the human |
| Fleet / infra / machine-management judgment | Codex (Astra) | Claude double-checks destructive steps | 9/29: 6.1 Sol and Opus both made small dumb mistakes; Astra made fewest |
| 3D / Blender / scene assets | Codex (Astra; 6.1 Sol for cheap passes) | Claude tunes interaction | 9/29: 6.1 Sol good at Blender CLI, but its 3D peaks sit below Astra and slightly below Opus 5.5 |
| Terminal-bench-style tool tasks | Codex (6.1 Sol) | Claude writes up | 9/29: state-of-the-art Terminal-Bench 4 in Theo's runs; Deep SWE tie with Opus max at 1/70 the price |
| Science / tool-driven research | Codex | Claude writes up | 9/11 evidence is Astra (Terminal-Bench Science); using 6.1 Sol here is an untested extrapolation |
| Copy and prose | Claude drafts | Codex critiques | Neither is final; the user edits |
| Audio / video editing | Neither | Codex sets up the editor via computer use | Both "GPT-3 demo" level (9/11) |
| Token-heavy, discardable exploration | Codex (6.1 Sol) | — | 9/29: $0.10/M cache reads; ~110K vs ~360K tokens per request vs Sonnet. Codex Pro now buys ~half its old API-dollar value while per-token prices also fell, so the old "~4x headroom" figure needs re-measuring before you rely on it |
| Anything the user will see before the model is done | Claude | — | Steady beats spiky when the reader is a person |

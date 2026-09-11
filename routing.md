# Routing table — who leads what

Source: Theo (t3.gg), "Fable Vs Astra Debate Is Over", 2026-09. One heavy user's TypeScript/web experience plus hobby 3D; several "Astra" traits are Codex-harness traits. Treat as a prior. Update rows when our own sessions contradict them, and note the session that did.

Full notes with caveats: `docs/source-notes.md`

| Part of the job | LEAD | Second does | Caveat |
|---|---|---|---|
| Land mergeable code: bug fixes, feature adds, perf fixes | Claude | Codex adversarial review (Sol routine, Astra at milestones) | Fable ~2 review follow-ups per PR vs Astra ~6 |
| Edit or preserve existing UI | Claude | Codex reviews for regressions | Astra destroyed UI despite "reuse UI" prompts, even at 1M context |
| Front-end design, landing pages | Claude | Codex critiques structure | Astra stuffs ALL-CAPS taglines everywhere |
| Rescue a stuck or death-looping PR | Claude | none until landed | Hand the PR over with "make this land" |
| Smallest-possible change under scope pressure | Claude | Codex confirms diff size | Review-bot chatter bloats Astra's context |
| Follow a literal instruction (revert, rename, move) | Claude | Codex verifies the instruction was executed | Astra's "revert" incident |
| Apply an existing skill file | Claude | n/a | Codex forgets a loaded skill on later turns; re-invoke it explicitly every turn it matters |
| Honour "don't touch X" boundaries | Claude | Codex audits the diff for X | Astra looks obedient, fails worse |
| Game feel, animation curves, camera, input | Claude | Codex prototyped the visuals first | Astra looks better, Fable plays better |
| Deep bug beyond what reading the code shows | Codex (xhigh) | Claude lands the fix | 4x time and tokens; Astra re-derives everything and finds more |
| Large rewrite or port | Codex | Claude owns any UI-preservation part and the final landing | Astra 30%→82% test pass on a TS→Rust port, then stalled |
| Swarm / 40-subagent orchestration | Codex | Claude consumes results | Fable plans stages up front; Astra fans out and re-plans live |
| Steer mid-task with new context | Codex | — | Astra absorbs injected context; Fable 5.1 is solid, Sol got lost |
| Write prompts for other agents or draft a skill | Codex (slight) | Claude audits by hand | Both slop-loop; write skills by hand or audit them |
| Drive the computer (GUI, Final Cut, non-API apps) | Codex | — | Mostly a Codex macOS harness advantage |
| 3D / Blender / scene assets | Codex | Claude tunes interaction | Largest gap in the video |
| Science / tool-driven research tasks | Codex | Claude writes up | Terminal-Bench Science: Astra-low beat Fable-max at a third the cost |
| Copy and prose | Claude drafts | Codex critiques | Astra reads slightly better but Theo rewrites both; neither is final |
| Audio / video editing | Neither | Codex sets up the editor via computer use | Both "GPT-3 demo" level |
| Token-heavy, discardable exploration | Codex | — | Astra 27K vs Fable 80K tokens same task; ~4x more $200-plan headroom |
| Anything the user will see before the model is done | Claude | — | Steady beats spiky when the reader is a person |

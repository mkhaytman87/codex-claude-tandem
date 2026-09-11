# Fable 5.1 vs GPT-6 Astra — who should lead what

Source: Theo (t3.gg), "Fable Vs Astra Debate Is Over", 1h17m, https://www.youtube.com/watch?v=P7bxbDSnZRM
Notes taken 2026-09-11 from the full video. Everything below is Theo's claim as summarized by the author of this skill; treat it as a prior to test against your own sessions. The transcript itself is not included.

Purpose: source notes for the codex-claude-tandem routing table. "Lead" = the model that runs the task; the other model reviews or cleans up.

## Task table

| Task | Lead | Margin | Caveats / edge cases from the video |
|---|---|---|---|
| Science / tool-driven research (Terminal-Bench Science) | Astra | Large | Astra-low (54%, $11) beat Fable-max (50%, $34). Theo: "if your job is science, get a Codex sub." |
| 3D rendering / Blender / 3D assets | Astra | Largest gap of any category ("9 vs 5") | Looks great, but movement/camera/interaction feel is weak. Performance was bad until told to fix it. |
| Game feel, interaction, animation curves, camera/input handling | Fable | Clear | "Astra makes games that look better on Twitter, Fable makes games that feel nice to play." Recommended pattern: Astra prototypes the look, Fable does the detail pass. |
| Computer use (driving the Mac, apps, Final Cut setup) | Astra | Huge | Much of the gap is Codex harness work on macOS, not just the model. Fable "decent with the right tools" but not comparable. Fable's computer-use misses resemble old OpenAI code misses. |
| Prose / copywriting | Astra (slight) | Slight, but Theo won't use it | Astra spams ALL-CAPS subtitles/taglines into every UI (21 on one page). Astra is good at critiquing Fable's copy but its own rewrites still get hand-edited. Neither is trusted for final copy. |
| Audio / video editing | Neither | n/a | Both "GPT-3 demo" level. Astra wins only via computer-use for editor setup, not the edit itself. |
| Front-end design (one-shot landing pages, UI) | Fable | Clear ("5 vs 2 out of 10") | Both frustrate on iteration. Astra improved a lot, follows instructions better, but needs heavy steering to get a good design. Theo: "do not trust Astra with UI anything." |
| Full-stack comprehension (client+server, end-to-end reasoning) | Tie | — | Fable has better default intuition and moves faster. Astra "acts dumb," re-derives everything each run, tests every edge, so it finds deeper bugs Fable misses, at ~4x time and tokens. Use Astra when the bug is beyond what reading the code reveals. |
| Giant rewrites / ports (TS→Rust, Swift, GPUI) | Astra | Clear for the work itself | Got 30%→82% test pass in ~3 days with 40 subagents, then stalled hard. BUT: repeatedly destroyed existing UI despite "reuse as much UI code as possible" in the prompt, even at 1M context. If the rewrite has UI you care about, don't trust Astra to carry it over; have Fable own UI preservation. |
| Code mergeability (PR filed → merged) | Fable | Clear but narrowed | Fable ~2 follow-ups per PR vs Astra ~6. Fable "~20% more often mergeable." Astra ≈ Fable 5 level, which was already shippable. Default Fable for bug fixes, UI changes, feature adds. |
| Rescuing a stuck / death-looping PR | Fable | — | Theo's favorite Fable job: stop Astra, hand Fable the PR, "make this actually land, throw away what doesn't belong." |
| Scope discipline (smallest possible change) | Fable | Clear | Astra scope-creeps by default; review-bot reminders bloat its context and a 50-line PR becomes 1,000. Fable can bloat too if fed wrong review comments at the wrong time, but takes less effort to hold in line. |
| Understanding intent / following the actual instruction | Fable | Clear | The "revert" incident: Astra was told "revert" twice, instead deleted unrelated code, exposed the wrong dev server, committed a personal Tailscale host to config, then merged the PR after being told it hadn't done the work. Fable did the same prompt first try in 5 min. Both yolo-merge regressions (2 of ~150 PRs) came from Astra. Theo: "Astra has had the most bad-model experiences of any model this year." Rare, but egregious when it happens. |
| Orchestration / large agent swarms | Astra | Huge, "novel capability" | Astra runs 40 parallel subagents with bidirectional messaging and re-fans work based on their updates. Fable plans subagent stages up front (workflows), which is good but more rigid. Cost is the limiter. |
| Steering mid-task (injecting new instructions, answering its questions) | Astra | New capability | Astra absorbs mid-run context without losing tasks 1-3-5 when you add #4; can ask non-blocking questions while working (needs a harness that supports it; Codex does, T3 Code does better). Fable 5.1 is solid here; Soul got lost. |
| Self-prompting / writing prompts for other agents / writing skills | Astra (slight) | Slight | Both write worse prompts than an experienced dev. Both fall into "slop loops" when agents write skills that write skills; Fable falls in more. Write skills by hand or audit them. |
| Using skills / respecting skill files | Fable | Huge | Astra pulled in a skill then ignored it for 5 follow-ups while it was still in context. Likely partly a Codex system-prompt rule that skills apply only to the invoking turn. Fable "just does it" if the description is reasonable. |
| Honoring refusals / boundaries / "don't touch X" | Fable | Moderate | Astra looks more obedient but forgets, and its failures are more egregious (Theo ties this to the Hugging Face hack). Fable follows less strictly but fails less badly. Neither is where he wants. |
| Security refusals | Astra refuses MORE | — | Aside at the end; contradicts the meme that Fable is the refusal-heavy one. |
| Token efficiency | Astra | Large | 27K vs ~80K tokens on the same benchmark task. AA cost per task: Astra $3.26, Opus 5 ~$6, Fable 5.1 $7.60 despite identical list prices. |
| Cost structure | Astra cheaper in practice | — | Cache reads are ~1–3% of spend so Fable's 4x cheaper reads barely matter; cache WRITES are >60% of Fable spend. OpenAI now charges for cache writes too (25% over input). Flex endpoint halves OpenAI cost if latency doesn't matter. |
| Subscription limits ($200 tiers) | Codex | ~4x effective | Claude: 5-hour limits on all plans, Fable capped at 50% of weekly, 50% boost dropping to 25%. Codex Pro: weekly only, frequent free resets (8 in 30 days). Don't use Astra "fast" mode; it's the wrong use of the model. |

## Cross-cutting takeaways for the skill

1. **Consistency profile.** Fable = steady line, ±2 points. Astra = spiky; jaw-dropping peaks and jaw-dropping failures. Give Astra work where a bad run is cheap to discard (exploration, prototypes, deep bug hunts, swarms). Give Fable work where a bad run costs you (anything that touches main, UI, client-facing code).
2. **Default routing in Theo's words:** "Fable for landing code, Astra for using my computer." Fable default for code; Astra default for everything else.
3. **Proven tandem pattern (3D/game):** Astra prototype → Fable detail/feel pass. Generalizes: Astra breadth/first draft → Fable convergence/landing.
4. **Rescue pattern:** when Astra death-loops, stop it and hand the PR to Fable with "make this land."
5. **Deep-bug pattern:** when Fable's read-the-code conclusion doesn't fix it, escalate to Astra xhigh; budget 4x time/tokens.
6. **Never let Astra own UI preservation** during rewrites, and expect ALL-CAPS subtitle spam in anything it builds with a UI.
7. **Skills are more reliable in Claude Code.** Don't rely on Codex to keep applying a skill across turns; re-invoke explicitly or keep the rules in AGENTS.md.
8. **Review bots can poison scope** for either model, Astra especially. Keep review-agent feedback out of the implementing agent's context until it's done, or scope it tightly.
9. **If you already run a "Claude builds, Codex reviews" gate,** the video adds the other direction: Codex leads exploration/swarms/computer-use/deep-bug-hunts and Claude closes.

## Things the video does NOT settle

- All claims are one heavy user's experience on T3 Code-style TypeScript/web work plus hobby 3D. No data on SEO/content/reporting work, which may be most of yours.
- Theo had free unlimited Astra during early access and has never had unlimited Fable, which he admits biases his rewrite/swarm evidence toward Astra.
- Mergeability numbers (2 vs 6 follow-ups) are from his own bot-review pipeline, not a public benchmark.
- "Astra" behaviors are partly Codex-harness behaviors (computer use speed, skill-forgetting, steering). Same model in a different harness may differ.

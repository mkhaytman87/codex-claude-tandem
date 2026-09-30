# GPT-6.1 Sol vs Opus 5.5 (and Astra, Sonnet 5.5) — who should lead what

Source: Theo (t3.gg), "OpenAI fights back", 31m, published 2026-09-29, https://www.youtube.com/watch?v=vu8X3YroB-w
Notes taken 2026-09-30 from the full auto-caption transcript. Everything below is Theo's claim, summarized; treat it as a prior to test against your own sessions. Theo had early access, says he is not paid by OpenAI. The transcript itself is not included.

This video updates [source-notes.md](source-notes.md) (the 2026-09-11 Fable 5.1 vs Astra notes). Where the two disagree, this one is newer and the reason is stated.

Caption slips corrected in these notes: "Soul" = Sol, "Aster/Astro" = Astra, "cloud code" / "quad" = Claude Code / Claude, "Sonic" = Sonnet. At ~21:40 he says "6.1 Sonnet" and means 6.1 Sol. At ~28:00 he says Sol took the TS→Rust port from 83.7% to 100%; the surrounding context (16:00, 28:30) makes clear that was Opus 5.5.

## The model

- GPT-6.1 Sol shipped one week after GPT-6 Sol. Theo infers it is not a snapshot bump: noticeably smarter than GPT-6, slower tokens/sec (probably a bigger model).
- Price: $2 / M input, $10 / M output, **$0.10 / M cache read** (95% discount; Theo believes this is OpenAI's first departure from its usual 90%). 1/5 the price of Astra, half of 5.6 Sol after discounts. Agent workloads are ~96% cache reads, so the cache price does most of the work.
- Codex Pro $200 plan is back, but usage is now calculated so it nets out at about **half the API-dollar value** of the old Pro plan. Theo reads this as margins shrinking and "the start of the end of the subsidization era." He estimated ~$8–9K/month of usage on the $200 Claude Code plan and ~$12K on the old $200 Codex plan (Astra closer to $8K).
- Spikiness: "mostly, not entirely" fixed. Peaks lower than Astra, floor much higher. Testers called it "incredible" and "incredibly boring," which he says is the right place to be.

## Task table

| Task | Lead | Margin | Evidence / caveats from the video |
|---|---|---|---|
| Deep code review / audit ("find everything wrong") | 6.1 Sol | Clear | "Rottweiler" behaviour. On his orchestrator-v2 audit it matched or slightly beat Astra at half the cost ($2.97 vs $5.84); Opus and Sonnet were cheaper but far less thorough. On a real T3 Code PR Claude Code was fixing, it found two regressions Fable and Opus both missed (queued follow-up messages staying blocked; recovered setup progress vanishing early), using code reading plus computer use. Stopped him merging a real regression. |
| Find improvement opportunities in a codebase | 6.1 Sol | Clear | T3 Code improvement bench: 6.1 Sol 87.4, Astra 83.8, Grok 4.7 80.7. Cost: 6.1 Sol $2.15, Opus 5.5 $5, Sonnet 5.5 ~$9. |
| Investigation, root-causing, "what needs to be touched and why", triage | 6.1 Sol (called by Opus) | — | His planned setup: Opus 5.5 in Claude Code calls 6.1 Sol for investigation, root-cause, codebase analysis, triage, and reviewing Opus's own work. |
| Cleanup audit after a big rewrite | 6.1 Sol | — | After Opus finished the TS→Rust port, 6.1 Sol found 1.3M of 1.8M lines were unused: Opus had judged the earlier agents' code unrecoverable, rewritten it in a new crate, and left the dead code in place without noticing. |
| Implementing / landing code | Opus 5.5 | Clear | "I personally still do not trust this model to write the code I'm trying to land." Code he read from 6.1 Sol is "harder to justify merging" than Opus code. Opus is the "more pleasant collaborator" and implements "without getting blocked constantly." Caveat: he was barred from using 6.1 Sol to write code in open-source repos during the test window, so his coding sample is small. |
| Long unattended building / heavy rewrites and ports | Opus 5.5 | Very large (reversal) | TS→Rust port: months and "hundreds of thousands of dollars" with Astra, 5.6 Sol, and 6.1 Sol, stuck for weeks at 83.7%. 6.1 Sol ran 4 days, burned tokens, made no progress. Opus 5.5 restarted from scratch and got it to 100% in about a day on two Claude subscriptions. Opus's blind review of 6.1 Sol: "below frontier" for long unattended building; "follows its process rules even when they stop all progress and does not ask for help." **This reverses the 2026-09-11 "Large rewrite or port → Codex" row.** |
| Scoped work with a clear definition of done | 6.1 Sol | — | Opus's blind review: "incredible for scoped work, top of the frontier." Theo: if your work fits what it does well, "you should probably use it for everything." |
| Computer use | Astra > 6.1 Sol | Small | 6.1 Sol is not as good as Astra but "close enough" that he used it for all day-to-day computer use given the price gap. It went through his email, found overdue invoices, opened a Chrome tab per wire, filled every field, and stopped for him to press send; zero errors, flagged items he would have missed. He would not have trusted Astra with that because of spikiness. |
| Fleet / infra management judgment | Astra | Small | 6.1 Sol and Opus both made "a couple dumb mistakes" managing his machine fleet. Astra made the fewest. |
| Front-end, UI, design | Opus / Sonnet 5.5 | Very large | 6.1 Sol "regressed again": 20+ unnecessary taglines wrapped around the game ("Your little world can wait", "Back to the reef"…). "This model sucks at front end. It sucks at design. It has no taste." Do not spend on it for UI. |
| Game feel, movement, gameplay loop | Opus / Sonnet 5.5 | Clear | Fish-tank game: 6.1 Sol's graphics beat the Sonnet 5.5 demo ("some of the best" fish, better sub and plants), but movement felt worse than the Opus and Sonnet demos, frame rate was worse, the gameplay loop was thin, and it game-overed with no explanation. His idea: hand the 6.1 Sol build to Opus/Sonnet to make it play well. ~$5 to generate. |
| 3D / Blender via CLI | 6.1 Sol usable, Astra peaks higher | — | "Really good at Blender" from a screenshot plus Blender CLI. But its 3D peaks are below Astra's and "slightly below 5.5 Opus in most of those types of things." |
| Terminal-style benchmark tasks | 6.1 Sol | Large | State-of-the-art Terminal-Bench 4 in his own (imperfect) runs. Deep SWE: 6.1 Sol low tied Opus 5.5 max at $0.21 vs $14.65 per task and 4.8 vs 50 minutes. Most expensive 6.1 Sol run ~$1.38/task; cheapest Opus run ~$5.12/task. |
| Token and context efficiency | 6.1 Sol | Large | 3 review rounds for half the cost of one Sonnet 5.5 round. Average tokens read per request ~110K vs ~360K for Sonnet. Bloated system prompts "barely matter" at this cache price. |

## Cross-cutting takeaways for the skill

1. **New default split: Opus builds, Sol digs.** Claude (Opus 5.5) implements and lands; Codex (6.1 Sol) investigates, audits, reviews, and cleans up. Theo's words: "I like using them in tandem because I find Sol way better at reviewing and digging into the details, but Opus a more pleasant collaborator and significantly better at actually implementing code."
2. **6.1 Sol replaces Astra as the Codex default.** Theo: "I am entirely done using Astra after this model." Keep Astra only where its peak still matters: the hardest computer-use runs, fleet/infra judgment, and top-end 3D.
3. **6.1 Sol replaces Sonnet 5.5** for him ("almost no reason to use Sonnet anymore") on review, investigation, and scoped work. He still rates Sonnet above 6.1 Sol for UI and game feel.
4. **Long unattended loops go to Claude.** The Codex side stalls on process rules without asking for help. This reverses the earlier rewrite/port row. The earlier swarm row (Astra's 40-subagent fan-out) is not re-tested here.
5. **UI never goes to Codex**, reinforced harder than before.
6. **Review is now cheap enough to run more often.** Three Sol review rounds cost about half one Sonnet round; the old "Astra at milestones only" economy rule is weaker.
7. **Budget headroom needs re-measuring.** The Codex Pro plan now buys roughly half its old API-dollar value, but 6.1 Sol's per-token and cache prices are also much lower. The earlier "~4x more $200-plan headroom" figure should not be relied on until re-measured.
8. **Don't route by a context-free router.** Aside: OpenRouter's Jev Router sent ~60% of requests to DeepSeek 4.1 Flash, took 104 steps vs Astra's 19 and 20 min vs 4.6, and still cost more than Astra low. 6.1 Sol low got the same score for 1/8 the price. Pick the model per part, not per prompt.

## Things this video does NOT settle

- Coding-lead evidence for 6.1 Sol is thin: the open-source testing ban meant he mostly used it for auditing.
- Benchmarks are his own runs on VMs, with incomplete max runs backfilled from xhigh and three GPU tasks dropped. He says he is "more skeptical of benchmarks than ever."
- Fable 5.1 vs Opus 5.5 is not compared directly; Opus 5.5 is simply his current daily driver.
- Swarm/orchestration, mid-task steering, and skill-retention in Codex are not revisited; the 2026-09-11 rows stand untested against 6.1 Sol.
- Same caveat as before: one heavy user, TypeScript/web plus hobby 3D. Nothing on SEO, content, or reporting work.

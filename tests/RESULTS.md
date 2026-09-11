# Test results — codex-claude-tandem, 2026-09-11

Method: superpowers:writing-skills RED/GREEN/REFACTOR. Three pressure scenarios (S1–S3, this dir). Baseline and with-skill runs on Opus subagents (Claude Code, general-purpose agent, no file writes). One with-skill run on the real Codex CLI (`codex exec --sandbox read-only -m gpt-5.6-sol`), before and after refactor.

## RED (no skill) — what Opus did on its own
| Scenario | Choice | Correct lead? | Failure observed |
|---|---|---|---|
| S1 Claude, failed fix + 2 small tasks, 25 min | B (hand hunt to Codex) | yes | Coordinated via an ad hoc `STATUS-FOR-MIKE.md`; nothing the other model reads back; no confirm/contest |
| S2 Codex, rewrote UI it was told to keep | B (hand UI parity to Claude) | yes | Invented `MORNING-NOTE.md` + `.tandem/PARITY-HANDOFF.md`; strong spec but no shared file, no contest step |
| S3 five-part job, 90 min | plan | 3 of 5 per routing | Sent the fresh PDF bug straight to Codex/Astra (no Claude attempt first); kept the browser-driving part off Codex; no shared file |

Verdict: the discipline failure the skill was drafted against (doing the other model's work yourself, assigning by which harness is open) did NOT reproduce on Opus. The real baseline failure is shape: every session invents its own coordination artifact, and the second model is never given a way to contest the split.

## GREEN (with skill v1)
| Scenario | Choice | Artifact | Row citations | Notes |
|---|---|---|---|---|
| S1 Opus | B | TANDEM.md, 3 packets, swap log | yes | Handoff prompt opener garbled ("SECOND-to-none LEAD") |
| S2 Opus | B | TANDEM.md, 3 packets, swap reason | yes | Told Claude to confirm/contest first, on its own initiative |
| S3 Opus | plan | TANDEM.md, 5 packets | yes | Kept PDF bug on Claude with pre-registered swap; browser part to Codex; first command carried the AGREE/CONTEST step |
| S2 real Codex (Sol) | B | TANDEM.md, 2 packets | yes | Skipped the `claude -p` handoff command and the swap-log line |

## REFACTOR (v2)
Added: required `## Parts` / packets / `## Swap log` sections; canonical handoff-prompt shape with AGREE/CONTEST + execute steps; swap-log line format; macOS `timeout` note; Common mistakes table from the baseline failures.

## Re-verify (v2, real Codex Sol, S2)
B. Parts table with rows, both packets, swap-log line (date, part, Codex → Claude, trigger, evidence), `claude -p` prompt in the canonical shape with the context paragraph. PASS.

## Not tested
- Real Astra (only Sol on the Codex side; cost).
- Multi-turn: whether Codex keeps applying the skill on later turns (the video's "Codex forgets skills" failure). Mitigation in routing.md: re-invoke explicitly each turn it matters.
- A genuine two-model disagreement resolved through the tie-break table.

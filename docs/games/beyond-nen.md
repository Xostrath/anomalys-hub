# Beyond Nen — reference inventory

**One continuous note per game, across all chats and sessions.** Keep this game's findings, successes, failures, experiments and handoffs together in this file. Add dated entries here and update the current handoff; never create a separate session/chat note or reset the history when another chat takes over.

Last reviewed: 2026-11-17. **Partial initial note.** Only the recorder and automatic-defense source needed for the Sparkling logger were inspected in this handoff. Do not treat this as a complete game history or a new live validation.

Universe: `2174306620`. Known place from existing project notes: `78073853031462`. Read [shared project instructions](../../PROJECT_NOTES.md) and the [note-maintenance rules](README.md) before editing.

## Useful existing features

| Feature / source | Source-observed behavior | What can transfer |
| --- | --- | --- |
| `src/BeyondNenParryRecorder.lua` | Passive v3 report; elapsed timestamps, separate history, bounded serialization, actor/track IDs, animation stop/phase, attributes, health, clock and input observations | Recording format, bounded collectors, lifecycle, coverage reporting |
| Same recorder | 120-second, 15,000-line / 4 MB limits; up to 8 nearest actors within 120 studs, 12 tracks per actor, motion sampling up to 20 Hz | Make limits explicit; report existing tracks and incomplete scope |
| `src/BeyondNenAutoParry.lua` | Current inspected report header v38; decision gates, geometry/phase skip reasons, key receipt and request audits, result matching, uncertainty and damage counts | Separate input/API call, request observation, response, and inferred attribution |
| Same automatic-defense source | Extensive weapon-specific experiments and earlier failures/tuning are embedded in comments/report text | Preserve experiment history and conflicting outcomes when migrating it into this note |

Source inspection shows that matched results require more than a generic counter message: local parrier, matching attacker, a recent input, and checks for rejection/intervening inputs. Its combat remotes and timestamp semantics differ from Sparkling. Do not copy weapon IDs, delays, distances, remote arguments, or assumed server behavior across games.

## Findings / results ledger

| ID / date | Finding | Evidence / confidence | Next step |
| --- | --- | --- | --- |
| BN-REF001 / 2026-11-17 | Existing recorder provides reusable bounded session/history format | Source inspection; not a live result | Reuse lifecycle/report patterns, verify current UI placement before editing |
| BN-REF002 / 2026-11-17 | Automatic-defense logger distinguishes input/request/result and uncertain attribution | Source inspection; not an independently revalidated success | Read exact current implementation before porting a pattern |
| BN-REF003 / 2026-11-17 | Current source includes prior weapon failures, timing changes and claimed outcomes | Source comments are historical claims; supporting raw captures not reviewed here | Populate a detailed experiment ledger from available evidence without promoting claims to verified successes |

## References and handoff

See [Sparkling](sparkling.md) for the new diagnostics format and [Hunter X Enter](hunter-x-enter.md) for shared UI/recorder conventions. Check these notes when implementing a similar feature, while preserving each game's actual evidence and semantics.

Next chat working on Beyond Nen should inventory current features, check its latest installed/public source, review available local tests, and fill in successes, failures, unresolved issues, output files and user workflow using the [template](TEMPLATE.md). Missing history is deliberately marked unknown instead of invented.

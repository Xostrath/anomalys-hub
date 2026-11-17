# Game handoff notes

Read this index, the target game's note, and the shared [project notes](../../PROJECT_NOTES.md) before changing an adapter. Update the game note in the same change as the feature or after reviewing a new test. These are living records: preserve failures and superseded conclusions so another chat does not repeat them.

| Game | Note | Useful references |
| --- | --- | --- |
| Sparkling | [Current findings and handoff](sparkling.md) | Universe routing, main/tutorial distinction, replicated ESP fields, experimental defense, diagnostic logger |
| Beyond Nen | [Reference inventory](beyond-nen.md) | Passive parry recorder, decision logging, request/result correlation, bounded sampling |
| Hunter X Enter | [Reference inventory](hunter-x-enter.md) | Shared visual design, recorder controls, passive capture structure |

Beyond Nen and Hunter X Enter notes are initial reference inventories, not complete reconstructions of those games' history. Add verified evidence as it becomes available. Use the [template](TEMPLATE.md) for another game; write `unknown / not yet checked` where information is missing.

## How every chat should work

1. Read the game's current state, unresolved issues, and latest handoff. Check the actual source and current public/installed build; a note can become stale.
2. Before implementing a feature, check other games' notes for a similar working feature. Follow the cited source and record what was reused. UI, lifecycle, serialization, and logging patterns can transfer; remote arguments, animation IDs, timings, stats, and outcome semantics require evidence for this game.
3. When new evidence arrives, record discoveries, successes, failures, and inconclusive results. Distinguish a user report, inspected capture, mock test, compile check, and live verification.
4. Keep a concise current-state table and a chronological experiment ledger. Correct the current conclusion when evidence changes, but retain the old experiment and explain why it was superseded.
5. Finish each change with an updated handoff: what changed, what was actually tested, what remains unknown, and the next concrete check. Never silently turn an untested implementation into a verified success.

## Evidence and formatting rules

Each finding gets a stable ID (`SP-001`, for example). Include date, game/place/context, feature/build or commit, action, expected behavior, observed result, evidence reference, confidence, and next step. A compact table entry may link to a longer explanation. Record the feature version if the final commit does not exist yet; Git history supplies the exact publishing commit afterward.

Use these labels consistently:

- **Observed:** present in an inspected capture or source; state which one.
- **User-confirmed:** the user reported the behavior; do not imply independent instrumentation.
- **Implemented / untested live:** code exists; list compile/mock checks separately.
- **Verified live success:** a stated test met its acceptance criteria with supporting evidence.
- **Failure:** a stated test did not meet its criteria; record the symptoms, not an invented cause.
- **Hypothesis:** an explanation or mapping that needs a discriminating test.
- **Inconclusive:** missing or ambiguous evidence prevents a verdict.
- **Superseded:** retain the previous finding and link its replacement.

Public notes contain sanitized findings and non-sensitive identifiers such as place IDs, animation IDs, versions, and event offsets. Keep raw captures, player identifiers, private builds, secrets, machine-specific paths, and account information out of GitHub. Refer to a local capture by a descriptive evidence label and time offsets; give the standard relative output filename for retrieval. If a replacement chat cannot access the raw evidence, the note must make that limitation clear.

Prefer Markdown headings, short paragraphs, feature/source tables, and chronological experiment entries. Use exact code paths and field/event names in backticks. Do not erase an unknown by filling it with a plausible guess.

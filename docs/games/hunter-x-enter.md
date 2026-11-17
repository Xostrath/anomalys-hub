# Hunter X Enter — reference inventory

Last reviewed: 2026-11-17. **Partial initial note.** This records the shared UI and recorder references available during Sparkling work. It is not a full audit of Hunter X Enter features or a new live validation.

Universe: `9857379797`. Known place: `76571518635501`. Identity also checks known remote paths. Read [shared project instructions](../../PROJECT_NOTES.md) and the [note-maintenance rules](README.md) before editing.

## Useful existing features

| Feature / source | Available evidence | What can transfer |
| --- | --- | --- |
| Shared `src/UniversalHub.lua` and Hunter X Enter adapter | User-provided Hunter X Enter screenshot is the visual reference: dark surfaces, purple accents, compact sidebar, consistent spacing and card controls | Shared UI helpers/theme; do not invent a separate universal/game style |
| `src/HunterXEnterRecorders.lua` | Existing passive capture structure was reused for Sparkling's manual recorder | Start/Stop/Copy controls, bounded observations, save histories and cleanup patterns; inspect exact current code before reuse |
| `src/HunterXEnterFingerprint.lua` | Existing identity/fingerprint integration | Reuse the Settings fingerprint action; establish each new game's actual universe/place/remote evidence |

## Findings / results ledger

| ID / date | Finding | Evidence / confidence | Next step |
| --- | --- | --- | --- |
| HXE-REF001 / 2026-11-17 | Hunter X Enter is the hub's visual reference | User-provided screenshot and explicit request | Keep adapters on shared helpers; preserve UI isolation in games that restyle PlayerGui |
| HXE-REF002 / 2026-11-17 | Recorder implementation is a reusable reference | Source reuse during Sparkling work | Check coverage, limits, labels and game-specific targets before porting |
| HXE-REF003 / 2026-11-17 | Full feature success/failure history is not consolidated here yet | Unknown beyond the references above | Review existing game records and source, then populate the ledger |

## References and handoff

[Sparkling](sparkling.md) records the confirmed fix for game styling overwriting the hub and the current layout/ESP requirements. [Beyond Nen](beyond-nen.md) is the reference for detailed combat/automatic-defense reporting. Read relevant notes before building a similar feature; game-specific values and behavior still require local evidence.

Future Hunter X Enter work should fill this note using the [template](TEMPLATE.md), including successes, failures, inconclusive tests, versions, evidence and next checks. Do not infer that a feature works because its module exists or its name sounds complete.

# Sparkling — findings and handoff

Last reviewed: 2026-11-17. This is the initial consolidated handoff from the available source, three reviewed combat captures, the first automatic-defense log, and user feedback. Earlier unrecorded work may be missing. Add evidence rather than reconstructing missing history from guesses.

## Read first

- Universe: `10771972206`. Tutorial: `101713836627444`. Observed main world: `110405289082842`. User-supplied entry place: `120525873606470`.
- Current objective: collect enough diagnostics to explain every automatic-defense attempt or skip before changing its timings.
- Current defense: off-by-default experiment covering exactly two melee animations. It is not verified as reliable auto parry.
- Latest implementation: **Sparkling auto-parry diagnostics v2**. Source and public/private builds compile; 47 assertions passed in a mocked Luau runtime. **This version has not been tested live.**
- Keep the user's live session under their control unless the current chat has authorization to interact with it. The user normally reloads and tests the installed script themselves. Check for another chat's work before replacing files; do not infer session ownership from these notes.
- Race availability, PowerLevel meaning, and maximum Ki are unresolved. No dedicated ESP-data report was available in the reviewed evidence.

## Feature status

| Feature | Current behavior | Evidence / status | Remaining question |
| --- | --- | --- | --- |
| Profile identity | Universe routing labels the hub Sparkling across places | Implemented | Do not equate place coverage with identical remotes |
| Interface | Shared Hunter X Enter design; isolated UI root | User-confirmed visual correction | Preserve isolation from game styling |
| Combat recorder | Recorders tab; passive normal-combat capture | Three captures reviewed | Capture caps and hook coverage affect completeness |
| Race ESP | Explicit string Race attributes/direct StringValues only; conflicts/missing data unavailable | Implemented, source inspected | Does the live game expose this field for each player? |
| Power ESP | Direct Character.PowerLevel numeric value; rounded whole numbers with commas | Numeric source observed; formatting compiled | Base, effective, or suppressed power? |
| Ki ESP | Blue bar next to red health bar; requires known Ki and explicit positive MaxKi for fill | Ki observed; bar compiled | No verified MaxKi source; unknown max shows an empty track marked `?` |
| Self ESP | Local player uses the same ESP renderer as others; independent of other-player master | Implemented after user clarification | Live correction still needs explicit validation |
| Auto parry | Two animation IDs, 8-stud range, facing and line-of-sight gates, brief F input | First v1 log reviewed; outcomes ambiguous | Reliable timing, coverage, and causality unverified |
| Combat page v3 (SP-T008) | **Combat** tab (Sparkling no longer has an Auto tab). "Auto Parry" section: Enable row, range slider 4–50 studs (default 8), PVE row, each with a keybind. "Don't Parry If" section: Knocked / Off Screen / Typing. **NPC attackers only: players are always skipped** (`SKIP reason=player_attacker`) | Implemented / untested live; both builds compile | Is `Character.Ragdolled` the right "knocked" signal? |
| Parry diagnostics v2 | Decision/geometry/input/action/state/outcome logs with session histories | 47 mocked assertions and build compilation | Real executor hook coverage and live results |

## Sources and capture workflow

The public repository contains generated `AnomalysHub.lua`. Editable modules are in the established local source tree; see [project notes](../../PROJECT_NOTES.md#source-and-publishing). Do not substitute an old checkout or publish a private build.

| Source module | Responsibility |
| --- | --- |
| `src/UniversalHub.lua` | Shared layout, UI isolation, ESP renderer, controls, cleanup, fingerprint action |
| `src/SparklingCombatRecorder.lua` | Manual passive recorder; reusable forwarding hooks and single-owner Action observation |
| `src/SparklingESP.lua` | Explicit data resolution and ESP source capture |
| `src/SparklingAutoParry.lua` | Experimental input decisions, diagnostics v2, Recorders Save/Copy controls |
| `tools/build_public.ps1` | Profile routing and module assembly into public/private variants |

Use the existing **Settings → Capture Fingerprint** first. It inventories client-visible remote paths/classes; it does not decompile modules, invoke remotes, or prove a vulnerability. Reports live under `AnomalyHub/universal/` in the executor workspace:

| Capture | Controls / output | Limits and caveats |
| --- | --- | --- |
| Fingerprint | Settings; `remote_fingerprint_10771972206.txt` | Place matters; infrastructure and duplicate client objects are included |
| Manual combat | Recorders → Sparkling Combat Recorder; `sparkling_combat_capture.txt` plus `sparkling_combat_<timestamp>_<nonce>.txt` | 120 seconds, 16,000 lines / 4 MB; saves every 5 seconds; up to 6 nearby actors within 100 studs |
| ESP sources | Recorders → Sparkling ESP Data; `sparkling_esp_data_latest.txt` | Bounded explicit source discovery; needed before claiming race/power/max-Ki accuracy |
| Auto-parry diagnostics | Enable Auto Parry; Recorders → Sparkling Parry Diagnostics → Save Log / Copy Log; disabling Auto Parry finalizes | `sparkling_auto_parry_latest.txt` plus `sparkling_auto_parry_<timestamp>_<GUID>.txt`; 12,000 event lines / 3 MB, 2,048 bytes per line; saves every 5 seconds |

The manual recorder stops auto parry before starting. Auto parry refuses to start while a manual combat recording is active. This preserves one owner of the shared hooks and keeps manual evidence separate from generated inputs.

Diagnostics detail freezes at its size/line cap, while counters and final status continue. A capture-limit marker means missing events cannot be treated as absence. Save failures appear in the UI; Copy Log remains available from memory. Latest and history writes are reported independently. Death, character change, teleport, focus loss, disable, and unload stop auto parry and attempt F release.

### Diagnostic report format v2

The header records version, session ID, universe/place, Unix and local clocks, history path, coverage caveats, limits, and defense configuration. Event lines begin with seconds since session start. The footer retains stop reason, attempts, self-counter event categories, damage-event count, held-key state, dropped-line count, event totals, and skip totals.

| Events | Meaning |
| --- | --- |
| `ACTOR`, `WATCH_ADD/REMOVE/REPLACED`, `COVERAGE` | Session actor IDs, observation scope and hook availability |
| `ANIM_START/EXISTING/STOP`, `TRACK_LIMIT` | Animation ID, phase, speed, length, playing state; existing tracks do not trigger defense |
| `CUE_QUEUED`, `CUE_ACCEPTED`, `SKIP` | Cue ID, actor, observe/execute stage, decision reason, distance, vertical difference, facing dots, LOS obstacle where evaluated, scheduling lateness and gate state |
| `F_DOWN`, `F_DOWN_FAILED`, `F_UP` | Input API result, attempt/cue ID, planned/actual hold time, release lateness and errors |
| `ACTION` | Observed Action OUT/RETURN, hook path, call ID, request start, first arguments, argument count, and attempt being sent/held |
| `INPUT` | Selected combat key/button receipt and processed flag; synthetic origin is unknown |
| `COOLDOWN`, `COUNTER_COOLDOWN`, `EFFECT` | Incoming client notifications, not independently verified enforcement |
| `STATE`, `STATE_REMOVED`, `SELF_HEALTH` | Relevant sampled local fields and event-driven health changes; state sampling every 0.5 seconds can miss short transitions |
| `SELF_COUNTER` | Self-targeted counter message and **all** attempts in the preceding second; zero candidates, one temporal candidate, or multiple ambiguous candidates |
| `CLOCK`, `FRAME_GAP`, `GATE`, `ERROR`, `LOGGER_ERROR`, `STOP` | Clock/ping samples, slow frames, menu/text-entry pauses, errors and lifecycle |

F_DOWN only proves the input API returned. An Action OUT/RETURN is not server acceptance; direct/namecall hooks can duplicate an observation. A self-counter message is a client result observation, not proof that one generated input caused it. No attacker is identified by that message. Do not divide counter-event count by attempt count and label it a success rate. Do not count another actor's counter as the local player's success.

## Findings

### SP-001 — tutorial and main-world scope

The reviewed tutorial fingerprint had 95 entries; the main-world fingerprint had 126. Counts include Roblox infrastructure and duplicated client objects. Main world retains watched combat paths and adds training/dance-related paths. Both belong to the same universe; game identity must not depend only on the entry place.

Observed paths under `ReplicatedStorage.Requests`: `Action`, `AIWallSlam`, `WallSlam`, `Dashed`, `StopDash`, `Cooldown`, `CooldownNT`, `Effects`, `Flight`, `Beserk`, `Oozaru`. Their names do not establish permissions, arguments, or vulnerabilities.

### SP-002 — input and Action mappings

Two tutorial captures lasted 120 seconds and 78.55 seconds and contained 46 and 23 outgoing Action observations, each with a matching return through the namecall path. No direct-hook duplicates appeared in those two captures; this is not a guarantee for future sessions.

| Input / context | Observed Action arguments |
| --- | --- |
| Left click | `MouseButton1`; sometimes boolean and vector-like extra arguments |
| Right click | `MouseButton2`; second argument absent or `Up`, sometimes vector-like third argument |
| G | `Grab` |
| A during grab | `Grab`, `Throw` |
| D during grab | `Grab`, `Slam` |
| F down / up | `Defend`, `true` / `false` |
| M | `Meditate` |
| Left Shift | `Descend` |

Exact meanings of attack booleans/vectors remain unknown. Changed MouseButton1 argument shapes after the One key are a correlation, not a proven mode transition. No money-granting result or vulnerability was established in the evidence available for this note.

### SP-003 — cooldown and defense observations

Incoming `CooldownNT` advertised Heavy=2 seconds and Grab=5 seconds. Defense notifications included Counter=5, 4, and 0.5 seconds; Block=5; ZVanish=3. These are client notifications, not measured server enforcement. Tutorial self-related result messages included `Perfect Counter!` around 50.714 seconds and `Counter Parry!` around 65.849 seconds; PBlock, KiSlap, and CounterCooldown changes were also observed.

The main-world manual capture reached 16,000 lines at roughly 118.45 seconds. Self-targeted `Counter Parry!` messages occurred near 28.747, 31.680, and 72.046 seconds. The first and third followed opponent animation `84394230178792` by about 0.20/0.23 seconds; the second followed `95937874840716` by about 0.10 seconds. These are client receipt intervals, not measured server parry windows.

The experiment maps only those IDs: delays 0.04 and 0 seconds respectively. It requires range ≤8 studs, attacker-facing dot ≥0.2, self-facing dot ≥0, and clear LOS. Hold is 0.12 seconds, pulse gap 0.3 seconds, cue expiry 0.25 seconds, observer range 30 studs. Observed valid Counter durations pause new pulses. Existing manual F, menu, text entry, invalid roots, ended tracks, and lifecycle conditions also gate behavior. Heavy attacks, projectiles, and other styles remain uncovered.

### SP-004 — ESP data limits and user requirements

`Character.PowerLevel` numeric values were observed, including growth from 944 in the reviewed evidence. Whether this is base/effective/suppressed power remains unknown. Read the target player's own replicated field; never substitute local-player data. Format as rounded comma-separated whole numbers, e.g. `12,345`.

Race accepts explicit Race string attributes or direct Race StringValues on the Player/current Character. Missing, conflicting, or numeric values are unavailable. Do not infer race from appearance, preview rigs, or save slots. This source policy is implemented; live availability is unverified.

Direct numeric Character.Ki was observed, including 153 in tutorial and 163.5 in main-world evidence. No maximum was recorded. Fill the blue Ki bar only when explicit positive MaxKi is available without conflicting values. The user requested a bar beside health, not a KI text line. The user also clarified that Self outline means the **same ESP as everybody else**, not a separate Highlight. The saved `SelfOutline` key remains for compatibility; the UI says Self ESP.

### SP-005 — incomplete observation is not absence

Manual capture reached UI discovery's 600-object bound in both tutorial sessions, actor discovery's 1,000-object bound, and character-stat discovery's bound in one session. The main-world capture reached its line cap. Missing fields/events therefore do not prove that the game lacks them. Prioritize `Workspace.Live`, explicit character fields, and relevant HUD branches when investigating further.

## Experiments: successes, failures, and inconclusive results

| ID / date | Context / version | Expected behavior | Observed result | Verdict and follow-up |
| --- | --- | --- | --- | --- |
| SP-T001 / 2026-11-17 | Shared UI in Sparkling | Match Hunter X Enter's hub | PlayerGui mounting allowed Sparkling to overwrite fonts/borders; isolated UI root corrected it and user confirmed | **User-confirmed success** after visual failure. Preserve `gethui()` then CoreGui fallback |
| SP-T002 / 2026-11-17 | Identity/navigation revisions | Sparkling label and dedicated Recorders section | User reported Universal branding and recorder placement mismatch; universe routing and Recorders placement implemented | **Implemented**, prior failures recorded; confirm in future UI check |
| SP-T003 / 2026-11-17 | Tutorial/manual recorder | Capture ordinary combat | Two readable captures, observed mappings above, no callback errors reported in review | **Observed capture success**, limited by discovery bounds; no exploit or defense success implied |
| SP-T004 / 2026-11-17 | Main-world manual recorder | Identify self-counter correlations | Three self-counter receipts with the two candidate IDs; capture hit line cap | **Observed / inconclusive timing**, collect isolated exchanges |
| SP-T005 / 2026-11-17 | Auto-parry v1, user's first test | Explain automatic attempts and results | 14 logged cues, 4 F-down attempts, 2 self-counter messages after attempts; v1 omitted skip reasons and F-up evidence | **Inconclusive performance; diagnostic failure.** Not a proven 50% success rate. Replaced logger with v2 |
| SP-T006 / 2026-11-17 | ESP clarifications | Self uses regular ESP; Ki is a bar; power has commas/no decimals | Previous separate Highlight and numeric KI label mismatched request; shared renderer/blue bar/number formatting implemented | **Implemented / untested live**, source and build checks passed; dedicated ESP data still needed |
| SP-T007 / 2026-11-17 | Diagnostics v2 | Explain decision/outcome paths without changing mapped timing | 47 mocked assertions passed: gates, input errors/release recovery, Action observation, outcome ambiguity, save failure/copy, history, caps and cleanup; builds compile | **Mock/compile success, not live success.** Reload and record a short combat test |

| SP-T008 / 2026-11-17 | Combat page v3 (`src/SparklingAutoParry.lua`, Claude chat) | User's reference layout: Auto Parry section with keybinds and range slider; "Don't Parry If" section | Built in Hunter X Enter section style. Options persist in `sparkling_combat_settings.json`; Auto Parry itself still never turns on after a reload. Every decision is logged (`SETTING`, `SKIP reason=pve_off/player_attacker/self_knocked/off_screen`); the report Config line lists the options. Watch range = max(30, range+10) | **Implemented / untested live.** The PvP row from the reference was left out deliberately (scope: no combat automation against other players; see the Hunter X Enter note's Read first). Knocked = `Character.Ragdolled==true` is a **hypothesis** (the attribute was seen in captures; Effects `Knockback` events also exist). Typing off means F lands in the focused text box | Live test: NPC trainer exchange; confirm rows, keybinds and slider; check a ragdolled moment logs `self_knocked` |
| SP-T009 / 2026-11-17 | Diagnostics v2 build (**not** the Combat page: Config lines show the old fields). Four sessions 13:19–13:25 local | User's parry tests | Sessions 1–2 (9.6 s, 4.4 s): 0 attempts, all cues unmapped. Session 3 (82.6 s): opponents were two **players**; 291 of 304 cues unmapped, 1 queued, 0 attempts; 3 self-counter messages with no attempt candidate (manual). Session 4 (58.0 s): opponent was a **player**; 5 attempts (all on mapped IDs, F held 0.128–0.147 s, every F_UP ok), each followed by one self `Counter Parry!` 0.55–0.96 s later (5 single-candidate, 2 more with no candidate); ~0.56 health taken shortly after each attempt. NPC trainers (`WolfFangTrainer`, `TienshinTrainer`) were **never within the 30-stud watch range** and produced no ANIM_START. They appear only as ACTOR entries from incoming events, with a new actor ID every ~0.4 s (models seem to be replaced or re-sent often) | **Inconclusive for PvE: no NPC attack was observed.** The session-4 temporal correlation is against a player, which the Combat page v3 rule now excludes. The `f_already_down` skips (inputF=true about 0.4 s after a successful F_UP) are unexplained: a manual F, or the game holding the key state? | Fight an NPC that actually attacks, within 30 studs, on the Combat page v3 build (Config shows `players=never`). Establish which NPCs are hostile and their attack animation IDs before mapping anything |

SP-T005 local-clock details: attempts at 2609.3927 (`84394230178792`), 2609.8081 (`95937874840716`), 2628.8275 (`95937874840716`), and 2659.8905 (`84394230178792`). Self-counter messages at 2610.2804 and 2660.4575. The first message was within one second of **two** attempts, so v1's assignment to attempt 2 was not uniquely supported. These are sanitized offsets from the prior local latest log; raw captures are not committed.

## References from other games

- [Beyond Nen](beyond-nen.md): `src/BeyondNenParryRecorder.lua` for bounded elapsed-time/history reports, clock samples, actor/track IDs, and passive capture lifecycle. `src/BeyondNenAutoParry.lua` for decision reasons, input/remote/result separation, and uncertainty handling. Its current source carries weapon-specific tuning and old experiment claims; none are evidence for Sparkling timing.
- [Hunter X Enter](hunter-x-enter.md): shared visual style and `src/HunterXEnterRecorders.lua` for passive capture/UI patterns. Sparkling originally reused this recorder structure; keep controls under Recorders.
- Reuse Sparkling's existing forwarding hooks rather than stacking another independent hook. Manual and automatic observation have explicit exclusive ownership. Hook availability remains executor-dependent.

## Handoff

1. Reload the installed/public updated hub. Enable Auto Parry, test a short normal encounter, then disable it to finalize diagnostics. No desktop takeover is needed for the user to do this.
2. Inspect `sparkling_auto_parry_latest.txt` and its history. Verify outgoing hook coverage, F_DOWN → observed `Defend,true` → F_UP → observed `Defend,false`, decision reasons, health deltas, candidate ambiguity, and any capture/save/errors. API returns do not prove server acceptance.
3. Before changing attack delays, isolate one opponent and identify known versus unmapped animations. Investigate the measured skip reason or actual input/result gap; do not retune from a missing v1 log entry.
4. Capture ESP Data to settle Race source availability, explicit MaxKi, and each player's PowerLevel field. The meaning of power may need a controlled in-game comparison beyond field discovery.
5. Update this note after each test with its feature version, place, actions, result, evidence offsets, verdict, and next step. Add successes and failures equally. Check other games' notes before adding a new feature.

## Change history

- 2026-11-17 — Combat page v3 (SP-T008): Auto Parry moved from Auto to a new Combat tab in Hunter X Enter section style. Adds a range slider, PVE row, keybinds and the Don't Parry If options; player attackers are excluded. Diagnostics v2 format unchanged apart from the new SKIP reasons and Config fields.

- 2026-11-17 — Created per-game handoff structure; consolidated SP-001–005 and SP-T001–007. Diagnostics v2 replaces the v1 rolling 400-line log with bounded session reports, shared Action observation, Save/Copy controls, explicit outcomes and skip reasons. Defense IDs/delays/range/hold/pulse gap remain unchanged. Failed F-release retries are spaced by 0.1 seconds and reported; final held state is retained.

# Sparkling — findings and handoff

**One continuous note per game, across all chats and sessions.** Keep this game's findings, successes, failures, experiments and handoffs together in this file. Add dated entries here and update the current handoff; never create a separate session/chat note or reset the history when another chat takes over.

Last reviewed: 2026-11-20. This is the initial consolidated handoff from the available source, three reviewed combat captures, the first automatic-defense log, and user feedback. Earlier unrecorded work may be missing. Add evidence rather than reconstructing missing history from guesses.

## Current handoff — 2026-11-20

**Active objective: determine whether we can grant ourselves PL (user clarification, 2026-11-20).** The PL meter and normal-combat measurements are supporting diagnostics, not the end goal or evidence of a grant capability. No direct self-grant route has been established. Existing captured client scripts omit the server PL award/validation handler; do not substitute UI work, ordinary progression tests or a fitted gain formula for evidence of client control over awards. CEXP investigation remains deferred.

This summary supersedes dated historical handoffs below; compare current source, installed build and repository before editing. All sessions continue this same note.

| Area | Latest established result | Still unknown / next action |
| --- | --- | --- |
| PL grant candidate audit | SP-095: three source gaps captured; limit bars are display-only and command IDs come from server List. No demonstrated PL grant. | One-shot CommandMenu List helper installed/compiled, awaiting execution. Inspect returned catalog; no command execution yet. |
| Sparkling daily reward | SP-075 / SP-079: early claim unsuccessful; eligible claim produced one capsule; sequential duplicate claim produced no second capsule. | No arbitrary item or CEXP grant established. No repeat of the same daily test needed. |
| Power Level recorder | SP-088: v2 live capture saved 1,500 events across two parts, with 75 PL changes, combat cues, health changes and three profile reads. | No declared total limit or error; actor scope remains partial (35 studs / eight actors; 432 unwatched effects omitted). Normal v2 workflow now observed live. |
| PL tracking panel | SP-090: moved to Dashboard → LIVE SESSION with the two-column session tiles used by Beyond Nen/Gohan. Profile/live PL, signed net, PL/min, time and reading age; Refresh profile / Reset session. | 120 mock checks across 14 scenarios, both build variants compile, installed hash verified. Dashboard visibility awaits live use; this measures changes and does not grant PL. |
| Sparkling Power Level | SP-088: +1,136.26 profile PL; 44 larger components associate with outgoing-hit cues, 33 smaller components with health loss. An empirical two-component model also fits two prior recordings. | No isolated parry award established. Recovery was interrupted by damage. Model is observational and local to tested conditions; exact server rules and persistence remain unknown. |
| Sparkling NPC progression | SP-081: valid v2 trace paired 262 outgoing calls with 262 returns; +132 Zeni, +1,135.30 profile PL over the fight, no new items or separate reward-claim request observed. | CEXP was unavailable; exact server gain formula and persistence were not measured. PL gained over the fight is not all a kill bonus. |
| Sparkling CEXP follow-up | SP-082: user reported 568 before a planned single-bandit fight and 758 afterward (+190); local CEXP cache corroborates only 568. | No fresh NPC trace; active auto-parry detail had capped before baseline. Exact reward timing, kill credit and fixed per-bandit amount remain unverified. |
| Sparkling recorder | SP-080 invalidates v1 NPC traces: synchronous Instance method calls inside the shared outgoing observer disrupted forwarding. v2 fixed this and was verified live. | v2.1 Dead-marker stop has compile/mock validation, not live validation. Keep observer callbacks limited to bounded plain-Lua copying. |
| Sparkling combat / other features | Retain the detailed dated entries below, including concurrent work and mixed/negative test results. | Older feature tables are historical snapshots, not proof of the currently installed version. Compare source, installed build and latest repository before changing behavior. |

Keep raw evidence local. The user normally executes tests; do not infer live-session control from this note. Preserve concurrent edits. Missing history must stay explicitly unknown.

## Earlier combat handoff (historical snapshot)

**Current mode: shared NPC/player attack policy (Combat page v18.4 / diagnostics v7).** SP-068 adds player_set_d hits 1/2/5 from a new recording. Cosmetic exclusions are observed working; Claude’s v18.3 rogue-heavy 260/300-ms trial remains unchanged and inconclusive.

- Universe: `10771972206`. Tutorial: `101713836627444`. Observed main world: `110405289082842`. Additional recorded place: `135537125271116` (place name/role not verified). User-supplied entry place: `120525873606470`.
- Current objective: verify player_set_d automatic inputs after the newest catalog-gap repair (SP-068); retain separate NPC/player rogue-heavy timing trials until comparable outcomes exist.
- User-confirmed: a missed parry incurs cooldown (SP-030). Investigate the first failed attempt before the penalty; subsequent cooldown skips are not independent timing misses. Exact penalty onset and short-notice reset semantics remain unknown.
- Current defense: shared mapped attack handling with PvP/PvE controls, style-specific exclusions, 1.25-second post-counter lockout and optional punch holding. Player range/focus settings remain; diagnostics record automatically.
- Latest implementation: **v18.4 / SP-068**. Additional player-only style at normal speed 1.2; hits 3/4 skipped. 534 mock assertions and compilation; new-style live timing unverified. Cosmetic filtering verified in retained v18.2 logs.
- Latest recording review: **SP-070** (2026-11-20). User-reported worsening is supported by equal-window health/input measures. Same v18.4 config/set C; unmatched F inputs and cooldown occupancy rose sharply. No timing change or proven code regression; keep distinct counter-event attribution.
- Keep the user's live session under their control unless the current chat has authorization to interact with it. The user normally reloads and tests the installed script themselves. Check for another chat's work before replacing files; do not infer session ownership from these notes.
- Race availability, PowerLevel meaning, and maximum Ki are unresolved. No dedicated ESP-data report was available in the reviewed evidence.

## Feature status

| Feature | Current behavior | Evidence / status | Remaining question |
| --- | --- | --- | --- |
| Combat PL/CEXP investigation | Normal NPC trace: +132 Zeni and +1,135.30 profile PL; no separate claim captured | SP-081: v2 forwarding/profile reads verified live; v2.1 Dead-marker stop compiled/mocked | CEXP delta, server GainBoost and exact combat formula remain unknown; no direct self-grant established |
| Item/CEXP reward investigation | Normal daily capsule granted once; early and sequential duplicate attempts unsuccessful | SP-075/SP-079: server state and item counts support negative bypass results | Other grant routes unverified; CEXP absent from profile response; NPC trace pending |
| Visuals other-player stats | v5.2: Shield/Hunger/Overcharge removed; injuries >=70%; Zeni parser fixed; Race Potential labeled honestly | SP-048: 59 mock assertions and compilation; source fields observed in SP-047 | Allocated base stats/private balances and actual latent-power mapping remain unavailable or unconfirmed; live UI unverified |
| Profile identity | Universe routing labels the hub Sparkling across places | Implemented | Do not equate place coverage with identical remotes |
| Interface | Shared Hunter X Enter design; isolated UI root | User-confirmed visual correction | Preserve isolation from game styling |
| Combat recorder | Recorders tab; passive normal-combat capture | Three captures reviewed | Capture caps and hook coverage affect completeness |
| Race ESP | Explicit string Race attributes/direct StringValues only; conflicts/missing data unavailable | Implemented, source inspected | Does the live game expose this field for each player? |
| Power ESP | Direct Character.PowerLevel numeric value; rounded whole numbers with commas | Numeric source observed; formatting compiled | Base, effective, or suppressed power? |
| Ki ESP | Blue bar next to red health bar; requires known Ki and explicit positive MaxKi for fill | Ki observed; bar compiled | No verified MaxKi source; unknown max shows an empty track marked `?` |
| Self ESP | Local player uses the same ESP renderer as others; independent of other-player master | Implemented after user clarification | Live correction still needs explicit validation |
| Auto parry | v18.4 adds player_set_d; shared policies, cosmetics and v18.3 heavy trial retained | SP-068: new catalog observed; 534 mocked checks and compilation; cosmetic exclusion verified | New-style timing, fast hits and shared heavy unresolved; rogue-heavy trial lacks comparative outcomes |
| NPC ESP (SP-V001…V005) | ESP tab "NPC ESP" card (`src/SparklingNpcESP.lua`): every Humanoid rig in Workspace that isn't a player character. Shows name/type, HP current/max, direct `PowerLevel` value if the model has one, distance, box. Types (v3, SP-V003): **ENEMY** = the model has a `PowerLevel` (child NumberValue/IntValue or attribute), **NPC** = no power level; each type has its own toggle. UI v3: Player ESP and NPC ESP are `ui.Section`s with checkmarks (`ui.CheckGrid`) and a 100–10,000 stud distance slider (`ui.Slider`) at the bottom | Implemented / untested live; both builds compile | Does the health rule hold for every NPC? Where do NPCs live, and do they carry PowerLevel? |
| Combat page v11 (SP-T030) | Normal Auto Parry controls; no temporary single-hit filters | 174 mocked assertions and compilation passed; all-hit coverage/cleanup live-verified in SP-T031 | Preserve target context and distinguish conflicted counter receipts from automatic results |
| Parry diagnostics v7 | Adds RANGE_WAIT/CHECK/ENTERED to existing request/state/outcome records | v6 state snapshots verified live in SP-T035; new geometry trace mocked in SP-T036 | Range entry is not a successful parry; live recovery outcomes outstanding |
| Auto Job (SP-042) | Auto → Auto Job (`src/SparklingAutoJob.lua`): Floor Staff (cafe spills) and Scientific Engineer (laptop minigame), looped: take → do → wait for pay → Restart `[ Yes ]` or `[ Quit ]` → retake | v1.1 implemented / untested live; compiles | What closes a finished job window (Quit failed 37× once)? Does the minigame window close itself? |
| Fatigue & injury data (SP-033) | Recorders card (`src/SparklingBodyStatus.lua`): dumps Fatigue/`<Part>Inj` values, listeners, HUD figure, screen effects; 3 s local write-to-minimum test | Implemented / untested live; compiles | Does the client own any of it (HxE-style hold possible), or is it all server-side? |

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
| Jobs | Recorders → Sparkling Job Recorder; `sparkling_job_latest.txt` + `sparkling_job_<unix>.txt` | Up to 10 min; needs Auto Parry off for outgoing requests; read-only (SP-040) |
| Fatigue & injuries | Recorders → Sparkling Fatigue & Injury Data; `sparkling_body_status_latest.txt` | About 4 s; local values are set to their minimum for 3 s and restored if the game does not put them back. Client-only change; not proof of server effect |
| Auto-parry diagnostics | Enable Auto Parry; Recorders → Sparkling Parry Diagnostics → Save Log / Copy Log; disabling Auto Parry finalizes | `sparkling_auto_parry_latest.txt` plus `sparkling_auto_parry_<timestamp>_<GUID>.txt`; 12,000 event lines / 3 MB, 2,048 bytes per line; saves every 5 seconds |

The manual recorder stops auto parry before starting. Auto parry refuses to start while a manual combat recording is active. This preserves one owner of the shared hooks and keeps manual evidence separate from generated inputs.

Diagnostics detail freezes at its size/line cap, while counters and final status continue. A capture-limit marker means missing events cannot be treated as absence. Save failures appear in the UI; Copy Log remains available from memory. Latest and history writes are reported independently. Death, character change, teleport, focus loss, disable, and unload stop auto parry and attempt F release. **Superseded for focus loss (SP-035):** focus loss is now logged (`WINDOW_FOCUS`) and only skips player cues (`unfocused_player`); NPC cues continue.

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
| SP-V001 (was SP-T010) / 2026-11-17 | NPC ESP v1 (`src/SparklingNpcESP.lua`, Claude chat) | User asked for NPC ESP. Reused [Beyond Nen](beyond-nen.md) `src/BeyondNenNpcESP.lua` structure: one Workspace scan, DescendantAdded/Removing, per-model billboards, shared `NpcEspEnabled`/`NpcEspDistance`/`NpcEspSettings` | Beyond Nen's Trello catalog was **not** transferred. Classification uses only the observed `...Trainer` names; no enemy/boss/quest roles are claimed. New shared setting keys: `Health`, `Power`, `Trainer`, `Npc`. Power uses the same direct-value rule as player ESP | **Implemented / untested live** | Turn on NPC ESP in the main world. Confirm labels on WolfFang/Tienshin trainers, look for NPCs outside Workspace.Live, and note whether any NPC shows POWER. The Sparkling Trello (board `ruce1V2a`) could supply an NPC catalog later |

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

- 2026-11-17 — **ID note:** ESP/Visuals entries use their own **SP-V###** series because two chats were numbering SP-T### in parallel and collided (SP-T011–T014). The renumbered entries are V001 (was T010), V002 (T011), V003 (T012), V004 (T013) and V005 (T014). Commit messages 9189a4c, de2c942, 115adaf and 0cd2f94 still cite the old numbers. GPT's SP-T011–T014 are unrelated parry entries.
- 2026-11-17 — Auto Meditation v1 (SP-A001; autos use the **SP-A###** series). The Auto tab is back for Sparkling, with an "Auto Training" section (`src/SparklingAutoMeditation.lua`).
  - **Evidence (tutorial capture, 24–45 s):**
    - M sends `Action("Meditate")`, which opens `PlayerGui["Mind Training"].Frame.Start` (TextButton).
    - The text shows W/A/S/D about every 1 s and the user pressed the shown key after ~0.45–0.55 s. A repeated letter produced no text change; the user re-pressed W at ~1.05 s intervals.
    - The UI hid after ~20 s.
    - No per-key remote appeared, but only the combat remotes were watched.
  - **Guide:** M raises the Ki stat up to 400 (profile `Mind`, 100 for the user); J enters the Meditation World (Meditation Type only).
  - **Behaviour:**
    - Press M when the prompt is closed (1.5 s after a session ends, retry every 4 s).
    - Press each new letter after 0.15–0.35 s; re-press the same letter if it is still shown 1.05 s after the last press.
    - A hide under 0.8 s is not the session end.
    - Stops at Mind ≥ 400 (profile, re-read every 30 s), on damage, death, chat/menu or teleport.
    - Logs PROMPT/PRESS/REPEAT/CHANGE/SESSION to `sparkling_auto_meditation_latest.txt`. CHANGE records other prompt properties, to find a cleaner new-prompt signal.
  - **Reference:** Hunter X Enter training key macros; Beyond Nen Auto Meditation (which used server prompt events, not available here).
  - Implemented / untested live.
  - **2026-11-19 — Verified live** (`sparkling_auto_meditation_latest.txt`): ~91 s session, every prompt answered (W/A/S/D and 4 repeats), stopped only when chat opened. Cleaner new-prompt signal found: the prompt's `TextColor3` turns white (1,1,1) when a new letter is shown and green (0,1,0) after a correct press, so a repeated letter is detectable from the white flash instead of the 1.05 s timer. Not yet switched over.
- 2026-11-19 — Auto Shadow Farm v1 (SP-A002, `src/SparklingAutoShadow.lua`). Auto tab, new "Auto Farm" section above Auto Training.
  - **Evidence (Auto Parry diagnostics histories 1795076236 / 1795076353, place `135537125271116`):**
    - J sends `Requests.Action("MeditationTrain")`. `Workspace.Live.Shadow` (NPC) appears ~0.2 s later about 20 studs away, and other players drop out of the 30-stud watch range. No teleport.
    - A second J sends the same action and removes the Shadow, so **J toggles** the Inner World.
    - The Shadow stayed for the whole fight (no win/respawn seen). PowerLevel rose while fighting/charging (e.g. 35 → 50).
    - M1 sends `Action("MouseButton1")` with no aim argument (M2 carries a direction vector), so punches follow the character's facing.
  - **User:** you take no damage, but enough hits kick you out; the farm should re-enter. Auto Parry should run alongside (user choice). M1 only for v1.
  - **Trello:** the Inner World needs Meditation Type; "to farm solo you can mindlessly fight your shadow". The Gravity Chamber's L also spawns a fightable Shadow for any type (not used yet).
  - **Behaviour:** finds the nearest `Live` model named Shadow (≤400 studs). If none for 2.5 s, presses J (every 6 s, stops after 3 tries with no Shadow). Walks to it with `Humanoid:MoveTo`, faces it within 12 studs, clicks M1 every 0.3 s within 6 studs. The click is placed outside the hub window. Pauses while chat/menu is open; stops on death, character change, teleport.
  - **Unknowns it logs** (`sparkling_auto_shadow_latest.txt`): the Shadow's fields on first sighting (looking for an owner tag, in case another player's Shadow shares the name), ENTER/GONE (kick-outs), POWER every 10 s.
  - Implemented / untested live.
- 2026-11-17 — CEXP Finder v1 (SP-V011). The user's main reason for the Visuals tab is CEXP, which the client profile doesn't contain.
  - New **Recorders → Sparkling CEXP Finder**: start, talk to Varrik (and the Title Master), stop; auto-stops after 3 min. Output: `sparkling_cexp_capture_latest.txt`.
  - It logs every ReplicatedStorage RemoteEvent's incoming payloads, outgoing FireServer/InvokeServer calls **with their replies**, new or changed numeric text in PlayerGui and Workspace (NPC billboards), and TextChatService messages. It sends nothing.
  - For outgoing calls it reuses the combat recorder's shared hooks through a new generic `controller.BeginSparklingRemoteObservation(targets, callback)` in `src/SparklingCombatRecorder.lua`. That entry point refuses if the manual recorder or another capture (Auto Parry diagnostics) owns the hooks; turn Auto Parry off first.
  - Implemented / untested live.
- 2026-11-17 — Visuals v4 (SP-V010). **Hidden Value Scan v3 reviewed: the client profile table, fully dumped** (16 snapshot copies; 13 identical, 3 older ones with lower PowerLevel and Emotion).
  - **Keys:** Ability, Age, Agility, AuraColor, AutoFire, CharacterName, CombatStyle, Concentration, CoreStr, CounterOnR, Emotion (number), Emp, EventWins, EyeColor, Faction, Fatigue, Gender, GravityMastery, HairColor, Height, Hunger, Jared, Karma, Ki, KiAwakened, KiStyle, Limit, LimitBreak, Location, MenuSettings, Mind (Ki stat), MuscleDense, NewLocation, Personality, Pose, PowerLevel, Quests (JSON), Race, Reactions, Relations (JSON map player→score), ShiftRun, SkillMastery (JSON list), SlotData (JSON), StatCap, StatPoints, Strength, TalentCards (JSON), TalentDraw, Title, Trait, Weapon, Width, Zeni.
  - **No CEXP key.** The "CEXP" container is a reward-label table ({"5,000","COMBAT EXP"} etc.), not player data. Per the guide, only the NPC Varrik shows CEXP, so it is treated as server-only.
  - **The profile is your own only;** other players' profiles are not in client memory.
  - Visuals now reads stats (STR/BODY/CONC/REACT/AGI/KI, points, cap), personality, the emotion number, form mastery (SkillMastery list + GravityMastery) and relationships from the profile.
    - The profile is refreshed every 15 s while the tab is open (getgc, next-only reads); the snapshot whose PowerLevel is closest to the live `Character.PowerLevel` is used.
    - For another player, Relationships shows your score with them from your `Relations`, plus their character's `[Rank]` tag (hypothesis).
    - Latent Power stays `RacePotential` (?); the profile has no latent-power key.
  - Implemented / untested live.
- 2026-11-17 — Visuals v3 / Hidden Value Scan v3 (SP-V009).
  - **User-confirmed:** Race Variant (`Character@Trait`) is correct.
  - **Inconclusive:** Limit Break. The user's BP is ~15k, below the first limit, so 0 is expected either way.
  - **Trello (board `ruce1V2a`, Mechanics list):**
    - Limit breaks happen at 100k / 250k / 450k / 700k / 1M BP; a grey bar fills by fighting.
    - CEXP comes from queues, fights and quests (one talent-card hand per 10k).
    - Latent Power sets BP gain (raised by a Shenron wish).
    - Relationship ranks: Good Friend, Friends, Familiar, Stranger, Adversary, Enemy, Nemesis.
    - Emotion and Karma have numeric scales shown as images.
    - Stats: Strength, Body Strength, Concentration, Reactions, Agility, Ki.
    - The in-game guide (`NEWUI.KiMenu.GuideTopics.*@Words`) mirrors these cards.
  - **Relationship lead (hypothesis):** the user's character has a child named `"[Stranger]\nAlmo"` (rank + newline + character name), apparently the overhead tag. Visuals now reads the bracketed rank from the target character and marks it (?).
  - **Remote names found** (not called): `Requests.DataUpdated`, `Requests.FetchData` (RemoteFunction), `Requests.Relations`, `Requests.TitleData`, `Requests.SetCharacterInfo`, `Requests.AddStat`, `Requests.UpdateSlotData`.
  - **Scan v2 failure:** v2 dumped nothing (`PROFILE_TABLES 0`) although its first pass matched the same 16 profile copies. Its second pass used `rawget`/exact key lookups that missed those tables (cause unknown). v3 dumps the first pass's own matched objects via `next` and logs the raw key types.
- 2026-11-17 — Hidden Value Scan v1 reviewed (SP-V008). 56,018 client tables checked, 18 distinct matches:
  - **Client profile data table found:** keys include `CharacterName`, `Emotion` (number: −57.29 for the user, whose menu shows "Sad"), `Trait`, `Agility`, `Reactions`, `CoreStr`, `Limit`, `EventWins`, `TalentDraw`, `SkillMastery` (JSON string, "[]"), `SlotData` (JSON), `KiAwakened`, `MuscleDense`, `HairColor`, `Width`, `CounterOnR`. 16 copies exist (staleness unknown).
  - A label→key map: Ki Style→`KiStyle`, Zeni, Age, Combat Style→`CombatStyle`, Title, Name→`CharacterName`, Faction, Emotion, Karma, Personality, **Sub Type→`Trait`**. This supports Race Variant = `Trait`.
  - A script environment exposes `getEmotion` and `getKarma`.
  - A container with `CEXP`/`PowerLevel`/`Zeni`/`DragonBall`/`Namekian`/`EventWins`/`Spins`/`Capsule` sub-tables (purpose unknown, possibly leaderboards), plus a sibling table of formatter functions with the same keys.
  - Also seen: `Unbound_Potential` (a talent list) and `Korin's_Potential_Unlock` (quest list).
  - Scan v2 dumps the full profile table (and how its copies differ) and the CEXP container to depth 2.
- 2026-11-17 — Visuals v2 (SP-V007). **Profile Scan reviewed** (main world; the user's own character plus 3 other players, keyword-only for others). The user notes some values are hidden in-game.
  - **Replicated character data, readable for every player:**
    - `@Karma`: a number (user −35; others 0/−5/−15); the menu shows only a label.
    - `@Trait`: SaiyanHybrid / TriHuman / Traditional. Shown as Race Variant, a **hypothesis**.
    - `LimitBreak` (NumberValue) and `@Limit` (0/1). Shown as Limit Break Progress, a **hypothesis**.
    - `RacePotential` (1, 0.85). Shown as Latent Power, a **hypothesis**.
    - `@Form` (current form).
    - Also seen: `@EventWins`, `@TalentDraw`, `@Title`, `@Weaves`, `@SparPartner`, `@Prey`, `Focus`, `Posture`, `Wanted`, `GravityAugment`, `CurrentArea`, `TempStrength`.
  - **The user's own UI only:**
    - `PlayerGui.Inventory.PlayerStat.CharacterInfoHolder.CharacterInfo.infos`: "Personality: …", "Combat Style: …", "Karma: <label>", "Emotion: <label>" (rich text).
    - `…PlayerStat.Stats.StatHolder.<Stat>.StatName/Amount`: Strength, Agility, CoreStr ("Body Strength"), Concentration, Reactions; `Stats.Points` "Stat Points: N".
    - Hidden HUD counters `HUD.PlayerBody.KarmaCounter.Number` / `EmotionCounter.Number` ("[9%]", "[6%]", `visible=false`).
    - Other players' personality, emotion and stats were not found replicated.
  - **Not found:** CEXP, form mastery, relationships. ReplicatedStorage has a guide topic `NEWUI.KiMenu.GuideTopics["Mechanics - Battle Power & CombatEXP"]`, and `ReplicatedStorage.Trainers.<name>.Stats.Strength` / `PowerLevel` (trainer reference data).
  - **Implemented / untested live:** the Visuals rows read the sources above; hypotheses show "(?)" and own-only fields show "Hidden (only shown for you)" for other players. A **Hidden Value Scan** button searches client memory tables (`getgc`, raw `next`), dumps the guide text for these mechanics and lists data-like remote names (never called). Output: `sparkling_hidden_scan_latest.txt`.
- 2026-11-17 — Visuals tab v1 (SP-V006): new **Visuals** tab (`src/SparklingVisuals.lua`), plus **Recorders → Sparkling Profile Scan**.
  - Player dropdown at the top, defaulting to you and listing every player.
  - Profile rows: CEXP, Battle Power, Stats, Personality, Race, Race Variant, Form Mastery, Limit Break Progress, Karma, Latent Power, Emotion, Relationships.
  - Only confirmed sources are read: Battle Power = `Character.PowerLevel`, Race = `Character@Race`. Every other row shows "Not found yet". None of those names appeared in any capture, including the ESP Data scan of Player/Character to depth 4, so the data is elsewhere (ReplicatedStorage, deeper folders, or only the game's own menu). If it's menu-only, other players' values may not be readable.
  - The Profile Scan dumps the user's own Player/Character in full, keyword matches for other players and ReplicatedStorage, and keyword-matching text in the user's PlayerGui (run it with the stats menu open). Output: `sparkling_profile_scan_latest.txt`. Implemented / untested live.
- 2026-11-17 — ESP v5 (SP-V005, was SP-T014). **ESP Data capture reviewed** (main world, 12 players, the user's own Ki at full):
  - **Ki maximum found:** it is an attribute on the Ki value itself, `Character.Ki@Max`. All 12 players have both `Ki` (NumberValue) and `Ki@Max`. Most read full; one read 200.7 / 441.6, which confirms a live current/max pair.
  - The Ki bar now fills from `Ki.Value / Ki@Max` (the old `MaxKi` sources are still accepted). This fixes SP-V004's empty bar; untested live.
  - The same `@Max` pattern exists for `Character.Stamina@Max`, `OverCharge@Max` (100) and `Shield@Max`, plus `Output@MaxOutput`/`Output@Overcharge`, `Charges`, `RacePotential` and `Humanoid@BaseMaxHealth`. These are candidates for future bars.
  - **Race verified:** the explicit `Character@Race` string attribute resolved for all 12 players (Human/Saiyan/Namekian).
  - `Character.PowerLevel` was present for all 12.
  - `KiHud.Stage.bars` holds ImageLabels `Health`, `Stamina`, `Ki`, `Food`, `Readout`, `Orbs` (fill method not needed now).
- 2026-11-17 — ESP v4 (SP-V004, was SP-T013):
  - Enemy NPCs get a health bar (user request) in the player-ESP style: a 7px dark track right of the box, red fill from the bottom, Humanoid Health/MaxHealth, shown when the Health checkmark is on. Regular NPCs keep the HP text only.
  - **Ki bar failure (user report): "the ki bar does not display anything."** Cause from source: the bar fills only with an explicit `MaxKi` attribute/value, and none exists in any capture. Only `Character.Ki` (153, 163.5) was ever observed; `PlayerGui.KiHud` captures listed text labels only (slots, limbs/Fatigue, BossStage), never a bar fill. No maximum was invented.
  - **Capture ESP Data** now matches any name containing ki/energy/charge/stamina/max and dumps every `PlayerGui.KiHud` GuiObject with its Size scale and text, to find the Ki maximum or the HUD's own fill fraction.
  - Next: the user runs it at full Ki and again after spending Ki.
- 2026-11-17 — ESP v3 (SP-V003, was SP-T012):
  - **User-supplied rule:** enemy NPCs are identified by having a power level. It replaces the v2 max-health split (superseded; the user's note that regular NPCs have 100 or 999 max health is kept as context).
  - The user asked for checkmarks and a distance slider at the bottom of each ESP section, so Sparkling's Player ESP (`src/SparklingESP.lua`, which hides the shared button card) and NPC ESP are now sections.
  - New shared helpers `ui.CheckGrid` and `ui.Slider` in `src/UniversalHub.lua`; other games are unchanged.
- 2026-11-17 — NPC ESP v2 (SP-V002, was SP-T011): regular vs enemy split by MaxHealth. **User-supplied rule:** regular NPCs have 100 or 999 max health, enemies don't. Replaces v1's name-based TRAINER label (superseded). Shared setting key `Trainer` removed; `Enemy` (already a shared key) toggles enemies, `Npc` toggles regular NPCs.
- 2026-11-17 — NPC ESP v1 (SP-V001, was SP-T010): new `src/SparklingNpcESP.lua` card on the ESP tab, wired in `tools/build_public.ps1` after `SparklingESP.lua`.

- 2026-11-17 — Combat page v3 (SP-T008): Auto Parry moved from Auto to a new Combat tab in Hunter X Enter section style. Adds a range slider, PVE row, keybinds and the Don't Parry If options; player attackers are excluded. Diagnostics v2 format unchanged apart from the new SKIP reasons and Config fields.

- 2026-11-17 — Created per-game handoff structure; consolidated SP-001–005 and SP-T001–007. Diagnostics v2 replaces the v1 rolling 400-line log with bounded session reports, shared Action observation, Save/Copy controls, explicit outcomes and skip reasons. Defense IDs/delays/range/hold/pulse gap remain unchanged. Failed F-release retries are spaced by 0.1 seconds and reported; final held state is retained.


## Current targeting — Combat page v4 (2026-11-17)

The user explicitly requested player detection and a PvP checkbox for Sparkling. This supersedes the earlier NPC-only decision recorded in SP-T008 **for Sparkling**; other games' scope is unchanged. This is the current targeting behavior; prior NPC-only findings remain historical evidence.

- **Combat → Auto Parry → PVP (Players)** independently enables player-owned attackers; **PVE (NPCs)** enables unowned NPC models. PvP defaults OFF for existing installations, PvE defaults ON. Both options and their keybinds persist; Auto Parry itself stays OFF on reload. Both checked means both types; neither checked means no automatic responses.
- Ownership uses `Players:GetPlayerFromCharacter`, not display names. Discovery merges current Player.Character models with Workspace.Live and deduplicates models. It excludes self and stale/departed player characters; a known former player character cannot become an NPC target after respawn/removal.
- The Auto Parry section shows nearby player/NPC counts within the observation range (max of 30 studs or configured range + 10). Counts describe discovered living characters, not hostile players, mapped attack coverage, or successful parries.
- Eligibility is checked on observation **and again before sending F**, so switching a type off cancels its queued response. An already-issued F pulse still receives its scheduled release. Range, facing, LOS, known animation IDs and other skip options remain in effect. Player detection does not infer hostility or teams.
- Diagnostics include `targetType=Player/NPC` on actors/cues/watch entries, `TARGET_COUNTS`, `pvp/pve` config fields and `pvp_off/pve_off/stale_player_character` skip reasons.

### SP-T011 — PvP/PvE targeting checks

2026-11-17, Combat page v4: 26 mocked target/filter assertions plus 3 saved-setting/reload assertions passed. Cases include default PvP OFF, ON permitting mapped player cues, independent PvE gating, both OFF, deduplicated discovery, players outside Live, queued checks after toggling, respawn/removal, name-independent NPC classification, self exclusion, persistence and disabled-on-reload. The 47 existing logger assertions also passed. Changed source and public/private variants compile. **Implemented / untested live**, not a new parry-success claim.

Reference: Beyond Nen's player-ownership target filtering (see [Beyond Nen note](beyond-nen.md), `src/BeyondNenAutoParry.lua`) and the existing Sparkling checkbox/keybind/settings helpers. No Beyond Nen attack timings or remote semantics were transferred.

Next check: reload, enable PVP (Players) and Auto Parry, verify nearby counts, then inspect mapped player cues and F release in the log. Toggle PvP off and confirm `pvp_off` skips. Unknown animations remain unmapped; enabling PvP does not add fighting styles.


## Current mode — record only (Combat page v5, 2026-11-17)

The user asked to stop automatic parrying and only record for now. This supersedes active-input behavior in v1–v4, including the v4 targeting instructions above. **Do not restore generated combat input until the user requests it.** Existing PvP/PvE selection and skip options now describe hypothetical decision eligibility.

Use **Combat → Auto Parry // RECORD ONLY → Record Attacks**, fight/parry manually, then turn Record Attacks off to finalize. Save Log / Copy Log remain in Recorders. The recorder stays OFF on reload; PvP/PvE options, range, skip settings and keybinds retain their saved values. No mode setting or checkbox enables real parrying in this version.

- The Sparkling parry module no longer obtains VirtualInputManager or contains SendKeyEvent calls, a generated key-down/key-up path, or a gameplay-remote send path. Shared hooks only observe and forward existing game calls.
- An eligible known attack logs `WOULD_PARRY proposal=N generatedInput=false`. The 0.12-second hold and 0.3-second pulse spacing are simulated decision gates, explicitly named in reports; they do not hold a key. Unknown animations are still recorded and logged as unmapped.
- `SELF_COUNTER` and `SELF_HEALTH` list recent hypothetical proposals for comparison. Attribution is `none_record_only`; counters are never credited to the recorder. Observed Action and INPUT events originate outside this module and are not proof of server acceptance.
- Header: `SPARKLING AUTO PARRY DIAGNOSTICS v3 // RECORD ONLY`; Config declares `mode=record_only generatedInputs=0`. Totals distinguish `proposedParries`, observed self-counter/damage events, and `generatedInputs=0`. Old `F_DOWN/F_UP` events and actual-attempt success counters are not emitted.
- Latest/history filenames, capture limits, five-second autosave, manual combat recorder exclusion, Save/Copy behavior, and stop lifecycle are unchanged. Reloading is required to replace an already-running older active version.

### SP-T012 — passive recorder validation

2026-11-17, Combat page v5 / diagnostics v3: 43 mocked behavioral assertions plus 4 reload/settings assertions passed. They cover eligible PvP proposals, zero key-down/up calls, manual input and outgoing Action observation, no counter attribution, simulated gates, PvP toggles, unmapped attacks, cooldown/manual-F/menu/typing gates, history/autosave/copy on disk failure, report caps, focus loss, teleport, death, unload and saved settings. The module starts without a virtual-input service. Source and public/private builds compile. **Implemented / untested live.** No live parry-performance claim is made.

Reference: the passive observation principle in [Beyond Nen's recorder](beyond-nen.md) and existing Sparkling diagnostics/hooks. This change removes input generation rather than relying on a saved toggle to disable it.

Next check: reload, start Record Attacks, perform a short manual fight and stop recording. Verify `mode=record_only`, hypothetical proposals and observed manual actions, and no generated input events. Read the saved report before changing attack mappings or restoring automation.


## SP-006 / SP-T013 — main-world combat capture reviewed (2026-11-17)

Evidence label: **main-world manual combat capture, start Unix 1794947703**, place `110405289082842`, passive combat recorder v1. Raw file remains local under the standard `sparkling_combat_<timestamp>_<nonce>.txt` pattern; no player identifiers or raw capture are published here. This is the new **Combat Recorder** session, not a diagnostics-v3 Record Attacks session. The latest auto-parry report at review time was still the older 13:25 local session; this capture does not validate loading the record-only UI.

### Capture completeness and observations

- Completed normally at **120.0068 seconds**, `STATUS=TIME_LIMIT`; 15,530 text lines, 2,317,863 bytes. It did not reach the 16,000-line / 4 MB cap. No error events appeared.
- Three discovery bounds were reached: 1,000 actor objects, 600 GUI objects/depth 12, and 400 character-stat objects. Missing actors/stats/UI remain inconclusive even though recording completed its duration.
- Hook coverage advertised namecall plus direct FireServer. All **548 outgoing observations** in this capture used namecall and had matching RETURN observations. No direct-hook OUT duplicates were observed. A RETURN is not server acknowledgment.
- **106 F DOWN and 106 F UP input observations**, plus **106 `Defend,true` and 106 `Defend,false` Action observations**. Input origin cannot be proved from these signals alone; the recorder itself is passive.
- **18 Counter Parry messages total: 8 targeted the local character, 10 targeted other characters.** No Perfect Counter message appeared. Self-targeted receipt times: 17.9808, 53.2563, 56.9077, 78.2704, 81.7938, 92.0078, 101.4314, 116.7551 seconds. These are observed result messages, not an automatic-parry success rate.
- Local health moved from the initial motion sample's 78.2255 to 26.1238. There were 81 observed negative health transitions (about 52.2297 total decrease, offset slightly by positive transitions). Damage causality, attacker and relation to any specific block are not established by totals.
- Named rigs under `Workspace.NPCs` were discovered, but none produced recorded ANIM_START events. This session still does not establish NPC attack mappings. Actor IDs are capture bindings, not unique people: the same path may be rebound under another ID.

### Five-hit combo mapping evidence

For each animation start, correlate the most recent same-actor `Character.Combo` change in the preceding **10 ms**, preserving capture event order. The following repeated associations include the local character and nearby live rigs; they strongly support this combo sequence in the observed fighting style, not all styles.

| Observed Combo | Animation ID | Same-actor close-time associations |
| --- | --- | --- |
| 1 | `114493250337466` | 38 |
| 2 | `84394230178792` | 37 |
| 3 | `95937874840716` | 32 |
| 4 | `139487903747364` | 25 |
| 5 | `131649056441662` | 20 |

Locomotion and other tracks sometimes started near the same attribute change; the table records strong repeated associations, not a claim that every simultaneous track is an attack. Existing hypothetical parry mappings remain the two IDs for combo 2 and 3; this review did not change code or enable input.

### Self-counter correlations

These rows select the latest preceding **non-self animation among the five candidate IDs** within one second of each self-counter receipt. Selection does not establish which actor or attack caused the result. Other animation/state events may coexist.

| Self-counter receipt (s) | Candidate animation | Candidate start (s) | Client receipt interval (s) |
| --- | --- | --- | --- |
| 17.9808 | `84394230178792` | 17.7775 | 0.2033 |
| 53.2563 | `95937874840716` | 53.0967 | 0.1596 |
| 56.9077 | `139487903747364` | 56.8287 | 0.0790 |
| 78.2704 | `95937874840716` | 78.1542 | 0.1162 |
| 81.7938 | `139487903747364` | 81.7209 | 0.0729 |
| 92.0078 | `139487903747364` | 91.9413 | 0.0665 |
| 101.4314 | `84394230178792` | 101.1637 | 0.2677 |
| 116.7551 | `95937874840716` | 116.6235 | 0.1316 |

**New candidate:** combo-4 animation `139487903747364`, supported by 25 close-time Combo=4 associations and three later self-counter messages. The 66.5–79 ms intervals are notification delays after a client animation observation, not server parry windows or recommended input delays. In some exchanges Defend was already pressed before that animation; do not tune an automated delay from these intervals.

### Additional mechanics / unresolved points

Outgoing Action observations included MouseButton1 (205), MouseButton2 (28), AerialM1 (15), Rush (9), ZVanish (5), OffMain (2), and Ki Blast (1), plus Update (40), UpdateRegion (9), Descend (3) and the Defend pairs. Argument semantics beyond previously established mappings remain unverified.

Incoming CooldownNT advertised Counter=0/0.5/5 seconds, Heavy=8, ZVanish=1/2/10, AerialM1=5, Weave=1, Rush=25, KiBlast=2. These differ from some tutorial observations, notably Heavy=2 and ZVanish=3. Treat durations as context-dependent client notifications; do not hardcode one earlier capture's value as universal behavior.

Verdict: **capture completed; repeated combo associations and self-counter observations found; defense timing and causality still inconclusive.** Next investigation should compare the combo-4 candidate with input edges, phase/speed, local state, distance and health in isolated exchanges. Keep record-only mode in force. Review these existing data before requesting another capture or expanding hypothetical mappings. No live automation or success-rate claim is justified by this recording.


## SP-007 / SP-T014 — first record-only live session (2026-11-17)

Evidence label: **Record Attacks session, start Unix 1794948099**, main-world place `110405289082842`, diagnostics v3 / Combat page v5. Reviewed the timestamped history and latest report, both 458,758 bytes. Raw files stay local under `AnomalyHub/universal/sparkling_auto_parry_<timestamp>_<GUID>.txt`; public notes omit player identifiers.

### Result and important configuration

- Stopped manually at **97.3245 seconds**, `STATUS=Disabled.`. The header confirms the record-only version was loaded. Totals: **0 proposed parries, 0 generated inputs, 6 self-counter messages, 43 negative health events, 0 dropped lines**. No ERROR, LOGGER_ERROR, FRAME_GAP or capture-limit events appeared.
- **Both `pvp=false` and `pve=false` throughout the session**; no SETTING events changed them. Range=8, watchRange=30, all three optional skip settings false. Therefore eligible would-be parry decisions were deliberately gated off; this was not a failure to record attacks.
- All 417 animation starts received skip entries: **24 `pve_off`**, **393 `unmapped_animation`**. Unmapped includes locomotion/idle/other tracks, so 393 must not be described as 393 missed attacks. The 24 mapped NPC cues were gated before geometry evaluation; this run cannot validate their range/facing/LOS or hypothetical input scheduling.
- 351 starts were classified NPC, 66 Player. Player/NPC ownership classification and nearby counts appeared in the report. The observer continued recording animation evidence even with both target filters disabled.
- 321 outgoing Action observations and 321 matching returns, through the namecall path; direct FireServer was advertised in coverage but no direct OUT duplicates were seen. Included 84 `Defend,true` / 84 `Defend,false` observations and matching 84 F DOWN / 84 F UP input observations. Those inputs originate outside this recorder; source is not proved to be manual by these events alone. No F_DOWN/F_UP generated-input events were recorded.
- Six local `Counter Parry!` receipts at **10.7785, 14.6842, 18.7138, 50.9944, 62.2250, 65.3536 seconds**, each with `recentProposalCount=0` and `attribution=none_record_only`. These are observed results, not successes credited to automatic parrying.

### First directly watched NPC combat in the reviewed evidence

`Workspace.Live.Thug5` was classified NPC and actually watched playing attacks. Actor IDs 7 and 17 refer to bindings with that same path at different times, not proof of two unique enemies. Other NPC names in incoming EFFECT actor entries alone, including Cell and Thug6, do not establish their attack behavior.

| Candidate combo association from SP-006 | Animation ID | Thug5 start observations across both bindings |
| --- | --- | --- |
| 1 | `114493250337466` | 14 |
| 2 | `84394230178792` | 12 |
| 3 | `95937874840716` | 12 |
| 4 | `139487903747364` | 9 |
| 5 | `131649056441662` | 8 |

This establishes NPC reuse of these animation assets. The combo-number associations came from the preceding manual combat capture; this diagnostic log does not sample the NPC's Combo attribute, so it is not an independent measurement of every NPC combo state.

Three self-counter messages followed Thug5's `95937874840716` starts by **0.1351, 0.1179 and 0.0667 seconds**. One followed `131649056441662` by **0.1588 seconds**; another followed `139487903747364` by **0.2632 seconds**. The last self-counter message had no preceding observed candidate among the five IDs within the chosen 0.75-second comparison window. These are client-time correlations with potentially multiple actors/events present, not identified server attack outcomes or input-delay recommendations. The fifth-hit association is one observation, insufficient to tune a new mapping.

### Verdict and next step

**Live recording success:** the new passive report captured NPC/player animations, external input/actions, damage, and self-counter messages with no generated inputs/errors/drops reported. **Hypothetical decision path not tested live in this session:** both target filters were OFF. This updates the earlier “untested live” status for recording, not for parry performance.

For a future NPC decision-recording test, enable **PVE (NPCs)** while leaving **RECORD ONLY** mode in place. Enable PVP only when evaluating player cues. These options select which cues can produce WOULD_PARRY proposals; neither enables real input in v5. Preserve this capture as evidence before requesting more data. Do not restore active input or guess new attack delays from the receipt intervals.


## SP-008 / SP-T015 — target-enabled decision recordings (2026-11-17)

Evidence: main-world diagnostics-v3 Record Attacks histories starting Unix **1794948410** and **1794948539**. Both report `mode=record_only`, `pvp=true`, `pve=true`, range=8, watchRange=30 and all three optional skip settings false. The latest report was 724,217 bytes. Raw histories remain local; no player identifiers are published.

The user clarified that attack mappings are shared between players and NPCs. Maintain one animation catalog; PvP/PvE selects eligible ownership, not separate attack definitions. Observed differences in speed, phase, geometry and state still matter for individual encounters.

### Capture results

| Session start | Duration / stop | Proposals | Self-counter messages | Negative health events | Generated inputs / dropped lines |
| --- | --- | --- | --- | --- | --- |
| 1794948410 | 51.360 s; stopped on focus loss | 1 | 2 | 8 | 0 / 0 |
| 1794948539 | 130.843 s; disabled manually | 5 | 2 | 30 | 0 / 0 |

The latest session has no ERROR/LOGGER_ERROR or capture-limit events. One 0.5026-second frame gap appeared at 57.9527 seconds, before its five proposals. All five proposals refer to the watched Thug5 NPC. This is live evidence that the enabled **hypothetical decision path** runs; it is not a test of generated blocking or parry success. Earlier-session focus-loss stopping is expected lifecycle behavior.

### Latest run: decision coverage

- 700 ANIM_START events; **42** used the five combo IDs catalogued in SP-006, all classified NPC. Of those, 16 used the currently mapped combo-2/3 IDs. The remaining **26 known combo starts** were still skipped as unmapped: combo 1 = 12, combo 4 = 7, combo 5 = 7.
- The 16 mapped starts yielded five proposals and 11 rejection reasons: Counter cooldown 5, attacker facing 4, self facing 1, line of sight 1. Ten cues were queued; five of those were rejected at execution for the observed cooldown. No PvP/PvE-off skips occurred.
- Overall `unmapped_animation=684` includes the 26 known-but-not-yet-mapped combo starts plus locomotion, idle and other unclassified tracks. It must not be described as 684 distinct missed attacks.
- Observation starts often showed speed=1.000, while all five accepted hypothetical decisions showed speed=1.150 with nonzero animation phase. Fixed wall-clock delays alone do not describe those changing phases. Do not infer a new delay without aligning phase/speed, input and outcome evidence.

### Five proposals versus subsequent observed damage

| Proposal time (s) | Animation / combo association | Distance (studs) | First later negative-health event (s) | Interval (s) |
| --- | --- | --- | --- | --- |
| 81.3282 | `84394230178792` / 2 | 2.500 | 81.5131 | 0.1849 |
| 93.1084 | `95937874840716` / 3 | 4.778 | 93.1921 | 0.0837 |
| 98.2531 | `95937874840716` / 3 | 2.878 | 98.4163 | 0.1632 |
| 105.7300 | `95937874840716` / 3 | 3.061 | 105.8138 | 0.0838 |
| 124.4151 | `95937874840716` / 3 | 2.519 | 124.5658 | 0.1507 |

Each first damage event was about -1.5991 health. These are chronological associations, not identification of the damaging attack or evidence that hypothetical inputs would succeed/fail. **No F press was generated.** Observed next Defend-down calls occurred 0.0336, 0.0878, 0.1293, 0.4390 and 0.8267 seconds after those proposals respectively, so the recorded external input is not equivalent to executing each proposal. The earlier session's proposal at 9.1018 seconds likewise preceded damage by about 0.4352 seconds.

The latest session had 60 observed Defend-down and 60 Defend-up outgoing calls. Its self-result messages were **Perfect Counter! at 118.4274** and **Counter Parry! at 125.6916**; neither had a proposal in the preceding one-second report window. The latter followed unmapped combo-5 animation `131649056441662` at 125.5392 by 0.1524 seconds. Around the Perfect Counter, unmapped animation `126361551838373` started at 118.0286 (0.3988 seconds earlier); its mechanic/type is unknown. Do not invent a heavy-attack classification or causal mapping from this single event.

### Line-of-sight investigation

The LOS rejection at **81.9191 seconds** named `Workspace.Thrown.Zvanishdummy.HumanoidRootPart` as the obstacle, with attacker distance 3.970 and both facing dots passing. The earlier session logged the same obstacle at **1.7510 seconds**, distance 4.027, shortly before a self-counter at 1.8384. This is repeated evidence that the raycast can hit a Z-vanish dummy rather than level geometry. The dummy's collision/query properties and intended treatment are not captured; inspect before broadly excluding folders.

A separate queued cue at 70.2810 logged an obstacle under `Workspace.Visuals.anotherHead<local-character>` but was rejected first for Counter cooldown. Preserve both facts: this is an additional possible visual obstruction, **not** an extra counted LOS rejection. Never publish the embedded character identifier.

### Verdict and next implementation priorities

**Live hypothetical-decision logging verified; actual parry performance remains untested and input generation stays disabled.** Record-only operation, both eligibility options, decision reasons and lifecycle produced usable evidence without dropped lines. No code or settings changed during this review.

Use these existing recordings before asking for another generic test. Next work should (1) inspect and handle confirmed cosmetic LOS obstructions while preserving real geometry, and (2) distinguish recognized combo-1/4/5 candidates from truly unknown animations in the shared passive catalog, without fabricating timing or claiming they are already eligible parries. The evidence also motivates phase/speed-aware analysis. Do not re-enable automatic input or present damage-after-proposal as a measured success/failure rate.


## SP-009 / SP-T016 — shared player/NPC animations and repeated gate cases (2026-11-17)

Evidence: diagnostics-v3 Record Attacks history starting Unix **1794948713**, main-world place `110405289082842`, 298,885 bytes. Stopped manually after **43.5638 seconds**. Both PvP/PvE ON; range=8, watchRange=30; optional knocked/off-screen/typing gates OFF. Raw capture remains local.

Totals: **3 hypothetical proposals, 0 generated inputs, 4 self-counter messages, 18 negative health events, 0 dropped lines**. No errors, frame-gap or limit events. Of 314 animation starts, 294 were unmapped, 11 out of range, 2 attacker-facing rejections, 3 execution-time Counter cooldown rejections, 1 LOS rejection, and 3 proposals. There were no target-type-off rejections.

### Shared animation evidence and coverage

This single session directly records the same five candidate combo IDs on both **Thug5 (NPC)** and a **player-owned character**. This strengthens the user's instruction to maintain one shared animation catalog. It does not establish equal effective speed or timing in every encounter.

| Combo association | Animation ID | NPC starts | Player starts |
| --- | --- | --- | --- |
| 1 | `114493250337466` | 8 | 7 |
| 2 | `84394230178792` | 4 | 7 |
| 3 | `95937874840716` | 3 | 6 |
| 4 | `139487903747364` | 2 | 5 |
| 5 | `131649056441662` | 2 | 2 |

All 13 mapped player cues were evaluated through the same filter path: 11 were outside 8 studs (8.926–25.647 observed), and 2 failed attacker-facing (dots -0.279 and -0.991). No proposal against a player is expected from those gates. The absence of player proposals is not evidence that PvP detection failed. The 26 starts from known combo IDs 1/4/5 are still counted as unmapped, mixed with movement/other tracks.

### Three proposals and observed outcomes

| Proposal time | Animation | Target / distance | Subsequent negative-health observation |
| --- | --- | --- | --- |
| 9.1628 s | `84394230178792` | Thug5 / 4.428 studs | -1.5991 at +0.1110 s |
| 18.7421 s | `95937874840716` | Thug5 / 3.161 studs | -1.6020 at +0.1575 s; another at +0.5058 s |
| 35.4884 s | `95937874840716` | Thug5 / 6.460 studs | No negative-health event in the next 1 s |

All three proposal tracks were running at speed=1.150 during evaluation. Neither damage after a proposal nor no damage afterward measures automatic-defense success: no input was generated, and the responsible attack/outcome is not established.

Self `Counter Parry!` messages occurred at **4.5348, 9.7366, 11.7572, 19.8294 seconds**. Only 9.7366 had a proposal within the preceding second (proposal 1, about 0.574 s earlier), and the report correctly kept `attribution=none_record_only`. A later combo-3 animation occurred just 0.0901 s before that counter, so assigning it to the earlier combo-2 proposal would be especially misleading. At 11.7572 both player and NPC candidate attacks were present nearby; do not pick an attacker from nearest timing alone. Combo-1 and combo-5 associations remain observations, not new timing measurements.

### Repeated LOS obstruction and cooldown scheduling

- At **9.6467 seconds**, combo-3 was rejected at 4.815 studs despite passing both facing checks. The obstacle was **`Workspace.Thrown.Zvanishdummy.Left Arm`**. Earlier captures hit that dummy's HumanoidRootPart. Any validated cosmetic-dummy exclusion needs to consider its model/descendants, not only one part name. The self-counter at 9.7366 followed the blocked cue by 0.0899 s; this remains correlation. Collision/query properties and the intended gameplay role of the dummy have not been captured.
- At **34.6847 seconds**, a queued combo-2 cue was removed with **0.0030 seconds** of observed Counter cooldown remaining. Cue age=0.0708 s, expiry=0.25 s; track still playing, distance=6.729, both facing dots pass, LOS clear. The current one-shot decision discards rather than defers such a cue. This is a concrete scheduling case for a bounded recheck after cooldown expiry, with all gates revalidated; it does not prove a later press would be successful. Other cooldown rejections had 0.2172 s and 2.9118 s remaining, so do not indiscriminately defer all cooldown skips beyond cue expiry.

Verdict: **passive capture and shared player/NPC evaluation observed working; repeated coverage/LOS/scheduling limitations recorded; no active-input performance result.** No implementation or settings changed during this review. Next work should use this evidence to distinguish recognized attack candidates, inspect cosmetic LOS blockers, and assess bounded cooldown rechecks. Keep recording-only mode until the user requests otherwise; another generic capture is not needed to establish these existing cases.


## SP-010 / SP-T017 — latest passive evidence and active timing test (2026-11-17)

### New user instruction and current mode

The user explicitly requested: consider opponents parrying them, build active auto parry to test timings, and continue recording. **This supersedes the previous temporary record-only instruction.** Current implementation is **Combat page v6 / diagnostics v4 // ACTIVE TIMING TEST**, off on every reload. Enable it from **Combat → Auto Parry // TIMING TEST → Enable Auto Parry**. Diagnostics start automatically and retain Save Log / Copy Log under Recorders. Disabling finalizes the history. No game session was controlled during this change.

Both chats may edit the same source and generated builds. This change is scoped to `src/SparklingAutoParry.lua` plus its exact generated-module replacement and game notes. The latest concurrent Ki-bar `Character.Ki@Max` change was retained. Recheck source/file hashes and remote HEAD before future installations; do not replace a newer entire build with a stale build.

### Latest recording evidence

Reviewed the passive report starting Unix **1794948819**, main-world place `110405289082842`, 340,074 bytes. It ended on focus loss at **55.566 seconds**, with 4 proposals, 3 self-counter messages, 15 negative health events and 0 generated inputs/dropped lines. Self-counter receipts: 9.6099 (Counter Parry), 15.8153 (Perfect Counter), 49.2749 (Counter Parry). These remain passive observations, not active parry successes.

There were **20 counter messages targeting other actors**. Five counter receipts on the nearby combat NPC's actor binding (7.9314, 14.1067, 30.4757, 34.7684 and 47.5751 seconds) were followed within roughly 4 ms by a local `Counter=0.5` notification, alongside recent outgoing local attacks. Other actors also produced counters; a sixth nearby-time half-second notice occurred around another actor's counter. The existing report lacks geometry at each recipient-counter receipt, so it cannot establish which opposing counter involved the local player. The repeated local short-cooldown association motivates a carefully labeled correlation, not a definitive “parried me” verdict.

### Shared active trial catalog and controls

| Combo association | Animation | Trial delay before F |
| --- | --- | --- |
| 1 | `114493250337466` | 0.04 s |
| 2 | `84394230178792` | 0.04 s |
| 3 | `95937874840716` | 0 s |
| 4 | `139487903747364` | 0 s |
| 5 | `131649056441662` | 0.04 s |

**These are experimental starting parameters, not measured server parry windows.** Combo 2/3 keep the existing seeds; 1/4/5 use explicitly uncalibrated trial seeds based on the observed catalog and early-input experiment. Player and NPC ownership share this one mapping. Other animations remain logged/unmapped. Track phase/speed, client scheduling lateness and geometry are recorded at execution; the scheduler does not claim to compensate perfectly for speed or network latency.

- **Timing offset (ms):** default 0, bounded -80 to +150; added to the catalog seed, with final delay floored at zero. Negative values schedule earlier, positive later. A timing edit clears queued cues rather than mixing schedules.
- **F hold (ms):** default 120, bounded 60–250. Settings and PvP/PvE preferences persist; enabled state does not. Pulse gap remains 0.3 s, cue expiry 0.25 s.
- Requires living/current target, selected PvP/PvE type, configured range, facing, clear LOS, playing/non-reused track, no manual F, observed Counter cooldown and optional knocked/off-screen gates. Text entry and game menus **always** pause/release active F; the old record-only SkipTyping preference no longer allows typing into a focused field.
- Input API failures stop the experiment and attempt F release even if key-down may have partly succeeded. Release errors are logged and retried, including stop/unload attempts. Focus loss, death/character change, teleport, unload and starting a manual combat recording stop it. (Focus loss superseded by SP-035: no longer stops it.)

### Opponents parrying the local character

Every non-self Counter Parry / Perfect Counter message now logs `OTHER_COUNTER` with recipient actor, distance, mutual facing, time since the latest observed local attack request and `attackerKnown=false`. This includes unrelated fights without counting them as the local player's failed attack.

`POSSIBLE_PARRIED` requires all of: a recent local attack request (≤0.6 s); recipient within min(configured range, 12 studs); both facing gates; and a local **Counter=0.5** notification within ±0.2 s of the other counter. MouseButton1, non-Up MouseButton2, AerialM1 and Rush request observations supply the attack evidence. Either receipt order works; a local notice is consumed at most once. Without outgoing hook coverage this specific correlation is unavailable, while incoming cooldown and other-counter logging still work.

The label remains **heuristic_only**, since Effects does not identify the attacker. On a match, held F is released and queued cues observed before the opponent counter are discarded. The code honors the actual incoming cooldown duration; it does **not** invent an extra stun time or extend the cooldown. Self-result events are separate and list recent actual attempts only as temporal candidates; they are not counted as proven input successes.

### Prior gate cases addressed

- Counter cooldowns with ≤80 ms remaining can defer a cue, but only if expiry still permits it, at most four rechecks. All target, geometry, playing-track, input and state gates are revalidated afterward. Long cooldowns/expired cues remain skips. This covers the prior 3 ms remainder case without indefinite replay.
- LOS can skip the exact **Model named `Zvanishdummy` directly under `Workspace.Thrown`**, including its limbs/root, then raycast again with a five-iteration bound. It logs ignored models. A wall behind it still blocks. Same-name models elsewhere, other characters, all of Visuals/Thrown, and arbitrary effects are **not** broadly excluded. This is a narrow trial based on repeated dummy obstructions; actual live collision behavior remains to validate. The separately observed `anotherHead<local-character>` obstruction is still not bypassed.

### Diagnostics, validation and handoff

Report header is diagnostics **v4 // ACTIVE TIMING TEST** with trial seeds, timing offset, hold duration and gates. Events include `CUE_ACCEPTED`, `F_DOWN`, `F_DOWN_FAILED`, `F_UP`, `DEFER`, `OTHER_COUNTER`, `POSSIBLE_PARRIED`, observed ACTION/INPUT, health, clocks and stop status. F_DOWN means the API returned, not that the server accepted a defense. The outgoing Action's `duringAttempt` field is contextual correlation; hooks can duplicate observations. Totals separate F attempts, self-counter messages, other-counter messages, possible-parried correlations and damage events. Latest/history filenames and limits remain unchanged.

**Validation:** 121 mocked assertions passed (47 logger/lifecycle regressions, 42 active-catalog/timing/LOS/counter tests, 26 PvP/PvE tests, 6 settings/migration checks). Changed source and public/private variants compile. No live active timing test was performed by this chat. Mock checks include walls behind dummies, near-expiry cooldown rechecks with changed geometry, all five combo IDs, both target types, recipient-counter pairing in both orders, unrelated/distant/stale/wrong-duration exclusions, key-up cleanup and timing bounds.

Next: reload the updated hub; retain offset=0 and hold=120 for the first short active trial so the starting configuration is reproducible. Enable desired target types and Auto Parry. Disable it after the exchange to finalize. Inspect actual F_DOWN/observed Defend/F_UP, damage and self/other counter timing, deferrals and release errors before changing one timing parameter at a time. Do not equate a counter-message ratio or absence of damage with confirmed success. Keep raw captures and player identifiers local.


## SP-011 / SP-T018 — first active baseline results (2026-11-17)

Reviewed diagnostics-v4 histories starting Unix **1794949315** and **1794949417**, both in main-world place `110405289082842`. Configuration: active test, PvP/PvE enabled, range 8, offset **0 ms**, hold **120 ms**, no setting changes. Raw captures remain local.

| Session | Duration / stop | F attempts | Self-counter messages | Other-counter messages | Possible-parried correlations | Negative health events |
| --- | --- | --- | --- | --- | --- | --- |
| 1794949315 | 66.404 s / focus loss | 2 | 1 | 8 | 0 | 17 |
| 1794949417 | 67.990 s / manual disable | 19 | 2 | 5 | 3 | 36 |

Both ended with `held=false`, zero dropped lines, no error/input-failure/release-failure/frame-gap events. All 21 attempted F-down calls had one corresponding observed `Defend,true`; all 21 F-up API calls succeeded and were followed by observed `Defend,false`. All actual attempts in these two sessions targeted Thug5; other player candidates were observed but did not produce attempts.

### Important input-path timing

Observed request timestamps are **client hook receipt**, not server receipt. Pair each true call using its `duringAttempt` ID, then pair the first false call within 150 ms after the corresponding successful F_UP. All 21 releases have such a false call.

| Measurement | Earlier session | Latest session |
| --- | --- | --- |
| F_DOWN log → observed `Defend,true` | 40.7–51.5 ms | **37.9–59.2 ms**, median 46.6 ms |
| F_UP log → observed `Defend,false` | 0.9–1.3 ms | 0.9–2.6 ms, median 1.1 ms |
| Observed true → false request span | 83.6–92.9 ms | **81.4–115.6 ms**, median 91.1 ms |
| Actual API-key hold from F_UP field | 123.1–143.9 ms | 123.7–160.7 ms |

This is evidence of a delayed game request after the synthetic F press and asymmetric press/release processing. A 120 ms configured key hold does not mean a 120 ms observed Defend-request span. The logs do not establish why the delay occurs or its server-side effect. Do not “correct” a delay simply by subtracting network ping.

**Logging limitation:** delayed false requests have `duringAttempt=none` because the module clears its held flag after the F-up API succeeds, before the game processes the event. That does not mean release was absent. Future request correlation should retain recent release IDs/timestamps and explicitly mark temporal matches without claiming manual/synthetic origin is proven.

### Outcomes and attribution limits

The earlier self counter at 17.9765 followed attempt 1 by about 0.3435 s. The latest session's two self counters are both ambiguous: at **17.9543**, attempts 6/7/8 fell in the preceding second; at **38.9380**, attempts 14/15 did. Counter-message count divided by attempt count is not a valid measured success rate.

In the latest session, 16 of 19 attempts were followed by a negative-health observation within 0.5 s, and 17 within 1 s. Some successive attempts share the same later damage event; these are not independent failures or proof of which attack caused damage. Attempts 8 and 19 had no negative-health event in the next second, which likewise does not prove success. The baseline is **not established as reliable** despite functioning input and logging.

Three `POSSIBLE_PARRIED` events occurred at **28.6463, 37.2402, 41.5476 seconds** against the nearby NPC binding. Recent local attack ages were about 0.044, 0.100 and 0.180 s; distances about 3.305, 6.001 and 6.000 studs; the paired local half-second cooldown notices followed the other counter by about 1.7–2.1 ms. These meet the implemented correlation rules, but attacker identity remains unconfirmed. No key was held at these three moments, so this session verifies correlation logging, not a live counter-triggered key-release path.

### Remaining obstructions / decisions

The latest session logged **three** LOS rejections, all naming `Workspace.Visuals.anotherHead<local-character>` at **12.0895, 35.4161 and 63.3236 seconds**, at distances 6.335, 2.057 and 7.216 studs with facing checks passing. The earlier session also logged two of these head-effect obstructions. The current narrow Z-vanish-dummy exclusion deliberately does not cover this separate object; no successful proposal in these sessions logged an ignored Z-vanish dummy, so its live benefit is not demonstrated here.

Other earlier-session LOS blockers were a real nearby NPC's arm/GripBrick and a map tree's Leaves. Do not fix visual obstructions by ignoring every rig or all level geometry. Inspect/identify the exact local head-effect object and scope any exclusion to it.

Latest mapped rejection reasons besides LOS: Counter cooldown 10, attacker facing 4, manual/input F already down 2, out of range 8. There were no DEFER events in either session, so short-cooldown rechecks remain mock-validated only. Existing shared combo coverage remains enabled.

Verdict: **live active input/cleanup/request observation and opponent-counter correlation logging verified; timings and actual parry reliability unproven.** No code or timing values changed during this review. Next priority is the repeatedly observed head-effect obstruction and better delayed-request/release correlation, then timing trials that account for the measured input-to-request lag. Preserve this baseline and change one timing variable at a time; do not infer reliable timing from raw counter ratios.


## SP-012 / SP-T019 — repeated active tests, early releases and live deferral (2026-11-17)

Reviewed three finalized diagnostics-v4 histories from main-world place `110405289082842`. All retain range 8, PvP/PvE enabled, offset 0 ms and hold 120 ms. These are user-run live captures; no runtime changes or game control occurred during review. Raw captures remain local.

| Session start (Unix) | Duration / stop | Automatic attempts | Self counters | Other counters | Possible parried | Negative-health events |
| --- | --- | --- | --- | --- | --- | --- |
| 1794949545 | 73.147 s / disabled | 10 | 4 | 6 | 3 | 33 |
| 1794949665 | 4.601 s / focus loss | 0 | 0 | 0 | 0 | 24 |
| 1794949677 | 65.343 s / disabled | 7 | 2 | 19 | 2 | 26 |

All reports end held=false with zero dropped lines. All 17 automatic key-up API calls report success; each press has an observed Defend=true and a subsequent Defend=false. No error/input-failure/frame-gap events were present. All attempts target the same nearby NPC binding, not players. Six of ten earlier attempts and five of seven latest attempts precede negative-health observations by at most 0.5 s. This is temporal association, not an independent failure rate or attacker attribution.

### Input interleaving changes the interpretation

Two combo-3 (`95937874840716`) trials had close self-counter receipts: earlier attempt 1 at 11.4695 s followed by Counter Parry at 11.5617 s (~92 ms), and latest attempt 5 at 38.8015 s followed by Counter Parry at 38.9104 s (~109 ms). Both tracks ran at speed 1.15; distances were 2.828 and 4.703 studs. These are repeatable temporal candidates, not proven automatic successes: multiple F/Defend cycles outside automatic attempts occur immediately beforehand.

In both cases, an INPUT F-up / Defend=false occurs **before** the module's scheduled F_UP: 11.5700 versus 11.6148 s, and 38.9173 versus 38.9665 s. The logger still considers F held until its deadline. The input origin remains unknown; do not assert the user manually released it or that the game forcibly released it without more evidence. This demonstrates interleaving between observed input/request state and the automation's internal held state.

The previous review's shortcut of pairing only false requests after F_UP is insufficient for these new cases. Pair the next observed Defend request after a candidate true, require it to be false, and never cross an intervening true request. Otherwise the latest attempt 5 would incorrectly acquire the false request from a separate F cycle at 39.1042 s, producing a fabricated 249.8 ms defense span. Its actual adjacent true/false request span is 62.9 ms. Earlier attempt 1's span is 56.3 ms. These remain client request intervals, not server windows or proven input ownership.

Across the 10 earlier attempts, press-to-Defend delay is 44.2–61.4 ms; API holds 131.0–185.2 ms; adjacent true/false spans 56.3–127.8 ms. Across the latest seven, delay is 47.1–69.8 ms; API holds 120.9–165.2 ms; adjacent request spans 51.6–114.3 ms. Other than the two early-release cases, false requests follow scheduled F_UP by 0.9–4.8 ms. A configured 120 ms hold still cannot be treated as a measured 120 ms defense window.

The other earlier self counters were at 14.6305 and 28.8724 s (no automatic attempt in the preceding second), and 57.7432 s (attempt 9 about 626 ms earlier, with damage in between). The latest other self counter at 10.1212 s has no automatic candidate within one second. Do not turn total counter counts into an automatic success rate.

### First observed cooldown deferral

Earlier cue 169, combo 2, deferred at 41.1465 s with 60.8 ms Counter cooldown remaining. It passed revalidation and pressed F at 41.2568 s, cue age 153.7 ms and 49.5 ms late relative to the rescheduled deadline. Negative health arrived at 41.3036 s; observed Defend=true followed at 41.3076 s, about 4 ms later. **The bounded retry mechanism executed live; this instance did not demonstrate timely defense.** Client receipt order is not server execution order. Account for scheduling lateness and input-to-request delay before considering a delayed cue useful.

### Remaining coverage and next handoff

The earlier session has two LOS skips: the local `Workspace.Visuals.anotherHead<local-character>` effect and an unidentified `Workspace.Thrown.Part`. The latest has four: three local head-effect blockers and one Thrown.Part. Do not assume every generic Part is cosmetic or ignore whole folders. The head-effect obstruction remains a repeated, separately scoped investigation.

Possible-parried matches occur earlier at 4.4457, 27.2290 and 40.7071 s, and latest at 8.2369 and 49.0203 s. They remain recipient/attack/geometry/short-cooldown correlations, not proven attacker identities; none overlaps an automatically held F in these reports.

**Current verdict:** input generation, cleanup, diagnostics and one cooldown retry are observed live; reliable parry timing remains unproven. Before another timing adjustment, improve diagnostic correlation to capture early observed releases and intervening F cycles, inspect the exact head-effect obstacle, and use an exchange without extra F input to distinguish automatic outcomes. Preserve offset 0 / hold 120 as the baseline. No runtime code or settings changed in this review; the live session remains under the user's control.


## SP-013 / SP-T020 — mostly isolated automatic-input baseline (2026-11-17)

**Evidence:** finalized diagnostics-v4 session starting Unix 1794949846, main-world place `110405289082842`, 62.601 seconds, manually disabled. Same baseline: range 8, both target filters enabled, offset 0 ms, hold 120 ms. Totals: 10 automatic attempts, 1 self counter, 12 other counters, 0 possible-parried matches, 51 negative-health observations; held=false, zero dropped lines, no input/error/frame-gap failures. All attempted targets were the nearby NPC binding. This review changes documentation only.

**Input isolation:** all 10 automatic presses have their corresponding observed F-down, Defend=true, successful F-up API result, observed F-up and Defend=false. No additional F cycles are present before attempt 10 completes. Three extra F/Defend cycles occur at 59.2983–59.7923 s, after its release; their origin is unknown. Thus the earlier self-counter exchange is isolated from extra observed F input, while the full session is not entirely automatic-only. No outgoing local attack requests were observed, consistent with zero possible-parried matches; the 12 other counters cannot be assigned to attacks by the local character.

### Cleaner positive candidate: combo 2

| Event | Client receipt time |
| --- | --- |
| NPC combo-2 animation `84394230178792` starts | 54.2107 s |
| Attempt 9 presses F | 54.2613 s |
| Observed Defend=true | 54.3179 s |
| F-up API succeeds / observed Defend=false | 54.3831 / 54.3852 s |
| Self Counter Parry message | 54.5123 s |
| Local Counter=0.5 notification / CounterImpact effect | 54.5145 / 54.5149 s |

At the attempt, distance=4.253 studs, attacker facing dot=0.908, self dot=0.851, track speed=1.15, phase=0.115 s, LOS clear. The self counter follows the automatic press by about 251 ms and the observed defense request by 194 ms; no other automatic attempt or extra F cycle is nearby. The next negative-health observation is at 58.0790 s, 3.818 seconds after the press. **This is the strongest isolated automatic-defense candidate in this review series**, with corroborating self effects and no immediate damage. It is still client-side temporal evidence, not server-confirmed attacker identity or a calibrated success window. Other nearby actors were active. The short 67.3 ms true/false request span and counter receipt after release must not be interpreted as an exact server window.

### Reliability and repeated limits

Eight of the ten attempts were followed by negative-health observations within 0.5 s. Attempt 6's next damage was about 0.693 s later; attempt 9's was 3.818 s later. Do not label the session a confirmed 10% success rate or count each damage event as a distinct failed attempt. The earlier combo-3 positive candidates did not reproduce in this cleaner baseline: its three attempts each precede damage by approximately 113–180 ms.

Press-to-Defend delays remain 42.3–60.5 ms. Actual key holds are 121.9–171.5 ms despite the 120 ms setting; adjacent true/false request spans are 67.3–121.3 ms. Releases follow the F-up API by 0.8–4.6 ms, with no early-release cases in the automatic sequences.

Eight Counter=5 notices and one Counter=0.5 notice were observed. The first seven long notices occur roughly 0.48–0.57 s after attempts 1–7; the eighth follows attempt 10 amid the later extra F inputs. This is a repeatable timing association, not proof that five seconds means failure or that short duration always means success. The scheduler rejected 23 cues for observed Counter cooldown, with no DEFER events. Preserve actual cooldown gating rather than bypassing it to increase attempts.

All three LOS skips again name the local `Workspace.Visuals.anotherHead<local-character>` effect, at 18.1444, 33.9440 and 53.7075 s. The middle skip also had 2.588 s cooldown remaining, so removing that obstacle alone would not have made the cue eligible. Other skips: out-of-range 13, self-facing 1, unmapped animation 198.

**Handoff:** the no-extra-F exchange provides a better automatic-result candidate than the mixed-input tests, but baseline timing remains unreliable. Keep this evidence alongside SP-T019's interleaving cases. Next implementation priorities remain precise input/request correlation and inspection of the exact head-effect LOS blocker, followed by one-variable timing trials. More unchanged generic recordings are not required to establish these repeated issues. No script, live-session state or timing setting was changed during this review.


## SP-014 / SP-T021 — request correlation and scoped head-effect LOS fix (2026-11-17)

**User authorization:** proceed with the input-correlation and repeated head-effect obstruction fixes after SP-T020. Implemented in `src/SparklingAutoParry.lua`, **Combat page v7 / diagnostics v5**. The public artifact and installed private artifact receive only the exact module replacement from their current contents, preserving concurrent ESP/other changes. No game-session control or timing adjustment is part of this change.

### Request/input correlation

Recent API pulses are retained in a bounded 16-entry list after key-up. F input events now include `inputSeq`, a temporal candidate `attempt`, and an association: `api_down_window`, `api_up_window`, `early_up_while_api_held`, `ambiguous_api_window`, or `unmatched`. Receipt windows are 200 ms; an early-up candidate requires the same currently held pulse within 0.5 s and no preceding API release request. Multiple eligible pulses stay ambiguous. These are diagnostic windows, not game timing windows. API release still occurs after an early observed release, preserving cleanup.

New **DEFEND_REQUEST** events describe observed outgoing boolean Defend requests. OUT/RETURN observations for one call do not advance the pairing state twice; duplicate OUT IDs are suppressed within a bounded 64-call cache. Each input receipt can be consumed only once. A true begins a request cycle; a later true closes/replaces the prior cycle. A false consumes that current cycle and cannot be reused. A pair has at most a one-second span and cannot cross an intervening F-down. `cycleAttempt` additionally requires both receipts to correlate with the same API pulse. Unmatched, stale, conflicting and ambiguous cases retain unknown attribution rather than borrowing the nearest attempt.

Fields include `inputAttempt`, `cycle`, `requestSpan`, `pairValid`, `cycleAttempt`, `releaseState`, `apiDownAge` or `apiUpAge`, and `interveningDown`. `releaseState=before_api_up_request` exposes SP-T019's early release; delayed ordinary releases retain the candidate attempt through `after_api_up_request`. `pairValid` describes an adjacent request interval, **not a successful parry or proven synthetic input**. The existing raw ACTION `duringAttempt` field remains contextual; use the new correlation events rather than treating a post-release `duringAttempt=none` as a missing release.

SELF_COUNTER now also lists `inputConflicts` for recent attempts with a later observed F-down. Existing counter candidate counts remain temporal. Unknown request/input origin and server acceptance are explicit. Distinct hook paths may still duplicate a request under different call IDs; those do not justify inventing a successful cycle. Hooks disconnect at stop, so asynchronous post-stop request receipts can be unavailable; successful API release and missing post-stop observation remain distinct.

Reference: reused Beyond Nen's separation of API input, input receipt, request receipt and conflicting subsequent inputs (`BeyondNenAutoParry.lua`, `UnmatchedDownAfter` / result-attribution checks). No Beyond Nen remote semantics, timings, weapon IDs or server confirmation assumptions were transferred. Hunter X Enter's UI/recorder conventions remain unchanged.

### Narrow local head-effect exclusion

LOS now reraycasts past a hit only when it is the exact **BasePart named `anotherHead` + current local Character.Name, directly under Workspace.Visuals**. The existing exact Zvanishdummy Model-under-Thrown exclusion remains. Other-player names, stale character names, same-name objects elsewhere, model wrappers, generic Thrown.Part, real rigs and map geometry remain blockers. A wall behind an excluded object still blocks, and the existing five-ray bound remains.

This is a scoped trial based on the repeated ray-hit paths in SP-T018–SP-T020. Actual live properties/gameplay purpose are not proven solely by the name. New **LOS_OBJECT** events capture the hit object's class, CanCollide, CanQuery, Anchored, Size, matched exclusion and excluded root, once per observed object with a 32-object session cap. Cue details retain `ignoredCosmetics`, including when a wall behind an effect blocks the ray. These observations allow the next capture to verify the exact live object instead of expanding exclusions by guesswork.

### Validation and handoff

**155 mocked assertions passed:** 47 logger/lifecycle regressions, 42 active trial/catalog/LOS/deferral/opponent-counter checks, 26 player/NPC checks, 6 settings/reload checks, and 34 new asynchronous correlation/early-release/interleaving/scoped-LOS checks. New cases include delayed requests after API release, early release followed by a separate F cycle, duplicate OUT/RETURN, intervening down without a new request, repeated true, stale/orphan receipts, overlapping release windows, new-session reset, exact head-part matching, walls behind it, wrong names/parents/classes, changed character names and bounded LOS observations. Updated source and both staged public/private variants compile. These are simulated checks, **not live validation of v5**.

Reload the installed hub to use v5, then enable Auto Parry (still off on reload). Preserve offset=0 / hold=120 for the first comparison; diagnostics continue automatically. Inspect new DEFEND_REQUEST/INPUT correlation and LOS_OBJECT plus ignoredCosmetics, then actual damage/counter observations. Do not adjust multiple timing variables or claim reliable defense merely because the obstruction no longer blocks. Source seeds, range/cooldown gates, hold settings, target filters and existing stop/release behavior are retained. The user controls live testing.


## SP-015 / SP-T022 — v5 live correlation and head-effect validation (2026-11-17)

**Evidence:** finalized diagnostics-v5 session starting Unix 1794950578, main-world place `110405289082842`, manually disabled at 71.881 s. Unchanged range 8, PvP/PvE enabled, offset 0 ms, hold 120 ms. Totals: 25 automatic attempts, 23 self-counter messages, no other-counter or possible-parried messages, 25 negative-health observations. All attempted targets were the nearby Thug5 NPC binding. Report ends held=false with zero dropped lines and no input/release/logger/error/frame-gap failures. No runtime changes during this review.

### Fixes verified in this capture

**Asynchronous request correlation:** all 25 automatic presses and releases have successful API calls and corresponding observed Defend true/false requests. All 25 release records carry `pairValid=true`, the correct `cycleAttempt`, `releaseState=after_api_up_request` and no intervening down. There is one additional observed F cycle at 23.5996–24.0815 s, outside the automatic attempts. v5 correctly assigns it `attempt=none` / `cycleAttempt=none`; the self counter at 23.7975 s likewise has zero automatic candidates. The report contains 52 DEFEND_REQUEST events: 50 automatic-correlated plus the two unmatched requests. Early-release and overlapping-window branches did not occur here and remain mock-validated only.

**Local head-effect exclusion:** eight encountered objects match the exact local head-effect path; all are `Part`, CanCollide=false, CanQuery=true, Anchored=false, Size=(2,1,1). The ray can hit these non-collidable but queryable objects. All eight affected cues passed the narrow exclusion at observation and later generated automatic attempts (10, 11, 14, 15, 18, 19, 20, 21). Seven still required the exclusion at execution; attempt 14 had clear geometry by then. All eight were followed by a self-counter message. There were **zero line-of-sight skips** in this session. This verifies the intended live exclusion, not the broader claim that every Visuals object is safe to ignore; wall-behind-effect behavior remains mock-tested.

### Outcomes by observed combo

| Combo | Automatic attempts | Self-counter messages with that sole recent candidate |
| --- | --- | --- |
| 1 / `114493250337466` | 19 | 18 |
| 2 / `84394230178792` | 3 | 3 |
| 3 / `95937874840716` | 2 | 0 |
| 4 / `139487903747364` | 0 | Not tested |
| 5 / `131649056441662` | 1 | 1 |

Thus **22 distinct automatic attempts** had a self-counter message with one recent candidate and `inputConflicts=[]`; the 23rd message belongs to the unmatched F cycle. The 22 automatic-associated receipts follow their candidate presses by approximately 132–201 ms. These are strong repeated client-side correlations, not a general 88% confirmed success rate or server-proven attacker identity. All were NPC trials; PvP performance is not established.

Only two automatic attempts precede damage within 0.5 s, both combo 3: attempt 23 at 58.1138 s precedes damage by 115.2 ms, and attempt 25 at 67.7136 s by 114.5 ms. Both tracks were speed=1.15, phase=0.0192 s at execution, distance about 4.668 and 3.833 studs, with facing/LOS passing. Damage arrived while the API key was still held, and no self counter followed. This repeats the weak combo-3 result from the isolated v4 baseline; it does not identify whether its seed is early/late or the server's acceptance conditions. Attempt 13 (combo 1) also had no self-counter message, but its next damage was 1.614 s later, so absence alone is not a proven failed parry. Total damage includes hits between attempts and gated exchanges.

### Conditions changed despite unchanged trial settings

F_DOWN-log to Defend-true receipt shrank to **14.5–18.9 ms** (median 16.2), versus 42.3–60.5 ms in SP-T020. Actual API holds were 132.0–135.9 ms; adjacent true/false request spans were 115.4–120.5 ms (median 118.0). Release request receipt lag was 0.6–3.5 ms. Displayed ping samples were approximately 99.9–113.6 ms, lower than the preceding baseline's cited 144–153 ms samples. The changed input/request scheduling and network observations are confounders: the correlation code itself does not shorten the injected hold or change attack seeds. Do not attribute the entire outcome improvement to the head-effect fix or assume these conditions persist.

Other rejections: observed Counter cooldown 8, out of range 7, self-facing 7, stopped track 1, unmapped animation 131. No DEFER event, other-counter pairing, or early observed release was exercised.

**Current handoff:** v5 ordinary delayed-release correlation, unmatched-cycle separation and the scoped head-effect exclusion are verified live in this capture. Retain the current timings as the successful comparison baseline for combos 1/2/5. The next timing investigation should isolate combo 3, preserving the other seeds rather than moving the global offset based on this one run. Combo 4 and broader player/weapon coverage remain unvalidated. No additional generic log is needed to establish the two fixes; do not silently promote all automation to reliable based on this favorable session.


## SP-016 / SP-T023 — first-hit disabled for later-hit trials (2026-11-17)

**User instruction:** review the new recording and remove first-hit parry to test the others. Current **Combat page v8 / diagnostics v5** is explicitly **Auto Parry // HITS 2-5 TEST**. This supersedes the earlier instruction to retain all five active seeds for the next comparison.

### Latest pre-change recording

Reviewed finalized diagnostics-v5 session starting Unix 1794952495, main-world place `110405289082842`. It stopped on focus loss at 80.948 s, held=false, no dropped lines or input/release/logger/error/frame-gap failures. Baseline remained range 8, offset 0 ms, hold 120 ms, PvP/PvE enabled. Totals: 29 automatic attempts, 19 self-counter messages, 8 other-counter messages, 3 possible-parried correlations and 30 negative-health observations. All 29 API releases succeeded with observed corresponding Defend false requests.

| Combo | Attempts | Sole recent automatic candidate for self counter | Damage within 0.5 s after attempt |
| --- | --- | --- | --- |
| 1 | 12 | 11 | 1 |
| 2 | 6 | 4 | 1 |
| 3 | 5 | 2 | 3 |
| 4 | 4 | 0 | 4 |
| 5 | 2 | 0 | 0 |

The 17 sole-candidate messages had no recorded input conflicts. Two additional self-counter messages were ambiguous: at 51.1438 s, attempts 21/22 were recent with an intervening input flagged for 21; at 61.4087 s, attempts 25/26 were recent with an intervening input flagged for 25. Do not credit either ambiguous message to a particular combo or turn this table into confirmed success rates. No damage after combo 5 does not itself establish a successful parry. The three possible-parried events remain heuristic. A short-cooldown defer occurred at 60.2114 s with 46.8 ms remaining; this review does not claim its eventual outcome as a distinct success.

Input-to-request delay remained low: 12.9–19.6 ms, median 16.4; actual holds 120.4–135.9 ms; adjacent Defend request spans 105.2–120.8 ms. No LOS rejection appeared. The new evidence reinforces repeated first-hit counter correlations while exposing later-hit variability, motivating the user's requested experiment rather than global timing changes.

### Implementation and validation

Animation **114493250337466 (combo 1)** is removed from the active timing catalog. Its observed animation start/stop remains recorded; the observer explicitly logs **SKIP reason=first_hit_disabled stage=observe** before scheduling. It never queues a defense attempt, consumes a pulse, or generates F input on its own. This applies to both NPC and player ownership because both use the same observer. It is deliberately disabled, not mislabeled as an unknown animation.

Hits 2–5 keep their existing seeds: combo 2=0.04 s, 3=0 s, 4=0 s, 5=0.04 s. Range, facing/LOS, cooldowns, input cleanup, offset/hold controls, target filters and continuous diagnostics are retained. Header Config now states `trialSeeds=combo1:disabled,...`; the Combat section and initial status explicitly identify the experiment. There is no new persisted toggle; restore hit 1 only on a later request. Diagnostics remain schema v5; implementation is Combat page v8.

**157 mocked assertions passed:** 47 logger/lifecycle, 44 active-feature/catalog checks (including first-hit animation logged/skipped with no generated input and all four remaining hits still active), 26 PvP/PvE, 6 settings/reload, 34 correlation/LOS. Source and both staged builds compile. This change is not live-validated yet. Source and exact module replacements preserve the concurrent Visuals tab / Profile Scan change from commit 2bac7ed and the latest installed private contents.

**Next check:** reload the installed hub, enable the HITS 2-5 TEST, and retain offset=0 / hold=120. Verify first-hit skip events and no combo-1 F_DOWN; then inspect subsequent combo attempts, request/counter receipts and cooldowns. First-hit damage is expected to become visible when its automatic defense is intentionally disabled; distinguish it from a failed later-hit attempt. Actual cooldown or other gates may still prevent a later hit from being attempted. Keep the user's live session under their control and record later findings here.


## SP-017 / SP-T024 — later-hit trial verified live (2026-11-17)

Reviewed finalized session starting Unix 1794952746, main-world place 110405289082842, Combat page v8 / diagnostics v5. Manually disabled at 72.016 s. Config confirms combo1:disabled, offset=0 ms, hold=120 ms, range=8 and both target filters enabled. All actual attempts target the Thug5 NPC binding. No runtime code/settings changed during this review.

**First-hit exclusion verified live:** 17 first-hit animation cues logged first_hit_disabled; zero first-hit F_DOWN attempts. Later hits still generated 11 attempts. All 11 successful API releases have corresponding observed Defend=false with correct cycleAttempt and no intervening down. No error, input/release failure, dropped line or frame-gap events; final held=false. One additional F cycle at 38.1999–38.6155 s is correctly unmatched, with no assigned automatic attempt.

| Combo | Attempts | Sole-candidate self counters | Negative health within 0.5 s |
| --- | --- | --- | --- |
| 1 | 0 (17 deliberately skipped cues) | Not applicable | Not an automatic-defense trial |
| 2 | 6 | 5 | 0 |
| 3 | 4 | 1 | 2 |
| 4 | 1 | 0 | 1 |
| 5 | 0 | Not tested | Not tested |

Seven total self-counter messages: six have a sole recent automatic candidate with no input conflict, while the message at 12.6651 s has two candidates, attempts 2 (combo 2, 649 ms earlier) and 3 (combo 3, 148 ms earlier). It flags inputConflicts=[2] and remains ambiguous. Do not allocate it to either combo or derive a confirmed success rate from the table. Combo 3's sole-candidate counter at 7.9995 s follows attempt 1 by 133 ms. Its other no-counter attempts are mixed: attempt 8 at 33.1032 s and attempt 11 at 52.6479 s precede damage by 477.5 and 130.8 ms respectively.

The only combo-4 attempt, 9 at 33.4969 s, precedes damage by 83.8 ms while its key is still held. This is the **same negative-health event** at 33.5807 s counted after combo-3 attempt 8; do not count it as two independently proven failed attacks. No self counter accompanies that exchange. Overall there are 27 negative-health observations, including exchanges where the first hit is intentionally not parried.

Combo 5 has no attempt: two cues were blocked by observed Counter cooldown (4.4183 s remaining at 34.1327 s, 1.1689 s at 41.3830 s); a third was blocked by real map geometry at 62.3629 s. The other LOS skip, combo 4 at 61.5981 s, also hit Workspace.Map.Model.MeshPart. Both observed parts are CanCollide=true, CanQuery=true, Anchored=true, approximately size (9.733,10.247,0.471). The narrow cosmetic exclusion correctly leaves these map blockers in place. No head-effect exclusion was exercised in this report.

Six other-counter messages produced five possible-parried correlations, still heuristic. No automatic key was held at those five timestamps. No DEFER event occurred. Other skip totals: cooldown 6, attacker facing 2, range 2, self facing 1, unmapped animation 355. Many idle/movement animation observations are unmapped; do not equate that total with 355 missed attacks.

Input-to-Defend delay remains 15.7–22.7 ms (median 16.9); API holds 131.5–140.4 ms; adjacent request spans 110.0–122.7 ms. Ordinary request correlation remains functional without adjusting timing.

**Handoff:** retain first-hit-disabled mode as requested. Hit 2 has repeated strong counter correlations, hit 3 remains mixed, hit 4 has another damage-associated trial, and hit 5 was not eligible here. Next investigation should isolate hits 3/4 and compare their animation phases/request timing, preserving hit 2's seed. To get hit-5 evidence, obtain an eligible exchange with cooldown clear and unobstructed geometry; do not bypass actual cooldown or wall checks. Current evidence does not determine an exact replacement timing. The mode itself is live-validated; broader parry reliability remains unproven.


## SP-018 / SP-T025 — isolated hit-3 / hit-4 selector (2026-11-17)

**Authorization and purpose:** user said to continue after the later-hit review and focus on hits 3/4 while preserving hit 2's timing. Current **Combat page v9 / diagnostics v5** provides **Auto Parry // SINGLE HIT TEST** with a **Test hit: 3 (click to switch)** button. It alternates between hit 3 and hit 4; default is hit 3. This replaces simultaneous hits 2-5 for the next diagnostic trial. The selected hit persists; active auto parry still starts OFF on reload.

SP-T023–SP-T024 show mixed hit-3 results and damage associated with hit 4, but do not establish a defensible replacement delay. In SP-T024, one damage event followed both a hit-3 and hit-4 attempt; one self counter likewise had hit-2/hit-3 candidates. Isolating the selected animation prevents automatic attempts for other combos from consuming the same input/cooldown window or creating these automatic-attempt overlaps. It does not eliminate other actors, manual input, or game-origin cooldowns.

### Behavior

- Hit 3 targets animation 95937874840716; hit 4 targets 139487903747364. Only the selected ID can queue an automatic defense. Both retain seed delay 0 s; timing offset and hold controls remain available and unchanged.
- First-hit animation stays recorded with first_hit_disabled. Other known nonselected hits log trial_hit_filtered with testHit; unknown animations retain unmapped_animation. Hit 2's 0.04 s seed and hit 5's 0.04 s seed remain in source but are filtered out of this isolated trial. They cannot generate F input in this mode. Ownership and existing PvP/PvE filters still apply to the selected hit.
- Switching the selector stops/saves an active session, clears watched tracks and pending cues, releases held F, disconnects the observation hook, then changes the saved selection. It never automatically enables the next run. If key release fails, the selection remains unchanged until release succeeds.
- Each session snapshots TestHit. Header Config, START and CUE_ACCEPTED identify the selection; copying a stopped report after switching still reports the original hit. Live status identifies HIT 3 TEST or HIT 4 TEST. Old settings without TestHit default to 3; only 4 selects the alternative, with invalid values falling back to 3.
- The shared section expands vertically to accommodate the selector above the existing timing boxes. Recorder controls, theme, LOS, cooldown gates and cleanup remain unchanged. No live game session is controlled during deployment.

### Validation and next check

**187 mocked assertions passed:** 47 logger/lifecycle, 41 active feature/deferral/counter checks, 26 PvP/PvE, 7 settings/reload, 1 invalid-hit fallback, 34 asynchronous correlation/LOS, and 31 selector-specific cases. Selector cases cover each known combo under both selections, first-hit logging without input, selected-hit persistence, session headers after switching, key-up on switch, stopped hooks, disabled next run, queued-cue removal, release failure/recovery and player-owned targets. Updated source and staged public/private builds compile. The selector and resulting single-hit trials are **not yet live-validated**.

Next: reload, leave **Test hit: 3**, offset=0 and hold=120, then enable and finish a short exchange. For hit 4, click the selector and enable again; switching finalizes the earlier run. Inspect the histories separately. Required acceptance: no F_DOWN animation outside the selected hit, consistent testHit header, expected filtered skips, paired release requests and cleanup. Then compare selected-hit phase/request/counter/damage observations before proposing a delay adjustment. Nonselected-hit damage is expected and must not be automatically attributed to the selected attempt. Keep the previously favorable hit-2 seed intact; do not tune a global delay from mixed-combo totals.


## SP-019 / SP-T026 — isolated hit-3 test against Shadow (2026-11-17)

**Evidence:** finalized diagnostics-v5 session starting Unix 1794954744, main-world place 110405289082842, Combat page v9. Header testHit=3, offset=0 ms, hold=120 ms, range=8, both target filters enabled. Manually disabled at 33.467 s; held=false, zero dropped lines, no logger/input/release/error/frame-gap failures. Totals: 3 automatic attempts, 1 self-counter message, 0 negative-health observations, no other-counter or possible-parried events. Runtime code and settings unchanged during this review.

**Important context change:** all three attempts targeted the NPC binding named **Shadow**, not the Thug5 binding from prior tests. The capture does not establish this target's damage rules or confirm that every observed animation was an attack that would have damaged the local character. Do not compare zero observed damage directly with the earlier Thug5 failure counts or count all three as confirmed successful parries.

**Selected-hit filtering verified live:** all F_DOWN events use hit-3 animation 95937874840716. Five first-hit cues log first_hit_disabled; 13 other known-hit cues log trial_hit_filtered. There are zero automatic inputs for any nonselected hit. Four selected cues queued: three executed and one was rejected for Counter cooldown. Another selected cue failed self-facing. Ordinary input correlation and release cleanup worked for all three executed attempts.

| Attempt | F press time | Distance | Track phase / speed | Subsequent self-counter receipt |
| --- | --- | --- | --- | --- |
| 1 | 4.5005 s | 4.485 studs | 0.0192 s / 1.15 | None |
| 2 | 23.7989 s | 4.968 studs | 0.0192 s / 1.15 | 23.9597 s, approximately 161 ms after press |
| 3 | 26.7390 s | 3.812 studs | 0.0192 s / 1.15 | None |

Attempt 2 is the only recent automatic candidate for that counter and has inputConflicts=[]. Its Defend=true receipt is at 23.8179 s, false at 23.9337 s, then the self counter at 23.9597 s. This is a useful isolated temporal correlation, not a server-confirmed success window. Attempts 1/3 have neither negative health nor a counter; their result remains inconclusive.

All three release records have pairValid=true, correct cycleAttempt, after_api_up_request and no intervening down. Input-to-request delay is 19.0–23.6 ms; API holds 134.0–152.3 ms; adjacent true/false request spans 115.8–133.8 ms. Three additional F cycles at 28.0395–28.3880, 30.8840–31.0768 and 31.1147–31.2698 s correctly remain unmatched. They occur after the three automatic attempts and do not create additional automatic successes. Their source is unknown. At 31.2896 s, a later hit-3 cue was skipped with 0.9033 s observed Counter cooldown remaining; do not bypass that gate.

**Handoff:** hit-3-only filtering, report selection, ordinary request correlation and cleanup are now live-validated. The hit-4 selection and switch-between-runs behavior still need live validation; their mocked tests remain separate evidence. Next select Test hit: 4, then enable a separate run at offset=0 / hold=120. Record the target context: Shadow and Thug5 cannot be silently pooled as equivalent damage trials. The current small hit-3 sample does not establish a replacement delay, so preserve the seeds until a discriminating comparison is available.


## SP-020 / SP-T027 — short hit-3 follow-up against Thug5 (2026-11-17)

Reviewed finalized session starting Unix 1794954940, main-world place 110405289082842, 14.317 s, manually disabled. **Header still reads testHit=3, not 4.** Offset=0 ms, hold=120 ms, range=8 and both target filters remain unchanged. This time the attempted target is Thug5; the preceding SP-T026 target was Shadow. No code/settings changed during review.

One automatic hit-3 attempt occurred at 7.0358 s: animation 95937874840716, phase=0.0240 s, speed=1.15, distance=3.793 studs, facing/LOS passing. Defend=true was observed at 7.0627 s, negative health (-1.5962) at 7.1410 s, then Defend=false at 7.1725 s. Damage followed the press by 105.2 ms while F was still held; there was no self-counter message. The release API succeeded, request correlation identified cycleAttempt=1, and final held=false. This is another damage-associated hit-3 attempt under the unchanged baseline, not enough to identify an exact timing correction or prove the damage source.

A second selected cue at 11.4580 s was skipped with 1.0497 s Counter cooldown remaining. Other gates recorded 3 first_hit_disabled and 7 trial_hit_filtered skips; there were no nonselected automatic inputs. The session contains 13 negative-health events overall, including hits outside the sole attempt. No dropped lines, input/release/logger/errors or frame-gap events. No extra F cycle appears in this capture.

**Handoff:** hit-3 filtering and request cleanup remain verified, and the Thug5 result differs from the preceding zero-damage Shadow session. The isolated hit-4 test remains outstanding; do not label this recording as hit-4 evidence. Use the Combat selector to change its visible label from Test hit: 3 to Test hit: 4, then enable a new run at offset=0 / hold=120. A successful switch should produce testHit=4 in the next report and only animation 139487903747364 in its F_DOWN events.


## SP-021 / SP-T028 — repeated hit-3 test and clearer hit buttons (2026-11-17)

Latest finalized session starts Unix 1794955088, main-world place 110405289082842, diagnostics v5, 19.846 s, manually disabled. Header still testHit=3, offset=0 / hold=120. All three automatic attempts target Thug5 using hit-3 animation 95937874840716. Press times 0.5436, 11.8570 and 17.5915 s precede negative-health observations by 95.0, 105.0 and 126.3 ms respectively; no self-counter message occurred. All three releases succeed and correlate correctly. Total negative-health observations=23; six other-counter messages are not local successes. Final held=false, no dropped lines or input/logger/error/frame-gap failures. First-hit-disabled skips=4, other-known-hit filtered skips=11. This is further hit-3 baseline evidence, **not hit-4 evidence**.

The user clarified that they could not find the hit selector. Installed/source inspection found the v9 toggle below target counts, connected through the normal shared button helper; the saved settings had no TestHit entry. This does not prove a click-handler failure. The concrete usability issue is the user's inability to locate the control; the layout was changed accordingly.

**Combat page v10 / diagnostics v5** replaces the buried click-to-toggle control with two explicit top-row buttons, **Hit 3** and **Hit 4**, immediately above Enable Auto Parry. The selected button says **Hit 3: SELECTED** or **Hit 4: SELECTED** and uses the shared accent color. Controls below it are shifted with non-overlapping fixed rows; section height expanded to 368 body pixels. Default remains hit 3 unless a valid saved hit-4 selection exists. Timing seeds, offset and hold settings are unchanged.

Selecting a different hit retains v9's stop/save, queue clear, key release and disabled-next-run behavior. Failed release prevents the change. Clicking the already-selected hit is a no-op and does not stop a running test. Both choices directly select their named hit; neither requires inferring the current toggle direction. Existing session snapshots retain their original testHit after a change.

**Validation:** 188 mocked assertions passed (47 logger/lifecycle, 41 active-feature, 26 ownership, 7 settings/reload, 1 invalid-setting fallback, 34 correlation/LOS, 32 selection/lifecycle checks). Updated source and both staged public/private variants compile. The new layout was source-inspected, not visually verified in the live game. Button behavior is mock-validated; live hit-4 testing remains outstanding.

**Next action:** reload the hub, open Combat, click **Hit 4** at the top of SINGLE HIT TEST and confirm it reads **Hit 4: SELECTED**, then enable Auto Parry. Preserve offset=0 / hold=120. The resulting report must say testHit=4 and only emit automatic input for animation 139487903747364. Do not keep treating unchanged hit-3 sessions as hit-4 timing trials.


## SP-022 / SP-T029 — hit switch verified; isolated hit-4 baseline (2026-11-17)

Reviewed two new finalized diagnostics-v5 histories in main-world place 110405289082842. Both use offset=0 ms, hold=120 ms, range=8 and both target filters. No runtime code or settings changed during this review.

| Session start | Selection / target | Duration / stop | Attempts | Self counters | Negative-health observations |
| --- | --- | --- | --- | --- | --- |
| 1794955705 | Hit 3 / Thug6 | 48.497 s / Test hit changed | 3 | 1 | 13 |
| 1794955781 | Hit 4 / Thug5 | 36.185 s / manually disabled | 4 | 0 | 39 |

**Selection/switch behavior verified live:** the earlier report finalizes with the hit-change stop reason and retains testHit=3. The later report says testHit=4 and all four F_DOWN entries use animation 139487903747364, with no other automatic attack IDs. Its 10 first-hit cues and 24 other known-hit cues are explicitly filtered. Both end held=false, with no dropped lines, logger/input/release/error/frame-gap failures. All seven API key-ups succeeded and have correctly paired Defend=false receipts, with no unmatched extra F cycles. Three additional hit-4 cues were blocked by observed Counter cooldown. No LOS rejection or defer occurred in the hit-4 run.

### Hit-4 baseline result

| Attempt | F press | Defend=true receipt | Negative-health receipt | Press to damage |
| --- | --- | --- | --- | --- |
| 1 | 5.0519 s | 5.0669 s | 5.1475 s | 95.6 ms |
| 2 | 14.3170 s | 14.3330 s | 14.3970 s | 80.0 ms |
| 3 | 22.9657 s | 22.9834 s | 23.0637 s | 98.0 ms |
| 4 | 30.9825 s | 30.9991 s | 31.0798 s | 97.3 ms |

Each next damage delta is -1.4911; each occurs after the observed Defend=true and before API/observed release. No local counter message accompanies any attempt. All tracks run at speed 1.15, phase 0.0192–0.0240 s at execution; distances 2.259–4.541 studs with clear LOS and facing gates passing. Actual API holds were 130.8–134.3 ms, press-to-request delay 15.0–17.8 ms, request spans 116.8–118.5 ms. Each attempt is later followed by an observed Counter=5 notice. These observations demonstrate an unsuccessful damage-prevention baseline under these conditions; they do not reveal server input acceptance or prove the responsible attack. A missing synthetic press or premature key release is not supported by this capture.

All four attempts were also preceded by local damage approximately 0.40–0.42 s earlier. Being hit by deliberately un-parried preceding combos is a potential confounder; the capture has no affirmative server acceptance or measured stun eligibility. Do not assume changing delay alone fixes this. Longer holds are not supported as the immediate explanation: damage occurred while F remained held.

### Hit-3 follow-up and next comparison

Against Thug6, attempt 1 at 7.6004 s has a sole-candidate self counter at 7.7316 s (~131 ms afterward, no input conflicts). Attempts 2 and 3 precede damage by 113.6 and 113.2 ms with no self counter. This remains mixed and must retain its target context separately from Shadow/Thug5.

**Handoff:** both selected-hit filters and stop/save-on-switch are now observed live. The hit-4 zero-offset baseline has four repeated damage-associated outcomes; another unchanged generic baseline is unnecessary. A bounded next comparison can keep hit 4 selected and hold=120 while changing only offset to **+30 ms**. This deliberately tests a later press, not a claimed calibrated fix: the baseline requests precede damage by roughly 64–81 ms, leaving room for a small controlled shift in client scheduling. Server receipt order, hit-stun/eligibility and attacker attribution remain unknown. Keep the original seed at 0 and record the offset explicitly; do not update other combo seeds from this trial. The +30 ms comparison is proposed, **not applied** during this review.


## SP-023 / SP-T030 — all-hit auto parry restored (2026-11-17)

**User instruction:** add auto parry back. Combat page **v11 / diagnostics v5** restores automatic evaluation of all five known combo IDs, including the previously disabled first hit. This supersedes the temporary first-hit-disabled and single-hit-only experiments. Combat section is simply **Auto Parry**; the Hit 3 / Hit 4 test buttons and their filters are removed, and the original compact control layout is restored.

Catalog: combo 1 (114493250337466)=0.04 s, combo 2 (84394230178792)=0.04 s, combo 3 (95937874840716)=0 s, combo 4 (139487903747364)=0 s, combo 5 (131649056441662)=0.04 s. These are retained experimental seeds, not newly calibrated timings. The proposed +30 ms hit-4 experiment was not applied. Installed saved offset=0 / hold=120 at preparation; those settings remain unchanged. Existing saved TestHit is ignored and omitted on subsequent normal saves, so a prior TestHit=4 cannot restrict restored coverage.

Continuous diagnostics, request/input correlation, scoped head-effect/Z-vanish LOS exclusions, observed cooldown gates, range/facing checks, target ownership/PvP/PvE preferences, typing/menu pauses and release/stop behavior remain. Auto parry still starts OFF on reload; the user enables it. Reports identify scope=all_five in Config/START/CUE_ACCEPTED, while unknown animations remain unmapped. Removing the temporary restrictions does not resolve the recorded hit-3/hit-4 timing limitations; preserve their evidence in SP-T024–SP-T029.

**Validation:** 174 mocked assertions passed: 47 logger/lifecycle, 42 active catalog/LOS/deferral/counter, 26 PvP/PvE, 6 settings/reload, 34 async correlation/LOS, and 19 restoration-specific checks. The latter load legacy TestHit=4 and verify every known hit generates an eligible input for both NPC and player ownership, test controls/filters are absent, old selection is omitted on save, scope is explicit and cleanup completes. Updated source and both staged builds compile. Restoration is not yet live-validated. Exact module replacement preserves concurrent changes in public/private builds.

**Handoff:** reload and enable **Combat → Auto Parry**. All five known combos can now trigger under their existing gates, with logging automatic while enabled. Do not reintroduce single-hit restrictions without a new request; retain the recorder to evaluate real outcomes and unresolved timing issues.


## SP-024 / SP-T031 — all-hit restoration verified live against Shadow (2026-11-17)

Reviewed the completed diagnostics-v5 session starting Unix 1794956187, main-world place 110405289082842, 152.328 s, manually disabled. Header scope=all_five, range=8, offset=0 ms, hold=120 ms, PvP/PvE enabled. A newer history starting 1794956345 was still RUNNING when inspected (snapshot at 10.019 s), so its partial counts are not merged with this finalized report; held=true in that mid-pulse snapshot is not evidence of a stuck key.

**All-hit restoration verified:** 29 automatic attempts cover every known combo, all targeting the Shadow NPC binding: combo 1=10, combo 2=8, combo 3=6, combo 4=4, combo 5=1. No first_hit_disabled or trial_hit_filtered rejection remains. All 29 F-up API calls succeed and have correctly paired Defend=false with matching cycleAttempt. Final held=false, zero dropped lines, no input/release/logger/error/frame-gap failures. The restored scope and cleanup meet their acceptance checks; timing reliability is a separate question.

Totals: 9 self-counter messages, 15 other-counter messages, 6 possible-parried heuristic matches, 0 negative-health observations. The lack of damage against Shadow cannot establish that 29 attacks were successfully parried or be pooled directly with Thug5 damage trials. Other player/NPC bindings were observed, but none was an actual automatic-input target.

Of the nine self-counter receipts, only **two** have a sole automatic candidate with no subsequent-input conflict: attempt 7 at 50.6310 s and attempt 12 at 78.4632 s (both combo 2; approximately 167/183 ms after their presses). The message at 12.1483 s has sole candidate attempt 2 but inputConflicts=[2], so it must not be counted as an isolated automatic result. Four more receipts are ambiguous between two attempts with intervening input flags (41.1305, 93.2612, 101.5622, 128.4103 s); two have no recent automatic candidate (8.8991, 115.1778 s). The logger retains these distinctions rather than crediting every counter to automation.

Observed extra F cycles are correctly retained as unmatched. The report contains 94 DEFEND_REQUEST events in total, including 58 request events for the 29 paired automatic pulses. Request observation coverage is present; neither API success nor request observation is server acceptance.

Press-to-Defend delay is 13.2–19.8 ms (median 16.3); actual API holds 128.4–137.2 ms; adjacent automatic request spans 113.2–122.9 ms. Release-request lag is 0.7–3.6 ms. One bounded cooldown deferral occurs at 10.9678 s with 49.1 ms remaining, followed by attempt 1 at 11.0172 s; this confirms execution, not a successful outcome. Skip totals: attacker facing 8, Counter cooldown 22, F already down 5, range 8, self-facing 5, unmapped animation 554. No LOS rejection occurs.

**Handoff:** retain restored all-five-hit mode as requested; no timing/settings/code changed during this review. Scope and input cleanup are live-verified. Counter attribution remains mixed, and zero damage against Shadow is not evidence of general reliability. Review the newer run only once enough evidence is available, keeping session boundaries and target context explicit. Do not reintroduce single-hit restrictions or silently tune seeds from raw counter totals.


## SP-025 / SP-T032 — later-hit eligibility audit after restoration (2026-11-17)

The previously partial session starting Unix 1794956345 is now finalized: diagnostics v5, main-world place 110405289082842, scope=all_five, offset=0 / hold=120, both target filters enabled, 119.398 seconds, manually disabled. All 21 automatic attempts target Shadow: hit 1=3, hit 2=12, hit 3=6, hits 4/5=0. All 21 releases succeed and have paired Defend=false receipts with cycleAttempt; final held=false, no dropped lines or input/release/logger/error/frame-gap events. Zero negative-health observations again cannot prove successful parries against this target.

Nine self-counter messages comprise six sole-candidate, conflict-free temporal associations (five hit 2, one hit 3), one ambiguous receipt between attempts 12/13 with input conflict on 12, and two without recent automatic candidates. This is correlation, not confirmed causation or a six-of-21 success rate. Seven other-counter messages and seven possible-parried heuristic matches are separate from local success.

**Later-hit finding:** all seven observed hit-4 cues and all five hit-5 cues reach the mapped cue path. Hit 4 skips: four Counter cooldown, three F already down. Hit 5 skips: four Counter cooldown, one F already down. Thus all 12 later-hit cues are accounted for without a selector/filter regression. Three of the four F-down-priority skips also have a positive cooldown, so skip totals reflect the first failing gate, not mutually exclusive underlying conditions. At these decisions the hub itself is not holding F. Input state alone does not establish whether an extra press came from the user or another input source.

**Cooldown audit:** reconstructing each later-hit deadline from the latest received Counter duration reproduces the reported remaining time within 0.4 ms. Explicit cooldown skips have 0.838–4.948 seconds remaining, well beyond the existing 80 ms bounded deferral. Source uses the latest named Counter notice to replace the deadline, including shorter notices. No arithmetic/stale-deadline bug is demonstrated. Whether the named notice always denotes actual parry unavailability is still unverified; do not bypass it or assume its server semantics from its name alone.

Two separate LOS rejections involve Workspace.Thrown.explosion (non-collidable, queryable Part) and Workspace.Thrown.Debris (collidable, queryable Part). A cosmetic explosion exclusion is a candidate for further inspection, not applied based on this single observation. Do not ignore all Thrown descendants or collidable debris. Existing exact local-head exclusion remains observed.

**Decision / next action:** keep all five hits and automatic recording active-capable; runtime and saved settings unchanged. Before changing timing, obtain a defensive-only encounter against a damage-dealing NPC with normal parry availability and no extra F input or outgoing attacks. This separates cooldown/input eligibility from timing and opponent-parry effects while retaining full combo coverage. Compare per-hit attempts, preceding damage/stun evidence, cooldown receipts, request pairs and counter/damage outcomes. If hit 4 reaches input but repeats the earlier damage baseline, compare a bounded +30 ms hit-4-only delay while leaving other seeds unchanged; that comparison is still proposed, not installed. No generic global timing shift or single-hit restriction is justified by this session.


## SP-026 / SP-T033 — Thug8 baseline and bounded hit-3 delay comparison (2026-11-17)

Reviewed finalized session starting Unix 1794956677, diagnostics v5, main-world place 110405289082842, all-five scope, offset=0 / hold=120, both target types enabled. Duration 75.725 seconds, manually disabled. All 20 automatic attempts target Thug8: hit 1=13, hit 2=4, hit 3=3, hits 4/5=0. All 20 API releases succeed and have paired Defend=false receipts with matching cycleAttempt. Final held=false, no dropped lines or input/release/logger/error/frame-gap failures. There are 22 negative-health observations, 14 self-counter messages, two other-counter messages and one possible-parried heuristic match.

**First hit:** 12 of 13 attempts have a sole-candidate self-counter association without conflicting input, 172–232 ms after the API press. Attempt 17 has no such isolated result; a later counter lists both attempts 17/18. Preserve this stronger first-hit baseline, while retaining that receipt attribution is temporal rather than server-confirmed causation. **Second hit:** one clean sole-candidate counter, one ambiguous counter shared with the prior hit-1 attempt, and two damage-associated attempts (+172.3 / +168.8 ms). The two damage receipts arrive after F release; this observation alone does not establish that extending the hold would fix them.

**Hit 3 repeatedly reaches input but takes damage:** attempts 14/16/20 press at 39.6267 / 52.9930 / 63.8961 seconds and precede damage by 98.2 / 96.8 / 113.1 ms, with no self-counter. Each damage receipt occurs after Defend=true and before both API and observed Defend=false releases. Tracks execute at phase 0.0192 seconds, speed 1.15, distance 3.910–4.322 studs, with facing/LOS passing and no reported cooldown or pre-existing F press. Preceding local damage is approximately 319 / 370 / 354 ms before these presses, so hit-stun or other server eligibility remains a confounder. The final sequence also contains outgoing attack activity and a possible-parried match: do not label the whole capture perfectly passive or blame extra input on the user without proof.

**Later-hit eligibility:** both observed hit-4 cues and both hit-5 cues are skipped for Counter cooldown, with remaining durations 0.8156 / 4.9130 seconds and 0.0985 / 4.2889 seconds respectively. Their reconstructed deadlines agree with received notices. The 98.5 ms remainder is above the existing 80 ms deferral bound, so it is skipped as designed. No f_already_down skip or LOS rejection occurs in this session. There is no new hit-4/5 timing result.

**Implemented comparison — Combat page v12 / diagnostics v5:** change only hit-3 animation 95937874840716 seed from 0 to 0.03 seconds. Keep hits 1/2/4/5 at 0.04/0.04/0/0.04, all-five coverage, hold preference, additive global offset, cooldown handling, filters and continuous diagnostics. This tests a slightly later press against the repeated zero-delay baseline; it is not a calibrated fix. Observed hit-3 Defend requests precede damage by approximately 82–88 ms, giving a bounded later-press comparison without claiming a server window. Do not extend holds or bypass cooldowns at the same time. The previously proposed hit-4 +30 ms comparison remains unapplied.

Config, START and CUE_ACCEPTED include timingTrial=hit3_plus30ms, and the seed catalog reports combo3:0.03. The Combat status identifies Hit 3 +30 ms trial while retaining all five hits and the normal controls. Starts OFF after reload. No saved settings are overwritten; a nonzero user offset still adds to every seed and must be retained in analysis.

**Validation:** source and both staged builds compile. 193 mocked assertions pass: 47 logger/lifecycle, 42 active-feature, 26 PvP/PvE, 6 settings, 34 asynchronous correlation/LOS, 19 all-hit restoration, 19 targeted timing/cleanup. Targeted checks confirm no hit-3 input before 30 ms, input after the deadline, unchanged other-hit scheduling, normal hold/release, geometry revalidation, cancellation on stop and additive user offset. Existing correlation fixtures were advanced past the new hit-3 deadline while preserving their API-relative event spacing. These are mocked scheduling checks, not live confirmation of improved parrying. Exact module replacement preserves unrelated concurrent modules. Existing Beyond Nen notes inform evidence separation; no foreign timing constants or remotes are imported.

**Handoff:** reload the installed hub and enable Combat auto parry; confirm the status identifies Hit 3 +30 ms trial. Repeat a comparable Thug encounter with offset=0 / hold=120, allowing automation to defend. Keep all hits enabled. Compare hit-3 request/health/counter timing against this baseline, retaining prior-damage and cooldown context. Revert the 30 ms trial if evidence worsens; do not claim success from request/API counts. Hit-4/5 cooldown eligibility and hit-2 mixed outcomes remain unresolved.


## SP-027 / SP-T034 — hit-3 delay shows no benefit; event-state diagnostics (2026-11-17)

Reviewed completed session starting Unix 1794957002, main-world place 110405289082842, 171.667 seconds, manually disabled. Config confirms timingTrial=hit3_plus30ms, combo3:0.03, all-five scope, offset=0 / hold=120. There are 25 automatic attempts: two against Shadow, then 23 against Thug5. All 25 releases succeed and have correctly paired Defend=false receipts; final held=false, zero dropped lines, no input/release/logger errors. A single 126.9 ms frame gap at 0.2914 seconds precedes the first automatic attempt by about 35 seconds; do not attribute later misses to it.

Totals: 11 self counters (all sole-candidate, no input conflicts: eight hit 1 and three hit 2), three other counters, two possible-parried heuristic matches, 44 negative-health observations. Automatic coverage: hit 1=11, hit 2=5, hit 3=6, hit 4=2, hit 5=1. API-to-observed-Defend latency is 15.7–34.6 ms, median 20.2; API hold 122.9–141.3 ms. These figures are not a server success rate. Outgoing attack activity and unmatched input remain present, so this is not a fully isolated passive comparison.

**Hit-3 trial verified, benefit not demonstrated:** one attempt against Shadow has no near-term damage or counter and is kept separate. All five against Thug5 precede damage, with no self counter: attempts 11/15/16/22/24 at 109.3524 / 126.5997 / 131.6854 / 147.6094 / 158.0434 seconds, damage after 76.5 / 54.7 / 72.2 / 50.4 / 82.5 ms. All damage occurs after observed Defend=true and before both API and observed release. Actual cue ages 38.1–50.6 ms confirm the later scheduling, with speed 1.15, phases 0.0623–0.0767 and facing/LOS passing. This failed to produce a successful hit-3 outcome in the damage-dealing encounter. The previous baseline used Thug8 and different encounter conditions, so do not claim a statistically established regression or infer an exact server parry window from receipt delays.

**Later hits now execute:** both hit-4 attempts against Thug5 precede damage (+53.6 / +99.0 ms) while F remains held, with no counter. The single hit-5 attempt precedes damage by 165.0 ms after release, with no counter. These do not prove the damage source or server acceptance. Additional hit-4 skips: five cooldown, one F already down; hit-5 skips: six cooldown, one F already down. Keep those eligibility failures separate from attempts that reached input. No LOS skip occurred.

**Implemented — Combat page v13 / diagnostics v6:** remove the unsuccessful +30 ms trial and restore hit-3 seed=0. All other seeds, all-hit coverage, settings and gates are retained. Config/START/CUE_ACCEPTED identify timingTrial=baseline_state_capture. Combat status identifies Diagnostics v6. All inputs remain off after reload until enabled by the user.

Add bounded event-time local state snapshots immediately after a successful F-down API call and on negative-health events, alongside the existing half-second state-delta sampler. STATE_CONTEXT records trigger=after_input with attempt/cue, or trigger=damage with damage delta/recent candidates, plus lastDamageAge, fields and omitted. STATE_POINT lists up to 64 sorted relevant client-visible fields from character/humanoid/player attributes and direct character ValueBases, plus Health/MaxHealth. Repeated unchanged fields are retained in these snapshots; no absent state is invented. At a damage trigger, lastDamageAge refers to the preceding damage observation; at an input trigger it refers to the latest observed damage. It is unknown before the first recorded damage and resets for each session. Events obey existing report line/byte limits. Missing fields are unknown, not proof of no stun or server eligibility.

Snapshot collection runs after the key-down call so it does not postpone scheduling that press; it can still incur client overhead that needs live observation. Snapshot errors log STATE_CAPTURE_ERROR without interrupting normal F-release cleanup. The periodic sampler remains unchanged. As with the Beyond Nen reference workflow, use these observations to distinguish decision/input/result stages; no game-specific timing or remote semantics are imported.

**Validation:** source and both staged builds compile; 192 mocked assertions pass (174 existing logger/active/PvP/settings/correlation/restoration checks plus 18 event-state checks). New checks cover a transient state between periodic samples, previous-damage age/session reset, 64-field bound, absent fields remaining absent, zero-delay rollback, and snapshot-error isolation from release cleanup. These validate the implementation, not live performance or improved parrying. Exact module transplantation preserves unrelated concurrent changes; saved settings are not overwritten.

**Handoff:** reload and enable the Diagnostics v6 build, with offset=0 / hold=120 for comparison. All five hits and automatic recording remain. Review after_input and damage snapshots for short-lived replicated stun/busy/attack/counter fields and previous-damage timing before another timing adjustment. A field appearing is evidence of client state only; do not invent a stun gate or bypass cooldowns based solely on its name. If no useful state is exposed, record that limitation rather than repeating arbitrary delay trials. Hits 2–5 remain unreliable; the hit-4 +30 ms proposal remains unapplied.


## SP-028 / SP-T035 — event snapshots validated; separate Shadow and PvP results (2026-11-19)

**Evidence and configuration:** reviewed three finalized diagnostics-v6 histories starting Unix 1795076236, 1795076353 and 1795076420. All report universe 10771972206 and place **135537125271116**, an additional recorded place rather than the previously documented main-world place. Its name/role is not verified. All use baseline_state_capture, all-five scope, offset=0 ms, hold=120 ms, range=8, PvP/PvE enabled, and end by manual disable. Exact source-module comparison confirms the current public artifact and installed private build each contain the same v13 module. No runtime or settings changed during this review.

| Capture | Duration | Actual automatic target | Attempts by hit 1/2/3/4/5 | Self-counter receipts | Negative-health events |
| --- | --- | --- | --- | --- | --- |
| First, 1795076236 | 88.696 s | Shadow NPC | 1 / 3 / 2 / 2 / 0 | 2, both sole-candidate and conflict-free | 0 |
| Second, 1795076353 | 66.397 s | Shadow NPC | 2 / 7 / 1 / 0 / 0 | 8, all sole-candidate and conflict-free | 0 |
| Third, 1795076420 | 195.844 s | Player binding 4 | 2 / 2 / 0 / 1 / 2 | 5: three without an automatic candidate, two ambiguous | 33 |

**NPC observations:** the first run has a hit-4 counter association at 45.3453 s, about 81 ms after attempt 2, and a hit-1 association at 47.0294 s, about 184 ms after attempt 3. The second has two hit-1 and six hit-2 associations, about 181–184 ms after input. These ten receipts have one candidate and inputConflicts=[]; they are stronger temporal evidence, not server-confirmed attribution or a ten-of-18 success rate. The three hit-3 attempts across the two runs have neither a self-counter nor negative health. Shadow's zero-damage encounters cannot establish general defense reliability or replace damage-dealing NPC evidence. Later-hit cooldown skips reconstruct from the received Counter notices; no selector regression or cooldown arithmetic error was found.

**PvP observations:** all seven automatic attempts target a player, confirming that player recognition reaches input with PvP enabled. They do not establish successful PvP defense. Counter receipts at 26.9143, 56.3801 and 79.4638 s have no recent automatic candidate. Receipts at 59.6648 and 100.9121 s list two candidates with intervening-input conflicts. None qualifies as an isolated automatic result. Unmatched F cycles and outgoing attack activity remain in the capture; their source and causal effect cannot be assumed.

Three of the seven PvP attempts precede damage within half a second: hit 5 at 4.8502 s (+133.5 ms), hit 1 at 58.6807 s (+165.4 ms), and hit 2 at 59.1313 s (+165.0 ms). The hit-5 damage occurs just before API release; the other two follow release. Their preceding-damage ages at input are approximately 401 ms, 3.117 s and 286 ms. This mixed pattern does not establish a single hold/delay correction or prove the damaging attacker. The other four attempts have no damage within half a second, which is also not proof of a successful block. Known-cue skips include 22 out-of-range, two Counter cooldown, one F-already-down and one attacker-facing rejection. All six known hit-3 cues were out of range, so this session supplies no PvP hit-3 input-timing result. The 702 unmapped-animation skips include ordinary overlapping animations; they are not 702 demonstrated missed attacks or sufficient evidence to expand the attack catalog.

**Diagnostics v6 verified live:** all 25 successful F-down API events have an after_input snapshot, and all 33 negative-health observations have a damage snapshot. There are 58 contexts and 632 state fields (8–12 per snapshot), omitted=0 throughout, no capture errors, no dropped lines and no frame-gap failures. Fields include Health/MaxHealth, AttackerTags, Fatigue, Ki, PowerLevel, Combo, CounterCooldown, PBlock, LastAttacker and BaseMaxHealth where exposed. One damage snapshot also has Ragdolled=false. No stun or busy field appears in these snapshots; absence only describes this recorder's client-visible attribute/direct-value coverage. It does not prove the character is free to parry. LastAttacker is a string observation, not a guaranteed attribution for each damage event; its value remains private.

PBlock is false at the three damage-associated PvP presses and their damage snapshots, but is also false at nine isolated NPC counter-associated presses; it is absent from the remaining hit-4 snapshot. PvP attempt 6 instead has PBlock=true at input, without a following self-counter. Thus the available field values do not provide a demonstrated eligibility classifier. CounterCooldown is an opaque absolute numeric value; do not compare it directly with client os.clock or infer enforcement from its name. No useful replicated stun/busy discriminator emerged from this batch.

**Input cleanup:** all 25 automatic F-up calls report success and all 25 Defend=false observations have valid matching cycleAttempt pairs. Every final report has held=false. API holds range 130.8–138.4 ms; observed request spans 116.5–124.2 ms; input-to-Defend delay 14.3–18.1 ms. This verifies logging, request correlation and release behavior in these runs, not server acceptance or calibrated defense timing.

**Decision and handoff:** preserve v13 baseline timing, all five hits, filters, observed cooldown gates and automatic recording. No code deployment or reload is required for this review. Event-state capture has met its live acceptance checks, but it did not resolve eligibility. Keep the Shadow findings separate from the player encounter and earlier Thug failures. Do not ask for another unchanged generic capture, claim success from zero damage, add every unmapped animation, invent a stun gate or make a global timing shift. A future timing/catalog change needs a concrete discriminating comparison or a verified game-specific state/attack mapping; this batch does not select one. Public notes contain only sanitized findings; raw captures and player identifiers stay local. No compile or mock suite was rerun because only documentation changed; prior implementation checks remain recorded in SP-T034.


## SP-029 / SP-T036 — bounded PvP range-entry recheck (2026-11-19)

**Existing evidence audited:** continued analysis of SP-T035's completed PvP run (Unix 1795076420, place 135537125271116). Of 22 out-of-range mapped-cue rejections, 13 concern the player binding targeted by the automatic attempts, while nine concern other nearby players. Ten cues start within the configured 8-stud radius plus 3 studs; nine of these also pass the recorded facing thresholds, all from the main opponent. Eight of those nine precede local negative health within 300 ms. Examples: hit 2 at 3.5156 s, distance 9.879, damage about 250 ms later; hit 4 at 4.3663 s, distance 9.998, damage about 83 ms later; hit 3 at 103.8282 s, distance 8.777, damage about 117 ms later. Several other rejections have substantial vertical separation or involve bystanders, so increasing range globally is not supported.

**Confirmed source behavior versus hypothesis:** the old observe path permanently discards a cue on its first out-of-range result. It cannot recover that swing even if the opponent subsequently enters range. The recording has no continuous root-position trace for those discarded cues, so it cannot prove that any particular opponent crossed the boundary before impact or that the observed damage came from that cue. Recovery is a bounded implementation improvement for that demonstrated code path, not a retrospectively measured missed-parry count or a proven live fix.

**Three damage-associated presses:** the hit-5 damage at 4.9837 s occurs about 4.9 ms before API release. The hit-1 and hit-2 damage at 58.8461 / 59.2963 s follows their API releases by approximately 32 ms. These mixed observations do not determine a shared hold correction; existing request timing and state snapshots also do not establish server acceptance. Preserve baseline seeds and hold preference instead of changing both coverage and timing together.

**Implemented / untested live — Combat page v14, diagnostics v7:** a fresh mapped player animation just outside the user's configured range can enter a brief approach queue. Admission requires current allowed player ownership, playing forward animation with finite positive speed and phase 0–50 ms, distance no greater than Range+3 studs, normal facing and line of sight. Only two approach cues may wait concurrently, within the existing eight-cue total limit. NPC cues and players beyond the margin retain their existing immediate rejection behavior.

An approach cue checks at 20 ms intervals, at most four times and no later than 80 ms from observation. If it enters the actual configured range, it can proceed through the normal gates; it never advances the original per-hit seed or additive user offset. Entry before the seed waits until the original deadline and revalidates geometry again. Expired cues are discarded even if a delayed frame subsequently sees an in-range target. Rechecks retain manual-F, held-key, current/live ownership, playing-track, optional knocked/off-screen, facing, wall, cooldown, typing/menu, stop and cleanup checks. The actual parry range does not increase. All five mapped hits remain enabled; no new attack IDs, cooldown bypass, direct remote actuation, NPC timing change or hold change is introduced.

**Recording:** header/config declares pvpRangeRecheckMs=80, rangeMargin=3, rangeStepMs=20 and rangeSlots=2. RANGE_WAIT marks admission, RANGE_CHECK retains per-check client distance/vertical/facing and track context, and RANGE_ENTERED marks observed entry before normal execution. Entry alone does not mean an input or parry succeeded. Normal CUE_ACCEPTED/F_DOWN/request/release/outcome logs remain the evidence for later stages; range_wait_expired and range_queue_full explain bounded rejections. Report remains subject to existing limits. This closes the missing client-geometry observation for future approach cues without claiming to reconstruct old movement. Combat status identifies PvP range recheck v14. Existing Recorders controls and state snapshots remain.

**Validation:** 231 mocked assertions pass: the existing 192 plus 39 approach scenarios covering movement across the actual boundary, seed/offset preservation, zero-delay hit recovery, expiry and delayed frames, unchanged NPC rejection, distant players, two-slot bound with room for a normal cue, duplicate/invalid tracks, walls, ownership/death, manual input, cooldown, typing/menu, PvP toggle, stop and release cleanup. Updated module and both staged builds compile; both regenerated build variants are also compile-checked before installation. Tests validate local scheduling and gating, not improved live PvP defense. Exact module replacement and hash-checked installation preserve unrelated modules and private/public separation. The shared bounded decision/input/result methodology follows the existing Sparkling/Beyond Nen notes; no foreign combat timings were imported.

**Handoff:** reload and enable the installed v14 build with the user's existing settings. A useful comparison now has a specific question: do RANGE_WAIT cues actually enter the configured radius within the bound, and do those inputs produce isolated counters or damage? Compare RANGE_CHECK geometry, RANGE_ENTERED, subsequent execution/release and outcomes; retain NPC/player separation and ambiguous-input flags. If there are no entries, do not call the change effective or enlarge the margin automatically. Hold timing and server eligibility remain unresolved. No unchanged generic recording is required to justify this implementation; future live validation concerns this explicit coverage change.


## SP-030 — missed-parry cooldown clarified by the user (2026-11-19)

**User-confirmed mechanic:** missing a parry incurs a parry cooldown. This supplies game context that earlier notes left unknown. Separate the initial unsuccessful attempt from subsequent attacks that arrive during the penalty: a cooldown-rejected cue is not another independent timing failure. Identical M1 animations do not imply identical parry availability after a miss. This correction does not explain why the initiating parry failed.

**Observed supporting sequence:** in the second Shadow run from SP-T035 (Unix 1795076353), hit-2 input at 34.0149 s and hit-3 input at 34.4994 s have no self-counter receipt. A Counter duration=5 notification arrives at 34.9626 s. Hit 4 at 35.0321 s is then skipped with about 4.931 s remaining, and hit 5 at 35.5650 s with about 4.398 s remaining. Those later skips are consistent with the user's explanation and are deliberate cooldown gating, not failure to detect those animations. The notice does not identify which of the two preceding attempts caused the penalty; do not automatically blame hit 3. The user confirmed the existence of the penalty, not that every duration=5 event has this exclusive meaning or its precise onset/reset rules.

The first Shadow run (Unix 1795076236) similarly has Counter duration=5 at 51.6456 and 57.2116 s following attempts without isolated counter confirmation. Separately, ten isolated self-counter receipts across the two runs are followed by duration=0.5 notifications. That is an observed association, not enough to assign all short notices one meaning. One 0.5 notice at 55.2304 s replaces a still-active longer recorded deadline in the current implementation; whether that should reset a missed-parry penalty remains unverified. Preserve the actual notice chronology rather than silently assuming one shared or separate server timer.

**Updated investigation priority:** analyze the first eligible unsuccessful attempt before each long cooldown, including prior input, hit phase, request/counter timing and the delay until the cooldown notice. Do not tune from all subsequent combo skips or describe the entire chain as random timing. Determine whether the penalty becomes active before the client notification and whether later short notices reset it before changing deadline handling. Keep the existing cooldown gate, timings and v14 PvP approach change as installed; no runtime edit is justified by this semantic clarification alone. The v14 approach change addresses a separate range-observation limitation, not the missed-parry penalty. Public notes retain user confirmation separately from capture-derived evidence; raw captures remain local.


## SP-031 — NPC impact timing across all captures; NPC hits 3–4 no longer parried, NPC hit 5 timing trial (2026-11-19, Claude)

**Method:** one pass over every diagnostics history with NPC swings (main world `110405289082842` and place `135537125271116`; Thug5/6/8 and Shadow, ~900 mapped swings, ping ~107–115 ms). For each mapped NPC swing: the client-seen `ANIM_START`, our `F_DOWN`, the attacker's **`HitOut` effect** (impact receipt), and the local effect within −30/+120 ms of impact: `Counter Parry!`, `Blunt` (hit taken), `BlockEffect`, `Dodged!`. Counter cooldown notices within 0.6 s of impact. The analysis script is local (not published).

**Impact after the swing is seen (median, ms):** hit 1 ≈ 234, hit 2 ≈ 234, hit 3 ≈ 116, hit 4 ≈ 85, hit 5 ≈ 184.

**Outcome by lead (impact − press):** below 80 ms, 0 counters in 13; 80–159 ms, 15 counters in 99 (≈15%); 160–179 ms, 11 in 26; 180–199 ms, 65 in 92 (≈71%); 200–239 ms, 15 in 21. A counter needs F roughly **160–180 ms before impact receipt** at this ping.

| Hit | Press after seen start | Lead | Counters / attempts |
| --- | --- | --- | --- |
| 1 | ~50 ms | ~180 ms | 52 / 73 (71%) |
| 2 | ~50 ms | ~180 ms | 40 / 76 (53%) |
| 3 | 2 ms (immediate) | ~115 ms | 9 / 61 |
| 4 | 2 ms (immediate) | ~80 ms | 2 / 25 |
| 5 | ~52 ms | ~135 ms | 4 / 10 |

**Combo structure (Verified from these captures):** hit 2 never followed a countered hit 1, and hit 3 never followed a countered hit 2 (no mapped swing followed any countered swing within 1.3 s). **A counter ends the NPC's combo.** Gaps 1→2, 2→3, 3→4 and 4→5 vary ~400–1,000 ms (p10–p90), too wide to predict a later hit from the previous one.

**Cooldown cost:** every counter (107/107) was followed by `Counter duration=0.5`. A failed attempt (`Blunt`) was followed by `duration=5` in 68 of 111 (the user-confirmed missed-parry penalty, SP-030). Counter-cooldown skips of hits 1/2 total 81 across the batch. The newest Shadow run (1795077608) shows the chain plainly: all six hit 3/4/5 attempts failed and each produced the 5 s penalty, then 9 hit-1/2 swings were skipped during it.

**Conclusion:** hits 3–4 cannot be parried reactively at this latency; the client sees their animation too late. Hit 5 is different: it lands ~184 ms after it is seen, like hits 1–2, and failed only because its 0.04 s seed left ~135 ms of lead. Attempting them mostly buys a 5 s penalty, which then blocks the next combo's hit 1 (71%) and hit 2 (53%). Hits 3–5 only occur after hits 1–2 already got through.

**Implemented / untested live — Combat page v15:** NPC cues for hits 3 and 4 are skipped at observe time (`SKIP reason=npc_late_hit hit=N`). Separate trial: NPC hit 5 is pressed with seed 0 (~180 ms lead expected), marked `trial=npc_hit5_seed0` on its CUE_QUEUED line. Header config adds `npcSkip=hit3,hit4 npcHit5Seed=0`. Player cues, all seeds, hold, range recheck and gates are unchanged. This is NPC-only: Claude does not work on the PvP path (project scope line); PvP behaviour is GPT's.

**Test:** fight the Shadow or a Thug with Auto Parry on. Expect `npc_late_hit` skips, fewer `duration=5` notices, and fewer `counter_cooldown` skips of hits 1/2; judge the hit-5 trial on its own counters. Trade-off for the Shadow farm: hits 3–4 now land instead of rarely being countered, so kick-outs may rise (compare with the first run: one in ~110 s). The open question is whether hit 1/2 counters per combo rise. Not yet tested: a slightly earlier hit 1/2 press (lead 200–239 ms countered 15 of 21, a small sample), and whether `Blunt` failures of hit 2 come from hit-stun after hit 1.

**Auto Shadow Farm v1 (SP-A002) first live run (user, 2026-11-19):** ~110 s, PowerLevel 1,599 → 2,197; one kick-out, re-entered 2.5 s later. The Shadow's fields include **`@Player` = the owner's name** and `@AIId=Shadow`, health 60/60. v1.1 now targets only a Shadow whose `@Player` is the local player (or has no owner tag).


## SP-032 — v15 verified against the Shadow; own punches block counters; where Shadow-farm BP comes from (2026-11-19, Claude)

**Evidence:** diagnostics v7 / Combat page v15 histories 1795078255 (416 s) and 1795078719 (1,872 s), both `npcSkip=hit3,hit4 npcHit5Seed=0`, Shadow only, run alongside Auto Shadow Farm. Detail lines hit the 12,000-line cap early in each (dropped 6,480 / 73,014), so per-swing figures cover the detail portion (271 swings); footer totals cover the whole session: 532 attempts, 345 self counters, 0 damage events.

| Shadow, pressed swings | Before v15 (SP-031 batch) | v15 |
| --- | --- | --- |
| Hit 1 counters | 3 / 15 | 35 / 57 (61%) |
| Hit 2 counters | 22 / 38 (58%) | 49 / 70 (70%) |
| Hit 5 counters | 0 / 3 | **15 / 16 (94%)** at seed 0, ~182 ms lead |
| Hit 1/2 skipped for counter cooldown | 39 | 18 |
| Share of swings that were hits 3–5 | 37% | 23% (combos end earlier) |

**Verified:** the hit-5 seed-0 trial works (94%), and skipping NPC hits 3–4 roughly halved cooldown losses. Keep both.

**Own punches block counters (Verified from these captures):** time from our last outgoing `Action("MouseButton1")` to the F press, hits 1–2: under 150 ms → **0 / 19 counters** (12 `BlockEffect`, 3 `Blunt`); 150–300 ms → 4 / 16; 300–500 ms → 16 / 23; 500–1,000 ms → 36 / 44; over 1 s → 28 / 32. A press too soon after a punch gives a block instead of a counter. This explains why hit 1/2 (which arrive while the farm is punching) trail hit 5 at the same lead.

**Where the farm's BP comes from (sampled `character.PowerLevel` rises in history 1795078255, 252 s):** ~1,900 of 2,417 PL followed our own landed hits (self `HitOut` + Shadow `Blunt`) within 0.6 s; ~240 followed hits taken; counters ~150. **Landing punches is the BP source;** parrying mainly prevents kick-outs. Decision: do not slow the farm's punching for better counters.

**Auto Shadow Farm second run (user, 2026-11-19):** ~39 min, PowerLevel 2,202 → 23,634, one kick-out (re-entered in 2.5 s). The `@Player` owner filter was active. Verified live.

**Next:** none required. If kick-outs become a problem, consider pausing punches for ~300 ms when the Shadow starts a hit-1 swing; not built.

## SP-033 — Fatigue and injuries: where they live; HxE-style hold not built yet (2026-11-19, Claude)

**Request (user):** a fatigue and injury block like Hunter X Enter's Infinite Stamina.

**Why the HxE approach does not transfer directly (source + capture inspection):** HxE stamina is owned by the client (`_G.StaminaSystem` upvalue) and the server trusted a client `ConsumeStamina` request (HXE-001). Sparkling differs:
- `Character.Fatigue` is a NumberValue (0–100). In the parry captures it rises ~0.00035 per sample at idle, in ~1 s steps, with no jumps from hits (1.13→1.21 over one 31-min Shadow-farm detail window; 1.78→1.87 in the next). Fatigue is not limiting the Shadow farm (Trello: penalties start at 80%, PL gain stops at 90–95%).
- Injuries are `Character.<Part>Inj` NumberValues with `@Minimum=0` (profile scan 2026-11-17: TorsoInj 13.6, LeftArmInj 28.0, RightLegInj 12.9), plus `Character@LimbPiecesWatched=true`. The HUD figure is `PlayerGui.KiHud.Stage.limbs` (LimbBody/LimbHaze images, LimbPercent, FatiguePercent "6%").
- No client request carrying fatigue or injury appears in the parry captures so far (their hooks cover `Requests.*`; not proof of full coverage).
- Trello: fatigue rises from landing M1/M2, taking damage and using skills; 90%+ halves damage, raises damage taken, nerfs movement/counter window and stops PL gain. Head injuries affect regeneration and blur vision, arm injuries lower damage, leg injuries lower speed. N (Sleep) lowers fatigue to 0%; hospital beds clear both.

**Expected:** server-side effects (damage, PL gain, regeneration) cannot be blocked from the client. Possible client-side pieces: the head-injury blur and the leg-injury speed (if a LocalScript sets WalkSpeed from the local value), and the HUD display.

**Implemented / untested live — Fatigue & Injury Data v1 (Recorders):** dumps matching values/attributes on player, character and humanoid; humanoid WalkSpeed/Jump; PostEffects in Lighting and the camera; the KiHud limbs figure (image ids, colors, text); `getconnections` listener sources for each value's Changed; names of matching LocalScripts/ModuleScripts. Then sets each value above its minimum to the minimum locally for 3 s, logging `REVERTED` (game put it back) or `STAYED` (restored by the recorder), with samples of WalkSpeed, HUD % and effects.

**Next:** user captures once with injured limbs. If values revert at once → server-owned; offer Auto Sleep (N) at a fatigue threshold instead. If they stay and the blur/speed follow → build a local hold for those parts only, and say plainly that server effects remain.

**Result — first capture (user, 2026-11-19, place 135537125271116, Verified from capture):**
- `Fatigue` set to 0 locally **reverted in 0.07 s** and kept rising (1.09→1.14 in 2.6 s while active; HUD showed 0% for one sample, then 1%). **Fatigue is server-owned; a client hold is not possible.**
- Six injury values exist: `HeadInj` 3.2, `TorsoInj` 2.1, `LeftArmInj` 56.9, `RightArmInj` 79.4, `RightLegInj` 3.5, `LeftLegInj` 2.8. Set to 0 locally they **stayed for 3 s** (then restored by the recorder). This only means the server did not change them in those 3 s; a replicated value is overwritten on its next server change. Their effects (arm damage, regeneration) are server-side per Trello.
- No `Changed` listeners on any value; WalkSpeed stayed 16 and `Lighting.Blur` stayed size 1 throughout (head injury only 3, so the blur test is inconclusive). Matching client scripts: `PlayerScripts.Primary.Effects.Bleed` and `RampageBlur` only.
- Recorder flaw (fixed in v1.1 source): the word "rest" matched `TempCoreStr` (50), which was zeroed locally for 3 s and restored.

**Conclusion:** an HxE-style fatigue/injury block cannot affect the server. The workable feature is **Auto Sleep** (N lowers fatigue to 0%, Trello) at a fatigue threshold; sleep mechanics (toggle, duration, where allowed) are not yet captured.

**User goal (2026-11-19): stop injuries from rising, not their debuffs.** Checked every outgoing request in all Sparkling captures: only `Requests.Action` (MouseButton1, Defend, `Update` = `{airborne=true/false}`), `Requests.Dashed` and `Requests.WallSlam`. None carries injury or fatigue, so there is nothing to withhold the way HxE withholds `ConsumeStamina`; the server raises `<Part>Inj` itself from hits taken and landed. **Not buildable from the client.** In-game reducers (Trello): Body Strength/Durability (`can't be injured as easily at higher levels`), Determined (60% injury nullification, 80–100% with its card), Odd Makeup / Gritted Teeth (racial), 2nd-form `no injury gain while blocking`, Android race (no injuries); beds, Senzu and the Rejuvenating Slumber card clear them.


## SP-034 — apply Claude's NPC parry policy to players (2026-11-19)

**User request:** apply the auto-parry behavior Claude implemented for NPCs to players as well. This explicitly extends SP-031's NPC-only policy; the earlier all-five player behavior is superseded for Sparkling. Claude's NPC implementation and SP-031/SP-032 evidence remain intact. The existing Sparkling PvP authorization controls this change regardless of historical NPC-only scope statements elsewhere in the project.

**Implemented / PvP untested live — Combat page v16, diagnostics v7:** both allowed player and NPC targets use hits 1, 2 and 5. Hits 3/4 are deliberately skipped at observation, before entering the pending or approach queues. NPC skips retain reason=npc_late_hit; player skips use reason=player_late_hit, with hit number and target type. Hit 5 uses seed 0 for players as it already did for NPCs, retaining the additive user timing offset. Hits 1/2 retain 40 ms seeds. Hold duration and all other preferences remain. Player hit-5 queues identify trial=player_hit5_seed0; the existing NPC trial tag remains. This changes neither the NPC policy nor the Shadow farm.

The PVP (Players) toggle still controls player inputs; no settings file is rewritten to enable it. Auto Parry starts off on reload. Current/alive ownership checks, manual-F exclusion, cooldown gating, facing/LOS, optional visibility/knocked filters, typing/menu pauses, stop/release cleanup and bounded approach recovery remain. Approach recovery now applies only to permitted hits 1/2/5. No new animations or speculative attack predictions are introduced. The missed-parry penalty still motivates skipping low-confidence attempts; a skipped hit is an intentional trade-off, not proof the target stopped attacking.

**Diagnostics/UI:** Config, START and CUE_ACCEPTED report scope=hits_1_2_5 timingTrial=shared_hit5_seed0. Config retains npcSkip=hit3,hit4 npcHit5Seed=0 and adds playerSkip=hit3,hit4 playerHit5Seed=0. The base catalog's trialSeeds field is still reported alongside these explicit target-policy overrides. Combat status identifies v16 and the shared player/NPC hit selection. Automatic recording remains enabled whenever auto parry runs.

**Validation:** 154 mocked assertions pass: 36 shared-policy scenarios, 26 PvP/PvE ownership/filter scenarios, 39 bounded range-recheck scenarios, 47 logger/lifecycle scenarios and six settings/reload checks. Policy tests verify no inputs or pending approach cues for hits 3/4, unchanged hits 1/2 scheduling, immediate hit 5 for both targets, additive user offsets, PvP-off behavior, player hit-5 range recovery and cleanup. Older generic fixtures that used hit 3/4 merely to obtain a zero-delay eligible pulse now use hit 5; the dedicated policy tests cover the new exclusions. Source module and both staged artifacts compile, and both generated build variants are checked before installation. Exact-module transplantation onto the latest builds, file hash checks and remote refresh preserve concurrent changes, including Claude's farm and recorder work.

**Evidence boundary / handoff:** the user requested the policy transfer; NPC outcome rates in SP-031/SP-032 are not PvP validation. The live player effect is still unverified. Reload v16, enable PVP (Players) and Auto Parry when testing players, and inspect player_late_hit skips plus permitted hits and their outcomes. Keep NPC and player result analysis separate. Neither a counter-rate promise nor a new timing calibration follows from this change.

## SP-035 — Shadow farm and Auto Parry while tabbed out (2026-11-19, Claude)

**Request (user):** make the Shadow farm and Auto Parry work tabbed out.

**Source inspection:** Auto Parry stopped itself on `WindowFocusReleased` (a lifecycle choice; no note records VIM input failing while unfocused). The Shadow farm had no focus check. Whether Volt's VirtualInputManager presses still reach the game while Roblox is unfocused or minimized is **unknown**.

**Implemented / untested live:**
- Auto Parry: focus loss no longer stops it. `WINDOW_FOCUS lost/gained` is logged. While unfocused, NPC cues press F as before; player cues are skipped as `unfocused_player` (PvP behavior while tabbed out is not automated by this change).
- Shadow farm: logs `FOCUS lost/gained` with punch counts, `focused=` on every J press and POWER line, and five `AIM` lines (camera LookVector, mouse UnitRay direction, direction to the Shadow) at the start, to match the M1 request's third argument.

**Request shapes seen (parry captures, for a possible fallback):** `Requests.Action:FireServer("Defend", true/false)`; `("MeditationTrain")`; `("MouseButton1", false|true, <unit Vector3>)`. The M1 boolean's meaning and the vector's source are unknown, so no direct-request fallback is built.

**Test plan:** run both features about 5 min with Roblox behind another window (not minimized), then a few minutes minimized; then focus it again. Read per period: F_DOWN followed by DEFEND_REQUEST (F reached the game), M1 OUT lines while unfocused (punches reached the game), POWER rising, FRAME_GAP lines and counter lead (a throttled client may lose the ~160–180 ms lead). If presses are dropped, build a direct-request fallback for NPC cues and the farm only.

## SP-036 — "Parry is a little inconsistent": the Shadow lunges in from outside range 8 (2026-11-19, Claude)

**Evidence (Verified from captures):** v16 diagnostics, place 135537125271116.
- Shadow farm, histories 1795113532 / 1795115356 / 1795116270 (detail portions): hit 1 162 swings, 76 pressed, 51 counters, **57 skipped `out_of_range`**, 21 `counter_cooldown`; hit 2 98 / 85 / 61, 10 out of range; hit 5 **26 / 26 / 26**; hits 3–4 skipped by design (59). Footers: 261/380, 103/131, 116/156 counters; 0 damage events.
- Of the Shadow's hit 1/2/5 swings skipped as out of range, those starting **8–12 studs** away hit us anyway (self `Blunt` within 0.8 s) **54 / 57** times; from 12–16 studs, 0 / 6. The Shadow closes distance during the swing (it is pushed back by our punches, then lunges).
- Saiyan Grunt group fight (1795117059, 1795117443; 155 + 127 damage events, one death): pressed swings countered well (hit 1 5/8, hit 2 9/12, hit 5 4/6) but most damage came from hits 3–4 (19 hits, skipped by design), out-of-range starts (9 of 19 at 8–12 studs landed), counter cooldown (6), several attackers at once, and moves the parry does not map (~15 hits after unmapped animations such as `102483335944327`, `88670347816269`).

**Implemented / untested live — Combat page v16.1:** your own Shadow (`Name=="Shadow"`, `@Player` = you) uses range 12 at both observe and press time (`cueRange`). The approach recheck (fresh swing within Range+3 waits up to 80 ms for entry) now applies to NPCs as well as players; NPCs other than your Shadow still need range 8 at press time. Header config adds `npcRangeRecheck=true shadowRange=12`. Player behavior is unchanged.

**Expected:** more Shadow hit-1 presses and counters, fewer hit-3/4 follow-ups; check that `duration=5` (missed-counter) notices do not rise. Grunt groups stay limited by hits 3–4 and multiple attackers.

## SP-037 — NPC heavy attacks: identified; counter timing trial (2026-11-19, Claude)

**Request (user):** heavy attacks should be tracked.

**Evidence (Verified from 37 parry captures):** the Shadow, Thugs and Saiyan Grunts share one heavy: wind-up `88670347816269`, then release `102483335944327` (143 of 152 releases follow a wind-up within 0.6 s). The release track (length 0.75 s) is stopped at phase ~0.42–0.52 s; the attacker's `HitOut` comes a median ~462 ms after the release starts (301–731 ms; ~783 ms after the wind-up). Outcomes at impact: **141 Blunt (hit), 0 counters, 0 blocks** out of 152. F taps that happened to land 164–195 ms before a heavy's impact (12, all pressed for an overlapping hit-1/2 cue) gave 0 counters; at the same lead, hits 1/2 counter ~70%. Trello: M2 = heavy; red mark = guardbreak; skills cannot be countered. **User (2026-11-19): heavies can be countered — "perfect block or guard".** In-game tutorial text (Inspection description, captured earlier): "Counter is split into a counter parry for light attacks and perfect counter for heavies." Five self `Perfect Counter!` receipts in the captures came 46–139 ms after our Defend press (F held ~60–140 ms, a normal tap), against other heavy animations (`126361551838373` ×3, `74235292377889`, `102007255940679`) pressed ~250–480 ms after they started. Own `@PBlock` stays true a median 567 ms once set (meaning unverified).

**Implemented / untested live — Combat page v16.2:** NPC heavy releases are queued as cues (`trial=npc_heavy heavyDelayMs=260|320|380`, rotating) and press F at that delay after the release starts (expected ~200/140/80 ms before the median impact); cue expiry extends to delay+0.1 s for these. Normal gates apply (range, facing, LOS, counter cooldown, pulse gap). Player heavies stay unmapped. Header config adds `npcHeavyTrial=102483335944327@260/320/380ms`.

**Risk:** a failed counter starts the 5 s counter cooldown, which can cost the next hit 1/2. **Judge by:** `Perfect Counter!` receipts per delay on heavy cues, then keep the best delay. If none of the three works, try holding F through the impact (block) next.

## SP-038 — Saiyan Grunt groups: why parry is unreliable; counter window by lead (2026-11-19, Claude)

**Evidence:** history 1795118069 (127 s, 3 Saiyan Grunts, 46 damage events, 16 presses, 9 counters). Its header has no `shadowRange`/`npcHeavyTrial`, so it ran **v16 without SP-036/037** (the hub was not re-executed after those installs).
- Hit 1: 17 swings, 3 pressed (0 counters), **10 skipped out of range**, 3 attacker facing; 7 hits taken.
- Hit 2: 13 swings, 7 pressed at 164–199 ms lead → 3 counters, 3 hits, 1 block (other grunts hitting at the same moment is the likely cause; not attributable from these logs).
- Hits 3–4: 16 swings, skipped by design, **14 hits taken**. Hit 5: **6 / 6 countered**.
- Heavies unmapped in that build.

**Counter window by lead (all NPC presses, 37 captures; lead = attacker HitOut receipt − our press):** <100 ms 3/40 counters; 100–140 8/52; 140–160 17/34; 160–180 70/106; 180–200 301/419; 200–230 36/52; **230–300 5/6**. A press needs ≥ ~150 ms of lead; the early edge is not found yet (few presses above 230 ms).
- Past hit 3/4 presses (pre-v15): 85, 11 counters; 52 ended `Blunt` + 5 s counter cooldown. `BlockEffect` never coincided with a 5 s cooldown.

**NPC combo rhythm (same actor, start of hit N → impact of hit N+1, p10/median/p90 ms):** hit 2 → hit 3: Shadow 651/701/801, Thug 654/717/849, Grunt 665/718/967; hit 3 → hit 4: Shadow 534/585/651, Thug 493/589/642, Grunt 518/582/735.

**Why groups are hard:** hits 3–4 are seen ~85–116 ms before they land (less than the ~150 ms a counter needs at ~100 ms ping); any failed press starts a 5 s counter cooldown, during which every grunt's combo lands; several grunts swing at once.

**Possible next trial (not built):** press hits 3–4 ahead of time from the previous hit's start (e.g. hit 3 at hit-2 start + ~470 ms, ~230 ms lead at the median), only when the previous hit was not pressed. Needs the early edge of the window first.

## SP-039 — Grunt run on v16.2: heavy trial never pressed (fixed in v16.3); hits 3–4 not predictable for grunts (2026-11-19, Claude)

**Evidence (Verified from capture):** history 1795118462 (196 s, 4 Saiyan Grunts, v16.2 header with `npcHeavyTrial`): 32 presses, 23 counters (72%), 55 damage events (0.28/s; the v16 run 1795118069 had 56% and 0.36/s against 3 grunts — slightly better, not conclusive).

| Hit | Swings | Pressed | Counters | Hits taken | Main skips |
| --- | --- | --- | --- | --- | --- |
| 1 | 29 | 10 | 4 | 10 | out_of_range 9, attacker_facing 4 |
| 2 | 23 | 12 | 9 | 11 | counter_cooldown 3, range 4 |
| 3 | 12 | 0 | 0 | 12 | npc_late_hit (by design) |
| 4 | 12 | 0 | 0 | 11 | npc_late_hit (by design) |
| 5 | 11 | 10 | **10** | 0 | — |

- **Heavy trial:** 4 heavy cues queued (260/320/380/320 ms), **0 pressed**: 3 `track_stopped`, 1 `counter_cooldown` (0.17 s left). The release is first seen at phase 0.30 and the track normally stops ~0.1–0.2 s later, before the press; the hit still lands 0.38–0.67 s after it is seen. This run says nothing about perfect-counter timing.
- **Approach recheck:** 8 RANGE_WAIT, 5 RANGE_ENTERED → presses gave 3 counters, 1 block, 1 empty.
- **Failed presses and the 5 s cooldown (4 grunt runs, 75 presses):** 50 counters; 12 `Blunt` with no 5 s cooldown; 8 empty presses, 2 of them followed by `Counter duration=5`. The 5 s lockout comes from some empty presses, not from every failure (refines SP-031's wording for grunt fights).
- **Hit-3 predictability (impact receipt of hit 3, p10–p90 spread):** from hit-2 start: Grunt 302 ms, Thug 195, Shadow 150; from hit-2 HitOut: Grunt 169 (235 within 6 studs), Thug 141–160, Shadow 119–132; from hit-2 ANIM_STOP: 133–548. **No reference is tight enough for grunts** (go threshold ~120 ms), so a predictive hit-3 press is not built. Hits 3–4 remain a hard limit against grunts at ~100 ms ping.

**Implemented / untested live — Combat page v16.3:** heavy cues are exempt from the `track_stopped` gate, expire at 0.48 s, and may defer through the 0.5 s post-counter cooldown if it ends by 0.38 s after the release was seen. Analysis script note: count `Perfect Counter!` as a heavy outcome.

## SP-040 — Job Recorder (2026-11-19, Claude)

**Request (user):** an auto job recorder (record jobs so Auto Job can be built).

**Trello (player wiki, not verified in game):** jobs are taken from NPCs — Tilent (Cafe Shop, Central City) = Food Industry; Rosa (Capsule Corp), Auber (Vegeta, Celer Village), Bink (Eter, Sapian City) = Technological Industry; Sebastian (Central City) = Environmental Industry. Level 1 tasks: Clean Spills / Capsule Minigame 1 (click a button as fast as you can) / Scrub Dirt; level 2: Deliver Groceries / Capsule Minigame 2 (click fast for 60 s) / Collect Garbage; level 3: cook (spin the mouse) / osu-style minigame / Find Lloyd Cat. 50 / 100 / 300 Zeni per job; ~400 jobs to level 2 and ~800 more to level 3. Hard Worker doubles job EXP; Charismatic doubles job money.

**Implemented / untested live — Recorders → Sparkling Job Recorder (`src/SparklingJobRecorder.lua`):** read-only, up to 10 min / 20,000 lines, saves every 5 s to `sparkling_job_latest.txt` plus `sparkling_job_<unix>.txt`. Records: every ReplicatedStorage RemoteEvent IN (StopDash/Effects/Cooldown/CooldownNT limited to 2/s each), OUT/RETURN through the shared hooks (refused while Auto Parry owns them), proximity prompts (shown/hold/triggered with action/object text and position), mouse clicks with the 3D target and the screen objects under the cursor, keys (not while typing), visible on-screen text/buttons (new, text and visibility changes), job-looking world objects and markers (BillboardGui/Highlight/Beam/ProximityPrompt/ClickDetector) added or removed, money/job/level values and attributes on you and your character, and your position every 2 s.

**Next:** user records one job type (a few tasks); then identify how a task is given, where it is, what completes it and what pays.

**Result — first three job recordings (user, 2026-11-19, histories 1795119155 / 1795119261 / 1795119578; Verified from captures):**
- **Take:** ProximityPrompt `Workspace.NPCs.Thlentil.HumanoidRootPart.Quest` ("Take Quest", at about (−221, 100, −37), the Cafe in Central City). A `Mission` ScreenGui appears: "Floor Staff/ [1]", "Go to Location and wipe the spills of the floor", "Pay: $200", "Time: 299s", "Progress: 0/10". `Requests.QuestMarker:InvokeServer("Job")` returns the job location (−206.2, 102.9, −38.3).
- **Task:** one prompt `Workspace.Quest.Spills.Spill.Spill` ("Spill", hold 0) that moves to a new spot after each wipe (about 10 fixed spots around x −199…−224, y 95.4, z −13…−58). Triggering it sends **no remote** (engine prompt); progress rises ~2.2 s later.
- **Finish:** at 10/10 the client itself sends `Requests.Quest:FireServer("Reward", PlayerGui.Mission)`. Quit sends `("Cancel", PlayerGui.Mission)`.
- **Glitch (user report + capture):** after the Reward request the server never answered — no remote, no `DataUpdated`, the Mission window stays at 10/10 with its timer running, and the spill prompt keeps appearing (run 1: user quit with Cancel ×5; run 3: stuck 90 s). The inventory label read Zeni 420 in runs 1–2 and **620 in run 3**, so one +200 payout happened between recordings.
- **Employment:** in run 2 the user spoke to `Workspace.NPCs.Tilent` ("Speak") → "You accepting people for jobs?" → "want to work here?" → "Sure" (`Requests.Dialogue:FireServer({exit=false, choice=...})`). Trello says Tilent gives the Food Industry job; the "Floor Staff" quest comes from a different NPC, Thlentil. Whether pay needs this employment is **unknown**.
- Recorder fix (v1.1 source): `DataUpdated` was truncated at 24 fields, so Zeni was never captured; money/job/quest fields of incoming data are now logged as `VALUE DATA`.

## SP-041 — Bandit (Thug) fight: own attacks lock the counter; NPC range 12 for all (2026-11-19, Claude)

**User report:** "too inconsistent… letting too many hits stack before it even thinks of parrying… even though I'm fighting multiple bandits… they attack one at a time."

**Evidence (Verified from capture):** history 1795120360 (149 s, 3 Thugs, user fighting manually; v16.3): 35 presses, 20 counters, 74 damage events, 71 `Blunt`.

| Hit | Swings | Pressed | Counters | Hits taken | Main skips |
| --- | --- | --- | --- | --- | --- |
| 1 | 43 | 16 | 7 | 15 | out_of_range 17, counter_cooldown 4 |
| 2 | 27 | 12 | 6 | 18 | out_of_range 5, attacker_facing 4 |
| 3 | 16 | 0 | 0 | 13 | npc_late_hit (by design) |
| 4 | 12 | 0 | 0 | 12 | npc_late_hit (by design) |
| 5 | 9 | 7 | 7 | 2 | — |

- **Your own attacks block the counter:** hit 1/2 presses with your own M1 sent < 150 ms before: 0/5 counters; 150–300 ms: 0/5; 300–500 ms: 2/3; 500–1,000 ms: 5/8; > 1 s: 5/7. Same pattern as the Shadow farm (SP-032). The pressed hit-2 failures (7 `Blunt` at 167–200 ms lead) are mostly this.
- **Heavies:** 7 heavy cues, all skipped `counter_cooldown`: each time the bandit started its heavy release **at the moment your own combo ended** — `CooldownNT` Counter 0.5 s and ZVanish 2 s arrive ~0.5 ms before the release is seen, right after your last `HitOut`. Their heavy then landed 218–733 ms after it was seen.
- **Out of range:** Thug hit 1/2/5 swings started 8–10 studs away landed 6/9, 10–12 studs 5/7 (they lunge, like the Shadow).
- `Counter duration=5` notices: 3 in the run.

**Implemented / untested live — Combat page v16.4:** every NPC uses range 12 (another player's Shadow stays at 8); header config `npcRange=12`. Heavy cues may defer through the counter cooldown until 0.62 s after the release is seen (expiry 0.65 s). Players unchanged.

**Hard limits (game rules, not fixable by timing):** a counter cannot happen within ~300 ms after your own M1, nor during the 0.5 s counter cooldown that follows your own finished combo; NPC hits 3–4 land ~0.1 s after they appear.

**Result — all nine job recordings (Verified from captures):** jobs **do pay**. The inventory Zeni label at the start of consecutive recordings reads 420 → 420 → 620 → 820 (Floor Staff) and 39 → 239 (Scientific Engineer): **+200 per completed job**. The "glitch" is that the job window does not close after the payout (Floor Staff stays at "Progress: 10/10"; the minigame ends without closing); pressing **[ Quit ]** (`Requests.Quest:FireServer("Cancel", PlayerGui.Mission)`) clears it so the next job can be taken. A `RestartMission` GUI ("Restart Quest") also exists.
- Jobs can also be taken with `Requests.Quest:FireServer("QuestBoard")` (sent by the game when the user clicked the quest board).
- **Scientific Engineer [1]** (Tech, place 102793290484692; board `Workspace.Map.Model.Model.Paperworks.Part.Quest` at (1465, 2133, −203), NPC `Hum` nearby): Mission "Help the company to develop a new system", "Progress: 0/1", Pay $200. Prompt `Workspace.Map.Model.Model.Laptop1.Bottom.CapsuleMinigame1` ("Capsule Minigame") opens `CapsuleMission1`: "Time: 60s (Aim for over 50)", "Counter: N", and a key to press (`Button.TextLabel`, "A" both times). Each key press raises the counter (user reached 241 and 117); from counter 50 on, each press also sends `Requests.Quest:FireServer("Increment", PlayerGui.CapsuleMission1)`. The game also sent `Cancel` a few times mid-minigame without a click (cause unknown).
- Rate: Floor Staff ≈ 36 s of wiping + quit/retake for $200; Scientific Engineer 60 s for $200.

## SP-042 — Auto Job v1: Floor Staff + Scientific Engineer, looped (2026-11-19, Claude)

**Request (user):** Auto Job for both jobs; after a payout, quit the stuck window and take the job again (user answers to design questions).

**More evidence from the recordings (Verified from captures):** after a job, a `PlayerGui.RestartMission` box appears ("Restart Quest", `[ Yes ]` / `[ No ]`, ~6 s countdown); clicking Yes makes the game send `Requests.Quest:FireServer("QuestBoard")` and a fresh job window appears. `[ Quit ]` on a finished window does not always close it: in history 1795120008 the user pressed it 37 times over 19 s and the window stayed. The Floor Staff window has a 300 s timer; Scientific Engineer's reads 9999 s.

**Implemented / untested live — Auto → Auto Job (`src/SparklingAutoJob.lua`):** uses `ProximityPrompt:InputHoldBegin/End` (90 ms tap, the Beyond Nen Auto Chests precedent) and VirtualInputManager key/mouse input; sends no requests itself.
- No job window: click Restart Quest `[ Yes ]` if shown, else walk to the nearest "Take Quest" prompt (150 studs) and trigger it.
- Floor Staff: walk to the nearest "Spill" prompt under `Workspace.Quest.Spills`, trigger it, wait for progress (or 3.5 s), repeat.
- Scientific Engineer: trigger the "Capsule Minigame" laptop prompt; while `CapsuleMission1` is open, tap the key it shows at 5/s (walks back if the key drifts the character over 5 studs from the laptop).
- Finished (a/b reaches b, or the minigame closed): wait 2 s for the pay, click Restart `[ Yes ]` if shown, else `[ Quit ]` up to 8 times (1.5 s apart); then wait for the window's timer (stops if it reads over 600 s). A job without progress for 90 s is quit.
- Clicks are refused while the hub window covers the button. Pauses for chat/menu; stops on death, character change, teleport or unload. Log: `sparkling_auto_job_latest.txt` (TAKE/TASK/PROGRESS/DONE/QUIT/RESTART lines and the inventory Zeni label).

**v1.1 (same day, before any live run; review fixes):** prompt scans limited to twice a second with a 2 s cache (v1 walked the whole Workspace every frame); "Take Quest" accepted only from `Workspace.NPCs.Thlentil` or `Paperworks` (other quest givers share the action text); the minigame key is resolved with `pcall` (digits mapped to `One`…`Nine`; an empty label waits) — v1 indexed `Enum.KeyCode` directly, which errors on a bad name; the whole step runs in `pcall` and stops with the error logged. The minigame is treated as finished 65 s after it opens or when its counter stops rising for 3 s while tapping (its closing was never recorded). Clicks use the ScreenGui's `IgnoreGuiInset` and log button geometry. New log lines `RESTART_BOX shown/gone` and `WINDOW gone sinceDone=… lastAction=…` record what actually clears a finished window. Job Recorder now logs `GUI SCREEN_REMOVED`.

**First live run, v1.1 (user, 2026-11-19, `sparkling_auto_job_latest.txt`; Verified from log):** Scientific Engineer ran four jobs; the inventory Zeni went 239 → 439 → 639 → 839 (+200 each). The job completes at counter 50: progress turns 1/1 about 10.5 s after the laptop prompt (taps=50–51 at 5/s), the Mission window disappears 0.3 s later, and the Restart Quest box appears at the same moment. **Restart Quest `[ Yes ]` was not pressed by the VirtualInputManager click** at the button's centre (button (1547,710) 97×27, `IgnoreGuiInset=true`, inset 58; click (1596,723)): 7–10 clicks per box, the box then went away on its own (~9 s) or by the user's own click. The minigame key changes per job (D, S, A, W). User: "it doesn't do the restart quest, I had to press it myself."

**v1.2 (implemented / untested live):** button presses rotate through: click at the centre, centre −inset, centre +inset, and the button's own handlers via `getconnections` (`Activated`, `MouseButton1Click`, `MouseButton1Down`). If the box disappears within 1.2 s of a press, that method is kept for later presses and logged as `PRESS_WORKED method=…`. Each press logs its method and geometry.

**v1.2 live run (user: "working perfectly"; Verified from log):** `PRESS_WORKED method=click+inset` on the first box. VirtualInputManager clicks need the top-bar inset added even for an `IgnoreGuiInset` ScreenGui (Restart `[ Yes ]` at (1547,710) 97×27 was pressed by a click at (1596,781), inset 58). Then **62 Scientific Engineer jobs in ~12.4 min** (one every ~12 s, ≈ $1,000/min), every restart pressed on the first try. The log's `zeni=` field read 1,1,…,13 — the inventory label groups thousands (inferred: the read value went up by 1 every 5 jobs = $1,000), so v1.2 read only the digits before the comma.

## SP-043 — Shadow farm arm-injury stop; Auto Job money target (2026-11-19, Claude)

**Request (user):** stop Auto Shadow Farm when either arm reaches 90% injury; option to stop Auto Job at an amount of money, 0 = forever.

**Implemented / untested live:**
- **Auto Shadow Farm:** stops when `Character.LeftArmInj` or `RightArmInj` (SP-033) is ≥ 90 ("Left/Right arm at N% injury"); POWER lines now log `arms=L/R`. The 0–100 scale is assumed from the HUD.
- **Auto Job v1.3:** "Stop at Zeni (0 = never)" box, saved to `sparkling_auto_job_target.txt`; checked once a second against the inventory label, now parsed with thousands separators and a k/m suffix. The exact label format above 999 is inferred, not captured.

## SP-044 — Visuals: Zeni row (2026-11-19, Claude)

**Request (user):** add Zeni to Visuals.

**Implemented / untested live:** Visuals → Profile has a **Zeni** row after Battle Power. For you it reads the inventory label (`PlayerGui.Inventory…infos.hh`, "Zeni:N", which updated live as Auto Job paid), parsed with thousands separators and a k/m suffix; fallback the profile snapshot's `Zeni`. For another player it shows a `Zeni` value or attribute on their character if one exists (none has been seen in captures), otherwise "Hidden (only shown for you)".

## SP-045 — Visuals: Race Variant confirmed; Limit Break Progress reworked (2026-11-19, Claude)

**User:** remove the (?) for Traditional (Race Variant) and for Limit Break Progress.

- **Race Variant:** `Character@Trait` (e.g. "Traditional") confirmed by the user as the race variant; (?) removed. **User-confirmed.**
- **Limit Break Progress:** Trello (Limit Breaking): BP progress stops at **100k / 250k / 450k / 700k / 1M** until a bar on the right of the screen is filled by fighting (faster against players close in power). Profile Scan: `Character@Limit` is 0 or 1 across players, read as breaks done (inference from Trello + values, not confirmed); `Character.LimitBreak` was 0 for every player scanned and is read as that bar's value (maximum unknown). The row now shows e.g. "0 breaks done · next wall 100k (40,267 BP to go)", or "at the 100k wall, bar N" once BP reaches the next wall. Implemented / untested live.
- **Next:** capture `LimitBreak` while filling the bar at a wall to learn its maximum.


## SP-046 — other-player Visuals stats and broader Profile Scan (2026-11-19)

**Request:** find other players' stats so the Visuals tab contains useful data instead of mostly unavailable profile rows. Refreshed the publishing checkout to include Claude's latest Zeni/race-variant/limit-break work (SP-044/SP-045); exact module replacement preserves those fields and unrelated modules.

**Existing evidence re-read:** local sparkling_esp_data_latest.txt contains multiple player characters with Ki and Stamina NumberValues and their explicit Max attributes, plus character Form and GodKi attributes. Examples of observed shapes include Ki 200.72 / Max 441.6 and Stamina 139.95 / Max 139.95; these are source examples, not current values for anyone. The Profile Scan has four other-player sections, including one direct TempStrength=50. The same scan contains nested AttackerTags entries with Strength belonging to an attacker and trainer/NPC template stats under ReplicatedStorage. Neither is a source for the selected player's allocated Strength. The hidden-memory scan contains local profile snapshots, without verified ownership for another player's allocated stats.

**Implemented / untested live — Visuals v5:** add Health, Ki, Stamina, Shield, Output, Fatigue, Injuries, Form ID, God Ki, Current Area and Temporary Stats rows. Each reads only the selected current character. Health uses Humanoid.Health/MaxHealth; Ki/Stamina/Shield use their ValueBase.Value and Max attribute, Output uses MaxOutput. Unknown/nonpositive maxima show / ? rather than being inferred from the current value. Numeric zero and boolean false remain valid. Non-finite values are omitted. The form is explicitly an ID because a trustworthy ID-to-form-name map has not been established.

Other-player Ki/Stamina/Form/GodKi and TempStrength have capture evidence. Shield/Output current values, Fatigue, injury values and CurrentArea were observed on the local character; the new rows show them on another character only if actually present there. Their availability for every remote player is not established. Missing condition fields are not filled with zero, inferred from battle power or copied from the local profile. Injury values are displayed as raw named values; no unverified percent scale is added.

For the allocated Stats row, other players are checked for exact direct character ValueBases/attributes named Strength, CoreStr, Concentration, Reactions, Agility and Mind. These keys match the known stat vocabulary, but captures have not yet established full allocated-stat replication for other players. Only existing finite numeric fields display. Conflicting direct-value/attribute sources are labeled instead of silently choosing one. Temporary Stats independently checks the corresponding Temp-prefixed fields; the observed TempStrength is never treated as allocated Strength. No recursive search through attacker tags, NPC/trainer templates, another player's objects or local profile memory supplies remote stats.

**Layout:** other-player rows that resolve to missing/own-only data collapse, with a compact availability note below the remaining rows. Own-profile rows retain their existing resolvers. Available rows reflow every half-second while Visuals is open; their labels/values and section height move together. Temporary Stats and Injuries have taller wrapped rows. Character loss clears stale resource rows; respawn and player selection re-read the current character. The picker, Zeni source, race variant and limit-break implementation remain.

**Profile Scan v2:** existing Recorders -> Run Profile Scan now includes bounded values/attributes for other players and their characters, rather than dropping fields based on keywords. Other-player roots retain the 1,500-object bound; local Player/Character bounds remain 4,000/3,000. PlayerGui/PlayerScripts/Backpack are excluded from player-root traversal. ReplicatedStorage stays keyword-filtered. Report stays within 6,000 lines and now replaces its last line with an explicit LIMIT/dropped-count marker if full. Reports remain local. The scan is read-only and sends no game requests; the existing Hidden Value Scan and CEXP Finder are preserved. This broader scan can reveal previously filtered fields but cannot make server-only data available.

**Validation:** 42 mocked assertions pass for selected-player source isolation, current/max resources, missing/zero maxima, zero/false values, NaN rejection, temporary versus allocated stats, conflicting sources, partial injuries, contiguous row positioning and section height, respawn/departure, own-profile preservation, non-keyword capture coverage, report bounds and read-only operation. Source and both staged builds compile; both regenerated build variants are checked before installation. No live UI interaction or live capture was performed. Live remote condition-field availability and visual rendering remain unverified; the tests validate source resolution and layout calculations.

**Handoff:** reload, open Visuals and select a player. Known replicated resources should fill the view, with conditional fields displayed only when available. If allocated stats remain unavailable, use the upgraded existing Run Profile Scan while other players are present; review exact ownership/path and capture limits before adding mappings. Do not infer base stats from Temp fields, trainer data, attacker tags or battle power. CEXP, full mastery, personality and emotion for others remain unavailable from reviewed evidence. Claude's other Visuals and game features remain intact.


## SP-047 — Profile Scan v2 confirms remote condition/temporary stats (2026-11-19)

**New evidence:** the user ran the upgraded Profile Scan. The completed local sparkling_profile_scan_latest.txt is v2, place 102793290484692, universe 10771972206; it contains the local player and two other players, ends with END and has no LIMIT marker. This validates the broader capture in this run; it does not independently validate the rendered Visuals UI.

Both other characters expose direct Fatigue, all six injury values, Shield with Max, Output with MaxOutput, Hunger, Focus, OverCharge with Max, and CurrentArea in addition to the previously established Ki/Stamina/PowerLevel and race/form fields. Thus the remote-condition-field availability left open in SP-046 is now observed for these two characters. CurrentArea is the literal string None for both; do not infer an actual area from it. One character has Output about 95.3 with MaxOutput=90 and nonzero OverCharge: preserve raw current/max readings without clamping or assigning a causal interpretation.

**Temporary versus base stats:** one remote character has direct TempStrength, TempCoreStr, TempConcentration, TempReactions and TempAgility, each 30. The other has TempCoreStr and TempConcentration, each 50. The existing v5 Temporary Stats resolver already covers these exact names. No allocated base Strength/CoreStr/Concentration/Reactions/Agility/Mind value or attribute was found on either player/character. Nested AttackerTags entries contain Strength=30000 on NPC/trainer tags; those belong to the tagged attacker, not the inspected player's base stats, and remain excluded. Absence in this bounded successful scan is not proof that every server/private data path has been exhausted.

**Implemented / display untested live — Visuals v5.1:** add optional Hunger, Focus and Overcharge rows using the newly observed direct character values. Hunger and Focus remain raw values; no unverified percentage or maximum is invented. Overcharge uses the explicit OverCharge.Max attribute and displays / ? if unavailable. Existing compact-row layout, own-profile behavior and the separation between temporary and allocated stats remain. Other-player condition rows added in SP-046 require no new mapping and will populate when those fields exist on the selected character.

**Validation:** 53 mocked assertions pass, extending the v5 source-isolation/layout/respawn/scan suite with missing and present new fields, over-max Output retention, and the full observed temporary-stat set without leakage into base stats. Source module and staged public/private builds compile; both regenerated variants are checked before installation. Runtime change is three optional resolvers plus a version comment, installed by exact-module replacement with file-hash checks. No stat writes, gameplay requests or live UI actions were performed. Raw captures/player identifiers remain local; the public note contains only sanitized source findings.

**Handoff:** reload Visuals v5.1 to see Hunger, Focus and Overcharge alongside the existing remote resources and condition fields. Do not reinterpret temporary boosts or attacker-tag Strength as allocated stats. Full base allocations, other-player CEXP, personality, emotion and mastery remain unavailable from reviewed evidence. Another unchanged scan is not needed to establish this result; a future mapping needs a new concrete source.


## SP-048 — Visuals cleanup, injury threshold, Zeni fix and Race Potential correction (2026-11-19)

**User request:** remove Shield, Overcharge and Hunger, show only injuries at 70%+, investigate unavailable Zeni, and explain Latent Power. Implemented in Visuals v5.2. Removed rows are absent from the profile field list. Injuries include only finite raw values >=70, formatted with a percent sign as requested; the threshold is applied before display rounding, so 69.99 does not appear as 70%. If no injury qualifies, the optional row collapses. No underlying character values change.

**Zeni source bug found and fixed:** the previous expression assigned digits,suffix from a logical-and chain ending in string.match. Luau adjusts the logical expression to one result, leaving suffix nil even when the numeric label matched. suffix:lower() then threw inside the row resolver, and the general protected UI update displayed the missing/unavailable fallback. The fix assigns both captures directly inside an if, strips rich-text markup and safely handles an empty suffix. Plain amounts, commas, zero, k/m (either case), carried-value fallback and rich text are tested. This repairs the local inventory-label path; it does not expose other players' private balances. Other-player Zeni still displays only when an explicit selected-character value/attribute exists and otherwise remains in the availability note. The code bug is confirmed; the user's exact viewing target at the time of the report was not established.

**Latent Power correction:** the old row was reading Character.RacePotential and guessing that it represented latent power, marked with (?). That equivalence has not been established by the captures. The local guide capture describes a latent-power mechanic, but does not map it to this value. v5.2 removes the misleading Latent Power row and replaces it with optional Race Potential, displaying the actual finite numeric field without an inferred x multiplier. Direct numeric formatting preserves observed values such as 0.85 and 1.15, which the general one-decimal formatter previously rounded. The footer states that Race Potential is not confirmed latent power. Actual latent power remains unresolved; do not restore the alias without evidence.

**Validation:** 59 mocked assertions pass, including the existing selected-player source/layout/scan checks and new boundary/nonfinite injury cases, removed-row checks, Zeni parsing/fallback/source isolation, and RacePotential precision. Source and staged public/private artifacts compile; both regenerated variants are checked before installation. Exact-module replacement with source/public/private hash checks preserves concurrent work and gameplay behavior. No live UI validation was performed. No new capture is required for the confirmed parser fix or requested filtering; reload the installed build to see them.

## SP-046 — Shadow training log, all runs so far (2026-11-19, Claude)

**Request (user):** take note of all the Shadow trainings done.

**Sources:** Auto Parry diagnostics histories that saw `Workspace.Live.Shadow` (PowerLevel from sampled `character.PowerLevel`; long sessions hit the 12,000-line detail cap, so their last sample is early and the end value comes from the next session's start or the farm log), and `sparkling_auto_shadow_latest.txt` (only the most recent farm run is kept). Times are local.

| Run (11-19) | Length | Power Level | Parry build | Shadow presses → counters | Notes |
| --- | --- | --- | --- | --- | --- |
| 01:17–01:40 (4 short sessions) | ~4 min | 35 → 2,187 | v14 | 36 → 15 | Manual fights before Auto Shadow Farm existed |
| 01:50 + 01:58 | ~39 min | 2,202 → 23,634 | v15 | 640 → 422 | First Auto Shadow Farm run (SP-032); 1 kick-out |
| 02:34 | ~26 min | 23,725 → ~35,000 | v15 | 432 → 304 | Steady 430–490 PL/min |
| 11:38 | ~23 min | 37,990 → ~48,680 | v16 | 380 → 261 | End value = next session's start |
| 12:09 | ~7 min | 48,697 → ~51,800 | v16 | 131 → 103 | |
| 12:24 | ~9 min | 51,807 → 55,578 | v16 + SP-035 | 156 → 116 | **Tabbed out for part of it:** 323 of 1,045 punches while unfocused; PL rose ~480/min in the unfocused stretch, same as focused. 1 kick-out. |

**Totals on 11-19:** about **104 minutes** of farming, Power Level **2,202 → 55,578 (+53,400, ~510/min)**; Auto Parry against the Shadow countered **1,206 of 1,739** presses (69%), 0 damage events in the farm runs. Kick-outs seen: 2 (one per logged run; the other runs' farm logs were overwritten).
- Earlier "Shadow" sightings on 11-17 (PL 14k–25k, 6 sessions) were a different character/setup and are not counted.
- **Tabbed-out test (SP-035) — Verified from logs for a background (not minimized) window:** punches and PL gain continued while unfocused. Minimized is still untested.
- Farm AIM samples (SP-035): the first five punches logged camera, mouse and Shadow directions; not yet matched against the M1 request vector.

## SP-047 — CEXP found: Varrik's dialogue (2026-11-19, Claude)

**Evidence (CEXP Finder capture, user at Varrik, place 102793290484692; Verified from capture):** talking to `Workspace.NPCs.Varrik` sends `Requests.Dialogue` IN `{msg="Hm. You've got the look of someone who's been in a few fights…", choices={"Can you check my combat experience? (5000 Zeni)","Never mind."}}`. Choosing the first sends `Requests.Dialogue:FireServer({exit=false, choice="Can you check my combat experience? (5000 Zeni)"})`; the server answers 83 ms later with `{msg="You have 1808 combat experience. Not bad. You're getting there.", choices={}}`. **The user's CEXP was 1,808** at 14:42. CEXP is not in the client profile and arrives only as this dialogue text; each check costs 5,000 Zeni.

**Implemented / untested live:** Visuals listens to `Requests.Dialogue` and, on "You have N combat experience", stores N with the time in `sparkling_cexp_value.txt` (seeded with this capture's 1,808). The CEXP row shows e.g. "1,808 (Varrik, 12 min ago)", or "Ask Varrik (5,000 Zeni) to read it" when nothing is stored. It never asks Varrik itself.

## SP-048 — CEXP tracker: what earns CEXP (2026-11-19, Claude)

**User:** "you're supposed to be recording how it's tracked" — i.e. what raises CEXP, not just the Varrik reading.

**What is known:** Trello (Battle Power & CombatEXP): CEXP comes from "queues, fighting and quests"; a completed queue gives 100 (200 for Battle type); every 10k CEXP gives a hand of talent cards (up to 200–250k); dying in Pietra costs 10% of CEXP and PL; weights add 10–50% CEXP gain; racial traits (Intellectual, Combat Intelligence, Forced Perfection, Monstrous Evolution) raise it. The client never holds the user's CEXP (SP-V010, SP-047); a client table keyed `CEXP` holds leaderboard handlers (`PowerLevel`, `CEXP`, `Zeni`, `EventWins`, `DragonBall`, `Spins`, `Capsule`, `Namekian`), not the user's value. **Which fights give CEXP and how much is unknown** — e.g. whether the Shadow farm (104 min, +53k PL before the 1,808 reading) gives any.

**Implemented / untested live — `src/SparklingCexpTracker.lua` (no UI):** counts, between Varrik checks: hits landed / taken (self `HitOut` / `Blunt`), counters and perfect counters, seconds in your Inner World (own Shadow within 400 studs) and hits landed there, seconds in other fights (a hit within 5 s), Power Level gained, Zeni earned and jobs paid (+200 steps), deaths. Counters persist in `sparkling_cexp_counters.json`. Each Varrik answer appends one line to `sparkling_cexp_history.txt` (time, CEXP, change since the previous check, PL, Zeni, minutes, all counters) and resets the counters. History seeded with 14:42 CEXP=1808.

**Test plan:** check Varrik, do one activity only (e.g. 15 min Shadow farm), check again; repeat for NPC fights, jobs and a queue. Each check costs 5,000 Zeni.

**Result — second Varrik check (user, 2026-11-19 15:25; Verified from `sparkling_cexp_history.txt`):** CEXP **1,808 → 1,808 (no change)** over 43 min. In the tracked last 18 min: 16.7 min in the Inner World, 943 hits landed (all on the Shadow), 238 hits taken, 205 counters, +18,999 Power Level, no other fights, no jobs. **The Shadow farm gives Power Level but no CEXP.** CEXP must come from other sources (Trello: queues, fighting other players/NPCs, quests) — next checks should isolate NPC fights, PvP and a queue.

## SP-049 — Warrior Clan Rogue combo mapped (2026-11-19, Claude)

**User:** "new animations for AP."

**Evidence (Verified from capture):** history 1795127425 (97 s, two `Warrior Clan Rogue` NPCs, v16.4): 1 press, 52 damage events, 426 `unmapped_animation` skips — their combo uses different animations. Same-actor order: **hit 1 `78875817992139` → hit 2 `120572377399879` → hit 3 `119528335719333` → hit 4 `92958519453859` → hit 5 `80105885088261`** (A→B 450 ms, B→C 583, C→E 515, E→D 384 median), all seen at phase 0, speed 1.15. Attacker `HitOut` after the swing is seen (median, min): hit 1 117 (102), hit 2 201 (183), hit 3 151 (149), hit 4 84 (84), hit 5 270 (266). They also use the shared heavy (`88670347816269` → `102483335944327`). The user's own manual presses produced 7 Counter Parry and 1 Perfect Counter in the run.

**Implemented / untested live — Combat page v16.5:** for NPCs, rogue hit 2 and hit 3 press at once (~200 / ~150 ms lead), hit 5 after 80 ms (~190 ms lead); hits 1 and 4 are skipped as `npc_late_hit` (too fast at ~100 ms ping, like the standard combo's hits 3–4). CUE lines carry `style=rogue hit=N`; header config adds `npcRogue=hit2,hit3@0,hit5@80ms;skip1,4`. Players using this style stay unmapped (`rogueStyle=player`). Hit 3's ~150 ms lead is at the edge of the window; expect ~50% there.

## SP-050 — Rogue fight on v16.5; rogue heavy mapped; heavy defer cut; CEXP counter reload fix (2026-11-19, Claude)

**Evidence (Verified from capture):** history 1795127683 (150 s, two Warrior Clan Rogues, v16.5, user fighting manually): 27 presses, 15 counters, 43 damage events.
- Presses: rogue hit 2 **9 / 13** counters, hit 3 **3 / 7** (its ~150 ms lead is at the window's edge), hit 5 **3 / 4** (plus 1 block); shared heavy 0 / 3.
- Hits taken by source: rogue **hit 1: 18** (skipped, lands ~117 ms after it is seen), rogue heavy `126361551838373`: 5 (unmapped), hit 4: 4 (skipped), pressed hits 2/3 that failed: 8, cooldown/pulse/facing skips: 7, a Ki explosion `112088711297135`: 1.
- **Rogue heavy `126361551838373`** (length 0.7, knockback): our `Blunt` 0.417–0.435 s after it was seen, in all five cases. Earlier captures hold five self `Perfect Counter!` receipts against it with F pressed 0.25–0.37 s after it started (SP-037 list).
- **Shared heavy after your combo:** in all four presses the NPC started its heavy at the moment the user's own combo ended (`CooldownNT` Counter 0.5 s arrives with the user's last `HitOut`). The v16.4 defer then pressed at ~0.50 s, after the hit had landed (0.25–0.53 s). Same pattern as the bandits (SP-041).

**Implemented / untested live — Combat page v16.6:** rogue heavy pressed at a fixed 0.30 s (~120 ms before its hit; NPCs only; header `heavy@300ms`). Heavy cues defer through the counter cooldown only if it ends by 0.40 s (was 0.62), expiry 0.45 s.

**CEXP tracker fix:** v1 lost its saved counters on every hub reload (`local got,data=ok and pcall(...)` keeps only pcall's first result), so the counts restarted at 15:34 when the hub was re-run; the rogue fight at 15:30–15:32 is missing from the next history line. Fixed; counters now carry over reloads.

## SP-051 — Rogue heavy: 5/5 Perfect Counters; facing exemption; the shared heavy is two hits (2026-11-19, Claude)

**Evidence (Verified from captures):** histories 1795128100 (11 min, 6 rogues + 2 players) and 1795128862 (9 min), v16.6.
- **Rogue heavy `126361551838373` pressed at 0.30 s: 5 / 5 `Perfect Counter!`** (receipts 99–132 ms after the press), each followed by `Counter duration=5`. Not pressed: 6 skipped for `attacker_facing`/`self_facing` (4 of them hit us ~418 ms in), 2 `track_stopped` (no hit followed — interrupted), 1 out of range.
- **Shared heavy `102483335944327`:** in all 15 releases the game sent `CooldownNT` Counter 0.5 s at the very moment the release was seen (0 ± 1 ms) — releasing this heavy locks your counter for 0.5 s (supersedes SP-041/050's reading that the lock came from the user's own combo). It also lands **two hits**: the first ~230–560 ms after the release, the second ~750–850 ms (six pairs, e.g. 248/799, 349/848, 334/748, 314/781, 299/749, 298/799). The user's own F presses 150–200 ms before the second hit gave `Blunt` (2) and `BlockEffect` (1); none countered. How to beat it is still unknown (asked the user: tap during the wind-up, hold F, or another key).

**Implemented / untested live — Combat page v16.7:** the rogue heavy no longer requires attacker or self facing (heavies turn and lunge); range, line of sight and other gates still apply.


## SP-052 — PvP had no inputs: additional player combo and geometry regression repair (2026-11-19)

**User report:** player auto parry seems not to work. Reviewed a frozen snapshot of history 1795128862, place 120023577538065, diagnostics v7. At capture the report was RUNNING at 891.882 seconds, held=false, droppedLines=0; it is not a finalized encounter. Later reads of the live report reached the detail cap, so later footer totals must not be paired with this earlier complete-detail snapshot. Config PvP/PvE=true, player range=8, offset=0, hold=120, standard player hits 1/2/5 policy, NPC rogue/heavy extensions present.

**Primary diagnosis — coverage, not measured failed automatic timing:** all 15 F_DOWN events in the snapshot target NPCs. No automatic press targets any player. The main player opponent (anonymous actor 7) produces 857 unmapped-animation skips, including 89 starts of a previously unsupported five-animation combo. Many other unmapped IDs are movement/reaction effects, so 857 is not a missed-attack count. There are 17 self-counter receipts and 85 negative-health observations overall, but these cannot be described as 17 successful player auto-parries or 85 failed player presses. Actual input during the player encounter is unmatched/manual-origin-unknown rather than API-attributed automation. No runtime errors appear in this older running snapshot.

**Observed new combo:** repeated same-actor adjacent transitions within 1.5 seconds are 1->2=23, 2->3=16, 3->4=10, 4->5=7, with no other forward transitions in that five-ID subset. Starts: 29/25/17/11/7. The attacker's HitOut followed by local Blunt/counter/block effects supports the mapping. Its style name is unknown; code calls it player_set_b. Two frequent 24.8571-second animations (85366892412803 / 125023580752762) overlap the sequence and are excluded, as are hit-reaction and movement tracks.

| Hit | Animation ID | Matched HitOut receipts | Median start-to-HitOut | Initial seed |
| --- | --- | --- | --- | --- |
| 1 | 113515398511336 | 25 | 218 ms | 40 ms |
| 2 | 94221477646836 | 25 | 116 ms | 0 ms |
| 3 | 86875409360329 | 17 | 150 ms | 0 ms |
| 4 | 122413336728467 | 8 | 217 ms | 40 ms |
| 5 | 139712447236537 | 7 | 200 ms | 20 ms |

Matching uses the first same-actor HitOut within 600 ms and before the next mapped swing. Receipts vary substantially, including initial swings at roughly 301–468 ms; these are client observations, not measured server impact/parry windows. Local counter effects occur near impacts of each of the five IDs. For example, at 643.6237 s hit 2 starts, F down is observed at 643.6411 s, HitOut at 643.7397 s and the local counter at 643.7406 s. Several sequences contain repeated F inputs and outgoing attacks; do not infer exact causation or a success rate. Fast-hit behavior on this player at this latency cannot be ruled out using the older NPC standard-combo timing conclusions.

**Implemented / untested live — Combat page v17:** add those five IDs for player targets only with the initial seeds above, additive user offset, and style/hit/timing=initial_trial on queued observations. These are different IDs from the standard five-hit combo: the standard hit-3/4 exclusions remain, as do its hit-5 override and Claude's NPC standard/rogue/heavy policies. New IDs on NPCs remain unmapped until separately evidenced. This fills the recorded player catalog gap; the seeds are a controlled starting point, not a calibrated PvP solution. Player shared-heavy and other unsupported styles remain unmapped. Current config identifies playerSetB=all5@40/0/0/40/20ms; Config/START/accepted cues use scope=per_style_catalog timingTrial=player_set_b_initial. PvP toggle, current/alive ownership, focus gating, manual-F, cooldown, range/LOS, typing/menu, tracking/expiry and release cleanup remain.

**Separate latest-source regression reproduced:** the v16.7 source and both installed/public artifacts had no local valid,reason,geometry=threat(...) assignment in observe, yet subsequently used all three variables. A mapped eligible NPC cue reproduced LOGGER_ERROR and no input in a before-fix fixture. The latest threat function also omitted the attacker-facing check for ordinary attacks. Restore the geometry call using the cue's actual range and scoped AnyFacing, and restore attacker-facing rejection except for the existing rogue-heavy exemption. Use the cue's range+3 and AnyFacing consistently during approach admission/rechecks; player range remains 8 by default, NPCs retain their established 12-stud behavior and Shadow-owner special case. This repairs the latest build; it is not attributed retroactively to the error-free snapshot recorded before that source was loaded.

**Validation:** reproduced the original geometry failure, then passed 128 mocked assertions: 60 new-catalog/geometry/concurrent-NPC checks plus 36 standard-policy, 26 ownership/PvP/PvE and six settings/reload checks. Coverage includes every new ID, intended seeds and additive offset, unknown movement/reaction rejection, player-only catalog scope, both facing checks, manual input, cooldown, focus, wall, death/ownership, approach entry, standard exclusions, NPC range and rogue mapping, rogue-heavy facing exemption, shared-heavy stopped-track behavior and cleanup. Source and staged public/private builds compile; both generated variants are checked before installation. Exact-module replacement and hash checks preserve Claude's concurrent unrelated changes. No live Roblox interaction or gameplay input was performed by the agent.

**Handoff:** reload v17 and enable PvP/Auto Parry. The next player run should first demonstrate queued/accepted player_set_b cues and correctly released inputs; only then assess counters, miss penalties and timing. Do not credit manual/unmatched counter events to automation, promise all fighting styles, or silently enable the NPC-only heavy catalog for players. Keep recording automatic. Original frozen evidence and analysis stay local; this note is sanitized.

## SP-052 — Death on the rogue planet sent the user to Heaven (2026-11-19, Claude)

**User:** "why did I go to heaven?"

**Evidence:** history 1795128862 (15:54:22, place **120023577538065**, 1,056 s): the session ended with `STOP Character changed or died`; the CEXP tracker counted `Deaths=1`. All Warrior Clan Rogue fights since 15:30 (histories 1795127425 → 1795128862) were on this place, together with players (a player attacking from 614 s on; health 128 at 918 s). The detail log hit its 12,000-line cap at ~918 s (6,450 lines dropped), so the last ~137 s before the death — what landed the final hit or a grip — were not recorded.

**Why Heaven (Trello):** "Dying in Pietra will send you to Heaven or Hell" and "you lose 10% of CEXP and Power Level and [are] set straight to the afterlife"; Pietra is "a battle grounds planet… higher gravity", with Pietra Wave Defense spawning groups of NPCs. Elsewhere a death respawns you at the hospital. Heaven vs Hell follows Karma (neutral or positive → Heaven). **Inference:** place 120023577538065 is Pietra (the death went straight to the afterlife; the rogues spawn there in groups). Not confirmed by a location label.

**Expected cost:** 10% of CEXP (1,808 → about 1,627 if nothing was gained) and 10% of Power Level; the next Varrik check will show it.

## SP-053 — Visuals: Gravity Mastery row (2026-11-19, Claude)

**User:** "add mastery gravity to the visuals".

**Source:** the profile's `GravityMastery` number (hidden scan 2026-11-19: `GravityMastery=1` for the user). It was previously only a " · Gravity N" suffix on Form Mastery. It is not replicated for other players (characters carry `GravityAugment`, 0 for everyone scanned, meaning unknown), so other players show "Hidden (only shown for you)".

**Implemented / untested live — Visuals:** a "Gravity Mastery" row shows the value and the next unlock from the in-game guide: Kaioken 10, Power Release (TriHuman) 80, Golden (Acrosian) 100, Astral State (Alien) 120, e.g. "1 · next 10 (Kaioken, 9 to go)". The suffix on Form Mastery was removed.

## SP-054 — Visuals CEXP row showed "Unavailable" after a hub reload (2026-11-19, Claude)

**User:** "cexp shows up unavailable".

**Cause (source inspection):** loading `sparkling_cexp_value.txt` ("1808 1795127153") used `local value,at=ok and text:match(...)`, which keeps only the first capture, so `cexp.At` was nil and the row's age calculation errored; the row's pcall then fell back to "Unavailable". It worked in the session that heard Varrik because the listener sets both fields. Same pattern as the CEXP tracker load bug (SP-050).

**Fixed / untested live:** the match runs first, then `ok` is checked. After a reload, the row should read "1,808 (Varrik, N h ago)".

**Same bug in Auto Job (source inspection):** the Floor Staff progress read (`local a,b=amount and amount.Text:match(...)`) lost the total, so "10/10" was never seen as finished and logs printed `progress=N/nil`; finishing relied on the Restart box instead. Fixed (both captures assigned directly). Untested live: the next Floor Staff run should log `PROGRESS N/10` and `DONE` at 10/10.

## SP-055 — Second CEXP reading: 1,808 → 3,950 in 107 minutes, despite one death (2026-11-19, Claude)

**Evidence (Verified from captures):** Varrik checks at 15:25:53 (1,808) and 17:21:42 (3,950); CEXP tracker history line plus 12 Auto Parry histories in between (`cexp_window` breakdown). Sanitized: player names removed, and SP-052's player name was replaced with "a player".

| Period | Activity | Hits landed | Hits taken | Counters |
|---|---|---|---|---|
| 15:30–16:12 | Warrior Clan Rogues, place 120023577538065 (+ one player from ~16:00) | 586 rogues + 94 player | 231 + 65 | 84 + 6 perfect, 9 |
| 16:12 | **Death** (SP-052) | | | |
| 16:16–16:28 | PvP against the same player, same place | 237 | 113 | 33 |
| ~16:30–17:07 | Auto Job: 115 jobs paid, +24,772 Zeni | — | — | — |
| 17:12–17:16 | Shadow, place 85995124525570 | 159 | 51 | 33 |

- **Net +2,142 CEXP** from about 925 non-Shadow hits landed (586 rogue, 331 player, 8 unclear): roughly **2.3 CEXP per landed hit** on average, more if the death took 10%. Rogues and PvP happened in the same window, so a single reading cannot split them.
- **The death did not wipe the gain.** At most about 10% of the CEXP held at 16:12 was lost (unknown, likely under ~300).
- **The place after the death:** PvP continued on place 120023577538065 at 16:16. If the death went straight to the afterlife, the afterlife is in the same place, or the user came back; SP-052's Pietra inference stays unconfirmed.
- **Shadow:** again consistent with no CEXP (the 15:25 reading had +0 after 943 Shadow hits).
- **Tracker caveat:** `PLGained`=313,963 adds every upward Power Level tick (the shown PL drops and recovers with fatigue/injury/health), so it overstates the gain; the net change is 67,314 → 99,183 = **+31,869**. Use the PL field in the history line.

**Next step:** one source per Varrik check (5,000 Zeni each), e.g. 15 minutes of rogues only, then 15 minutes of PvP only, to price each source.

## SP-056 — Saiyan Grunt ambush and EX Wolf Fang fights: no new attacks; effects blocked line of sight; own punches (2026-11-19, Claude)

**Evidence (Verified from captures):** histories 1795134532 (186 s, place 135537125271116, two `SaiyanGrunt1_ambush` NPCs plus Nahray; ended in a death) and 1795134947 (135 s, place 110405289082842 Earth, three `EXWolfFang`), v16.7.
- **Every hit we took that traces to an NPC (attacker HitOut within 60 ms) came from an already-mapped animation.** Grunts: hit 1 `114493250337466` 19, hit 2 `84394230178792` 10, hit 3 `95937874840716` 10 (lead 118 ms), hit 4 `139487903747364` 8 (lead 85 ms). The ambush grunts play them at speed 1.15. EX Wolf Fang uses the rogue combo: hit 1 `78875817992139` 20 (lead 117 ms), hit 2 10, hit 4 9, hit 3 8, hit 5 3. 11 + 10 hits had no NPC HitOut (unclear source). The many unmapped animations nearby did not precede our hits from the same attacker.
- **Press timing is identical for counters and failures:** grunt hits 1/2 pressed 50–53 ms after the start with the result ~183 ms later, and rogue hits 2/3 at 2 ms. Grunt hit 2: 9 counters / 5 Blunt; grunt hit 1: 3 / 4 (+5 no outcome, 2 blocks); grunt hit 5: 4 / 4; rogue heavy: 5 Perfect Counters / 2 Blunt; rogue hit 2: 4 / 3; hit 3: 2 / 2 (+1 `Dodged!`).
- **Own punches:** for presses where the user's own M1 went down within ~220 ms before the press, 15 of 22 failed; otherwise 11 of 32 failed (repeats SP finding: own attacks block the counter).
- **Line of sight (bug):** on Earth, 10 cues were skipped for `line_of_sight` and 6 of those hit us. Every obstacle was a non-colliding combat effect under `Workspace.Thrown` (`Main.In1`/`in2` meshes ~20×10×8, `explosion`, `WallCrack.Ball`, `Part`).
- **Death (place 135537125271116):** grunt combos took health 19.7 → 1 between 175.7 and 182.6 s (hits 3 and 4 too fast, one heavy skipped by the counter cooldown, two by attacker facing); health went to 0 at 185.8 s while knocked.

**Implemented / untested live — Combat page v16.8:** a non-colliding BasePart under `Workspace.Thrown` is excluded from the line-of-sight ray (logged `exclusion=thrown_effect`); collidable Thrown parts still block, and up to 8 exclusions per check (was 5).

**Next step:** decide whether Auto Parry should hold back the user's own M1 for the ~200 ms before a mapped NPC hit lands (asked the user).

## SP-057 — Own M1 before the press makes counters fail; optional click hold (2026-11-19, Claude)

**User:** asked for a toggle that holds their punches so they stop spoiling Auto Parry's counters.

**Evidence (Verified from captures; 26 histories since 12:00, 737 presses):** every press sent its `Defend` request (55 / 55 checked in SP-056's two histories), so the failure is server-side. Outgoing `Requests.Action("MouseButton1", bool, lookVector)` is sent for each M1 click.

| Your M1 sent, relative to the press | Presses | Failed |
|---|---|---|
| 0–100 ms before | 64 | 60 |
| 100–200 ms before (none closer) | 50 | 43 |
| Only after the press (0–250 ms) | 138 | 18 |
| None from 400 ms before to 250 ms after | 396 | 107 |

- **Offline replay of two hold policies** (failed presses with an M1 in the 200 ms before: 92):
  - Holding from the NPC cue to the press would have removed every such M1 for **44**, at under 1% of all clicks.
  - Also holding while the same NPC is mid-combo (0.6 s after its previous mapped hit) adds 18 more but holds 25% of all 6,105 clicks (0.8 s: +21, 32%).
  - 27–30 remain uncoverable (the M1 came before the swing could be seen).
- **Same-NPC gap between mapped hit starts:** median 549 ms (p25 466, p75 634).

**Implemented / untested live — Combat page v17.2, "Hold Punches Before Parry (NPCs)", off by default:**
- When an NPC cue is queued, outgoing `Requests.Action` `"MouseButton1"` FireServer calls are not sent until 30 ms after its scheduled press (logged `CLICK_HELD`). Nothing else is held (never `Defend`); player cues never set it; the hold is an expiring deadline cleared on stop.
- **Shared hooks (SparklingCombatRecorder):** hooks installed from now on call the newest `forward` through `probe.Forward`, so later reloads update them. Hooks left from an earlier hub run in the same game session can't hold clicks; Auto Parry reports "needs a rejoin" (`CLICK_HOLD available=false`).
- **Line-of-sight fix renumbered** v17.1 (was v16.8 in SP-056), after GPT's v17.

**Next step:** a recording with the toggle on. Compare the counter rate with M1 in the cue window, and count `CLICK_HELD`.

## SP-058 — Visuals: Event Wins row (2026-11-19, Claude)

**User:** "how can i see event wins".

**Source:** the character attribute `EventWins` replicates for every player (Profile Scan 2026-11-17: values 1.5, 0 and 4 on three characters). The profile table also holds `EventWins` (own only), used as the fallback. No in-game display is documented in the Trello or Game Research notes.

**Implemented / untested live — Visuals:** an "Event Wins" row for any selected player (attribute first, then your profile).

## SP-059 — PlunderTrainer combo mapped; manual F mashing (2026-11-19, Claude)

**User:** "auto parry for you to do" (new recording, history 1795148185, laptop-job place 102793290484692, ~103 s, ended in death). Hold Punches was **off** (`holdClicks=false`), so SP-057 is still untested.

**Evidence (Verified from capture):**
- One NPC, `PlunderTrainer`, did nearly all the damage (HP 167 → 1, mostly 3.61 per hit). Only 8 cues were queued; 448 swings were skipped as `unmapped_animation`.
- **Its combo is a separate animation set** (only in this capture, only on that NPC; all seen at phase 0, speed 1.25). Order from same-actor sequences, with the attacker's HitOut after the swing is seen:

| Hit | Animation | Median impact (min–max) | Starts / impacts |
|---|---|---|---|
| 1 | 115562914490424 | 218 ms (216–233) | 22 / 18 |
| 2 | 139627968387116 | 167 ms (166–185) | 18 / 17 |
| 3 | 92411449150342 | 118 ms (100–135) | 14 / 14 |
| 4 | 124959370864638 | 267 ms (251–269) | 10 / 10 |
| 5 | 80989963053908 | 184 ms (168–200) | 10 / 9 |

  Hit 4 is sometimes replaced by the rogue heavy `126361551838373`. The trainer also uses the shared heavy `102483335944327` (after wind-up `88670347816269`).
- **Counter window:** the user's own two counters on hit 3 came from F pressed ~148 ms before impact (14–31 ms *before* hit 3 was visible), matching the ~150+ ms lead used for the other combos.
- **Manual F mashing:** the user pressed F by hand about every 200 ms throughout the fight. Most presses ended in Blunt or a block. A failed press starts the 5 s counter cooldown, which also blocks Auto Parry (two heavy cues were skipped for `counter_cooldown`).
- **Rogue heavy at 0.30 s:** 0/3 here (hit landed 98–115 ms after the press, about 400–417 ms after the swing, the same timing that went 5/5 on rogues in SP-050). Press 2 had the user's own M1 2 ms after F and their own PBlock already set. n=3, not retuned.
- Unmapped and left alone: `123910713670259` (4/4 hit, 464–684 ms) and `88418489809399` (300–1151 ms) are too spread for a fixed delay.

**Implemented / untested live — Combat page v17.3:** trainer hits 1 / 2 / 4 / 5 are pressed after 40 / 0 / 80 / 0 ms (~178 / 167 / 187 / 184 ms of lead); hit 3 is skipped as too fast (`npc_late_hit`). NPCs only: players using these animations stay unmapped (`trainerStyle=player`). Cues log `style=plunder_trainer`. These are trial seeds from one recording.

**Next step:** a trainer fight with Auto Parry on, no manual F, and Hold Punches on (after a rejoin). Compare the counter rate per hit.

## SP-060 — PlunderTrainer mapping result; 1.2 s lockout after any counter (2026-11-19, Claude)

**User:** "done" (history 1795148752, laptop-job place, ~156 s, ended in death). Hold Punches was still **off** (`holdClicks=false`); the user pressed F by hand only twice.

**Evidence (Verified from capture) — SP-059 trainer mapping, first live run:**

| Trainer hit | Pressed | Counter Parry | Blunt | Block | No effect |
|---|---|---|---|---|---|
| 1 (@40 ms) | 12 | 3 | 4 | 3 | 2 |
| 2 (@0) | 12 | 6 | 4 | 0 | 2 |
| 4 (@80 ms) | 11 | 7 | 3 | 1 | 0 |
| 5 (@0) | 4 | 2 | 1 | 1 | 0 |
| rogue heavy (@300 ms) | 2 | 0 | 2 | 0 | 0 |

18 counters from 41 presses, versus none from Auto Parry on trainer combos before SP-059. Successful and failed presses had the same lead before impact (133–200 ms), so the failures were not timing.

**Evidence (Verified from captures, 38 histories today) — post-counter lockout:** the `CooldownNT` "Counter" notice of 0.5 s arrives after your own counter and when an opponent counters you. F presses after it:

| Time after the 0.5 s notice | Presses | Counters |
|---|---|---|
| 0.50–1.20 s | 192 | 0 |
| 1.20–1.25 s | 2 | 1 |
| 1.25–1.50 s | 52 | 39 |
| 1.50 s and later | 670 | 553 |

- The same holds after your own counter (0 of 25 before 1.25 s) and after an opponent countered you (1 of 169).
- 187 of 202 presses inside the window took a Blunt hit. 8 of 192 failed ones were followed by a 5 s counter cooldown.
- The 5 s notice (failed counter) has no extra lockout: presses 5.0–5.5 s after it countered 5 of 10.
- **So the counter state is locked about 1.2 s after any counter, not 0.5 s.** After an opponent counters your attack, its next hits in that window land regardless. That was the larger source (169 of 194).

**Implemented / untested live — Combat page v17.4:** a 0.5 s (any sub-1 s) counter notice now sets the local cooldown to 1.15 s (`COUNTER_COOLDOWN ... lockout=1.15`). Cues inside it are skipped as `counter_cooldown`. Heavy cues keep their existing defer rule (up to 0.40 s after the release is seen). The 1.15 s value is a trial at the edge of the measured window.

**Next step:** a recording with Hold Punches on (after a rejoin). Check that no counters were lost at 1.15–1.25 s and that no 5 s cooldowns follow lockout presses.

## SP-061 — v17.4 lockout checked live; AlienBandit combo mapped; Auto Meditation Ki counter (2026-11-19, Claude)

**User:** "new auto parry recordings, also is the auto meditation ki counter correct?" Two histories on the laptop-job place: 1795149155 (PlunderTrainer, ~144 s, ended in death) and 1795149911 (Alien Bandits, ~99 s). Hold Punches was **off** in both (`holdClicks=false`), so SP-057 is still untested.

**Evidence (Verified from captures) — v17.4 post-counter lockout:** no press came inside 1.2 s of a 0.5 s notice in either history (24 such notices in the trainer fight).

**PlunderTrainer, second run (v17.4):** 18 counters from 30 presses (was 18 / 41 in SP-060).

| Hit | Counters / presses |
|---|---|
| 1 | 3 / 9 |
| 2 | 10 / 12 |
| 4 | 2 / 3 |
| 5 | 2 / 3 |
| rogue heavy | 1 / 3 |

- Across both trainer runs, hit 2 countered 14 / 17 with no own M1 in the 400 ms before, and 2 / 7 with one. This matches SP-057.
- Hit 1 countered only 5 / 15 even without a recent M1, with the same lead as its successes (133–200 ms). Its failures cluster 0.7–1.1 s after the user's last M1, which may be a post-combo state. Unresolved; not retuned.

**AlienBandit combo (history 1795149911):**
- It is a separate animation set: speed 1.25, phase 0, one attacking bandit.
- Order from same-actor sequences:

| Hit | Animation | Median impact | Range | Impacts |
|---|---|---|---|---|
| 1 | 115411232011168 | 301 ms | 283–318 | 11 |
| 2 | 85425709738831 | 167 ms | 165–168 | 8 |
| 3 | 107415938517971 | 216 ms | 200–218 | 7 |
| 4 | 84926821033390 | 184 ms | 183–200 | 5 |
| 5 | 99917228120363 | 218 ms | 201–234 | 4 |

- Hit 2 or 3 is sometimes followed by the rogue heavy; the shared heavy also appears.
- These five accounted for 29 of 45 Blunt hits taken. Auto Parry pressed only 3 times (rogue heavy, 1 counter).
- The user pressed F by hand 25 times, and the history holds 8 five-second failed-counter cooldowns.

**Implemented / untested live — Combat page v17.5:**
- Bandit hits 1–5 are pressed after 120 / 0 / 40 / 0 / 40 ms (~181 / 167 / 176 / 184 / 178 ms of lead).
- NPCs only: players using these animations stay unmapped (`banditStyle=player`). Cues log `style=alien_bandit`.

**Auto Meditation Ki counter (source inspection):**
- The status shows `Ki <profile Mind> / 400`.
- Mind is read from the client-memory profile copy SparklingVisuals picks: of the snapshot copies, the one whose PowerLevel is closest to the live PowerLevel. It is re-scanned every 30 s while meditating.
- In the latest log it read 100 at start, 30 s and 60 s, while every prompt was answered.
- Mind is the stat that Trello's "100 Mind" requirement (Selflessness State) uses. Whether the copy updates live during meditation is **unverified**: it has never been compared with the in-game number (Doctor).
- **Next step:** the user checks Ki at a Doctor. If it isn't 100, the counter is reading a stale snapshot and needs a live source.

## SP-062 — Ki is profile `Ki`, not `Mind` (2026-11-19, Claude)

**User:** checked Ki at a Doctor: **244.4**. Auto Meditation's counter showed 100.

**Evidence:**
- User report: Doctor Ki 244.4.
- **Verified from capture:** the Hidden Value Scan v3 profile (2026-11-17) holds two fields:
  - `Mind=100`, flat and the same in every copy.
  - `Ki=15.6` in the newest copies and `15.4` in the older ones. It is fractional and rose 0.2 between snapshots, like the Doctor's 244.4.
- So the meditation Ki stat is profile `Ki`. `Mind` is a separate stat. The SP-A001 note ("profile `Mind`") and the Visuals Stats row ("KI" = Mind) were wrong.

**Implemented / untested live:**
- **Auto Meditation:** the counter and the 400 cap read profile `Ki`, rounded to 0.1. The log uses `START ki=` / `KI`.
- **Visuals Stats (own):** `KI` now shows profile Ki and a separate `MIND` shows Mind. For other players the direct-field key formerly labelled `KI` (Mind) is now labelled `MIND`.
- **Profile copy choice:** when several snapshot copies are equally close to the live PowerLevel (it doesn't change while meditating), the one with more Ki is used.

**Next step:** confirm the counter matches the Doctor (around 244).

## SP-063 — Hold Punches first live run; lockout 1.25 s; Wolf Fang Claw (2026-11-19, Claude)

**User:** "new auto parry test". Three histories, all v17.5:
- 1795151179 — Alien Bandits, laptop-job place, Hold off.
- 1795151513 — EX Wolf Fang ambush, Earth, ~250 s, Hold **on**.
- 1795152104 — Wolf Fang, Hold on; still recording when read.

**Evidence (Verified from captures):**
- **AlienBandit mapping (SP-061), first run:** 11 counters / 17 presses (hits 1–4: 5/8, 1/4, 4/4, 1/1).
- **Hold Punches (SP-057), first run:** `CLICK_HELD` 7 times out of 410 outgoing M1 (held 5–205 ms).
  - Wolf Fang ambush: 25 counters / 31 presses (81%). Only 3 presses had an own M1 in the 200 ms before (2 countered); the other 28 countered 23 times.
  - Earlier, 64 of 252 recorded presses had an M1 in the 100 ms before (SP-057).
  - The third history so far: 8 / 9, with 6 holds.
  - The Hold Punches result is live on one fight: a large improvement, but still a small sample.
- **Lockout edge:** six presses landed 1.15–1.20 s after a 0.5 s notice (deferred to the v17.4 lockout end) and all failed. Pooled with SP-060: 1.05–1.20 s 0 / 28, 1.20–1.30 s 4 / 8.
- **What still hits in the Wolf Fang ambush (101 HP drops):**
  - Rogue hit 1 `78875817992139` (28): skipped, it lands ~117 ms after it is seen.
  - **Wolf Fang Claw** `123139123318422` (15): unmapped.
  - Rogue hit 2 when not pressed (14). The rest is spread across other skills.
- **Wolf Fang Claw:**
  - A skill: Weave "Wolf Fang Claw", length 1.43 s, multi-hit "Sharp" plus Bleed. First self hit a median 301 ms after the start (56 starts in 4 histories, 50–634 ms).
  - Manual F 85 ms and 186 ms after the start each gave Counter Parry about 200 ms later, at 283 and 384 ms.
  - So the skill can be countered.

**Implemented / untested live — Combat page v17.6:**
- The post-counter lockout is now **1.25 s** (`postCounterLockout=1.25s`).
- **Wolf Fang Claw** is pressed 100 ms after it is seen (~200 ms before the median first hit). NPCs only; logs `style=npc_skill skill=wolf_fang_claw`. Trial seed.

**Next step:** a Wolf Fang fight on v17.6. Count Wolf Fang Claw counters, and check that no presses land before 1.25 s.


## SP-064 — Full NPC attack policy shared with players (2026-11-19)

**User request:** "apply all the npc auto parry to the player side". Supersedes the NPC-only catalog restrictions in SP-049 through SP-063 and the player-heavy limitation in SP-052. Built on Claude's latest v17.6 source, preserving concurrent modules. This is an explicitly requested policy transfer, not new evidence of equivalent PvP timing or success.

**Implemented / untested live — Combat page v18:** all NPC-mapped animation IDs now use the same attack policy for player targets. Existing player_set_b coverage remains additive and player-only. Standard combo still presses hits 1/2/5 and skips 3/4. Added player coverage:

| Catalog | Delay after observed animation start (plus user offset) | Exclusions / behavior |
| --- | --- | --- |
| Warrior Clan Rogue | hit 2/3: 0; hit 5: 80 ms | Skip hits 1/4 |
| Rogue heavy | 300 ms | Same explicit facing exemption; LOS/range still required |
| PlunderTrainer | hits 1/2/4/5: 40/0/80/0 ms | Skip hit 3 |
| AlienBandit | hits 1–5: 120/0/40/0/40 ms | All five mapped |
| Wolf Fang Claw | 100 ms | Same skill ID; other skills remain unmapped |
| Shared heavy release | rotating 260/320/380 ms | Same stopped-track exception, expiry and bounded cooldown deferral |

Player and NPC shared-heavy rotations have separate indices so a player cue cannot change the existing NPC trial sequence. No timing constants changed. The 1.25-second post-counter lockout and cooldown/input/approach/LOS policies already share one execution path and remain shared. The optional **Hold Punches Before Parry (Players & NPCs)** now applies to accepted mapped player cues as well, from cue time through scheduled press +30 ms; PvP-disabled, skipped and unmapped cues do not activate it. It retains its existing saved toggle and stop cleanup.

**Target controls retained:** PvP/PvE toggles, current/alive player ownership, player focus gating, configured player range (default 8), NPC-specific 12-stud range/Shadow-owner exception, typing/menu pauses, manual F protection and input release cleanup. "Shared policy" describes attack handling; it does not silently change user targeting settings or enable background player inputs. Unknown IDs stay unmapped. Existing player_set_b timing remains 40/0/0/40/20 ms.

**Diagnostics:** Config, START and CUE_ACCEPTED identify scope=shared_npc_player_catalog timingTrial=npc_policy_pvp_transfer. Catalog config fields are now sharedHeavyTrial/sharedRogue/sharedTrainer/sharedBandit/sharedWolfFangClaw; skill cues use style=shared_skill. targetType still distinguishes NPC and player observations; heavy trial tags distinguish npc_heavy/player_heavy. Header identifies mappedCue punch holding. Diagnostics continue automatically while enabled. Existing readers must accept these new shared labels rather than filtering only npc-prefixed fields.

**Validation:** 317 mocked assertions pass: 189 new shared-catalog/timing/cooldown/targeting/punch-hold checks, 60 existing player-catalog/geometry/NPC-preservation checks, 36 standard-policy checks, 26 target ownership/PvP/PvE checks and six settings/reload checks. Every newly shared light/skill ID is exercised for players and NPCs at its intended delay, exclusions remain skipped, heavy trial sequences stay independent, stopped-track handling and the scoped facing exception work, walls/range/manual F/PvP/focus still block inputs, 1.25-second lockout and additive offset persist, and punch-hold toggling/cleanup work. Source, staged public/private, and regenerated public/private variants compile. No live gameplay interaction was performed. NPC results from older notes are not counted as player successes.

**Handoff:** reload v18, enable PvP and Auto Parry, and optionally enable Hold Punches Before Parry. Review the next existing player diagnostic by style/targetType: first establish queued cues and released inputs, then counters versus damage and cooldown. Shared-heavy reliability was unresolved even on NPCs (SP-051); do not describe it as proven. Reused Sparkling's existing path only; the Hunter X Enter reference note was checked for workflow, and no cross-game IDs, remotes or timings were imported. Sanitized notes only; raw reports remain local.


## SP-065 — New player combo caused zero automatic inputs; first v18 PvP results (2026-11-20)

**User:** "new findings for the auto parry". Reviewed frozen local snapshots of histories 1795153147, 1795153976, 1795155611, 1795155621, 1795155653, 1795156077 and 1795158150. Raw reports/identifiers remain local. Findings below refer to those snapshots, not subsequently growing footer totals.

**Newest v18 run (1795158150, place 135537125271116):** PvP/PvE=true, range=8, offset=0, hold=120 ms, Hold Punches=true and available. Frozen footer RUNNING at 260.575 s: **0 automatic attempts**, 7 self-counter receipts, 199 negative-health observations, held=false, droppedLines=1592. Event detail ends at 192.6077 s. Thus none of the seven counters can be credited to this auto-parry session. No runtime errors appear in the retained detail. The main opponent is anonymous actor 2 (928 starts), with another player contributing seven starts.

- **Catalog gap:** all five IDs below were unmapped. Copying the NPC catalog did not cover this player's different animations. There were 955 unmapped skips in the full snapshot, many from ambient/movement/reaction animations; this is not 955 missed attacks.
- **Known heavy is cooldown-blocked:** six eligible queues are shared-heavy releases. Each coincides with Counter=0.5 notice within 0.6 ms, producing the existing 1.25-second lockout. All six execute decisions skip for counter_cooldown. Their 260/320/380-ms seeds, <=400-ms deferral and 450-ms expiry cannot outlast that lockout. One additional release failed facing at observation. Do not bypass the lockout or claim that mapping the heavy solved its mechanics; this repeats SP-051's unresolved behavior on a player. No heavy timing change made.
- Hold Punches reports seven held clicks even with no parry input: it starts at queue admission, before execution gates such as cooldown. CLICK_HELD alone is not proof of a successful defense.

**Observed additional player style (code label player_set_c; actual style name unknown):** normal spaced sequences are at phase 0, speed 1.175. Rapid repetitions also reuse these IDs at speed 1.000, with unreliable impact pairing; their combat meaning is unresolved. Normal sequences repeatedly run 1->2->3->4->5. Adjacent transitions within 1.2 s in this subset: 1->2=19, 2->3=22, 3->4=12, 4->5=4. The capture also contains dense repeated/backward starts; do not treat every start as a distinct clean combo or assign every nearby HitOut to it.

| Hit | Animation | Starts | Spaced start-to-HitOut observations | v18.1 initial seed |
| --- | --- | --- | --- | --- |
| 1 | 91205200727116 | 36 | n=19, median 183 ms, 167–205 ms | 0 ms |
| 2 | 97820331057780 | 34 | n=19, median 201 ms, 165–218 ms | 20 ms |
| 3 | 116312003316079 | 33 | n=15, median 202 ms, 184–219 ms | 20 ms |
| 4 | 79083230788146 | 22 | n=6, median 132 ms, 115–150 ms | Skipped as player_late_hit |
| 5 | 111874012490307 | 6 | n=3, median 318 ms, 316–320 ms; separate normal 4->5 sequence at 351 ms | 100 ms |

Matching: first same-actor HitOut within 600 ms, before the next candidate start. The table's spaced subset additionally requires >250 ms since the previous candidate start, removing obvious rapid overlap but also excluding the normal 249-ms fourth-to-fifth transition. These are client observations, not server parry windows. Hits 1/2/3 have local Blunt observations immediately beside 8/11/10 of the spaced HitOut events. Hit 4 is still an attack candidate despite no Blunt in that subset: its outgoing HitOut aligns with a local counter, and the style sequence is repeated. Mapping confidence is stronger for hits 1–3 than the less frequent fifth.

**Manual/unmatched counter evidence:** hit 2 has two local counter receipts, hit 3 one, hit 4 one, hit 5 one near same-actor HitOut. Multiple nearby F presses prevent causal attribution for hits 2–4. In particular, the fourth-hit counter at 12.515 s follows F presses both 64 ms before animation start and 104 ms after it: it does not establish that a reactive press can counter the 115–150-ms hit. Leave hit 4 skipped, consistent with the established short-lead policy, pending discriminating evidence. Hit 5 at 18.1466 s has one F-down in the preceding 400-ms impact window, at 18.2353 s (+89 ms), followed by HitOut at 18.4973 and local counter at 18.4979. The 100-ms seed is a trial informed by that observation, not a calibrated guarantee. The first three seeds retain roughly 160–180 ms of median client lead; no exact server window is asserted.

**Earlier v18 player run (1795155653):** finalized Disabled at 338.665 s, totals 10 attempts and 5 self counters, but detail capped at 182.5688 s (5074 dropped lines). Only the first eight automatic presses can be individually reviewed: three standard hit 1, two standard hit 2, two standard hit 5 and one rogue heavy. Outcomes within 600 ms: two Blunt receipts, two BlockEffect receipts, one Counter Parry receipt and three without one of those outcomes. Every one has a successful F_UP API result (121–135 ms hold); one hit-5 input also has an early-up event while the API pulse remained held. The counter at 178.5531 s follows a standard hit-2 automatic press by 149 ms, with no intervening unmatched F event in that window. This is evidence of a counter receipt following an automated PvP press, not proof that all five session counters or the omitted final two attempts succeeded. The other retained self-counter receipts are unmatched. Rogue-heavy player reliability remains unresolved.

**Other new runs:**
- 1795153147, pre-v18 NPC fight, finalized Teleport at 290.154 s, no dropped lines: 32 presses, 24 self-counter receipts. Wolf Fang Claw was pressed twice: one was followed by BlockEffect at +198 ms then Sharp at +349 ms; the other had no local counter/block/Blunt/Sharp effect within 600 ms. Its new mapping is active, but this is not evidence of a successful automatic Wolf Fang counter.
- 1795153976, pre-v18 player fight: six standard-combo presses retained, none followed by a local counter within 600 ms. Session totals two counters and 47 damage observations; detail capped near the end. No timing conclusion drawn from aggregate totals.
- 1795155611 / 1795155621 are brief toggles with no attempts or damage.
- 1795156077, v18 long run: finalized on character change/death at 1628.960 s, totals 319 attempts / 256 counter receipts / 107 damage observations, but detail ends at 235.506 s with 58,311 dropped lines. Retained 64 presses target NPCs; 56 have a counter receipt within 600 ms. Do not present 256/319 as a verified success rate or extrapolate the retained NPC targets to the missing portion.

**Implemented / untested live — v18.1:** add player_set_c hits 1/2/3/5 with seeds 0/20/20/100 ms plus the user's existing offset; recognize and explicitly skip hit 4. These IDs remain player-only because NPC use is unverified. Only the observed normal speed 1.175 (tolerance 0.025 for numeric variation) is eligible; other speeds log unverified_style_speed. This deliberately leaves the speed-1 rapid repetitions unmapped as actionable attacks; do not infer that they can be parried from shared IDs. Keep all existing shared NPC mappings, player_set_b, heavy trials, cooldowns, geometry, focus, ownership, PvP/PvE controls, approach checks and optional punch holding. Config adds playerSetC; cue diagnostics identify style/hit/timing=initial_trial. Ambient candidates 107629357392315 and 118936983430888 (both 1.95-second tracks, frequent overlapping starts) and common movement/reaction tracks stay unmapped. No blanket unmapped-animation trigger added.

**Validation:** 382 mocked assertions pass: 65 new-style timing/scope/gate checks plus all 317 v18 checks. Covers intended seeds and offset, fourth-hit exclusion, NPC scope exclusion, ambient/reaction rejection, configured range and approach recovery, player focus/ownership/PvP, manual F, cooldown, stopped tracks, typing/menu/death, optional punch hold and cleanup. Source and both staged/generated variants compile. No live session control or gameplay input by the agent. Reused Sparkling's existing catalog/decision pipeline; no cross-game timing imported.

**Handoff:** reload v18.1 for this additional style. Existing recording is sufficient to implement the coverage fix; no repeat capture was required first. Next player results must show actual automatic player_set_c presses before timing success is assessed. Keep the fast fourth hit and shared-heavy problem explicitly unresolved. The detail-cap issue is now material in busy/long fights: retained event windows and footer totals must be reported separately; future recorder changes should preserve useful per-attempt evidence without silently dropping failure context. No logger format/cap change was made in v18.1.


## SP-066 — Live PvP counters; cosmetic LOS failures reproduced and fixed (2026-11-20)

**User:** "new recordings". Read frozen snapshots of 1795158781, 1795158876, 1795160040 and 1795160661. The three fight snapshots have no dropped lines. Last run was still RUNNING; do not replace its snapshot totals with later growing totals. All use v18.1 playerSetC config, PvP/PvE=true, range=8, offset=0, hold=120 ms and Hold Punches=true. Raw reports and actor identifiers stay local.

| Session | Snapshot status / elapsed | Automatic presses | Self-counter receipts | Receipts within 600 ms of an automatic press |
| --- | --- | --- | --- | --- |
| 1795158781 | Disabled / 1.433 s | 0 | 0 | 0 |
| 1795158876 | Disabled / 81.847 s | 3 | 1 | 1 |
| 1795160040 | Disabled / 154.775 s | 19 | 10 | 10 |
| 1795160661 | RUNNING / 275.624 s | 6 | 6 | 3 |

All 28 retained automatic presses have successful F_UP API results, and no LOGGER_ERROR/failed-input events occur in these snapshots. Counter receipts have a single recent candidate attempt with inputConflicts=[]; attribution remains temporal, not server confirmation of causation. The other three newest-run counter receipts have no automatic candidate. Do not count those as auto-parry successes.

**New style player_set_c is active:** the 81.847-second run presses hit 3 twice and hit 2 once. First hit-3 press at 25.6500 s is followed by the opponent's HitOut at +180 ms and local Counter Parry at +181 ms, without an intervening unmatched F event. This is live evidence that the new mapping can lead to a counter. The later hit-2/hit-3 presses at 31.0495/31.4170 s have no counter. A BlockEffect at 31.5808 follows the second hit-3 press by 164 ms, and a subsequent Blunt at 31.9318 belongs to the later sequence. Do not independently credit the same effect to both presses merely because both are within a 600-ms window.

Hits 1 and 5 never reached input in that run. Mapped skips: 16 attacker-facing, 5 self-facing, 14 out-of-range, 5 approach-expiry, 5 line-of-sight and 4 cooldown; hit 4 remains intentionally skipped (11 starts). No unverified_style_speed skips. Geometry explains much of the low input count, but that does not establish that all facing/range gates should be removed.

**Complete 155-second standard-style fight (1795160040):**

| Attack | Automatic presses | Local counter receipts within 600 ms |
| --- | --- | --- |
| Standard hit 1 | 2 | 1 |
| Standard hit 2 | 5 | 2 |
| Standard hit 5 | 5 | 5 |
| Rogue heavy 126361551838373 | 7 | 2 Perfect Counter |

No unmatched F event intervenes in these press/counter windows. Standard hit 5 is the strongest evidence in this sample; keep its zero-delay seed. These are small per-encounter counts, not a general success guarantee. Rogue heavy's five other presses were followed by Blunt. Successful heavy receipts came 112 and 132 ms after input; failed hits came 58–117 ms after input. Four failed inputs were ~301–302 ms after animation start, overlapping the successful 301/305-ms scheduling; one was delayed to 327 ms. Prior own-M1 ages were 1.0–7.7 seconds, so a punch immediately before input does not explain these seven heavy outcomes. Existing cooldown had cleared. Do not invent a new universal heavy delay from this mixed evidence. Shared heavy 102483335944327 still had seven cooldown skips and four facing skips; zero presses.

**Newest mixed-player run (1795160661):** the first opponent uses mapped PlunderTrainer hits. All three automatic inputs (hit 1, hit 5, hit 2) are followed by same-opponent HitOut and Counter Parry at +148/+132/+165 ms. Later unmatched F presses in the broad 600-ms windows occur only after those receipts (+350–582 ms), so they are not intervening causes of those earlier counters. Against the next standard-style opponent, three automatic presses show zero counter receipts: one has a Blunt at +180 ms, two have no matching local outcome/attacker HitOut within 600 ms. Three other self counters in the session have no automatic candidate. Unknown repeated IDs are not automatically attacks: 18151234538 is a 20-second track with 80 starts; 127030057484621 / 85648965461461 (0.7833 s, 18/14 starts) and 83185442366893 (0.75 s, 13 starts) remain unclassified. No speculative new mappings added.

**Confirmed code/observation failure — cosmetic objects blocking LOS:**
- Run 1795158876: four decisions stop at the exact target player's `Workspace.Visuals.anotherHead<target-character-name>` (hit 5 three times, hit 1 once), and one at `Workspace.Thrown.Part`.
- Run 1795160040: two decisions stop at the target head effect, two at `Workspace.Thrown.asrushPart`.
- Run 1795160661: two stop at the target head effect; one stops at `Workspace.Thrown.Debris`.
- The eight head-effect blockers are direct Parts in Visuals, CanCollide=false, CanQuery=true, unanchored, size 2/1/1. Names match the actual cue target's character, checked locally without publishing identifiers. The three Part/asrushPart blockers are CanCollide=false, CanQuery=true, size 8/8/5. Debris is CanCollide=true and must continue blocking. Thus **11 first blockers are cosmetic/non-colliding**, versus one real collidable object; this does not prove all 11 would otherwise pass later gates or counter successfully.
- Source explains both gaps. Head filtering previously covered only the local character's exact effect. The intended SP-056 Thrown filter sat after an ancestor walk containing `if not cursor then return end`; normal Part -> Thrown -> Workspace -> DataModel -> nil chains returned from the whole helper before reaching the non-colliding-part check. It was present in code but ineffective for these objects. Logs explicitly show exclusion=none for them. Do not treat SP-056's implemented status as prior live verification.

**Implemented / untested live — v18.2:** pass the current target model to cosmeticObstacle. Exclude only its exact direct `anotherHead` BasePart under Visuals when CanCollide=false and it belongs to a current live Player character. Local head and exact Zvanishdummy handling remain. Change the ancestor walk's nil case from return to break so the existing exact non-colliding Thrown-part test executes. Raycast again after each exclusion; preserve eight-step cap. Unrelated/nested/lookalike Visuals objects, collidable target-head parts, actual walls and collidable Thrown debris remain blocking. No broad Visuals/Thrown folder exclusion. No changes to attack catalogs, timings, cooldowns, range/facing, target toggles, punch holding or input lifecycle. Config adds exact_noncolliding_target_anotherHead; LOS_OBJECT logs target_head_effect/thrown_effect when actually excluded.

**Validation:** before-fix fixture reproduces both head-effect and non-colliding-Thrown failures (eight assertions). After-fix 446 mocked assertions pass: 64 targeted LOS assertions plus all 382 v18.1 checks. Tests use ancestry-aware raycasts to verify exact target and local head effects, Zvanishdummy, nested non-colliding Thrown effect, walls behind cosmetics, multiple effects, collidable debris/head, unrelated and prefix-lookalike names, wrong/nested folders, and iteration cap. Existing timing, player/NPC scope, focus, ownership, manual F, cooldown, approach, cleanup and settings checks pass. Source, staged public/private and regenerated public/private builds compile. No live session control or agent gameplay inputs. Reused this game's existing LOS policy; no cross-game timings or semantics imported.

**Handoff:** reload v18.2 to remove these confirmed false LOS blockers. The recordings support retaining current standard hit-5 timing and newly shared Trainer coverage; rogue-heavy reliability remains mixed and shared-heavy cooldown remains unresolved. Next retained diagnostics should distinguish target_head_effect/thrown_effect exclusions from actual obstacles. Do not widen facing/range or retune all attacks just to increase the input count. Automatic recording continues.

## SP-067 — Shadow and rogue NPC results on v18.x; rogue heavy timing trial (2026-11-20, Claude)

**User:** "more auto parry test for you". New since SP-066: 1795162218 (brief toggle), 1795162254 (PvP, 1303 s), 1795163589 and 1795164047 (PvP vs Plunder-style players, the second ended in death), 1795164356 (Warrior Clan Rogues, place 120023577538065, 603 s, ended in death). All are v18.2. Also re-read the NPC part of 1795156077 (Shadow). Raw reports and identifiers stay local.

**NPC evidence (Verified from captures):**
- **Shadow (1795156077, retained first 235 s):** 56 counters / 64 presses (88%), 22 held clicks, 0 presses inside 1.25 s of a 0.5 s notice. The Shadow's two rogue-heavy presses both took Blunt.
- **Warrior Clan Rogues (1795164356):** retained first ~299 s: 29 counters / 36 presses (81%): hit 2 14/16, hit 3 6/8, hit 5 7/8, heavy 2/4. Footer for all 603 s: 72 attempts, 54 self-counter receipts, 164 damage events (footer totals, not per-press attribution). 0 presses inside the lockout.
- Rogue hits taken in the retained part, by most recent NPC animation within 600 ms: hit 1 17, hit 2 8, hit 3 5, hit 4 5, hit 5 3, heavy 3. Hits 1 and 4 stay skipped (too fast; 119 `npc_late_hit` skips in the footer). Many others follow 180435571 / 12034165582, which look like idle/reaction tracks, not attacks.
- Wolf Fang Claw: no EX Wolf Fang in these runs; still untested.

**Rogue heavy `126361551838373`, pooled over every recording with a press (13 histories, NPC and player targets):**
- 74 presses, all at 300–366 ms after the start: 31 Perfect Counter, 25 Blunt, 18 no outcome (usually our punch interrupted the heavy).
  - NPC targets: 29 / 15 / 17.
  - Player targets: 2 / 10 / 2.
- Every Perfect Counter arrived 400–435 ms after the start, i.e. at least 100 ms after F. Ten of the 25 Blunts landed earlier, at 383–402 ms (83–102 ms after F).
- Unpressed heavies hit us a median 418 ms after the start (p10 400, p90 448; n=147).
- Neither an own M1 more than 200 ms before F nor time since the last counter notice (1.25 s or more) separates the outcomes.
- Earlier manual Perfect Counters (SP-050) came from presses 0.25–0.37 s after the start.
- Reading: with a fixed 0.30 s press, the early impacts don't leave enough lead.

**Implemented / untested live — Combat page v18.3:**
- The rogue heavy press now alternates **260 / 300 ms** per target type (separate NPC and player counters, user offset still added).
- Cues log `trial=npc_rogue_heavy` / `player_rogue_heavy heavyDelayMs=260|300`. Config: `heavy@260/300ms`.
- The shared catalog (SP-064) still applies the same policy to players.
- No other timing, gate or catalog changed. Source and both variants compile; no live test yet.

**Next step:** a rogue fight on v18.3. Compare Perfect Counter / Blunt for heavyDelayMs=260 vs 300. If 260 wins clearly, make it fixed; if it loses on late impacts, return to 300. Also an EX Wolf Fang fight for the Claw.


## SP-068 — Another unmapped PvP combo; cosmetic filter verified; rogue-heavy trial inconclusive (2026-11-20)

**User:** "new auto parry test". Reviewed frozen snapshots through 1795165947. Claude's concurrent v18.3/SP-067 change was present in source, installed/public builds and the newest two recordings; retain its independent NPC/player 260/300-ms rogue-heavy rotation. Findings primarily concern 1795165243 and 1795165947; earlier v18.2 histories were already reviewed in SP-067 and are used below only to check cosmetic filtering. Raw reports/player identifiers stay local.

**Latest PvP recording 1795165947:** finalized Disabled at 185.311 s, droppedLines=0, held=false. Config v18.3, PvP/PvE=true, range=8, offset=0, hold=120 ms, Hold Punches=true. **Zero automatic inputs**, five self-counter receipts and 87 negative-health observations. All five counters are unmatched/manual-origin-unknown; none can be credited to this auto-parry session. No runtime errors. Six queued cues were all the known shared heavy 102483335944327, and all six were cooldown-blocked. Two other mapped decisions failed facing and six were out of range. There were no LOS skips, so the cosmetic fix is not the cause of zero inputs. The 1,058 unmapped skips include frequent ambient/reaction animations and are not an attack count.

**Observed new style, player_set_d (actual fighting-style name unknown):** anonymous actor 2 has a distinct five-animation sequence. Same-actor adjacent transitions within 1.2 s: 1->2=22, 2->3=14, 3->4=8, 4->5=7; other transitions/repeats also occur. These are different IDs from the standard, shared NPC, player_set_b and player_set_c catalogs.

| Hit | Animation | Starts, all speeds | Matched normal-speed HitOut | Median (range) | Initial v18.4 seed |
| --- | --- | --- | --- | --- | --- |
| 1 | 80038210953396 | 35 | 22 | 251 ms (234–268) | 80 ms |
| 2 | 99203841295550 | 25 | 21 | 184 ms (166–216) | 0 ms |
| 3 | 111392139942881 | 20 | 15 | 100 ms (66–117) | Skip |
| 4 | 120956971139764 | 15 | 7 | 85 ms (65–101) | Skip |
| 5 | 121955051073038 | 10 | 5 | 267 ms (251–269) | 80 ms |

Matching uses the first same-actor HitOut within 600 ms and before the next candidate start; requiring >250 ms since the previous candidate start does not change these matches. Every matched start has speed 1.200. Other starts reuse the IDs at speed 1.000 or 2.000, without matched impacts under this method; their meaning remains unverified. Normal-speed start counts are 28/22/15/7/5. Local Blunt occurs immediately beside 19/18/7/6/4 of the respective matched HitOut events. This supports attack mapping; client receipt intervals are not server parry windows.

Three hit-3 HitOut events have nearby local Counter Parry receipts. All include multiple unmatched F presses: one before animation start (roughly 99–147 ms before) and another only 29–48 ms before HitOut. These do not establish a reliable reactive timing for a 66–117-ms hit. Preserve the established short-lead exclusion for hits 3/4. Initial seeds on hits 1/2/5 leave approximately 171/184/187 ms of median observed lead; they are trials, not calibrated success claims. The two frequent 2-second tracks 86878004337726 / 111852511691817 and common movement/reaction IDs stay unmapped.

**NPC v18.3 recording 1795165243:** finalized Teleport at 279.455 s, droppedLines=0, held=false. 29 automatic presses, 21 self-counter receipts and 36 negative-health observations. By mapped animation, local counter receipts within 600 ms of a press: rogue hit 2 11/15, hit 3 6/8, hit 5 4/5; rogue heavy 0/1. These are temporal associations, not blanket proof of server input acceptance.

**Rogue-heavy rotation is not decided by this test:** three heavy cues were queued. The first chose 260 ms and actually pressed at +267 ms (24.8827 s); F released successfully after 132.5 ms. No Counter/Perfect Counter/Blunt/Sharp/BlockEffect result followed within 600 ms. The other 300-ms and 260-ms cues were blocked by the remaining counter cooldown (~751/784 ms). Thus there is only one executed trial and no comparable 300-ms outcome. Do not declare 260 ms superior, convert it to fixed timing, or roll back Claude's trial from this evidence. The newest player fight had no rogue-heavy input at all.

**v18.2 cosmetic fix — verified observed behavior in retained logs:** LOS_OBJECT reports target_head_effect exclusions in 1795162254 (five distinct logged objects) and 1795164047 (one). Non-colliding thrown_effect exclusions appear in 1795162254 (two), 1795163589 (one), 1795164047 (one), and 1795164356 (three). These are object-log counts, not successful-parry counts. One remaining blocker in the long PvP capture is another character's HumanoidRootPart, which is not a cosmetic head effect; no broad live-character exclusion added. The newest NPC run only encounters already-supported Zvanishdummy/local-head objects; the newest player run has no LOS_OBJECT entries. Do not infer an increased success rate solely from the filter working. Long older histories remain detail-capped as documented in SP-067.

**Implemented / untested live — v18.4:** add player_set_d hits 1/2/5 at 80/0/80 ms plus user offset; recognize and skip hits 3/4. New IDs are player-only and require the observed normal speed 1.2 within 0.025, reusing player_set_c's scoped speed-check pattern. Speed-1/2 repeats are logged as unverified_style_speed rather than triggering F. Config and queued diagnostics identify the new style. Existing catalogs, player_set_c speed=1.175 behavior, cooldowns, range/facing, cosmetic LOS, optional punch holding and input cleanup stay intact. Preserve v18.3 rogue-heavy rotation exactly; no cross-game timings/remotes imported.

**Validation:** 534 mocked assertions pass: 88 for style D and independent NPC/player 260/300-ms rogue-heavy rotation; 64 cosmetic LOS; 65 style C; 189 shared-catalog/timing checks (updated only to expect v18.3's first 260-ms rogue-heavy seed); 60 earlier player-catalog/geometry; 36 standard policy; 26 ownership/target toggles; six settings/reload. Covers all new IDs/exclusions, speed rejection, player-only scope, additive offsets, range recovery, focus/PvP/manual/cooldown/stopped/typing/menu/death/ownership gates, punch holding and release cleanup. Source and staged/generated public/private builds compile. Exact-module transplant and hash checks preserve concurrent unrelated modules. No live Roblox control or gameplay input by this agent.

**Handoff:** reload v18.4 for this opponent's new style. Verify actual player_set_d inputs before assessing timing results. Retain the 260/300-ms rogue-heavy trial until comparable executed outcomes exist. Known shared-heavy cooldown and fast-hit reactive timing remain unresolved. Automatic diagnostics continue; no extra recording was required to establish this catalog gap.


## SP-069 — Set C counter receipts confirmed; recording predates v18.4 and hits detail cap (2026-11-20)

**User:** "another one". Frozen snapshot of history 1795166202. At read: RUNNING, elapsed 375.804 s, attempts=16, selfCounterEvents=15, damageEvents=158, held=false, droppedLines=5104. Detailed events stop at 181.7887 s. Do not combine the later totals with the earlier retained decisions to claim 15/16 successes. Raw snapshot and analysis remain local.

**Version/scope:** config contains the v18.3 260/300-ms rogue-heavy trial and player_set_c, but no playerSetD field. Therefore this is a v18.3 session, not a live test of installed v18.4. PvP/PvE=true, range=8, offset=0, hold=120 ms, Hold Punches=true. The retained main opponent uses player_set_c at normal speed 1.175. All 13 detailed automatic presses target that player/style. No new fighting-style mapping is needed from this retained section.

| Set C hit | Retained automatic presses | Counter receipts following the press |
| --- | --- | --- |
| 1 (0-ms seed) | 6 | 3 |
| 2 (20-ms seed) | 3 | 1 |
| 3 (20-ms seed) | 2 | 2 |
| 5 (100-ms seed) | 2 | 1 |
| Total | 13 | 7 |

**Observed positive evidence:** all seven counter receipts follow same-opponent HitOut within about 1 ms and an automatic F press 132–233 ms earlier. SELF_COUNTER shows one candidate attempt and inputConflicts=[] in each case. No unmatched F event occurs between the automatic press and its counter receipt; some later manual/unmatched presses occur after the counter and must not be counted as intervening. All 13 F_UP API calls report success (about 131–139 ms holds); no runtime/failed-input errors appear in retained detail. This supports working reactive inputs for set C, especially the two hit-3 examples; it remains client-side temporal attribution, not a general success guarantee.

**Do not classify every other press as a missed parry:** of the six without a counter receipt, four have neither same-opponent HitOut nor a local damage/block/counter outcome within 600 ms. One hit-1 press has a BlockEffect aligned with HitOut at +162 ms. One hit-2 press at 88.4777 s has HitOut at +164 ms without a local outcome; the Blunt at +550 ms aligns with the opponent's subsequent hit 3 (started 88.8089), so it must not be attributed to the hit-2 press using only a broad window. The hit-5 counter at 160.341 s is surrounded by other Blunt effects, so a counter receipt does not mean the player took no damage during that sequence. Five other retained counter receipts have no recent automatic candidate and are excluded from the seven above. The final three automatic presses and later counter receipts lie beyond the retained detail and cannot be individually assessed.

**Gates:** the full frozen footer reports 41 cooldown, 31 range, 20 attacker-facing, 12 self-facing, five manual-F, four approach-expiry and two stopped-track skips, plus 20 intentional late-hit skips and 48 unverified-style-speed skips. All 48 speed skips are visible in retained detail and use speed 1.000, distinct from the mapped normal 1.175. This does not justify widening the normal-speed guard. Known shared-heavy queues remain blocked by cooldown/facing; there are no retained rogue-heavy trial presses, so this recording cannot choose 260 versus 300 ms.

**Cosmetic fix remains verified:** retained LOS_OBJECT entries exclude target_head_effect, local_head_effect and non-colliding thrown_effect objects (Part, FX3 MeshPart and asrushPart). There are no line_of_sight skips in the frozen totals. No further LOS widening is indicated.

**Decision / validation:** findings-only update; keep gameplay source, timing constants and the installed v18.4 build unchanged. No new compile/mock run is appropriate for a documentation-only review. Analysis matches automatic attempts to actor-specific HitOut, counter events, intervening input and releases, and distinguishes later-combo damage from the earlier attempt. Neither 7/13 nor the capped 15/16 total should be advertised as an overall reliability estimate.

**Handoff:** v18.4 is already installed but must be reloaded to start a session containing playerSetD. The existing recording supports retaining set C's current timing; it does not test the newly added set D. The recurring 12,000-line detail cap is a recorder limitation: later attempts/outcomes become unavailable despite continued totals. A future bounded segmented/compact recording design should preserve per-attempt evidence; do not silently infer omitted outcomes or repeatedly retune from footer ratios. No extra test was requested as a prerequisite to this analysis.



## SP-070 — User reports worsening; equal-window comparison shows more lockout/input competition (2026-11-20)

**User:** "check again i think it go worse". Reviewed frozen snapshots of 1795166882 and 1795167356 and compared the complete first 180 seconds of each, with 1795166202 as older context. The user's report is supported by the latest encounter's reduced automatic activity and greater recorded health loss; do not dismiss it by quoting a counter/attempt footer ratio.

**Version/control check:** both new runs have byte-for-byte identical Config lines, including v18.4 playerSetD, set-C seeds 0/20/20/100 ms, range=8, offset=0, hold=120 ms, PvP/PvE=true, Hold Punches=true, normal-speed guard and cosmetic fix. Retained opponent attacks use set C; there are zero set-D animation starts in either retained segment. The v18.4 addition therefore did not trigger new inputs in these sections, and did not change set-C timing. This does not by itself prove absence of every runtime/game-state cause, but provides no basis for rolling back the new catalog as the explanation.

| Complete first 180 seconds | 1795166882 | 1795167356 (latest) |
| --- | --- | --- |
| Automatic F presses | 25 | 9 |
| Distinct counter receipts after recent automatic input | 12 | 5 |
| Of those, logger flags multiple recent attempts | 5 | 0 |
| Unmatched F-down events | 8 | 29 |
| Negative-health observations | 111 | 183 |
| Sum of observed negative health deltas | 131.92 | 154.24 |
| Decisions blocked by counter cooldown | 19 | 41 |
| Approximate time under recorded counter lockout | 42.09 s | 65.88 s |
| Five-second Counter notices | 4 | 9 |
| Counter=0.5 notices | 18 | 21 |

Health loss here is the sum of client health decreases, not a count of enemy attacks or a controlled damage benchmark. These are different encounter windows, not a randomized A/B test. Cooldown time integrates each recorded notice until its expiry, the next overriding notice, or t=180, matching the controller's replacement semantics; it is not simply notices multiplied by duration. Older 1795166202's same 180-second window had 13 presses, seven counter receipts, 141 negative-health events, 137.40 total negative deltas and ten five-second notices, illustrating substantial encounter variation.

**Attribution correction for dense sequences:** a counter can fall within 600 ms of two consecutive automatic presses. Count each SELF_COUNTER event once, not one success for every earlier press inside that window. A naive per-press search yields 15 qualifying presses for the middle run's first 180 s, but only 12 distinct counter receipts; five have candidateCount=2 / attribution=ambiguous. No 15/25 success claim is warranted. In the full retained middle segment there are 28 presses and 14 distinct counter receipts, not 17 independent successes from broad per-press windows. Keep that distinction in future analysis tooling.

**Latest five-second notices and F input source:** nine notices occur at 18.4134, 31.1956, 62.2288, 67.7953, 90.7269, 116.7101, 136.3098, 164.8915 and 178.3399 s. Every one follows an unmatched F-down by 82–448 ms. There is no prior automatic input for the first; the most recent AP press is 1.297–60.828 s earlier for the rest. None of these nine is temporally adjacent to an AP press. In the previous run, three of four five-second notices follow unmatched input; one is 64 ms after an automatic press. This supports increased input competition/lockout as a substantial contributor, not proof that manual input caused every cooldown. Unmatched means not associated with this AP pulse: it could be the user or another input source. Do not label it definitely manual or blame the user.

**Remaining behavior:** in the newest retained 214.4406 seconds, 11 AP presses produce six counter receipts with single recent candidates; one other counter is unmatched. Set-C hit 5 has three presses followed by three counters; hit 2 one/one; hit 3 two/one; hit 1 five/one. Do not call the other five presses five measured timing failures: several have no same-opponent HitOut/local outcome, and the one later Blunt near the last hit-1 input belongs after the immediate hit window. The preceding run includes mixed hit-1 counter/block/damage outcomes at overlapping observed lead times (~165–198 ms), providing no clean new seed. Retained F_UP calls succeed and no runtime errors occur. Median sampled pings overlap; the latest does not show an obvious latency increase.

**Gates and cosmetics:** latest first-180-s facing skips total 31 (22 attacker, nine self), versus 18 (15/three); range skips are 22 versus 37; six manual-F/already-held skips appear versus zero. These may further reduce response coverage but do not prove those geometry decisions are wrong. Forty-one cooldown skips include shared-heavy and light cues; they are not forty-one independently mistimed parries. The one latest LOS skip is collidable Workspace.Thrown.Debris and is intentionally preserved. No evidence here supports ignoring real debris or weakening the scoped cosmetic fix.

**Capture limits:** middle run finalized Disabled at 240.274 s, footer 35 attempts / 18 counter receipts / 143 health decreases, 1913 dropped lines, detail ends at 200.8065 s. Latest snapshot was RUNNING at 275.622 s, footer 11 attempts / seven counter receipts / 189 health decreases, 1116 dropped lines, detail ends at 214.4406 s. This is why the comparison uses t<=180 only. Later live totals are not interchangeable with frozen detailed evidence. No raw reports or player names are published.

**Decision:** findings-only update; keep installed/source v18.4 unchanged. Latest overall protection was worse, but no isolated code regression or defensible timing correction is established. Do not claim the AP is reliable merely because some counters work, disable cooldown protection to inflate inputs, or retune set C based on omitted/cross-attributed events. For an interpretable future run, AP should be the sole F-input source (normal movement/attacking can continue); this is guidance, not a prerequisite or requested confirmation. Existing recordings were analyzed first. No new build/tests are needed for this documentation-only change. Next engineering priority remains preserving compact per-attempt evidence beyond the detail cap rather than repeatedly inferring outcomes from footer totals.

## SP-071 — Scientific Engineer [2]: tablet click minigame; Auto Job v1.4 (2026-11-20, Claude)

**User:** "new job recorder". Read Job Recorder 1795169598 (brief) and 1795169603 (108 s, place 135537125271116), plus the Auto Job v1.3 log running alongside. Raw captures stay local.

**Evidence (Verified from captures):**
- **Level-up:** Auto Job v1.3 finished Scientific Engineer [1] 12 times in a row, +200 Zeni each (578 -> 2,778, ~12 s per job, Restart Quest pressed each time).
  - The next window read **"Scientific Engineer [2]"**, "Pay: $500", "Help the company to develop a new system", Progress 0/1.
- **[2] uses a different prompt:** v1.3 pressed the nearest "Capsule Minigame", the laptop (`Laptop1...CapsuleMinigame1`), 11 times and nothing opened. Then it switched to `Workspace.Map.Tablet.Part.CapsuleMinigame2`, which opens **`PlayerGui.CapsuleMission2`**:
  - `Main.Timer` reads "Time: 60s(Aim for over 300)" and counts down once a second.
  - `Main.Counter` reads "Counter: n"; there is also `Main.Cancel` "[ Quit ]".
- **The counter counts mouse clicks:**
  - 356 recorded MouseButton1 clicks (anywhere on screen, mostly unprocessed world clicks) gave 344 counter steps, about 1:1.
  - Each step came 51–115 ms after a click (typically 85–100 ms).
  - The user clicked about 4 per second.
- **No completion recorded:** the two long attempts closed early, at counter 133 (13 s left) and 150 (24 s left). Six other openings lasted 0.7–7 s.
  - v1.3 was pressing the tablet prompt every 1.2 s the whole time, so the cause of the early closes is unknown. The repeated presses are the main suspect, and `Workspace.Thrown.SPCapsule` also appeared before some closes.
  - Zeni stayed 2,778 and progress stayed 0/1, so the pay and the meaning of "over 300" are unverified.

**Implemented / untested live — Auto Job v1.4:**
- The job level picks the prompt: `[n]` presses `CapsuleMinigame<n>` (laptop for [1], tablet for [2]), else the nearest one.
- While CapsuleMission2 is open, the prompt is left alone. Auto Job mouse-clicks at **8 per second** on open screen: the middle, or three fallback spots, skipping the hub window and both [ Quit ] buttons.
- [2] counts as finished only when the job window's progress is complete. A close without progress is logged (`MINIGAME2 closed: ... counter, clicks, last timer, progress, zeni`) and the tablet is pressed again after 1.5 s.
- An idle counter (5 s without change while clicking) is logged once.
- [1] (key minigame) and Floor Staff are unchanged. Source and both variants compile; no live test yet.

**Next step:** run Auto Job at the Scientific Engineer board with the job at [2]. Check that the counter passes 300, how the minigame ends, the pay, and whether 8 clicks/s registers 1:1.

## SP-072 — Auto Job v1.5: [2] minigame clicks at full speed (2026-11-20, Claude)

**User:** "full blast the auto click".

**Implemented / untested live:**
- The Scientific Engineer [2] click minigame (SP-071) now presses the mouse on one frame and releases it on the next. That is ~30 clicks/s at 60 FPS (`CLICK_RATE=30`, hold 0), up from 8/s with a 60 ms hold.
- Nothing else changed. Source and both variants compile.
- Unknown: whether the server counts every click at this rate, and whether it reacts to it.
  - The `MINIGAME2 closed` line logs clicks vs counter, so a capped count will show as counter << clicks.
  - If it caps, lower CLICK_RATE to the rate that still counts 1:1.

## SP-073 — Auto Parry v18.5: ping compensation (2026-11-20, Claude)

**User:** "check the auto parry i think its bugged", then "we need like a ping compensation".

**Evidence (Verified from captures):**
- **Today:** histories 1795196868 and 1795196902 (Inner World Shadow, plus one Warrior Clan Rogue) had 26 F presses, **0 counters** and 19 Blunts. Ping was ~187 ms.
- **The presses weren't late on the client.** Shadow presses still came a median ~182 ms before the attacker's HitOut, the same as in the good runs.
- **Earlier Shadow runs:**

  | History | Ping | Counters |
  |---|---|---|
  | 1795126135 | 78 ms | 60/78 |
  | 1795133566 | 71 ms | 33/50 |
  | 1795156077 | 105 ms | 56/61 |

- **What decides the outcome is lead minus ping.** Grouped by that value across all five histories:
  - 20–140 ms: 149 of 182 countered
  - 0 ms or below: 0 of 20 (all Blunt or no outcome)

  The swing reaches us one-way late and our F reaches the server one-way late, so the server sees roughly lead − RTT.
- **Blunt is a hit, not a block:** across 40 histories, 1,806 of 2,049 self "Blunt" effects were followed by health damage within 0.4 s. A late press also triggers the 5 s cooldown.
- **All seeds were tuned at ~100 ms ping:** every PvP and NPC history from 01:33–03:19 had a median ping of 99–107 ms.

**Implemented / untested live — v18.5:**
- New setting **Ping Compensation**, on by default, under "Don't Parry If".
- Ping is read from `Stats.Network.ServerStatsItem["Data Ping"]:GetValue()` at most every 0.5 s.
- Every mapped cue, NPC or player, shifts by (ping − 100) ms, capped at −80/+250 ms. Higher ping presses earlier; lower ping presses later.
- **Skipping:** if the delay can't absorb the shift and the cue would still be 60+ ms short of its tuned lead, it is skipped with `SKIP reason=ping_too_high shortMs=`.
  - At ~187 ms ping, the standard hits 1/2 (seed 40 ms) press immediately (about 35 ms net lead, borderline).
  - Hit 5 (seed 0) is skipped.
  - Heavies press about 87 ms earlier.
- `CUE_QUEUED` lines now carry `pingMs=` and `pingShiftMs=`; the Config line gains `pingComp=`.
- The PvP seeds (GPT's) themselves are unchanged. Compensation is a no-op at 100 ms and can be switched off.
- Source and both variants compile. The shift and skip arithmetic was checked offline. No live run yet.

**Next step:**
- Fight the Shadow at high ping and check counters and the `ping_too_high` skips.
- Also test at normal ping (~100 ms) to confirm nothing changed there.

## SP-074 — Item/CEXP grant-path inspection; bounded daily-claim test prepared (2026-11-20)

**User objective:** determine whether real items or CEXP can be granted, beyond changing Visuals. **Observed source, not a verified grant vulnerability.** The user ran the standalone private source inspector. Seven selected client scripts saved successfully in place `102793290484692`: Primary, NewMenu, NewDaily, Dialogue.LocalScript, Inventory.Handler, Inventory.Passive and Inventory.Quest. These are decompiled client scripts; they do not expose server validation. Raw captures, account identifiers and captured source remain local.

**Concrete reward path:** NewDaily uses `Requests.DailyReward:FireServer("State")` and `:FireServer("Claim")`. Its incoming handler receives `(kind, state, rewards)` and recognizes `State` and `Claimed`. The button checks `claimed`, `played` and `need` locally and applies a three-second debounce. Whether the server independently enforces those checks is unknown. The request supplies no reward name or amount. The UI labels include CEXP=5,000, Zeni=10,000, PowerLevel=5,000, EventWins=10, Capsule=1, DragonBall=random, Namekian dragon balls and Spins=5–10. These are display mappings, not verified payout amounts or a selectable arbitrary-grant API. Pending items may be described as arriving on spawn.

**Correction to the SP-047 interpretation:** the similarly keyed CEXP tables/functions match NewDaily's reward labels and icon renderers. Calling these leaderboard handlers was unsupported; SP-V010's reward-label interpretation is supported by the new source. They are not the player's numeric CEXP balance. Absence of a balance from these inspected tables is not proof that every client data path lacks it.

**Inventory path:** Inventory.Handler serializes numeric hotbar slot -> tool-name mappings into JSON for `UpdateSlotData`. It resolves actual usable items by finding existing Tool instances in the character or Backpack. This establishes layout persistence, not item creation. Primary/NewMenu/Inventory also use `FetchData:InvokeServer()` with no arguments for profile reads. The inspected PlayerChoices call sites update menu preferences; no item/CEXP grant signature was established there. No mutation of these paths has been attempted.

**Implemented / untested live:** separate private `SparklingRewardTest.lua`, not a hub module. It reads daily state and selected profile/tool fields, then tests one early claim or one duplicate claim. If initially eligible, it makes one normal control claim and only tests a duplicate after a fresh state reports `claimed=true`. Maximum two Claim requests; at least 3.5 seconds between them; no caller-provided amount/item; no loops of claims, hooks, profile mutations or hotbar changes. Missing state or a timer within 20 seconds of eligibility causes no claim. Session change stops claims; a one-minute local rerun guard prevents accidental rapid repetition. Existing hub and concurrent work remain unchanged.

**Validation:** standalone Luau compilation and 51 mock assertions across eight scenarios passed: early, already claimed, legitimate-control-then-duplicate, missing state fields, near timer boundary, state timeout, unconfirmed control, character change. Checks include request counts, listener cleanup, guard release and exclusion of character identity from profile snapshots. This does not test server behavior. Read-only discovery already ran; the claim test has only been installed for the user to execute.

**Output / next handoff:** `AnomalyHub/universal/sparkling_reward_test_latest.json` and timestamped history. Review `baselineEligibility`, `test`, phase-tagged incoming state/reward events, and FetchData/tool deltas together. A FireServer return is not acknowledgment. A Claimed event alone does not establish a persistent grant; concurrent gameplay and uncorrelated state events can confound attribution. A normal eligible payout does not establish a bypass. Rejoin persistence/use remains a separate verification if an unexpected grant is observed. Reused the bounded local source-capture approach documented for Hunter X Enter's Nen capture; all remote signatures here come from Sparkling's captured code.

## SP-075 — Early daily-reward claim produced no grant (2026-11-20)

**User:** ran the standalone SP-074 reward test. **Verified live negative result for this early-claim attempt; not proof that every reward path is secure.** Place `102793290484692`, test v1, elapsed 6.081 s, exactly one Claim request. Frozen local evidence: reward test history ending `1795198156`. Raw profile keys/tool names remain local.

**Before:** server State reported `claimed=false`, `played=630`, `need=1800`, `lobby=false`, `pending=false`, day 1. The scheduled reward for that day was Capsule, not CEXP. The test sent `DailyReward:FireServer("Claim")` at t=0.2331 s, about 19.5 minutes before eligibility. At t=0.3299 s an incoming State still reported `claimed=false`, `played=630`, `pending=false`. A separate State request at t=3.7477 s received `claimed=false`, `played=635`, `pending=false` at t=3.8474 s. No Claimed event or reward list was received.

**Before/after checks:** both no-argument FetchData calls returned successfully. Selected balances and all four observed tool types/counts were identical. No new capsule or other item appeared in the retained observation window. CEXP was absent from the returned profile keys, so there is no direct before/after CEXP measurement. Do not label this a tested CEXP-grant rejection or a persistence test.

**Conclusion:** bypassing the client button's wait check with the known Claim signature did not grant the scheduled reward. The prompt State response and subsequent unchanged state/items support server enforcement of eligibility in this case. There was no explicit textual rejection; the observed outcome is unchanged unclaimed state, no reward acknowledgment and no measured grant. A duplicate claim was not attempted because the baseline was ineligible. No hub or test-script changes are justified by this negative result.

**Next handoff:** mark early claim unsuccessful and do not repeat the same ineligible call expecting a different result. Duplicate-claim handling remains untested; the existing bounded script can test it once the account is already claimed or legitimately eligible, without changing request signatures. Arbitrary item/CEXP granting is still unverified. No claimed grant exists that would justify a rejoin/use persistence check. Documentation-only update; prior compilation/mocks remain applicable to the unchanged test.

### SP-075 follow-up — Duplicate-only test stopped before claiming (2026-11-20)

The user ran standalone duplicate-only v2 in the same recorded place. The server reported played=790 of need=1800, claimed=false, pending=false: 1,010 seconds (16:50) remained at capture time. The script sent zero Claim requests and exited after 0.218 seconds with an explicit not-eligible status. This verifies the local eligibility guard, not the server's duplicate-claim protection. No after-test snapshot exists because no claim was attempted. Duplicate handling is still untested; do not count this as a rejected duplicate or another early-claim attempt.

The installed standalone script is now SparklingDuplicateRewardTest.lua. It compiles and passes 50 mock assertions across eight scenarios. It retains the maximum of two claims (eligible control plus duplicate), but skips all early claims and displays the remaining time. It uses the same sparkling_reward_test_latest.json/history outputs; version=2 and mode=duplicate_only distinguish it. No hub changes. Next check: run it again when the game's daily-reward display is ready or already claimed. The recorded 16:50 is a historical reading, not a live countdown or guaranteed wall-clock eligibility time.

## SP-076 — Combat/training progression source capture prepared (2026-11-20)

**User objective:** trace how normal combat/training leads to PL and CEXP gains while the daily-claim test waits for eligibility. The seven SP-074 captured scripts do not establish a combat progression award request. Primary handles incoming effects and miscellaneous callbacks; its inspected outgoing calls include profile reads, Oozaru and RPSChoice, not an identified PL/CEXP award. The reward-only capture omitted the local character's scripts. Its manifest does identify Modules.ActionCheck, SpeedCheck, TalentModule, SkillGivingModule, MissionQuestModule, QuestGivers, GameGuide and TouchInput, plus the client Actions folder. Names are leads, not verified behavior.

**Implemented / untested live:** standalone private SparklingProgressionCapture.lua. Reads local character scripts, selected INvokes callbacks, those eight modules, Actions, own tools and current mission/feedback scripts. Also captures matching in-game guide Words attributes. Maximum 40 source targets, three decompiler workers, 45-second overall deadline; no replacement workers for blocked decompilers, late results ignored. Incremental local manifests expose unavailable sources and limits. No requests, hooks, requires/execution of captured modules, profile mutation, gameplay input or hub replacement. Reuses the bounded source-capture approach referenced in Hunter X Enter notes and SP-074; no other game's remote semantics transferred.

**Validation:** standalone Luau compilation passed; installed file hash matched the prepared artifact. No live run/result yet. The user executes scripts themselves. No generated public/private hub changes or corresponding rebuild required. No server-side code is accessible merely because a client module is captured.

**Handoff:** output AnomalyHub/universal/sparkling_progression_sources_latest.json and timestamped manifests/source files. Read results and coverage first, then trace exact outgoing call sites and their arguments to existing combat recordings. Distinguish server effects/profile notifications from requests that award progression. Known activity correlations are not an exact gain formula: Shadow window had no CEXP increase; mixed fights/jobs windows cannot isolate per-hit CEXP, and the tracker sums of upward PL ticks overstate net growth. Raw captured source and player paths remain local.

## SP-077 — Progression capture reveals server-oriented reward code, not a client grant endpoint (2026-11-20)

**Capture (observed source, read-only)**
- **Run:** the user ran SP-076 in place `102793290484692`.
  - All 25 selected scripts saved; status complete, elapsed 0.334 s.
  - Neither the source-target limit nor the deadline was reached.
- **Guide capture** hit its 15-topic cap, so guide coverage is partial.
- **Nothing was attempted.** This was a capture only, not an attempted progression grant.
- **Raw evidence:** timestamped progression-source history ending `1795198703`. Captured source and player paths remain local.

**Combat requests (own-character Input)**
- `performAction(action, ...)` is `Requests.Action:FireServer(action, ...)`.
- **M1 on the ground** sends `MouseButton1`, a movement/held-key boolean and the look vector. The buffered-M1 code resolves that boolean to held W, which settles the earlier ambiguity.
- **M1 in the air** uses `AerialM1`.
- **M2** sends direction variants and look data.
- **Keys:** M / J / L map to Meditate / MeditationTrain / ShadowSpawn.
- **Other sends:** Input also sends WallSlam collision data and an airborne Update. Neither is an established progression-award API.
- **No requested amounts:** none of these call sites supply a desired PL/CEXP increment.
- **Missing from the captures:** the server dispatcher and any reward calls that follow an attack.

**CEXP / mission rewards (`Modules.MissionQuestModule`, replicated)**
- **The routine:** it contains server-oriented `Claimed(player, actor)` code.
  - It requires `ServerStorage.Modules.MainModule` and gets `ServerStorage.DataFolder[player.Name]`.
  - It writes the field named **CombatEXP**, not CEXP.
- **Decompiled reward expressions:**

  | Reward | Expression |
  |---|---|
  | CEXP | `random(10,20) * MainModule.GainBoost(player,"CEXP")` |
  | CEXP, Tamagami branch | `200 * GainBoost` |
  | Zeni | `random(10,100) * GainBoost(player,"Zeni")` |

- **Tamagami branch:** uses RandomEventReward, falling back to a server DragonBalls collection, and clones the selected item into the player's Backpack.
- **`3rdShadow` branch:** increments GuardsBeaten.
- **Limits:** these are code observations. They are not live payout measurements, and they don't prove the active server invokes this version.

**Decompiler caveats**
- **Duplicate block:** the reconstructed MissionQuestModule repeats a generic CEXP/Zeni reward block after the branches.
- **Wrong variable:** it then appears to iterate and JSON-encode the player variable where decoded quest data would be expected.
- **Input.LiveLoop:** also contains implausible reconstructed variable uses.
- **So don't:**
  - add the apparent duplicate reward blocks into an exact payout;
  - claim a double-award bug;
  - conclude whether 3rdShadow does or doesn't receive the later reward.

  The caller and the reconstructed control flow need independent corroboration.
- **Still valid:** the earlier Shadow-only CEXP observation (no CEXP gained) stands for that tested window.

**Skills / talents**
- **SkillGivingModule:**
  - It reads ServerStorage data and calls the server's `MainModule.GrantSkill`.
  - Its EventSkillGained use is **InvokeClient**: the server selects and offers a skill, and the client returns an acceptance.
  - It is not evidence of an InvokeServer grant request.
- **TalentModule:**
  - It checks server CombatEXP and other stat requirements.
  - Some descriptions mention gain bonuses, but no numerical combat GainBoost implementation was captured.
- **Client calls don't write server data:** don't call these module functions on the client expecting a write.

**PL and what wasn't found**
- **Searched:** the 25 progression scripts plus the 7 SP-074 reward scripts.
- **Not found:**
  - a combat `PowerLevel.Value = ...` award assignment;
  - the MainModule implementation;
  - the server Action handler;
  - any call to `MissionQuestModule.Claimed`.
- **Guide:** battle power comes from fighting, and latent power affects gains. It gives no exact combat formula.
- **Absence is scoped to the inspected code.** It is not proof that no vulnerable path exists.
- **ReplicatedStorage exposed code meant for server execution.** It did not expose the server-only instances that code references, or any way to run it on the server.

**Decision / handoff**
- **Result:** the source review established a concrete CEXP/mission reward routine and separated it from attack requests.
- **Not performed:** no grant test, remote mutation, module execution, gameplay input or hub modification. This is a documentation-only update; captured code was not compiled.
- **Open:** the server caller and validation for `Claimed`, plus the hidden GainBoost and combat PL logic. Existing recordings can correlate normal outcomes but can't reveal that server code.
- **Don't** invent a remote command named Claimed just because an exported module function has that name.
- **Separate:** the daily duplicate test (SP-075 follow-up) is still waiting for eligibility.

## SP-078 — Focused normal NPC reward trace prepared (2026-11-20)

**User objective:** trace one normal NPC defeat and reward after SP-077 found reward code but no caller. **Implemented / untested live:** separate private SparklingNpcRewardTrace.lua. Requires a loaded Sparkling hub, shared outgoing hooks free, and one living NPC with a direct PowerLevel child within 60 studs in Workspace.Live. Selects the nearest qualifying non-player model and announces its name. Auto Parry/other capture owners must be off; the script refuses to take their hooks. It reuses BeginSparklingRemoteObservation from the combat/job recorders rather than installing a separate hook stack. No hub modules or generated builds changed.

Records normal outgoing requests/returns from the initial ReplicatedStorage remote inventory; incoming events; selected DataUpdated progression fields; target health, death/removal candidates and knockdown markers; self/target state samples; tool counts/additions; two explicitly tagged FetchData() reads before/after. Sends no attacks, grant/claim requests, replayed actions or paid Varrik query. Drops incoming Cooldown/CooldownNT/StopDash and Effects not directly referencing self/target, with counts. Newly added remote instances are flagged as coverage gaps. A hook RETURN is not server acceptance, duplicated direct/namecall observations must be deduplicated, and NPC death/removal is not proof of local kill credit.

Stops 20 seconds after target death/removal candidate, on rerun, hub unload, character/recorder-owner change, error or five-minute limit. Saves numbered JSONL chunks every five seconds and at stop, rotating at 1,200 events/500 KB so earlier evidence is retained. Overall cap 50,000 events/10 MB; oversized records and shortened strings are reported. Manifest: AnomalyHub/universal/sparkling_npc_reward_latest.json plus timestamped manifest and _partN.jsonl files. Source is installed privately; raw evidence stays local.

**Validation:** standalone Luau compilation and 45 mock assertions across seven lifecycle scenarios passed: target death with 20-second aftermath, five-minute timeout/chunk rotation, ownership loss preserving another callback, rerun stop, busy hooks, unavailable hooks, disk failure cleanup. Installed file hash matches the prepared artifact. These are mock checks, not live recorder or reward validation.

**Next handoff:** user turns off Auto Parry/other recorders, stands near one enemy NPC, runs the standalone script and fights the announced target normally. Analyze the manifest, all referenced parts, coverage and candidate-defeat window before inferring any client completion request. CEXP may remain absent from profile responses; lack of a measured CEXP delta is not proof of no gain. Do not conflate ordinary combat actions, recorder-generated profile reads, ambiguous nearby-player kills or server effects with a grant endpoint.

## SP-079 — Normal daily capsule granted; duplicate claim gave no second reward (2026-11-20)

**Verified live outcome for this test:** duplicate-only v2 completed in place 110405289082842, 9.717 seconds, two Claim requests. Frozen local history ending 1795199722. Baseline State: played=1800/need=1800, claimed=false, day 1 reward Capsule, pending=false. Baseline inventory contained no breakthrough capsule.

**Positive control:** normal Claim at t=0.2178 s received Claimed at t=0.3331 s, reward list containing Capsule, claimed=true. Fresh State at t=3.8335 s confirmed claimed=true. The after-control inventory contained one Breakthrough Capsule (Event). This verifies an ordinary eligible reward acknowledgment plus item appearance; persistence/use after rejoin was not tested.

**Duplicate attempt:** second Claim at t=3.9497 s, 3.732 seconds after the first request and after confirmed claimed state. At t=4.0650 s the server sent State, still claimed=true/pending=false, without a reward list. A separate later State again confirmed claimed=true. No second Claimed event appeared. Final inventory still contained exactly one capsule and the same other tools. All three profile reads succeeded; Zeni, PowerLevel and EventWins were unchanged throughout. CEXP was absent from profile responses and this was a capsule reward, so this is not a direct CEXP duplication measurement.

**Conclusion:** the straightforward sequential duplicate daily claim failed; normal eligibility and repeated-claim state were enforced in the observed tests (SP-075/SP-079). Do not label the normal first capsule as a successful dupe, or claim that every possible reward route is secure. No further repetition of the same sequential call is indicated. No code changes or new tests were run for this findings-only update. Raw inventory/profile data remains local.

**Handoff:** daily claim branch has a successful legitimate control and negative early/duplicate results. Continue the separate SP-078 normal NPC reward trace; this daily test provides no NPC defeat/caller evidence. The user's report here referred specifically to the duplicate test, not completion of the NPC recording.

## SP-080 — NPC trace v1 interfered with namecalls; results invalid; v2 queues serialization (2026-11-20)

**Failure caused by recorder, not evidence of game reward validation.** Reviewed three local v1 histories ending 1795199567, 1795199772 and 1795200162. Their request logs contain respectively 205/100/8 OUT observations and zero RETURN observations. Every available before/after FetchData snapshot fails with "JSONEncode is not a valid member of RemoteFunction ReplicatedStorage.Requests.FetchData". These captures cannot support negative grant/remote conclusions or clean combat progression attribution.

All selected Thug5 and recorded no target health changes. The first ends at 87.161 seconds after a removal-from-Workspace candidate at 66.812 seconds, not a confirmed kill. The middle manifest is a frozen recording snapshot at 97.307 seconds; the last ends at 9.781 seconds, Stopped by rerunning script. Incoming profile/effect updates still arrive, but do not validate forwarding of the instrumented requests. Do not count NPC removal as local defeat credit, or use this run to tune combat. Latest observed v1 run was stopped.

**Root cause reproduced:** the shared hook invokes its observer callback before forwarding to the original __namecall. The recorder performed HttpService:JSONEncode (and potentially other Instance methods) synchronously inside that callback. This can overwrite the current namecall method, so the original tries JSONEncode on the RemoteFunction/RemoteEvent instead of InvokeServer/FireServer. A new dispatch-state mock reproduces the old recorder changing a normal forwarded attack to JSONEncode. The earlier 45 lifecycle mocks did not model this engine dispatch state and were insufficient; retain this failure and add that regression case.

**Implemented / untested live — standalone v2:** outgoing observer performs only bounded plain-Lua table copies into a queue. All serialization, Instance inspection, profile filtering and file writes happen later outside the callback. Original observation timestamps are retained. Maximum pending queue 2,000; overflow is flagged and ends capture. Baseline FetchData must succeed before the trace announces recording; failure releases hooks and stops. Loading v2 stops an active old v1 before starting; rerunning v2 stops it normally. Shared hub hook code, auto-parry and unrelated Claude changes are unchanged.

**Validation:** old v1 fails the new regression with "Recorder changed the forwarded attack method to JSONEncode". V2 passes 65 assertions across eight lifecycle/namecall scenarios: no engine-method calls in the outgoing observer, a normal attack forwards once, before/after profile responses stay intact, baseline-read failure aborts, and cleanup/ownership/chunking cases continue to pass. Standalone Luau compiles; installed v2 matches prepared hash, with hash guard preserving concurrent edits and a local v1 backup. This remains mock validation until a new v2 capture shows successful outgoing returns/profile reads during a real fight.

**Next handoff:** one new normal NPC trace is necessary because recorder interference invalidated v1. User keeps Auto Parry/other recorders off, stands near the intended NPC and runs the same installed loader. Inspect version=2, baseline profile success and OUT/RETURN coverage first. No new grant calls or guesses at a Claimed remote were added. Daily reward results SP-079 were from the separate daily test and are not inferred from these NPC traces.

## SP-081 — V2 NPC trace forwards correctly; normal fight gains observed without a separate claim (2026-11-20)

**Verified live recorder behavior / observed reward sequence:** v2 history ending 1795200321, place 110405289082842, target Thug5 initially 26.31 studs away, 70.261 seconds, stopped by rerunning. One part, 1,052 events, no declared limits, ownership loss, new missed remotes or callback errors. Both before/after FetchData snapshots received successfully. All 262 outgoing calls have matching RETURN observations by call ID; all captured request paths are namecall, with no direct-path duplicate observations. This validates the repaired callback on this run; RETURN still does not itself acknowledge a server reward.

**Outgoing inventory:** 250 Action calls (125 MouseButton1, 24 MouseButton2, 14 AerialM1, 46 Defend, eight Rush, two SetSpace, 26 Update, five ZVanish), three Dashed, seven WallSlam and two recorder-tagged FetchData reads. No Quest/Mission/reward-claim/completion request appears. Initial remote inventory observed 64 incoming events; 127 noisy notices and 207 unrelated Effects were intentionally omitted. Incoming capture therefore is not a full server-event transcript; outgoing capture includes the loaded remote inventory.

**Defeat/reward timeline:** target health changed 41 times, reaching 1 HP at t=65.5598 s. Dead child added at 66.2409 s; no Humanoid.Died/zero-health or removal candidate was recorded before manual stop. At 66.4411 s (about 200 ms after Dead), DataUpdated changes Zeni from 1,516 to 1,648: **+132**. The final successful profile read confirms that amount. Ordinary attack/airborne actions surround the event; no separately exposed completion claim was observed. This supports server-driven reward processing for the observed fight, not proof of every possible server path or of a directly callable grant endpoint.

**PL/items/CEXP:** successful profile reads show PL 13,389.348080443497 -> 14,524.645515763334, net **+1,135.297435319837** across the encounter. Incoming profile updates already reached the final PL at 66.1238 s, before Dead and the Zeni update: do not call the whole increase a kill-completion bonus. Tool counts are identical before/after; Combat additions are equip-related and are not new rewards. No CEXP/CombatEXP field appears in either selected profile snapshot or the 82 selected profile-event observations, so no direct CEXP delta was measured. No rejoin persistence check was performed. Do not derive CEXP from Zeni or treat +132 as confirmation of the decompiler's apparently duplicated award blocks; multipliers/control flow remain unknown.

**Small recorder correction — v2.1, implemented / untested live:** target Dead child now starts the same 20-second aftermath timer while retaining the candidate label; Knocked alone does not. The game held this defeated target at 1 HP, explaining why v2 did not auto-stop. The trace was manually stopped successfully, so this does not invalidate its data. Same loader; an older active recorder is stopped before replacement. No additional user test is required for this finding. Standalone compiles and 73 assertions across nine lifecycle/namecall scenarios pass, including a new Dead-marker-at-1-HP case. Installed file hash verified; hub/shared hook/gameplay code unchanged.

**Handoff:** normal NPC combat and reward sequence is now captured with functioning hooks. It reveals no extra claim call to replay. Exact server combat PL formula, GainBoost implementation and CEXP amount remain unknown; passive client evidence alone cannot fill those gaps. Daily tests SP-079 separately found a normal capsule grant and unsuccessful sequential duplicate. Preserve these separate outcomes rather than asking for another identical capture without a discriminating question.

## SP-082 — Reported +190 CEXP after planned single-bandit test; recording coverage insufficient (2026-11-20)

**User-reported result:** user gave a baseline of **568 CEXP**, said they would fight one bandit and check again, then reported **758** and uncertainty whether a recording saved. Arithmetic delta is **+190 CEXP**. This is a reported before/after result for the planned test window, not an instrument-verified payout. Defeat/kill credit and absence of other reward activity were not independently confirmed.

**Local evidence inspected:** at review, sparkling_cexp_value.txt still contained 568 with timestamp 1795201152; sparkling_cexp_history.txt ended with the same 568 reading. Neither file contained the reported 758 endpoint. The existing baseline history spans a much longer prior window, so its accumulated counters must not be attributed to this one fight. No tracker/cache values were edited to manufacture corroboration.

**Recording status:** the newest dedicated NPC reward manifest was still SP-081 history 1795200321_6950FBAC (v2, Thug5, stopped by rerun), not a new bandit trace. The generic combat capture was still from 2026-11-17. Auto-parry diagnostics v7 session beginning 1795200120, place 94737939156598, was still RUNNING and saving totals, but had reached its 12,000-event detail limit. Retained event detail ended around elapsed 313.0046 seconds; the cached 568 baseline was at session offset 1,032 seconds. Thus detailed capture had already ended before baseline. At the first inspected snapshot elapsed was 1,472.960 seconds with 47,838 dropped lines; increasing totals do not recover missing events. The logger was not stopped or altered during this review.

**Interpretation / confidence:** arithmetic is certain, the baseline has local-file corroboration, and the endpoint is user-reported. This supports a reported CEXP increase during ordinary combat activity. It does not establish a fixed 190-CEXP-per-bandit rate, whether the award happened on hits or defeat, the server multiplier, a PvP reward rule, or a self-grant method. Do not combine this window with the earlier SP-081 NPC trace or infer missing requests from capped diagnostics.

**Handoff:** retain this result without requiring an identical repeat just to restate the delta. If precise reward attribution is needed later, start a fresh focused NPC trace before combat, verify it is actually collecting, and pair it with before/after CEXP readings and explicit target/outcome information. Never use an already capped auto-parry session as full event evidence. No gameplay code change or new live test was performed; this update records inspected files separately from the user's reported endpoint.

## SP-083 — PL focus: profile and live power differ; combat growth precedes defeat (2026-11-20)

**Scope / evidence:** user explicitly set CEXP aside to focus on Power Level. Reanalyzed the already valid SP-081 v2 capture, history 1795200321_6950FBAC, place 110405289082842, Thug5. This is new analysis of existing live evidence, not a new run or a current reading of the user's character. No gameplay inputs, remote calls, changes to the hub, or paid checks were made.

**Two PL readings:** successful before/after FetchData profiles show 13,389.348080443497 -> 14,524.645515763334, net **+1,135.297435319837**. The 100 sampled SELF_STATE records start at live Character.PowerLevel 10,711.478464354797 (t=0.3148) and end at 11,619.716412610667 (t=69.5755), net **+908.2379482558699**. At both endpoints live PL is exactly 80% of the corresponding profile value. Pairing each live sample with the latest preceding profile update yields 79/100 ratios within 0.000001 of 0.8; the full range is approximately 0.800000–0.801266. Samples and profile delivery are asynchronous; do not infer a new multiplier from the small residuals.

**Growth timing:** 82 PROFILE_EVENT records contain 74 strictly increasing PL changes and no decreases relative to the initial profile. First increase: t=5.2764 (+6.69125). Last incoming profile increase: t=66.1244 (+21.20604), before the Dead marker at t=66.2409. Observed profile increments range from about 6.69125 to 21.27483; a delivered update need not represent exactly one hit. No additional profile growth appeared between that last change and the final read. This confirms growth throughout this recorded combat window, not a single defeat payout, and does not establish which individual action earned each increment. The earlier SP-081 last-value time refers to the incoming request observation (~66.1238); the PROFILE_EVENT is timestamped slightly later.

**Meaning / limits:** live Character.PowerLevel and profile PowerLevel must be labeled separately. In this run the live/profile difference is real; its cause is unconfirmed. The live health went from full to roughly 55% while the endpoint ratio remained 0.8, so a simple current-health fraction does not explain these endpoints. This does not rule out health, fatigue, form, output, or other modifiers in the actual formula. Re-searching the 25 progression and seven reward source captures found no assignment explaining this 80% relation or the server combat PL gain formula. Guide descriptions and incidental constants do not prove causation. Neither profile reads nor this ratio prove save/rejoin persistence, a universal base/effective mapping, or an arbitrary self-grant route. Do not scale other players' ESP by 1.25 based on this one character/run.

**Measurement pitfall confirmed in source:** SparklingCexpTracker increments PLGained on every positive live-PL step without subtracting decreases. It is accumulated upward movement, not net or verified permanent gain; prior mixed windows already demonstrate overstatement (see earlier tracker caveat). This particular short trace has no sampled live-PL decreases, so its upward sum happens to equal its net +908.24. Use endpoint deltas and recorded state when comparing gains.

**Next focused check:** if testing continues, collect fresh paired profile/live readings around normal combat and recovery while recording form/output/fatigue/injury context where available. Separate measured combat gain from recovered or transformed displayed power. Preserve the existing recorder's bounded hook-safe observation pattern. The current trace omits several possible modifier fields, so another identical trace alone cannot identify the scaling cause. CEXP remains deferred; no repeat is needed merely to confirm that combat can increase PL.

## SP-084 — Focused PL combat/recovery recorder installed (2026-11-20)

**Implemented / untested live:** user authorized the focused PL capture. Standalone SparklingPowerTrace.lua v1 is installed in the executor workspace; lasting source is tools/diagnostics/SparklingPowerTrace.lua in the editable source tree. It uses the existing controller.UI Card/Button/Create helpers to add a POWER LEVEL TRACE card in Recorders. The main hub and generated builds were not changed, so run the standalone again after reloading the hub to mount the card. CEXP remains out of scope.

**User workflow:** load the Sparkling hub, execute SparklingPowerTrace.lua, open Recorders, and click Start baseline. Wait for RECORDING, rest about 10 seconds, mark combat before one ordinary fight, then mark recovery and rest until health/PL stabilize. For the first comparison keep form/output settings unchanged and avoid unrelated reward activities. Stop + Save afterward. These phase markers are user annotations, not verified combat/kill/recovery outcomes. Rerunning the script while active also stops and saves. The record can measure self PL during either NPC or player combat; it does not independently track opponent identity or kill credit.

**Data / provenance:** sample the local character every 0.5 seconds, retaining distinct direct values, character attributes and selected value attributes (Max/Min/Minimum/Maximum/MaxOutput). Allowlist covers PL, Output, MaxOutput, Form/Transformation, Race/RacePotential, Fatigue, six injuries, health/max health, Ki/Stamina and maxima, OverCharge, limits, combat timer, sleep/rest and knocked/dead context. Missing values remain absent, not zero. Incoming Requests.DataUpdated captures only selected scalar fields. Each self sample labels its latest profile PL, source and age; a ratio uses that most recent received value, not a synchronous server measurement. CEXP and unrelated profile/account fields are excluded. Form/RacePotential names remain observations, not proof of an effective/base or latent-power mapping.

**Read-only integration:** no outgoing hook ownership, input generation, state writes, reward calls or gameplay commands. At most three ordinary FetchData() reads per run: baseline, recovery marker and final stop. A valid numeric baseline is required. An eight-second timeout aborts startup or records failure; the underlying request cannot be cancelled, so no further reads are issued after timeout and late results are ignored. Repeated recovery clicks cannot issue more reads. If recovery read is still pending when stopped, final read is skipped explicitly. Existing auto-parry hook ownership is untouched; recording compatibility does not isolate its gameplay effects.

**Bounds / visibility:** saves every five seconds and at stop; history and latest manifests are sparkling_power_trace_<timestamp>_<nonce>.json and sparkling_power_trace_latest.json under AnomalyHub/universal, with history-specific _partN.jsonl files. Parts rotate at 800 events / 400 KB. Whole run stops at 10 minutes, 4 MB or 4,500 events; it does not silently continue with frozen detail. Card shows READY, STARTING, RECORDING with phase/count/readings/profile age, SAVING, SAVED, FULL or SAVE FAILED as appropriate. Death, character replacement, hub unload and recorder replacement stop collection and disconnect listeners. Raw captures remain local. A saved file is evidence of recording, not proof of server persistence or causality.

**Validation:** standalone Luau compilation passed. 116 mock assertions across 13 scenarios cover normal phase flow and separate profile/live endpoints; form/output/injury fields; missing-value honesty and profile allowlist; repeated recovery marker; rerun stop; death; character change; unload; event cap/chunk retention; ten-minute stop; initial and later write failures; failed/missing baseline; timeout/late response; pending recovery during stop; bounded reads; listener cleanup; and preservation of an existing outgoing observer. Local output, lasting source and installed standalone SHA-256 match. No live UI or reward behavior has been validated for this new recorder; compilation/mocks do not establish that the 80% ratio will recur.

**Next handoff:** inspect the newest power-trace manifest and every listed part after the user runs it. Verify terminal status, limits/errors, phase markers, read success and profile ages. Compare baseline/recovery/final profile deltas separately from live deltas and condition changes. Do not infer gain from summed positive ticks, call a phase marker a kill, or assume a stale profile/live ratio proves a formula. No need to disable auto-parry merely to free hooks; keep unrelated automatic farming out of a controlled comparison.

## SP-085 — Missing PL card corrected by integrating it into the hub (2026-11-20)

**Failure / cause:** the user could not find Power Level Trace in Recorders. SP-084 installed only a standalone script; inspecting the installed AnomalyPrivate.lua confirmed it did not contain that recorder. Reloading the main hub therefore could not mount it. No claim is made that the user ran the standalone or that its UI failed after execution. The previous separate-script workflow was insufficient for the expected built-in tab.

**Implemented correction:** src/SparklingPowerTrace.lua v1.1 is now included by tools/build_public.ps1 before the Sparkling Job Recorder in both build variants. A profile guard returns immediately for other games. The card mounts automatically for Sparkling, stays idle until Start baseline, and uses LayoutOrder=-20 to place it near the top of Recorders. Two-column button sizing now follows the available card width. Existing limits, measurements, three-read maximum, explicit terminal states, no-hook operation and phase workflow remain as documented in SP-084. The source module is canonical; the optional standalone and tools/diagnostics copy match it. Reload the hub to see the card; executing a separate script is no longer required.

**Concurrent edit preservation / deployment:** inserted only the new module at an exact existing module boundary in the current installed private hub and current public artifact. Removing that insertion reproduces each prior file byte-for-byte. The installed file was backed up and hash-checked before replacement and after installation. Lasting source and build assembly were updated; fresh public/private builds were generated in staging without syncing another checkout. No whole stale generated hub was copied over the current installed build.

**Validation:** the updated recorder passes the same 116 mock assertions across 13 scenarios. The actual patched public/private artifacts and freshly generated public/private artifacts all compile. Installed hash matches the tested patched private artifact. No live UI control or game execution was performed, so visible placement and capture still await the user's reload/test. This is a packaging/placement correction, not a verified new PL finding.

**Next handoff:** user reloads the hub, opens Recorders, finds POWER LEVEL TRACE near the top and starts the baseline. Follow the SP-084 combat/recovery workflow and inspect sparkling_power_trace_latest.json and its listed parts afterward. Keep this result and the earlier standalone failure in the same game note.

## SP-086 — PL recorder live success; combat gain retained through recovery, Output 80 matches observed ratio (2026-11-20)

**Verified live capture:** user reported completion. Latest power trace history 1795202375_000963C6, place 110405289082842, status Stopped by user, elapsed 112.69485 s. Its single part contains all 303 declared events / 209,317 bytes; counts and byte lengths verified. All three FetchData checkpoints succeeded; no failed/skipped reads, captureError or limited flag. Baseline, combat and recovery phases are present. This verifies the built-in recorder's basic collection/save workflow, not every mock-tested failure/lifecycle case or the card's exact visual placement. Report schema version remains 1; SP-085 integration is module v1.1.

| Checkpoint | Time (s) | Profile PL | Live Character.PowerLevel | Live/profile | Health |
| --- | --- | --- | --- | --- | --- |
| Baseline | 0.2324 | 15,880.386711984664 | 12,704.309369587732 | 0.8 | 96.9874 / 96.9874 |
| Recovery marker read | 77.8782 | 16,953.36102665897 | 13,562.688821327176 | 0.8 | 58.7369 / 97.4502 |
| Final | 112.6934 | 16,953.36102665897 | 13,562.688821327176 | 0.8 | 93.3954 / 97.4502 |

**Gain / timing:** profile net **+1,072.9743146743076**; live net **+858.3794517394435**. Combat marker t=5.2650, recovery marker t=77.6461. The user collected about five seconds of baseline rather than the suggested ten; it contains no observed PL change. Among 71 incoming profile events, 67 distinct PL increases and zero decreases occurred, all during the marked combat phase (first t=8.7307, last t=66.2942). The baseline and recovery windows contain zero profile increases. Both profile and live PL remain unchanged during the roughly 35-second recovery period. This is observed in-session retention through recovery, not a save/rejoin persistence test. Phases are user annotations; this self-only recorder does not verify target identity, kill credit, hits landed/taken or exact reward causation.

**Output hypothesis now supported by context:** every sampled Output was 80, its MaxOutput attribute was 80, and character Form was 0. Race was Alien in the checkpoints and RacePotential was 1; no claim is made that these caused output or that RacePotential is latent power. All three checkpoint pairs exactly match live PL = profile PL × Output / 100. Across 223 self-state observations, 203 ratios are within 0.000001 of 0.8; the total range is approximately 0.800000–0.801339. The remainder use asynchronously delivered profile/live values, so do not treat small transient mismatches as a different multiplier. Output never changed in this run; the data therefore supports, but does not isolate causally or prove universally, this formula. MaxOutput was also 80, so this is consistent with 80 being the observed output cap, not evidence that the player could simply raise it to 100.

**Recovery / condition limits:** health recovered from about 60.3% to 95.8% while PL stayed fixed, supporting that this observed PL increase was not merely health recovery. Final health was not fully restored; left/right arm injuries stayed 22/23 during recovery, so this was not a fully injury-healed comparison. Live Fatigue increased 0.953697 -> 0.977497 -> 0.989747, while all three profile Fatigue values remained 0.952997. Thus profile fields do not all represent current live state; use explicitly sourced live condition readings. The latest-profile age reaches 34.12 s during quiet recovery; the final explicit FetchData read independently confirms the unchanged profile PL.

**Decision / next handoff:** no repeat is needed to show that normal combat increases profile PL and that this gain remains during the observed recovery window. Keep profile and live PL distinct in reports/visuals, and do not add back the 20% as missing progression or rescale other players' values from this one character. Exact per-action gain, latent-power modifiers, alternate output/form behavior and persistence remain unknown. A future discriminating output test would use a normal in-game output change, if available, with no combat and paired reads; do not assume the current cap can be exceeded. No direct grant route is established. Raw captures stay local; this change updates only the continuous game note.

## SP-087 — PL v2 adds combat-event timeline instead of another totals-only test (2026-11-20)

**Problem / user request:** the prior recorder established actual profile growth and the live/output relationship, but omitted combat events needed for the broader question of what awards PL. User challenged the extra testing this omission caused and authorized adding those observations. The earlier SP-086 run remains valid for its limited measurements; it cannot retrospectively distinguish landed hits, damage taken or counters. Do not claim totals-only logging answers per-action reward attribution.

**Implemented / untested live:** src/SparklingPowerTrace.lua v2 is installed and built into both variants, visible as POWER LEVEL TRACE v2 near the top of Recorders. Existing Start baseline / Mark combat / Mark recovery / Stop + Save controls remain. Capture an ordinary fight; users need not label individual hits. Mount remains idle until Start. No input generation, remote replay, state modification, additional FetchData budget or outgoing-hook ownership was added. Auto-parry can coexist, but its inputs are not automatically distinguished from manual inputs.

**New timestamped evidence:** every event has a sequence ID. COMBAT_INPUT records Mouse1, Mouse2 and F down/up, processed status and unknown manual/synthetic origin; focused text entry and all other keys are excluded. These are input observations, not outgoing requests or known server acceptance. COMBAT_EFFECT retains bounded effect kind/message/payload for self and watched actors, with explicit hit_out / blunt / counter / block cue labels from existing Sparkling effect handling. CooldownNT names/durations, exact self/actor Humanoid health changes, Humanoid.Died and added Dead/Knocked markers are also recorded. These cues do not establish attacker identity, kill credit or automatic-parry success. LIVE_PL_CHANGE listens to the existing local PowerLevel value independently of profile changes.

**Nearby actor scope:** refresh the nearest eight humanoid models among up to 500 direct Workspace.Live children every second, within 35 studs. Assign stable session-local actor IDs (up to 64 identities), distinguish NPC/player, and record initial health/live PL and NPC labels without player names. Join/leave events disclose changes to the watched set; leaving is not a death. Unwatched effects are counted, scan/nearby/identity limits are explicit, and exited actors' listeners are disconnected. Missing Effects/CooldownNT coverage is reported rather than silently assumed present. No replay or recursive actor-source inspection occurs.

**PL correlation:** PL_CHANGE compares consecutive selected profile values, including decreases, and records source, previous-profile age, current live PL and references to all retained combat signals in the preceding 0.75 seconds (up to 128). Multiple competing cues are preserved. The event explicitly says temporal_only; later effects remain in the complete timeline so analysis can examine arrival order/latency instead of assigning the preceding cue as cause. Repeated identical profile values do not count as gains. The existing profile/live state sample every 0.5 seconds and three bounded baseline/recovery/final reads remain.

**Bounds / errors:** 10-minute run limit retained; total cap increased to 12,000 events / 8 MB for combat detail, with the same 800-event / 400-KB chunks and five-second autosave. At capacity collection stops with FULL status rather than silently freezing. Existing failure, timeout, late-response, unload/death/character-change cleanup and terminal-save reporting remain. Recent-context eviction inside the correlation window is separately flagged; the full event log remains the evidence source. Raw histories remain local under the existing sparkling_power_trace prefix; report schema version is now 2.

**References reused:** SparklingCexpTracker and SparklingAutoParry supply previously observed Effects signatures (EmitReferenceParticle / HitOut / Blunt and Weave counter messages), cooldown and health-event patterns. Beyond Nen's recorder provides the precedent of bounded actor observations and separating cues from attribution; no foreign IDs, delays or reward semantics were transferred. Those prior labels remain observational clues, not newly proven reward triggers.

**Validation / deployment:** 265 mocked assertions across 16 scenarios pass. Existing lifecycle, data-field, timeout, file-failure, cap/chunk and ownership checks remain; new cases cover input whitelist/textbox exclusion, self/nearby effects, far-effect omission counts, exact health events, defeat/cooldown markers, actor departure/disconnection, nearest-eight overflow, missing event remotes, event-driven live PL, deduplicated profile changes, valid event-reference IDs, competing cues and post-PL delayed effects. Patched public/private artifacts and freshly generated public/private variants compile. Exact module replacement preserves unrelated artifact bytes, installed private hash verified, optional standalone and source copies synchronized. No new live combat result yet.

**Handoff:** reload, confirm v2 in the card title, start baseline, mark combat and fight normally, mark recovery then Stop + Save. Analyze all parts by sequence/timestamp: compare profile changes with self HitOut, self Blunt/health loss, nearby health changes and counter cues, including delayed effects and ambiguous clusters. Keep exact per-action formulas unknown unless a discriminating pattern is supported; timestamps alone cannot guarantee causation. Avoid another broad recording if the captured evidence already answers the question. All findings continue in this same game note.

## SP-088 — V2 live timeline separates attack-associated and damage-associated PL components (2026-11-20)

**Capture validation:** user completed v2 history 1795203129_B1F1E9EE in place 110405289082842. Stopped by user after 142.6938 s; 1,500 events / 605,027 bytes in two parts (800 + 700). Part bytes/counts and every sequence ID validated. Three successful profile checkpoints, 75 PL_CHANGE events; no declared total cap, capture error or failed read. Coverage includes inputs, Effects, CooldownNT, live-PL change events and nearby actor health. 432 effects addressed outside the watched actor set were intentionally omitted; these are not all necessarily combat effects. Thug5 (actor1) repeatedly left/re-entered the 35-stud watched area; one player actor was briefly nearby. Do not treat opponent effect/health coverage as complete while out of scope. No outgoing requests were captured by this recorder.

**Totals:** profile PL 20,320.3544432136 -> 21,456.618280324932, net **+1,136.2638371113317**; live PL 16,256.28355457088 -> 17,165.294624259946, net **+909.011069689066**. The recovery checkpoint at t=78.2113 had profile PL 21,436.544530324933; another +20.07375 occurred afterward. Checkpoints retain Output=80 / MaxOutput=80 / Form=0 and live/profile=0.8. Phase markers were combat t=0.7151 and recovery t=78.0287; very short baseline, but three direct profile reads and event-level timeline provide the before/after evidence.

**Distinct increment classes:** 31 updates are exactly +6.69125 (within floating-point tolerance); 42 are larger increments decreasing from about +20.84248 to +20.77401; two updates around +27.49506/+27.49334 contain both components. These account for all 75 positive PL changes. There are no negative PL changes in this run.

**Association method / outcomes:** analyze cues both before and after profile delivery, not only the recorder's preceding-signal summaries. Match chronologically with distinct signal IDs, maximizing the number of pairs within the stated window and then minimizing absolute timing distance; do not reuse a signal for multiple changes. All 44 updates containing a larger component match distinct self HitOut cues within +/-0.6 s (43/44 have a nearest cue within +/-0.35 s). All 33 updates containing the smaller component match distinct negative self-health events within +/-0.35 s. The capture contains 52 self HitOut cues and 34 negative self-health events, so eight HitOut cues and one health drop remain unmatched under those rules. The unmatched health drop is t=11.4969, -0.42416 health. These are observed temporal associations, not a controlled causal proof or a rule that every attack/health loss grants PL.

**Representative evidence:** at t=4.3145 profile gain +20.84248 precedes self HitOut at 4.4636, actor1 Blunt at 4.4647 and actor1 health loss at 4.4653. At t=8.2974 the +6.69125 update follows incoming attack/self-health loss by about 0.234 s. Two mixed updates at 48.746 and 49.111 have both nearby self HitOut and self Blunt/health-loss signals. Some larger updates also occur near opponent counter cues (for example t=13.1636 before the opponent counter at 13.3138); therefore label these attack-associated, not guaranteed unblocked damage or confirmed kills.

**Empirical numerical model, not recovered server code:** let P be profile PL immediately before an individual component. The smaller component B=6.69125; the larger component A(P)=22.1125-P/16000. All current larger-only updates and two combined A(P)+B updates fit within 1.82e-12 absolute residual. Under that decomposition the attack-associated component totals **+915.4525871113361**, and 33 smaller components total **+220.81125**. These component totals explain the observed +1,136.26, but do not establish universal per-hit payouts or server implementation.

**Cross-check against existing captures:** SP-081 history 1795200321 has 74 updates fitting 30 B and 44 A components (maximum residual ~9.06e-13). SP-086 history 1795202375 has 67 updates fitting 25 B-only, 39 A-only and three combined updates. Those three require B+A(P+B), not A(P)+B, consistent with two changes being delivered together in the opposite order; residual <=1.67e-12 after considering both orders. Thus the numerical model fits all 216 observed updates across these three runs. This is corroboration on existing data, not new server-source proof. Do not extrapolate an implied level cap or assume constants transfer to other characters, opponents, forms, races, output settings or PL ranges. Event-level action associations exist only in the latest v2 run; the older recorders did not independently label those components.

**Parry evidence:** six self Counter Parry and two self Perfect Counter cues were captured. None has a PL_CHANGE within +/-0.25 s. Some wider windows contain ordinary attack/damage-associated changes, including a smaller gain about 0.299 s before one counter. No separate consistent parry award is established by these eight cues. Do not state that parrying can never award PL, and do not count every counter-adjacent gain as a counter reward.

**Recovery was not fully idle:** actor1 had Dead markers at 73.3610 and 83.3948, left at 84.3296, then reappeared at 133.6925 with full health. During the phase labeled recovery, self health loss/Blunt cues precede smaller gains at 134.2928, 141.9094 and 142.4746; together these are +20.07375. There were no PL updates between the final larger update at 73.3283 and the first later damage-associated update at 134.2928. Phase labels do not override actual combat evidence. This explains the post-marker increase without attributing it to resting or passive healing; no kill-credit claim is inferred from repeated Dead markers.

**Decision / handoff:** ordinary attacking and receiving damage now have distinct, repeatable PL associations; most gain in this run belongs to the larger attack-associated component. No additional broad repeat is needed for that finding. Exact server eligibility, why some HitOut/health-loss cues yield no matched update, rejoin persistence, and any universal scaling remain unknown. No direct self-grant route was discovered. Preserve model status as empirical, inspect ambiguities before tuning anything, and reuse this completed dataset instead of requesting another undifferentiated run. Documentation/analysis only; no gameplay or recorder code change in this review.

## SP-089 — Accurate session PL panel and retirement of positive-only gain counting (2026-11-20)

**User objective / implemented:** turn the established profile-versus-live distinction into useful hub tracking. New src/SparklingPowerTracker.lua, included in both variants by tools/build_public.ps1, adds YOUR POWER LEVEL near the top of Visuals. It always describes the local player, independently of the existing other-player selector. Rows are Profile PL, Live battle power, Net profile gain, and Profile PL / min. Existing Visuals Profile row Battle Power was relabeled Live Battle Power to identify its source honestly; other players' profile PL is not inferred or rescaled.

**Metric definition:** net = latest received profile PL - session baseline profile PL, retaining negative changes. Rate = net × 60 / full elapsed session seconds, including idle time; show Measuring until ten seconds have elapsed. Values use whole numbers with comma separators and signed net/rate. The session begins at the first successful profile baseline and restarts on a new character or successful Reset session. Live power is a separate direct Character.PowerLevel read and never contributes to net/rate, so suppression/recovery or form changes in that live field cannot be accumulated as new profile progression. A profile decrease and equal recovery return net to zero rather than adding a positive gain. This metric is in-session observed profile change, not a rejoin persistence guarantee or inferred attack reward.

**Data freshness / requests:** one bounded FetchData() read seeds the session, then ordinary incoming DataUpdated events update profile PL. Refresh profile requests another read without resetting the baseline; Reset session reads first and resets only on success. No repeated automatic polling, fees, outgoing hooks, combat input or grant calls. Reads are debounced five seconds with at most one outstanding request and an eight-second display timeout. An underlying timed-out InvokeServer cannot be cancelled; its late response is ignored and another read waits until it finishes. Profile events arriving during a refresh take precedence over the potentially older response. Character epochs invalidate old pending results and request a new baseline once any old request clears. The panel displays elapsed time, profile age, and read failure/timeout status; unavailable profile data is not replaced with live PL or zero. Failures preserve an existing baseline; missing initial profile data leaves net/rate unavailable. controller.SparklingPowerStats.Get() exposes a copied snapshot of these measurements for future UI consumers.

**Legacy tracker correction:** SparklingCexpTracker v2 removes PLGained from its activity counter keys and stops adding every upward live-PL step. Existing saved PLGained is retained only as LegacyPLUpwardSum in schema PLTrackingVersion=2; it is neither incremented nor presented as a valid net gain. Existing legacy values survive subsequent reloads. Unrelated activity counts and CEXP check behavior are preserved. New CEXP history lines explicitly identify PLSource=live and PLGainMetric=retired_positive_sum instead of writing the misleading PLGained total. Old history files are not rewritten; retain their earlier caveats. The new PL panel's session metric is independent of the potentially multi-session CEXP counters, so do not equate their time windows.

**Validation / deployment:** 106 mocked assertions across 14 scenarios cover live suppression/recovery, signed profile loss/recovery/net gain, idle-inclusive rate, under-ten-second display, invalid numbers, successful/failed resets, repeated-click read limits, event-during-read ordering, new characters including a pending old read, timeouts and late response disposal, absent/failed/nonnumeric profile reads, unload, comma/sign formatting, legacy schema migration and preservation of unrelated counters. Patched public/private artifacts and freshly generated public/private variants compile. Source/build assembly updated; exact targeted artifact edits preserve unrelated bytes and native line endings. Installed private build backed up and hash verified. No live UI execution was performed in this change; visibility and ongoing readings remain awaiting user use.

**Next handoff:** reload the hub, open Visuals → YOUR POWER LEVEL. Use Reset session before a comparison and Refresh profile for a fresh endpoint if the age is old. No recording is required for ordinary net/rate display; the separate v2 Power Level Trace remains available when event-level evidence is needed. Do not treat old LegacyPLUpwardSum or live-power recovery as progression, and do not replace signed net with an upward-only sum again. CEXP investigation remains deferred.

## SP-090 — Restore the PL self-grant objective; move measurements to Dashboard Live Session (2026-11-20)

**User correction / purpose:** user clarified that the objective remains finding whether PL can be granted to oneself, and that session measurements belong on Dashboard like Beyond Nen and Gohan. The preceding tracking-panel work drifted from that objective. Its legitimate purpose is distinguishing measured profile changes from live/display changes during an investigation; it is not a PL award mechanism or evidence of a vulnerability. Future chats must keep that distinction and the active objective explicit instead of treating additional statistics or broad combat repeats as completion.

**Implemented placement/layout:** SparklingPowerTracker v1.1 now mounts one LIVE SESSION card on ui.Pages.Home (Dashboard), LayoutOrder=-1, with a two-column grid of six tiles: Profile PL, Live Battle Power, Net Profile Gain, Profile PL/min, Session Time and Profile Updated. Refresh profile / Reset session and status remain beneath the grid. There is no duplicate tracking card in Visuals. Existing Visuals player-detail rows remain separate. Measurement/read/reset semantics from SP-089 are unchanged; the card accurately states that this session restarts on character change/reset rather than claiming cross-hop persistence.

**References inspected:** src/BeyondNenSession.lua uses Dashboard SESSION STATS with a two-column grid and reset control; src/GohanHub.lua also places SESSION STATS and a two-column session grid on its Home page. Reused layout, spacing, tiles and shared colors/type, not their game-specific stat sources or session restore rules. Beyond Nen's note was consulted for cross-game precedent; its partial historical status is unchanged.

**Grant investigation status / scoped recheck:** revisited the previously captured 25 progression and seven reward client scripts. Input forwards action names/arguments through Requests.Action; observed daily reward requests are State and Claim, and the PowerLevel reward table is a UI label. No confirmed client PL-grant command or selected award amount was found in those observed paths. The available source still omits the server Action/PL award and validation handler. The SP-088 empirical gain model does not expose that missing handler or make its fields client-writable. This is a source/evidence limitation, not proof every route is secure. No new remote mutation, grant attempt, gameplay input, or live experiment was performed here. The exact source/validation of a suspected award path would be needed before designing a meaningful further test; do not invent command names or request another normal fight merely to restate normal progression.

**Validation / deployment:** 120 mock assertions across 14 meter/migration scenarios pass, including mounting exactly one card in Home with no Visuals duplicate and retaining numeric formatting/net/rate behavior. Updated module, actual patched public/private artifacts and freshly generated public/private artifacts compile. Installed private build backed up and hash verified; exact module replacement preserves unrelated bytes. No live visual inspection was performed. Reload and open Dashboard → LIVE SESSION; no additional combat recording is required for this placement change.

## SP-091 — Auto Shadow Farm v2: rests in a bed at 90% injury (2026-11-20, Claude)

**User:** "if injuries go over 90% it should find a nearby bed, then wait till it reaches 1% and start again".

**Evidence used (Verified from captures / source):**
- **Injury values:** the six `HeadInj` / `TorsoInj` / `LeftArmInj` / `RightArmInj` / `LeftLegInj` / `RightLegInj` NumberValues on your character run 0–100 (SP-033). Auto Shadow v1 stopped when an arm reached 90 (SP-043).
- **Beds:** the job recorder (place 135537125271116) saw nine `Workspace.Map["Healing Pod"].Bed.ClickDetector` instances, including a nested `ClickDetector.ClickDetector` and one duplicate position.
- **Healing:** Trello says beds and N (Sleep) clear injuries.
- **Not yet recorded:** what clicking a bed does (sit, anchor, teleport, heal rate). Beds in other places, including the 120023577538065 Shadow runs, haven't been checked.

**Implemented / untested live — v2:**

*Trigger*
- When any of the six injuries reaches 90 (was: either arm, stop), the farm enters a rest phase instead of stopping.

*Leaving the Inner World*
- It taps J and waits up to 5 s for the Shadow to vanish (two tries). Otherwise it stops.

*Finding a bed*
- One scan of `Workspace.Map` looks for a ClickDetector or ProximityPrompt under a part or model named `Bed`, or under a `Healing Pod`.
- Nested detectors and duplicate positions are skipped.
- It takes the nearest within 1,000 studs. If none is found, it stops with "no bed within 1000 studs".

*Walking there*
- It follows PathfindingService waypoints; if no path is found, it walks in a straight line.
- Stuck means under 1 stud moved in 3 s. It allows up to 3 path attempts and an 8 s timeout per waypoint.
- It pauses while chat or the menu is open, and stops within `MaxActivationDistance − 3` (4–8 studs) of the bed.

*Using the bed*
- Clicks 1–2 use the mouse at the bed's screen point. The second click is shifted by the top-bar inset, which Auto Job's buttons needed. The bed is skipped if the hub window covers that point.
- Click 3 uses the executor's `fireclickdetector`, which fires the bed's own click event.
- Prompts use `InputHoldBegin` / `InputHoldEnd`, as in Auto Job.

*Waiting to heal*
- Injuries must start falling within 15 s, or the bed is clicked again: 3 uses, then the next-nearest bed, 2 beds in total.
- Once they're falling, it waits until every injury is ≤ 1, with a 10-minute cap on the whole rest.

*Resuming*
- If the character is sitting, anchored or PlatformStand, it taps Space once.
- The normal J branch then re-enters the Inner World.

*Log lines (`sparkling_auto_shadow_latest.txt`)*
- REST start and done, BED candidates / path / use, BEDSTATE (sit, seat, anchored, state, position) and HEAL every 5 s.
- POWER lines add the worst injury.
- Stopping the farm cancels the walk.

**Checks:** source and both variants compile. No live or mock run.

**Next step:** run Auto Shadow until an injury hits 90%, near a hospital or healing pod. Then read the log for which bed it chose, whether the path or the click worked, the bed state, and how fast injuries fall. If beds don't work, N (Sleep) is the documented alternative.

## SP-092 — PL grant candidate audit and recorder coverage correction (2026-11-20)

**Scope / outcome:** reviewed the existing seven reward and 25 progression source captures, their script inventory, the remote fingerprint, four historical manual combat logs, the valid SP-081 NPC trace, and the v2 PL trace's documented coverage. This is offline source/log analysis, not a new grant attempt or live validation. No captured call demonstrably accepts an arbitrary PL award. Missing server scripts do not make black-box proof impossible: a controlled unexpected profile gain, independently read back and checked after rejoining, could establish a flaw without source. Conversely, an unobserved handler cannot be assumed secure.

**Request inventory:** the four manual combat histories contain 46, 23, 323 and 548 parsed outgoing observations respectively, through the namecall path, involving Action, Dashed and WallSlam. Their differing duration, caps and watched sets prevent whole-game absence claims. The valid NPC trace retains 262 OUT / 262 RETURN observations: 250 Action, three Dashed, seven WallSlam and two recorder FetchData reads. FireServer return only confirms local forwarding returned, not server acceptance. Earlier broken NPC traces remain excluded. V2 PL trace has no outgoing request capture.

**Critical coverage correction:** SparklingCombatRecorder.syncRemotes explicitly targets eleven combat paths (Action, AIWallSlam, WallSlam, Dashed, StopDash, Cooldown, CooldownNT, Effects, Flight, Beserk, Oozaru). It does not watch AddStat, PlayerChoices, DailyReward or Dialogue. Their absence from those combat logs cannot reject those routes. A future route-specific recording must explicitly include its actual request path, incoming response/state and profile checkpoints; current PL trace alone is insufficient for request attribution. Reused Beyond Nen's distinction between input/request/result as an evidence convention; no foreign remote semantics transferred.

| Inspected route | Observed arguments / meaning | PL relevance and remaining uncertainty |
| --- | --- | --- |
| AddStat (reward source 2, lines 275 and 679–700) | Stat key from Agility, Strength, CoreStr, Concentration or Reactions; spend amount capped by displayed StatPoints after a KiAwakened check. | A field-and-amount request is an input-validation review lead, but PowerLevel is absent from the inspected stat list. Client checks do not prove server checks, nor establish that arbitrary keys work. No observed PL assignment here. |
| PlayerChoices (source 2, lines 2109–2138 and 2341) | Settings Data attribute plus 0/1; separate MenuSettings payload. | Setting selection is observed. Generic profile mutation or acceptance of a PL key is unproven. No invented PL command should be inferred from its name. |
| DailyReward (source 3, lines 21 and 1015–1044) | UI lists PowerLevel as a 5,000 reward; outgoing Claim has no reward key, amount or selected day. | Explicit PL reward label makes this a known award-related path, but actual PL-day payout is unmeasured. Prior negative early/duplicate capsule tests apply only to those tested conditions. Another capsule repeat is not useful. |
| Dialogue (source 4, lines 96–106) | Choice text selected from incoming options plus exit flag. | No PL-specific offered choice was captured. A dialogue path alone is not a grant command. |
| Action (progression source 1) | Normal named combat/training actions, booleans and direction/look data; no requested PL increment at inspected calls. | Normal combat awards are established by SP-088. Reward eligibility and server validation remain unknown. |
| MissionQuestModule / SkillGivingModule | Server-oriented reward code and InvokeClient skill selection. | Not client-callable PL award functions; retain SP-077 decompiler and direction caveats. |

**Known source gaps:** the saved inventory names Primary.CommandMenu, NEWUI.KiMenu.Code.LimitBar and HudLimit, but their bodies are not among the 32 decompiled sources reviewed. Fingerprint also contains Admin/CommandMenu remotes; their names do not establish exposed commands, permission bypass or PL support. Wish-related nested persona scripts are listed, but no PL wish callsite was recovered. Inventory presence is not source coverage. The generic grant-inspection GC dump also includes the scanner's own constants and reports a script-list cap; string hits are not independent evidence of a game grant function.

**Decision / next step:** no further broad fight recording is warranted. Before choosing a mutation test, fill the specific client-source gaps above and inspect the normal callsites/argument construction. If a genuine award-related action is identified, record baseline, one isolated action with its request, incoming profile update, fresh endpoint and post-rejoin read; retain an idle comparison and stop combat/training automation to avoid confounding. Server source is helpful but not a prerequisite for that evidence. Until then the honest verdict remains ordinary gains confirmed, arbitrary self-grant unproven. No hub/runtime changes or new live requests were made in this audit; no Luau build test is applicable. Preserve concurrent SP-091 Auto Shadow work.

## SP-093 — Targeted PL source capture prepared for the remaining client-code gaps (2026-11-20)

**Implemented / not run live:** standalone SparklingPLSourceCapture.lua v1 captures exactly three previously inventoried source containers: LocalPlayer.PlayerScripts.Primary.CommandMenu, ReplicatedStorage.NEWUI.KiMenu.Code.LimitBar, and ReplicatedStorage.NEWUI.HudLimit. Reuses the SP-076 progression-capture lifecycle. No broad discovery, remote calls, gameplay input, hooks or module execution; reads accessible Source or uses the executor decompiler. Missing/unloaded objects and per-script failures are explicit, not evidence of absent functionality.

**Bounds / output:** at most three workers and three scripts, 1.5 MB per source, 45-second deadline, late results ignored, periodic manifest saves. Local output prefix AnomalyHub/universal/sparkling_pl_sources with timestamp/nonce history, numbered source files and _latest.json manifest. Saved sources remain local. The existing hub and other diagnostics are unchanged. Canonical helper is tools/diagnostics/SparklingPLSourceCapture.lua; optional standalone installed as SparklingPLSourceCapture.lua in the executor workspace. Run once using loadstring(readfile("SparklingPLSourceCapture.lua"))(); no combat needed.

**Validation:** Luau compilation passed; checked that discovery has only the three explicit targets and no recursive scan, require, FireServer or InvokeServer. Installed file hash matches the prepared helper. No live execution, successful source extraction or grant behavior is claimed. Next: inspect the resulting manifest and available source callsites; handle unavailable targets honestly, and select a specific recording only if the source supplies a concrete relevant action. No new grant candidate is established merely by preparing this capture.

## SP-094 — Dashboard SESSION STATS; Visuals CEXP saved per account (2026-11-20, Claude)

**User:** "make a dashboard live tracker just like gohan and beyond nen", and "combat exp in visuals persist on different accounts".

**Bug (source inspection):**
- Visuals v5.2 saved the last Varrik CEXP reading to one shared file, `sparkling_cexp_value.txt`.
- Any account or character slot on the same PC therefore showed the last reading heard by any account.

**Fixed — Visuals v5.3:**
- Readings are saved in `sparkling_cexp_<UserId>.json`, keyed by the profile `CharacterName`.
- A reading heard before the profile scan names the character is held under "?" and moves to the character once it's known.
- The old shared file isn't read any more, because it can't say whose reading it was. Each character needs one new Varrik check.
- The entry is exposed as `controller.SparklingCexp`.

**New — `src/SparklingSession.lua` (SESSION STATS card, top of Dashboard):**
- **Layout:** the same as Beyond Nen's SESSION STATS (2-column tiles, Reset Session, status line), with Gohan's tile set adapted to Sparkling.
- **Tiles:**
  - session time;
  - power level and gain: profile PL from the Power Tracker meter, so forms don't inflate it;
  - PL / hour;
  - combat exp: last Varrik reading plus the gain since the session's first reading;
  - Zeni earned, from the inventory "Zeni:" label (rises only), with a per-hour rate;
  - event wins gained: `Character@EventWins`, rises only;
  - server hops;
  - deaths.
- **Counting rule:** rises are counted only after a source has been stable for 3 s and only within the same character. Respawns and hops never count as gains.
- **Persistence:** saved per account in `sparkling_session_<UserId>.json` and restored after a hop (JobId change = +1 hop).
- **New session:** after 15 min away, on a different CharacterName, or with Reset Session.
- **Sends no game requests.** The existing LIVE SESSION card (Power Tracker, SP-090) is unchanged and sits below it.
- Built into both variants after the CEXP tracker.

**Checks:** source, Visuals and both variants compile. No live run.

**Next step:** load the hub and check the tiles fill in:
- PL after the first profile read;
- Zeni after a job pays;
- that a hop keeps the session and adds 1 hop;
- on a second account, that Visuals CEXP shows "Ask Varrik" until that account is checked.

## SP-095 — Targeted source capture succeeds; command catalog is supplied by server (2026-11-20)

**Verified capture / source inspection:** history 1795204818_1ABE94F4, place 94737939156598, completed in 0.117 s. All three targets found and saved (24,187 / 2,916 / 2,916 bytes), with manifest byte counts checked and no declared limits. This validates the SP-093 capture, not a PL grant.

**Limit-bar result:** LimitBar and HudLimit are byte-identical rendering modules. Their set method clamps a local display fraction to 0–1; isFull checks rendered rows, and step updates pixel buffers. No remote request or PL award appears in either body. Setting this display full is not an observed server limit break. This closes these particular display-module leads, not every limit-break mechanism.

**Command-menu result:** source lines 277–303 implement normal execution using server-supplied metadata. Non-Interact entries use CommandMenu InvokeServer with Run, entry.id, and a table containing target/value; Interact entries use Request with an ID and selected player. Lines 306–344 first fetch CommandMenu List and build entries from that response. Metadata consumed includes id, category, label, hint, args and value; amount/text args produce an input box. The client does not contain the complete command catalog. A special Give branch defaults text to the equipped Tool name, so the word Give is not evidence of arbitrary item creation or PL awards. InteractRequest replies accept/decline an incoming request, not grant an arbitrary reward. RightBracket opens this normal Chat Prompts menu. No PL command or server authorization rule was recovered. Decompiled duplicated UI branches remain unreliable for exact control-flow reconstruction.

**Concrete next read prepared:** tools/diagnostics/SparklingCommandCatalog.lua (also installed as a standalone executor-workspace file) makes exactly one normal CommandMenu List request and saves the bounded response locally under AnomalyHub/universal/sparkling_command_catalog, with timestamp/nonce history and _latest.json. It never invokes Run or Request, selects a player, executes a command or changes profile values. Eight-second timeout, no retry, per-environment pending-read lock retained until an underlying timed-out call returns, late result ignored; response copy bounded by 2,000 nodes, 200 entries per table, depth five and 2,048 characters per string. Initial save failure prevents the request. This read is justified by the observed menu callsite rather than guessed arguments.

**Validation / handoff:** catalog helper compiles and installed hash matches; it has not yet run live. Run loadstring(readfile("SparklingCommandCatalog.lua"))() once, then inspect the returned IDs/labels/argument metadata before selecting any action. A returned command is not proof of a permission bypass or a successful grant. Keep raw catalog/source local. Existing hub artifacts and concurrent SP-094 Dashboard/CEXP changes preserved. No further combat recording is needed to obtain the catalog.

## SP-096 — SESSION STATS: spars, clearer PL / hour; LIVE SESSION card removed (2026-11-20, Claude)

**User:** "remove live session, also pl/hour seems to be bugged it says +10,990 … add spars, spars should be based off fights with players and if you knocked them".

**PL / hour (Verified from the saved session file):**
- The value was arithmetically right: `sparkling_session_<UserId>.json` held StartPL 157,254.7 → CurrentPL 157,855.3 over 195 s, i.e. +600 PL, which is ≈ +11,077 / h.
- The tile showed the bare signed number, which reads like an amount gained.
- It now shows `+11,077 / h (+185 / min)`.

**LIVE SESSION removed:**
- Power Tracker v1.2 no longer builds its Dashboard card.
- The meter (`controller.SparklingPowerStats`: FetchData seed + DataUpdated) is unchanged; SESSION STATS reads profile PL from it.

**Spars (Implemented / untested live):**

*Hit pairing*
- `Requests.Effects ("EmitReferenceParticle", character, "HitOut" | "Blunt")`: HitOut = that character landed a hit, Blunt = it was hit.
- Your HitOut paired with a player's Blunt within 30 ms, within 20 studs, is your hit on them. Your Blunt paired with their HitOut is their hit on you.
- Checked offline on PvP history 1795169031 (Verified from capture): 139 of the 170 pairs within 150 ms arrived within 5 ms. The rest, 30–150 ms apart, look coincidental, so the window is 30 ms.

*When a fight counts*
- A spar is counted at 3 traded hits with at least one of them yours.
- A fight ends after 30 s without hits.

*Result*
- **Won:** a `Knocked` child is added to their character (the game's client scripts check `FindFirstChild("Knocked")`), or they die, within 10 s of your last hit.
- **Lost:** the same happens to you within 10 s of their last hit.
- That `Knocked` replicates for other players is unverified; a `Dead` marker on another actor was seen in a Power Trace.

*Display and log*
- Tile: `SPARS (WON / LOST)`. The counters persist with the session.
- Local log `sparkling_spars_latest.txt` (not committed) records each spar start, win, loss and end.

**Checks:** both variants compile. No live run.

**Next step:**
- Spar a player and knock them, then check the tile and the spar log (start, WON).
- If no WON appears after a knock, `Knocked` doesn't replicate and the result needs another signal.

## SP-097 — Floating damage numbers (2026-11-20, Claude)

**User:** "add damage numbers", then chose floating numbers on hits.

**Evidence (Verified from captures):**
- **Same packet:** in Power Trace 1795203129 part 2, your `HitOut`, the target's `Blunt` and its replicated Humanoid health drop arrive within ~2 ms of each other. For example: HitOut 53.32866, Blunt 53.32974, health −1.21 at 53.33015.
- **Offline check:** across the saved Power Trace JSONL files, crediting a drop only when your HitOut and the target's Blunt are both within 60 ms of it matched 36 of 39 actor health drops. The other 3 had no HitOut of yours nearby.
- **Scale:** max health there was ~53, so hits are a few HP, and numbers under 10 show one decimal.

**Implemented / untested live — `src/SparklingDamageNumbers.lua` (ESP page, "Damage Numbers" section):**
- **Toggles:** "Damage Numbers (your hits)" (default on) and "Damage You Take" (default off, shown in red).
  - Saved in `sparkling_damage_numbers.json`.
- **What's watched:** every Humanoid under `Workspace.Live`, players and NPCs.
- **Attribution:** health drops wait 60 ms, then count only when they match your HitOut and the target's Blunt within that window. Skill hits without a HitOut aren't shown.
- **Display:**
  - A BillboardGui over the head rises 2.2 studs and fades over ~0.9 s.
  - Hits of 10%+ of max health are larger and gold.
  - At most 40 are shown at once.
  - The numbers are parented beside the hub window, so hiding the hub keeps them.
- **Visibility:** only you see them. No game requests.

**Checks:** source and both variants compile. No live run.

**Next step:** hit an NPC and a player and check the numbers match their health bar. Then report whether skill hits should also show.

## SP-098 — Damage numbers: none on the Inner World Shadow; ScreenGui container (2026-11-20, Claude)

**User:** "damage numbers dont pop up".

**Evidence (Verified from captures):**
- `sparkling_damage_numbers.json` was written at 13:18:24 (Enabled=true), so the module loaded and its toggle worked.
- Auto Parry history 1795205896 (13:18–13:19) had only two actors: you and your Inner World Shadow.
  - 53 self HitOut, 48 Shadow Blunt, each pair ~1 ms apart. So the hit pairing matched.
- The Auto Shadow log reads the Shadow's `health=1000000000/1000000000`.
  - Numbers come from the target's health dropping, and the Shadow's never drops.
  - So no number can appear on it. This is expected, not attribution failing.
- Real NPC/player health drops were matched offline in SP-097 (36 of 39).

**Changed / untested live:**
- The numbers are now under their own ScreenGui (`AnomalyDamageNumbers`, beside the hub window), the same structure as the ESP BillboardGuis. Before, they sat under a bare Folder.
- Showing on non-Shadow targets is still unconfirmed live, so the rendering path is the remaining unknown.
- Both variants compile.

**Next step:** hit an NPC (e.g. a Warrior Clan Rogue) or a player with Damage Numbers on. If nothing pops there, the rendering or the health replication needs a capture.

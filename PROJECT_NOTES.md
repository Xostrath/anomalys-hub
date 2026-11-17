# Anomaly's Hub project notes

## Interface conventions

All adapters use the shared UniversalHub controller and UI helpers. Hunter X Enter is the visual reference: dark panels, purple accents, compact sidebar, consistent type and spacing, and responsive two-column panels on Auto, Settings, and Recorders.

| Section | Purpose |
| --- | --- |
| Dashboard | Profile identity and session summary |
| General | Shared utilities such as spectate and noclip |
| Auto | Implemented game-specific automation |
| Player & NPC ESP | Available overlay controls |
| Servers | Server navigation |
| Recorders | Activity recordings and their Start / Stop + Save / Copy controls |
| Settings | Fingerprint discovery, preferences, reload and unload |

Do not add empty feature tabs or imply that one game's features exist in another. Sparkling's combat recorder belongs in Recorders, even though its initial version was placed in General.

### UI mounting

Create the hub in the executor's isolated `gethui()` root when available, with CoreGui as the next option. PlayerGui is a warned fallback. Sparkling displayed pixel fonts and decorative borders when the hub was in PlayerGui; the user confirmed that isolating it restored the intended appearance. Retain this fix in future builds.

## Existing discovery workflow

Use **Settings → Game Adapter Discovery → Capture Fingerprint** first. The controller exposes `CaptureRemoteFingerprint()` for the same operation. It records place ID, universe ID, profile, and client-visible RemoteEvent, RemoteFunction, and UnreliableRemoteEvent paths/classes. It does not invoke remotes or extract module source.

The output is `AnomalyHub/universal/remote_fingerprint_<universeId>.txt` inside the executor workspace. It is also copied to the clipboard when available. Inspect existing captures before asking for another or creating a standalone scanner. These captures are local evidence and must not be committed here.

## Game profiles

| Profile | Universe | Observed places | Status |
| --- | --- | --- | --- |
| HunterXEnter | `9857379797` | `76571518635501` | Existing game adapter; identity also checks known remote paths |
| BeyondNen | `2174306620` | `78073853031462` | Existing adapter using its remote fingerprint |
| Sparkling | `10771972206` | Tutorial: `101713836627444`; user-supplied entry link: `120525873606470` | Explicit universe-based identity; passive combat recorder |
| Universal | Unrecognized games | Any | Shared utilities and discovery fallback |

The build also includes the legacy Gohan profile. Inspect its existing routing before changing detection precedence.

### Sparkling evidence and scope

Tutorial capture: 95 entries in place `101713836627444`. Main-world capture: 126 entries in place `110405289082842`, with profile Sparkling. Both contain Roblox infrastructure and duplicated client objects; counts are not unique mechanics. The main-world capture retains the watched combat paths and adds training/dance-related paths. Universe identity covers places within the same experience; it does not imply matching remote coverage in each place.

Observed combat-related paths under `ReplicatedStorage.Requests` include `Action`, `AIWallSlam`, `WallSlam`, `Dashed`, `StopDash`, `Cooldown`, `CooldownNT`, `Effects`, `Flight`, `Beserk`, and `Oozaru`. These are the combat recorder's watched paths. Names alone do not establish arguments, behavior, permissions, or vulnerabilities.

**Recorders → Sparkling Combat Recorder** captures normal inputs (excluding text entry), outgoing calls/returns where executor hooks work, incoming events, relevant attributes and values, motion, health, animations, and up to six nearby actors within 100 studs. It sends no gameplay inputs or remote requests itself.

- Latest: `AnomalyHub/universal/sparkling_combat_capture.txt`
- History: `AnomalyHub/universal/sparkling_combat_<timestamp>_<nonce>.txt`
- Limits: 120 seconds, 16,000 lines / 4 MB; saves every five seconds.
- Stops on death, character changes, teleport, unload, or manual Stop + Save.
- Reports outgoing hook coverage. Direct/namecall paths may duplicate an observation; do not sum them as distinct requests without examining their timing and nesting.
- Compiled successfully; two live tutorial captures have been reviewed (see findings below). Further combat semantics remain unconfirmed.

## Source and publishing

This GitHub repository is the **public artifact repository**, not the complete editable source tree. `AnomalysHub.lua` is generated; `loader.lua` loads the public build.

The established local source tree is the `gohanhub-current` directory in the Codex workspace. The older `D:\gohanhub-rewrite` referenced by its legacy README predates the Hunter X Enter modules; do not assume it is current. Compare source, installed private build, publishing checkout, and latest remote before editing.

Relevant source files:

- `src/UniversalHub.lua`: shared UI, settings, capture fingerprint, controller, cleanup.
- `src/HunterXEnterFingerprint.lua`: Hunter identity checks.
- `src/SparklingCombatRecorder.lua`: Sparkling's recorder.
- `tools/build_public.ps1`: profile routing, module assembly, public/private separation.

Build with `tools/build_public.ps1 -Url https://raw.githubusercontent.com/Xostrath/anomalys-hub/main/AnomalysHub.lua -SkipPublishCopy`. Inspect the script before running: omitting `-SkipPublishCopy` can update the publishing checkout; `-SyncPrivate` installs the private output into Volt.

Public output: `dist/AnomalysHub.lua`. Private output: `dist/private/AnomalyPrivate.lua`. **Never upload the private output**, `secrets.local.lua`, raw diagnostic evidence, or the entire working directory.

Use an isolated publishing checkout when another checkout contains unrelated edits. Transplant only the intended change onto current `main`; never replace it with a stale full build or force-push. Update source/build integration so future rebuilds retain the change. Verify the pushed commit and report compilation separately from live testing.

## Sparkling tutorial recordings reviewed (2026-11-17)

Two local captures were reviewed: a 120-second session ending at the time limit, and a 78.55-second session stopped manually. They contain 46 and 23 outgoing Action observations respectively, all through the namecall path, each with its matching return. No direct-hook duplicate OUT observations appeared in these two captures. FireServer returns are not server acknowledgments.

Observed input-to-Action mappings:

| Input/context | Observed Action arguments |
| --- | --- |
| Left click | MouseButton1; some calls also contain a boolean and vector-like third argument |
| Right click | MouseButton2; observed variants include a missing second argument or Up, plus a vector-like third argument |
| G | Grab |
| A during grab sequence | Grab, Throw |
| D during grab sequence | Grab, Slam |
| F down / release | Defend, true / false |
| M | Meditate |
| Left Shift | Descend |

Do not assign an exact meaning to the attack boolean or vector from serialization alone. In the second capture, MouseButton1 calls changed shape after the One-key input; that is correlation, not proof of the cause.

Incoming CooldownNT payloads advertised Heavy=2 seconds and Grab=5 seconds. Defense-related payloads included Counter durations of 5, 4, and 0.5 seconds, Block=5, and ZVanish=3. These are observed client notifications, not independently verified server enforcement timings.

The first capture included incoming Effects messages labelled Perfect Counter! (about 50.714 seconds) and Counter Parry! (about 65.849 seconds). PBlock, KiSlap, and CounterCooldown changes provide additional defense signals. These observations do not yet establish a reliable timing window for automatic defense.

The recorder also observed health changes, animation starts/stops, Ki, Fatigue, PowerLevel, CombatTimer, and grab-related state. GUI discovery reached its 600-object bound in both sessions; actor discovery reached 1,000 objects, and one session reached the character-stat scan bound. No callback/capture errors or report-size limit were reported. Prioritize Workspace.Live and relevant HUD branches before treating missing actor or UI observations as absence. Raw captures and player identifiers remain local.

## Sparkling player ESP sources

`src/SparklingESP.lua` resolves each player independently from their current Character. The Power Level label reads only a finite, nonnegative direct Character.PowerLevel NumberValue/IntValue, observed in tutorial captures. It is not yet verified as base, effective, or suppressed battle power. Race accepts only explicit Race string attributes or direct Race StringValues on the Player/current Character; missing, numeric, or conflicting values show Unavailable. It does not infer race from appearance, search preview rigs/save slots, or substitute the local player's data for others.

Race and Power Level have separate persisted ESP toggles. Recorders → Sparkling ESP Data → Capture ESP Data saves bounded field/source observations for current players to `AnomalyHub/universal/sparkling_esp_data_latest.txt`. No requests are sent. Source/build variants compile; live race availability and power semantics still require this capture.

## Sparkling Ki, outline, and experimental defense

Player ESP now has a Ki level toggle reading only direct Character.Ki numeric values (observed in both tutorial and main-world captures). It shows the raw replicated value, not an invented percentage or maximum. A separately persisted Self outline toggle uses a local Highlight on the current living character, independent of the player-ESP master toggle, rebinds after respawn, and is removed on unload. PowerLevel remains a replicated value whose base/effective/suppression meaning is unverified; the dedicated ESP data capture is still needed.

A third combat capture from main-world place 110405289082842 reached the 16,000-line limit at about 118.45 seconds. Self-targeted Counter Parry messages appeared near 28.747, 31.680, and 72.046 seconds. The first and third followed opponent animation 84394230178792 (combo 2) by about .20/.23 seconds; the second followed 95937874840716 (combo 3) by about .10 seconds. Other counter messages in this crowded capture refer to other actors and must not be counted as the local player's results. These are client timing correlations, not measured server parry windows.

Auto → Auto Parry is an off-by-default experiment scoped to these two animation IDs. It sends a brief F input, never remote calls, only when an attacker is within 8 studs, both characters face the encounter, and line of sight is clear. It honors observed incoming Counter cooldown durations, avoids overlapping manual F input, releases its held key, and stops on character loss/death, teleport, loss of window focus, unload, or starting a manual combat recording. It does not cover all fighting styles, heavy attacks, or projectiles. Its log is `AnomalyHub/universal/sparkling_auto_parry_latest.txt`; post-attempt self counter messages are counted as correlations, not guaranteed causation. Source and public/private builds compile; no live automatic-defense test has been performed yet.

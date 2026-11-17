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

The current capture is from the **tutorial world**, not the main world. It contains 95 remote entries, including Roblox infrastructure and duplicated client objects. Do not describe these as 95 unique game mechanics. Main-world paths still need a capture. Universe identity covers places within the same experience; it does not imply matching remote coverage in each place.

Observed combat-related paths under `ReplicatedStorage.Requests` include `Action`, `AIWallSlam`, `WallSlam`, `Dashed`, `StopDash`, `Cooldown`, `CooldownNT`, `Effects`, `Flight`, `Beserk`, and `Oozaru`. These are the combat recorder's watched paths. Names alone do not establish arguments, behavior, permissions, or vulnerabilities.

**Recorders → Sparkling Combat Recorder** captures normal inputs (excluding text entry), outgoing calls/returns where executor hooks work, incoming events, relevant attributes and values, motion, health, animations, and up to six nearby actors within 100 studs. It sends no gameplay inputs or remote requests itself.

- Latest: `AnomalyHub/universal/sparkling_combat_capture.txt`
- History: `AnomalyHub/universal/sparkling_combat_<timestamp>_<nonce>.txt`
- Limits: 120 seconds, 16,000 lines / 4 MB; saves every five seconds.
- Stops on death, character changes, teleport, unload, or manual Stop + Save.
- Reports outgoing hook coverage. Direct/namecall paths may duplicate an observation; do not sum them as distinct requests without examining their timing and nesting.
- Compiled successfully; runtime behavior and combat semantics require review of live recordings.

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

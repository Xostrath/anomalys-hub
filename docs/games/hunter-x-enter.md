# Hunter X Enter — findings and handoff

Last reviewed: 2026-11-17. Scope: full feature history from 2026-11-16 onward (training, stamina/hunger, Nen, fishing, food, anti-macro). Work before 2026-11-16 is not reconstructed here. Read the [note rules](README.md) and [project notes](../../PROJECT_NOTES.md) first.

## Read first

- Universe `9857379797`; known place `76571518635501`. The profile is `HunterXEnter` (identity also checks known remote paths).
- **The user considers the Hunter X Enter script finished for now (2026-11-17)**. Work moved to [Sparkling](sparkling.md). Only fix regressions here unless asked.
- The user loads the **private build**: `dist/private/AnomalyPrivate.lua`, installed as `AnomalyPrivate.lua` in the executor workspace. Back up the installed copy before replacing it, then hash-check it after copying. Public builds go to GitHub via the commit workflow in [project notes](../../PROJECT_NOTES.md).
- Scope line, set 2026-10-17 and still in force: build self/NPC features (training, farming, fishing, food, UI). **Do not build combat automation aimed at other players.** Infinite Nen therefore holds the aura only while training (see HXE-005), never for techniques used manually in a fight. Money/item granting and food non-consumption were declined too; don't re-offer them.
- Never tune timings by guesswork. Read the newest report in `AnomalyHub/universal/` first. The user expects us to read the reports instead of asking them to re-record.

## Feature status

| Feature | Module | Status | Evidence |
| --- | --- | --- | --- |
| Auto Training (Push-ups K, Speed, Climb, Meditation) | `src/HunterXEnterTraining.lua` | Push-ups v7 verified live; Speed 0%→100% rest rule implemented / untested live | Push-ups: 61 results / 0 misses (HXE-T007) |
| Auto Nen v2 (all 10 skills) | `src/HunterXEnterAutoNen.lua` | **Verified live** for Ren (2026-11-17) | `hunter_auto_nen_test.txt`: "Training Ren · aura 100%" |
| Infinite Nen v2 | `src/HunterXEnterInfiniteNen.lua` | v1 verified live (basic Nen). v2 rule (basic Nen, or the skill Auto Nen trains) observed holding Ren at 100% | HXE-T006, HXE-T013 |
| Infinite Stamina | `src/HunterXEnterStaminaHold.lua` | Verified live (earlier sessions) | General section, y=0 |
| Reduced Hunger | `src/HunterXEnterReducedHunger.lua` | User-confirmed (push-ups) | HXE-T005 |
| Auto Eat | `src/HunterXEnterAutoEat.lua` | **Verified live** 2026-11-17 | Ate Bread 10→100%, paused and resumed Auto Nen + Auto Training |
| Auto Fish v7 | `src/HunterXEnterAutoFish.lua` | v5/v6 verified live; v7 fixes implemented / untested live | HXE-T008…T012 |
| Auto Anti-Macro (answers the game's check) | `src/HunterXEnterAutoCheck.lua` | Observed success (colour question) | HXE-T012. **Stripped from every public commit** |
| Check alert | `src/HunterXEnterCheckAlert.lua` | Working | Banner + notification when a check appears |
| Recorders (training, hunger flow, fishing) | `src/HunterXEnterRecorders.lua` | Working capture tools | Passive; send no requests |
| Nen Code Capture | `src/HunterNenSourceCapture.lua` | Private diagnostic | Dumps client Nen/fishing scripts for reading |
| Hunger Run Test | `src/HunterHungerRunTest.lua` | Private diagnostic | HXE-T001 |

UI layout: **General** is one stacked section of checkboxes (`ui.Section` + `ui.Checkbox`, created by StaminaHold as `ui.GeneralSection`): Infinite Stamina, Auto Fish, Reduced Hunger, Infinite Nen, then Auto Eat with its threshold slider. **Auto** has the `Auto Training` section (`ui.TrainingSection`), which includes Auto Nen's skill picker, plus the anti-macro card. This is the house style that other games copy.

## Sources and capture workflow

- **Public commit workflow:** build with `tools/build_public.ps1 -SkipPublishCopy` and copy `dist/AnomalysHub.lua` to the repo.
  - Strip the anti-macro block from what gets committed: the exact text `;(function()` + newline + the contents of `src/HunterXEnterAutoCheck.lua` + newline + `end)()` + newline must occur exactly once.
  - Compile-check the stripped file, then commit it with `git hash-object -w --path=AnomalysHub.lua` and `git update-index --cacheinfo`.
  - Diff against HEAD with `--ignore-cr-at-eol`. The working tree then keeps the full build, so `git status` showing `M AnomalysHub.lua` afterwards is expected. Never force-push.

- **Shared auto priority registry:** `controller.HunterAutos`, a list of `{Name, Active(), Busy()?, Pause()->token, Resume(token)}`. Training, Auto Nen and Auto Fish register entries. Auto Eat pauses every active entry, eats, then resumes them in reverse order. It waits up to 8 s on `Busy` (a fish bite or reel) and never pauses during a legendary fight.
- **Shared key ownership:** Training exposes `HunterTrainingInputBusy`; Auto Nen exposes `HunterNenInputUntil` and `HunterNenNeedsActivationWindow`. The anti-macro answerer exposes `HunterCheckActive()`, and Auto Fish holds new casts while it returns true.
- **Input:** game keys are sent with `VirtualInputManager:SendKeyEvent`. Always force-release on state changes, because a cached held state caused HXE-T003.
- **Reports** (executor workspace `AnomalyHub/universal/`), one per feature:

| Report | Feature |
| --- | --- |
| `hunter_auto_fish_latest.txt` | Auto Fish, latest session |
| `hunter_auto_fish_<unix>_<part>.txt` | Auto Fish archive; rolls over at 3000 lines, nothing dropped |
| `hunter_auto_fish_legend_<unix>.txt` | One file per legendary fight |
| `hunter_auto_eat_latest.txt` | Auto Eat |
| `hunter_auto_nen_test.txt` | Auto Nen |
| `hunter_infinite_nen_latest.txt` | Infinite Nen |
| `hunter_autocheck_log.txt` | Anti-macro answerer; per-load copy in `hunter_autocheck_<unix>.txt` |
| Training report | Auto Training |
| `hunter_nen_code_*` | Nen Code Capture dumps |

  Decompiled client modules from the Nen Code Capture are the primary source for game mechanics:
  - `FishingClient`
  - `FishingLegendCinematic`
  - `FishingLegendHost`
  - `FishingConfig`
  - `NenClient`

## Findings

### HXE-001 — Stamina and hunger

- Observed (Hunger Flow capture, 2026-11-16): idle hunger drain is server-timed at 1/s. It takes 0.014 hunger when Saturation is 0; otherwise it takes 0.028 saturation and hunger stays frozen.
- Each `RemoteEvents.PhysicsRemote:FireServer("ConsumeStamina",{amount=0.8})` from running, push-ups, jumping, climbing or swimming is followed about 80 ms later by one hunger −0.0112 drop and one Physics_Stamina gain.
- Eating: `InventoryRemotes.InventoryAction:FireServer("Use","Bread")` gives +8 hunger and +6 saturation (Auto Eat observed exactly +8 per Bread). Inventory counts arrive via `InventoryRemotes.InventoryUpdated` (a full name→count table) and `GetInventory`.
- Client stamina lives in `_G.StaminaSystem` (`GetStamina`, `GetMaxStamina`). The fallback reader is `Character.StaminaBar3D.Container.Fill` plus its `PctLabel`.

### HXE-002 — Reduced Hunger (withhold method)

A persistent `__namecall` hook (`env.__AnomalyHunterStaminaWithhold`) withholds `ConsumeStamina`. The hook only reads `probe.Filter` and `probe.Remote`, so its logic can't change without a rejoin. The running surcharge disappears (HXE-T001); idle drain remains. On push-ups, the user reported that stats were still gained (HXE-T005). That contradicts the earlier prediction that the Stamina stat would stop training: **superseded for push-ups**; running is unverified.

### HXE-003 — Push-ups gauge timing

Every NO_RESULT in the 2026-11-16 23:03 report was a tap less than 0.07 s after `ShowGauge`. v7 waits at least 0.2 s and until server time ≥ t0, and retries once if there's no result after 0.6 s. Result: HXE-T007.

### HXE-004 — Nen system (from decompiled `NenClient`)

- Input: `KeybindController.Matches(action, input)`. The action is `"BasicNen"` for Nen, otherwise the skill name. `toggleSkill` sets `Player.ActiveNenSkill` and fires `RemoteEvents.NenToggle(true, skill)`.
- **Binds are rebindable.** Game defaults are T Ten, R Ren, Y Gyo, Z Ken, U Ko, C In, V En, H Zetsu, N Shu. The user's binds are Nen L, Ten Y, Ren U, Zetsu H, In Z, Gyo T, Shu N, Ken G, Ko R, En C. Auto Nen presses the user's map and only *logs* `KeybindController.GetDisplay` mismatches ("rebound?").
- Skills other than Nen need a server unlock (`NenSkillsFunction:InvokeServer("IsUnlocked",{skillName})`). A locked skill's key is silently ignored, so Auto Nen stops after 3 unconfirmed tries.
- Pool: `_G.NenSystem.getCurrentNen` has exactly one upvalue, the live pool. `getMaxNen` = 10·(1+clamp(Player.Nen/3600)). Every skill drains the same pool.
- Drain multipliers: Ten 0.3, Ren 0.6, Zetsu 0.85, Gyo 1.1, Shu 1.4, In 1.7, Ken 2, Ko 2.25, En 5.
- Stat cap: `MaxSkillPoints` 3600. The user's basic Nen reached 3600 on 2026-11-17, which is why training moved to other skills.
- Quirk: In and Zetsu combine (pressing In while Zetsu is active keeps `ActiveNenSkill="Zetsu"` with an internal flag).
- Toggling is refused while `Meditating`, `PushingUp`, carried, downed or executing.

### HXE-005 — Infinite Nen scope

Infinite Nen writes the pool upvalue to max every frame while training. v1 held only for `ActiveNenSkill=="Nen"`, so with Ten active it did nothing (`writes=0`). The user read that as "doesn't work anymore" (HXE-T013). v2 holds for basic Nen, **or** for the skill Auto Nen is currently training (`controller.HunterAutoNenTraining()`). Techniques switched on manually in a fight drain normally (the scope line in Read first).

### HXE-006 — Fishing protocol (`RemoteEvents.FishingRemote`)

- **OUT:** `("Charge",true)` on E down, then `("Cast",Vector3 target,power)` on E up, then `("Hook")` after a bite, then `("Catch")`.
- **IN:**
  - `Casting`
  - `Landed`
  - `("Bite",1.1,false)`
  - `("Hooked",{minReel,rarity,difficulty})`
  - `("Caught",name,kg,added)`
  - `Escaped`
  - `Reset`
  - `LegendHooked`
- Character attributes: `FishAnim` (hold/charge/cast/wait/bite/reel/catch) and `FishAnimT`, plus `FishingRodEquipped`.
- **The server gates `Catch` on elapsed time (`minReel`) only, not on reel progress.** Auto Fish fires Catch at minReel+0.05 s, retrying up to 3 times 0.35 s apart. It keeps steering only so the fish can't escape before then.
- **`added==false` means "Inventory full! You had to release it."** and the fish is lost. Auto Fish v7 stops on the first one.
- **Casts aim along the camera's look direction** (`doCast`). If no water is found, the client sends `("Charge",false)`, FishAnim goes `charge → hold`, and the game shows "Face the water to cast".
- Reel UI: `PlayerGui.FishingUI.Reel.Track.Zone/Fish` plus `Progress.Fill`. Holding E lifts the zone; it has strong momentum (FALL 2.6 / LIFT 3.8 track-heights/s²).
- Throughput: about 13–14 s per fish (5.5 s cast/bite wait dominates). That is roughly 2,000 fish per 8 h, or about 20 swordfish (1%) and about 4 legendaries (0.2%), assuming bag space.

### HXE-007 — Legendary fish ("Master of the Swamp", 0.2%)

- `LegendHooked {minReel≈22, maxTime 154, intro 6.5, seed, musicLength}` makes `FishingLegendCinematic` run a tension fight.
- State: `Player.LegendCinematic=true`. HUD at `PlayerGui.LegendCinematic`: 28 tension segments, a `Callout` (RELEASE/MASH/PULL), and a `NN%` progress label.
- Holding E raises tension by 0.42/s (+0.85 during a "rush"); releasing lowers it by 0.6/s. Tension ≥1 for 0.7 s snaps the line and **breaks the rod**.
- The client sends `LegendProgress` and `Catch` itself once progress hits 1.
- The developer's own autoplay rule, which Auto Fish copies: hold while not in a rush and tension < (0.85 if already holding, else 0.5); tap during MASH.
- **The cinematic switches the camera back to Custom without restoring its angle**, so casts after a fight can face away from the water (HXE-T011).

### HXE-008 — Anti-macro check

- PlayerGui shows a panel titled "Anti Macro Check" with a question and answer buttons.
- Observed question type: "what is the color of this message?" with options Blue/Green/Yellow/Red (answer = the label's TextColor). The solver also handles named-colour buttons, simple arithmetic and "click X" questions; unknown types are logged UNSOLVED for manual answering.
- Answers are virtual mouse clicks after a 1.4–3.6 s human-like delay. The hub hides itself if it covers the button.
- **Kept out of public builds** by a deliberate commit-time strip.

## Experiments: successes, failures and inconclusive results

| Date / ID | Context / version | Action and expectation | Observed | Verdict | Follow-up |
| --- | --- | --- | --- | --- | --- |
| 2026-11-16 / HXE-T001 | Hunger Run Test | Withhold ConsumeStamina 10 s while running; the surcharge should stop | 91 withheld, 1 straggler drop (vs ~90); speed unaffected; Physics_Stamina gain stopped | **Verified live** | Led to Reduced Hunger |
| 2026-11-16 / HXE-T002 | Auto Fish v1 | Fire Hook/Catch remotes directly | Did not land fish | **Failure** | Switched to E-key input |
| 2026-11-16 / HXE-T003 | Auto Fish v2 (VIM E) | Cast, hook, steer reel | Cast and hooked, but the reel never steered: the hook-tap E DOWN was still cached as held, so `key(true)` was a no-op | **Failure** (cause in log) | v3 forces E up on every FishAnim change |
| 2026-11-16 / HXE-T004 | Auto Fish v3 | Full manual-style catch | 2/2 caught, ~4 s reels | **User-confirmed** | — |
| 2026-11-16 / HXE-T005 | Reduced Hunger, push-ups | Hunger saved; Stamina stat predicted to stop | Hunger saved **and** push-up + stamina stats still gained | **User-confirmed**; prediction superseded | Running stat effect unverified |
| 2026-11-16 / HXE-T006 | Infinite Nen v1 | Aura held while training Nen | 167 s hold; Player.Nen kept rising ~1.05/s | **Verified live** | — |
| 2026-11-16 / HXE-T007 | Auto Push-ups v7 | No early taps | 61 results / 0 misses | **Verified live** | — |
| 2026-11-17 / HXE-T008 | Auto Fish v4 → v5 (minigame skip) | Catch at minReel | 6/6 at ~12.9 s/fish; 13/13 at ~14.3 s/fish; 0 lost | **Verified live** | — |
| 2026-11-17 / HXE-T009 | Auto Fish v5, first legendary | Survive the fight | Rod snapped: steering read the hidden reel's stale positions and held E. No report survived (the restart overwrote the latest file) | **Failure** | v6: legend branch, `reel.Visible` check, per-session archives |
| 2026-11-17 / HXE-T010 | Auto Fish v6, legendary | Win the fight | Server sent `Caught MasterOfTheSwamp 469.3`; fight log shows progress 70%→100% with tension kept under ~0.86. **The user says it is not in their inventory** | **Fight won (verified live); reward inconclusive.** `added` was not logged; most likely a full bag | v7 logs `added` and stops on a full bag |
| 2026-11-17 / HXE-T011 | Auto Fish v6 after the fight | Keep fishing | ~13 min of `charge → hold` cast failures (cast found no water): the camera was left by the cinematic. `CastFails` reset on every charge, so it never stopped | **Failure**; cause found in decompiled source | v7 saves the camera direction of good casts, re-aims (+90/180/270°), stops after 5. **Untested live** — test: turn the camera away mid-session |
| 2026-11-17 / HXE-T012 | Anti-macro during Auto Fish | Answer without breaking fishing | Colour check answered "Blue" in 2.1 s; fishing failures before and after were unrelated (HXE-T011) | **Observed success** | v7 also holds new casts while a check is visible |
| 2026-11-17 / HXE-T013 | Infinite Nen v1 with Ten | — | `skill=Ten writes=0` | Scope limitation, not breakage | v2 (HXE-005) |
| 2026-11-17 / HXE-T014 | Auto Eat v1 | Eat below threshold, pause/resume autos | Paused Auto Nen + Auto Training, 11× Bread 9.99→97.99 (+8 each, ~0.5 s apart), resumed both | **Verified live** | — |
| 2026-11-17 / HXE-T015 | Auto Nen v2 + Infinite Nen v2 | Train Ren with U | "Training Ren · aura 100%" sustained | **Verified live** (report) | Other skills: same path, not individually tested |
| 2026-11-17 / HXE-T017 | Auto Fish v7, second legendary | Win the fight | **Lost:** `Reset legendlost` 10.5 s after the hook. E was held (`held=1`) from the end of the intro, but tension stayed 0 and the game kept calling "REEL IN E"; progress drained 15%→0%. Re-reading HXE-T010: the won fight had the **same dead hold** and was rescued only because a MASH callout made the hub re-tap | **Failure.** Cause from both logs: the game only counts a fresh E press; a hold that began too early is never registered | v8: if tension hasn't risen 0.45 s into a hold (outside a rush), release and re-press (`LEGEND_REPRESS` lines). **Untested live** |
| 2026-11-17 / HXE-T016 | Speed rest 0%→100% | Run to empty, rest to full | — | **Implemented / untested live** | Check `STAMINA_REST` lines; a sprint lost at ≤15% counts as empty |

## References from other games

- **Beyond Nen** ([note](beyond-nen.md)): bounded recorder reports and decision logging. Hunter X Enter's per-session archive and legend files use the same "never lose a rare event" idea.
- **Sparkling** copies this note's UI pattern: sections, checkboxes and sliders like Auto Eat's threshold slider.
- **Reusable patterns from here:**
  - the `HunterAutos` pause/resume registry
  - force-releasing keys on state change
  - a key-only approach plus reading server events, instead of replaying remotes
  - reading decompiled client modules before guessing mechanics
  - an `added`/result flag check before trusting that a reward landed

## Handoff

1. **Latest change (2026-11-17):**
   - Auto Fish v7: camera re-aim, inventory-full stop, waits for anti-macro checks
   - Auto Nen v2: all skills with the user's binds
   - Infinite Nen v2: training-only rule
   - Speed 0→100% rest
   - Auto Eat made public

   Auto Eat and Auto Nen/Infinite Nen (Ren) are verified live. The other v7 and Speed changes compile but are **untested live**.
2. **Open:** confirm v7's camera recovery (turn the camera away while fishing). Confirm the inventory-full stop fires (fish with a full bag). Check that a Speed rest reaches 0% rather than the early-stop rule.
3. **Unknown:** the bag capacity number; whether running under Reduced Hunger still trains the Stamina stat; the Ki/aura impact of other skills at the 3600 cap.
4. No active recording sessions.

## Change history

- 2026-11-17 — Note rewritten from the initial reference inventory into a full ledger (HXE-001…008, HXE-T001…T016) by the Claude chat that built these features.

# Hunter X Enter — findings and handoff

Last reviewed: 2026-11-17. Scope: full feature history from 2026-11-16 onward (training, stamina/hunger, Nen, fishing, food, anti-macro). Work before 2026-11-16 is not reconstructed here. Read the [note rules](README.md) and [project notes](../../PROJECT_NOTES.md) first.

## Read first

- Universe `9857379797`; known place `76571518635501`. The profile is `HunterXEnter` (identity also checks known remote paths).
- **The user considers the Hunter X Enter script finished for now (2026-11-17)**. Work moved to [Sparkling](sparkling.md). Only fix regressions here unless asked.
- The user loads the **private build**: `dist/private/AnomalyPrivate.lua`, installed as `AnomalyPrivate.lua` in the executor workspace. Back up the installed copy before replacing it, then hash-check it after copying. Public builds go to GitHub via the commit workflow in [project notes](../../PROJECT_NOTES.md).
- Scope: build self/NPC features (training, farming, fishing, food, UI); do not add player-targeted combat automation from these notes. **User update 2026-11-17:** Infinite Nen now applies to any manually or automatically activated Nen ability while its toggle is ON, superseding the earlier training-only restriction (HXE-009). Other historical scope decisions remain unchanged.
- Never tune timings by guesswork. Read the newest report in `AnomalyHub/universal/` first. The user expects us to read the reports instead of asking them to re-record.

## Feature status

| Feature | Module | Status | Evidence |
| --- | --- | --- | --- |
| Auto Training (Push-ups K, Speed, Climb, Meditation) | `src/HunterXEnterTraining.lua` | Push-ups v7 verified live; Speed 0%→100% rest rule implemented / untested live | Push-ups: 61 results / 0 misses (HXE-T007) |
| Auto Nen v2 (all 10 skills) | `src/HunterXEnterAutoNen.lua` | **Verified live** for Ren (2026-11-17) | `hunter_auto_nen_test.txt`: "Training Ren · aura 100%" |
| Infinite Nen v3 | `src/HunterXEnterInfiniteNen.lua` | Any active Nen ability, independent of Auto Nen; compiled and mock-tested, awaiting live validation | HXE-009; prior basic Nen/Ren evidence HXE-T006, HXE-T013, HXE-T015 |
| Infinite Stamina | `src/HunterXEnterStaminaHold.lua` | Verified live (earlier sessions) | General section, y=0 |
| Reduced Hunger | `src/HunterXEnterReducedHunger.lua` | User-confirmed (push-ups) | HXE-T005 |
| Auto Eat | `src/HunterXEnterAutoEat.lua` | **Verified live** 2026-11-17 | Ate Bread 10→100%, paused and resumed Auto Nen + Auto Training |
| Auto Fish v7 | `src/HunterXEnterAutoFish.lua` | v5/v6 verified live; v7 fixes implemented / untested live | HXE-T008…T012 |
| Auto Anti-Macro (answers the game's check) | `src/HunterXEnterAutoCheck.lua` | Observed success (colour question) | HXE-T012. **Stripped from every public commit** |
| Check alert | `src/HunterXEnterCheckAlert.lua` | Working | Banner + notification when a check appears |
| Perfect Block recorder | `src/HunterXEnterRecorders.lua` | Six local perfect-block outcomes across two captures, including two isolated key cycles; timing/eligibility unresolved | HXE-011/012; latest capture hit its line cap at 94.884 s; passive only |
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

### HXE-005 — Infinite Nen v2 scope (superseded by HXE-009)

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
   - Infinite Nen v3: any active ability while enabled (HXE-009); replaces the training-only rule
   - Speed 0→100% rest
   - Auto Eat made public

   Auto Eat and Auto Nen/Infinite Nen (Ren) are verified live. The other v7 and Speed changes compile but are **untested live**.
2. **Open:** confirm v7's camera recovery (turn the camera away while fishing). Confirm the inventory-full stop fires (fish with a full bag). Check that a Speed rest reaches 0% rather than the early-stop rule.
3. **Unknown:** the bag capacity number; whether running under Reduced Hunger still trains the Stamina stat; the Ki/aura impact of other skills at the 3600 cap.
4. Latest block review HXE-012 adds four local outcomes, including two isolated key cycles; the capture stopped at its 16,000-line cap. Compare same-animation failed cycles and stun/block state before proposing active timings.

## Change history

- 2026-11-17 — Note rewritten from the initial reference inventory into a full ledger (HXE-001…008, HXE-T001…T016) by the Claude chat that built these features.


## HXE-009 — Infinite Nen v3: any active ability (2026-11-17)

**Explicit user request:** make infinite aura work when any Nen ability is turned on. This supersedes HXE-005's training-only restriction for Infinite Nen. Existing player-targeted automation scope is otherwise unchanged; this change only alters the local aura feature's activation condition.

**Observed problem:** the latest local v2 report shows Ren and then En active while writes stays zero. Source gates non-basic skills on equality with HunterAutoNenTraining(), so manually activated skills receive no hold when Auto Nen is inactive or training another skill. This is the same scope limitation previously recorded for Ten, not evidence that the pool setter stopped working.

**Implemented:** Infinite Nen v3 follows ActiveNenSkill whenever it is a nonempty string other than None/none, matching Auto Nen's existing active-skill convention. It no longer calls HunterAutoNenTraining(). This covers Nen, Ten, Ren, Gyo, Ken, Ko, In, En, Zetsu and Shu, including manual activation and switching between techniques. The Infinite Nen toggle still controls the feature and retains its saved preference. No skill is activated automatically. Inactive/nil/invalid values stop writes; disabling or unloading stops writes. Existing finite-value and captured getter-shape validation remain. Skill switches now produce HOLD entries even when the active flag stays true. Report and fallback card identify v3 and the expanded scope. Auto Nen's compatibility accessor remains available, with its obsolete scope comment corrected.

**Validation:** changed modules and both staged builds compile. 31 mocked assertions cover all ten known abilities without consulting the trainer, no redundant write when full, inactive sentinels, disable/re-enable, unload and skill-switch logging. Any-active scope is implemented but not yet live-validated; the historical basic-Nen and Auto-Nen/Ren successes do not individually validate every skill. Source and exact public/private module replacements preserve concurrent work; private build backed up and hash-verified when installed. Existing game-specific activeSkill logic was reused; other game notes' input/result evidence distinctions remain applicable, with no foreign aura semantics imported.

**Handoff:** reload the private hub, leave General → Infinite Nen ON, then turn on a Nen ability normally. Auto Nen can stay OFF. Verify the ability stays active past its normal drain duration and inspect the v3 report's HOLD/SAMPLE/write counts; a visually full bar alone is insufficient proof. Keep any new failures and skill-specific exceptions in this note.


## HXE-010 — perfect-block recording review (2026-11-17)

Reviewed four finalized passive-v1 block histories from place 76571518635501 / universe 9857379797. Session starts 1794967736 / 1794967744 / 1794967806 / 1794968067 end at 0.044 / 42.560 / 120.024 / 80.401 seconds with DEATH / STOPPED / TIME_LIMIT / UNLOAD respectively. The first is not a usable combat trial. No recorder ERROR events occur. Every capture reports actor discovery hitting 1000 objects and GUI discovery hitting 600 objects, so actor/UI coverage is incomplete. A newer session 1794968184 was still RECORDING at the 25.079-second snapshot; do not merge its partial counts into completed tests.

**Observed block signaling:** F DOWN/UP and outgoing ReplicatedStorage.Assets.Remotes.Combat observations contain (Combat, Block, Start) / (Combat, Block, End). The 120-second capture has 20 F-down/up pairs and 123 Start / 29 End OUT observations. The latest completed capture has seven F-down/up pairs and 90 Start / 13 End OUT observations. These are hook observations, not independent accepted actions: repeated requests and duplicate hook paths must be distinguished before drawing conclusions. No remote is sent or replayed by this review.

**Replicated state:** local Character.Blocking, Character.PerfectBlockWindow, Character.Stunned and Character.StunType are present. The latest completed run sets Blocking=true at 10.3613 seconds and PerfectBlockWindow=0.18 at 10.3615 after F-down at 10.1919 (~169.6 ms later). They clear at 10.5167/10.5168 after F-up at 10.4020. Another F-down at 13.0377 precedes Blocking/window updates at 13.3221/13.3222 (~284.5 ms); F-up is 13.3443, Blocking clears 13.4625, window clears 13.5136. These are client receipt delays, not server timestamps or an input latency calibration. StunType values Normal and Total occur; Stunned carries a numeric-looking string and later becomes nil. The epoch/expiry meaning is unverified.

**Outcome evidence:** the latest completed run has one ClientFX Combat/Hit with BlockHit and blockenmieux naming the local character (13.4432 seconds), consistent with a normal block. Its three PerfectBlockHit/perfectblock effects name other characters, not the local effect target. Across the four finalized reports, five such perfect-block effects are observed and none names the local character in the primary model argument. Do not credit nearby effects as this user's perfect blocks or infer defender/attacker roles solely from proximity. Five local negative-health changes occur in the latest completed run; the preceding 120-second run has 21 and the 42.56-second run has two.

The normal-block effect arrives about 121 ms after the local 0.18 window-attribute receipt while that attribute has not yet cleared. This directly cautions against treating that attribute's receipt time plus 180 ms as a guaranteed perfect-block schedule. Network ordering, server eligibility, actor role and effect semantics need further correlation. The value 0.18 is an observed field, not a calibrated automation constant.

**Decision / handoff:** the recorder is collecting useful input, state and outcome signals, but no local perfect-block success has been established in these completed captures. No runtime change or active block automation was made. Retain a local-target result filter and compare F, block-request, Blocking/window/stun, attacker-animation and health timelines when reviewing the newer run after it completes. A directly observed local perfect block is needed before claiming successful timing. Reuse Sparkling/Beyond Nen's separation of input, request receipt and outcome evidence; do not transfer Sparkling animation IDs, cooldowns or delays. Raw captures and character/player names remain private.


## HXE-011 — two local perfect-block outcomes observed (2026-11-17)

The previously partial session starting Unix 1794968184 has now finalized at 120.004 seconds (TIME_LIMIT), passive recorder v1, place 76571518635501. The report has 11,363 text lines, 43 F-down and 43 F-up observations, 243 Block/Start and 58 Block/End OUT hook observations, 35 observed local health decreases, and no recorder ERROR event. Actor discovery (1000) and GUI discovery (600) limits remain; coverage is incomplete. Multiple OUT observations per physical key cycle are not independent accepted blocks. No runtime change, remote replay or automated input was performed during review.

**Verified observed outcome signals:** two incoming ClientFX Combat/Hit events name the local character and carry SFX=perfectblock, Particle=PerfectBlockHit, Type=Fists, at 59.6354 and 71.1579 seconds. In each sequence the opponent actor 2 immediately receives Stunned and StunType=Total, and the local PerfectBlockWindow clears. No local health decrease is recorded at either effect. This combination supports two local perfect-block outcomes, unlike the nonlocal effects in HXE-010. Five additional perfect-block effects target other characters and are excluded. Do not describe this as a two-of-43 success rate: key presses are not attack opportunities.

| Event | First outcome (seconds) | Second outcome (seconds) |
| --- | --- | --- |
| Previous local health decrease | 58.7742 | 70.2540 |
| Earlier F down before opponent animation | 59.3831 | 70.9585 |
| Opponent animation 92677769777799 begins | 59.4311 | 70.9698 |
| Local Stunned / StunType clear | 59.4680 / 59.4681 | 70.9705 / 70.9706 |
| Local Blocking=true / PerfectBlockWindow=0.18 | 59.5159 / 59.5160 | 71.0706 / 71.0707 |
| F up | 59.5365 | 71.0623 |
| Another F down | 59.5956 | 71.1283 |
| Local window clears | 59.6353 | 71.1577 |
| Opponent receives Total stun | 59.6354 | 71.1578 |
| Local perfect-block effect | 59.6354 | 71.1579 |

Both outcomes follow the same opponent animation, 92677769777799, by 204.3 and 188.1 ms from client animation-start receipt. Each is preceded by an observed Combo=2 HitAnimation targeting the local character; the later attack's exact combo index is not independently confirmed. The shared animation is a useful candidate association, not a universal timing constant or complete weapon map.

**Attribution limit:** the nearest F down is only 39.8 / 29.6 ms before the perfect-block effect, but local Blocking/window state already appeared before that press. Earlier presses, releases, repeated requests and network receipt order overlap. Do not set an automatic press delay from the nearest-key delta or assume the latest press caused the outcome. The earlier F-down-to-Blocking receipts are approximately 132.8 / 112.1 ms, while local stun is clearing in both sequences. These are client observations; exact server acceptance and stun eligibility remain unknown.

**Handoff:** local perfect-block success signals are now available and supersede HXE-010's unresolved local-outcome question. Preserve the paired effect/local-target/opponent-stun evidence as the success reference. Next discriminate input timing using one deliberate F press per incoming attack, with full release between attempts and actor/animation context retained; avoid holding or repeatedly tapping throughout a combo when calibrating. Do not transfer the 188–204 ms effect interval into a scheduling delay. No active perfect-block implementation is claimed. Continue separating attempts, state transitions, confirmed local effects, failures and uncertain cases as in the Sparkling/Beyond Nen notes.


## HXE-012 — four more local perfect blocks; two isolated key cycles (2026-11-17)

Reviewed finalized passive-v1 session starting Unix 1794968758, place 76571518635501. It stops with LIMIT_REACHED at 94.8839 seconds after reaching the 16,000-event-line cap (16,010 text lines including report/header material), before its normal 120-second duration. Later combat is not covered. Actor-discovery (1000 objects) and GUI-discovery (600) warnings persist; no recorder ERROR event occurs. Captured: 60 F-down / 60 F-up observations, 352 Block/Start and 118 Block/End OUT observations, 12 other Combat/M1 OUT observations, 29 local health decreases. These are observed events, not unique accepted actions or a success-rate denominator.

**Four local success references:** incoming PerfectBlockHit/perfectblock effects name the local character at 40.5011, 50.8854, 56.4690 and 80.7079 seconds. Every one is paired with the same nearby opponent (actor 1) entering Stunned / StunType=Total and the local window clearing. No local damage is observed at the effect instants. Ten other perfect-block effects target other characters and are excluded. Together with HXE-011, six local outcome signals are now documented across two recordings. The 40.5011 and 56.4690 cases still have overlapping F cycles and cannot isolate the responsible press.

**Two clearer single-key-cycle sequences:** within the one-second window preceding each of the following outcomes, there is one F-down followed by its F-up and no intervening second F-down before the effect. Local Blocking/window becomes newly present after that down; the next press occurs after the effect. This improves temporal attribution but does not supply server input timestamps or eliminate duplicate/repeated Start requests.

| Event | First isolated sequence | Second isolated sequence |
| --- | --- | --- |
| Prior local damage | 50.0960 s | 79.7879 s |
| Opponent animation 87786111103136 starts | 50.6518 s | 80.5145 s |
| F down | 50.6760 s | 80.4702 s |
| Local Stunned / StunType clear | 50.8117 / 50.8118 s | 80.5146 / 80.5147 s |
| Local Blocking=true / PerfectBlockWindow=0.18 | 50.8119 / 50.8120 s | 80.5889 / 80.5890 s |
| F up | 50.8593 s | 80.6412 s |
| Local perfect block / opponent Total stun | 50.8854 s | 80.7079 s |
| Next F down | 50.9420 s | 80.7214 s |

F holds are 183.3 and 171.0 ms. F-down-to-outcome is 209.4 / 237.7 ms; F-down-to-Blocking receipt is 135.9 / 118.7 ms. The first press occurs 24.2 ms after observed opponent animation start; the second occurs 44.3 ms before it. Therefore a fixed delay from client AnimationPlayed cannot be inferred from these successes. Both presses occur while local Stunned is still present; Blocking arrives after it clears. This supports investigating game-side eligibility, buffering/retry and replicated-state timing together. It does not justify discarding every press made while a stun attribute is set or claiming that the attribute's clear receipt is the exact server eligibility boundary.

The two remaining successes associate with opponent animation 87786111103136 at 40.2564 seconds (effect +244.7 ms) and 92677769777799 at 56.2743 seconds (effect +194.7 ms). The latter repeats HXE-011's candidate animation. Effects follow these observable animations, but their timestamps alone are not calibrated hit windows, and this is not a complete attack/weapon map.

**Decision / handoff:** successful local outcomes now have repeated support, including two much cleaner key cycles; another undifferentiated F-spam baseline is unnecessary. No runtime or active-block code changed during this review. Use these as positive references when comparing failed cycles of the same animation, particularly whether local stun persisted, block state appeared, and the effect targeted self. Preserve request-observation ambiguity and incomplete actor coverage. Before relying on long further captures, reduce irrelevant block-recorder traffic or record shorter focused sessions: this run ended about 25 seconds early at its line cap. Do not simply claim a 180 ms universal timing from the window attribute or hold durations. Continue the game-specific investigation without importing Sparkling timing constants.

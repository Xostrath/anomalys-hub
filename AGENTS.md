# Anomaly's Hub contributor instructions

Read [PROJECT_NOTES.md](PROJECT_NOTES.md), the [game-note index](docs/games/README.md), and the target game’s note before editing or adding a game profile.

- Inspect existing UI helpers, capture tools, and adapters before creating replacements.
- Use the shared Anomaly interface. Hunter X Enter is the visual reference; game adapters change content, not the theme.
- Put activity capture controls in **Recorders**, not General or Auto. The existing **Capture Fingerprint** discovery action remains in Settings.
- Give supported games an explicit profile identity and sidebar/header labels. Do not leave a supported game branded Universal.
- Keep the hub outside PlayerGui when an isolated UI root is available; game styling can otherwise overwrite fonts and borders.
- Treat captures as observations, not proof of server authority or successful actions. Report hook coverage and possible duplicate observations.
- Preserve concurrent and unrelated edits. This repository holds generated public artifacts; make lasting changes in the source tree and build pipeline too.
- Publish only the intended public changes. Never commit private builds, local secrets, raw captures, or personal workspace files.
- Compile-check changed Luau and both build variants when applicable. Compilation is not live validation; state what was actually tested.
- Update PROJECT_NOTES.md when adding profiles, changing navigation, or changing the build/capture workflow.

## Game evidence and handoff

- Maintain a living per-game note in `docs/games/`. Use `TEMPLATE.md` when adding a game; explicitly mark missing information unknown.
- Record discoveries, verified successes, failures, and inconclusive tests in the same change or immediately after reviewing a new recording. Include version/place, evidence, expected versus observed behavior, confidence, and next step. Preserve superseded results.
- Separate source inspection, compile/mock checks, user confirmation, and verified live results. A feature existing in code is not proof that it works in game.
- Before creating a feature, check other games’ notes and their referenced implementations for useful precedent. Record what was reused; do not transfer game-specific timings, remotes, or semantics without evidence.
- Keep the current feature table and next-chat handoff current. Public notes contain sanitized findings, never raw captures, player identifiers, private builds, or secrets.

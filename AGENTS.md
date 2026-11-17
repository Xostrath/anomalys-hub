# Anomaly's Hub contributor instructions

Read [PROJECT_NOTES.md](PROJECT_NOTES.md) before editing or adding a game profile.

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

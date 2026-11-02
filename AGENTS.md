# AGENTS.md — Working rules for Bar Tab

These rules apply to any coding agent (Claude, Codex, Copilot or others) working on this repo.
Read `HANDOFF.md` and `PROJECT_MEMORY.md` first.

## Process
1. **Approval gates.** Work in agreed stages. Finish a stage, test it, report to the owner, then stop.
   Never start the next stage without the owner's approval.
2. **Back up first.** Before changing `index.html` for a new stage, push a backup branch of the
   current `main` (for example `backup-stage2-before-stage3`). Tags may not push; branches do.
3. **GitHub is the master copy.** Make changes in the repo, then rebuild any artifact copy from
   `index.html`. Never edit the artifact copy by hand.
4. **Small, testable steps.** No whole-app rewrites. No new external dependencies without a clear
   reason agreed with the owner.
5. **Commits:** describe what changed in plain language. Commit only what you intended to change.

## Preserve what exists
- Keep every existing feature: logging, estimates, the businessman's customization, animations,
  ragdoll physics, throwable props, mini-games, tilt, personalities, reports, history, backups.
- The owner's mobile layout (bottom tabs, quick bar, opt-in dragging, editing toolbar) stays.
- Keep joint tracking and party escalation in the web version. App Store compliance is a separate
  future configuration.

## Data safety
- Never rename or remove a `localStorage` key or field. Add new fields with defaults.
- **When you add a setting, update the settings loader too.** This project has twice had settings
  that were saved but not read back after a reload. Test reloads for every new setting.
- Never silently delete user data. Destructive actions need an in-page confirm. Restores keep a
  safety copy.
- Old saved data must keep loading. Test with old-format data whenever storage changes.

## Content rules
- Alcohol and "high" figures are labelled as rough estimates, never proof someone is sober or safe to drive.
- No mechanisms that encourage excessive drinking. No achievements or rewards for drinking more.
- Budget reactions are about spending, not intoxication.
- Reports, case files and court verdicts use only logged data. Never invent events.
- Natural English in all user-facing text. Keep humour varied (rotate lines, avoid repeats).
- No passwords, tokens or private customer information in code, notes or commits.

## Quality bar (check before reporting a stage done)
- Re-run all automated checks, not just the new ones.
- Test over a local HTTP server, not `file://` (storage is unreliable there).
- Layouts at 375×667, 430×932, 667×375, 932×430 and desktop: no sideways scrolling, buttons at
  least 44×44 px, 16 px minimum side gutters, nothing hidden behind the bottom bar.
- Check that new overlays don't break the mini-games or ragdoll dragging.
- Keep animations light enough for older phones (CSS transforms and opacity; avoid heavy filters
  and per-frame layout work).
- No JavaScript errors in the console during tests.

## Reporting
At the end of a stage, give the owner: what changed, what was tested, bugs found and fixed, and
remaining limitations. Update `HANDOFF.md` to match.

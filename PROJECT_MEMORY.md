# Bar Tab — Project Memory

Long-lived facts about the project: what it is, how it's built, and the decisions behind it.
For current status and the next step, see `HANDOFF.md`. For working rules, see `AGENTS.md`.

Last updated: 2026-11-02

---

## What it is

Bar Tab is a mobile-first drink counter and spending tracker with an animated, interactive
businessman mascot. The goal is a funny, sarcastic "night-out companion" that people share
because it's entertaining and keep using because it helps them watch their spending.

The owner (King) maintains it himself and wants it to stay suitable for an eventual Apple
App Store build, which will be a separate, compliant configuration (see Decisions).

## Confirmed facts

### Hosting and copies
- **Master copy:** GitHub repo `rln0518/bar-tab`, branch `main`, file `index.html`.
- **Website:** GitHub Pages at `https://rln0518.github.io/bar-tab/`.
- **Secondary copy:** a Claude artifact named "Bar Tab", rebuilt from `index.html` by taking the
  `<title>`, the Google Fonts `<link>` and the `<body>` contents. It declares the `downloads`
  capability so backups and share images can be saved. It is never edited directly.
- The artifact and the website store data separately (each browser origin has its own storage).

### Technology
- One self-contained HTML file: inline CSS and JavaScript, inline SVG for the scene and character.
- No build step, no libraries, no package manager.
- Only external resource: Google Fonts (Big Shoulders Display, Manrope), with system-font fallbacks.
- All data lives in the browser's `localStorage`. No accounts, no server, no cloud.

### Saved data (localStorage keys)
Never rename or remove these; only add fields.

| Key | Holds |
|---|---|
| `bartab` | Tonight's log: entries `{id, t, s, p}` (drink id, timestamp, standard drinks, price paid). `id` is a drink id, `water` or `joint`. |
| `bartab-sessions` | Saved nights: `{id, start, end, savedAt, closed: "manual"\|"auto", currency, budget, weight, food, persona, reportSeen, entries}` |
| `bartab-set` | Settings: weight, food (ate a meal), prices, look (skin/hair/beard/suit/tie), currency `{code, custom}`, budget, thresholds `[4]`, persona |
| `bartab-abv` | Beer strength slider |
| `bartab-best` | Mini-game high scores |
| `bartab-night` | Budget-reaction stage reached tonight (so reactions never replay) |
| `bartab-report-seen` | Id of the last Morning After report shown (guards against reopening) |
| `bartab-before-restore` | Safety copy made before a backup restore (powers "Undo last restore") |
| `bartab-achievements` | *(Stage 2, not yet on GitHub)* unlocked achievements and counters |

### Features on GitHub (`main`, through Stage 1)
- **Logging:** 9 drinks plus Light Beer, Joint tile, water, undo; beer-strength slider; per-drink prices;
  quick bar (last drink repeats, + Joint, + Water, Undo).
- **Estimates:** standard drinks, 7-level "how drunk" meter, sober countdown, pace warning, weight
  and meal adjustment, separate "high" level for joints, hangover state.
- **Mascot:** customizable businessman (skin, hair, beard, suit, tie), 7 levels of dance animation,
  speech bubbles, tap reactions, ragdoll drag-and-throw physics, throwable office props (desk,
  laptop, chair, plant, mug, phone, disco ball), impact effects, tilt mode, live digital clock.
- **Mini-games:** Trash Toss and Wrecking Ball, with saved best scores.
- **Phone layout** (built by the owner): bottom tabs (Drinks, Character, More/Log, History), quick
  bar, opt-in dragging so swipes scroll, keyboard-aware editing toolbar.
- **Stage 0:** session history (New night saves, auto-save after 8 h idle), History tab, currency
  (BZ$ / US$ / custom), optional nightly budget with 4 editable warning points, backup and restore.
- **Stage 1:** Morning After Report with share card, 4 personalities (Troll, Disappointed Uncle,
  British Butler, Grim Reaper) plus the original Businessman, Financial Grim Reaper budget
  reactions, easter eggs (Q4 chart crash, clock alarm, 7-tap secret, exact-budget line, 11:11 wish).

### Backup points on GitHub
| Branch | Commit | Contents |
|---|---|---|
| `backup-v4-before-stage0` | `a974201` | Version before Stage 0 |
| `backup-stage0-before-stage1` | `cc38832` | Stage 0 |
| `backup-stage1-before-stage2` | `9f51285` | Stage 1 (same as current `main`) |

Git tags could not be pushed from Claude's environment, so backups are branches.

## Key decisions

1. **GitHub is the master copy.** The artifact is rebuilt from it, never the other way round.
2. **Never silently delete data.** New night and auto-save move the night into history. Deleting a
   saved night needs a confirm. Restores keep a safety copy with undo.
3. **Auto-save rule:** a night closes after 8 hours with nothing logged, counted from the last
   logged entry, so an active night is never interrupted. Checked on open, every 30 s, when the
   app comes back to the foreground, and before each new entry.
4. **"Ate a meal" resets** when a night is saved. Weight and other preferences persist.
5. **Hangover** comes from the most recent drinking, saved or not: shown when the meter is at zero
   and a big night (peak above 4.5 effective drinks) happened within 18 hours.
6. **Money:** default currency BZ$; US$ and a custom symbol available. Changing currency changes
   the symbol only, never converts prices. No default budget; the user sets and changes it.
7. **Budget reactions** focus on spending, never intoxication. Stages: warning points 25/50/75/100%
   (editable), plus a "well over" stage at 115% of the limit. Each plays once per night.
8. **Joint tracking and party escalation stay in the web version.** An optional "Adult Mode" may be
   added later, but hiding features behind a switch is not assumed to satisfy Apple's rules. A
   separate, compliant App Store configuration will be designed when that work starts.
9. **Dancing ignores the phone's Reduce Motion setting** (owner's request; Reduce Motion had been
   freezing the character).
10. **Reports and case files use only logged data.** No invented events. Verdicts are seeded per
    night so the same night always shows the same text.
11. **Achievements never reward drinking more or getting drunker** (Stage 2).
12. **Staged development with approval gates.** Each stage starts with a backup branch, ends with
    tests and a summary, and waits for the owner's approval before the next stage.

## Known limitations (confirmed)
- Alcohol and "high" figures are rough educational estimates, not a breathalyzer, and must never be
  used to judge whether someone can drive.
- The artifact and the website keep separate data; backups are the way to move data between them.
- Safari can clear site data for websites not opened for a while; the backup feature mitigates this.
- Tilt mode can't read the phone's motion sensor inside the Claude app; it needs the website in Safari.
- Testing over `file://` in Chromium is unreliable (storage writes are lost on reload); test over a
  local HTTP server instead.
- The single file is large (about 185 KB on `main`, about 230 KB with Stage 2) and will need
  splitting into files before any App Store packaging.

## Needs verification
- Whether GitHub Pages is switched on and serving the latest `main` (Claude could not open
  `github.io` from its environment).
- Behaviour on a real iPhone: the share sheet, custom fonts on generated images, the iOS keyboard
  "Done" toolbar, Safari's motion permission for tilt, and backup file downloads.
- Whether a Morning After report can still reopen once after a very fast reload (mitigated with a
  second marker, not reproduced since, not fully ruled out).

## Roadmap (agreed order)
- **Stage 2 (in progress):** alcohol-meter continuity fix, Night-Out Detective, Bar Tab Court, Achievements.
- **Stage 3:** Group Tabs, offline on one device, no accounts.
- **Stage 4:** "Alternate universe" purchase comparisons (editable estimated prices, labelled as
  estimates) and Monthly Bar Tab Wrapped with share cards.
- **Stage 5:** home-screen app setup, offline support and polish.

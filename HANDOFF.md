# Bar Tab — Handoff

Where the work stands right now and what to do next.
Background and decisions: `PROJECT_MEMORY.md`. Rules: `AGENTS.md`.

Last updated: 2026-11-02

---

## Current state

| Item | Status |
|---|---|
| `main` on GitHub | Stage 1 complete (`9f51285`). Approved by the owner. |
| Backup before Stage 2 | Branch `backup-stage1-before-stage2` (`9f51285`) exists. |
| Stage 2 code | Written and tested, **but not committed or pushed**. It exists only in Claude's cloud working copy of the repo (`index.html`, about 506 lines added). See the risk below. |
| Claude artifact | Shows Stage 1. Not yet rebuilt with Stage 2. |
| Stage 2 summary to owner | Not yet delivered. |
| Stage 3 | Not started. Must not start without the owner's approval. |

## Stage 2: what was built (uncommitted)

**Alcohol-meter continuity fix.** The meter now simulates absorption across recent drinks from both
tonight and saved nights (last 24 h), so New night resets drinks and money but not alcohol still
in the estimate. A note shows how much is carried over from the last saved night. Hangover uses the
same model. The meter label now says "(estimate)" and the note says it is not a breathalyzer.

**Night-Out Detective.** A typewriter-style case file per saved night, opened from History or the
Morning After report: case number, title, evidence tags, a timeline with one line per logged
entry, spending milestones and budget-violation alerts from real totals, notes for 90-minute+
silences, a quote from the chosen personality, a summary and a stamp.

**Bar Tab Court.** "The People v. Your Wallet": the businessman as judge (wig, robe, gavel, using
the user's chosen look), charges and commendations from real data, evidence, a "Hear the verdict"
reveal (Guilty / Not Guilty / Hung Jury), a sentence, a remark from the personality, and a
shareable verdict image. Gavel easter egg (5 quick taps).

**Achievements.** 16 badges (one secret) in a gallery at the top of History, with locked and
unlocked states, how-to-earn descriptions, unlock toasts that never block logging, retro unlocks
from past nights, persistence in `bartab-achievements`, and inclusion in backups. None reward
drinking more.

## Stage 2 test status

Tested over a local HTTP server with Playwright (Chromium), not on a real phone.

- Stage 2 suite: 52/52 passing, twice.
- Stage 0 suite: 51/51, Stage 1 suite: 67/67, three runs each, all stable over HTTP.
- Tilt, Trash Toss and Wrecking Ball checks passing.
- Case file and court at 375×667, 430×932, 667×375, 932×430 and desktop: no sideways overflow,
  all buttons at least 44×44.

**Not yet re-tested:** two small fixes made after the last full run:
1. Drink names in case-file lines were capitalised mid-sentence ("Another Beer"); now lowercase with the sentence start capitalised.
2. The achievement toast moves to the bottom of the screen while a report, case file or court is
   open, so it no longer covers the Close button.

The test scripts live in Claude's temporary workspace, not in the repo (see Unresolved).

## Bugs found and fixed during Stage 2
- A court charge variant didn't state how much the budget was exceeded.
- The gavel was hard to tap (small target, covered by the bench); now has a larger tap area.
- Test harness: `file://` pages lost storage on reload; all suites moved to `http://127.0.0.1:8765`.

## Unresolved issues
1. **Risk: Stage 2 could be lost.** It is uncommitted in an ephemeral cloud workspace. If that
   workspace is reclaimed, Stage 2 must be rebuilt. Safest fix: push it to a work-in-progress
   branch (for example `stage2-wip`) as soon as the owner agrees, without touching `main`.
2. **Needs real-iPhone testing:** share sheet, canvas fonts, iOS keyboard toolbar, tilt permission,
   backup downloads.
3. **GitHub Pages status unverified** from Claude's side.
4. **Test suites are not in the repo.** Consider adding them (for example under `tests/`) if the
   owner approves, so any agent can re-run them.
5. **Report reopen edge case** after a very fast reload: mitigated, not fully ruled out.

## Next step

1. Get the owner's OK to save Stage 2 (push to `stage2-wip`, or commit to `main` after the checks below).
2. Re-run all suites (Stage 0, 1, 2, games) over HTTP to cover the two late fixes.
3. Check the case file, court and achievements once more at the four phone sizes and desktop.
4. Commit Stage 2 to `main`, rebuild the artifact from `index.html`, publish it.
5. Send the owner the Stage 2 summary (changes, tests, bugs fixed, limitations), then **stop** and wait
   for approval before Stage 3 (Group Tabs).

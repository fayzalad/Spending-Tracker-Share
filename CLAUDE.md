# Tracker (Share fork) — project record

This is a **fork** of a personal spending tracker, customised for a friend in the UK. The full
project record — architecture, money model, month model, data model, complete feature inventory
and the whole commit history — lives in the personal repo's `CLAUDE.md`, at
`Claude code/Spending tracker/CLAUDE.md` (repo `fayzalad/Spending-Tracker`). **Read that first.**
This file covers only what differs here, plus this fork's own append-only session log.

Per the global "always record what you changed" rule: this fork keeps its session log **here**,
in `CLAUDE.md`. `README.md` is the friend-facing guide and gets a plain-language "What's new"
entry instead; it is not the place for operational notes.

---

## How this fork differs

| | Personal | This fork |
|---|---|---|
| Remote | `fayzalad/Spending-Tracker` | `fayzalad/Spending-Tracker-Share` |
| Currency | R / £ / $ / ₵, `homeCur` ZAR | **GBP only** (`SYM={GBP:'£'}`), `homeCur` GBP |
| Default allowance day | 25 | 1 |
| Investments | Full tab, CoinGecko + Alpha Vantage | Removed entirely |
| Locale | `en-ZA` | `en-GB` |
| Categories | Full SA list | Trimmed |
| Tests | 425 | 355 |
| README | Technical reference + changelog | Plain-language guide for the friend |

The fork also kept two things the personal repo reverted on 2026-09-12: the **default currency**
setting and the **first-time user guide**. Histories are independent — a commit in one never
touches the other, and changes are ported by hand.

## Porting changes from the personal repo

Most changes apply almost verbatim, but watch for:

- **No `cycCur` UI.** The personal build has a per-month currency selector (`cycCurRow`,
  `cycCurSel`); this fork has none. Drop those lines when porting.
- **`ensureCycCur()`** exists here and does not in the personal build. Any function that refiles
  entries must call it over the resulting set of cycles.
- **`en-GB` not `en-ZA`** in every `toLocaleDateString`. This matters in tests: en-ZA renders
  "23 Sept", en-GB renders "23 Sep", so a `/23 Sept/` assertion fails here. Use `/23 Sep/`.
- **Display amounts are GBP-converted**, so a test asserting a raw rand figure from the DOM will
  fail. Assert on stored state, or on a converted value.

## Running and deploying

```bash
node test.js
```

**355 checks.** Boots the real `index.html` in jsdom with a frozen clock. Run before and after
every change.

To deploy: edit `index.html`, **bump the `slip-build` meta tag on line 7**, commit, push to
`main`. Pages redeploys in about a minute and the running app offers the update.

---

## Session log

Newest first. Append-only: never rewrite or delete an older entry. If a later change undoes an
earlier one, record the undo as its own entry.

### 2026-09-25 — Created CLAUDE.md for this fork

- **Changed:** Added this file: how the fork differs from the personal repo, the gotchas when
  porting a change across, and this session log. Points at the personal repo's `CLAUDE.md` for
  the full project record rather than duplicating it, so the two cannot drift apart.
- **Why:** Requested a complete written record of everything done to the tracker. The global
  always-record rule applies per project root, and this is its own root.
- **Files:** `CLAUDE.md` (new)
- **Revert:** `git rm CLAUDE.md && git commit`
- **Verified:** Divergences checked by reading this fork's `index.html` — `SYM={GBP:'£'}`,
  `homeCur:'GBP'`, `day:1`, `en-GB` locale, no investments tab.

### 2026-09-24 — A stranded allowance turns the month over on launch

- **Changed:** Ported `healForwardIncome()` from the personal repo, run once from `boot()`. Any
  income entry filed into a month later than its own date moves that month's start onto the
  entry's date and lets everything unpinned refile; the fork's version also re-runs
  `ensureCycCur` over the resulting cycles. Skips entries carrying `hold: true`, and never touches
  a cycle earlier than the live one. `e.hold` is written in `add()` when a forward month is picked
  with the start-the-month checkbox unticked. `moveSave` deletes `hold`. Build `2026-09-24-2`.
- **Why:** The earlier fix that day only changed what happens when you *log* an entry; anything
  already recorded the old way still needed correcting by hand.
- **Files:** `index.html`, `test.js`, `README.md`. Commit `888024e`
- **Revert:** `git revert 888024e`, then bump the build stamp and push
- **Verified:** 351 → 355 tests, all passing. Live build confirmed serving `2026-09-24-2` from
  Pages.

### 2026-09-24 — Filing money forward turns the month over on the day it arrived

- **Changed:** Ported `incomeRow()` and `startRow()` from the personal repo, minus the per-month
  currency row this fork does not have, and with `en-GB` dates. Choosing "the next one" under
  Counts toward auto-ticks the start-the-month checkbox; the income row rebuilds on any date or
  month change instead of once when Received is tapped; the label names the month being started.
  The same correction runs from the edit sheet in `moveSave`. Build `2026-09-24-1`. `README.md`
  gained a "What's new" section and a maintainer section.
- **Why:** Money logged before the allowance day sat invisible until that day came round, because
  filing it forward did not move the month boundary.
- **Files:** `index.html`, `test.js`, `README.md`. Commit `70d4a49`
- **Revert:** `git revert 70d4a49`, then bump and push
- **Verified:** 329 → 351 tests, all passing. New sections 70, 70b, 70c, 71, 71b.

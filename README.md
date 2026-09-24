# Tracker

A simple, private spending tracker that lives on your phone. No account, no ads, nobody but you
sees your numbers. This guide walks through setting it up and using it day to day — no technical
background needed.

## What it actually does

Every month, you tell it what you got paid. From that, it works out:

- how much you can safely spend **today** without running out before your next payday
- how much is left for **this week**
- what's already spoken for (rent, subscriptions — the stuff that isn't really "spending money")
- how you're tracking against a savings goal, if you've set one

It doesn't connect to your bank. You type in what you spend as you go (or catch up every few
days) — it's a running total, not automatic transaction importing.

## Getting it on your phone

The link opens a normal web page, but it's built to be installed like an app:

**iPhone (Safari):** open the link → tap the Share icon (square with an arrow) → **Add to Home
Screen**. It'll appear as an icon with the rest of your apps and opens full-screen, no browser bar.

**Android (Chrome):** open the link → tap the **⋮** menu → **Add to Home screen** (or Chrome may
offer to do this automatically with a banner).

You can also just keep it as a browser tab if you'd rather — everything works the same either way.

## Setting it up for the first time

The first thing it asks for is what day of the month you get paid — your **allowance day**. This
matters because the app doesn't think in calendar months; it thinks in cycles that run from one
payday to the next. If you're paid on the 25th, your "month" runs 25th to 24th, not 1st to 31st.

You can change this later in **Settings → Month** if your pay date shifts, or override a single
month by hand (useful if you get paid a few days early or late one time).

## Logging money

Tap **Add** at the bottom. There are three kinds of entry:

- **Spent** — anything that leaves your pocket. Pick a category (Groceries, Uber Eats, Rent,
  whatever fits) and type the amount.
- **Received** — money coming in. This is how you tell it a new month has started, by logging
  your pay.
- **Refund** — money coming back for something you'd already logged as spent.

There's also **Read a receipt**: open the photo of a receipt in your phone's own Photos app,
select and copy the text (iPhones do this natively — press and hold on the text in a photo), then
paste it in. It pulls out the total, date and a guess at the category so you don't have to type
them by hand. Nothing about the photo itself is ever stored — just the numbers you confirm.

## Reading the dashboard

The two tabs at the top, **This week** and **This month**, are two views onto the same money —
switch between whichever number you find more useful day to day.

- The big number is what's safe to spend **right now** without borrowing from tomorrow.
- Below it: what's been spent so far, and how many days are left in that stretch.
- The bar chart shows spending day by day, with a dashed line marking your daily pace — if a bar
  is above the line, that day went over.
- **Rent & the like** only shows up in the month view, not the week one, since things like rent
  aren't a weekly cost — you'd only ever pay them once a cycle.

Categories you mark as "kept out" (Settings → Rules) — rent and levies by default — don't count
against your daily spending pace at all. They're money that's already spoken for, shown separately
so they don't make it look like you're overspending on everyday things.

## Bills and recurring income

If you've got the same subscriptions or bills every month, add them once under **Settings →
Bills**. The app then nudges you when one hasn't been logged yet this cycle, so you don't forget
or double-pay from memory. Same idea for **recurring income** under Settings → Month, if you're
paid the same amount every time.

## Savings

Tap **Put away this month** on the dashboard to move money into savings — it comes out of your
spendable pot and won't count toward next month either. **Take some out** reverses it.

## Finding past entries

**Search** at the bottom searches everything you've ever logged — by category, note, amount, or
even a month name. **Months** shows a running history of every past cycle, with a CSV or PDF
export available for each one if you want a proper statement.

## Backing up your data

Everything lives only on this one phone until you back it up — if you lose the phone or clear your
browser data, it's gone. **Settings → Data** lets you connect a free GitHub account as a private,
personal backup:

1. Create a GitHub account if you don't have one already (free, at github.com).
2. Create a **Personal Access Token** with just the "gist" permission (GitHub → Settings →
   Developer settings → Personal access tokens).
3. Paste that token into Settings → Data in the app.

Your data is then saved to a **private** Gist under your own GitHub account — nobody else can see
it, and it lets you open the same tracker on a second device (like an iPad) and stay in sync.
This step is entirely optional; the app works fully without it, just without a safety net.

If you'd rather not set up GitHub at all, **Download a backup** in the same Settings tab saves a
plain file you can keep anywhere (email it to yourself, save it to a cloud drive), and **Restore
from backup** loads one back in.

## If a friend wants their own copy

This is safe to share — everyone's data stays private to their own phone (and their own GitHub
backup, if they set one up). Nothing is shared between people using the same link; there's no
shared account or server in the middle.

## What's new

### 24 September 2026

**Telling it a new month has started now works properly when you're paid early or late.**

When you log your pay, the **Counts toward** dropdown lets you say which month the money belongs
to. Previously, choosing "the next one" filed the money forward but left the month itself running
until your usual allowance day — so if you were paid a couple of days early, the money vanished
from the dashboard until that day came round, and the app kept spreading your old month's
leftovers over days you'd already been paid for.

Now, choosing the next month **starts that month on the day the money actually arrived**. The
dashboard updates straight away and the previous month closes the day before.

- The checkbox under the dropdown now names the month it's about to start and the day it'll start
  it — for example *"Start October from 23 Sept instead of the 25th"* — and keeps up if you change
  the date. It used to show whatever date was there when you first tapped **Received**.
- If you'd rather the money waited for your usual allowance day, just untick that box.
- Already logged your pay the old way and it isn't showing? Tap the entry, then **Save changes** —
  that's enough to move the month onto the right day.

This is handy if your pay date moves around rather than landing on the same day each month: just
log it whenever it arrives and let the app set the month from that.

### 24 September 2026 (later the same day)

**It now fixes itself when you open the app.**

The change above only applied to money logged from then on — anything already recorded the old
way still needed correcting by hand. It no longer does. When the app opens, it checks for pay
that was recorded against a later month than the date it arrived on, and starts that month on
the right day for you. Nothing to tap.

If you deliberately told it *not* to start a new month (by unticking the box), that choice is
remembered and left alone. Closed months are never rearranged.

---

## For whoever maintains this

This is a fork of a personal tracker, customised for one person (GBP throughout, no Investments
tab, a trimmed category list). It's a single `index.html` — no framework, no build step, no
backend.

- `node test.js` runs the suite (**355 checks**) — it boots the real `index.html` in jsdom with a
  frozen clock. Run it before and after every change.
- To deploy: edit `index.html`, **bump the `slip-build` meta tag** on line 7, then push. GitHub
  Pages redeploys in about a minute and the running app offers the update.
- `TRACKER-PROJECT.md` has the money model, the cycle rules, and a list of bugs that must not be
  reintroduced.
- Changes here are ported from the personal repo (`fayzalad/Spending-Tracker`). The two have
  independent history — a commit in one never touches the other.

# BTC Sentinel — Cloud Bot (runs without your computer on)

This version runs **in the cloud on GitHub**, on a schedule, so your computer can be
off and the bot keeps paper-trading. It's still **pretend money** — nothing real is
ever bought or sold.

## What's in this folder

- **bot.js** — the bot itself. Runs once each time it's triggered, then remembers
  everything in `state.json`.
- **.github/workflows/bot.yml** — tells GitHub to run `bot.js` every 5 minutes.
- **state.json** — the bot's memory (cash, holdings, trade history). Starts at $10,000.
- **dashboard.html** — a webpage you can open any time to SEE what the bot has done.
  You don't need it open for the bot to run; it just reads the bot's saved results.

---

## One-time setup (about 10 minutes, no payment, no card)

You'll need a free GitHub account: https://github.com/signup

### 1. Make a new repository
- Go to https://github.com/new
- Name it something like `btc-sentinel`
- Set it to **Public** (public repos get unlimited free minutes)
- Click **Create repository**

### 2. Upload these files
- On your new repo's page, click **Add file ▸ Upload files**
- Drag in `bot.js`, `state.json`, and `dashboard.html`
- For the workflow file, it's easiest to create it by hand:
  - Click **Add file ▸ Create new file**
  - In the name box type exactly: `.github/workflows/bot.yml`
    (the slashes create the folders automatically)
  - Paste in the contents of the `bot.yml` from this folder
- Click **Commit changes**

### 3. Turn on Actions
- Click the **Actions** tab at the top of your repo
- If it asks, click the green button to enable workflows
- You should see **"BTC Sentinel Bot"** listed on the left

### 4. Give the bot permission to save its progress
- Go to **Settings ▸ Actions ▸ General**
- Scroll to **Workflow permissions**
- Select **Read and write permissions**, then **Save**
  (this lets the bot update `state.json` after each run)

### 5. Test it right now
- Go back to the **Actions** tab
- Click **BTC Sentinel Bot**, then **Run workflow ▸ Run workflow**
- After ~20 seconds it'll show a green check. Click into the run to read the log —
  you'll see the current BTC price and the bot's decision.
- From now on it runs **every 5 minutes on its own**.

### 6. Watch it with the dashboard
- Your `state.json` has a public web address. It looks like:
  `https://raw.githubusercontent.com/YOUR-USERNAME/btc-sentinel/main/state.json`
  (swap in your username; if your default branch is `master`, use that instead of `main`)
- Open `dashboard.html` (double-click it, or host it on Netlify like your other apps),
  paste that URL in the box, and hit **LOAD**. Turn on auto-refresh and leave the tab
  open whenever you want to watch — but the bot runs with or without it.

---

## Things to know (the honest part)

- **It checks every 5 minutes, not every few seconds.** That's GitHub's fastest free
  schedule. A slow-moving average bot is fine with that, but it means fewer trades and
  slower reactions than the live in-browser version.
- **Runs can be a few minutes late.** GitHub queues scheduled jobs globally and doesn't
  promise exact timing. Not a problem for this; just don't expect clockwork.
- **GitHub pauses schedules after 60 days of no repo activity.** Since the bot commits
  `state.json` on most runs, it keeps itself active. If you ever see it stop, open the
  Actions tab and hit Run workflow once to wake it up.
- **It's still paper money.** This deliberately does not connect to Robinhood, Coinbase,
  or any real account. Letting it run for a few weeks and checking the win rate is the
  whole point — that number tells you whether the strategy would have made or lost real
  money, at no risk.

## Want stocks instead of BTC later?
Stocks need a Finnhub API key (stored as a repo **Secret**, not in the file) and only
trade 9:30am–4pm ET on weekdays, so the bot would sit idle nights and weekends. Bitcoin
trades 24/7 and needs no key, which is why this always-on version uses BTC. Ask and I
can build the stock version of this same setup.

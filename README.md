# ASOT Player

A stateful browser player for **Armin van Buuren – A State of Trance episodes 001–499**, streamed directly from the [Internet Archive](https://archive.org/details/Armin_van_Buuren_A_State_of_Trance_001-499).

**Live version: [jrajsky.github.io/asot-player](https://jrajsky.github.io/asot-player/)**

---

## Why this exists

The Internet Archive hosts the full ASOT 001–499 collection for free, but its built-in player has no memory — every time you come back it has forgotten where you stopped, which episodes you finished, and what you were in the middle of. If you listen to these 1–2 hour episodes across multiple sessions, you are constantly hunting for your place manually.

This player solves that. It remembers everything, entirely in your browser — no account, no server, no installation.

---

## Features

- Streams all 499 episodes directly from archive.org
- **Remembers playback position** for every episode — resume exactly where you left off
- **Marks episodes as completed** once you reach 97% of the duration
- Episodes played for less than 20 seconds are left as unplayed (skipping past the start of an episode by accident does not mark it as in-progress)
- All state is stored in **browser localStorage** — serverless, private, persistent across sessions
- Restores your last scroll position, search query, and filter when you reopen the page
- **Optional cross-device sync** via a private GitHub Gist — progress is shared across your laptop, phone, or any other device

---

## Episode states

| Indicator | Meaning |
|-----------|---------|
| `·` grey | Not yet played |
| `▶` amber + progress bar | In progress — shows timestamp where you stopped |
| `✓` green | Completed (≥ 97% listened) |
| `♫` purple | Currently playing |

The header stats bar shows counts for each state. Click a count (e.g. **2 in progress**) to jump to the first matching episode in the list.

---

## Controls

### Clicking the list

| Action | Result |
|--------|--------|
| Click any episode | Start playing; resumes from saved position if in progress |

### Player bar

| Control | Action |
|---------|--------|
| Play / Pause button | Toggle playback |
| ⏮ Previous | Jump to previous episode |
| ⏭ Next | Jump to next episode |
| ⏪ −30 s | Skip back 30 seconds |
| ⏩ +30 s | Skip forward 30 seconds |
| Seek bar | Click or drag to jump to any position |
| Volume slider | Adjust volume |

### Keyboard shortcuts

| Key | Action |
|-----|--------|
| `Space` | Play / Pause |
| `←` / `→` | Seek −10 s / +10 s |
| `↑` / `↓` | Volume up / down |
| `N` | Next episode |
| `P` | Previous episode |

### Navigation & filtering

| Element | Action |
|---------|--------|
| **1–100 / 101–200 / …** buttons | Jump to the first episode of that hundred |
| Search box | Filter list by episode title or number |
| Filter dropdown | Show all / unplayed / in progress / completed episodes |
| Stats bar counts | Click to scroll to the first episode with that state |

---

## Cross-device sync via GitHub Gist

Progress can be synced across devices (laptop, phone, etc.) using a private GitHub Gist as storage. No server or account beyond GitHub is required.

### Step 1 — Create a GitHub Personal Access Token

1. Go to **github.com** → click your profile picture (top right) → **Settings**
2. Scroll down in the left sidebar → **Developer settings** (very bottom)
3. **Personal access tokens** → **Tokens (classic)**
4. Click **Generate new token** → **Generate new token (classic)**
5. Give it a name, e.g. `asot-player`
6. Set expiration to **No expiration** (or whatever you prefer)
7. Under **Select scopes**, tick only **`gist`** — nothing else needed
8. Click **Generate token**
9. **Copy the token now** — GitHub shows it only once

### Step 2 — Enter it in the player (first device)

1. Open the player
2. Click the **↻ sync icon** in the top-right of the header
3. Paste the token into the **GitHub token** field
4. Leave the Gist ID field empty
5. Click **Sync now**

The player will create a new private Gist and auto-fill the Gist ID field. **Copy that Gist ID** — you'll need it on other devices.

### Step 3 — Set up a second device

1. Open the player on the second device
2. Click the **↻ sync icon**
3. Paste the **same token**
4. Paste the **Gist ID** from Step 2
5. Click **Sync now**

Done — both devices now share the same progress.

### Day-to-day behaviour (nothing to do)

- **Opening the player** → automatically pulls from the Gist
- **Pausing or closing the tab** → automatically pushes to the Gist
- The dot on the ↻ button tells you the status: pulsing amber = syncing, green = ok, red = error

---

## Privacy & security

- The GitHub token is stored only in your browser's `localStorage` — it is never sent anywhere except directly to `api.github.com`
- The Gist is created as **private** — it is not publicly visible
- The token only has `gist` scope, so it cannot access your repositories, account settings, or anything else
- If you ever want to stop syncing, click **Forget token** in the sync panel — this removes the token and Gist ID from the browser immediately

---

## Frequently asked questions

**Does it work offline?**
Playback requires an internet connection to stream from archive.org. Your progress is stored locally and always available offline — it just won't sync until you're back online.

**What if I lose the token?**
Generate a new one on GitHub following Step 1 again. Paste the new token together with the existing Gist ID — the Gist and all your progress data are still there.

**What if I clear browser storage?**
Local progress is lost, but if you had Gist sync set up the data is safe on GitHub. Re-enter your token and Gist ID, click **Sync now**, and everything is restored.

**Can I use it on multiple browsers on the same device?**
Yes — set up the same token and Gist ID in each browser.

**Will episodes played briefly (< 20 s) show as in progress?**
No. Playback position is only saved after 20 seconds have elapsed. Skipping past an episode by accident leaves it marked as unplayed.

---

## Technical notes

- Single static HTML file (`index.html`) — no build step, no dependencies, no server required
- Audio streamed on demand from `archive.org/download/`; nothing is downloaded to your device
- Progress data stored under the localStorage key `asot_player_v1`
- View state (scroll position, filter, search) stored under `asot_view_v1`
- Gist sync config (token, Gist ID) stored under `asot_gist_v1`
- Position is saved every 4 seconds during playback, on pause, and when the tab is closed
- Sync pushes on pause and tab close only — not on every periodic save — to stay well within GitHub API rate limits
- Conflict resolution: per episode, the more advanced state always wins (`completed` > higher position > lower position)

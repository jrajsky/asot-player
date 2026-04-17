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

## Technical notes

- Single static HTML file — no build step, no dependencies, no server required
- Audio streamed on demand from `archive.org/download/`; nothing is downloaded to your device
- Progress data stored under the localStorage key `asot_player_v1`
- View state (scroll position, filter, search) stored under `asot_view_v1`
- Position is saved every 4 seconds during playback, on pause, and when the tab is closed

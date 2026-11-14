# 1-2-3 Bowling

A mobile-first, single-file web app for tracking the **1-2-3** bowling mini-game live across multiple players' phones or offline in local mode, complete with per-game `±` settlement, read-only game locking, and lifetime trend analytics.

## Scoring System

| Event | Points | Notes |
| :--- | :---: | :--- |
| **Strike** | **+1 PT** | Tapped live from each player's phone card |
| **Pin 10 Leave** | **+2 PT** | Tapped live whenever Pin 10 is left standing |
| **Highest Pinfall** | **+3 PT** | Automatically awarded at the end of each game to the highest manual game score (0–300); ties award +3 PT to all tied leaders |

## Features

### Local-Only Mode (Signed Out / Offline)
- **Zero Cloud Sync:** When not signed in with Google, the app runs strictly in your browser's `localStorage` with no Firebase syncing.
- **New Local Game:** Tap **New Local Game** anytime to reset to a blank slate (no players, Game 1, today's date), then manually add players, track multi-game scores, and view `±` settlement.
- **Ephemeral Browser Storage:** **Save Session to Lifetime** is hidden in Local Mode, and local data is cleared when browser cache/storage is cleared.

### Google Sign-In & Cloud Room Sync
- **Google Sign-In + Redirect Fallback:** Players sign in with Google (or **Use Redirect** if a desktop/mobile browser blocks popups) to tie their account (`uid`) to a fixed bowler name stored in Firebase (`/users/<uid>/playerName`), editable anytime.
- **Room-Gated Cloud Sync:** Signed-in users see a **Join a Room** prompt until connected to a room code. Clicking **New Code** starts with a clean slate (today's date, Game 1, empty roster).
- **Auto-Join & Conflict-Free Sync:** Connecting to a room or opening a `?room=CODE` invite link automatically adds your player and selects your phone view. Strike and Pin 10 taps use atomic Firebase `runTransaction()` updates, and Final Game Scores write per-player `pinfall` fields so simultaneous taps never collide.
- **Exclusive Phone Card Controls:** All edits (Strikes, Pin 10s, and Final Game Score) happen on the **[Name]'s Phone** card; the summary scoreboard is read-only to prevent misclicks, and player pills display names only.
- **Mid-Session Joins & Drop-Offs:** Adding or removing a player only affects the active and future games—completed earlier games remain untouched.
- **Score Validation & Locked Previous Games:** Advancing to **Next Game →** requires every player in the active game to have a Final Game Score entered and prompts for confirmation. Once a new game starts, earlier game pills remain clickable to inspect inputs in **read-only (`LOCKED`)** mode.
- **Creator-Only Final Commit:** Only the room creator (`creatorId === uid`) sees and can click **Save Session to Lifetime & Close Room** at the bottom of the Live Session tab (with confirmation). Any blank, unplayed trailing game is automatically excluded from the saved count.

### ± Settlement & Head-to-Head Matrix
- **Per-Game Zero-Sum `Net ±`:** Each game is settled independently among the players in that game (`(N × PlayerPts) − TotalGroupPts`), and session cumulative `Net ±` sums each player's per-game `±`.
- **Accurate Head-to-Head Matrix:** In **Full Session** view, each `(A vs B)` cell sums `(Pts_A − Pts_B)` only across games where both players participated.
- **Scope & Copy:** Toggle between **Full Session** or any individual game, and copy a formatted summary to your clipboard.

### Lifetime Leaderboard, Trends & Developer Controls
- **Lifetime Leaderboard & Charts:** Tracks Games, Total Points, Avg Points, Avg Pinfall, High Game, Strikes, Pin 10s, and Lifetime `±`, with interactive Chart.js trends across 5 metrics.
- **Developer-Only Admin & Room Cleanup (`/developerUid`):** Only the configured developer UID (displayed in your signed-in header for quick verification) can **Export JSON**, **Import JSON**, **Clear All**, **Delete** saved sessions, or view/join/delete rooms in the **Developer: Active Cloud Rooms** panel.

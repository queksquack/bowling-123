# 1-2-3 Bowling

A mobile-first, single-file web app for tracking the **1-2-3** bowling mini-game live across multiple players' phones, complete with pairwise `±` settlement and lifetime trend analytics.

## Scoring System

| Event | Points | Notes |
| :--- | :---: | :--- |
| **Strike** | **+1 PT** | Tapped live frame-by-frame from each player's phone |
| **Pin 10 Leave** | **+2 PT** | Tapped live whenever Pin 10 is left standing |
| **Highest Pinfall** | **+3 PT** | Automatically awarded at the end of each game to the highest manual game score (0–300); ties award +3 PT to all tied leaders |

## Features

### Live Multi-Phone Sessions
- **Google Sign-In & Fixed Bowler Identity:** Players sign in with Google to tie their account (`uid`) to a permanent bowler name stored in Firebase (`/users/<uid>/playerName`), editable anytime.
- **Live Room Codes & Invite Links:** Create or join a room code and share a `?room=CODE` invite link. Connecting automatically adds your player to the room and selects your phone view.
- **Conflict-Free Real-Time Sync:** Strike and Pin 10 taps use atomic Firebase `runTransaction()` updates, and manual pinfall scores write per-player fields so simultaneous taps from different phones never overwrite each other.
- **Multi-Game Sessions:** Sessions default to today's date (editable) and support any number of games (`Game 1`, `Game 2`, `Game 3`, ...) under one roster, with live **Session Cumulative Standings**.
- **Creator-Only Room Closing:** Only the room creator sees and can click **Save Session to Lifetime & Close Room** at the bottom of the Live Session tab.

### ± Settlement & Head-to-Head Matrix
- **Zero-Sum Pairwise `Net ±`:** Calculates each player's net point difference against every other player in the game (`(N × PlayerPts) − TotalGroupPts`).
- **Head-to-Head Matrix:** Displays exact `±` differences between every pair of players.
- **Flexible Scope & Copy:** Toggle between **Full Session** cumulative `±` or any individual game, and copy a formatted text summary to your clipboard.

### Lifetime Leaderboard & Trends
- **Persistent History:** Saved sessions and game-by-game breakdowns are stored in Firebase (and cached locally in `localStorage`).
- **Lifetime Leaderboard:** Tracks Games Played, Total Points, Average Points, Average Pinfall, High Game, Total Strikes, Total Pin 10s, and Cumulative Lifetime `±`.
- **Interactive Trend Charts:** Powered by Chart.js to plot **Total 1-2-3 Points**, **Cumulative Net ±**, **Bowling Game Score (Pinfall)**, **Strikes**, and **Pin 10 Leaves** over time.
- **JSON Backup:** Export or import your full session history as a `.json` file.

## Tech Stack
- **Frontend:** Single-file HTML + [Tailwind CSS](https://tailwindcss.com/) + [Chart.js](https://www.chartjs.org/)
- **Backend / Sync:** [Firebase Authentication](https://firebase.google.com/docs/auth) (Google Sign-In) & [Firebase Realtime Database](https://firebase.google.com/docs/database) (`asia-southeast1`)
- **Hosting:** Static hosting on [GitHub Pages](https://pages.github.com/)


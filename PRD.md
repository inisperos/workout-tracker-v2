# Workout Tracker — Product Requirements

**Target audience:** Me
**Hosted on:** GitHub Pages

## Current status

- The MVP, Stats, and Calendar phases are complete.
- The current styling is accepted as the baseline and should remain unchanged.
- Future changes should be limited to bug fixes unless a new feature or design change is explicitly requested.

## Original objective

Be able to see an aggregate of my workouts and allow myself a visual cue of the frequency and volume per muscle group per week. I also want to be able to view my PR and past sessions while I record the previous one.

## Core objectives

- **Science-based lifting simplicity** — Allow a user to plainly see if they are achieving 2x frequency and the sets they would like to aim for per muscle group, per week.
- **Notes app replacement** — Provide an efficient, minimalist, single-page dashboard tracking interface to completely replace fragmented notes-app tracking for a structured strength training routine.
- **Seamless progression visibility** — Surface volume tracking, movement history, and personal records directly within the workout flow so historical data actively drives current session performance.
- **Time-insensitive logging** — Allow flexible session logging by providing an explicit date-override toggle ("Today" vs "Other"), ensuring past or delayed workout logs accurately backdate historical progress.
- **Comprehensive chronological history** — Provide a visual, real-time rolling 12-month interactive calendar view grouped by day that flags completed lifting sessions and displays detailed drill-down exercise data on tap.

## Timeline / phases

Each screen must have all its listed features to be considered "done" before moving to the next phase.

- **MVP** — Main Dashboard, Track Workout, and View History → Screens A, B, C, D, E
- **Phase 2** — See Stats → Screens F, G **complete**
- **Phase 3** — Add calendar → Screen H **complete**
- **Phase 4** — GitHub-backed persistence for workout data
- **Phase 5** — Styling

### Persistence and sync requirements

- The app is hosted on GitHub Pages, so it cannot write files to the repository directly without an authenticated GitHub API request.
- Store the workout database in `data/workout-data.json` on the `main` branch of `inisperos/workout-tracker-v2`.
- On startup, fetch the JSON file from GitHub and reload sessions, exercises, and weekly targets so the current sets are visible on every phone.
- Keep a browser-local cache as an offline fallback. Every workout, exercise, target, or deletion change updates the cache immediately and commits the JSON file to GitHub when sync is configured.
- Provide a Sync action that accepts a GitHub fine-grained personal access token with repository Contents read/write permission. Store that token only in the phone's browser storage; never include it in the workout JSON or committed source.
- If no remote file exists yet, create it on the first authenticated sync. If GitHub is unavailable, keep showing the cached data and report the sync failure in the UI.
- Because this repository is used with GitHub Pages, the committed workout JSON is publicly readable. Do not store sensitive personal information in workout names, locations, or notes.
- The JSON file is the shared source of truth across devices. A future backend can replace the GitHub Contents API if multi-user accounts, conflict resolution, or stronger secret handling are needed.

## Screen architecture

### Screen A: Main Dashboard
- Click to Stats (Screen F)
- Click to Calendar (Screen H)
- Log workout (Screen B)
- Click on muscle group to see exercises for the week
- Manage exercises: edit an exercise name, change its muscle group, or delete it with confirmation

### Screen B: Log Workout Specification
- Back to Main Dashboard (Screen A)
- Choose Date between Today and Other (user can pick a date if Other is pressed)
- Choose Location between Planet Fitness and Other (user can type a location if Other is pressed)
- Choose Type between Push, Pull, Upper, and Legs
- Click Next → Active Session (Screen C)

### Screen C: Active Session
- Done → Main Dashboard (Screen A)
- Click Add New Workout → Screen E
- If past sessions exist, clicking the button next to an exercise → Screen D
- Show running weekly set totals grouped by muscle group
- Right-swipe an exercise to cue it to the top of the list
- Start a two-minute rest timer when Add set is pressed
- Weight is optional when logging a set; reps remain required
- Barbell and dumbbell exercise PR/history cues are independent of location

### Screen D: Log Exercise Specification
- User can type weight x reps
- If Add Set is pressed, another text box appears
- Back and Next → Active Session (Screen C)
- Barbell bench press shows its best rep set and estimated 1RM

### Home workout logs and session editing
- The dashboard shows workout logs for the selected week, including when browsing previous weeks.
- A submitted session can be reopened to edit its sets and location.

### Screen E: Add New Workout
- User can type name of the workout
- User can select which muscle group it targets
- Back and Next → Active Session (Screen C)
- Supported muscle groups include Adductors

### Screen F: Stats Dashboard
- **Workout View (F1)**
  - Show gym days by workout type in a pie graph
  - Show gym days by month in a bar graph
- **General View (F2)**
  - Click on a muscle group to see what workouts a user has done and their PR
  - Click a button for that exercise to show its history (Screen G)
- **Overall actions**
  - Click back to dashboard (Screen A)
  - Click time window to show history
  - Click between Workout View (F1) and General View (F2)

### Screen G: Exercise History
- Click back to Workout View (F1)

### Screen H: Calendar View
- Click back to dashboard (Screen A)
- Click between months
- Click a date to show the exercise done

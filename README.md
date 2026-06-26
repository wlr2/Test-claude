# 📅 Planner — Calendar & Todos (single-file, offline)

A touch-friendly personal planner: a month calendar and a todo list, with
optional voice commands. It's a single `index.html` with all CSS and
JavaScript inline — **no frameworks, no build step, no external scripts or
CDNs.** Everything except voice works fully offline. Data is saved on the
device in `localStorage`.

## What it does

- **Calendar** — month grid with prev/next month and a **Today** button.
  Today and the selected day are highlighted. Tap a date to see that day's
  events; **+ Add event** adds a title + optional time. Dates with events or
  due todos show a small dot.
- **Todos** — add, complete (checkbox), delete. Optional due date; a due todo
  shows a dot on the calendar. Active and completed are listed separately and
  the completion state persists.
- **Persistence** — events and todos are stored as JSON in `localStorage`.
  Every read/write is wrapped in `try/catch`, so a quota or parse error never
  crashes the app. Use **Reset data** to clear everything.
- **Voice (optional)** — see below. The app is fully usable by touch/typing
  without it.

## Get it onto an iPad and open it in Safari

The app is one file, so you just need to get `index.html` onto the iPad and
open it in **Safari**. Any of these work:

1. **iCloud Drive / Files** — copy `index.html` into Files, tap it, and choose
   to open in Safari (or long-press → Share → open in Safari).
2. **AirDrop** — AirDrop `index.html` from a Mac; accept and open in Safari.
3. **Email/Messages to yourself** — attach `index.html`, then on the iPad save
   it to Files and open it in Safari.
4. **Local web server** (if you have one) — serve the folder and browse to it.

To keep it handy, open it in Safari and use **Share → Add to Home Screen** for
a quick launcher.

> ⚠️ **Voice note:** the **Home-Screen / PWA** version of the page does **not**
> support the Web Speech API. For voice, open the file in **Safari proper**.
> Everything else (calendar, todos, persistence) works fine in either, and
> offline.

## Voice commands

Voice is **progressive enhancement**. If the Web Speech API isn't available,
the mic button is hidden and a short note is shown — the app stays fully
usable by touch.

**Voice requires:** Safari (not the Home-Screen/PWA version), microphone
permission (you'll be prompted on first use), and an **internet connection**
(Safari's speech recognition is server-based). Everything else works offline.

Tap **🎤 Voice**, speak, then pause — the app finalizes after about 1.2s of
silence (iOS end-of-speech events are unreliable, so a timer is used). The live
transcript is shown while you speak, and each action is confirmed on screen and
optionally read back.

Recognized commands (case-insensitive):

| Say… | Does |
| --- | --- |
| `add todo <text>` / `add task <text>` | Adds a todo |
| `add event <text> [today\|tomorrow\|<weekday>] [at <time>]` | Adds an event on the parsed date (default today). Times like `3pm`, `3:30 pm`, `15:30` |
| `schedule <text> …` | Same as add event |
| `complete <text>` / `done <text>` | Marks a matching active todo complete |
| `what's on today` | Speaks today's events |
| `read my todos` | Speaks your active todos |

Anything else shows "Didn't catch a command" without breaking anything. A
denied mic permission or missing internet shows a message and you can keep
typing.

## Reset data

Tap **Reset data** at the bottom (it confirms first). This clears all events
and todos from this device's `localStorage`.

## Notes / design choices

- Dates are only ever built from explicit numeric parts or strict ISO
  `YYYY-MM-DD` strings — never `new Date("M/D/YYYY")`, which is invalid in
  Safari.
- The DOM is updated incrementally (only the affected list/grid re-renders) and
  speech/timers are cleaned up on `pagehide` to avoid leaks in iPad Safari.
- Add dialogs use the browser's built-in `prompt()` to stay dependency-free.
- Spoken weekdays resolve to the **next** occurrence (including today).

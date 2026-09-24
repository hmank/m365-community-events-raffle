# M365 Community Events Raffle

A single-file, dependency-free raffle web app for running live prize draws at
Microsoft 365 community events. It is designed to be projected on a screen at the
front of a room: huge winner name, high-contrast dark theme, confetti, and a
fullscreen button.

**Live demo:** https://hmank.github.io/m365-community-events-raffle/

Everything is one `index.html` — no frameworks, no build step, no server, and no
external dependencies.

---

## Features

- **Paste-and-go entrants** – paste names one per line. Blank lines are ignored and
  duplicates are removed (case-insensitive), with a count of how many were loaded and
  how many duplicates were skipped.
- **Flexible prizes** – optionally set a number of prizes and/or a list of prize
  names (awarded in order):
  - A number sets the total; unnamed slots become "Prize #n".
  - Prize names alone set the total.
  - Neither = unlimited draws.
- **Manual, one-per-click draws** – a big **Draw winner** button (also triggered by
  the **Space bar**) runs a 5-second countdown with names shuffling on screen, then
  reveals one winner with confetti. Nothing draws automatically.
- **Fair selection** – winners are chosen with `crypto.getRandomValues` (unbiased),
  decided at the start of the countdown.
- **Tracking** – winners are removed from the pool, a live winners list (order, name,
  prize) and eligible count are shown, and progress reads "Prizes left: X of Y".
- **"Not here"** – mark a revealed winner absent; the prize goes back and you click
  **Draw winner** again to redraw it. Absent people are listed separately and can be
  returned to the pool.
- **Return by mistake** – put any winner or absent person back into the pool (their
  prize returns too).
- **Copy winners / Reset all** – export the winners list to the clipboard, or clear
  everything (with confirmation).
- **Nothing is saved** – all data lives in memory only. Refreshing or closing the tab
  clears everything, and a warning prompts you before you leave with data loaded.
- **Accessible** – semantic HTML, fully keyboard usable, respects
  `prefers-reduced-motion`.
- **Multi-language ready** – 13 languages are built in (English, Danish, Swedish,
  Norwegian, Finnish, German, Dutch, French, Spanish, Portuguese, Italian, Polish, and
  Arabic with right-to-left layout). The picker is **hidden by default** — see below to
  turn it on.

---

## How to use it

1. Open the live demo (or your own copy) in a browser.
2. Paste entrant names into **Entrants**, one per line.
3. (Optional) Set a **Number of prizes** and/or type **Prize names**, one per line.
4. Click **Load list**.
5. Click **Draw winner** (or press **Space**) once per prize. Watch the countdown and
   the confetti reveal.
6. If a winner isn't in the room, click **Not here**, then **Draw winner** again to
   redraw that prize.
7. Use **Copy winners** to save the results, **Fullscreen** for projecting, and
   **Reset all** to start over.

> Tip: click **Fullscreen** before projecting for the largest, most readable winner
> name.

---

## Run it locally

No install or build is required.

- **Easiest:** download `index.html` and double-click it to open in your browser.
- **Recommended (avoids browser file restrictions):** serve the folder and open it
  over `http://`. For example, with Python installed:

  ```bash
  # from the folder containing index.html
  python -m http.server 8000
  # then open http://localhost:8000/index.html
  ```

---

## Enabling the language dropdown

The multi-language dropdown ships **disabled**. To turn it on, edit `index.html` and
find the **Language settings** block near the top of the `<script>` section:

```js
// ── Language settings — edit these to configure ──────────────
const SHOW_LANGUAGE_PICKER = false;  // true = show the language dropdown
const AUTO_DETECT_LANGUAGE = true;   // false = always start in English
const DEFAULT_LANG = 'en';
const ENABLED_LANGUAGES = ['en','da','sv','nb','fi','de','nl','fr','es','pt','it','pl','ar'];
```

- **Show the dropdown:** set `SHOW_LANGUAGE_PICKER` to `true`. That's the only change
  needed — the picker appears in the header with all enabled languages, and the app
  auto-selects the visitor's browser language when possible.
- **Always start in English** (even with the picker shown): set
  `AUTO_DETECT_LANGUAGE = false`.
- **Limit which languages appear:** remove codes from `ENABLED_LANGUAGES` (keep `'en'`
  as the fallback).
- **Add a new language:** add an entry to the `LANG_NAMES` map and a matching
  dictionary in the `I18N` object, then add its code to `ENABLED_LANGUAGES`. Any missing
  text automatically falls back to English.

Save the file and reload the page.

---

## Publish your own copy on GitHub Pages

1. Create a new GitHub repository and add `index.html` at the root.
2. Push it:

   ```bash
   git init -b main
   git add index.html README.md
   git commit -m "M365 Community Events Raffle"
   git remote add origin https://github.com/<your-user>/<your-repo>.git
   git push -u origin main
   ```

3. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a
   branch**, choose **main** / **/ (root)**, and Save.
4. Wait a minute, then open `https://<your-user>.github.io/<your-repo>/`.

---

## Privacy

There is no analytics, no tracking, no cookies, and no network calls. All entrant
names, prizes, and winners exist only in the browser tab's memory and are gone the
moment you refresh or close it.

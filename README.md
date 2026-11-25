# VKT ReadPos — 读位

English | [中文](languages/README_zh.md) | [日本語](languages/README_ja.md) | [Deutsch](languages/README_de.md) | [Español](languages/README_es.md) | [Français](languages/README_fr.md)

Reading position memory and page scenes for Chrome, Edge and Firefox. Every long page remembers where you stopped; a page that holds state — a dashboard, a half-filled form, a ticket you were writing — can be saved as a *scene* and brought back whole.

Install: https://annmax1983.com/extensions/readpos

## Features

### Reading positions (free)

- **Per page, automatically** — the scroll offset is recorded 600 ms after you stop scrolling, and again when the tab is hidden or closed.
- **The continue pill** — reopening a page shows *Continue where you left off · 62%* in the bottom-right corner. One click smooth-scrolls back. It never scrolls on its own.
- **Quiet** — the pill fades after a few seconds; moving the pointer into the bottom-right corner brings it back. Nothing else is drawn on the page.
- **Short pages stay quiet** — a position below the trigger distance (600 px by default) is not offered.
- **Pause a site, clear a page** — one click each, in the popup.
- **20 pages of history** — the popup lists recent positions with the percentage reached; click a row to reopen it.
- **SPA routes counted separately** — `/app/inbox` and `/app/settings` are different pages. Hash routers (`#/route`) too.
- **Lazy pages** — if the saved offset is beyond the current document height, the restore retries twice and then gives up quietly.
- **Portable** — JSON export and import, free.
- **Confirm before restoring** — a scene restore always lists what will be overwritten first. Free and on by default.

### Pro

- **Page scenes** — save the scroll offset plus every form field on the page under a name you choose, and put them all back later.
- **500 pages of history** instead of 20.
- **Pill timing** — delay, fade and trigger distance all adjustable.
- **Ignore list** — domains and regular expressions.
- **Several named scenes per page** — 1 to 20 each.
- **Read-only fields** optionally included in a scene.
- **Keyboard shortcut** to save a scene.
- **Scenes in the export** — the JSON file carries positions and scenes.

| | Free | Pro |
|---|---|---|
| Reading positions | ✅ | ✅ |
| Pages of history | 20 | 500 |
| Continue pill | fixed timing | delay / fade / trigger distance adjustable |
| Ignore list | domains | domains **and** regular expressions |
| Page scenes (save + restore) | — | ✅ |
| Text fields, textareas, selects, checkboxes, radios, open panels | — | ✅ |
| Multiple named scenes per page | — | ✅ |
| Scene cap per page | — | configurable (1–20) |
| Read-only fields included | — | optional |
| Save with a keyboard shortcut | — | optional (off by default) |
| JSON export / import | positions | positions + scenes |

One licence activates on two browsers. Removing the licence frees the slot.

## What a scene captures

| Element | Restored |
|---|---|
| `input` text-like (`text`, `email`, `url`, `tel`, `number`, `search`, `date`, …) | value |
| `textarea` | value |
| `select` | selected option (falls back to matching the option label) |
| `checkbox`, `radio` | checked state |
| `details` | open / closed |
| scroll position | yes, same retry logic as the pill |

Values go back through the native property setter and are followed by `input` and `change` events, so React and Vue notice them. Fields are found by structural path first, then by `id`, then by `name`, so a page that reflows slightly still restores.

### Not captured

- **Passwords, files, hidden inputs, one-time codes, card numbers, CVV, IBAN, SSN** — excluded at capture time. There is no setting that re-enables them.
- **Read-only and disabled fields** — unless you switch read-only on (Pro).
- **`role="tab"` panels, `contenteditable` editors** — no framework-safe way to put the state back, so they are left alone rather than half-done.
- **Fields inside shadow roots and cross-origin iframes** — no stable path can be built across those boundaries.

## Usage

### Reading

1. Scroll any page. Stop, and the position is remembered.
2. Come back to that page. If you are still near the top, the pill appears in the bottom-right corner after a moment.
3. Click it to smooth-scroll to your spot, or ignore it and it fades.
4. Open the popup for the rest: **Continue reading**, **Pause on this site**, **Clear this page**, and the history list.

### Saving a scene (Pro)

1. Put the page in the state you want to keep.
2. Open the popup, click **Save scene**, give it a name, save. Or press the keyboard shortcut if you enabled it.
3. Later, on the same page, the popup lists every scene saved there.
4. Click **Restore**. A panel lists what will be overwritten — confirm, and every field and the scroll offset come back.

The scene stays editable: rename it or delete it from its row at any time.

### Managing data

- **History row** — click to reopen that page; the bin icon removes just that record.
- **Data section** — shows how many positions and scenes are stored and how much space they take.
- **Clear all positions** / **Delete all scenes** — both ask for confirmation first.
- **Export** writes positions (and scenes on Pro) to a JSON file. **Import** merges instead of replacing and keeps the newer record when both sides know the same page.

### Settings

- **Free** — pause a site, clear a page, export and import.
- **Pro** — pill delay, fade and trigger distance; ignore domains and regular expressions; scenes per page; read-only fields; keyboard shortcut.

On Firefox the optional shortcut is set in the Add-ons Manager: **about:addons → gear icon → Manage Extension Shortcuts**.

## Browsers

| Browser | Minimum |
|---|---|
| Chrome | 116 |
| Edge | 116 |
| Firefox | 140 |

The Firefox build lives in `codeup/VKT-ReadPos`. A JSON export from one imports into the other unchanged.

## Links

- Website: https://annmax1983.com/extensions/readpos
- Privacy policy: [privacy-policy.html](privacy-policy.html)

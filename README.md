# VKT ReadPos — 读位

English | [中文](languages/README_zh.md) | [日本語](languages/README_ja.md) | [Deutsch](languages/README_de.md) | [Español](languages/README_es.md) | [Français](languages/README_fr.md)

Reading position memory and page restore points for Chrome, Edge and Firefox. Every long page remembers where you stopped; a page that holds state — a dashboard, a half-filled form, a ticket you were writing — can be saved as a *restore point* and put back whole.

Install: https://annmax1983.com/extensions/readpos

## Features

### Reading positions (free)

- **Per page, automatically** — the scroll offset is recorded 600 ms after you stop scrolling, and again when the tab is hidden or closed.
- **Continue where you left off** — reopen the exact page and it scrolls back to your spot on its own. Any other page gets a small pill instead: *Continue where you left off · 62%*, one click away. The automatic jump can be switched off in Settings.
- **Quiet** — the pill fades after a few seconds; moving the pointer into the bottom-right corner brings it back. Nothing else is drawn on the page.
- **Short pages stay quiet** — a position below the trigger distance (600 px by default) is not offered.
- **Pause a site, clear a page** — one click each, in the popup.
- **Record only the sites you choose** — a recording list you can fill by hand or with one click on "add current site"; every other site is left alone. There is no "record everything" switch: an empty list records nothing. Free.
- **Exclude one page, or a pattern** — *Exclude this page* in the popup is one click, and that address is never remembered again, on a site that otherwise keeps recording. Pattern rules (regular expressions) take out whole sections across every site without naming them one by one. Both lists are free and live on the **Excluded** tab of the full list.
- **20 pages of history** — the popup lists the pages of the site you are on, with the percentage reached; click a row to reopen it.
- **SPA routes counted separately** — `/app/inbox` and `/app/settings` are different pages. Hash routers (`#/route`) too.
- **Lazy pages** — if the saved offset is beyond the current document height, the restore retries twice and then gives up quietly.
- **Portable** — JSON export and import, free.
- **Confirm before restoring** — a restore point restore always lists what will be overwritten first. Free and on by default.

### Pro

- **Page restore points** — save the scroll offset plus every form field on the page under a name you choose, and put them all back later.
- **500 pages of history** instead of 20.
- **Pill timing** — delay, fade and trigger distance all adjustable.
- **Several named restore points per page** — 1 to 20 each.
- **Read-only fields** optionally included in a restore point.
- **Keyboard shortcut** to save a restore point.
- **Restore points travel with the export** — the JSON file carries positions and restore points; importing restore points back needs Pro.

| | Free | Pro |
|---|---|---|
| Reading positions | ✅ | ✅ |
| Pages of history | 20 | 500 |
| Continue pill | fixed timing | delay / fade / trigger distance adjustable |
| Page restore points (save + restore) | — | ✅ |
| Text fields, textareas, selects, checkboxes, radios, open panels | — | ✅ |
| Multiple named restore points per page | — | ✅ |
| Restore points per page | — | configurable (1–20) |
| Read-only fields included | — | optional |
| Save with a keyboard shortcut | — | optional (off by default) |
| JSON export / import | positions | positions + restore points |

One licence activates on two browsers. Removing the licence frees the slot.

## What a restore point captures

| Element | Restored |
|---|---|
| `input` text-like (`text`, `email`, `url`, `tel`, `number`, `search`, `date`, …) | value |
| `textarea` | value |
| `select` | selected option (falls back to matching the option label) |
| `select multiple` | every chosen option |
| component dropdowns (Element Plus, Ant Design, Arco, Naive, Semi, iView) | the chosen label |
| `contenteditable` rich-text editors | the text |
| fields inside open shadow roots (Web Components) | value |
| `checkbox`, `radio` | checked state |
| `details` | open / closed |
| scroll position | yes, same retry logic as the pill |

Values go back through the native property setter and are followed by `input` and `change` events, so React and Vue notice them; if a framework still ignores that, the text is retyped with `insertText`. Fields are found by a short CSS selector first (it reaches into open shadow roots), then by `id`, then by `name` plus the current value, and only then by structural path. Before writing, the label is compared with the one recorded at save time, so a reordered form never gets the wrong values. If a field still refuses, the text is typed in through the browser's debugger — attach, one insert, detach — because real input is the only thing such a component accepts.

### Not captured

- **Passwords, files, hidden inputs, one-time codes, card numbers, CVV, IBAN, SSN** — excluded at capture time. There is no setting that re-enables them.
- **Read-only and disabled fields** — unless you switch read-only on (Pro).
- **`role="tab"` panels** — switching a tab fires the framework's own loading logic, so there is nothing safe to write back.
- **Fields inside closed shadow roots and cross-origin iframes** — the nodes cannot be reached, so no path can be built.

## Usage

### Reading

1. Scroll any page. Stop, and the position is remembered.
2. Come back to that page. The exact page scrolls to your spot on its own; a page that does not match exactly shows the pill in the bottom-right corner instead.
3. With the pill, click to scroll back — or ignore it and it fades. Turn the automatic jump off in Settings if you would rather click every time.
4. Open the popup for the rest: **Continue reading**, **Pause on this site**, **Exclude this page**, **Clear this page**, and the history list.

### Saving a restore point (Pro)

1. Put the page in the state you want to keep.
2. Open the popup, click **Save restore point**, give it a name, save. Or press the keyboard shortcut if you enabled it.
3. Later, on the same page, the popup lists every restore point saved there.
4. Click **Restore**. A panel lists what will be overwritten — confirm, and every field and the scroll offset come back.

The restore point stays editable: rename it or delete it from its row at any time.

### Managing data

- **History row** — click to reopen that page; the bin icon removes just that record.
- **Excluded tab** — the full list has one: add a page by pasting its address, or a pattern rule, and every entry can be removed again from its row. A rule shows how many stored pages it currently catches.
- **Data section** — shows how many positions and restore points are stored and how much space they take.
- **Clear all positions** / **Delete all restore points** — both ask for confirmation first.
- **Export** writes positions (and restore points on Pro) to a JSON file. **Import** merges instead of replacing and keeps the newer record when both sides know the same page.

### Settings

- **Free** — pause a site, exclude pages and patterns, clear a page, export and import.
- **Pro** — pill delay, fade and trigger distance; restore points per page; read-only fields; keyboard shortcut.

On Firefox the optional shortcut is set in the Add-ons Manager: **about:addons → gear icon → Manage Extension Shortcuts**.

## Permissions

- **`storage`** — positions, restore points, settings and licence state. Local only.
- **Access to http and https pages** — the content script has to exist before you scroll, or there would be nothing to remember.
- **`debugger`** — one job: typing into a field that ignores every scripted write. Connect, insert once, disconnect. It never reads the page and never stays attached.

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

# VKT ReadPos — 读位

English | [中文](languages/README_zh.md) | [日本語](languages/README_ja.md) | [Deutsch](languages/README_de.md) | [Español](languages/README_es.md) | [Français](languages/README_fr.md)

Reading position memory and page scene snapshots for Chrome and Edge. Every long page remembers where you stopped; short pages that hold state — a dashboard, a half-filled form, a ticket you were writing — can be saved as a *scene* and brought back whole.

Everything is computed and stored on your device. No account, no sync service, no telemetry. The only network request this extension ever makes is licence validation.

## Highlights (free)

- **Position memory, per page** — the scroll offset is written 600 ms after you stop scrolling and again when the tab is hidden or closed. No polling, no per-pixel writes.
- **The continue pill** — reopening a page shows a small control in the bottom-right corner: *Continue where you left off · 62%*. One click smooth-scrolls back. It never scrolls on its own.
- **Quiet by design** — the pill fades after a few seconds; moving the pointer into the bottom-right corner brings it back. Nothing else is ever drawn on the page.
- **Short pages stay quiet** — a position below the trigger distance (600 px by default) is not offered.
- **Per-site pause, clear this page** — both one click away in the popup.
- **20 pages of history** — the popup lists recent positions with the percentage reached; click a row to reopen it.
- **SPA routes tracked separately** — `/app/inbox` and `/app/settings` are different pages. Hash routers (`#/route`) count too.
- **Lazy-loaded pages handled** — if the saved offset is beyond the current document height, the restore retries twice and then gives up silently rather than fighting the page.
- **Your data, portable** — JSON export and import, free.
- **Ask before restoring** — a scene restore always shows what will be overwritten first. On by default, and free: a confirmation step is not an upsell.

## Pro (US$9.90, one-time)

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
| Keyboard shortcut to save | — | optional (off by default) |
| JSON export / import | ✅ positions | ✅ positions + scenes |

One licence activates on two browsers. Removing the licence frees the slot. Validation is throttled to once every two hours, works offline from the cached licence, and a failed check never locks you out.

## What a scene captures

| Element | Restored |
|---|---|
| `input` text-like (`text`, `email`, `url`, `tel`, `number`, `search`, `date`, …) | value |
| `textarea` | value |
| `select` | selected option (falls back to matching the option label) |
| `checkbox`, `radio` | checked state |
| `details` | open / closed |
| scroll position | yes, same retry logic as the pill |

Values are written through the native property setter and followed by `input` and `change` events, so React and Vue notice them. Fields are located by a structural path first, then by `id`, then by `name` — a page that reflows slightly still restores.

### What it deliberately will not touch

- **Passwords, files, hidden inputs, one-time codes, card numbers, CVV, IBAN, SSN.** Excluded at capture time, not filtered at restore time. There is no setting to enable them.
- **Read-only and disabled fields**, unless you switch read-only on (Pro). Computed fields are noise in a scene.
- **`role="tab"` panels.** This was in the original plan and is gone: tab state is driven by the framework, so writing `aria-selected` back either does nothing or triggers navigation. A feature that looks like it works but does not is worse than no feature.
- **`contenteditable` editors.** There is no reliable, framework-safe way to put text back into a rich editor. Left out rather than half-done.
- **Fields inside shadow roots and cross-origin iframes.** A stable path cannot be built across those boundaries, so they are skipped instead of being guessed at.

## How it works

1. Scroll any page. ~600 ms after you stop, the offset is written to `chrome.storage.local` under a normalised URL key.
2. Come back to the page. If the saved offset is past the trigger distance and you are still near the top, the pill appears after 600 ms.
3. Click it — the page smooth-scrolls to your spot. If the page has grown since (lazy images), the restore retries and then stays quiet.
4. On Pro, **Save scene** in the popup (or the optional shortcut) captures scroll + fields. **Restore** shows a confirmation panel listing what will be overwritten, then writes everything back.

## Privacy

- Reading positions, scenes and settings live in `chrome.storage.local`. Nothing is uploaded.
- No analytics, no telemetry, no accounts.
- Captured values never leave the browser: restoring a scene is a local read and a local write.
- The only network call is licence activation/validation against `api.annmax1983.com`, and only after you enter a key.

## Permissions

| Permission | Why |
|---|---|
| `storage` | Positions, scenes, settings, licence state — all local |
| `http://*/*`, `https://*/*` | The content script has to exist on every page **before** you scroll, otherwise there is nothing to remember. Unlike a bookmark tool, this one cannot wait to be asked per site. It is the reason this extension asks for broad page access, and the store listing says so plainly. |

No `tabs`, no `scripting`, no `debugger`, no `activeTab`: tab metadata comes from the host permission, and the content script is declarative. Nothing runs on `chrome://` or `edge://` pages, `file://` or another extension's pages.

## Supported browsers

Chrome and Edge (Manifest V3, Chrome 116+). No Firefox build yet.

## Development

```bash
npm install         # dev-only: terser, puppeteer-core (screenshots)
npm test            # node --test — 99 assertions
npm run build       # minify + zip → publish/readpos-v{version}.zip
npm run locales     # regenerate _locales/*/messages.json from tools/make-locales.js
npm run icons       # regenerate icons/*.png + assets/icon-master.png
npm run shoot       # store screenshots → screenshots/ (needs Chrome)
npm run e2e         # live browser check of the shipping scripts (needs puppeteer-core)
```

Layout:

```
manifest.json        MV3; declarative content script on http(s), document_start
common.js            RP: URL keys, settings, position/scene helpers (pure, unit-tested)
background.js        service worker: owns all storage, message router, Pro gating
content/content.js   scroll tracking, the pill, scene capture/restore, confirm panel
popup/               popup.html / popup.js / popup.css
license.js           LicenseManager IIFE (api.annmax1983.com/api/license)
_locales/            en, zh_CN, ja, de, es, fr — 105 keys, identical key sets
tools/               locale generator, icon renderer, screenshot harness
index.html            landing page — workspace standard template, 6 languages
privacy-policy.html   policy page, 8 sections × 6 languages (family template)
store-listing/        six CRLF text files, one per store language
```

Two details that are easy to break:

- The content script runs at `document_start` and wraps `history.pushState`/`replaceState` **before** the page's own router does, or the first SPA navigation is missed.
- `PILL` and the confirm panel live in closed shadow roots. A page stylesheet that sets `button { … }` globally would otherwise deform both.

## Links

- Website: https://annmax1983.com
- Licence & pricing: https://annmax1983.com/checkout?plugin=readpos

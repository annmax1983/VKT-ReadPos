# VKT ReadPos

[English](../README.md) | [中文](README_zh.md) | [日本語](README_ja.md) | Deutsch | [Español](README_es.md) | [Français](README_fr.md)

Leseposition und Seitenzustand für Chrome, Edge und Firefox. Jede lange Seite merkt sich, wo Sie aufgehört haben; kürzere Seiten mit Zustand — ein Dashboard, ein halb ausgefülltes Formular, ein angefangenes Ticket — lassen sich als *Zustand* sichern und vollständig zurückholen.

Alles wird auf Ihrem Gerät berechnet und gespeichert. Kein Konto, kein Sync-Dienst, keine Telemetrie. Die einzige Netzwerkanfrage ist die Lizenzprüfung.

## Kostenlos

- **Leseposition pro Seite** — 600 ms nach dem letzten Scrollen wird einmal geschrieben, erneut beim Verstecken oder Schließen des Tabs. Kein Schreiben pro Pixel.
- **Weiterlesen-Schaltfläche** — beim erneuten Öffnen erscheint unten rechts *Weiter, wo Sie aufgehört haben · 62%*. Ein Klick scrollt weich zurück. **Die Seite wird nie von selbst bewegt.**
- **Unaufdringlich** — die Schaltfläche blendet sich nach einigen Sekunden aus; Zeiger in die untere rechte Ecke bringt sie zurück. Sonst wird nichts auf der Seite gezeichnet.
- **Kurze Seiten bleiben still** — Positionen unter der Schwelle (Standard 600 px) werden nicht angeboten.
- **Website pausieren / Diese Seite löschen** — je ein Klick im Popup.
- **20 Seiten Verlauf** — mit Prozentangabe; ein Klick auf die Zeile öffnet die Seite.
- **SPA-Routen getrennt** — `/app/inbox` und `/app/settings` sind verschiedene Seiten, Hash-Router (`#/route`) ebenfalls.
- **Nachladen berücksichtigt** — liegt die gespeicherte Position jenseits der aktuellen Dokumenthöhe, wird zweimal erneut versucht und dann stillschweigend aufgegeben.
- **Daten mitnehmbar** — JSON-Export und -Import, kostenlos.
- **Nachfragen vor dem Wiederherstellen** — vor jedem Zurückschreiben wird gezeigt, was überschrieben wird. Standardmäßig an, und kostenlos: eine Sicherheitsabfrage ist kein Verkaufsargument.

## Pro (9,99 US-Dollar, einmalig)

| | Kostenlos | Pro |
|---|---|---|
| Lesepositionen | ✅ | ✅ |
| Verlauf | 20 Seiten | 500 Seiten |
| Weiterlesen-Schaltfläche | feste Zeiten | Verzögerung / Ausblenden / Schwelle einstellbar |
| Ausnahmen | Domains | Domains **und** reguläre Ausdrücke |
| Seitenzustand (sichern + wiederherstellen) | — | ✅ |
| Textfelder, Textareas, Auswahlfelder, Kontrollkästchen, Radiobuttons, offene Bereiche | — | ✅ |
| Mehrere benannte Zustände pro Seite | — | ✅ |
| Zustände pro Seite | — | einstellbar (1–20) |
| Schreibgeschützte Felder einbeziehen | — | optional |
| Tastenkürzel zum Sichern | — | optional (standardmäßig aus) |
| JSON-Export/-Import | ✅ Positionen | ✅ Positionen + Zustände |

Eine Lizenz gilt für zwei Browser. Entfernen gibt den Platz frei. Die Prüfung läuft höchstens alle zwei Stunden, funktioniert offline aus dem Cache und sperrt Sie bei einem Fehlschlag nie aus.

## Was ein Zustand erfasst

Werte von `input` (text, email, url, tel, number, search, date …), `textarea`, die gewählte Option von `select`, den Zustand von `checkbox`/`radio`, offene `details` und die Scrollposition. Geschrieben wird über den nativen Setter, gefolgt von `input` und `change` — React und Vue merken es. Felder werden über Strukturpfad, dann `id`, dann `name` gefunden.

### Was bewusst nicht angefasst wird

- **Passwörter, Dateien, versteckte Felder, Einmalcodes, Kartennummern, CVV, IBAN, SSN** — bereits beim Erfassen ausgeschlossen, nicht erst beim Wiederherstellen gefiltert. Es gibt keine Einstellung, die das einschaltet.
- **Schreibgeschützte und deaktivierte Felder** — standardmäßig übersprungen (Pro: schreibgeschützt optional).
- **`role="tab"`-Bereiche** — aus dem ursprünglichen Plan gestrichen. Tab-Zustand gehört dem Framework; `aria-selected` zurückzuschreiben bewirkt entweder nichts oder löst Navigation aus. Ein Feature, das zu funktionieren scheint, ist schlechter als keines.
- **`contenteditable`-Editoren** — kein verlässlicher, framework-sicherer Weg zurück; lieber weggelassen.
- **Felder in Shadow Roots und Cross-Origin-iframes** — dort lässt sich kein stabiler Pfad bauen, also übersprungen statt geraten.

## Datenschutz

Positionen, Zustände und Einstellungen liegen ausschließlich im lokalen Speicher der Erweiterung (`chrome.storage.local`, unter Firefox `browser.storage.local`). Keine Statistik, keine Telemetrie, keine Konten. Erfasste Werte verlassen den Browser nicht; die einzige Netzwerkanfrage ist die Lizenzprüfung gegen `api.annmax1983.com`.

## Berechtigungen

| Berechtigung | Grund |
|---|---|
| `storage` | Positionen, Zustände, Einstellungen, Lizenzstatus — alles lokal |
| `http://*/*`, `https://*/*` | Das Content-Script muss **vor** dem Scrollen auf jeder Seite existieren, sonst gibt es nichts zu merken. Anders als bei einem Lesezeichen-Tool lässt sich das nicht pro Website nachträglich erfragen — daher der breite Seitenzugriff, den die Store-Beschreibung ausdrücklich nennt. |

Kein `tabs`, kein `scripting`, kein `debugger`, kein `activeTab`. Auf `chrome://`-Seiten (Chrome), `edge://`-Seiten (Edge), `about:`-Seiten (Firefox), `file://` und den Seiten anderer Erweiterungen läuft nichts.

## Unterstützte Browser

| Browser | Wo der Build liegt | Minimum |
|---|---|---|
| Chrome | dieses Repository | Chrome 116 — Manifest V3, `background.service_worker` |
| Edge | dieses Repository, dasselbe Paket wie Chrome | Edge 116 |
| Firefox | `codeup/VKT-ReadPos` | Firefox 140 — Manifest V3, `background.scripts` |

Beide Builds sind dieselbe Erweiterung: `common.js`, `content/content.js`, `popup/`, `_locales/`, die Icons und dieses Dokument sind gemeinsam, und ein JSON-Export des einen lässt sich unverändert in den anderen importieren. Nur drei Dinge unterscheiden sich: das Manifest (eine Event-Page aus einer Skriptliste statt eines Service Workers, dazu eine gecko-id und die von AMO verlangte Datenerklärung), der API-Namensraum (Firefox nutzt `browser.*` mit Promises statt `chrome.*` mit Callbacks) und das Packaging (der Firefox-Build erzeugt zusätzlich ein unminifiziertes Quell-Zip für die Prüfung).

Ein sichtbarer Unterschied: Der optionale Tastaturkurzbefehl wird im Add-ons-Manager von Firefox gesetzt (about:addons → Zahnrad → Tastenkürzel verwalten), nicht unter `chrome://extensions/shortcuts`.

## Entwicklung

```bash
npm install
npm test            # node --test (99 Assertions)
npm run build       # minifizieren + zip → publish/readpos-v{version}.zip
npm run locales     # sechs Sprachdateien neu erzeugen
npm run icons       # Icons neu rendern
npm run shoot       # Store-Screenshots (Chrome nötig)
```

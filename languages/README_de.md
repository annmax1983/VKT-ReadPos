# VKT ReadPos

[English](../README.md) | [中文](README_zh.md) | [日本語](README_ja.md) | Deutsch | [Español](README_es.md) | [Français](README_fr.md)

Leseposition und Seitenzustand für Chrome, Edge und Firefox. Jede lange Seite merkt sich, wo Sie aufgehört haben; Seiten mit Zustand — ein Dashboard, ein halb ausgefülltes Formular, ein angefangenes Ticket — lassen sich als *Szene* sichern und vollständig zurückholen.

Installation: https://annmax1983.com/extensions/readpos

## Funktionen

### Lesepositionen (kostenlos)

- **Pro Seite, automatisch** — der Scroll-Offset wird 600 ms nach dem Anhalten gespeichert, und erneut, wenn der Tab verborgen oder geschlossen wird.
- **Die Weiterlesen-Pille** — beim erneuten Öffnen einer Seite erscheint unten rechts *Weiterlesen · 62 %*. Ein Klick scrollt weich zurück. Von selbst scrollt sie nie.
- **Unauffällig** — die Pille blendet nach einigen Sekunden ab; bewegt man den Zeiger in die untere rechte Ecke, kommt sie zurück. Sonst wird nichts auf der Seite gezeichnet.
- **Kurze Seiten bleiben still** — Positionen unter der Auslöseschwelle (standardmäßig 600 px) werden nicht angeboten.
- **Website pausieren, Seite löschen** — je ein Klick im Popup.
- **20 Einträge Verlauf** — das Popup listet die letzten Positionen mit Prozentwert; ein Klick auf eine Zeile öffnet die Seite.
- **SPA-Routen getrennt** — `/app/inbox` und `/app/settings` sind verschiedene Seiten. Hash-Routen (`#/route`) ebenso.
- **Nachladende Seiten** — liegt der gespeicherte Offset jenseits der aktuellen Dokumenthöhe, wird zweimal erneut versucht und dann still aufgegeben.
- **Mitnehmbar** — JSON-Export und -Import, kostenlos.
- **Bestätigung vor dem Wiederherstellen** — vor dem Zurückholen einer Szene wird immer aufgelistet, was überschrieben wird. Kostenlos und standardmäßig an.

### Pro

- **Seiten-Szenen** — Scroll-Offset plus jedes Formularfeld der Seite unter einem Namen Ihrer Wahl speichern und später alles zurückschreiben.
- **500 Einträge Verlauf** statt 20.
- **Pill-Timing** — Verzögerung, Ausblenden und Auslöseschwelle einstellbar.
- **Ausschlussliste** — Domains und reguläre Ausdrücke.
- **Mehrere benannte Szenen pro Seite** — je 1 bis 20.
- **Schreibgeschützte Felder** — optional in einer Szene enthalten.
- **Tastaturkurzbefehl** zum Sichern einer Szene.
- **Szenen im Export** — die JSON-Datei enthält Positionen und Szenen.

| | Kostenlos | Pro |
|---|---|---|
| Lesepositionen | ✅ | ✅ |
| Einträge Verlauf | 20 | 500 |
| Weiterlesen-Pille | festes Timing | Verzögerung / Ausblenden / Auslöseschwelle einstellbar |
| Ausschlussliste | Domains | Domains **und** reguläre Ausdrücke |
| Seiten-Szenen (speichern + wiederherstellen) | — | ✅ |
| Textfelder, Textareas, Auswahlfelder, Kontrollkästchen, Radios, offene Panels | — | ✅ |
| Mehrere benannte Szenen pro Seite | — | ✅ |
| Obergrenze Szenen pro Seite | — | einstellbar (1–20) |
| Schreibgeschützte Felder einbeziehen | — | optional |
| Mit Kurzbefehl sichern | — | optional (standardmäßig aus) |
| JSON-Export / -Import | Positionen | Positionen + Szenen |

Eine Lizenz aktiviert zwei Browser. Das Entfernen der Lizenz gibt den Platz frei.

## Was eine Szene enthält

| Element | Wiederhergestellt |
|---|---|
| `input` textartig (`text`, `email`, `url`, `tel`, `number`, `search`, `date`, …) | Wert |
| `textarea` | Wert |
| `select` | gewählte Option (Ersatz: Übereinstimmung über die Beschriftung) |
| `checkbox`, `radio` | Zustand des Kontrollkästchens |
| `details` | offen / geschlossen |
| Scroll-Position | ja, dieselbe Wiederholungslogik wie die Pille |

Werte werden über den nativen Setter zurückgeschrieben, gefolgt von `input` und `change`, damit React und Vue es mitbekommen. Felder werden zuerst über den strukturellen Pfad gefunden, dann über `id`, dann über `name` — eine Seite, die sich leicht verschiebt, wird trotzdem wiederhergestellt.

### Wird nicht erfasst

- **Passwörter, Dateien, versteckte Felder, Einmalcodes, Kartennummern, CVV, IBAN, SSN** — beim Erfassen ausgeschlossen. Es gibt keine Einstellung, die sie wieder aktiviert.
- **Schreibgeschützte und deaktivierte Felder** — außer Sie schalten schreibgeschützt ein (Pro).
- **`role="tab"`-Panels, `contenteditable`-Editoren** — kein framework-sicherer Weg, den Zustand zurückzuschreiben; deshalb lieber weggelassen als halb umgesetzt.
- **Felder in Shadow Roots und Cross-Origin-iframes** — über diese Grenzen lässt sich kein stabiler Pfad bauen.

## Benutzung

### Lesen

1. Scrollen Sie eine beliebige Seite. Sobald Sie anhalten, wird die Position gemerkt.
2. Kehren Sie zu der Seite zurück. Wenn Sie noch oben sind, erscheint nach einem Moment die Pille unten rechts.
3. Klicken Sie sie an, um weich zu Ihrer Stelle zu scrollen — oder ignorieren Sie sie, dann blendet sie ab.
4. Der Rest steckt im Popup: **Weiterlesen**, **Auf dieser Website pausieren**, **Diese Seite löschen** und der Verlauf.

### Eine Szene sichern (Pro)

1. Bringen Sie die Seite in den Zustand, den Sie behalten wollen.
2. Öffnen Sie das Popup, klicken Sie **Szene sichern**, geben Sie einen Namen ein und speichern Sie. Alternativ der Kurzbefehl, wenn aktiviert.
3. Später listet das Popup auf derselben Seite jede gesicherte Szene.
4. Klicken Sie **Wiederherstellen**. Ein Panel zeigt, was überschrieben wird — bestätigen, und alle Felder plus Scroll-Offset kommen zurück.

Die Szene bleibt bearbeitbar: umbenennen oder löschen Sie sie jederzeit über ihre Zeile.

### Daten verwalten

- **Verlaufszeile** — Klick öffnet die Seite; das Papierkorb-Symbol entfernt nur diesen Eintrag.
- **Datenbereich** — zeigt, wie viele Positionen und Szenen gespeichert sind und wie viel Platz sie brauchen.
- **Alle Positionen löschen** / **Alle Szenen löschen** — beide fragen vorher nach.
- **Export** schreibt Positionen (und mit Pro Szenen) in eine JSON-Datei. **Import** führt zusammen statt zu ersetzen und behält den neueren Datensatz, wenn beide Seiten dieselbe Seite kennen.

### Einstellungen

- **Kostenlos** — Website pausieren, Seite löschen, Export und Import.
- **Pro** — Pill-Verzögerung, Ausblenden und Auslöseschwelle; ausgeschlossene Domains und reguläre Ausdrücke; Szenen pro Seite; schreibgeschützte Felder; Tastaturkurzbefehl.

Unter Firefox wird der optionale Kurzbefehl im Add-ons-Manager gesetzt: **about:addons → Zahnrad → Tastenkürzel verwalten**.

## Browser

| Browser | Mindestversion |
|---|---|
| Chrome | 116 |
| Edge | 116 |
| Firefox | 140 |

Der Firefox-Build liegt in `codeup/VKT-ReadPos`. Ein JSON-Export aus dem einen lässt sich im anderen unverändert importieren.

## Links

- Website: https://annmax1983.com/extensions/readpos
- Datenschutzerklärung: [privacy-policy.html](../privacy-policy.html)

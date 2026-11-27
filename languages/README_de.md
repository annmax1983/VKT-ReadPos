# VKT ReadPos

[English](../README.md) | [中文](README_zh.md) | [日本語](README_ja.md) | Deutsch | [Español](README_es.md) | [Français](README_fr.md)

Leseposition und Seitenzustand für Chrome, Edge und Firefox. Jede lange Seite merkt sich, wo Sie aufgehört haben; Seiten mit Zustand — ein Dashboard, ein halb ausgefülltes Formular, ein angefangenes Ticket — lassen sich als *Wiederherstellungspunkt* sichern und vollständig zurückholen.

Installation: https://annmax1983.com/extensions/readpos

## Funktionen

### Lesepositionen (kostenlos)

- **Pro Seite, automatisch** — der Scroll-Offset wird 600 ms nach dem Anhalten gespeichert, und erneut, wenn der Tab verborgen oder geschlossen wird.
- **Weiterlesen, automatisch** — dieselbe Seite öffnet sich wieder an der gespeicherten Stelle. Jede andere Seite bekommt stattdessen die kleine Pille *Weiterlesen · 62 %*, ein Klick entfernt. Der automatische Sprung lässt sich in den Einstellungen abschalten.
- **Unauffällig** — die Pille blendet nach einigen Sekunden ab; bewegt man den Zeiger in die untere rechte Ecke, kommt sie zurück. Sonst wird nichts auf der Seite gezeichnet.
- **Kurze Seiten bleiben still** — Positionen unter der Auslöseschwelle (standardmäßig 600 px) werden nicht angeboten.
- **Website pausieren, Seite löschen** — je ein Klick im Popup.
- **Nur gewählte Websites aufzeichnen** — Domains von Hand oder per Klick auf „Aktuelle Website hinzufügen“; alle anderen bleiben unberührt. Es gibt keinen Schalter für „alles aufzeichnen“ — eine leere Liste zeichnet nichts auf. Kostenlos.
- **Eine Seite ausschließen — oder ein Muster** — „Diese Seite ausschließen“ im Popup ist ein Klick: Diese Adresse wird nie wieder aufgezeichnet, der Rest der Website schon. Musterregeln (reguläre Ausdrücke) nehmen ganze Bereiche auf allen Websites heraus, ohne sie einzeln aufzählen zu müssen. Beide Listen sind kostenlos und liegen im Tab **Ausgeschlossen** der vollständigen Liste.
- **20 Einträge Verlauf** — das Popup listet die Positionen der aktuellen Website mit Prozentwert; ein Klick auf eine Zeile öffnet die Seite.
- **SPA-Routen getrennt** — `/app/inbox` und `/app/settings` sind verschiedene Seiten. Hash-Routen (`#/route`) ebenso.
- **Nachladende Seiten** — liegt der gespeicherte Offset jenseits der aktuellen Dokumenthöhe, wird zweimal erneut versucht und dann still aufgegeben.
- **Mitnehmbar** — JSON-Export und -Import, kostenlos.
- **Bestätigung vor dem Wiederherstellen** — vor dem Zurückholen eines Wiederherstellungspunkts wird immer aufgelistet, was überschrieben wird. Kostenlos und standardmäßig an.

### Pro

- **Seiten-Wiederherstellungspunkte** — Scroll-Offset plus jedes Formularfeld der Seite unter einem Namen Ihrer Wahl speichern und später alles zurückschreiben.
- **500 Einträge Verlauf** statt 20.
- **Pill-Timing** — Verzögerung, Ausblenden und Auslöseschwelle einstellbar.
- **Mehrere benannte Wiederherstellungspunkte pro Seite** — je 1 bis 20.
- **Schreibgeschützte Felder** — optional in einem Wiederherstellungspunkt enthalten.
- **Tastaturkurzbefehl** zum Sichern eines Wiederherstellungspunkts.
- **Wiederherstellungspunkte reisen mit** — die JSON-Datei enthält Positionen und Wiederherstellungspunkte; Wiederherstellungspunkte zurückzuimportieren braucht Pro.

| | Kostenlos | Pro |
|---|---|---|
| Lesepositionen | ✅ | ✅ |
| Einträge Verlauf | 20 | 500 |
| Weiterlesen-Pille | festes Timing | Verzögerung / Ausblenden / Auslöseschwelle einstellbar |
| Seiten-Wiederherstellungspunkte (speichern + wiederherstellen) | — | ✅ |
| Textfelder, Textareas, Auswahlfelder, Kontrollkästchen, Radios, offene Panels | — | ✅ |
| Mehrere benannte Wiederherstellungspunkte pro Seite | — | ✅ |
| Wiederherstellungspunkte pro Seite | — | einstellbar (1–20) |
| Schreibgeschützte Felder einbeziehen | — | optional |
| Mit Kurzbefehl sichern | — | optional (standardmäßig aus) |
| JSON-Export / -Import | Positionen | Positionen + Wiederherstellungspunkte |

Eine Lizenz aktiviert zwei Browser. Das Entfernen der Lizenz gibt den Platz frei.

## Was ein Wiederherstellungspunkt enthält

| Element | Wiederhergestellt |
|---|---|
| `input` textartig (`text`, `email`, `url`, `tel`, `number`, `search`, `date`, …) | Wert |
| `textarea` | Wert |
| `select` | gewählte Option (Ersatz: Übereinstimmung über die Beschriftung) |
| `select multiple` | alle gewählten Optionen |
| Komponenten-Dropdowns (Element Plus, Ant Design, Arco, Naive, Semi, iView) | die gewählte Beschriftung |
| `contenteditable`-Editoren | der Text |
| Felder in offenen Shadow Roots (Web Components) | Wert |
| `checkbox`, `radio` | Zustand des Kontrollkästchens |
| `details` | offen / geschlossen |
| Scroll-Position | ja, dieselbe Wiederholungslogik wie die Pille |

Werte werden über den nativen Setter zurückgeschrieben, gefolgt von `input` und `change`, damit React und Vue es mitbekommen; ignoriert ein Framework auch das, wird der Text per `insertText` neu getippt. Felder werden zuerst über einen kurzen CSS-Selektor gefunden (er reicht in offene Shadow Roots hinein), dann über `id`, dann über `name` plus aktuellen Wert, erst zuletzt über den strukturellen Pfad. Vor dem Schreiben wird die Beschriftung mit der beim Sichern erfassten verglichen — ein umsortiertes Formular bekommt so keine falschen Werte. Weigert sich ein Feld trotzdem, wird der Text über den Debugger des Browsers getippt — verbinden, einmal einfügen, trennen; solche Komponenten akzeptieren nur echte Eingaben.

### Wird nicht erfasst

- **Passwörter, Dateien, versteckte Felder, Einmalcodes, Kartennummern, CVV, IBAN, SSN** — beim Erfassen ausgeschlossen. Es gibt keine Einstellung, die sie wieder aktiviert.
- **Schreibgeschützte und deaktivierte Felder** — außer Sie schalten schreibgeschützt ein (Pro).
- **`role="tab"`-Panels** — ein Tab-Wechsel löst die Ladelogik des Frameworks aus, sicher zurückschreiben lässt sich da nichts.
- **Felder in geschlossenen Shadow Roots und Cross-Origin-iframes** — die Knoten sind nicht erreichbar, ein Pfad lässt sich nicht bauen.

## Benutzung

### Lesen

1. Scrollen Sie eine beliebige Seite. Sobald Sie anhalten, wird die Position gemerkt.
2. Kehren Sie zu der Seite zurück. Dieselbe Seite springt von selbst an Ihre Stelle; bei einer anderen erscheint stattdessen die Pille unten rechts.
3. Bei der Pille: anklicken oder ignorieren, dann blendet sie ab. Wer immer klicken möchte, schaltet den automatischen Sprung in den Einstellungen aus.
4. Der Rest steckt im Popup: **Weiterlesen**, **Auf dieser Website pausieren**, **Diese Seite ausschließen**, **Diese Seite löschen** und der Verlauf.

### Einen Wiederherstellungspunkt sichern (Pro)

1. Bringen Sie die Seite in den Zustand, den Sie behalten wollen.
2. Öffnen Sie das Popup, klicken Sie **Wiederherstellungspunkt sichern**, geben Sie einen Namen ein und speichern Sie. Alternativ der Kurzbefehl, wenn aktiviert.
3. Später listet das Popup auf derselben Seite jeden gesicherten Wiederherstellungspunkt.
4. Klicken Sie **Wiederherstellen**. Ein Panel zeigt, was überschrieben wird — bestätigen, und alle Felder plus Scroll-Offset kommen zurück.

Der Wiederherstellungspunkt bleibt bearbeitbar: Sie können ihn jederzeit über seine Zeile umbenennen oder löschen.

### Daten verwalten

- **Verlaufszeile** — Klick öffnet die Seite; das Papierkorb-Symbol entfernt nur diesen Eintrag.
- **Tab „Ausgeschlossen“** — in der vollständigen Liste: eine Adresse einfügen oder eine Musterregel anlegen; jeder Eintrag lässt sich in seiner Zeile wieder entfernen. Eine Regel zeigt, wie viele gespeicherte Seiten sie gerade trifft.
- **Datenbereich** — zeigt, wie viele Positionen und Wiederherstellungspunkte gespeichert sind und wie viel Platz sie brauchen.
- **Alle Positionen löschen** / **Alle Wiederherstellungspunkte löschen** — beide fragen vorher nach.
- **Export** schreibt Positionen (und mit Pro Wiederherstellungspunkte) in eine JSON-Datei. **Import** führt zusammen statt zu ersetzen und behält den neueren Datensatz, wenn beide Seiten dieselbe Seite kennen.

### Einstellungen

- **Kostenlos** — Website pausieren, Seiten und Muster ausschließen, Seite löschen, Export und Import.
- **Pro** — Pill-Verzögerung, Ausblenden und Auslöseschwelle; Wiederherstellungspunkte pro Seite; schreibgeschützte Felder; Tastaturkurzbefehl.

Unter Firefox wird der optionale Kurzbefehl im Add-ons-Manager gesetzt: **about:addons → Zahnrad → Tastenkürzel verwalten**.

## Berechtigungen

- **`storage`** — Positionen, Wiederherstellungspunkte, Einstellungen, Lizenzstatus. Nur lokal.
- **Zugriff auf http- und https-Seiten** — das Content-Skript muss vor dem Scrollen da sein, sonst gibt es nichts zu merken.
- **`debugger`** — für genau eine Sache: in ein Feld tippen, das jeden Schreibzugriff per Skript verwirft. Verbinden, einmal einfügen, sofort trennen. Es liest nie die Seite und bleibt nie verbunden.

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

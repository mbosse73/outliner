# Konzept und Plan

## Leitidee

Ein Inhalt, viele Ansichten. Alle Informationen liegen als hierarchische Punkte (Nodes) vor, beliebig tief verschachtelt. Ein Punkt kann zugleich Notiz, Aufgabe, Projekt, Dokument, Idee, Wissenseintrag, Termin oder Planungselement sein.

Aus derselben Struktur entstehen verschiedene Darstellungen: Gliederung, Mindmap, Kanban, Gantt, Kalender, Aufgabenliste. Der Benutzer entscheidet nicht vorher, welches Werkzeug er braucht — die Ansicht passt sich der Aufgabe an.

Vorbilder: Workflowy für globale Struktur, Tags, Suche und Fokusmodus. Bike Outliner für Tempo, Tastaturbedienung und dokumentorientiertes Arbeiten.

## Architektur

**Ein Datenmodell, viele Projektionen.** Nodes liegen in einer flachen Map, der Baum ergibt sich aus `parentId` und `children`. Jede Ansicht ist eine andere Lesart derselben Nodes:

- Kanban → Gruppierung nach `task.status`
- Kalender und Gantt → Filter auf `task.start` und `task.due`
- Mindmap → radiales Layout desselben Teilbaums
- Aufgabenliste → Filter auf `task != null`

Damit gibt es zwischen den Ansichten nichts zu synchronisieren.

```
Node = {
  id, parentId, children[],
  text, note, collapsed, fav,
  tags[], task: { status, prio, start, due, progress },
  created, modified
}
```

**Undo** ist ein Befehlsstapel auf dem Datenmodell. Eine Transaktion merkt sich den Zustand der berührten Nodes vorher und nachher; Texteingaben innerhalb von 900 ms werden zusammengefasst.

**Rendern** geschieht inkrementell: Zeilen werden über ihre id wiederverwendet, nur veränderte Teile werden neu gezeichnet. Beim Tippen steht reiner Text im Feld, sonst die ausgezeichnete Fassung mit Tags und Trefferhervorhebung — so bleibt der Cursor unberührt.

## Stufen

### Stufe 1 — Kern *(fertig)*
Baum mit unbegrenzter Verschachtelung, Tastaturbedienung, Ein- und Ausklappen, Fokusmodus mit Pfadleiste, Notizzeile, Undo/Redo, Autosave, JSON- und Markdown-Ausgabe.

### Stufe 2 — Organisation *(fertig)*
Globale Suche als gefilterte Gliederung, Tags per `#`, Anheften, Befehlspalette mit Sprungzielen, zuletzt Bearbeitetem und Befehlen.

### Stufe 3 — Aufgaben *(fertig)*
Status, Priorität, Start- und Fälligkeitsdatum, Fortschritt als optionale Eigenschaften eines Punktes, per Tastatur inline gesetzt. Sichten „Heute", „Offen", „Wichtig". Überfälliges in Signalrot.

Abgehakt wird per Klick aufs Kästchen oder über die Befehlspalette. Alles Übrige steht als Zeichen im Text und wird von dort ins Modell gespiegelt — dieselbe Bauart wie die Tags: `@2026-03-09` die Frist, `@von..bis` der Zeitraum, `!hoch` die Priorität, `%50` der Fortschritt. Auch der Palettenbefehl für die Priorität schreibt nur dieses Zeichen in die Zeile; damit bleibt der Text die einzige Wahrheit und es gibt keinen zweiten Ort, der abgeglichen werden müsste.

Die Sichten sind gespeicherte Suchen (`!heute`, `!offen`, `!wichtig`), keine eigene Ansicht — dieselbe Entscheidung wie oben, nur eine Ebene tiefer.

In der Notizzeile gelten die Zeichen nicht: sie ist Prosa.

### Stufe 4 — Weitere Ansichten *(teilweise)*
Kanban mit frei definierbaren Spalten, Kalender in Tag/Woche/Monat, Gantt mit Zeiträumen und Meilensteinen, Mindmap. Alles als SVG und CSS, ohne Bibliothek.

Fertig sind **Gantt** und **Mindmap**, umschaltbar über die Kopfzeile und die Palette. Beide sind Projektionen: sie lesen dieselbe Zeilenauswahl wie die Gliederung, also gelten Fokus, Zuklappen und Suche dort unverändert.

Im Gantt wird ein Punkt mit Zeitraum zum Balken, einer mit bloßer Frist zur Raute — der Meilenstein, den dieser Plan schon vorsah. Ein übergeordneter Punkt ohne eigenes Datum bekommt einen zusammenfassenden Balken vom frühesten Anfang bis zur spätesten Frist darunter; gerechnet wird über den ganzen Teilbaum, auch über zugeklappte Kinder. Undatierte Zweige erscheinen gar nicht. Die Mindmap ordnet denselben Zweig radial an, zeichnet die Verbindungen als SVG-Pfade und die Beschriftungen als HTML darüber.

Offen: **Kalender** sowie **Kanban** — Letzteres erst, wenn `task.status` mehr als `offen` und `erledigt` kennt und es einen Ort für die Spaltendefinition gibt.

### Stufe 5 — Wissensnetz
Interne Verweise `[[…]]`, Rückverweise, verwandte Themen. Spiegelungen: ein Punkt erscheint an mehreren Stellen, ohne kopiert zu werden.

### Stufe 6 — Dokumente
Einzelne Bereiche als eigenständige Dokumente behandeln, Import und Export, Archivierung.

### Später
KI-Unterstützung für Zusammenfassung und Strukturierung, automatische Projektplanung, Synchronisation zwischen Geräten, Vorlagen, Automatisierungen.

## Gestaltung

Die Oberfläche folgt dem Token-System des zugehörigen Dashboards:

| Rolle | Wert |
|---|---|
| Flächen | `--paper` #f7f5f0, `--sheet` #fffefb, `--raise` #efece4 |
| Linien | `--rule` #e2ded4, `--rule2` #cbc6ba |
| Schrift | `--ink` #1a1a18, `--ink2` #54514b, `--ink3` #87837c |
| Interaktion | `--tinte` #2f3a8c |
| Dringlich | `--signal` #a8321f — reserviert für Überfälliges ab Stufe 3 |
| Erledigt | `--gut` #2b6b46 |

Serif (Iowan Old Style) für Titel, Monospace für Etiketten, Zeiten und Zähler, Sans für Inhalte. Die Oberfläche ist ausdrücklich nur hell; der automatische Dunkelmodus mobiler Browser ist per `color-scheme: only light` abgeschaltet.

Eine Zutat gibt es nur hier: Die senkrechte Führungslinie des Zweigs, in dem der Cursor steht, leuchtet über alle Ebenen mit. In tiefen Strukturen zeigt sie ohne Nachdenken, wo man ist.

## Offene Entscheidungen

- Verhältnis zum Dashboard: eigenständiges Werkzeug oder später ein Modul. Bis dahin bleiben Logik, Darstellung und Speicher getrennt, damit beides möglich ist.
- Speicherform bei größeren Beständen: `localStorage` reicht für einige tausend Punkte, darüber wäre IndexedDB nötig.
- Markdown-Import (bisher nur Ausgabe).

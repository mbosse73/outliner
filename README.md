# Outliner

Ein Outliner als **einzelne HTML-Datei**. Kein Server, kein Build, keine Abhängigkeiten — Datei öffnen und schreiben.

Die Idee dahinter: *ein Inhalt, viele Ansichten*. Alles ist ein Punkt in einer Baumstruktur, der zugleich Notiz, Aufgabe, Projekt oder Termin sein kann. Aus derselben Struktur sollen später Kanban, Kalender, Gantt und Mindmap entstehen.

## Benutzen

`index.html` herunterladen und im Browser öffnen. Das war es.

Wer die Datei über GitHub Pages veröffentlicht, erreicht sie unter `https://<benutzername>.github.io/<repo>/` und kann sie auf dem Handy zum Startbildschirm hinzufügen.

## Tastatur

| Taste | Wirkung |
|---|---|
| `Enter` | neuer Punkt, teilt an der Cursorstelle |
| `Tab` / `Umschalt`+`Tab` | einrücken / ausrücken |
| `Alt`+`↑` `↓` | Punkt samt Unterpunkten verschieben |
| `↑` `↓` | Zeile hoch, Zeile runter |
| `Rücktaste` am Zeilenanfang | mit der Zeile darüber verbinden |
| `Strg`+`.` | Zweig ein- oder ausklappen |
| `Umschalt`+`Enter` | Notizzeile öffnen |
| `Alt`+`Umschalt`+`→` `←` | in einen Punkt springen / heraus |
| `Esc` | Suche zurücksetzen, sonst eine Ebene heraus |
| `Strg`+`F` | ins Suchfeld |
| `Strg`+`K` | Befehlspalette |
| `Strg`+`Umschalt`+`F` | Punkt anheften |
| `Strg`+`Z` / `Strg`+`Umschalt`+`Z` | rückgängig / wiederherstellen |
| `Strg`+`S` | Sicherung speichern |

## Suchen

Die Suche filtert die Gliederung selbst, statt eine Trefferliste zu zeigen: Treffer bleiben stehen, ihre übergeordneten Punkte bleiben als Weg sichtbar. Man sieht den Treffer also immer im Zusammenhang und kann sofort darin weiterschreiben.

- mehrere Wörter grenzen weiter ein
- `#tag` sucht nach einem Tag — Tags entstehen durch ein `#` im Text
- `!fav` zeigt nur Angeheftetes

Beim Umbauen der Struktur (Enter, Tab, Verschieben) wird der Filter aufgehoben, damit keine unsichtbaren Punkte entstehen.

## Daten

Gespeichert wird laufend im `localStorage` des Browsers.

**Wichtig:** Der Speicher hängt an der Herkunft der Seite. Eine lokal geöffnete Datei und dieselbe Datei über GitHub Pages sind für den Browser zwei verschiedene Orte mit getrennten Daten. Beim Umzug also erst `Datei › Sicherung speichern`, dann am neuen Ort `Datei › Sicherung öffnen`.

Zwei Ausgabeformate:

- **JSON** — verlustfrei, für Sicherung und Umzug
- **Markdown** — eingerückte Liste, für die Weiterverwendung anderswo

## Stand

Umgesetzt sind Stufe 1 und 2. Der weitere Plan steht in [KONZEPT.md](KONZEPT.md), die Arbeitsregeln für Änderungen in [CLAUDE.md](CLAUDE.md).

## Aufbau

Eine Datei, innen in nummerierte Abschnitte geteilt: Datenmodell, Undo, Strukturbefehle, Suche, Rendern, Cursor, Fokusmodus, Tastatur, Speichern, Datei, Palette.

Zwei Entscheidungen tragen den Rest:

- **Ein Datenmodell, viele Projektionen.** Nodes liegen in einer flachen Map; jede Ansicht ist nur eine andere Lesart derselben Daten. Zwischen Ansichten gibt es deshalb nichts zu synchronisieren.
- **Undo als Befehlsstapel auf dem Datenmodell**, nicht auf dem DOM. Jede Änderung merkt sich nur die berührten Nodes vorher und nachher; Texteingaben werden gebündelt.

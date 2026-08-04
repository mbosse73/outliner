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
- `!aufgabe`, `!offen`, `!erledigt` grenzen auf Aufgaben ein
- `!heute` zeigt, was fällig oder überfällig ist, `!überfällig` nur Letzteres
- `!wichtig` zeigt Offenes mit hoher Priorität, `!hoch` `!mittel` `!niedrig` filtern nach Stufe

Die Sichten „Heute", „Offen" und „Wichtig" in der Befehlspalette sind nichts anderes als diese Suchen — es gibt keine zweite Ansicht, nur einen anderen Filter auf dieselbe Gliederung.

Beim Umbauen der Struktur (Enter, Tab, Verschieben) wird der Filter aufgehoben, damit keine unsichtbaren Punkte entstehen.

## Aufgaben

Jeder Punkt kann eine Aufgabe sein. Ein Klick aufs Kästchen hakt sie ab; über die Befehlspalette (`Strg`+`K`) lässt sich ein Punkt zur Aufgabe machen und wieder zurücknehmen. Ein neues Tastenkürzel gibt es dafür nicht.

Alles Weitere wird getippt, nicht ausgewählt — Zeichen im Text, genau wie ein `#` einen Tag setzt:

| Geschrieben | Bedeutung |
|---|---|
| `@2026-03-09` | Fälligkeit |
| `@heute` `@morgen` `@übermorgen` | von heute aus gerechnet |
| `@mo` … `@so` | der nächste solche Wochentag |
| `@2026-03-02..2026-03-06` | Zeitraum: Start und Fälligkeit |
| `!hoch` `!mittel` `!niedrig` | Priorität |
| `%50` | Fortschritt |

Beim Verlassen der Zeile wird aus `@morgen` das ausgeschriebene Datum, damit Text und gespeicherte Frist nicht ab dem nächsten Tag auseinanderlaufen. Ein Punkt mit einem dieser Zeichen wird dadurch von selbst zur Aufgabe. Überfälliges steht in Signalrot.

Die Priorität lässt sich auch über die Palette setzen — der Befehl schreibt dasselbe Zeichen in die Zeile. Der Text bleibt so die einzige Wahrheit, und beide Wege führen zum selben Ergebnis.

In der Notizzeile gilt nichts davon: sie ist Prosa, ein Datum darin setzt keine Frist.

## Ansichten

Oben rechts stehen drei Ansichten derselben Punkte; `Strg`+`K` führt ebenfalls hin. Was Sie gerade sehen, hängt nicht davon ab, wo Sie geschrieben haben — Fokus, Zuklappen und Suche gelten überall gleich. Wer nach `!offen` sucht und dann aufs Gantt wechselt, sieht dort genau diese Auswahl.

- **Gliederung** — schreiben und umbauen. Nur hier wird bearbeitet.
- **Gantt** — ein Punkt mit Zeitraum wird zum Balken, einer mit bloßer Frist zur Raute. Ein übergeordneter Punkt ohne eigenes Datum fasst zusammen, was darunter liegt: vom frühesten Anfang bis zur spätesten Frist, auch bei zugeklapptem Zweig. Punkte ganz ohne Datum erscheinen nicht.
- **Kalender** — in Monat, Woche und Tag. Ein Zeitraum läuft als Balken über die Tage und bricht am Sonntag um; überschneidet sich etwas, rückt es in die nächste Spur. Anders als im Gantt erscheint hier nur, was **selbst** ein Datum trägt — ein Monatsfeld hat wenig Platz, und eine Zusammenfassung nennt keinen Termin, den man wahrnehmen könnte.
- **Mindmap** — derselbe Zweig radial angeordnet. Ein Klick springt in einen Punkt und wechselt zurück zur Gliederung.

Der Kalender kennt **keine Uhrzeiten** — das Modell speichert Tage, keine Zeitpunkte. Die Tagesansicht zählt deshalb auf, was an einem Tag läuft, beginnt, endet oder fällig wird, statt nach der Uhr zu ordnen.

Eine Abweichung ist Absicht: **Zuklappen wirkt nicht auf den Kalender.** Fokus und Suche schon — das sind bewusste Einschränkungen. Ein zugeklappter Zweig ist dagegen nur eine Lesebequemlichkeit der Gliederung und soll keine Termine still verschwinden lassen.

Auf schmalen Fenstern weicht die Leiste, damit sie den Pfad nicht zerdrückt; die Palette führt weiterhin zu jeder Ansicht.

## Verweise

`[[Umzug]]` im Text verweist auf den Punkt, der so heißt. Ein Klick springt hin. Getroffen wird der Punkt über seinen **blanken** Text — angehängte Zeichen zählen nicht mit, `[[Umzug]]` findet also auch „Umzug #wohnen @2026-09-01 !hoch".

Springt man in einen Punkt hinein, steht unter seinem Titel, wer auf ihn verweist. In der Gliederung selbst kostet das keinen Platz.

**Ein Verweis zeigt auf einen Text, nicht auf eine feste Kennung.** Benennt man das Ziel um, geht er ins Leere. Damit das nicht unbemerkt geschieht, wird ein loser Verweis gestrichelt und blass gezeichnet statt einfach wirkungslos zu sein.

## Daten

Gespeichert wird laufend im `localStorage` des Browsers.

**Wichtig:** Der Speicher hängt an der Herkunft der Seite. Eine lokal geöffnete Datei und dieselbe Datei über GitHub Pages sind für den Browser zwei verschiedene Orte mit getrennten Daten. Beim Umzug also erst `Datei › Sicherung speichern`, dann am neuen Ort `Datei › Sicherung öffnen`.

Zwei Ausgabeformate:

- **JSON** — verlustfrei, für Sicherung und Umzug
- **Markdown** — eingerückte Liste, für die Weiterverwendung anderswo

## Stand

Umgesetzt sind Stufe 1 bis 3, von Stufe 4 die Ansichten Gantt, Kalender und Mindmap, und von Stufe 5 die Verweise samt Rückverweisen. Kanban ist zurückgestellt; verwandte Themen und Spiegelungen stehen aus. Der weitere Plan steht in [KONZEPT.md](KONZEPT.md), die Arbeitsregeln für Änderungen in [CLAUDE.md](CLAUDE.md).

## Aufbau

Eine Datei, innen in nummerierte Abschnitte geteilt: Datenmodell, Undo, Strukturbefehle, Suche, Rendern, Cursor, Fokusmodus, Tastatur, Speichern, Datei, Palette.

Zwei Entscheidungen tragen den Rest:

- **Ein Datenmodell, viele Projektionen.** Nodes liegen in einer flachen Map; jede Ansicht ist nur eine andere Lesart derselben Daten. Zwischen Ansichten gibt es deshalb nichts zu synchronisieren.
- **Undo als Befehlsstapel auf dem Datenmodell**, nicht auf dem DOM. Jede Änderung merkt sich nur die berührten Nodes vorher und nachher; Texteingaben werden gebündelt.

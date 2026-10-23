# Gruppenverteiler

Eine praktische Webseite zur automatisierten Verwaltung und Verteilung von Gruppen. Die Anwendung ermöglicht es, Teilnehmer effizient in Arbeitsgruppen, Kurse oder Teams einzuteilen.

Link: https://joe91.github.io/gruppenverteiler

## 🎯 Hauptfunktionen

- **Automatische Gruppenverteilung**: Intelligente Algorithmen verteilen Teilnehmer gleichmäßig auf mehrere Gruppen
- **Flexible Aufteilungsmodi**: Aufteilung nach Anzahl der Gruppen oder nach Gruppengröße
- **Ausschlussmanagement**: Ausschlüsse für einzelne Personen-Kombinationen können angegeben werden
- **Interaktive Personenverwaltung**: Einzelne Personen können mit Checkbox ausgeschlossen werden
- **Liste im Browser bearbeiten**: Namen und Ausschlüsse direkt ändern, Personen hinzufügen oder entfernen – auch ganz ohne Datei
- **Listen-Verwaltung**: Häufig benötigte Datenlisten können gespeichert und wiedergeladen werden
- **Browser-Speicher**: Alle Daten werden lokal im Browser gespeichert – keine Serverübertragung notwendig
- **Offline-Funktionalität**: Volle Funktionalität auch ohne Internetverbindung
- **Export-Funktionen**: Gruppen können als Excel-Datei exportiert werden
- **Zufallsauswahl**: Zufällige Auswahl von Personen oder ganzen Gruppen

## 📁 Eingangsformat: Excel und CSV

Die Anwendung akzeptiert Excel-Dateien (.xlsx, .xls) und CSV-Dateien mit folgenden Anforderungen:

### Dateistruktur

Die Eingabedatei sollte eine Tabelle mit Spalten enthalten. Jede Zeile repräsentiert eine Person.

**Beispiel:**
| Vorname | Ausschlüsse | Nachname |
|---------|-------------|----------|
| Max | Anna, Peter | Müller |
| Anna | Max | Schmidt |
| Peter | - | Braun |

### Spalten konfigurieren

In der Anwendung können folgende Spaltenindizes konfiguriert werden (0-basiert, d.h. die erste Spalte hat Index 0):

- **Spalte: Vorname** (Standard: 0)
  - Die Spalte mit den Vornamen der Personen
  - Diese Spalte ist erforderlich

- **Spalte: Nachname** (Standard: -1 = keine)
  - Die Spalte mit den Nachnamen der Personen
  - Optional; wenn gesetzt, wird der Nachname bei der Anzeige berücksichtigt
  - Wenn mehrere Personen denselben Vornamen haben, wird der Nachname (gekürzt auf 3 Buchstaben) zur Unterscheidung hinzugefügt
  - Reichen 3 Buchstaben nicht (z.B. Müller/Müllner), wird der volle Nachname angezeigt; bei komplett gleichen Namen wird nummeriert: `Anna (1)`, `Anna (2)`

- **Spalte: Ausschlüsse** (Standard: 1)
  - Die Spalte mit den Ausschlüssen
  - Optional; kann auf -1 gesetzt werden, wenn keine Ausschlüsse vorhanden sind

- **Erste Zeile ignorieren** (Standard: ✓ aktiviert)
  - Wenn aktiviert, wird die erste Zeile als Überschrift behandelt und nicht als Person verarbeitet

### Beispiel-Datei

```
Vorname, Ausschlüsse, Nachname
Max, Anna; Peter, Müller
Anna, Max, Schmidt
Peter, , Braun
Sarah, Anna, Hoffmann
```

Nach dem Upload können die Spaltenindizes entsprechend angepasst werden.

## 📋 Ausschlüsse angeben

Ausschlüsse verhindern, dass zwei Personen in derselben Gruppe eingeteilt werden.

### Format der Ausschlussangabe

Ausschlüsse werden durch **Kommas getrennt** in eine Spalte der Excel/CSV-Datei eingetragen.

**Gültige Formate für Ausschlüsse:**
- `Vorname` – wenn die Person einen eindeutigen Vornamen hat
  - Beispiel: `Anna` schließt die Person mit Vornamen "Anna" aus
- `Vorname Nachname` – der vollständige Name
  - Beispiel: `Anna Schmidt`
- `Vorname Nachname.` – der Name mit verkürztem Nachnamen (3 Buchstaben)
  - Beispiel: `Anna Sch.` (wird automatisch erkannt)

### Beispiele

**Eingabedatei:**
```
Vorname, Ausschlüsse, Nachname
Max, Anna, Müller
Anna, Max; Peter, Schmidt
Peter, , Braun
```

**Ergebnis:**
- Max wird nicht mit Anna eingeteilt
- Anna wird nicht mit Max oder Peter eingeteilt
- Peter hat keine Ausschlüsse

**Mit vollständigen Namen:**
```
Vorname, Ausschlüsse, Nachname
Max, Anna Schmidt, Müller
Anna, Max Müller; Peter Braun, Schmidt
Peter, , Braun
```

### Hinweis zu Ausschlüssen

- Ausschlüsse werden **bidirektional** verarbeitet: Wenn Max Anna ausschließt, werden sie nicht zusammen gruppiert
- Wenn es unmöglich ist, alle Ausschlüsse zu beachten (z.B. bei zu wenig Alternativen), wird eine Warnung angezeigt und die Person wird trotzdem platziert
- Ausschlüsse sind optional – die Spalte kann leer sein oder der Index auf -1 gesetzt werden

## ⚙️ Aufteilungsmodi

### 1. Anzahl Gruppen (Standard)
- Gibt vor, wie viele Gruppen erstellt werden sollen
- Die Personen werden gleichmäßig verteilt
- Beispiel: 12 Personen in 3 Gruppen = 4 Personen pro Gruppe

### 2. Gruppengröße
- Gibt vor, wie groß jede Gruppe sein soll
- Die Anzahl der Gruppen wird automatisch berechnet
- **Rest-Modus bei Gruppengröße:**
  - **Verteilen** (Standard): Die restlichen Personen werden auf die Gruppen verteilt
    - Beispiel: 13 Personen, Größe 4 → Eine Gruppe mit 5, zwei mit 4
  - **Kleinere Gruppen**: Die restlichen Personen bilden eine separate kleinere Gruppe
    - Beispiel: 13 Personen, Größe 4 → Zwei Gruppen mit 4, eine mit 5

## 💾 Listen verwalten

Häufig verwendete Datenlisten können gespeichert werden:

1. **Datei hochladen** und konfigurieren
2. **Namen eingeben** im Feld "Name der Liste"
3. **"Liste speichern"** anklicken
4. Die Liste wird im Browser gespeichert und kann jederzeit erneut geladen werden
5. Zum Löschen auf das 🗑️-Symbol klicken

Die Listen werden im **lokalen Browser-Speicher** gespeichert und sind nur auf diesem Gerät/Browser verfügbar.

## 👥 Personen-Management

Nach dem Datei-Upload wird eine Liste aller Personen angezeigt:

- **Checkbox aktivieren** um eine Person vom Gruppierungsprozess auszuschließen
- Die ausgeschlossenen Personen werden durchgestrichen angezeigt
- Der Status zeigt: Gesamt | Aktiv | Ignoriert
- Nur aktive Personen werden in die Gruppen eingeteilt
- Haben mehrere Personen denselben Namen, erscheint ein Hinweis mit Link zum Bearbeiten

### ✏️ Liste bearbeiten

Über **„Liste bearbeiten“** wird die Personenliste zu einer editierbaren Tabelle (Vorname, Nachname, Ausschlüsse):

- **Gleiche Namen unterscheiden**: Doppelte Namen sind orange markiert – einfach einen Nachnamen ergänzen
- **Umbenennen**: Ausschlüsse anderer Personen, die eindeutig auf den alten Namen verweisen, werden automatisch angepasst
- **Ausschlüsse**: Komma-getrennt eintragen; Einträge, die zu keiner Person passen, werden orange markiert (Details per Mouseover)
- **Hinzufügen / Entfernen**: „➕ Person hinzufügen“ bzw. 🗑️; **Enter** springt ins nächste Feld, im letzten Feld wird eine neue Person angelegt
- Eine bestehende Gruppeneinteilung wird sofort aktualisiert (geänderte Namen, entfernte Personen)
- Alle Änderungen werden automatisch in der aktuellen Liste gespeichert

Mit **„oder Liste ohne Datei erstellen“** unter dem Upload-Feld lässt sich eine Liste komplett von Hand eingeben.

## 📊 Export und Verwendung

Nach der Gruppeneinteilung können die Gruppen exportiert werden:

- **Button "Als Excel exportieren"** erstellt eine Excel-Datei
- Die Gruppen werden in Spalten nebeneinander dargestellt
- Der Dateiname enthält einen Zeitstempel und optional den Namen der gespeicherten Liste
- Format: `Gruppeneinteilung_YYYY-MM-DDTHH-MM-SS.xlsx`

## 🎯 Zufallsauswahl

Zusätzliche Funktionen für Zufallsauswahl:

- **Zufällige Person**: Wählt zufällig eine Person aus der aktiven Liste
- **Zufällige Gruppe**: Wählt zufällig eine der erstellten Gruppen

Diese Funktionen sind nützlich für Losziehungen oder spontane Auswahl während des Unterrichts.

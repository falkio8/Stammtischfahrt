# Stammtischfahrten

Eine einfache, eigenständige Webseite zur Chronik unserer jährlichen Stammtischfahrten – *est. 09*.
Wer hat wann wo ausgerichtet, und welches **Ausrichterspiel** hat den nächsten Gastgeber bestimmt?

---

## Überblick

Die gesamte Anwendung besteht aus **einer einzigen HTML-Datei** (`index.html`).
Kein Server, kein Build, keine externen Abhängigkeiten – einfach im Browser öffnen.
Das Logo ist als Base64-Data-URI direkt eingebettet, es werden also **keine Zusatzdateien** benötigt.

## Features

- **Chronik-Tabelle** mit den Spalten *Wann · Wer · Wo · Ausrichterspiel*
- **Direkt editierbar** im Browser: Einträge hinzufügen, bearbeiten und löschen
- **Rotationen farblich hervorgehoben**
  - 1. Rotation · 2011–2016
  - 2. Rotation · 2017–2024
  - 3. Rotation · seit 2025
- **Export / Import** der kompletten Liste als JSON-Datei (zum Sichern oder Teilen mit der Runde)
- **Lokale Speicherung** im Browser (`localStorage`) – Änderungen bleiben zwischen Besuchen erhalten
- Dunkelblaues Design mit dem Gold des Logos, reduzierte, stilisierte Icons
- Responsiv – funktioniert auf Desktop und Smartphone

## Nutzung

1. Repository klonen oder `index.html` herunterladen
2. `index.html` im Browser öffnen (Doppelklick genügt)

```bash
git clone <repo-url>
cd stammtischfahrten
# index.html im Browser öffnen
```

Optional per lokalem Webserver:

```bash
python3 -m http.server 8000
# danach http://localhost:8000 im Browser aufrufen
```

## Daten sichern & teilen

Da `localStorage` nur pro Browser und Gerät gilt, gibt es zwei Buttons:

| Aktion     | Beschreibung                                                                 |
|------------|------------------------------------------------------------------------------|
| **Export** | Speichert die komplette Liste als `stammtischfahrten_JJJJ-MM-TT.json`.        |
| **Import** | Lädt eine zuvor exportierte JSON-Datei und ersetzt die aktuelle Liste.        |

So lässt sich der Stand dauerhaft sichern oder an die anderen aus der Runde weitergeben.

### Datenformat

```json
{
  "app": "stammtischfahrten",
  "version": 2,
  "exported": "2026-09-14T09:47:00.000Z",
  "fahrten": [
    { "year": 2012, "who": "Torben", "where": "Konstanz", "game": "Kicker" },
    { "year": 2020, "who": "", "where": "", "game": "", "note": "Corona" }
  ]
}
```

| Feld    | Typ    | Beschreibung                                              |
|---------|--------|----------------------------------------------------------|
| `year`  | Zahl   | Jahr der Fahrt (bestimmt zugleich die Rotation)          |
| `who`   | Text   | Ausrichter (kann leer sein)                              |
| `where` | Text   | Ort (kann leer sein)                                     |
| `game`  | Text   | Ausrichterspiel, das den nächsten Gastgeber bestimmt hat |
| `note`  | Text   | Optional, z. B. `"Corona"` für ausgefallene Jahre        |

## Spielregel

Es entscheidet immer ein **Ausrichterspiel**, wer als Nächstes ausrichten muss –
bis alle einmal dran waren, dann geht es von vorne los. Jeder Durchlauf = eine Rotation.

## Anpassen

- **Ausgangsdaten**: Das Array `DEFAULT_DATA` in `index.html` enthält die Standard-Chronik.
- **Rotationsgrenzen**: Die Funktion `rotationClass(year)` legt fest, welches Jahr zu welcher Rotation gehört.
- **Farben**: Die CSS-Variablen `--gold`, `--navy-*` sowie `--rot1/2/3` oben im `:root`-Block steuern das Farbschema.

## Technik

- Reines HTML, CSS und Vanilla JavaScript – keine Frameworks
- Icons als Inline-SVG
- Logo eingebettet als Base64-Data-URI

## Projektstruktur

```
stammtischfahrten/
├── index.html    # komplette Anwendung (inkl. eingebettetem Logo)
└── README.md
```

## Lizenz

Privates Projekt für die Stammtischrunde. Nutzung und Anpassung im Kreis frei.
